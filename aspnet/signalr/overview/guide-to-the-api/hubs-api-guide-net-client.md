---
uid: signalr/overview/guide-to-the-api/hubs-api-guide-net-client
title: "Guide d’API ASP.NET SignalR concentrateurs - Client .NET (c#) | Documents Microsoft"
author: pfletcher
description: "Ce document fournit une introduction à l’utilisation de l’API de Hubs pour SignalR version 2, les clients .NET, tels que le Windows Store (WinRT), WPF, Silverlight et les inconvénients..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: 6d02d9f7-94e5-4140-9f51-5a6040f274f6
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/guide-to-the-api/hubs-api-guide-net-client
msc.type: authoredcontent
ms.openlocfilehash: a03c8c42622a768d706acf5ac1f23b37a830d426
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/10/2017
---
<a name="aspnet-signalr-hubs-api-guide---net-client-c"></a><span data-ttu-id="f0ae4-103">Guide d’API ASP.NET SignalR concentrateurs - Client .NET (c#)</span><span class="sxs-lookup"><span data-stu-id="f0ae4-103">ASP.NET SignalR Hubs API Guide - .NET Client (C#)</span></span>
====================
<span data-ttu-id="f0ae4-104">par [Patrick Fletcher](https://github.com/pfletcher), [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="f0ae4-104">by [Patrick Fletcher](https://github.com/pfletcher), [Tom Dykstra](https://github.com/tdykstra)</span></span>

> <span data-ttu-id="f0ae4-105">Ce document fournit une introduction à l’utilisation de l’API de Hubs pour SignalR version 2, les clients .NET, tels que le Windows Store (WinRT), WPF, Silverlight et les applications de console.</span><span class="sxs-lookup"><span data-stu-id="f0ae4-105">This document provides an introduction to using the Hubs API for SignalR version 2 in .NET clients, such as Windows Store (WinRT), WPF, Silverlight, and console applications.</span></span>
> 
> <span data-ttu-id="f0ae4-106">L’API de concentrateurs SignalR vous permet de vous permettent d’effectuer des appels de procédure distante (RPC) à partir d’un serveur pour les clients connectés et à partir de clients sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="f0ae4-106">The SignalR Hubs API enables you to make remote procedure calls (RPCs) from a server to connected clients and from clients to the server.</span></span> <span data-ttu-id="f0ae4-107">Dans le code serveur, vous définissez les méthodes qui peuvent être appelées par les clients, et vous appelez des méthodes qui s’exécutent sur le client.</span><span class="sxs-lookup"><span data-stu-id="f0ae4-107">In server code, you define methods that can be called by clients, and you call methods that run on the client.</span></span> <span data-ttu-id="f0ae4-108">Dans le code client, vous définissez les méthodes qui peuvent être appelées à partir du serveur, et vous appelez des méthodes qui s’exécutent sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="f0ae4-108">In client code, you define methods that can be called from the server, and you call methods that run on the server.</span></span> <span data-ttu-id="f0ae4-109">SignalR prend en charge de tous les éléments client-serveur pour vous.</span><span class="sxs-lookup"><span data-stu-id="f0ae4-109">SignalR takes care of all of the client-to-server plumbing for you.</span></span>
> 
> <span data-ttu-id="f0ae4-110">SignalR offre également une API de niveau inférieur appelée connexions persistantes.</span><span class="sxs-lookup"><span data-stu-id="f0ae4-110">SignalR also offers a lower-level API called Persistent Connections.</span></span> <span data-ttu-id="f0ae4-111">Pour obtenir une présentation SignalR, des concentrateurs et des connexions persistantes, ou pour obtenir un didacticiel qui montre comment générer une application SignalR complète, consultez [SignalR - mise en route](../getting-started/index.md).</span><span class="sxs-lookup"><span data-stu-id="f0ae4-111">For an introduction to SignalR, Hubs, and Persistent Connections, or for a tutorial that shows how to build a complete SignalR application, see [SignalR - Getting Started](../getting-started/index.md).</span></span>
> 
> ## <a name="software-versions-used-in-this-topic"></a><span data-ttu-id="f0ae4-112">Versions du logiciel utilisées dans cette rubrique</span><span class="sxs-lookup"><span data-stu-id="f0ae4-112">Software versions used in this topic</span></span>
> 
> 
> - [<span data-ttu-id="f0ae4-113">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="f0ae4-113">Visual Studio 2013</span></span>](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - <span data-ttu-id="f0ae4-114">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="f0ae4-114">.NET 4.5</span></span>
> - <span data-ttu-id="f0ae4-115">SignalR version 2</span><span class="sxs-lookup"><span data-stu-id="f0ae4-115">SignalR version 2</span></span>
>   
> 
> 
> ## <a name="previous-versions-of-this-topic"></a><span data-ttu-id="f0ae4-116">Versions précédentes de cette rubrique</span><span class="sxs-lookup"><span data-stu-id="f0ae4-116">Previous versions of this topic</span></span>
> 
> <span data-ttu-id="f0ae4-117">Pour plus d’informations sur les versions antérieures de SignalR, consultez [SignalR des Versions antérieures](../older-versions/index.md).</span><span class="sxs-lookup"><span data-stu-id="f0ae4-117">For information about earlier versions of SignalR, see [SignalR Older Versions](../older-versions/index.md).</span></span>
> 
> ## <a name="questions-and-comments"></a><span data-ttu-id="f0ae4-118">Questions et des commentaires</span><span class="sxs-lookup"><span data-stu-id="f0ae4-118">Questions and comments</span></span>
> 
> <span data-ttu-id="f0ae4-119">Veuillez laisser des commentaires sur la façon dont vous avez aimé ce didacticiel et nous pouvons améliorer dans les commentaires en bas de la page.</span><span class="sxs-lookup"><span data-stu-id="f0ae4-119">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="f0ae4-120">Si vous avez des questions qui ne sont pas directement liées à ce didacticiel, vous pouvez les valider pour le [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) ou [StackOverflow.com](http://stackoverflow.com/).</span><span class="sxs-lookup"><span data-stu-id="f0ae4-120">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>


## <a name="overview"></a><span data-ttu-id="f0ae4-121">Vue d'ensemble</span><span class="sxs-lookup"><span data-stu-id="f0ae4-121">Overview</span></span>

<span data-ttu-id="f0ae4-122">Ce document contient les sections suivantes :</span><span class="sxs-lookup"><span data-stu-id="f0ae4-122">This document contains the following sections:</span></span>

- [<span data-ttu-id="f0ae4-123">Programme d’installation du client</span><span class="sxs-lookup"><span data-stu-id="f0ae4-123">Client Setup</span></span>](#clientsetup)
- [<span data-ttu-id="f0ae4-124">Comment établir une connexion</span><span class="sxs-lookup"><span data-stu-id="f0ae4-124">How to establish a connection</span></span>](#establishconnection)

    - [<span data-ttu-id="f0ae4-125">Connexions entre les domaines à partir de clients Silverlight</span><span class="sxs-lookup"><span data-stu-id="f0ae4-125">Cross-domain connections from Silverlight clients</span></span>](#slcrossdomain)
- [<span data-ttu-id="f0ae4-126">Comment configurer la connexion</span><span class="sxs-lookup"><span data-stu-id="f0ae4-126">How to configure the connection</span></span>](#configureconnection)

    - [<span data-ttu-id="f0ae4-127">Comment définir le nombre maximal de connexions simultanées dans les clients WPF</span><span class="sxs-lookup"><span data-stu-id="f0ae4-127">How to set the maximum number of concurrent connections in WPF clients</span></span>](#maxconnections)
    - [<span data-ttu-id="f0ae4-128">Comment spécifier des paramètres de chaîne de requête</span><span class="sxs-lookup"><span data-stu-id="f0ae4-128">How to specify query string parameters</span></span>](#querystring)
    - [<span data-ttu-id="f0ae4-129">Comment spécifier le mode de transport</span><span class="sxs-lookup"><span data-stu-id="f0ae4-129">How to specify the transport method</span></span>](#transport)
    - [<span data-ttu-id="f0ae4-130">Comment spécifier des en-têtes HTTP</span><span class="sxs-lookup"><span data-stu-id="f0ae4-130">How to specify HTTP headers</span></span>](#httpheaders)
    - [<span data-ttu-id="f0ae4-131">Comment spécifier des certificats clients</span><span class="sxs-lookup"><span data-stu-id="f0ae4-131">How to specify client certificates</span></span>](#clientcertificate)
- [<span data-ttu-id="f0ae4-132">Comment créer le concentrateur proxy.</span><span class="sxs-lookup"><span data-stu-id="f0ae4-132">How to create the Hub proxy</span></span>](#proxy)
- [<span data-ttu-id="f0ae4-133">Comment définir des méthodes sur le client que le serveur peut appeler.</span><span class="sxs-lookup"><span data-stu-id="f0ae4-133">How to define methods on the client that the server can call</span></span>](#callclient)

    - [<span data-ttu-id="f0ae4-134">Méthodes sans paramètres</span><span class="sxs-lookup"><span data-stu-id="f0ae4-134">Methods without parameters</span></span>](#clientmethodswithoutparms)
    - [<span data-ttu-id="f0ae4-135">Méthodes avec des paramètres, en spécifiant les types de paramètres</span><span class="sxs-lookup"><span data-stu-id="f0ae4-135">Methods with parameters, specifying parameter types</span></span>](#clientmethodswithparmtypes)
    - [<span data-ttu-id="f0ae4-136">Méthodes avec des paramètres, en spécifiant des objets dynamiques pour les paramètres</span><span class="sxs-lookup"><span data-stu-id="f0ae4-136">Methods with parameters, specifying dynamic objects for the parameters</span></span>](#clientmethodswithdynamparms)
    - [<span data-ttu-id="f0ae4-137">Comment supprimer un gestionnaire</span><span class="sxs-lookup"><span data-stu-id="f0ae4-137">How to remove a handler</span></span>](#removehandler)
- [<span data-ttu-id="f0ae4-138">Comment appeler des méthodes de serveur à partir du client</span><span class="sxs-lookup"><span data-stu-id="f0ae4-138">How to call server methods from the client</span></span>](#callserver)
- [<span data-ttu-id="f0ae4-139">Comment gérer les événements de durée de vie de connexion</span><span class="sxs-lookup"><span data-stu-id="f0ae4-139">How to handle connection lifetime events</span></span>](#connectionlifetime)
- [<span data-ttu-id="f0ae4-140">Comment gérer les erreurs</span><span class="sxs-lookup"><span data-stu-id="f0ae4-140">How to handle errors</span></span>](#handleerrors)
- [<span data-ttu-id="f0ae4-141">Comment activer la journalisation côté client</span><span class="sxs-lookup"><span data-stu-id="f0ae4-141">How to enable client-side logging</span></span>](#logging)
- [<span data-ttu-id="f0ae4-142">Exemples de code d’application console pour les méthodes de client que le serveur peut appeler, WPF et Silverlight</span><span class="sxs-lookup"><span data-stu-id="f0ae4-142">WPF, Silverlight, and console application code samples for client methods that the server can call</span></span>](#wpfsl)

<span data-ttu-id="f0ae4-143">Pour un projet de client exemple .NET, consultez les ressources suivantes :</span><span class="sxs-lookup"><span data-stu-id="f0ae4-143">For a sample .NET client projects, see the following resources:</span></span>

- <span data-ttu-id="f0ae4-144">[Gustave-armenta / SignalR-Samples](https://github.com/gustavo-armenta/SignalR-Samples) sur GitHub.com (exemples d’application console de WinRT, Silverlight,).</span><span class="sxs-lookup"><span data-stu-id="f0ae4-144">[gustavo-armenta / SignalR-Samples](https://github.com/gustavo-armenta/SignalR-Samples) on GitHub.com (WinRT, Silverlight, console app examples).</span></span>
- <span data-ttu-id="f0ae4-145">[DamianEdwards / SignalR-MoveShapeDemo / MoveShape.Desktop](https://github.com/DamianEdwards/SignalR-MoveShapeDemo/tree/master/MoveShape/MoveShape.Desktop) sur GitHub.com (par exemple, WPF).</span><span class="sxs-lookup"><span data-stu-id="f0ae4-145">[DamianEdwards / SignalR-MoveShapeDemo / MoveShape.Desktop](https://github.com/DamianEdwards/SignalR-MoveShapeDemo/tree/master/MoveShape/MoveShape.Desktop) on GitHub.com (WPF example).</span></span>
- <span data-ttu-id="f0ae4-146">[SignalR / Microsoft.AspNet.SignalR.Client.Samples](https://github.com/SignalR/SignalR/tree/master/samples/Microsoft.AspNet.SignalR.Client.Samples) sur GitHub.com (exemple d’application Console).</span><span class="sxs-lookup"><span data-stu-id="f0ae4-146">[SignalR / Microsoft.AspNet.SignalR.Client.Samples](https://github.com/SignalR/SignalR/tree/master/samples/Microsoft.AspNet.SignalR.Client.Samples) on GitHub.com (Console app example).</span></span>

<span data-ttu-id="f0ae4-147">Pour plus d’informations sur la façon de programmer le serveur ou les clients JavaScript, consultez les ressources suivantes :</span><span class="sxs-lookup"><span data-stu-id="f0ae4-147">For documentation on how to program the server or JavaScript clients, see the following resources:</span></span>

- [<span data-ttu-id="f0ae4-148">Guide d’API concentrateurs SignalR - serveur</span><span class="sxs-lookup"><span data-stu-id="f0ae4-148">SignalR Hubs API Guide - Server</span></span>](hubs-api-guide-server.md)
- [<span data-ttu-id="f0ae4-149">Guide d’API concentrateurs SignalR - Client JavaScript</span><span class="sxs-lookup"><span data-stu-id="f0ae4-149">SignalR Hubs API Guide - JavaScript Client</span></span>](hubs-api-guide-javascript-client.md)

<span data-ttu-id="f0ae4-150">Des liens vers des rubriques de référence de l’API sont à la version de .NET 4.5 de l’API.</span><span class="sxs-lookup"><span data-stu-id="f0ae4-150">Links to API Reference topics are to the .NET 4.5 version of the API.</span></span> <span data-ttu-id="f0ae4-151">Si vous utilisez le .NET 4, consultez [la version de .NET 4 des rubriques API](https://msdn.microsoft.com/en-us/library/jj891075(v=vs.100).aspx).</span><span class="sxs-lookup"><span data-stu-id="f0ae4-151">If you're using .NET 4, see [the .NET 4 version of the API topics](https://msdn.microsoft.com/en-us/library/jj891075(v=vs.100).aspx).</span></span>

<a id="clientsetup"></a>

## <a name="client-setup"></a><span data-ttu-id="f0ae4-152">Programme d’installation du client</span><span class="sxs-lookup"><span data-stu-id="f0ae4-152">Client setup</span></span>

<span data-ttu-id="f0ae4-153">Installer le [Microsoft.AspNet.SignalR.Client](http://nuget.org/packages/Microsoft.AspNet.SignalR.Client) package NuGet (pas le [Microsoft.AspNet.SignalR](http://nuget.org/packages/microsoft.aspnet.signalr) package).</span><span class="sxs-lookup"><span data-stu-id="f0ae4-153">Install the [Microsoft.AspNet.SignalR.Client](http://nuget.org/packages/Microsoft.AspNet.SignalR.Client) NuGet package (not the [Microsoft.AspNet.SignalR](http://nuget.org/packages/microsoft.aspnet.signalr) package).</span></span> <span data-ttu-id="f0ae4-154">Ce package prend en charge WinRT, Silverlight, WPF, les applications console et les clients Windows Phone, pour .NET 4 et 4.5 de .NET.</span><span class="sxs-lookup"><span data-stu-id="f0ae4-154">This package supports WinRT, Silverlight, WPF, console application, and Windows Phone clients, for both .NET 4 and .NET 4.5.</span></span>

<span data-ttu-id="f0ae4-155">Si la version de SignalR que vous disposez sur le client est différente de la version que vous disposez sur le serveur, SignalR est souvent capable de s’adapter à la différence.</span><span class="sxs-lookup"><span data-stu-id="f0ae4-155">If the version of SignalR that you have on the client is different from the version that you have on the server, SignalR is often able to adapt to the difference.</span></span> <span data-ttu-id="f0ae4-156">Par exemple, un serveur exécutant la version 2 de SignalR prendra en charge les clients qui ont installé des 1.1.x, ainsi que les clients dont la version 2 est installé.</span><span class="sxs-lookup"><span data-stu-id="f0ae4-156">For example, a server running SignalR version 2 will support clients that have 1.1.x installed as well as clients that have version 2 installed.</span></span> <span data-ttu-id="f0ae4-157">Si la différence entre la version sur le serveur et la version sur le client est trop élevée, ou si le client est plus récent que le serveur, SignalR lève une `InvalidOperationException` exception lorsque le client tente d’établir une connexion.</span><span class="sxs-lookup"><span data-stu-id="f0ae4-157">If the difference between the version on the server and the version on the client is too great, or if the client is newer than the server, SignalR throws an `InvalidOperationException` exception when the client tries to establish a connection.</span></span> <span data-ttu-id="f0ae4-158">Le message d’erreur est «`You are using a version of the client that isn't compatible with the server. Client version X.X, server version X.X`».</span><span class="sxs-lookup"><span data-stu-id="f0ae4-158">The error message is "`You are using a version of the client that isn't compatible with the server. Client version X.X, server version X.X`".</span></span>

<a id="establishconnection"></a>

## <a name="how-to-establish-a-connection"></a><span data-ttu-id="f0ae4-159">Comment établir une connexion</span><span class="sxs-lookup"><span data-stu-id="f0ae4-159">How to establish a connection</span></span>

<span data-ttu-id="f0ae4-160">Avant de pouvoir établir une connexion, vous devez créer un `HubConnection` de l’objet et créer un proxy.</span><span class="sxs-lookup"><span data-stu-id="f0ae4-160">Before you can establish a connection, you have to create a `HubConnection` object and create a proxy.</span></span> <span data-ttu-id="f0ae4-161">Pour établir la connexion, appelez le `Start` méthode sur le `HubConnection` objet.</span><span class="sxs-lookup"><span data-stu-id="f0ae4-161">To establish the connection, call the `Start` method on the `HubConnection` object.</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample1.cs?highlight=1,4)]

> [!NOTE]
> <span data-ttu-id="f0ae4-162">Pour les clients JavaScript que vous inscrivez au moins un gestionnaire d’événements avant d’appeler le `Start` méthode pour établir la connexion.</span><span class="sxs-lookup"><span data-stu-id="f0ae4-162">For JavaScript clients you have to register at least one event handler before calling the `Start` method to establish the connection.</span></span> <span data-ttu-id="f0ae4-163">Cela n’est pas nécessaire pour les clients .NET.</span><span class="sxs-lookup"><span data-stu-id="f0ae4-163">This is not necessary for .NET clients.</span></span> <span data-ttu-id="f0ae4-164">Pour les clients JavaScript, le code proxy généré crée automatiquement des proxys pour tous les Hubs qui existent sur le serveur et l’inscription d’un gestionnaire est de vous indiquez comment les concentrateurs de votre client prévoit d’utiliser.</span><span class="sxs-lookup"><span data-stu-id="f0ae4-164">For JavaScript clients, the generated proxy code automatically creates proxies for all Hubs that exist on the server, and registering a handler is how you indicate which Hubs your client intends to use.</span></span> <span data-ttu-id="f0ae4-165">Mais pour un client .NET vous créer des proxys de Hub manuellement, donc SignalR part du principe que vous utilisez n’importe quel concentrateur que vous créez un proxy pour.</span><span class="sxs-lookup"><span data-stu-id="f0ae4-165">But for a .NET client you create Hub proxies manually, so SignalR assumes that you will be using any Hub that you create a proxy for.</span></span>


<span data-ttu-id="f0ae4-166">L’exemple de code utilise la valeur par défaut « / signalr « URL pour se connecter à votre service de SignalR.</span><span class="sxs-lookup"><span data-stu-id="f0ae4-166">The sample code uses the default "/signalr" URL to connect to your SignalR service.</span></span> <span data-ttu-id="f0ae4-167">Pour plus d’informations sur la façon de spécifier une autre URL de base, consultez [ASP.NET SignalR concentrateurs API Guide - Server - l’URL /signalr](hubs-api-guide-server.md#signalrurl).</span><span class="sxs-lookup"><span data-stu-id="f0ae4-167">For information about how to specify a different base URL, see [ASP.NET SignalR Hubs API Guide - Server - The /signalr URL](hubs-api-guide-server.md#signalrurl).</span></span>

<span data-ttu-id="f0ae4-168">Le `Start` méthode s’exécute de façon asynchrone.</span><span class="sxs-lookup"><span data-stu-id="f0ae4-168">The `Start` method executes asynchronously.</span></span> <span data-ttu-id="f0ae4-169">Pour vous assurer que les lignes de code ne s’exécutent jusqu'à une fois la connexion établie, utilisez `await` dans une méthode asynchrone ASP.NET 4.5 ou `.Wait()` dans une méthode synchrone.</span><span class="sxs-lookup"><span data-stu-id="f0ae4-169">To make sure that subsequent lines of code don't execute until after the connection is established, use `await` in an ASP.NET 4.5 asynchronous method or `.Wait()` in a synchronous method.</span></span> <span data-ttu-id="f0ae4-170">N’utilisez pas `.Wait()` dans un client WinRT.</span><span class="sxs-lookup"><span data-stu-id="f0ae4-170">Don't use `.Wait()` in a WinRT client.</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample2.cs?highlight=1)]

[!code-css[Main](hubs-api-guide-net-client/samples/sample3.css?highlight=1)]

<a id="slcrossdomain"></a>

### <a name="cross-domain-connections-from-silverlight-clients"></a><span data-ttu-id="f0ae4-171">Connexions entre les domaines à partir de clients Silverlight</span><span class="sxs-lookup"><span data-stu-id="f0ae4-171">Cross-domain connections from Silverlight clients</span></span>

<span data-ttu-id="f0ae4-172">Pour plus d’informations sur la façon d’activer les connexions inter-domaines à partir de clients de Silverlight, consultez [rendre un Service disponible entre les limites de domaine](https://msdn.microsoft.com/en-us/library/cc197955(v=vs.95).aspx).</span><span class="sxs-lookup"><span data-stu-id="f0ae4-172">For information about how to enable cross-domain connections from Silverlight clients, see [Making a Service Available Across Domain Boundaries](https://msdn.microsoft.com/en-us/library/cc197955(v=vs.95).aspx).</span></span>

<a id="configureconnection"></a>

## <a name="how-to-configure-the-connection"></a><span data-ttu-id="f0ae4-173">Comment configurer la connexion</span><span class="sxs-lookup"><span data-stu-id="f0ae4-173">How to configure the connection</span></span>

<span data-ttu-id="f0ae4-174">Avant d’établir une connexion, vous pouvez spécifier une des options suivantes :</span><span class="sxs-lookup"><span data-stu-id="f0ae4-174">Before you establish a connection, you can specify any of the following options:</span></span>

- <span data-ttu-id="f0ae4-175">Limite de connexions simultanées.</span><span class="sxs-lookup"><span data-stu-id="f0ae4-175">Concurrent connections limit.</span></span>
- <span data-ttu-id="f0ae4-176">Les paramètres de chaîne de requête.</span><span class="sxs-lookup"><span data-stu-id="f0ae4-176">Query string parameters.</span></span>
- <span data-ttu-id="f0ae4-177">Le mode de transport.</span><span class="sxs-lookup"><span data-stu-id="f0ae4-177">The transport method.</span></span>
- <span data-ttu-id="f0ae4-178">En-têtes HTTP.</span><span class="sxs-lookup"><span data-stu-id="f0ae4-178">HTTP headers.</span></span>
- <span data-ttu-id="f0ae4-179">Certificats clients.</span><span class="sxs-lookup"><span data-stu-id="f0ae4-179">Client certificates.</span></span>

<a id="maxconnections"></a>

### <a name="how-to-set-the-maximum-number-of-concurrent-connections-in-wpf-clients"></a><span data-ttu-id="f0ae4-180">Comment définir le nombre maximal de connexions simultanées dans les clients WPF</span><span class="sxs-lookup"><span data-stu-id="f0ae4-180">How to set the maximum number of concurrent connections in WPF clients</span></span>

<span data-ttu-id="f0ae4-181">Clients de WPF, vous devrez peut-être augmenter le nombre maximal de connexions simultanées à partir de sa valeur par défaut de 2.</span><span class="sxs-lookup"><span data-stu-id="f0ae4-181">In WPF clients, you might have to increase the maximum number of concurrent connections from its default value of 2.</span></span> <span data-ttu-id="f0ae4-182">La valeur recommandée est 10.</span><span class="sxs-lookup"><span data-stu-id="f0ae4-182">The recommended value is 10.</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample4.cs?highlight=4)]

<span data-ttu-id="f0ae4-183">Pour plus d’informations, consultez [ServicePointManager.DefaultConnectionLimit](https://msdn.microsoft.com/en-us/library/system.net.servicepointmanager.defaultconnectionlimit.aspx).</span><span class="sxs-lookup"><span data-stu-id="f0ae4-183">For more information, see [ServicePointManager.DefaultConnectionLimit](https://msdn.microsoft.com/en-us/library/system.net.servicepointmanager.defaultconnectionlimit.aspx).</span></span>

<a id="querystring"></a>

### <a name="how-to-specify-query-string-parameters"></a><span data-ttu-id="f0ae4-184">Comment spécifier des paramètres de chaîne de requête</span><span class="sxs-lookup"><span data-stu-id="f0ae4-184">How to specify query string parameters</span></span>

<span data-ttu-id="f0ae4-185">Si vous souhaitez envoyer des données sur le serveur lorsque le client se connecte, vous pouvez ajouter des paramètres de chaîne de requête à l’objet de connexion.</span><span class="sxs-lookup"><span data-stu-id="f0ae4-185">If you want to send data to the server when the client connects, you can add query string parameters to the connection object.</span></span> <span data-ttu-id="f0ae4-186">L’exemple suivant montre comment définir un paramètre de chaîne de requête dans le code client.</span><span class="sxs-lookup"><span data-stu-id="f0ae4-186">The following example shows how to set a query string parameter in client code.</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample5.cs)]

<span data-ttu-id="f0ae4-187">L’exemple suivant montre comment lire un paramètre de chaîne de requête dans le code serveur.</span><span class="sxs-lookup"><span data-stu-id="f0ae4-187">The following example shows how to read a query string parameter in server code.</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample6.cs?highlight=5)]

<a id="transport"></a>

### <a name="how-to-specify-the-transport-method"></a><span data-ttu-id="f0ae4-188">Comment spécifier le mode de transport</span><span class="sxs-lookup"><span data-stu-id="f0ae4-188">How to specify the transport method</span></span>

<span data-ttu-id="f0ae4-189">Dans le cadre du processus de connexion, un client SignalR est normalement négocie avec le serveur pour déterminer le transport meilleure prise en charge par le serveur et client.</span><span class="sxs-lookup"><span data-stu-id="f0ae4-189">As part of the process of connecting, a SignalR client normally negotiates with the server to determine the best transport that is supported by both server and client.</span></span> <span data-ttu-id="f0ae4-190">Si vous savez déjà que vous souhaitez utiliser le transport, vous pouvez ignorer ce processus de négociation.</span><span class="sxs-lookup"><span data-stu-id="f0ae4-190">If you already know which transport you want to use, you can bypass this negotiation process.</span></span> <span data-ttu-id="f0ae4-191">Pour spécifier le mode de transport, passez dans un objet de transport à la méthode Start.</span><span class="sxs-lookup"><span data-stu-id="f0ae4-191">To specify the transport method, pass in a transport object to the Start method.</span></span> <span data-ttu-id="f0ae4-192">L’exemple suivant montre comment spécifier le mode de transport dans le code client.</span><span class="sxs-lookup"><span data-stu-id="f0ae4-192">The following example shows how to specify the transport method in client code.</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample7.cs?highlight=4)]

<span data-ttu-id="f0ae4-193">Le [Microsoft.AspNet.SignalR.Client.Transports](https://msdn.microsoft.com/en-us/library/jj918090(v=vs.111).aspx) espace de noms inclut les classes suivantes qui vous permet de spécifier le transport.</span><span class="sxs-lookup"><span data-stu-id="f0ae4-193">The [Microsoft.AspNet.SignalR.Client.Transports](https://msdn.microsoft.com/en-us/library/jj918090(v=vs.111).aspx) namespace includes the following classes that you can use to specify the transport.</span></span>

- <span data-ttu-id="f0ae4-194">[LongPollingTransport](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.client.transports.longpollingtransport(v=vs.111).aspx)</span><span class="sxs-lookup"><span data-stu-id="f0ae4-194">[LongPollingTransport](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.client.transports.longpollingtransport(v=vs.111).aspx)</span></span>
- <span data-ttu-id="f0ae4-195">[ServerSentEventsTransport](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.client.transports.serversenteventstransport(v=vs.111).aspx)</span><span class="sxs-lookup"><span data-stu-id="f0ae4-195">[ServerSentEventsTransport](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.client.transports.serversenteventstransport(v=vs.111).aspx)</span></span>
- <span data-ttu-id="f0ae4-196">[WebSocketTransport](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.client.transports.websockettransport(v=vs.111).aspx) (disponible uniquement lorsque le serveur et le client utilisent .NET 4.5).</span><span class="sxs-lookup"><span data-stu-id="f0ae4-196">[WebSocketTransport](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.client.transports.websockettransport(v=vs.111).aspx) (Available only when both server and client use .NET 4.5.)</span></span>
- <span data-ttu-id="f0ae4-197">[AutoTransport](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.client.transports.autotransport(v=vs.111).aspx) (choisit automatiquement le transport meilleure prise en charge par le client et le serveur.</span><span class="sxs-lookup"><span data-stu-id="f0ae4-197">[AutoTransport](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.client.transports.autotransport(v=vs.111).aspx) (Automatically chooses the best transport that is supported by both the client and the server.</span></span> <span data-ttu-id="f0ae4-198">Il s’agit du transport par défaut.</span><span class="sxs-lookup"><span data-stu-id="f0ae4-198">This is the default transport.</span></span> <span data-ttu-id="f0ae4-199">En la passant dans pour la `Start` méthode a le même effet que passant ne pas quoi que ce soit.)</span><span class="sxs-lookup"><span data-stu-id="f0ae4-199">Passing this in to the `Start` method has the same effect as not passing in anything.)</span></span>

<span data-ttu-id="f0ae4-200">Le transport ForeverFrame n’est pas inclus dans cette liste, car elle est utilisée uniquement par les navigateurs.</span><span class="sxs-lookup"><span data-stu-id="f0ae4-200">The ForeverFrame transport is not included in this list because it is used only by browsers.</span></span>

<span data-ttu-id="f0ae4-201">Pour plus d’informations sur la vérification de la méthode de transport dans le code serveur, consultez [ASP.NET SignalR concentrateurs API Guide - Server - comment obtenir des informations sur le client à partir de la propriété de contexte](hubs-api-guide-server.md#contextproperty).</span><span class="sxs-lookup"><span data-stu-id="f0ae4-201">For information about how to check the transport method in server code, see [ASP.NET SignalR Hubs API Guide - Server - How to get information about the client from the Context property](hubs-api-guide-server.md#contextproperty).</span></span> <span data-ttu-id="f0ae4-202">Pour plus d’informations sur les transports et de secours, consultez [Introduction à SignalR - Transports et secours](../getting-started/introduction-to-signalr.md#transports).</span><span class="sxs-lookup"><span data-stu-id="f0ae4-202">For more information about transports and fallbacks, see [Introduction to SignalR - Transports and Fallbacks](../getting-started/introduction-to-signalr.md#transports).</span></span>

<a id="httpheaders"></a>

### <a name="how-to-specify-http-headers"></a><span data-ttu-id="f0ae4-203">Comment spécifier des en-têtes HTTP</span><span class="sxs-lookup"><span data-stu-id="f0ae4-203">How to specify HTTP headers</span></span>

<span data-ttu-id="f0ae4-204">Pour définir des en-têtes HTTP, utilisez le `Headers` propriété sur l’objet de connexion.</span><span class="sxs-lookup"><span data-stu-id="f0ae4-204">To set HTTP headers, use the `Headers` property on the connection object.</span></span> <span data-ttu-id="f0ae4-205">L’exemple suivant montre comment ajouter un en-tête HTTP.</span><span class="sxs-lookup"><span data-stu-id="f0ae4-205">The following example shows how to add an HTTP header.</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample8.cs?highlight=2)]

<a id="clientcertificate"></a>

### <a name="how-to-specify-client-certificates"></a><span data-ttu-id="f0ae4-206">Comment spécifier des certificats clients</span><span class="sxs-lookup"><span data-stu-id="f0ae4-206">How to specify client certificates</span></span>

<span data-ttu-id="f0ae4-207">Pour ajouter des certificats clients, utilisez la `AddClientCertificate` méthode sur l’objet de connexion.</span><span class="sxs-lookup"><span data-stu-id="f0ae4-207">To add client certificates, use the `AddClientCertificate` method on the connection object.</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample9.cs?highlight=2)]

<a id="proxy"></a>

## <a name="how-to-create-the-hub-proxy"></a><span data-ttu-id="f0ae4-208">Comment créer le concentrateur proxy.</span><span class="sxs-lookup"><span data-stu-id="f0ae4-208">How to create the Hub proxy</span></span>

<span data-ttu-id="f0ae4-209">Pour définir des méthodes sur le client un concentrateur peut appeler à partir du serveur et pour appeler des méthodes sur un concentrateur au niveau du serveur, créez un proxy pour le concentrateur en appelant `CreateHubProxy` sur l’objet de connexion.</span><span class="sxs-lookup"><span data-stu-id="f0ae4-209">In order to define methods on the client that a Hub can call from the server, and to invoke methods on a Hub at the server, create a proxy for the Hub by calling `CreateHubProxy` on the connection object.</span></span> <span data-ttu-id="f0ae4-210">La chaîne vous passez dans `CreateHubProxy` est le nom de votre classe de concentrateur ou le nom spécifié par le `HubName` attribut si celui utilisé sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="f0ae4-210">The string you pass in to `CreateHubProxy` is the name of your Hub class, or the name specified by the `HubName` attribute if one was used on the server.</span></span> <span data-ttu-id="f0ae4-211">Correspondance de nom respecte la casse.</span><span class="sxs-lookup"><span data-stu-id="f0ae4-211">Name matching is case-insensitive.</span></span>

<span data-ttu-id="f0ae4-212">**Classe de concentrateur sur le serveur**</span><span class="sxs-lookup"><span data-stu-id="f0ae4-212">**Hub class on server**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample10.cs?highlight=1)]

<span data-ttu-id="f0ae4-213">**Créer le proxy client pour la classe de concentrateur**</span><span class="sxs-lookup"><span data-stu-id="f0ae4-213">**Create client proxy for the Hub class**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample11.cs?highlight=2)]

<span data-ttu-id="f0ae4-214">Si vous ajoutez votre classe Hub avec un `HubName` d’attribut, utilisez ce nom.</span><span class="sxs-lookup"><span data-stu-id="f0ae4-214">If you decorate your Hub class with a `HubName` attribute, use that name.</span></span>

<span data-ttu-id="f0ae4-215">**Classe de concentrateur sur le serveur**</span><span class="sxs-lookup"><span data-stu-id="f0ae4-215">**Hub class on server**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample12.cs)]

<span data-ttu-id="f0ae4-216">**Créer le proxy client pour la classe de concentrateur**</span><span class="sxs-lookup"><span data-stu-id="f0ae4-216">**Create client proxy for the Hub class**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample13.cs?highlight=2)]

<span data-ttu-id="f0ae4-217">Si vous appelez `HubConnection.CreateHubProxy` plusieurs fois avec le même `hubName`, vous obtenez la même mise en cache `IHubProxy` objet.</span><span class="sxs-lookup"><span data-stu-id="f0ae4-217">If you call `HubConnection.CreateHubProxy` multiple times with the same `hubName`, you get the same cached `IHubProxy` object.</span></span>

<a id="callclient"></a>

## <a name="how-to-define-methods-on-the-client-that-the-server-can-call"></a><span data-ttu-id="f0ae4-218">Comment définir des méthodes sur le client que le serveur peut appeler.</span><span class="sxs-lookup"><span data-stu-id="f0ae4-218">How to define methods on the client that the server can call</span></span>

<span data-ttu-id="f0ae4-219">Pour définir une méthode que le serveur peut appeler, utilisez du proxy `On` méthode pour inscrire un gestionnaire d’événements.</span><span class="sxs-lookup"><span data-stu-id="f0ae4-219">To define a method that the server can call, use the proxy's `On` method to register an event handler.</span></span>

<span data-ttu-id="f0ae4-220">Correspondance de nom de méthode respecte la casse.</span><span class="sxs-lookup"><span data-stu-id="f0ae4-220">Method name matching is case-insensitive.</span></span> <span data-ttu-id="f0ae4-221">Par exemple, `Clients.All.UpdateStockPrice` sur le serveur s’exécute `updateStockPrice`, `updatestockprice`, ou `UpdateStockPrice` sur le client.</span><span class="sxs-lookup"><span data-stu-id="f0ae4-221">For example, `Clients.All.UpdateStockPrice` on the server will execute `updateStockPrice`, `updatestockprice`, or `UpdateStockPrice` on the client.</span></span>

<span data-ttu-id="f0ae4-222">Plates-formes clientes différentes ont des exigences différentes pour la façon dont vous écrivez le code de la méthode pour mettre à jour l’interface utilisateur.</span><span class="sxs-lookup"><span data-stu-id="f0ae4-222">Different client platforms have different requirements for how you write method code to update the UI.</span></span> <span data-ttu-id="f0ae4-223">Les exemples présentés sont pour les clients de WinRT (Windows Store .NET).</span><span class="sxs-lookup"><span data-stu-id="f0ae4-223">The examples shown are for WinRT (Windows Store .NET) clients.</span></span> <span data-ttu-id="f0ae4-224">Exemples d’application console, WPF et Silverlight sont fournies dans [une section distincte, plus loin dans cette rubrique](#wpfsl).</span><span class="sxs-lookup"><span data-stu-id="f0ae4-224">WPF, Silverlight, and console application examples are provided in [a separate section later in this topic](#wpfsl).</span></span>

<a id="clientmethodswithoutparms"></a>

### <a name="methods-without-parameters"></a><span data-ttu-id="f0ae4-225">Méthodes sans paramètres</span><span class="sxs-lookup"><span data-stu-id="f0ae4-225">Methods without parameters</span></span>

<span data-ttu-id="f0ae4-226">Si la méthode que vous traitez n’a pas de paramètres, utilisez la surcharge non générique de la `On` méthode :</span><span class="sxs-lookup"><span data-stu-id="f0ae4-226">If the method you're handling does not have parameters, use the non-generic overload of the `On` method:</span></span>

<span data-ttu-id="f0ae4-227">**Code de serveur client de méthode sans paramètres**</span><span class="sxs-lookup"><span data-stu-id="f0ae4-227">**Server code calling client method without parameters**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample14.cs?highlight=5)]

<span data-ttu-id="f0ae4-228">**Code de WinRT Client pour la méthode est appelée depuis le serveur sans paramètres ([voir des exemples WPF et Silverlight plus loin dans cette rubrique](#wpfsl))**</span><span class="sxs-lookup"><span data-stu-id="f0ae4-228">**WinRT Client code for method called from server without parameters ([see WPF and Silverlight examples later in this topic](#wpfsl))**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample15.cs)]

<a id="clientmethodswithparmtypes"></a>

### <a name="methods-with-parameters-specifying-the-parameter-types"></a><span data-ttu-id="f0ae4-229">Méthodes avec des paramètres, en spécifiant les types de paramètres</span><span class="sxs-lookup"><span data-stu-id="f0ae4-229">Methods with parameters, specifying the parameter types</span></span>

<span data-ttu-id="f0ae4-230">Si la méthode que vous traitez possède des paramètres, spécifiez les types des paramètres en tant que les types génériques de la `On` (méthode).</span><span class="sxs-lookup"><span data-stu-id="f0ae4-230">If the method you're handling has parameters, specify the types of the parameters as the generic types of the `On` method.</span></span> <span data-ttu-id="f0ae4-231">Il existe des surcharges génériques de la `On` méthode pour vous permettre de spécifier des paramètres jusqu'à 8 (4 sur Windows Phone 7).</span><span class="sxs-lookup"><span data-stu-id="f0ae4-231">There are generic overloads of the `On` method to enable you to specify up to 8 parameters (4 on Windows Phone 7).</span></span> <span data-ttu-id="f0ae4-232">Dans l’exemple suivant, un paramètre est envoyé à la `UpdateStockPrice` (méthode).</span><span class="sxs-lookup"><span data-stu-id="f0ae4-232">In the following example, one parameter is sent to the `UpdateStockPrice` method.</span></span>

<span data-ttu-id="f0ae4-233">**Appel de méthode du client avec un paramètre de code de serveur**</span><span class="sxs-lookup"><span data-stu-id="f0ae4-233">**Server code calling client method with a parameter**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample16.cs?highlight=3)]

<span data-ttu-id="f0ae4-234">**La classe de Stock utilisée pour le paramètre**</span><span class="sxs-lookup"><span data-stu-id="f0ae4-234">**The Stock class used for the parameter**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample17.cs)]

<span data-ttu-id="f0ae4-235">**Code de WinRT Client pour une méthode appelée depuis le serveur avec un paramètre ([voir des exemples WPF et Silverlight plus loin dans cette rubrique](#wpfsl))**</span><span class="sxs-lookup"><span data-stu-id="f0ae4-235">**WinRT Client code for a method called from server with a parameter ([see WPF and Silverlight examples later in this topic](#wpfsl))**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample18.cs?highlight=1,5)]

<a id="clientmethodswithdynamparms"></a>

### <a name="methods-with-parameters-specifying-dynamic-objects-for-the-parameters"></a><span data-ttu-id="f0ae4-236">Méthodes avec des paramètres, en spécifiant des objets dynamiques pour les paramètres</span><span class="sxs-lookup"><span data-stu-id="f0ae4-236">Methods with parameters, specifying dynamic objects for the parameters</span></span>

<span data-ttu-id="f0ae4-237">En guise d’alternative à la spécification des paramètres comme des types génériques de la `On` (méthode), vous pouvez spécifier des paramètres en tant qu’objets dynamiques :</span><span class="sxs-lookup"><span data-stu-id="f0ae4-237">As an alternative to specifying parameters as generic types of the `On` method, you can specify parameters as dynamic objects:</span></span>

<span data-ttu-id="f0ae4-238">**Appel de méthode du client avec un paramètre de code de serveur**</span><span class="sxs-lookup"><span data-stu-id="f0ae4-238">**Server code calling client method with a parameter**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample19.cs?highlight=3)]

<span data-ttu-id="f0ae4-239">**La classe de Stock utilisée pour le paramètre**</span><span class="sxs-lookup"><span data-stu-id="f0ae4-239">**The Stock class used for the parameter**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample20.cs)]

<span data-ttu-id="f0ae4-240">**Code de WinRT Client pour une méthode appelée depuis le serveur avec un paramètre, à l’aide d’un objet dynamique pour le paramètre ([voir des exemples WPF et Silverlight plus loin dans cette rubrique](#wpfsl))**</span><span class="sxs-lookup"><span data-stu-id="f0ae4-240">**WinRT Client code for a method called from server with a parameter, using a dynamic object for the parameter ([see WPF and Silverlight examples later in this topic](#wpfsl))**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample21.cs?highlight=1,5)]

<a id="removehandler"></a>

### <a name="how-to-remove-a-handler"></a><span data-ttu-id="f0ae4-241">Comment supprimer un gestionnaire</span><span class="sxs-lookup"><span data-stu-id="f0ae4-241">How to remove a handler</span></span>

<span data-ttu-id="f0ae4-242">Pour supprimer un gestionnaire, appelez sa `Dispose` (méthode).</span><span class="sxs-lookup"><span data-stu-id="f0ae4-242">To remove a handler, call its `Dispose` method.</span></span>

<span data-ttu-id="f0ae4-243">**Code client pour une méthode appelée à partir du serveur**</span><span class="sxs-lookup"><span data-stu-id="f0ae4-243">**Client code for a method called from server**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample22.cs?highlight=1)]

<span data-ttu-id="f0ae4-244">**Code client pour supprimer le Gestionnaire**</span><span class="sxs-lookup"><span data-stu-id="f0ae4-244">**Client code to remove the handler**</span></span>

[!code-css[Main](hubs-api-guide-net-client/samples/sample23.css?highlight=1)]

<a id="callserver"></a>

## <a name="how-to-call-server-methods-from-the-client"></a><span data-ttu-id="f0ae4-245">Comment appeler des méthodes de serveur à partir du client</span><span class="sxs-lookup"><span data-stu-id="f0ae4-245">How to call server methods from the client</span></span>

<span data-ttu-id="f0ae4-246">Pour appeler une méthode sur le serveur, utilisez la `Invoke` méthode sur le concentrateur proxy.</span><span class="sxs-lookup"><span data-stu-id="f0ae4-246">To call a method on the server, use the `Invoke` method on the Hub proxy.</span></span>

<span data-ttu-id="f0ae4-247">Si la méthode serveur n’a aucune valeur de retour, utilisez la surcharge non générique de la `Invoke` (méthode).</span><span class="sxs-lookup"><span data-stu-id="f0ae4-247">If the server method has no return value, use the non-generic overload of the `Invoke` method.</span></span>

<span data-ttu-id="f0ae4-248">**Code de serveur pour une méthode qui n’a aucune valeur de retour**</span><span class="sxs-lookup"><span data-stu-id="f0ae4-248">**Server code for a method that has no return value**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample24.cs?highlight=3)]

<span data-ttu-id="f0ae4-249">**Code client appelant une méthode qui n’a aucune valeur de retour**</span><span class="sxs-lookup"><span data-stu-id="f0ae4-249">**Client code calling a method that has no return value**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample25.cs?highlight=1)]

<span data-ttu-id="f0ae4-250">Si la méthode serveur a une valeur de retour, spécifiez le type de retour que le type générique de la `Invoke` (méthode).</span><span class="sxs-lookup"><span data-stu-id="f0ae4-250">If the server method has a return value, specify the return type as the generic type of the `Invoke` method.</span></span>

<span data-ttu-id="f0ae4-251">**Code de serveur pour une méthode qui a une valeur de retour et prend un paramètre de type complexe**</span><span class="sxs-lookup"><span data-stu-id="f0ae4-251">**Server code for a method that has a return value and takes a complex type parameter**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample26.cs?highlight=1)]

<span data-ttu-id="f0ae4-252">**La classe de Stock utilisée pour le paramètre et la valeur de retour**</span><span class="sxs-lookup"><span data-stu-id="f0ae4-252">**The Stock class used for the parameter and return value**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample27.cs)]

