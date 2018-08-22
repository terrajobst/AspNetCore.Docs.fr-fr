---
uid: signalr/overview/older-versions/signalr-1x-hubs-api-guide-net-client
title: Guide de l’API ASP.NET SignalR Hubs - Client .NET (SignalR 1.x) | Microsoft Docs
author: pfletcher
description: Ce document fournit une introduction à l’utilisation de l’API Hubs pour la version 2 dans les clients .NET, tels que Windows Store (WinRT), WPF, Silverlight et les inconvénients de SignalR...
ms.author: riande
ms.date: 04/17/2013
ms.assetid: c334adc3-d6dc-44f3-9f06-f7634475aad3
msc.legacyurl: /signalr/overview/older-versions/signalr-1x-hubs-api-guide-net-client
msc.type: authoredcontent
ms.openlocfilehash: 5889429645ea1c682ea43c4b17afb3745318e32d
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41827872"
---
<a name="aspnet-signalr-hubs-api-guide---net-client-signalr-1x"></a><span data-ttu-id="b0488-103">Guide de l’API ASP.NET SignalR Hubs - Client .NET (SignalR 1.x)</span><span class="sxs-lookup"><span data-stu-id="b0488-103">ASP.NET SignalR Hubs API Guide - .NET Client (SignalR 1.x)</span></span>
====================
<span data-ttu-id="b0488-104">par [Patrick Fletcher](https://github.com/pfletcher), [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="b0488-104">by [Patrick Fletcher](https://github.com/pfletcher), [Tom Dykstra](https://github.com/tdykstra)</span></span>

> <span data-ttu-id="b0488-105">Ce document fournit une introduction à l’utilisation de l’API Hubs pour SignalR version 2 dans les clients .NET, tels que Windows Store (WinRT), WPF, Silverlight et les applications de console.</span><span class="sxs-lookup"><span data-stu-id="b0488-105">This document provides an introduction to using the Hubs API for SignalR version 2 in .NET clients, such as Windows Store (WinRT), WPF, Silverlight, and console applications.</span></span>
> 
> <span data-ttu-id="b0488-106">L’API de concentrateurs SignalR vous permet de vous permettent d’effectuer des appels de procédure distante (RPC) à partir d’un serveur aux clients connectés et à partir de clients sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="b0488-106">The SignalR Hubs API enables you to make remote procedure calls (RPCs) from a server to connected clients and from clients to the server.</span></span> <span data-ttu-id="b0488-107">Dans le code serveur, vous définissez des méthodes qui peuvent être appelées par les clients, et vous appelez des méthodes qui s’exécutent sur le client.</span><span class="sxs-lookup"><span data-stu-id="b0488-107">In server code, you define methods that can be called by clients, and you call methods that run on the client.</span></span> <span data-ttu-id="b0488-108">Dans le code client, vous définissez des méthodes qui peuvent être appelées à partir du serveur, et vous appelez des méthodes qui s’exécutent sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="b0488-108">In client code, you define methods that can be called from the server, and you call methods that run on the server.</span></span> <span data-ttu-id="b0488-109">SignalR s’occupe de tous les éléments client-serveur pour vous.</span><span class="sxs-lookup"><span data-stu-id="b0488-109">SignalR takes care of all of the client-to-server plumbing for you.</span></span>
> 
> <span data-ttu-id="b0488-110">SignalR offre également une API de niveau inférieur appelée connexions persistantes.</span><span class="sxs-lookup"><span data-stu-id="b0488-110">SignalR also offers a lower-level API called Persistent Connections.</span></span> <span data-ttu-id="b0488-111">Pour une introduction à SignalR Hubs et connexions persistantes, ou pour obtenir un didacticiel qui montre comment générer une application de SignalR complète, consultez [SignalR - mise en route](../getting-started/index.md).</span><span class="sxs-lookup"><span data-stu-id="b0488-111">For an introduction to SignalR, Hubs, and Persistent Connections, or for a tutorial that shows how to build a complete SignalR application, see [SignalR - Getting Started](../getting-started/index.md).</span></span>


## <a name="overview"></a><span data-ttu-id="b0488-112">Vue d'ensemble</span><span class="sxs-lookup"><span data-stu-id="b0488-112">Overview</span></span>

<span data-ttu-id="b0488-113">Ce document contient les sections suivantes :</span><span class="sxs-lookup"><span data-stu-id="b0488-113">This document contains the following sections:</span></span>

- [<span data-ttu-id="b0488-114">Programme d’installation du client</span><span class="sxs-lookup"><span data-stu-id="b0488-114">Client Setup</span></span>](#clientsetup)
- [<span data-ttu-id="b0488-115">Comment établir une connexion</span><span class="sxs-lookup"><span data-stu-id="b0488-115">How to establish a connection</span></span>](#establishconnection)

    - [<span data-ttu-id="b0488-116">Connexions inter-domaines à partir de clients Silverlight</span><span class="sxs-lookup"><span data-stu-id="b0488-116">Cross-domain connections from Silverlight clients</span></span>](#slcrossdomain)
- [<span data-ttu-id="b0488-117">Comment configurer la connexion</span><span class="sxs-lookup"><span data-stu-id="b0488-117">How to configure the connection</span></span>](#configureconnection)

    - [<span data-ttu-id="b0488-118">Comment définir le nombre maximal de connexions simultanées dans les clients WPF</span><span class="sxs-lookup"><span data-stu-id="b0488-118">How to set the maximum number of concurrent connections in WPF clients</span></span>](#maxconnections)
    - [<span data-ttu-id="b0488-119">Comment spécifier des paramètres de chaîne de requête</span><span class="sxs-lookup"><span data-stu-id="b0488-119">How to specify query string parameters</span></span>](#querystring)
    - [<span data-ttu-id="b0488-120">Comment spécifier le mode de transport</span><span class="sxs-lookup"><span data-stu-id="b0488-120">How to specify the transport method</span></span>](#transport)
    - [<span data-ttu-id="b0488-121">Comment spécifier des en-têtes HTTP</span><span class="sxs-lookup"><span data-stu-id="b0488-121">How to specify HTTP headers</span></span>](#httpheaders)
    - [<span data-ttu-id="b0488-122">Comment spécifier des certificats clients</span><span class="sxs-lookup"><span data-stu-id="b0488-122">How to specify client certificates</span></span>](#clientcertificate)
- [<span data-ttu-id="b0488-123">Comment créer le concentrateur proxy</span><span class="sxs-lookup"><span data-stu-id="b0488-123">How to create the Hub proxy</span></span>](#proxy)
- [<span data-ttu-id="b0488-124">Comment définir des méthodes sur le client que le serveur peut appeler.</span><span class="sxs-lookup"><span data-stu-id="b0488-124">How to define methods on the client that the server can call</span></span>](#callclient)

    - [<span data-ttu-id="b0488-125">Méthodes sans paramètres</span><span class="sxs-lookup"><span data-stu-id="b0488-125">Methods without parameters</span></span>](#clientmethodswithoutparms)
    - [<span data-ttu-id="b0488-126">Méthodes avec des paramètres, en spécifiant les types de paramètres</span><span class="sxs-lookup"><span data-stu-id="b0488-126">Methods with parameters, specifying parameter types</span></span>](#clientmethodswithparmtypes)
    - [<span data-ttu-id="b0488-127">Méthodes avec des paramètres, en spécifiant des objets dynamiques pour les paramètres</span><span class="sxs-lookup"><span data-stu-id="b0488-127">Methods with parameters, specifying dynamic objects for the parameters</span></span>](#clientmethodswithdynamparms)
    - [<span data-ttu-id="b0488-128">Comment supprimer un gestionnaire</span><span class="sxs-lookup"><span data-stu-id="b0488-128">How to remove a handler</span></span>](#removehandler)
- [<span data-ttu-id="b0488-129">Comment appeler des méthodes de serveur à partir du client</span><span class="sxs-lookup"><span data-stu-id="b0488-129">How to call server methods from the client</span></span>](#callserver)
- [<span data-ttu-id="b0488-130">Comment gérer les événements de durée de vie de connexion</span><span class="sxs-lookup"><span data-stu-id="b0488-130">How to handle connection lifetime events</span></span>](#connectionlifetime)
- [<span data-ttu-id="b0488-131">Comment gérer les erreurs</span><span class="sxs-lookup"><span data-stu-id="b0488-131">How to handle errors</span></span>](#handleerrors)
- [<span data-ttu-id="b0488-132">Comment activer la journalisation côté client</span><span class="sxs-lookup"><span data-stu-id="b0488-132">How to enable client-side logging</span></span>](#logging)
- [<span data-ttu-id="b0488-133">WPF, Silverlight et exemples de code d’application console pour les méthodes de client que le serveur peut appeler.</span><span class="sxs-lookup"><span data-stu-id="b0488-133">WPF, Silverlight, and console application code samples for client methods that the server can call</span></span>](#wpfsl)

<span data-ttu-id="b0488-134">Pour un exemples de projets de client .NET, consultez les ressources suivantes :</span><span class="sxs-lookup"><span data-stu-id="b0488-134">For a sample .NET client projects, see the following resources:</span></span>

- <span data-ttu-id="b0488-135">[gustavo-armenta / SignalR-Samples](https://github.com/gustavo-armenta/SignalR-Samples) sur GitHub.com (WinRT, Silverlight, les exemples d’application console).</span><span class="sxs-lookup"><span data-stu-id="b0488-135">[gustavo-armenta / SignalR-Samples](https://github.com/gustavo-armenta/SignalR-Samples) on GitHub.com (WinRT, Silverlight, console app examples).</span></span>
- <span data-ttu-id="b0488-136">[DamianEdwards / SignalR-MoveShapeDemo / MoveShape.Desktop](https://github.com/DamianEdwards/SignalR-MoveShapeDemo/tree/master/MoveShape/MoveShape.Desktop) sur GitHub.com (par exemple, WPF).</span><span class="sxs-lookup"><span data-stu-id="b0488-136">[DamianEdwards / SignalR-MoveShapeDemo / MoveShape.Desktop](https://github.com/DamianEdwards/SignalR-MoveShapeDemo/tree/master/MoveShape/MoveShape.Desktop) on GitHub.com (WPF example).</span></span>
- <span data-ttu-id="b0488-137">[SignalR / Microsoft.AspNet.SignalR.Client.Samples](https://github.com/SignalR/SignalR/tree/master/samples/Microsoft.AspNet.SignalR.Client.Samples) sur GitHub.com (exemple d’application Console).</span><span class="sxs-lookup"><span data-stu-id="b0488-137">[SignalR / Microsoft.AspNet.SignalR.Client.Samples](https://github.com/SignalR/SignalR/tree/master/samples/Microsoft.AspNet.SignalR.Client.Samples) on GitHub.com (Console app example).</span></span>

<span data-ttu-id="b0488-138">Pour obtenir une documentation sur la façon de programmer le serveur ou les clients JavaScript, consultez les ressources suivantes :</span><span class="sxs-lookup"><span data-stu-id="b0488-138">For documentation on how to program the server or JavaScript clients, see the following resources:</span></span>

- [<span data-ttu-id="b0488-139">Guide de l’API SignalR Hubs - serveur</span><span class="sxs-lookup"><span data-stu-id="b0488-139">SignalR Hubs API Guide - Server</span></span>](../guide-to-the-api/hubs-api-guide-server.md)
- [<span data-ttu-id="b0488-140">Guide de l’API SignalR Hubs - Client JavaScript</span><span class="sxs-lookup"><span data-stu-id="b0488-140">SignalR Hubs API Guide - JavaScript Client</span></span>](../guide-to-the-api/hubs-api-guide-javascript-client.md)

<span data-ttu-id="b0488-141">Liens vers des rubriques de référence de l’API sont à la version de .NET 4.5 de l’API.</span><span class="sxs-lookup"><span data-stu-id="b0488-141">Links to API Reference topics are to the .NET 4.5 version of the API.</span></span> <span data-ttu-id="b0488-142">Si vous utilisez .NET 4, consultez [la version de .NET 4 des rubriques API](https://msdn.microsoft.com/library/jj891075(v=vs.100).aspx).</span><span class="sxs-lookup"><span data-stu-id="b0488-142">If you're using .NET 4, see [the .NET 4 version of the API topics](https://msdn.microsoft.com/library/jj891075(v=vs.100).aspx).</span></span>

<a id="clientsetup"></a>

## <a name="client-setup"></a><span data-ttu-id="b0488-143">Programme d’installation du client</span><span class="sxs-lookup"><span data-stu-id="b0488-143">Client setup</span></span>

<span data-ttu-id="b0488-144">Installer le [Microsoft.AspNet.SignalR.Client](http://nuget.org/packages/Microsoft.AspNet.SignalR.Client) package NuGet (pas le [Microsoft.AspNet.SignalR](http://nuget.org/packages/microsoft.aspnet.signalr) package).</span><span class="sxs-lookup"><span data-stu-id="b0488-144">Install the [Microsoft.AspNet.SignalR.Client](http://nuget.org/packages/Microsoft.AspNet.SignalR.Client) NuGet package (not the [Microsoft.AspNet.SignalR](http://nuget.org/packages/microsoft.aspnet.signalr) package).</span></span> <span data-ttu-id="b0488-145">Ce package prend en charge WinRT, Silverlight, WPF, application console et clients Windows Phone, .NET 4 et .NET 4.5.</span><span class="sxs-lookup"><span data-stu-id="b0488-145">This package supports WinRT, Silverlight, WPF, console application, and Windows Phone clients, for both .NET 4 and .NET 4.5.</span></span>

<span data-ttu-id="b0488-146">Si la version de SignalR que vous disposez sur le client est différente de la version que vous disposez sur le serveur, SignalR est souvent capable de s’adapter à la différence.</span><span class="sxs-lookup"><span data-stu-id="b0488-146">If the version of SignalR that you have on the client is different from the version that you have on the server, SignalR is often able to adapt to the difference.</span></span> <span data-ttu-id="b0488-147">Par exemple, lors de la version 2.0 de SignalR est publié et vous qui installez sur le serveur, le serveur prendra en charge les clients qui ont 1.1.x installé, ainsi que les clients qui ont 2.0 est installé.</span><span class="sxs-lookup"><span data-stu-id="b0488-147">For example, when SignalR version 2.0 is released and you install that on the server, the server will support clients that have 1.1.x installed as well as clients that have 2.0 installed.</span></span> <span data-ttu-id="b0488-148">Si la différence entre la version sur le serveur et la version sur le client est trop grande, SignalR lève une `InvalidOperationException` exception lorsque le client tente d’établir une connexion.</span><span class="sxs-lookup"><span data-stu-id="b0488-148">If the difference between the version on the server and the version on the client is too great, SignalR throws an `InvalidOperationException` exception when the client tries to establish a connection.</span></span> <span data-ttu-id="b0488-149">Le message d’erreur est «`You are using a version of the client that isn't compatible with the server. Client version X.X, server version X.X`».</span><span class="sxs-lookup"><span data-stu-id="b0488-149">The error message is "`You are using a version of the client that isn't compatible with the server. Client version X.X, server version X.X`".</span></span>

<a id="establishconnection"></a>

## <a name="how-to-establish-a-connection"></a><span data-ttu-id="b0488-150">Comment établir une connexion</span><span class="sxs-lookup"><span data-stu-id="b0488-150">How to establish a connection</span></span>

<span data-ttu-id="b0488-151">Avant de pouvoir établir une connexion, vous devez créer un `HubConnection` de l’objet et de créer un proxy.</span><span class="sxs-lookup"><span data-stu-id="b0488-151">Before you can establish a connection, you have to create a `HubConnection` object and create a proxy.</span></span> <span data-ttu-id="b0488-152">Pour établir la connexion, appelez le `Start` méthode sur le `HubConnection` objet.</span><span class="sxs-lookup"><span data-stu-id="b0488-152">To establish the connection, call the `Start` method on the `HubConnection` object.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample1.cs?highlight=1,4)]

> [!NOTE]
> <span data-ttu-id="b0488-153">Pour les clients JavaScript, vous devez inscrire au moins un gestionnaire d’événements avant d’appeler le `Start` méthode pour établir la connexion.</span><span class="sxs-lookup"><span data-stu-id="b0488-153">For JavaScript clients you have to register at least one event handler before calling the `Start` method to establish the connection.</span></span> <span data-ttu-id="b0488-154">Cela n’est pas nécessaire pour les clients .NET.</span><span class="sxs-lookup"><span data-stu-id="b0488-154">This is not necessary for .NET clients.</span></span> <span data-ttu-id="b0488-155">Pour les clients JavaScript, le code proxy généré crée automatiquement des proxys pour tous les concentrateurs qui existent sur le serveur et l’inscription d’un gestionnaire sert à indiquer les Hubs votre client prévoit d’utiliser.</span><span class="sxs-lookup"><span data-stu-id="b0488-155">For JavaScript clients, the generated proxy code automatically creates proxies for all Hubs that exist on the server, and registering a handler is how you indicate which Hubs your client intends to use.</span></span> <span data-ttu-id="b0488-156">Mais pour un client .NET vous créer des proxys de Hub manuellement, afin de SignalR part du principe que vous utilisez n’importe quel Hub que vous créez un proxy pour.</span><span class="sxs-lookup"><span data-stu-id="b0488-156">But for a .NET client you create Hub proxies manually, so SignalR assumes that you will be using any Hub that you create a proxy for.</span></span>


<span data-ttu-id="b0488-157">L’exemple de code utilise la valeur par défaut « / signalr « URL pour se connecter à votre service de SignalR.</span><span class="sxs-lookup"><span data-stu-id="b0488-157">The sample code uses the default "/signalr" URL to connect to your SignalR service.</span></span> <span data-ttu-id="b0488-158">Pour plus d’informations sur la façon de spécifier une autre URL de base, consultez [Guide de l’API ASP.NET SignalR Hubs - Server - URL /signalr](../guide-to-the-api/hubs-api-guide-server.md#signalrurl).</span><span class="sxs-lookup"><span data-stu-id="b0488-158">For information about how to specify a different base URL, see [ASP.NET SignalR Hubs API Guide - Server - The /signalr URL](../guide-to-the-api/hubs-api-guide-server.md#signalrurl).</span></span>

<span data-ttu-id="b0488-159">Le `Start` méthode s’exécute de façon asynchrone.</span><span class="sxs-lookup"><span data-stu-id="b0488-159">The `Start` method executes asynchronously.</span></span> <span data-ttu-id="b0488-160">Pour vous assurer que les lignes de code ne s’exécutent jusqu'à une fois la connexion établie, utilisez `await` dans une méthode asynchrone de ASP.NET 4.5 ou `.Wait()` dans une méthode synchrone.</span><span class="sxs-lookup"><span data-stu-id="b0488-160">To make sure that subsequent lines of code don't execute until after the connection is established, use `await` in an ASP.NET 4.5 asynchronous method or `.Wait()` in a synchronous method.</span></span> <span data-ttu-id="b0488-161">N’utilisez pas `.Wait()` dans un client WinRT.</span><span class="sxs-lookup"><span data-stu-id="b0488-161">Don't use `.Wait()` in a WinRT client.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample2.cs?highlight=1)]

[!code-css[Main](signalr-1x-hubs-api-guide-net-client/samples/sample3.css?highlight=1)]

<span data-ttu-id="b0488-162">La classe `HubConnection` est thread-safe.</span><span class="sxs-lookup"><span data-stu-id="b0488-162">The `HubConnection` class is thread-safe.</span></span>

<a id="slcrossdomain"></a>

### <a name="cross-domain-connections-from-silverlight-clients"></a><span data-ttu-id="b0488-163">Connexions inter-domaines à partir de clients Silverlight</span><span class="sxs-lookup"><span data-stu-id="b0488-163">Cross-domain connections from Silverlight clients</span></span>

<span data-ttu-id="b0488-164">Pour plus d’informations sur l’activation des connexions inter-domaines à partir de clients de Silverlight, consultez [rendre un Service disponible entre les limites du domaine](https://msdn.microsoft.com/library/cc197955(v=vs.95).aspx).</span><span class="sxs-lookup"><span data-stu-id="b0488-164">For information about how to enable cross-domain connections from Silverlight clients, see [Making a Service Available Across Domain Boundaries](https://msdn.microsoft.com/library/cc197955(v=vs.95).aspx).</span></span>

<a id="configureconnection"></a>

## <a name="how-to-configure-the-connection"></a><span data-ttu-id="b0488-165">Comment configurer la connexion</span><span class="sxs-lookup"><span data-stu-id="b0488-165">How to configure the connection</span></span>

<span data-ttu-id="b0488-166">Avant d’établir une connexion, vous pouvez spécifier une des options suivantes :</span><span class="sxs-lookup"><span data-stu-id="b0488-166">Before you establish a connection, you can specify any of the following options:</span></span>

- <span data-ttu-id="b0488-167">Limite de connexions simultanées.</span><span class="sxs-lookup"><span data-stu-id="b0488-167">Concurrent connections limit.</span></span>
- <span data-ttu-id="b0488-168">Paramètres de chaîne de requête.</span><span class="sxs-lookup"><span data-stu-id="b0488-168">Query string parameters.</span></span>
- <span data-ttu-id="b0488-169">Le mode de transport.</span><span class="sxs-lookup"><span data-stu-id="b0488-169">The transport method.</span></span>
- <span data-ttu-id="b0488-170">En-têtes HTTP.</span><span class="sxs-lookup"><span data-stu-id="b0488-170">HTTP headers.</span></span>
- <span data-ttu-id="b0488-171">Certificats clients.</span><span class="sxs-lookup"><span data-stu-id="b0488-171">Client certificates.</span></span>

<a id="maxconnections"></a>

### <a name="how-to-set-the-maximum-number-of-concurrent-connections-in-wpf-clients"></a><span data-ttu-id="b0488-172">Comment définir le nombre maximal de connexions simultanées dans les clients WPF</span><span class="sxs-lookup"><span data-stu-id="b0488-172">How to set the maximum number of concurrent connections in WPF clients</span></span>

<span data-ttu-id="b0488-173">Les clients de WPF, vous devrez peut-être augmenter le nombre maximal de connexions simultanées à partir de sa valeur par défaut de 2.</span><span class="sxs-lookup"><span data-stu-id="b0488-173">In WPF clients, you might have to increase the maximum number of concurrent connections from its default value of 2.</span></span> <span data-ttu-id="b0488-174">La valeur recommandée est 10.</span><span class="sxs-lookup"><span data-stu-id="b0488-174">The recommended value is 10.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample4.cs?highlight=4)]

<span data-ttu-id="b0488-175">Pour plus d’informations, consultez [ServicePointManager.DefaultConnectionLimit](https://msdn.microsoft.com/library/system.net.servicepointmanager.defaultconnectionlimit.aspx).</span><span class="sxs-lookup"><span data-stu-id="b0488-175">For more information, see [ServicePointManager.DefaultConnectionLimit](https://msdn.microsoft.com/library/system.net.servicepointmanager.defaultconnectionlimit.aspx).</span></span>

<a id="querystring"></a>

### <a name="how-to-specify-query-string-parameters"></a><span data-ttu-id="b0488-176">Comment spécifier des paramètres de chaîne de requête</span><span class="sxs-lookup"><span data-stu-id="b0488-176">How to specify query string parameters</span></span>

<span data-ttu-id="b0488-177">Si vous souhaitez envoyer des données au serveur lorsque le client se connecte, vous pouvez ajouter des paramètres de chaîne de requête à l’objet de connexion.</span><span class="sxs-lookup"><span data-stu-id="b0488-177">If you want to send data to the server when the client connects, you can add query string parameters to the connection object.</span></span> <span data-ttu-id="b0488-178">L’exemple suivant montre comment définir un paramètre de chaîne de requête dans le code client.</span><span class="sxs-lookup"><span data-stu-id="b0488-178">The following example shows how to set a query string parameter in client code.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample5.cs)]

<span data-ttu-id="b0488-179">L’exemple suivant montre comment lire un paramètre de chaîne de requête dans le code serveur.</span><span class="sxs-lookup"><span data-stu-id="b0488-179">The following example shows how to read a query string parameter in server code.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample6.cs?highlight=5)]

<a id="transport"></a>

### <a name="how-to-specify-the-transport-method"></a><span data-ttu-id="b0488-180">Comment spécifier le mode de transport</span><span class="sxs-lookup"><span data-stu-id="b0488-180">How to specify the transport method</span></span>

<span data-ttu-id="b0488-181">Dans le cadre du processus de connexion, un client SignalR est normalement négocie avec le serveur pour déterminer le meilleur transport qui est pris en charge par le serveur et client.</span><span class="sxs-lookup"><span data-stu-id="b0488-181">As part of the process of connecting, a SignalR client normally negotiates with the server to determine the best transport that is supported by both server and client.</span></span> <span data-ttu-id="b0488-182">Si vous connaissez déjà le transport à utiliser, vous pouvez ignorer ce processus de négociation.</span><span class="sxs-lookup"><span data-stu-id="b0488-182">If you already know which transport you want to use, you can bypass this negotiation process.</span></span> <span data-ttu-id="b0488-183">Pour spécifier le mode de transport, passez un objet de transport à la méthode Start.</span><span class="sxs-lookup"><span data-stu-id="b0488-183">To specify the transport method, pass in a transport object to the Start method.</span></span> <span data-ttu-id="b0488-184">L’exemple suivant montre comment spécifier le mode de transport dans le code client.</span><span class="sxs-lookup"><span data-stu-id="b0488-184">The following example shows how to specify the transport method in client code.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample7.cs?highlight=4)]

<span data-ttu-id="b0488-185">Le [Microsoft.AspNet.SignalR.Client.Transports](https://msdn.microsoft.com/library/jj918090(v=vs.111).aspx) espace de noms inclut les classes suivantes que vous pouvez utiliser pour spécifier le transport.</span><span class="sxs-lookup"><span data-stu-id="b0488-185">The [Microsoft.AspNet.SignalR.Client.Transports](https://msdn.microsoft.com/library/jj918090(v=vs.111).aspx) namespace includes the following classes that you can use to specify the transport.</span></span>

- <span data-ttu-id="b0488-186">[LongPollingTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.longpollingtransport(v=vs.111).aspx)</span><span class="sxs-lookup"><span data-stu-id="b0488-186">[LongPollingTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.longpollingtransport(v=vs.111).aspx)</span></span>
- <span data-ttu-id="b0488-187">[ServerSentEventsTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.serversenteventstransport(v=vs.111).aspx)</span><span class="sxs-lookup"><span data-stu-id="b0488-187">[ServerSentEventsTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.serversenteventstransport(v=vs.111).aspx)</span></span>
- <span data-ttu-id="b0488-188">[WebSocketTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.websockettransport(v=vs.111).aspx) (disponible uniquement lorsque le serveur et client utilisent .NET 4.5).</span><span class="sxs-lookup"><span data-stu-id="b0488-188">[WebSocketTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.websockettransport(v=vs.111).aspx) (Available only when both server and client use .NET 4.5.)</span></span>
- <span data-ttu-id="b0488-189">[AutoTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.autotransport(v=vs.111).aspx) (choisit automatiquement le transport de meilleures est pris en charge par le client et le serveur.</span><span class="sxs-lookup"><span data-stu-id="b0488-189">[AutoTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.autotransport(v=vs.111).aspx) (Automatically chooses the best transport that is supported by both the client and the server.</span></span> <span data-ttu-id="b0488-190">Il s’agit du transport par défaut.</span><span class="sxs-lookup"><span data-stu-id="b0488-190">This is the default transport.</span></span> <span data-ttu-id="b0488-191">En la passant dans à la `Start` méthode a le même effet que passant ne pas quoi que ce soit.)</span><span class="sxs-lookup"><span data-stu-id="b0488-191">Passing this in to the `Start` method has the same effect as not passing in anything.)</span></span>

<span data-ttu-id="b0488-192">Le transport ForeverFrame n’est pas inclus dans cette liste, car il est utilisé uniquement par les navigateurs.</span><span class="sxs-lookup"><span data-stu-id="b0488-192">The ForeverFrame transport is not included in this list because it is used only by browsers.</span></span>

<span data-ttu-id="b0488-193">Pour plus d’informations sur la vérification de la méthode de transport dans le code serveur, consultez [Guide de l’API ASP.NET SignalR Hubs - Server - comment obtenir des informations sur le client à partir de la propriété de contexte](../guide-to-the-api/hubs-api-guide-server.md#contextproperty).</span><span class="sxs-lookup"><span data-stu-id="b0488-193">For information about how to check the transport method in server code, see [ASP.NET SignalR Hubs API Guide - Server - How to get information about the client from the Context property](../guide-to-the-api/hubs-api-guide-server.md#contextproperty).</span></span> <span data-ttu-id="b0488-194">Pour plus d’informations sur les transports et les solutions de secours, consultez [Introduction à SignalR - Transports et les solutions de secours](../getting-started/introduction-to-signalr.md#transports).</span><span class="sxs-lookup"><span data-stu-id="b0488-194">For more information about transports and fallbacks, see [Introduction to SignalR - Transports and Fallbacks](../getting-started/introduction-to-signalr.md#transports).</span></span>

<a id="httpheaders"></a>

### <a name="how-to-specify-http-headers"></a><span data-ttu-id="b0488-195">Comment spécifier des en-têtes HTTP</span><span class="sxs-lookup"><span data-stu-id="b0488-195">How to specify HTTP headers</span></span>

<span data-ttu-id="b0488-196">Pour définir des en-têtes HTTP, utilisez le `Headers` propriété sur l’objet de connexion.</span><span class="sxs-lookup"><span data-stu-id="b0488-196">To set HTTP headers, use the `Headers` property on the connection object.</span></span> <span data-ttu-id="b0488-197">L’exemple suivant montre comment ajouter un en-tête HTTP.</span><span class="sxs-lookup"><span data-stu-id="b0488-197">The following example shows how to add an HTTP header.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample8.cs?highlight=2)]

<a id="clientcertificate"></a>

### <a name="how-to-specify-client-certificates"></a><span data-ttu-id="b0488-198">Comment spécifier des certificats clients</span><span class="sxs-lookup"><span data-stu-id="b0488-198">How to specify client certificates</span></span>

<span data-ttu-id="b0488-199">Pour ajouter des certificats clients, utilisez la `AddClientCertificate` méthode sur l’objet de connexion.</span><span class="sxs-lookup"><span data-stu-id="b0488-199">To add client certificates, use the `AddClientCertificate` method on the connection object.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample9.cs?highlight=2)]

<a id="proxy"></a>

## <a name="how-to-create-the-hub-proxy"></a><span data-ttu-id="b0488-200">Comment créer le concentrateur proxy</span><span class="sxs-lookup"><span data-stu-id="b0488-200">How to create the Hub proxy</span></span>

<span data-ttu-id="b0488-201">Pour définir des méthodes sur le client un Hub peut appeler à partir du serveur et pour appeler des méthodes sur un concentrateur sur le serveur, créer un proxy pour le Hub en appelant `CreateHubProxy` sur l’objet de connexion.</span><span class="sxs-lookup"><span data-stu-id="b0488-201">In order to define methods on the client that a Hub can call from the server, and to invoke methods on a Hub at the server, create a proxy for the Hub by calling `CreateHubProxy` on the connection object.</span></span> <span data-ttu-id="b0488-202">La chaîne vous passez à `CreateHubProxy` est le nom de votre classe de concentrateur ou le nom spécifié par le `HubName` attribut si un seul était utilisé sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="b0488-202">The string you pass in to `CreateHubProxy` is the name of your Hub class, or the name specified by the `HubName` attribute if one was used on the server.</span></span> <span data-ttu-id="b0488-203">Correspondance de noms respecte la casse.</span><span class="sxs-lookup"><span data-stu-id="b0488-203">Name matching is case-insensitive.</span></span>

<span data-ttu-id="b0488-204">**Classe de concentrateur sur le serveur**</span><span class="sxs-lookup"><span data-stu-id="b0488-204">**Hub class on server**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample10.cs?highlight=1)]

<span data-ttu-id="b0488-205">**Créer le proxy client pour la classe de concentrateur**</span><span class="sxs-lookup"><span data-stu-id="b0488-205">**Create client proxy for the Hub class**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample11.cs?highlight=2)]

<span data-ttu-id="b0488-206">Si vous décorez votre classe Hub avec un `HubName` d’attribut, utilisez ce nom.</span><span class="sxs-lookup"><span data-stu-id="b0488-206">If you decorate your Hub class with a `HubName` attribute, use that name.</span></span>

<span data-ttu-id="b0488-207">**Classe de concentrateur sur le serveur**</span><span class="sxs-lookup"><span data-stu-id="b0488-207">**Hub class on server**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample12.cs)]

<span data-ttu-id="b0488-208">**Créer le proxy client pour la classe de concentrateur**</span><span class="sxs-lookup"><span data-stu-id="b0488-208">**Create client proxy for the Hub class**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample13.cs?highlight=2)]

<span data-ttu-id="b0488-209">L’objet proxy est thread-safe.</span><span class="sxs-lookup"><span data-stu-id="b0488-209">The proxy object is thread-safe.</span></span> <span data-ttu-id="b0488-210">En fait, si vous appelez `HubConnection.CreateHubProxy` plusieurs fois avec le même `hubName`, vous obtenez la même mise en cache `IHubProxy` objet.</span><span class="sxs-lookup"><span data-stu-id="b0488-210">In fact, if you call `HubConnection.CreateHubProxy` multiple times with the same `hubName`, you get the same cached `IHubProxy` object.</span></span>

<a id="callclient"></a>

## <a name="how-to-define-methods-on-the-client-that-the-server-can-call"></a><span data-ttu-id="b0488-211">Comment définir des méthodes sur le client que le serveur peut appeler.</span><span class="sxs-lookup"><span data-stu-id="b0488-211">How to define methods on the client that the server can call</span></span>

<span data-ttu-id="b0488-212">Pour définir une méthode que le serveur peut appeler, utilisez le proxy `On` méthode pour inscrire un gestionnaire d’événements.</span><span class="sxs-lookup"><span data-stu-id="b0488-212">To define a method that the server can call, use the proxy's `On` method to register an event handler.</span></span>

<span data-ttu-id="b0488-213">Correspondance de noms de méthode respecte la casse.</span><span class="sxs-lookup"><span data-stu-id="b0488-213">Method name matching is case-insensitive.</span></span> <span data-ttu-id="b0488-214">Par exemple, `Clients.All.UpdateStockPrice` sur le serveur s’exécutera `updateStockPrice`, `updatestockprice`, ou `UpdateStockPrice` sur le client.</span><span class="sxs-lookup"><span data-stu-id="b0488-214">For example, `Clients.All.UpdateStockPrice` on the server will execute `updateStockPrice`, `updatestockprice`, or `UpdateStockPrice` on the client.</span></span>

<span data-ttu-id="b0488-215">Plateformes clientes différentes exigences sont différentes pour l’écriture du code de méthode pour mettre à jour de l’interface utilisateur.</span><span class="sxs-lookup"><span data-stu-id="b0488-215">Different client platforms have different requirements for how you write method code to update the UI.</span></span> <span data-ttu-id="b0488-216">Les exemples présentés sont pour les clients de WinRT (Windows Store .NET).</span><span class="sxs-lookup"><span data-stu-id="b0488-216">The examples shown are for WinRT (Windows Store .NET) clients.</span></span> <span data-ttu-id="b0488-217">WPF, Silverlight et exemples d’applications console sont fournies dans [une section distincte plus loin dans cette rubrique](#wpfsl).</span><span class="sxs-lookup"><span data-stu-id="b0488-217">WPF, Silverlight, and console application examples are provided in [a separate section later in this topic](#wpfsl).</span></span>

<a id="clientmethodswithoutparms"></a>

### <a name="methods-without-parameters"></a><span data-ttu-id="b0488-218">Méthodes sans paramètres</span><span class="sxs-lookup"><span data-stu-id="b0488-218">Methods without parameters</span></span>

<span data-ttu-id="b0488-219">Si la méthode que vous traitez n’a pas de paramètres, utilisez la surcharge non générique de la `On` méthode :</span><span class="sxs-lookup"><span data-stu-id="b0488-219">If the method you're handling does not have parameters, use the non-generic overload of the `On` method:</span></span>

<span data-ttu-id="b0488-220">**Code de serveur client de méthode sans paramètres**</span><span class="sxs-lookup"><span data-stu-id="b0488-220">**Server code calling client method without parameters**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample14.cs?highlight=5)]

<span data-ttu-id="b0488-221">**Code de WinRT Client pour la méthode appelée depuis le serveur sans paramètres ([voir des exemples WPF et Silverlight plus loin dans cette rubrique](#wpfsl))**</span><span class="sxs-lookup"><span data-stu-id="b0488-221">**WinRT Client code for method called from server without parameters ([see WPF and Silverlight examples later in this topic](#wpfsl))**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample15.cs)]

<a id="clientmethodswithparmtypes"></a>

### <a name="methods-with-parameters-specifying-the-parameter-types"></a><span data-ttu-id="b0488-222">Méthodes avec des paramètres, en spécifiant les types de paramètres</span><span class="sxs-lookup"><span data-stu-id="b0488-222">Methods with parameters, specifying the parameter types</span></span>

<span data-ttu-id="b0488-223">Si la méthode que vous traitez possède des paramètres, spécifiez les types des paramètres comme les types génériques de la `On` (méthode).</span><span class="sxs-lookup"><span data-stu-id="b0488-223">If the method you're handling has parameters, specify the types of the parameters as the generic types of the `On` method.</span></span> <span data-ttu-id="b0488-224">Il existe des surcharges génériques de la `On` méthode pour vous permettre de spécifier des paramètres jusqu'à 8 (4 sur Windows Phone 7).</span><span class="sxs-lookup"><span data-stu-id="b0488-224">There are generic overloads of the `On` method to enable you to specify up to 8 parameters (4 on Windows Phone 7).</span></span> <span data-ttu-id="b0488-225">Dans l’exemple suivant, un seul paramètre est envoyé à la `UpdateStockPrice` (méthode).</span><span class="sxs-lookup"><span data-stu-id="b0488-225">In the following example, one parameter is sent to the `UpdateStockPrice` method.</span></span>

<span data-ttu-id="b0488-226">**Appel de méthode de client avec un paramètre de code de serveur**</span><span class="sxs-lookup"><span data-stu-id="b0488-226">**Server code calling client method with a parameter**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample16.cs?highlight=3)]

<span data-ttu-id="b0488-227">**La classe de Stock utilisée pour le paramètre**</span><span class="sxs-lookup"><span data-stu-id="b0488-227">**The Stock class used for the parameter**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample17.cs)]

<span data-ttu-id="b0488-228">**Code de WinRT Client pour une méthode appelée depuis le serveur avec un paramètre ([voir des exemples WPF et Silverlight plus loin dans cette rubrique](#wpfsl))**</span><span class="sxs-lookup"><span data-stu-id="b0488-228">**WinRT Client code for a method called from server with a parameter ([see WPF and Silverlight examples later in this topic](#wpfsl))**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample18.cs?highlight=1,5)]

<a id="clientmethodswithdynamparms"></a>

### <a name="methods-with-parameters-specifying-dynamic-objects-for-the-parameters"></a><span data-ttu-id="b0488-229">Méthodes avec des paramètres, en spécifiant des objets dynamiques pour les paramètres</span><span class="sxs-lookup"><span data-stu-id="b0488-229">Methods with parameters, specifying dynamic objects for the parameters</span></span>

<span data-ttu-id="b0488-230">Comme alternative à la spécification des paramètres comme des types génériques de la `On` (méthode), vous pouvez spécifier des paramètres en tant qu’objets dynamiques :</span><span class="sxs-lookup"><span data-stu-id="b0488-230">As an alternative to specifying parameters as generic types of the `On` method, you can specify parameters as dynamic objects:</span></span>

<span data-ttu-id="b0488-231">**Appel de méthode de client avec un paramètre de code de serveur**</span><span class="sxs-lookup"><span data-stu-id="b0488-231">**Server code calling client method with a parameter**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample19.cs?highlight=3)]

<span data-ttu-id="b0488-232">**La classe de Stock utilisée pour le paramètre**</span><span class="sxs-lookup"><span data-stu-id="b0488-232">**The Stock class used for the parameter**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample20.cs)]

<span data-ttu-id="b0488-233">**Code de WinRT Client pour une méthode appelée depuis le serveur avec un paramètre, à l’aide d’un objet dynamique pour le paramètre ([voir des exemples WPF et Silverlight plus loin dans cette rubrique](#wpfsl))**</span><span class="sxs-lookup"><span data-stu-id="b0488-233">**WinRT Client code for a method called from server with a parameter, using a dynamic object for the parameter ([see WPF and Silverlight examples later in this topic](#wpfsl))**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample21.cs?highlight=1,5)]

<a id="removehandler"></a>

### <a name="how-to-remove-a-handler"></a><span data-ttu-id="b0488-234">Comment supprimer un gestionnaire</span><span class="sxs-lookup"><span data-stu-id="b0488-234">How to remove a handler</span></span>

<span data-ttu-id="b0488-235">Pour supprimer un gestionnaire, appelez sa `Dispose` (méthode).</span><span class="sxs-lookup"><span data-stu-id="b0488-235">To remove a handler, call its `Dispose` method.</span></span>

<span data-ttu-id="b0488-236">**Code client pour une méthode appelée à partir du serveur**</span><span class="sxs-lookup"><span data-stu-id="b0488-236">**Client code for a method called from server**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample22.cs?highlight=1)]

<span data-ttu-id="b0488-237">**Code client pour supprimer le Gestionnaire**</span><span class="sxs-lookup"><span data-stu-id="b0488-237">**Client code to remove the handler**</span></span>

[!code-css[Main](signalr-1x-hubs-api-guide-net-client/samples/sample23.css?highlight=1)]

<a id="callserver"></a>

## <a name="how-to-call-server-methods-from-the-client"></a><span data-ttu-id="b0488-238">Comment appeler des méthodes de serveur à partir du client</span><span class="sxs-lookup"><span data-stu-id="b0488-238">How to call server methods from the client</span></span>

<span data-ttu-id="b0488-239">Pour appeler une méthode sur le serveur, utilisez le `Invoke` méthode sur le concentrateur proxy.</span><span class="sxs-lookup"><span data-stu-id="b0488-239">To call a method on the server, use the `Invoke` method on the Hub proxy.</span></span>

<span data-ttu-id="b0488-240">Si la méthode de serveur n’a aucune valeur de retour, utilisez la surcharge non générique de la `Invoke` (méthode).</span><span class="sxs-lookup"><span data-stu-id="b0488-240">If the server method has no return value, use the non-generic overload of the `Invoke` method.</span></span>

<span data-ttu-id="b0488-241">**Code de serveur pour une méthode qui ne retourne aucune valeur**</span><span class="sxs-lookup"><span data-stu-id="b0488-241">**Server code for a method that has no return value**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample24.cs?highlight=3)]

<span data-ttu-id="b0488-242">**Code client appelant une méthode qui ne retourne aucune valeur**</span><span class="sxs-lookup"><span data-stu-id="b0488-242">**Client code calling a method that has no return value**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample25.cs?highlight=1)]

