---
uid: signalr/overview/guide-to-the-api/hubs-api-guide-net-client
title: Guide de l’API ASP.NET SignalR Hubs - Client .NET (c#) | Microsoft Docs
author: pfletcher
description: Ce document fournit une introduction à l’utilisation de l’API Hubs pour la version 2 dans les clients .NET, tels que Windows Store (WinRT), WPF, Silverlight et les inconvénients de SignalR...
ms.author: riande
ms.date: 06/10/2014
ms.assetid: 6d02d9f7-94e5-4140-9f51-5a6040f274f6
msc.legacyurl: /signalr/overview/guide-to-the-api/hubs-api-guide-net-client
msc.type: authoredcontent
ms.openlocfilehash: 0a2b24039259ef90579a7f215bb9e35ebef7b9b9
ms.sourcegitcommit: 74e3be25ea37b5fc8b4b433b0b872547b4b99186
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2018
ms.locfileid: "53288038"
---
<a name="aspnet-signalr-hubs-api-guide---net-client-c"></a><span data-ttu-id="2268f-103">Guide de l’API ASP.NET SignalR Hubs - Client .NET (c#)</span><span class="sxs-lookup"><span data-stu-id="2268f-103">ASP.NET SignalR Hubs API Guide - .NET Client (C#)</span></span>
====================
<span data-ttu-id="2268f-104">par [Patrick Fletcher](https://github.com/pfletcher), [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="2268f-104">by [Patrick Fletcher](https://github.com/pfletcher), [Tom Dykstra](https://github.com/tdykstra)</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> <span data-ttu-id="2268f-105">Ce document fournit une introduction à l’utilisation de l’API Hubs pour SignalR version 2 dans les clients .NET, tels que Windows Store (WinRT), WPF, Silverlight et les applications de console.</span><span class="sxs-lookup"><span data-stu-id="2268f-105">This document provides an introduction to using the Hubs API for SignalR version 2 in .NET clients, such as Windows Store (WinRT), WPF, Silverlight, and console applications.</span></span>
>
> <span data-ttu-id="2268f-106">L’API de concentrateurs SignalR vous permet de vous permettent d’effectuer des appels de procédure distante (RPC) à partir d’un serveur aux clients connectés et à partir de clients sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="2268f-106">The SignalR Hubs API enables you to make remote procedure calls (RPCs) from a server to connected clients and from clients to the server.</span></span> <span data-ttu-id="2268f-107">Dans le code serveur, vous définissez des méthodes qui peuvent être appelées par les clients, et vous appelez des méthodes qui s’exécutent sur le client.</span><span class="sxs-lookup"><span data-stu-id="2268f-107">In server code, you define methods that can be called by clients, and you call methods that run on the client.</span></span> <span data-ttu-id="2268f-108">Dans le code client, vous définissez des méthodes qui peuvent être appelées à partir du serveur, et vous appelez des méthodes qui s’exécutent sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="2268f-108">In client code, you define methods that can be called from the server, and you call methods that run on the server.</span></span> <span data-ttu-id="2268f-109">SignalR s’occupe de tous les éléments client-serveur pour vous.</span><span class="sxs-lookup"><span data-stu-id="2268f-109">SignalR takes care of all of the client-to-server plumbing for you.</span></span>
>
> <span data-ttu-id="2268f-110">SignalR offre également une API de niveau inférieur appelée connexions persistantes.</span><span class="sxs-lookup"><span data-stu-id="2268f-110">SignalR also offers a lower-level API called Persistent Connections.</span></span> <span data-ttu-id="2268f-111">Pour une introduction à SignalR Hubs et connexions persistantes, ou pour obtenir un didacticiel qui montre comment générer une application de SignalR complète, consultez [SignalR - mise en route](../getting-started/index.md).</span><span class="sxs-lookup"><span data-stu-id="2268f-111">For an introduction to SignalR, Hubs, and Persistent Connections, or for a tutorial that shows how to build a complete SignalR application, see [SignalR - Getting Started](../getting-started/index.md).</span></span>
>
> ## <a name="software-versions-used-in-this-topic"></a><span data-ttu-id="2268f-112">Versions des logiciels utilisées dans cette rubrique</span><span class="sxs-lookup"><span data-stu-id="2268f-112">Software versions used in this topic</span></span>
>
>
> - [<span data-ttu-id="2268f-113">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="2268f-113">Visual Studio 2013</span></span>](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)
> - <span data-ttu-id="2268f-114">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="2268f-114">.NET 4.5</span></span>
> - <span data-ttu-id="2268f-115">SignalR version 2</span><span class="sxs-lookup"><span data-stu-id="2268f-115">SignalR version 2</span></span>
>
>
>
> ## <a name="previous-versions-of-this-topic"></a><span data-ttu-id="2268f-116">Versions précédentes de cette rubrique</span><span class="sxs-lookup"><span data-stu-id="2268f-116">Previous versions of this topic</span></span>
>
> <span data-ttu-id="2268f-117">Pour plus d’informations sur les versions antérieures de SignalR, consultez [les Versions antérieures de SignalR](../older-versions/index.md).</span><span class="sxs-lookup"><span data-stu-id="2268f-117">For information about earlier versions of SignalR, see [SignalR Older Versions](../older-versions/index.md).</span></span>
>
> ## <a name="questions-and-comments"></a><span data-ttu-id="2268f-118">Questions et commentaires</span><span class="sxs-lookup"><span data-stu-id="2268f-118">Questions and comments</span></span>
>
> <span data-ttu-id="2268f-119">Veuillez laisser des commentaires sur la façon dont vous avez apprécié ce didacticiel et ce que nous pouvions améliorer dans les commentaires en bas de la page.</span><span class="sxs-lookup"><span data-stu-id="2268f-119">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="2268f-120">Si vous avez des questions qui ne sont pas directement liées à ce didacticiel, vous pouvez les publier à le [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) ou [StackOverflow.com](http://stackoverflow.com/).</span><span class="sxs-lookup"><span data-stu-id="2268f-120">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>

## <a name="overview"></a><span data-ttu-id="2268f-121">Vue d'ensemble</span><span class="sxs-lookup"><span data-stu-id="2268f-121">Overview</span></span>

<span data-ttu-id="2268f-122">Ce document contient les sections suivantes :</span><span class="sxs-lookup"><span data-stu-id="2268f-122">This document contains the following sections:</span></span>

- [<span data-ttu-id="2268f-123">Programme d’installation du client</span><span class="sxs-lookup"><span data-stu-id="2268f-123">Client Setup</span></span>](#clientsetup)
- [<span data-ttu-id="2268f-124">Comment établir une connexion</span><span class="sxs-lookup"><span data-stu-id="2268f-124">How to establish a connection</span></span>](#establishconnection)

    - [<span data-ttu-id="2268f-125">Connexions inter-domaines à partir de clients Silverlight</span><span class="sxs-lookup"><span data-stu-id="2268f-125">Cross-domain connections from Silverlight clients</span></span>](#slcrossdomain)
- [<span data-ttu-id="2268f-126">Comment configurer la connexion</span><span class="sxs-lookup"><span data-stu-id="2268f-126">How to configure the connection</span></span>](#configureconnection)

    - [<span data-ttu-id="2268f-127">Comment définir le nombre maximal de connexions simultanées dans les clients WPF</span><span class="sxs-lookup"><span data-stu-id="2268f-127">How to set the maximum number of concurrent connections in WPF clients</span></span>](#maxconnections)
    - [<span data-ttu-id="2268f-128">Comment spécifier des paramètres de chaîne de requête</span><span class="sxs-lookup"><span data-stu-id="2268f-128">How to specify query string parameters</span></span>](#querystring)
    - [<span data-ttu-id="2268f-129">Comment spécifier le mode de transport</span><span class="sxs-lookup"><span data-stu-id="2268f-129">How to specify the transport method</span></span>](#transport)
    - [<span data-ttu-id="2268f-130">Comment spécifier des en-têtes HTTP</span><span class="sxs-lookup"><span data-stu-id="2268f-130">How to specify HTTP headers</span></span>](#httpheaders)
    - [<span data-ttu-id="2268f-131">Comment spécifier des certificats clients</span><span class="sxs-lookup"><span data-stu-id="2268f-131">How to specify client certificates</span></span>](#clientcertificate)
- [<span data-ttu-id="2268f-132">Comment créer le concentrateur proxy</span><span class="sxs-lookup"><span data-stu-id="2268f-132">How to create the Hub proxy</span></span>](#proxy)
- [<span data-ttu-id="2268f-133">Comment définir des méthodes sur le client que le serveur peut appeler.</span><span class="sxs-lookup"><span data-stu-id="2268f-133">How to define methods on the client that the server can call</span></span>](#callclient)

    - [<span data-ttu-id="2268f-134">Méthodes sans paramètres</span><span class="sxs-lookup"><span data-stu-id="2268f-134">Methods without parameters</span></span>](#clientmethodswithoutparms)
    - [<span data-ttu-id="2268f-135">Méthodes avec des paramètres, en spécifiant les types de paramètres</span><span class="sxs-lookup"><span data-stu-id="2268f-135">Methods with parameters, specifying parameter types</span></span>](#clientmethodswithparmtypes)
    - [<span data-ttu-id="2268f-136">Méthodes avec des paramètres, en spécifiant des objets dynamiques pour les paramètres</span><span class="sxs-lookup"><span data-stu-id="2268f-136">Methods with parameters, specifying dynamic objects for the parameters</span></span>](#clientmethodswithdynamparms)
    - [<span data-ttu-id="2268f-137">Comment supprimer un gestionnaire</span><span class="sxs-lookup"><span data-stu-id="2268f-137">How to remove a handler</span></span>](#removehandler)
- [<span data-ttu-id="2268f-138">Comment appeler des méthodes de serveur à partir du client</span><span class="sxs-lookup"><span data-stu-id="2268f-138">How to call server methods from the client</span></span>](#callserver)
- [<span data-ttu-id="2268f-139">Comment gérer les événements de durée de vie de connexion</span><span class="sxs-lookup"><span data-stu-id="2268f-139">How to handle connection lifetime events</span></span>](#connectionlifetime)
- [<span data-ttu-id="2268f-140">Comment gérer les erreurs</span><span class="sxs-lookup"><span data-stu-id="2268f-140">How to handle errors</span></span>](#handleerrors)
- [<span data-ttu-id="2268f-141">Comment activer la journalisation côté client</span><span class="sxs-lookup"><span data-stu-id="2268f-141">How to enable client-side logging</span></span>](#logging)
- [<span data-ttu-id="2268f-142">WPF, Silverlight et exemples de code d’application console pour les méthodes de client que le serveur peut appeler.</span><span class="sxs-lookup"><span data-stu-id="2268f-142">WPF, Silverlight, and console application code samples for client methods that the server can call</span></span>](#wpfsl)

<span data-ttu-id="2268f-143">Pour un exemples de projets de client .NET, consultez les ressources suivantes :</span><span class="sxs-lookup"><span data-stu-id="2268f-143">For a sample .NET client projects, see the following resources:</span></span>

- <span data-ttu-id="2268f-144">[gustavo-armenta / SignalR-Samples](https://github.com/gustavo-armenta/SignalR-Samples) sur GitHub.com (WinRT, Silverlight, les exemples d’application console).</span><span class="sxs-lookup"><span data-stu-id="2268f-144">[gustavo-armenta / SignalR-Samples](https://github.com/gustavo-armenta/SignalR-Samples) on GitHub.com (WinRT, Silverlight, console app examples).</span></span>
- <span data-ttu-id="2268f-145">[DamianEdwards / SignalR-MoveShapeDemo / MoveShape.Desktop](https://github.com/DamianEdwards/SignalR-MoveShapeDemo/tree/master/MoveShape/MoveShape.Desktop) sur GitHub.com (par exemple, WPF).</span><span class="sxs-lookup"><span data-stu-id="2268f-145">[DamianEdwards / SignalR-MoveShapeDemo / MoveShape.Desktop](https://github.com/DamianEdwards/SignalR-MoveShapeDemo/tree/master/MoveShape/MoveShape.Desktop) on GitHub.com (WPF example).</span></span>
- <span data-ttu-id="2268f-146">[SignalR / Microsoft.AspNet.SignalR.Client.Samples](https://github.com/SignalR/SignalR/tree/master/samples/Microsoft.AspNet.SignalR.Client.Samples) sur GitHub.com (exemple d’application Console).</span><span class="sxs-lookup"><span data-stu-id="2268f-146">[SignalR / Microsoft.AspNet.SignalR.Client.Samples](https://github.com/SignalR/SignalR/tree/master/samples/Microsoft.AspNet.SignalR.Client.Samples) on GitHub.com (Console app example).</span></span>

<span data-ttu-id="2268f-147">Pour obtenir une documentation sur la façon de programmer le serveur ou les clients JavaScript, consultez les ressources suivantes :</span><span class="sxs-lookup"><span data-stu-id="2268f-147">For documentation on how to program the server or JavaScript clients, see the following resources:</span></span>

- [<span data-ttu-id="2268f-148">Guide de l’API SignalR Hubs - serveur</span><span class="sxs-lookup"><span data-stu-id="2268f-148">SignalR Hubs API Guide - Server</span></span>](hubs-api-guide-server.md)
- [<span data-ttu-id="2268f-149">Guide de l’API SignalR Hubs - Client JavaScript</span><span class="sxs-lookup"><span data-stu-id="2268f-149">SignalR Hubs API Guide - JavaScript Client</span></span>](hubs-api-guide-javascript-client.md)

<span data-ttu-id="2268f-150">Liens vers des rubriques de référence de l’API sont à la version de .NET 4.5 de l’API.</span><span class="sxs-lookup"><span data-stu-id="2268f-150">Links to API Reference topics are to the .NET 4.5 version of the API.</span></span> <span data-ttu-id="2268f-151">Si vous utilisez .NET 4, consultez [la version de .NET 4 des rubriques API](https://msdn.microsoft.com/library/jj891075(v=vs.100).aspx).</span><span class="sxs-lookup"><span data-stu-id="2268f-151">If you're using .NET 4, see [the .NET 4 version of the API topics](https://msdn.microsoft.com/library/jj891075(v=vs.100).aspx).</span></span>

<a id="clientsetup"></a>

## <a name="client-setup"></a><span data-ttu-id="2268f-152">Programme d’installation du client</span><span class="sxs-lookup"><span data-stu-id="2268f-152">Client setup</span></span>

<span data-ttu-id="2268f-153">Installer le [Microsoft.AspNet.SignalR.Client](http://nuget.org/packages/Microsoft.AspNet.SignalR.Client) package NuGet (pas le [Microsoft.AspNet.SignalR](http://nuget.org/packages/microsoft.aspnet.signalr) package).</span><span class="sxs-lookup"><span data-stu-id="2268f-153">Install the [Microsoft.AspNet.SignalR.Client](http://nuget.org/packages/Microsoft.AspNet.SignalR.Client) NuGet package (not the [Microsoft.AspNet.SignalR](http://nuget.org/packages/microsoft.aspnet.signalr) package).</span></span> <span data-ttu-id="2268f-154">Ce package prend en charge WinRT, Silverlight, WPF, application console et clients Windows Phone, .NET 4 et .NET 4.5.</span><span class="sxs-lookup"><span data-stu-id="2268f-154">This package supports WinRT, Silverlight, WPF, console application, and Windows Phone clients, for both .NET 4 and .NET 4.5.</span></span>

<span data-ttu-id="2268f-155">Si la version de SignalR que vous disposez sur le client est différente de la version que vous disposez sur le serveur, SignalR est souvent capable de s’adapter à la différence.</span><span class="sxs-lookup"><span data-stu-id="2268f-155">If the version of SignalR that you have on the client is different from the version that you have on the server, SignalR is often able to adapt to the difference.</span></span> <span data-ttu-id="2268f-156">Par exemple, un serveur exécutant la version 2 de SignalR prendra en charge les clients qui ont installé des 1.1.x, ainsi que les clients dont la version 2 est installé.</span><span class="sxs-lookup"><span data-stu-id="2268f-156">For example, a server running SignalR version 2 will support clients that have 1.1.x installed as well as clients that have version 2 installed.</span></span> <span data-ttu-id="2268f-157">Si la différence entre la version sur le serveur et la version sur le client est trop grande, ou si le client est plus récent que le serveur, SignalR lève une `InvalidOperationException` exception lorsque le client tente d’établir une connexion.</span><span class="sxs-lookup"><span data-stu-id="2268f-157">If the difference between the version on the server and the version on the client is too great, or if the client is newer than the server, SignalR throws an `InvalidOperationException` exception when the client tries to establish a connection.</span></span> <span data-ttu-id="2268f-158">Le message d’erreur est «`You are using a version of the client that isn't compatible with the server. Client version X.X, server version X.X`».</span><span class="sxs-lookup"><span data-stu-id="2268f-158">The error message is "`You are using a version of the client that isn't compatible with the server. Client version X.X, server version X.X`".</span></span>

<a id="establishconnection"></a>

## <a name="how-to-establish-a-connection"></a><span data-ttu-id="2268f-159">Comment établir une connexion</span><span class="sxs-lookup"><span data-stu-id="2268f-159">How to establish a connection</span></span>

<span data-ttu-id="2268f-160">Avant de pouvoir établir une connexion, vous devez créer un `HubConnection` de l’objet et de créer un proxy.</span><span class="sxs-lookup"><span data-stu-id="2268f-160">Before you can establish a connection, you have to create a `HubConnection` object and create a proxy.</span></span> <span data-ttu-id="2268f-161">Pour établir la connexion, appelez le `Start` méthode sur le `HubConnection` objet.</span><span class="sxs-lookup"><span data-stu-id="2268f-161">To establish the connection, call the `Start` method on the `HubConnection` object.</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample1.cs?highlight=1,4)]

> [!NOTE]
> <span data-ttu-id="2268f-162">Pour les clients JavaScript, vous devez inscrire au moins un gestionnaire d’événements avant d’appeler le `Start` méthode pour établir la connexion.</span><span class="sxs-lookup"><span data-stu-id="2268f-162">For JavaScript clients you have to register at least one event handler before calling the `Start` method to establish the connection.</span></span> <span data-ttu-id="2268f-163">Cela n’est pas nécessaire pour les clients .NET.</span><span class="sxs-lookup"><span data-stu-id="2268f-163">This is not necessary for .NET clients.</span></span> <span data-ttu-id="2268f-164">Pour les clients JavaScript, le code proxy généré crée automatiquement des proxys pour tous les concentrateurs qui existent sur le serveur et l’inscription d’un gestionnaire sert à indiquer les Hubs votre client prévoit d’utiliser.</span><span class="sxs-lookup"><span data-stu-id="2268f-164">For JavaScript clients, the generated proxy code automatically creates proxies for all Hubs that exist on the server, and registering a handler is how you indicate which Hubs your client intends to use.</span></span> <span data-ttu-id="2268f-165">Mais pour un client .NET vous créer des proxys de Hub manuellement, afin de SignalR part du principe que vous utilisez n’importe quel Hub que vous créez un proxy pour.</span><span class="sxs-lookup"><span data-stu-id="2268f-165">But for a .NET client you create Hub proxies manually, so SignalR assumes that you will be using any Hub that you create a proxy for.</span></span>


<span data-ttu-id="2268f-166">L’exemple de code utilise la valeur par défaut « / signalr « URL pour se connecter à votre service de SignalR.</span><span class="sxs-lookup"><span data-stu-id="2268f-166">The sample code uses the default "/signalr" URL to connect to your SignalR service.</span></span> <span data-ttu-id="2268f-167">Pour plus d’informations sur la façon de spécifier une autre URL de base, consultez [Guide de l’API ASP.NET SignalR Hubs - Server - URL /signalr](hubs-api-guide-server.md#signalrurl).</span><span class="sxs-lookup"><span data-stu-id="2268f-167">For information about how to specify a different base URL, see [ASP.NET SignalR Hubs API Guide - Server - The /signalr URL](hubs-api-guide-server.md#signalrurl).</span></span>

<span data-ttu-id="2268f-168">Le `Start` méthode s’exécute de façon asynchrone.</span><span class="sxs-lookup"><span data-stu-id="2268f-168">The `Start` method executes asynchronously.</span></span> <span data-ttu-id="2268f-169">Pour vous assurer que les lignes de code ne s’exécutent jusqu'à une fois la connexion établie, utilisez `await` dans une méthode asynchrone de ASP.NET 4.5 ou `.Wait()` dans une méthode synchrone.</span><span class="sxs-lookup"><span data-stu-id="2268f-169">To make sure that subsequent lines of code don't execute until after the connection is established, use `await` in an ASP.NET 4.5 asynchronous method or `.Wait()` in a synchronous method.</span></span> <span data-ttu-id="2268f-170">N’utilisez pas `.Wait()` dans un client WinRT.</span><span class="sxs-lookup"><span data-stu-id="2268f-170">Don't use `.Wait()` in a WinRT client.</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample2.cs?highlight=1)]

[!code-css[Main](hubs-api-guide-net-client/samples/sample3.css?highlight=1)]

<a id="slcrossdomain"></a>

### <a name="cross-domain-connections-from-silverlight-clients"></a><span data-ttu-id="2268f-171">Connexions inter-domaines à partir de clients Silverlight</span><span class="sxs-lookup"><span data-stu-id="2268f-171">Cross-domain connections from Silverlight clients</span></span>

<span data-ttu-id="2268f-172">Pour plus d’informations sur l’activation des connexions inter-domaines à partir de clients de Silverlight, consultez [rendre un Service disponible entre les limites du domaine](https://msdn.microsoft.com/library/cc197955(v=vs.95).aspx).</span><span class="sxs-lookup"><span data-stu-id="2268f-172">For information about how to enable cross-domain connections from Silverlight clients, see [Making a Service Available Across Domain Boundaries](https://msdn.microsoft.com/library/cc197955(v=vs.95).aspx).</span></span>

<a id="configureconnection"></a>

## <a name="how-to-configure-the-connection"></a><span data-ttu-id="2268f-173">Comment configurer la connexion</span><span class="sxs-lookup"><span data-stu-id="2268f-173">How to configure the connection</span></span>

<span data-ttu-id="2268f-174">Avant d’établir une connexion, vous pouvez spécifier une des options suivantes :</span><span class="sxs-lookup"><span data-stu-id="2268f-174">Before you establish a connection, you can specify any of the following options:</span></span>

- <span data-ttu-id="2268f-175">Limite de connexions simultanées.</span><span class="sxs-lookup"><span data-stu-id="2268f-175">Concurrent connections limit.</span></span>
- <span data-ttu-id="2268f-176">Paramètres de chaîne de requête.</span><span class="sxs-lookup"><span data-stu-id="2268f-176">Query string parameters.</span></span>
- <span data-ttu-id="2268f-177">Le mode de transport.</span><span class="sxs-lookup"><span data-stu-id="2268f-177">The transport method.</span></span>
- <span data-ttu-id="2268f-178">En-têtes HTTP.</span><span class="sxs-lookup"><span data-stu-id="2268f-178">HTTP headers.</span></span>
- <span data-ttu-id="2268f-179">Certificats clients.</span><span class="sxs-lookup"><span data-stu-id="2268f-179">Client certificates.</span></span>

<a id="maxconnections"></a>

### <a name="how-to-set-the-maximum-number-of-concurrent-connections-in-wpf-clients"></a><span data-ttu-id="2268f-180">Comment définir le nombre maximal de connexions simultanées dans les clients WPF</span><span class="sxs-lookup"><span data-stu-id="2268f-180">How to set the maximum number of concurrent connections in WPF clients</span></span>

<span data-ttu-id="2268f-181">Les clients de WPF, vous devrez peut-être augmenter le nombre maximal de connexions simultanées à partir de sa valeur par défaut de 2.</span><span class="sxs-lookup"><span data-stu-id="2268f-181">In WPF clients, you might have to increase the maximum number of concurrent connections from its default value of 2.</span></span> <span data-ttu-id="2268f-182">La valeur recommandée est 10.</span><span class="sxs-lookup"><span data-stu-id="2268f-182">The recommended value is 10.</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample4.cs?highlight=4)]

<span data-ttu-id="2268f-183">Pour plus d’informations, consultez [ServicePointManager.DefaultConnectionLimit](https://msdn.microsoft.com/library/system.net.servicepointmanager.defaultconnectionlimit.aspx).</span><span class="sxs-lookup"><span data-stu-id="2268f-183">For more information, see [ServicePointManager.DefaultConnectionLimit](https://msdn.microsoft.com/library/system.net.servicepointmanager.defaultconnectionlimit.aspx).</span></span>

<a id="querystring"></a>

### <a name="how-to-specify-query-string-parameters"></a><span data-ttu-id="2268f-184">Comment spécifier des paramètres de chaîne de requête</span><span class="sxs-lookup"><span data-stu-id="2268f-184">How to specify query string parameters</span></span>

<span data-ttu-id="2268f-185">Si vous souhaitez envoyer des données au serveur lorsque le client se connecte, vous pouvez ajouter des paramètres de chaîne de requête à l’objet de connexion.</span><span class="sxs-lookup"><span data-stu-id="2268f-185">If you want to send data to the server when the client connects, you can add query string parameters to the connection object.</span></span> <span data-ttu-id="2268f-186">L’exemple suivant montre comment définir un paramètre de chaîne de requête dans le code client.</span><span class="sxs-lookup"><span data-stu-id="2268f-186">The following example shows how to set a query string parameter in client code.</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample5.cs)]

<span data-ttu-id="2268f-187">L’exemple suivant montre comment lire un paramètre de chaîne de requête dans le code serveur.</span><span class="sxs-lookup"><span data-stu-id="2268f-187">The following example shows how to read a query string parameter in server code.</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample6.cs?highlight=5)]

<a id="transport"></a>

### <a name="how-to-specify-the-transport-method"></a><span data-ttu-id="2268f-188">Comment spécifier le mode de transport</span><span class="sxs-lookup"><span data-stu-id="2268f-188">How to specify the transport method</span></span>

<span data-ttu-id="2268f-189">Dans le cadre du processus de connexion, un client SignalR est normalement négocie avec le serveur pour déterminer le meilleur transport qui est pris en charge par le serveur et client.</span><span class="sxs-lookup"><span data-stu-id="2268f-189">As part of the process of connecting, a SignalR client normally negotiates with the server to determine the best transport that is supported by both server and client.</span></span> <span data-ttu-id="2268f-190">Si vous connaissez déjà le transport à utiliser, vous pouvez ignorer ce processus de négociation.</span><span class="sxs-lookup"><span data-stu-id="2268f-190">If you already know which transport you want to use, you can bypass this negotiation process.</span></span> <span data-ttu-id="2268f-191">Pour spécifier le mode de transport, passez un objet de transport à la méthode Start.</span><span class="sxs-lookup"><span data-stu-id="2268f-191">To specify the transport method, pass in a transport object to the Start method.</span></span> <span data-ttu-id="2268f-192">L’exemple suivant montre comment spécifier le mode de transport dans le code client.</span><span class="sxs-lookup"><span data-stu-id="2268f-192">The following example shows how to specify the transport method in client code.</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample7.cs?highlight=4)]

<span data-ttu-id="2268f-193">Le [Microsoft.AspNet.SignalR.Client.Transports](https://msdn.microsoft.com/library/jj918090(v=vs.111).aspx) espace de noms inclut les classes suivantes que vous pouvez utiliser pour spécifier le transport.</span><span class="sxs-lookup"><span data-stu-id="2268f-193">The [Microsoft.AspNet.SignalR.Client.Transports](https://msdn.microsoft.com/library/jj918090(v=vs.111).aspx) namespace includes the following classes that you can use to specify the transport.</span></span>

- <span data-ttu-id="2268f-194">[LongPollingTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.longpollingtransport(v=vs.111).aspx)</span><span class="sxs-lookup"><span data-stu-id="2268f-194">[LongPollingTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.longpollingtransport(v=vs.111).aspx)</span></span>
- <span data-ttu-id="2268f-195">[ServerSentEventsTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.serversenteventstransport(v=vs.111).aspx)</span><span class="sxs-lookup"><span data-stu-id="2268f-195">[ServerSentEventsTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.serversenteventstransport(v=vs.111).aspx)</span></span>
- <span data-ttu-id="2268f-196">[WebSocketTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.websockettransport(v=vs.111).aspx) (disponible uniquement lorsque le serveur et client utilisent .NET 4.5).</span><span class="sxs-lookup"><span data-stu-id="2268f-196">[WebSocketTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.websockettransport(v=vs.111).aspx) (Available only when both server and client use .NET 4.5.)</span></span>
- <span data-ttu-id="2268f-197">[AutoTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.autotransport(v=vs.111).aspx) (choisit automatiquement le transport de meilleures est pris en charge par le client et le serveur.</span><span class="sxs-lookup"><span data-stu-id="2268f-197">[AutoTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.autotransport(v=vs.111).aspx) (Automatically chooses the best transport that is supported by both the client and the server.</span></span> <span data-ttu-id="2268f-198">Il s’agit du transport par défaut.</span><span class="sxs-lookup"><span data-stu-id="2268f-198">This is the default transport.</span></span> <span data-ttu-id="2268f-199">En la passant dans à la `Start` méthode a le même effet que passant ne pas quoi que ce soit.)</span><span class="sxs-lookup"><span data-stu-id="2268f-199">Passing this in to the `Start` method has the same effect as not passing in anything.)</span></span>

<span data-ttu-id="2268f-200">Le transport ForeverFrame n’est pas inclus dans cette liste, car il est utilisé uniquement par les navigateurs.</span><span class="sxs-lookup"><span data-stu-id="2268f-200">The ForeverFrame transport is not included in this list because it is used only by browsers.</span></span>

<span data-ttu-id="2268f-201">Pour plus d’informations sur la vérification de la méthode de transport dans le code serveur, consultez [Guide de l’API ASP.NET SignalR Hubs - Server - comment obtenir des informations sur le client à partir de la propriété de contexte](hubs-api-guide-server.md#contextproperty).</span><span class="sxs-lookup"><span data-stu-id="2268f-201">For information about how to check the transport method in server code, see [ASP.NET SignalR Hubs API Guide - Server - How to get information about the client from the Context property](hubs-api-guide-server.md#contextproperty).</span></span> <span data-ttu-id="2268f-202">Pour plus d’informations sur les transports et les solutions de secours, consultez [Introduction à SignalR - Transports et les solutions de secours](../getting-started/introduction-to-signalr.md#transports).</span><span class="sxs-lookup"><span data-stu-id="2268f-202">For more information about transports and fallbacks, see [Introduction to SignalR - Transports and Fallbacks](../getting-started/introduction-to-signalr.md#transports).</span></span>

<a id="httpheaders"></a>

### <a name="how-to-specify-http-headers"></a><span data-ttu-id="2268f-203">Comment spécifier des en-têtes HTTP</span><span class="sxs-lookup"><span data-stu-id="2268f-203">How to specify HTTP headers</span></span>

<span data-ttu-id="2268f-204">Pour définir des en-têtes HTTP, utilisez le `Headers` propriété sur l’objet de connexion.</span><span class="sxs-lookup"><span data-stu-id="2268f-204">To set HTTP headers, use the `Headers` property on the connection object.</span></span> <span data-ttu-id="2268f-205">L’exemple suivant montre comment ajouter un en-tête HTTP.</span><span class="sxs-lookup"><span data-stu-id="2268f-205">The following example shows how to add an HTTP header.</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample8.cs?highlight=2)]

<a id="clientcertificate"></a>

### <a name="how-to-specify-client-certificates"></a><span data-ttu-id="2268f-206">Comment spécifier des certificats clients</span><span class="sxs-lookup"><span data-stu-id="2268f-206">How to specify client certificates</span></span>

<span data-ttu-id="2268f-207">Pour ajouter des certificats clients, utilisez la `AddClientCertificate` méthode sur l’objet de connexion.</span><span class="sxs-lookup"><span data-stu-id="2268f-207">To add client certificates, use the `AddClientCertificate` method on the connection object.</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample9.cs?highlight=2)]

<a id="proxy"></a>

## <a name="how-to-create-the-hub-proxy"></a><span data-ttu-id="2268f-208">Comment créer le concentrateur proxy</span><span class="sxs-lookup"><span data-stu-id="2268f-208">How to create the Hub proxy</span></span>

<span data-ttu-id="2268f-209">Pour définir des méthodes sur le client un Hub peut appeler à partir du serveur et pour appeler des méthodes sur un concentrateur sur le serveur, créer un proxy pour le Hub en appelant `CreateHubProxy` sur l’objet de connexion.</span><span class="sxs-lookup"><span data-stu-id="2268f-209">In order to define methods on the client that a Hub can call from the server, and to invoke methods on a Hub at the server, create a proxy for the Hub by calling `CreateHubProxy` on the connection object.</span></span> <span data-ttu-id="2268f-210">La chaîne vous passez à `CreateHubProxy` est le nom de votre classe de concentrateur ou le nom spécifié par le `HubName` attribut si un seul était utilisé sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="2268f-210">The string you pass in to `CreateHubProxy` is the name of your Hub class, or the name specified by the `HubName` attribute if one was used on the server.</span></span> <span data-ttu-id="2268f-211">Correspondance de noms respecte la casse.</span><span class="sxs-lookup"><span data-stu-id="2268f-211">Name matching is case-insensitive.</span></span>

<span data-ttu-id="2268f-212">**Classe de concentrateur sur le serveur**</span><span class="sxs-lookup"><span data-stu-id="2268f-212">**Hub class on server**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample10.cs?highlight=1)]

<span data-ttu-id="2268f-213">**Créer le proxy client pour la classe de concentrateur**</span><span class="sxs-lookup"><span data-stu-id="2268f-213">**Create client proxy for the Hub class**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample11.cs?highlight=2)]

<span data-ttu-id="2268f-214">Si vous décorez votre classe Hub avec un `HubName` d’attribut, utilisez ce nom.</span><span class="sxs-lookup"><span data-stu-id="2268f-214">If you decorate your Hub class with a `HubName` attribute, use that name.</span></span>

<span data-ttu-id="2268f-215">**Classe de concentrateur sur le serveur**</span><span class="sxs-lookup"><span data-stu-id="2268f-215">**Hub class on server**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample12.cs)]

<span data-ttu-id="2268f-216">**Créer le proxy client pour la classe de concentrateur**</span><span class="sxs-lookup"><span data-stu-id="2268f-216">**Create client proxy for the Hub class**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample13.cs?highlight=2)]

<span data-ttu-id="2268f-217">Si vous appelez `HubConnection.CreateHubProxy` plusieurs fois avec le même `hubName`, vous obtenez la même mise en cache `IHubProxy` objet.</span><span class="sxs-lookup"><span data-stu-id="2268f-217">If you call `HubConnection.CreateHubProxy` multiple times with the same `hubName`, you get the same cached `IHubProxy` object.</span></span>

<a id="callclient"></a>

## <a name="how-to-define-methods-on-the-client-that-the-server-can-call"></a><span data-ttu-id="2268f-218">Comment définir des méthodes sur le client que le serveur peut appeler.</span><span class="sxs-lookup"><span data-stu-id="2268f-218">How to define methods on the client that the server can call</span></span>

<span data-ttu-id="2268f-219">Pour définir une méthode que le serveur peut appeler, utilisez le proxy `On` méthode pour inscrire un gestionnaire d’événements.</span><span class="sxs-lookup"><span data-stu-id="2268f-219">To define a method that the server can call, use the proxy's `On` method to register an event handler.</span></span>

<span data-ttu-id="2268f-220">Correspondance de noms de méthode respecte la casse.</span><span class="sxs-lookup"><span data-stu-id="2268f-220">Method name matching is case-insensitive.</span></span> <span data-ttu-id="2268f-221">Par exemple, `Clients.All.UpdateStockPrice` sur le serveur s’exécutera `updateStockPrice`, `updatestockprice`, ou `UpdateStockPrice` sur le client.</span><span class="sxs-lookup"><span data-stu-id="2268f-221">For example, `Clients.All.UpdateStockPrice` on the server will execute `updateStockPrice`, `updatestockprice`, or `UpdateStockPrice` on the client.</span></span>

<span data-ttu-id="2268f-222">Plateformes clientes différentes exigences sont différentes pour l’écriture du code de méthode pour mettre à jour de l’interface utilisateur.</span><span class="sxs-lookup"><span data-stu-id="2268f-222">Different client platforms have different requirements for how you write method code to update the UI.</span></span> <span data-ttu-id="2268f-223">Les exemples présentés sont pour les clients de WinRT (Windows Store .NET).</span><span class="sxs-lookup"><span data-stu-id="2268f-223">The examples shown are for WinRT (Windows Store .NET) clients.</span></span> <span data-ttu-id="2268f-224">WPF, Silverlight et exemples d’applications console sont fournies dans [une section distincte plus loin dans cette rubrique](#wpfsl).</span><span class="sxs-lookup"><span data-stu-id="2268f-224">WPF, Silverlight, and console application examples are provided in [a separate section later in this topic](#wpfsl).</span></span>

<a id="clientmethodswithoutparms"></a>

### <a name="methods-without-parameters"></a><span data-ttu-id="2268f-225">Méthodes sans paramètres</span><span class="sxs-lookup"><span data-stu-id="2268f-225">Methods without parameters</span></span>

<span data-ttu-id="2268f-226">Si la méthode que vous traitez n’a pas de paramètres, utilisez la surcharge non générique de la `On` méthode :</span><span class="sxs-lookup"><span data-stu-id="2268f-226">If the method you're handling does not have parameters, use the non-generic overload of the `On` method:</span></span>

<span data-ttu-id="2268f-227">**Code de serveur client de méthode sans paramètres**</span><span class="sxs-lookup"><span data-stu-id="2268f-227">**Server code calling client method without parameters**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample14.cs?highlight=5)]

<span data-ttu-id="2268f-228">**Code de WinRT Client pour la méthode appelée depuis le serveur sans paramètres ([voir des exemples WPF et Silverlight plus loin dans cette rubrique](#wpfsl))**</span><span class="sxs-lookup"><span data-stu-id="2268f-228">**WinRT Client code for method called from server without parameters ([see WPF and Silverlight examples later in this topic](#wpfsl))**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample15.cs)]

<a id="clientmethodswithparmtypes"></a>

### <a name="methods-with-parameters-specifying-the-parameter-types"></a><span data-ttu-id="2268f-229">Méthodes avec des paramètres, en spécifiant les types de paramètres</span><span class="sxs-lookup"><span data-stu-id="2268f-229">Methods with parameters, specifying the parameter types</span></span>

<span data-ttu-id="2268f-230">Si la méthode que vous traitez possède des paramètres, spécifiez les types des paramètres comme les types génériques de la `On` (méthode).</span><span class="sxs-lookup"><span data-stu-id="2268f-230">If the method you're handling has parameters, specify the types of the parameters as the generic types of the `On` method.</span></span> <span data-ttu-id="2268f-231">Il existe des surcharges génériques de la `On` méthode pour vous permettre de spécifier des paramètres jusqu'à 8 (4 sur Windows Phone 7).</span><span class="sxs-lookup"><span data-stu-id="2268f-231">There are generic overloads of the `On` method to enable you to specify up to 8 parameters (4 on Windows Phone 7).</span></span> <span data-ttu-id="2268f-232">Dans l’exemple suivant, un seul paramètre est envoyé à la `UpdateStockPrice` (méthode).</span><span class="sxs-lookup"><span data-stu-id="2268f-232">In the following example, one parameter is sent to the `UpdateStockPrice` method.</span></span>

<span data-ttu-id="2268f-233">**Appel de méthode de client avec un paramètre de code de serveur**</span><span class="sxs-lookup"><span data-stu-id="2268f-233">**Server code calling client method with a parameter**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample16.cs?highlight=3)]

<span data-ttu-id="2268f-234">**La classe de Stock utilisée pour le paramètre**</span><span class="sxs-lookup"><span data-stu-id="2268f-234">**The Stock class used for the parameter**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample17.cs)]

<span data-ttu-id="2268f-235">**Code de WinRT Client pour une méthode appelée depuis le serveur avec un paramètre ([voir des exemples WPF et Silverlight plus loin dans cette rubrique](#wpfsl))**</span><span class="sxs-lookup"><span data-stu-id="2268f-235">**WinRT Client code for a method called from server with a parameter ([see WPF and Silverlight examples later in this topic](#wpfsl))**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample18.cs?highlight=1,5)]

<a id="clientmethodswithdynamparms"></a>

### <a name="methods-with-parameters-specifying-dynamic-objects-for-the-parameters"></a><span data-ttu-id="2268f-236">Méthodes avec des paramètres, en spécifiant des objets dynamiques pour les paramètres</span><span class="sxs-lookup"><span data-stu-id="2268f-236">Methods with parameters, specifying dynamic objects for the parameters</span></span>

<span data-ttu-id="2268f-237">Comme alternative à la spécification des paramètres comme des types génériques de la `On` (méthode), vous pouvez spécifier des paramètres en tant qu’objets dynamiques :</span><span class="sxs-lookup"><span data-stu-id="2268f-237">As an alternative to specifying parameters as generic types of the `On` method, you can specify parameters as dynamic objects:</span></span>

<span data-ttu-id="2268f-238">**Appel de méthode de client avec un paramètre de code de serveur**</span><span class="sxs-lookup"><span data-stu-id="2268f-238">**Server code calling client method with a parameter**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample19.cs?highlight=3)]

<span data-ttu-id="2268f-239">**La classe de Stock utilisée pour le paramètre**</span><span class="sxs-lookup"><span data-stu-id="2268f-239">**The Stock class used for the parameter**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample20.cs)]

<span data-ttu-id="2268f-240">**Code de WinRT Client pour une méthode appelée depuis le serveur avec un paramètre, à l’aide d’un objet dynamique pour le paramètre ([voir des exemples WPF et Silverlight plus loin dans cette rubrique](#wpfsl))**</span><span class="sxs-lookup"><span data-stu-id="2268f-240">**WinRT Client code for a method called from server with a parameter, using a dynamic object for the parameter ([see WPF and Silverlight examples later in this topic](#wpfsl))**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample21.cs?highlight=1,5)]

<a id="removehandler"></a>

### <a name="how-to-remove-a-handler"></a><span data-ttu-id="2268f-241">Comment supprimer un gestionnaire</span><span class="sxs-lookup"><span data-stu-id="2268f-241">How to remove a handler</span></span>

<span data-ttu-id="2268f-242">Pour supprimer un gestionnaire, appelez sa `Dispose` (méthode).</span><span class="sxs-lookup"><span data-stu-id="2268f-242">To remove a handler, call its `Dispose` method.</span></span>

<span data-ttu-id="2268f-243">**Code client pour une méthode appelée à partir du serveur**</span><span class="sxs-lookup"><span data-stu-id="2268f-243">**Client code for a method called from server**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample22.cs?highlight=1)]

<span data-ttu-id="2268f-244">**Code client pour supprimer le Gestionnaire**</span><span class="sxs-lookup"><span data-stu-id="2268f-244">**Client code to remove the handler**</span></span>

[!code-css[Main](hubs-api-guide-net-client/samples/sample23.css?highlight=1)]

<a id="callserver"></a>

## <a name="how-to-call-server-methods-from-the-client"></a><span data-ttu-id="2268f-245">Comment appeler des méthodes de serveur à partir du client</span><span class="sxs-lookup"><span data-stu-id="2268f-245">How to call server methods from the client</span></span>

<span data-ttu-id="2268f-246">Pour appeler une méthode sur le serveur, utilisez le `Invoke` méthode sur le concentrateur proxy.</span><span class="sxs-lookup"><span data-stu-id="2268f-246">To call a method on the server, use the `Invoke` method on the Hub proxy.</span></span>

<span data-ttu-id="2268f-247">Si la méthode de serveur n’a aucune valeur de retour, utilisez la surcharge non générique de la `Invoke` (méthode).</span><span class="sxs-lookup"><span data-stu-id="2268f-247">If the server method has no return value, use the non-generic overload of the `Invoke` method.</span></span>

<span data-ttu-id="2268f-248">**Code de serveur pour une méthode qui ne retourne aucune valeur**</span><span class="sxs-lookup"><span data-stu-id="2268f-248">**Server code for a method that has no return value**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample24.cs?highlight=3)]

<span data-ttu-id="2268f-249">**Code client appelant une méthode qui ne retourne aucune valeur**</span><span class="sxs-lookup"><span data-stu-id="2268f-249">**Client code calling a method that has no return value**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample25.cs?highlight=1)]

<span data-ttu-id="2268f-250">Si la méthode de serveur possède une valeur de retour, spécifiez le type de retour en tant que le type générique de la `Invoke` (méthode).</span><span class="sxs-lookup"><span data-stu-id="2268f-250">If the server method has a return value, specify the return type as the generic type of the `Invoke` method.</span></span>

<span data-ttu-id="2268f-251">**Code de serveur pour une méthode qui a une valeur de retour et accepte un paramètre de type complexe**</span><span class="sxs-lookup"><span data-stu-id="2268f-251">**Server code for a method that has a return value and takes a complex type parameter**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample26.cs?highlight=1)]

<span data-ttu-id="2268f-252">**La classe de Stock utilisée pour le paramètre et la valeur de retour**</span><span class="sxs-lookup"><span data-stu-id="2268f-252">**The Stock class used for the parameter and return value**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample27.cs)]