<span data-ttu-id="f0ae4-253">**Code client appelant une méthode qui a une valeur de retour et prend un paramètre de type complexe dans une méthode async de ASP.NET 4.5**</span><span class="sxs-lookup"><span data-stu-id="f0ae4-253">**Client code calling a method that has a return value and takes a complex type parameter, in an ASP.NET 4.5 async method**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample28.cs?highlight=1-2)]

<span data-ttu-id="f0ae4-254">**Code client appelant une méthode qui a une valeur de retour et prend un paramètre de type complexe dans une méthode synchrone**</span><span class="sxs-lookup"><span data-stu-id="f0ae4-254">**Client code calling a method that has a return value and takes a complex type parameter, in a synchronous method**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample29.cs?highlight=1-2)]

<span data-ttu-id="f0ae4-255">Le `Invoke` méthode exécute de façon asynchrone et retourne un `Task` objet.</span><span class="sxs-lookup"><span data-stu-id="f0ae4-255">The `Invoke` method executes asynchronously and returns a `Task` object.</span></span> <span data-ttu-id="f0ae4-256">Si vous ne spécifiez pas `await` ou `.Wait()`, la ligne suivante de code sera exécuté avant que la méthode que vous appelez a terminé l’exécution.</span><span class="sxs-lookup"><span data-stu-id="f0ae4-256">If you don't specify `await` or `.Wait()`, the next line of code will execute before the method that you invoke has finished executing.</span></span>