<span data-ttu-id="b0488-243">Si la méthode de serveur possède une valeur de retour, spécifiez le type de retour en tant que le type générique de la `Invoke` (méthode).</span><span class="sxs-lookup"><span data-stu-id="b0488-243">If the server method has a return value, specify the return type as the generic type of the `Invoke` method.</span></span>

<span data-ttu-id="b0488-244">**Code de serveur pour une méthode qui a une valeur de retour et accepte un paramètre de type complexe**</span><span class="sxs-lookup"><span data-stu-id="b0488-244">**Server code for a method that has a return value and takes a complex type parameter**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample26.cs?highlight=1)]

<span data-ttu-id="b0488-245">**La classe de Stock utilisée pour le paramètre et la valeur de retour**</span><span class="sxs-lookup"><span data-stu-id="b0488-245">**The Stock class used for the parameter and return value**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample27.cs)]

<span data-ttu-id="b0488-246">**Code client appelant une méthode qui a une valeur de retour et prend un paramètre de type complexe, dans une méthode async de ASP.NET 4.5**</span><span class="sxs-lookup"><span data-stu-id="b0488-246">**Client code calling a method that has a return value and takes a complex type parameter, in an ASP.NET 4.5 async method**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample28.cs?highlight=1-2)]

<span data-ttu-id="b0488-247">**Code client appelant une méthode qui a une valeur de retour et prend un paramètre de type complexe, dans une méthode synchrone**</span><span class="sxs-lookup"><span data-stu-id="b0488-247">**Client code calling a method that has a return value and takes a complex type parameter, in a synchronous method**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample29.cs?highlight=1-2)]