<span data-ttu-id="2268f-253">**Code client appelant une méthode qui a une valeur de retour et prend un paramètre de type complexe, dans une méthode async de ASP.NET 4.5**</span><span class="sxs-lookup"><span data-stu-id="2268f-253">**Client code calling a method that has a return value and takes a complex type parameter, in an ASP.NET 4.5 async method**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample28.cs?highlight=1-2)]

<span data-ttu-id="2268f-254">**Code client appelant une méthode qui a une valeur de retour et prend un paramètre de type complexe, dans une méthode synchrone**</span><span class="sxs-lookup"><span data-stu-id="2268f-254">**Client code calling a method that has a return value and takes a complex type parameter, in a synchronous method**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample29.cs?highlight=1-2)]

<span data-ttu-id="2268f-255">Le `Invoke` méthode exécute de façon asynchrone et retourne un `Task` objet.</span><span class="sxs-lookup"><span data-stu-id="2268f-255">The `Invoke` method executes asynchronously and returns a `Task` object.</span></span> <span data-ttu-id="2268f-256">Si vous ne spécifiez pas `await` ou `.Wait()`, la ligne de code suivante s’exécute avant que la méthode que vous appelez ait terminé l’exécution.</span><span class="sxs-lookup"><span data-stu-id="2268f-256">If you don't specify `await` or `.Wait()`, the next line of code will execute before the method that you invoke has finished executing.</span></span>