<a id="connectionlifetime"></a>

## <a name="how-to-handle-connection-lifetime-events"></a><span data-ttu-id="f0ae4-257">Comment gérer les événements de durée de vie de connexion</span><span class="sxs-lookup"><span data-stu-id="f0ae4-257">How to handle connection lifetime events</span></span>

<span data-ttu-id="f0ae4-258">SignalR fournit des événements de durée de vie que vous pouvez gérer la connexion suivante :</span><span class="sxs-lookup"><span data-stu-id="f0ae4-258">SignalR provides the following connection lifetime events that you can handle:</span></span>

- <span data-ttu-id="f0ae4-259">`Received`: Déclenché lorsque des données sont reçues sur la connexion.</span><span class="sxs-lookup"><span data-stu-id="f0ae4-259">`Received`: Raised when any data is received on the connection.</span></span> <span data-ttu-id="f0ae4-260">Fournit les données reçues.</span><span class="sxs-lookup"><span data-stu-id="f0ae4-260">Provides the received data.</span></span>
- <span data-ttu-id="f0ae4-261">`ConnectionSlow`: Déclenché lorsque le client détecte une connexion lente ou suppression fréquemment.</span><span class="sxs-lookup"><span data-stu-id="f0ae4-261">`ConnectionSlow`: Raised when the client detects a slow or frequently dropping connection.</span></span>
- <span data-ttu-id="f0ae4-262">`Reconnecting`: Déclenché lorsque le transport sous-jacent commence la reconnexion.</span><span class="sxs-lookup"><span data-stu-id="f0ae4-262">`Reconnecting`: Raised when the underlying transport begins reconnecting.</span></span>
- <span data-ttu-id="f0ae4-263">`Reconnected`: Déclenché lorsque le transport sous-jacent s’est reconnecté.</span><span class="sxs-lookup"><span data-stu-id="f0ae4-263">`Reconnected`: Raised when the underlying transport has reconnected.</span></span>
- <span data-ttu-id="f0ae4-264">`StateChanged`: Déclenché lorsque l’état de la connexion change.</span><span class="sxs-lookup"><span data-stu-id="f0ae4-264">`StateChanged`: Raised when the connection state changes.</span></span> <span data-ttu-id="f0ae4-265">Fournit l’ancien état et le nouvel état.</span><span class="sxs-lookup"><span data-stu-id="f0ae4-265">Provides the old state and the new state.</span></span> <span data-ttu-id="f0ae4-266">Pour plus d’informations sur la connexion valeurs d’état, consultez [énumération ConnectionState](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.client.connectionstate(v=vs.111).aspx).</span><span class="sxs-lookup"><span data-stu-id="f0ae4-266">For information about connection state values see [ConnectionState Enumeration](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.client.connectionstate(v=vs.111).aspx).</span></span>
- <span data-ttu-id="f0ae4-267">`Closed`: Déclenché lors de la connexion s’est déconnecté.</span><span class="sxs-lookup"><span data-stu-id="f0ae4-267">`Closed`: Raised when the connection has disconnected.</span></span>