<span data-ttu-id="b0488-248">Le `Invoke` méthode exécute de façon asynchrone et retourne un `Task` objet.</span><span class="sxs-lookup"><span data-stu-id="b0488-248">The `Invoke` method executes asynchronously and returns a `Task` object.</span></span> <span data-ttu-id="b0488-249">Si vous ne spécifiez pas `await` ou `.Wait()`, la ligne de code suivante s’exécute avant que la méthode que vous appelez ait terminé l’exécution.</span><span class="sxs-lookup"><span data-stu-id="b0488-249">If you don't specify `await` or `.Wait()`, the next line of code will execute before the method that you invoke has finished executing.</span></span>

<a id="connectionlifetime"></a>

## <a name="how-to-handle-connection-lifetime-events"></a><span data-ttu-id="b0488-250">Comment gérer les événements de durée de vie de connexion</span><span class="sxs-lookup"><span data-stu-id="b0488-250">How to handle connection lifetime events</span></span>

<span data-ttu-id="b0488-251">SignalR fournit des événements de durée de vie que vous pouvez gérer la connexion suivante :</span><span class="sxs-lookup"><span data-stu-id="b0488-251">SignalR provides the following connection lifetime events that you can handle:</span></span>

- <span data-ttu-id="b0488-252">`Received`: Déclenché lorsque toutes les données sont reçues sur la connexion.</span><span class="sxs-lookup"><span data-stu-id="b0488-252">`Received`: Raised when any data is received on the connection.</span></span> <span data-ttu-id="b0488-253">Fournit les données reçues.</span><span class="sxs-lookup"><span data-stu-id="b0488-253">Provides the received data.</span></span>
- <span data-ttu-id="b0488-254">`ConnectionSlow`: Déclenché lorsque le client détecte une connexion lente ou suppression fréquemment.</span><span class="sxs-lookup"><span data-stu-id="b0488-254">`ConnectionSlow`: Raised when the client detects a slow or frequently dropping connection.</span></span>
- <span data-ttu-id="b0488-255">`Reconnecting`: Déclenché lorsque le transport sous-jacent commence à se reconnecter.</span><span class="sxs-lookup"><span data-stu-id="b0488-255">`Reconnecting`: Raised when the underlying transport begins reconnecting.</span></span>
- <span data-ttu-id="b0488-256">`Reconnected`: Déclenché lorsque le transport sous-jacent s’est reconnecté.</span><span class="sxs-lookup"><span data-stu-id="b0488-256">`Reconnected`: Raised when the underlying transport has reconnected.</span></span>
- <span data-ttu-id="b0488-257">`StateChanged`: Déclenché lorsque l’état de connexion change.</span><span class="sxs-lookup"><span data-stu-id="b0488-257">`StateChanged`: Raised when the connection state changes.</span></span> <span data-ttu-id="b0488-258">Fournit l’ancien état et le nouvel état.</span><span class="sxs-lookup"><span data-stu-id="b0488-258">Provides the old state and the new state.</span></span> <span data-ttu-id="b0488-259">Pour plus d’informations sur la connexion, valeurs d’état consultez [énumération ConnectionState](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.connectionstate(v=vs.111).aspx).</span><span class="sxs-lookup"><span data-stu-id="b0488-259">For information about connection state values see [ConnectionState Enumeration](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.connectionstate(v=vs.111).aspx).</span></span>
- <span data-ttu-id="b0488-260">`Closed`: Déclenché lors de la connexion a déconnecté.</span><span class="sxs-lookup"><span data-stu-id="b0488-260">`Closed`: Raised when the connection has disconnected.</span></span>