<a id="connectionlifetime"></a>

## <a name="how-to-handle-connection-lifetime-events"></a><span data-ttu-id="2268f-257">Comment gérer les événements de durée de vie de connexion</span><span class="sxs-lookup"><span data-stu-id="2268f-257">How to handle connection lifetime events</span></span>

<span data-ttu-id="2268f-258">SignalR fournit des événements de durée de vie que vous pouvez gérer la connexion suivante :</span><span class="sxs-lookup"><span data-stu-id="2268f-258">SignalR provides the following connection lifetime events that you can handle:</span></span>

- <span data-ttu-id="2268f-259">`Received`: Déclenché lorsque toutes les données sont reçues sur la connexion.</span><span class="sxs-lookup"><span data-stu-id="2268f-259">`Received`: Raised when any data is received on the connection.</span></span> <span data-ttu-id="2268f-260">Fournit les données reçues.</span><span class="sxs-lookup"><span data-stu-id="2268f-260">Provides the received data.</span></span>
- <span data-ttu-id="2268f-261">`ConnectionSlow`: Déclenché lorsque le client détecte une connexion lente ou suppression fréquemment.</span><span class="sxs-lookup"><span data-stu-id="2268f-261">`ConnectionSlow`: Raised when the client detects a slow or frequently dropping connection.</span></span>
- <span data-ttu-id="2268f-262">`Reconnecting`: Déclenché lorsque le transport sous-jacent commence à se reconnecter.</span><span class="sxs-lookup"><span data-stu-id="2268f-262">`Reconnecting`: Raised when the underlying transport begins reconnecting.</span></span>
- <span data-ttu-id="2268f-263">`Reconnected`: Déclenché lorsque le transport sous-jacent s’est reconnecté.</span><span class="sxs-lookup"><span data-stu-id="2268f-263">`Reconnected`: Raised when the underlying transport has reconnected.</span></span>
- <span data-ttu-id="2268f-264">`StateChanged`: Déclenché lorsque l’état de connexion change.</span><span class="sxs-lookup"><span data-stu-id="2268f-264">`StateChanged`: Raised when the connection state changes.</span></span> <span data-ttu-id="2268f-265">Fournit l’ancien état et le nouvel état.</span><span class="sxs-lookup"><span data-stu-id="2268f-265">Provides the old state and the new state.</span></span> <span data-ttu-id="2268f-266">Pour plus d’informations sur la connexion, valeurs d’état consultez [énumération ConnectionState](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.connectionstate(v=vs.111).aspx).</span><span class="sxs-lookup"><span data-stu-id="2268f-266">For information about connection state values see [ConnectionState Enumeration](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.connectionstate(v=vs.111).aspx).</span></span>
- <span data-ttu-id="2268f-267">`Closed`: Déclenché lorsque la connexion a déconnecté.</span><span class="sxs-lookup"><span data-stu-id="2268f-267">`Closed`: Raised when the connection has disconnected.</span></span>