<span data-ttu-id="f0ae4-268">Par exemple, si vous souhaitez afficher les messages d’avertissement pour les erreurs qui ne sont pas irrécupérable mais entraînent des problèmes de connexion intermittents, par exemple que la lenteur ou fréquentes la suppression de la connexion, gèrent les `ConnectionSlow` événement.</span><span class="sxs-lookup"><span data-stu-id="f0ae4-268">For example, if you want to display warning messages for errors that are not fatal but cause intermittent connection problems, such as slowness or frequent dropping of the connection, handle the `ConnectionSlow` event.</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample30.cs)]

<span data-ttu-id="f0ae4-269">Pour plus d’informations, consultez [compréhension et gestion des événements de durée de vie de connexion dans SignalR](handling-connection-lifetime-events.md).</span><span class="sxs-lookup"><span data-stu-id="f0ae4-269">For more information, see [Understanding and Handling Connection Lifetime Events in SignalR](handling-connection-lifetime-events.md).</span></span>

<a id="handleerrors"></a>

## <a name="how-to-handle-errors"></a><span data-ttu-id="f0ae4-270">Comment gérer les erreurs</span><span class="sxs-lookup"><span data-stu-id="f0ae4-270">How to handle errors</span></span>

<span data-ttu-id="f0ae4-271">Si vous n’activez pas explicitement des messages d’erreur détaillés sur le serveur, l’objet exception qui SignalR renvoie une erreur et contient un minimum d’informations sur l’erreur.</span><span class="sxs-lookup"><span data-stu-id="f0ae4-271">If you don't explicitly enable detailed error messages on the server, the exception object that SignalR returns after an error contains minimal information about the error.</span></span> <span data-ttu-id="f0ae4-272">Par exemple, si un appel à `newContosoChatMessage` échoue, le message d’erreur dans l’objet d’erreur contient «`There was an error invoking Hub method 'contosoChatHub.newContosoChatMessage'.`« envoi de messages d’erreur détaillés pour les clients en production n’est pas recommandé pour des raisons de sécurité, mais si vous souhaitez activer les messages d’erreur détaillés pour à des fins de résolution des problèmes, utilisez le code suivant sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="f0ae4-272">For example, if a call to `newContosoChatMessage` fails, the error message in the error object contains "`There was an error invoking Hub method 'contosoChatHub.newContosoChatMessage'.`" Sending detailed error messages to clients in production is not recommended for security reasons, but if you want to enable detailed error messages for troubleshooting purposes, use the following code on the server.</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample31.cs?highlight=2)]