<span data-ttu-id="b0488-261">Par exemple, si vous souhaitez afficher les messages d’avertissement pour les erreurs qui ne sont pas irrécupérable mais entraînent des problèmes de connexion intermittents, par exemple que lenteur ou fréquentes suppression de la connexion, gèrent le `ConnectionSlow` événement.</span><span class="sxs-lookup"><span data-stu-id="b0488-261">For example, if you want to display warning messages for errors that are not fatal but cause intermittent connection problems, such as slowness or frequent dropping of the connection, handle the `ConnectionSlow` event.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample30.cs)]

<span data-ttu-id="b0488-262">Pour plus d’informations, consultez [compréhension et gestion des événements de durée de vie des connexions dans SignalR](../guide-to-the-api/handling-connection-lifetime-events.md).</span><span class="sxs-lookup"><span data-stu-id="b0488-262">For more information, see [Understanding and Handling Connection Lifetime Events in SignalR](../guide-to-the-api/handling-connection-lifetime-events.md).</span></span>

<a id="handleerrors"></a>

## <a name="how-to-handle-errors"></a><span data-ttu-id="b0488-263">Comment gérer les erreurs</span><span class="sxs-lookup"><span data-stu-id="b0488-263">How to handle errors</span></span>

<span data-ttu-id="b0488-264">Si vous n’activez explicitement les messages d’erreur détaillés sur le serveur, l’objet d’exception SignalR retourne après une erreur contient un minimum d’informations sur l’erreur.</span><span class="sxs-lookup"><span data-stu-id="b0488-264">If you don't explicitly enable detailed error messages on the server, the exception object that SignalR returns after an error contains minimal information about the error.</span></span> <span data-ttu-id="b0488-265">Par exemple, si un appel à `newContosoChatMessage` échoue, le message d’erreur dans l’objet d’erreur contient «`There was an error invoking Hub method 'contosoChatHub.newContosoChatMessage'.`« envoi de messages d’erreur détaillés aux clients en production n’est pas recommandé pour des raisons de sécurité, mais si vous souhaitez activer les messages d’erreur détaillés pour à des fins de résolution des problèmes, utilisez le code suivant sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="b0488-265">For example, if a call to `newContosoChatMessage` fails, the error message in the error object contains "`There was an error invoking Hub method 'contosoChatHub.newContosoChatMessage'.`" Sending detailed error messages to clients in production is not recommended for security reasons, but if you want to enable detailed error messages for troubleshooting purposes, use the following code on the server.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample31.cs?highlight=2)]