<span data-ttu-id="2268f-268">Par exemple, si vous souhaitez afficher les messages d’avertissement pour les erreurs qui ne sont pas irrécupérable mais entraînent des problèmes de connexion intermittents, par exemple que lenteur ou fréquentes suppression de la connexion, gèrent le `ConnectionSlow` événement.</span><span class="sxs-lookup"><span data-stu-id="2268f-268">For example, if you want to display warning messages for errors that are not fatal but cause intermittent connection problems, such as slowness or frequent dropping of the connection, handle the `ConnectionSlow` event.</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample30.cs)]

<span data-ttu-id="2268f-269">Pour plus d’informations, consultez [compréhension et gestion des événements de durée de vie des connexions dans SignalR](handling-connection-lifetime-events.md).</span><span class="sxs-lookup"><span data-stu-id="2268f-269">For more information, see [Understanding and Handling Connection Lifetime Events in SignalR](handling-connection-lifetime-events.md).</span></span>

<a id="handleerrors"></a>

## <a name="how-to-handle-errors"></a><span data-ttu-id="2268f-270">Comment gérer les erreurs</span><span class="sxs-lookup"><span data-stu-id="2268f-270">How to handle errors</span></span>

<span data-ttu-id="2268f-271">Si vous n’activez explicitement les messages d’erreur détaillés sur le serveur, l’objet d’exception SignalR retourne après une erreur contient un minimum d’informations sur l’erreur.</span><span class="sxs-lookup"><span data-stu-id="2268f-271">If you don't explicitly enable detailed error messages on the server, the exception object that SignalR returns after an error contains minimal information about the error.</span></span> <span data-ttu-id="2268f-272">Par exemple, si un appel à `newContosoChatMessage` échoue, le message d’erreur dans l’objet d’erreur contient «`There was an error invoking Hub method 'contosoChatHub.newContosoChatMessage'.`« envoi de messages d’erreur détaillés aux clients en production n’est pas recommandé pour des raisons de sécurité, mais si vous souhaitez activer les messages d’erreur détaillés pour à des fins de résolution des problèmes, utilisez le code suivant sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="2268f-272">For example, if a call to `newContosoChatMessage` fails, the error message in the error object contains "`There was an error invoking Hub method 'contosoChatHub.newContosoChatMessage'.`" Sending detailed error messages to clients in production is not recommended for security reasons, but if you want to enable detailed error messages for troubleshooting purposes, use the following code on the server.</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample31.cs?highlight=2)]