<a id="handleerrors"></a>

<span data-ttu-id="f0ae4-273">Pour gérer les erreurs SignalR déclenche, vous pouvez ajouter un gestionnaire pour le `Error` événement sur l’objet de connexion.</span><span class="sxs-lookup"><span data-stu-id="f0ae4-273">To handle errors that SignalR raises, you can add a handler for the `Error` event on the connection object.</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample32.cs)]

<span data-ttu-id="f0ae4-274">Pour gérer les erreurs d’appels de méthode, encapsulez le code dans un bloc try-catch.</span><span class="sxs-lookup"><span data-stu-id="f0ae4-274">To handle errors from method invocations, wrap the code in a try-catch block.</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample33.cs)]

<a id="logging"></a>

## <a name="how-to-enable-client-side-logging"></a><span data-ttu-id="f0ae4-275">Comment activer la journalisation côté client</span><span class="sxs-lookup"><span data-stu-id="f0ae4-275">How to enable client-side logging</span></span>

<span data-ttu-id="f0ae4-276">Pour activer la journalisation côté client, définissez la `TraceLevel` et `TraceWriter` propriétés de l’objet de connexion.</span><span class="sxs-lookup"><span data-stu-id="f0ae4-276">To enable client-side logging, set the `TraceLevel` and `TraceWriter` properties on the connection object.</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample34.cs?highlight=2-3)]