<a id="handleerrors"></a>

<span data-ttu-id="b0488-266">Pour gérer les erreurs qui déclenche SignalR, vous pouvez ajouter un gestionnaire pour le `Error` événement sur l’objet de connexion.</span><span class="sxs-lookup"><span data-stu-id="b0488-266">To handle errors that SignalR raises, you can add a handler for the `Error` event on the connection object.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample32.cs)]

<span data-ttu-id="b0488-267">Pour gérer les erreurs à partir des appels de méthode, encapsuler le code dans un bloc try-catch.</span><span class="sxs-lookup"><span data-stu-id="b0488-267">To handle errors from method invocations, wrap the code in a try-catch block.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample33.cs)]

<a id="logging"></a>

## <a name="how-to-enable-client-side-logging"></a><span data-ttu-id="b0488-268">Comment activer la journalisation côté client</span><span class="sxs-lookup"><span data-stu-id="b0488-268">How to enable client-side logging</span></span>

<span data-ttu-id="b0488-269">Pour activer la journalisation côté client, définissez la `TraceLevel` et `TraceWriter` propriétés sur l’objet de connexion.</span><span class="sxs-lookup"><span data-stu-id="b0488-269">To enable client-side logging, set the `TraceLevel` and `TraceWriter` properties on the connection object.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample34.cs?highlight=2-3)]