<a id="handleerrors"></a>

<span data-ttu-id="2268f-273">Pour gérer les erreurs qui déclenche SignalR, vous pouvez ajouter un gestionnaire pour le `Error` événement sur l’objet de connexion.</span><span class="sxs-lookup"><span data-stu-id="2268f-273">To handle errors that SignalR raises, you can add a handler for the `Error` event on the connection object.</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample32.cs)]

<span data-ttu-id="2268f-274">Pour gérer les erreurs à partir des appels de méthode, encapsuler le code dans un bloc try-catch.</span><span class="sxs-lookup"><span data-stu-id="2268f-274">To handle errors from method invocations, wrap the code in a try-catch block.</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample33.cs)]

<a id="logging"></a>

## <a name="how-to-enable-client-side-logging"></a><span data-ttu-id="2268f-275">Comment activer la journalisation côté client</span><span class="sxs-lookup"><span data-stu-id="2268f-275">How to enable client-side logging</span></span>

<span data-ttu-id="2268f-276">Pour activer la journalisation côté client, définissez la `TraceLevel` et `TraceWriter` propriétés sur l’objet de connexion.</span><span class="sxs-lookup"><span data-stu-id="2268f-276">To enable client-side logging, set the `TraceLevel` and `TraceWriter` properties on the connection object.</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample34.cs?highlight=2-3)]