<a id="wpfsl"></a>

## <a name="wpf-silverlight-and-console-application-code-samples-for-client-methods-that-the-server-can-call"></a><span data-ttu-id="f0ae4-277">Exemples de code d’application console pour les méthodes de client que le serveur peut appeler, WPF et Silverlight</span><span class="sxs-lookup"><span data-stu-id="f0ae4-277">WPF, Silverlight, and console application code samples for client methods that the server can call</span></span>

<span data-ttu-id="f0ae4-278">Les exemples de code ci-dessus pour définir des méthodes de client que le serveur peut appeler s’appliquent aux clients de WinRT.</span><span class="sxs-lookup"><span data-stu-id="f0ae4-278">The code samples shown earlier for defining client methods that the server can call apply to WinRT clients.</span></span> <span data-ttu-id="f0ae4-279">Les exemples suivants montrent le code équivalent pour les clients d’application console, WPF et Silverlight.</span><span class="sxs-lookup"><span data-stu-id="f0ae4-279">The following samples show the equivalent code for WPF, Silverlight, and console application clients.</span></span>

### <a name="methods-without-parameters"></a><span data-ttu-id="f0ae4-280">Méthodes sans paramètres</span><span class="sxs-lookup"><span data-stu-id="f0ae4-280">Methods without parameters</span></span>