<a id="wpfsl"></a>

## <a name="wpf-silverlight-and-console-application-code-samples-for-client-methods-that-the-server-can-call"></a><span data-ttu-id="b0488-270">WPF, Silverlight et exemples de code d’application console pour les méthodes de client que le serveur peut appeler.</span><span class="sxs-lookup"><span data-stu-id="b0488-270">WPF, Silverlight, and console application code samples for client methods that the server can call</span></span>

<span data-ttu-id="b0488-271">Les exemples de code indiqués plus haut pour définir des méthodes de client que le serveur peut appeler s’appliquent aux clients de WinRT.</span><span class="sxs-lookup"><span data-stu-id="b0488-271">The code samples shown earlier for defining client methods that the server can call apply to WinRT clients.</span></span> <span data-ttu-id="b0488-272">Les exemples suivants montrent le code équivalent pour WPF, Silverlight et clients d’application console.</span><span class="sxs-lookup"><span data-stu-id="b0488-272">The following samples show the equivalent code for WPF, Silverlight, and console application clients.</span></span>

### <a name="methods-without-parameters"></a><span data-ttu-id="b0488-273">Méthodes sans paramètres</span><span class="sxs-lookup"><span data-stu-id="b0488-273">Methods without parameters</span></span>

<span data-ttu-id="b0488-274">**Code de client WPF pour la méthode appelée à partir du serveur sans paramètres**</span><span class="sxs-lookup"><span data-stu-id="b0488-274">**WPF client code for method called from server without parameters**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample35.cs?highlight=1)]