<a id="wpfsl"></a>

## <a name="wpf-silverlight-and-console-application-code-samples-for-client-methods-that-the-server-can-call"></a><span data-ttu-id="2268f-277">WPF, Silverlight et exemples de code d’application console pour les méthodes de client que le serveur peut appeler.</span><span class="sxs-lookup"><span data-stu-id="2268f-277">WPF, Silverlight, and console application code samples for client methods that the server can call</span></span>

<span data-ttu-id="2268f-278">Les exemples de code indiqués plus haut pour définir des méthodes de client que le serveur peut appeler s’appliquent aux clients de WinRT.</span><span class="sxs-lookup"><span data-stu-id="2268f-278">The code samples shown earlier for defining client methods that the server can call apply to WinRT clients.</span></span> <span data-ttu-id="2268f-279">Les exemples suivants montrent le code équivalent pour WPF, Silverlight et clients d’application console.</span><span class="sxs-lookup"><span data-stu-id="2268f-279">The following samples show the equivalent code for WPF, Silverlight, and console application clients.</span></span>

### <a name="methods-without-parameters"></a><span data-ttu-id="2268f-280">Méthodes sans paramètres</span><span class="sxs-lookup"><span data-stu-id="2268f-280">Methods without parameters</span></span>

<span data-ttu-id="2268f-281">**Code de client WPF pour la méthode appelée à partir du serveur sans paramètres**</span><span class="sxs-lookup"><span data-stu-id="2268f-281">**WPF client code for method called from server without parameters**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample35.cs?highlight=1)]