<span data-ttu-id="f0ae4-281">**Code de client WPF pour la méthode appelée à partir du serveur sans paramètres**</span><span class="sxs-lookup"><span data-stu-id="f0ae4-281">**WPF client code for method called from server without parameters**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample35.cs?highlight=1)]

<span data-ttu-id="f0ae4-282">**Code de client Silverlight pour la méthode est appelée depuis le serveur sans paramètres**</span><span class="sxs-lookup"><span data-stu-id="f0ae4-282">**Silverlight client code for method called from server without parameters**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample36.cs?highlight=1)]

<span data-ttu-id="f0ae4-283">**Code du client d’application console pour la méthode est appelée depuis le serveur sans paramètres**</span><span class="sxs-lookup"><span data-stu-id="f0ae4-283">**Console application client code for method called from server without parameters**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample37.cs?highlight=1)]

### <a name="methods-with-parameters-specifying-the-parameter-types"></a><span data-ttu-id="f0ae4-284">Méthodes avec des paramètres, en spécifiant les types de paramètres</span><span class="sxs-lookup"><span data-stu-id="f0ae4-284">Methods with parameters, specifying the parameter types</span></span>

<span data-ttu-id="f0ae4-285">**Code de client WPF pour une méthode appelée depuis le serveur avec un paramètre**</span><span class="sxs-lookup"><span data-stu-id="f0ae4-285">**WPF client code for a method called from server with a parameter**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample38.cs?highlight=1,4)]