<span data-ttu-id="b0488-275">**Code de client Silverlight pour la méthode appelée à partir du serveur sans paramètres**</span><span class="sxs-lookup"><span data-stu-id="b0488-275">**Silverlight client code for method called from server without parameters**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample36.cs?highlight=1)]

<span data-ttu-id="b0488-276">**Code du client d’application console pour la méthode appelée depuis le serveur sans paramètres**</span><span class="sxs-lookup"><span data-stu-id="b0488-276">**Console application client code for method called from server without parameters**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample37.cs?highlight=1)]

### <a name="methods-with-parameters-specifying-the-parameter-types"></a><span data-ttu-id="b0488-277">Méthodes avec des paramètres, en spécifiant les types de paramètres</span><span class="sxs-lookup"><span data-stu-id="b0488-277">Methods with parameters, specifying the parameter types</span></span>

<span data-ttu-id="b0488-278">**Code de client WPF pour une méthode appelée à partir du serveur avec un paramètre**</span><span class="sxs-lookup"><span data-stu-id="b0488-278">**WPF client code for a method called from server with a parameter**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample38.cs?highlight=1,4)]

<span data-ttu-id="b0488-279">**Code de client Silverlight pour une méthode appelée à partir du serveur avec un paramètre**</span><span class="sxs-lookup"><span data-stu-id="b0488-279">**Silverlight client code for a method called from server with a parameter**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample39.cs?highlight=1,5)]