<span data-ttu-id="2268f-282">**Code de client Silverlight pour la méthode appelée à partir du serveur sans paramètres**</span><span class="sxs-lookup"><span data-stu-id="2268f-282">**Silverlight client code for method called from server without parameters**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample36.cs?highlight=1)]

<span data-ttu-id="2268f-283">**Code du client d’application console pour la méthode appelée depuis le serveur sans paramètres**</span><span class="sxs-lookup"><span data-stu-id="2268f-283">**Console application client code for method called from server without parameters**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample37.cs?highlight=1)]

### <a name="methods-with-parameters-specifying-the-parameter-types"></a><span data-ttu-id="2268f-284">Méthodes avec des paramètres, en spécifiant les types de paramètres</span><span class="sxs-lookup"><span data-stu-id="2268f-284">Methods with parameters, specifying the parameter types</span></span>

<span data-ttu-id="2268f-285">**Code de client WPF pour une méthode appelée à partir du serveur avec un paramètre**</span><span class="sxs-lookup"><span data-stu-id="2268f-285">**WPF client code for a method called from server with a parameter**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample38.cs?highlight=1,4)]

<span data-ttu-id="2268f-286">**Code de client Silverlight pour une méthode appelée à partir du serveur avec un paramètre**</span><span class="sxs-lookup"><span data-stu-id="2268f-286">**Silverlight client code for a method called from server with a parameter**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample39.cs?highlight=1,5)]