<span data-ttu-id="f0ae4-286">**Code de client Silverlight pour une méthode appelée depuis le serveur avec un paramètre**</span><span class="sxs-lookup"><span data-stu-id="f0ae4-286">**Silverlight client code for a method called from server with a parameter**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample39.cs?highlight=1,5)]

<span data-ttu-id="f0ae4-287">**Code du client d’application console pour une méthode appelée depuis le serveur avec un paramètre**</span><span class="sxs-lookup"><span data-stu-id="f0ae4-287">**Console application client code for a method called from server with a parameter**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample40.cs?highlight=1-2)]

### <a name="methods-with-parameters-specifying-dynamic-objects-for-the-parameters"></a><span data-ttu-id="f0ae4-288">Méthodes avec des paramètres, en spécifiant des objets dynamiques pour les paramètres</span><span class="sxs-lookup"><span data-stu-id="f0ae4-288">Methods with parameters, specifying dynamic objects for the parameters</span></span>

<span data-ttu-id="f0ae4-289">**Code de client WPF pour une méthode appelée depuis le serveur avec un paramètre, à l’aide d’un objet dynamique pour le paramètre**</span><span class="sxs-lookup"><span data-stu-id="f0ae4-289">**WPF client code for a method called from server with a parameter, using a dynamic object for the parameter**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample41.cs?highlight=1,4)]

<span data-ttu-id="f0ae4-290">**Code de client Silverlight pour une méthode appelée depuis le serveur avec un paramètre, à l’aide d’un objet dynamique pour le paramètre**</span><span class="sxs-lookup"><span data-stu-id="f0ae4-290">**Silverlight client code for a method called from server with a parameter, using a dynamic object for the parameter**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample42.cs?highlight=1,5)]

<span data-ttu-id="f0ae4-291">**Code du client d’application console pour une méthode appelée depuis le serveur avec un paramètre, à l’aide d’un objet dynamique pour le paramètre**</span><span class="sxs-lookup"><span data-stu-id="f0ae4-291">**Console application client code for a method called from server with a parameter, using a dynamic object for the parameter**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample43.cs?highlight=1-2)]