<span data-ttu-id="b0488-280">**Code du client d’application console pour une méthode appelée depuis le serveur avec un paramètre**</span><span class="sxs-lookup"><span data-stu-id="b0488-280">**Console application client code for a method called from server with a parameter**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample40.cs?highlight=1-2)]

### <a name="methods-with-parameters-specifying-dynamic-objects-for-the-parameters"></a><span data-ttu-id="b0488-281">Méthodes avec des paramètres, en spécifiant des objets dynamiques pour les paramètres</span><span class="sxs-lookup"><span data-stu-id="b0488-281">Methods with parameters, specifying dynamic objects for the parameters</span></span>

<span data-ttu-id="b0488-282">**Code de client WPF pour une méthode appelée à partir du serveur avec un paramètre, à l’aide d’un objet dynamique pour le paramètre**</span><span class="sxs-lookup"><span data-stu-id="b0488-282">**WPF client code for a method called from server with a parameter, using a dynamic object for the parameter**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample41.cs?highlight=1,4)]

<span data-ttu-id="b0488-283">**Code de client Silverlight pour une méthode appelée à partir du serveur avec un paramètre, à l’aide d’un objet dynamique pour le paramètre**</span><span class="sxs-lookup"><span data-stu-id="b0488-283">**Silverlight client code for a method called from server with a parameter, using a dynamic object for the parameter**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample42.cs?highlight=1,5)]

<span data-ttu-id="b0488-284">**Code du client d’application console pour une méthode appelée depuis le serveur avec un paramètre, à l’aide d’un objet dynamique pour le paramètre**</span><span class="sxs-lookup"><span data-stu-id="b0488-284">**Console application client code for a method called from server with a parameter, using a dynamic object for the parameter**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample43.cs?highlight=1-2)]