<span data-ttu-id="2268f-287">**Code du client d’application console pour une méthode appelée depuis le serveur avec un paramètre**</span><span class="sxs-lookup"><span data-stu-id="2268f-287">**Console application client code for a method called from server with a parameter**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample40.cs?highlight=1-2)]

### <a name="methods-with-parameters-specifying-dynamic-objects-for-the-parameters"></a><span data-ttu-id="2268f-288">Méthodes avec des paramètres, en spécifiant des objets dynamiques pour les paramètres</span><span class="sxs-lookup"><span data-stu-id="2268f-288">Methods with parameters, specifying dynamic objects for the parameters</span></span>

<span data-ttu-id="2268f-289">**Code de client WPF pour une méthode appelée à partir du serveur avec un paramètre, à l’aide d’un objet dynamique pour le paramètre**</span><span class="sxs-lookup"><span data-stu-id="2268f-289">**WPF client code for a method called from server with a parameter, using a dynamic object for the parameter**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample41.cs?highlight=1,4)]

<span data-ttu-id="2268f-290">**Code de client Silverlight pour une méthode appelée à partir du serveur avec un paramètre, à l’aide d’un objet dynamique pour le paramètre**</span><span class="sxs-lookup"><span data-stu-id="2268f-290">**Silverlight client code for a method called from server with a parameter, using a dynamic object for the parameter**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample42.cs?highlight=1,5)]

<span data-ttu-id="2268f-291">**Code du client d’application console pour une méthode appelée depuis le serveur avec un paramètre, à l’aide d’un objet dynamique pour le paramètre**</span><span class="sxs-lookup"><span data-stu-id="2268f-291">**Console application client code for a method called from server with a parameter, using a dynamic object for the parameter**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample43.cs?highlight=1-2)]
