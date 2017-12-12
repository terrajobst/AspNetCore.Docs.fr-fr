---
uid: signalr/overview/guide-to-the-api/hubs-api-guide-server
title: "Guide d’API ASP.NET SignalR concentrateurs - serveur (c#) | Documents Microsoft"
author: pfletcher
description: "Ce document fournit une introduction à la programmation côté serveur de l’API de concentrateurs SignalR ASP.NET pour SignalR version 2, avec des exemples de code illustrant..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: b19913e5-cd8a-4e4b-a872-5ac7a858a934
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/guide-to-the-api/hubs-api-guide-server
msc.type: authoredcontent
ms.openlocfilehash: 1cd5569554c3fbd966ee5d55ad08a79b81af36de
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/10/2017
---
<a name="aspnet-signalr-hubs-api-guide---server-c"></a><span data-ttu-id="98d9e-103">Guide d’API ASP.NET SignalR concentrateurs - serveur (c#)</span><span class="sxs-lookup"><span data-stu-id="98d9e-103">ASP.NET SignalR Hubs API Guide - Server (C#)</span></span>
====================
<span data-ttu-id="98d9e-104">par [Patrick Fletcher](https://github.com/pfletcher), [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="98d9e-104">by [Patrick Fletcher](https://github.com/pfletcher), [Tom Dykstra](https://github.com/tdykstra)</span></span>

> <span data-ttu-id="98d9e-105">Ce document fournit une introduction à la programmation côté serveur de l’API de concentrateurs SignalR ASP.NET pour SignalR version 2, avec des exemples de code illustrant les options courantes.</span><span class="sxs-lookup"><span data-stu-id="98d9e-105">This document provides an introduction to programming the server side of the ASP.NET SignalR Hubs API for SignalR version 2, with code samples demonstrating common options.</span></span>
> 
> <span data-ttu-id="98d9e-106">L’API de concentrateurs SignalR vous permet de vous permettent d’effectuer des appels de procédure distante (RPC) à partir d’un serveur pour les clients connectés et à partir de clients sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="98d9e-106">The SignalR Hubs API enables you to make remote procedure calls (RPCs) from a server to connected clients and from clients to the server.</span></span> <span data-ttu-id="98d9e-107">Dans le code serveur, vous définissez les méthodes qui peuvent être appelées par les clients, et vous appelez des méthodes qui s’exécutent sur le client.</span><span class="sxs-lookup"><span data-stu-id="98d9e-107">In server code, you define methods that can be called by clients, and you call methods that run on the client.</span></span> <span data-ttu-id="98d9e-108">Dans le code client, vous définissez les méthodes qui peuvent être appelées à partir du serveur, et vous appelez des méthodes qui s’exécutent sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="98d9e-108">In client code, you define methods that can be called from the server, and you call methods that run on the server.</span></span> <span data-ttu-id="98d9e-109">SignalR prend en charge de tous les éléments client-serveur pour vous.</span><span class="sxs-lookup"><span data-stu-id="98d9e-109">SignalR takes care of all of the client-to-server plumbing for you.</span></span>
> 
> <span data-ttu-id="98d9e-110">SignalR offre également une API de niveau inférieur appelée connexions persistantes.</span><span class="sxs-lookup"><span data-stu-id="98d9e-110">SignalR also offers a lower-level API called Persistent Connections.</span></span> <span data-ttu-id="98d9e-111">Pour obtenir une présentation SignalR, des concentrateurs et des connexions persistantes, consultez [Introduction à SignalR 2](../getting-started/introduction-to-signalr.md).</span><span class="sxs-lookup"><span data-stu-id="98d9e-111">For an introduction to SignalR, Hubs, and Persistent Connections, see [Introduction to SignalR 2](../getting-started/introduction-to-signalr.md).</span></span>
> 
> ## <a name="software-versions-used-in-this-topic"></a><span data-ttu-id="98d9e-112">Versions du logiciel utilisées dans cette rubrique</span><span class="sxs-lookup"><span data-stu-id="98d9e-112">Software versions used in this topic</span></span>
> 
> 
> - [<span data-ttu-id="98d9e-113">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="98d9e-113">Visual Studio 2013</span></span>](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - <span data-ttu-id="98d9e-114">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="98d9e-114">.NET 4.5</span></span>
> - <span data-ttu-id="98d9e-115">SignalR version 2</span><span class="sxs-lookup"><span data-stu-id="98d9e-115">SignalR version 2</span></span>
>   
> 
> 
> ## <a name="topic-versions"></a><span data-ttu-id="98d9e-116">Versions de la rubrique</span><span class="sxs-lookup"><span data-stu-id="98d9e-116">Topic versions</span></span>
> 
> <span data-ttu-id="98d9e-117">Pour plus d’informations sur les versions antérieures de SignalR, consultez [SignalR des Versions antérieures](../older-versions/index.md).</span><span class="sxs-lookup"><span data-stu-id="98d9e-117">For information about earlier versions of SignalR, see [SignalR Older Versions](../older-versions/index.md).</span></span>
> 
> ## <a name="questions-and-comments"></a><span data-ttu-id="98d9e-118">Questions et des commentaires</span><span class="sxs-lookup"><span data-stu-id="98d9e-118">Questions and comments</span></span>
> 
> <span data-ttu-id="98d9e-119">Veuillez laisser des commentaires sur la façon dont vous avez aimé ce didacticiel et nous pouvons améliorer dans les commentaires en bas de la page.</span><span class="sxs-lookup"><span data-stu-id="98d9e-119">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="98d9e-120">Si vous avez des questions qui ne sont pas directement liées à ce didacticiel, vous pouvez les valider pour le [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) ou [StackOverflow.com](http://stackoverflow.com/).</span><span class="sxs-lookup"><span data-stu-id="98d9e-120">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>


## <a name="overview"></a><span data-ttu-id="98d9e-121">Vue d'ensemble</span><span class="sxs-lookup"><span data-stu-id="98d9e-121">Overview</span></span>

<span data-ttu-id="98d9e-122">Ce document contient les sections suivantes :</span><span class="sxs-lookup"><span data-stu-id="98d9e-122">This document contains the following sections:</span></span>

- [<span data-ttu-id="98d9e-123">Comment inscrire SignalR intergiciel (middleware)</span><span class="sxs-lookup"><span data-stu-id="98d9e-123">How to register SignalR middleware</span></span>](#route)

    - [<span data-ttu-id="98d9e-124">L’URL /signalr</span><span class="sxs-lookup"><span data-stu-id="98d9e-124">The /signalr URL</span></span>](#signalrurl)
    - [<span data-ttu-id="98d9e-125">Configuration des options de SignalR</span><span class="sxs-lookup"><span data-stu-id="98d9e-125">Configuring SignalR options</span></span>](#options)
- [<span data-ttu-id="98d9e-126">Comment créer et utiliser des classes de Hub</span><span class="sxs-lookup"><span data-stu-id="98d9e-126">How to create and use Hub classes</span></span>](#hubclass)

    - [<span data-ttu-id="98d9e-127">Durée de vie Hub</span><span class="sxs-lookup"><span data-stu-id="98d9e-127">Hub object lifetime</span></span>](#transience)
    - [<span data-ttu-id="98d9e-128">Casse mixte des noms de concentrateur dans les clients JavaScript</span><span class="sxs-lookup"><span data-stu-id="98d9e-128">Camel-casing of Hub names in JavaScript clients</span></span>](#hubnames)
    - [<span data-ttu-id="98d9e-129">Plusieurs concentrateurs</span><span class="sxs-lookup"><span data-stu-id="98d9e-129">Multiple Hubs</span></span>](#multiplehubs)
    - [<span data-ttu-id="98d9e-130">Concentrateurs de fortement typé</span><span class="sxs-lookup"><span data-stu-id="98d9e-130">Strongly-Typed Hubs</span></span>](#stronglytypedhubs)
- [<span data-ttu-id="98d9e-131">Comment définir des méthodes dans la classe de concentrateur que les clients peuvent appeler.</span><span class="sxs-lookup"><span data-stu-id="98d9e-131">How to define methods in the Hub class that clients can call</span></span>](#hubmethods)

    - [<span data-ttu-id="98d9e-132">Casse mixte des noms de méthode dans les clients JavaScript</span><span class="sxs-lookup"><span data-stu-id="98d9e-132">Camel-casing of method names in JavaScript clients</span></span>](#methodnames)
    - [<span data-ttu-id="98d9e-133">Quand exécuter de façon asynchrone</span><span class="sxs-lookup"><span data-stu-id="98d9e-133">When to execute asynchronously</span></span>](#asyncmethods)
    - [<span data-ttu-id="98d9e-134">Définir des surcharges</span><span class="sxs-lookup"><span data-stu-id="98d9e-134">Defining overloads</span></span>](#overloads)
    - [<span data-ttu-id="98d9e-135">Signaler la progression à partir des appels de méthode de concentrateur</span><span class="sxs-lookup"><span data-stu-id="98d9e-135">Reporting progress from hub method invocations</span></span>](#progress)
- [<span data-ttu-id="98d9e-136">Comment appeler des méthodes de client à partir de la classe de concentrateur</span><span class="sxs-lookup"><span data-stu-id="98d9e-136">How to call client methods from the Hub class</span></span>](#callfromhub)

    - [<span data-ttu-id="98d9e-137">En sélectionnant les clients qui reçoivent l’appel RPC</span><span class="sxs-lookup"><span data-stu-id="98d9e-137">Selecting which clients will receive the RPC</span></span>](#selectingclients)
    - [<span data-ttu-id="98d9e-138">Aucune validation de la compilation pour les noms de méthode</span><span class="sxs-lookup"><span data-stu-id="98d9e-138">No compile-time validation for method names</span></span>](#dynamicmethodnames)
    - [<span data-ttu-id="98d9e-139">Une correspondance de nom sans respecter la casse (méthode)</span><span class="sxs-lookup"><span data-stu-id="98d9e-139">Case-insensitive method name matching</span></span>](#caseinsensitive)
    - [<span data-ttu-id="98d9e-140">Exécution asynchrone</span><span class="sxs-lookup"><span data-stu-id="98d9e-140">Asynchronous execution</span></span>](#asyncclient)
- [<span data-ttu-id="98d9e-141">Comment gérer l’appartenance au groupe à partir de la classe de concentrateur</span><span class="sxs-lookup"><span data-stu-id="98d9e-141">How to manage group membership from the Hub class</span></span>](#groupsfromhub)

    - [<span data-ttu-id="98d9e-142">Exécution asynchrone de méthodes Add et Remove</span><span class="sxs-lookup"><span data-stu-id="98d9e-142">Asynchronous execution of Add and Remove methods</span></span>](#asyncgroupmethods)
    - [<span data-ttu-id="98d9e-143">Persistance de l’appartenance au groupe</span><span class="sxs-lookup"><span data-stu-id="98d9e-143">Group membership persistence</span></span>](#grouppersistence)
    - [<span data-ttu-id="98d9e-144">Groupes d’utilisateur unique</span><span class="sxs-lookup"><span data-stu-id="98d9e-144">Single-user groups</span></span>](#singleusergroups)
- [<span data-ttu-id="98d9e-145">Comment gérer les événements de durée de vie de connexion dans la classe de concentrateur</span><span class="sxs-lookup"><span data-stu-id="98d9e-145">How to handle connection lifetime events in the Hub class</span></span>](#connectionlifetime)

    - [<span data-ttu-id="98d9e-146">Lorsque les OnConnected, OnDisconnected et OnReconnected sont appelées</span><span class="sxs-lookup"><span data-stu-id="98d9e-146">When OnConnected, OnDisconnected, and OnReconnected are called</span></span>](#onreconnected)
    - [<span data-ttu-id="98d9e-147">État de l’appelant ne pas remplie</span><span class="sxs-lookup"><span data-stu-id="98d9e-147">Caller state not populated</span></span>](#nocallerstate)
- [<span data-ttu-id="98d9e-148">Comment obtenir des informations sur le client à partir de la propriété de contexte</span><span class="sxs-lookup"><span data-stu-id="98d9e-148">How to get information about the client from the Context property</span></span>](#contextproperty)
- [<span data-ttu-id="98d9e-149">Comment passer l’état entre les clients et de la classe de concentrateur</span><span class="sxs-lookup"><span data-stu-id="98d9e-149">How to pass state between clients and the Hub class</span></span>](#passstate)
- [<span data-ttu-id="98d9e-150">Comment gérer les erreurs dans la classe de concentrateur</span><span class="sxs-lookup"><span data-stu-id="98d9e-150">How to handle errors in the Hub class</span></span>](#handleErrors)
- [<span data-ttu-id="98d9e-151">Comment appeler des méthodes de client et de gérer des groupes à l’extérieur de la classe de concentrateur</span><span class="sxs-lookup"><span data-stu-id="98d9e-151">How to call client methods and manage groups from outside the Hub class</span></span>](#callfromoutsidehub)

    - [<span data-ttu-id="98d9e-152">Appel de méthodes de client</span><span class="sxs-lookup"><span data-stu-id="98d9e-152">Calling client methods</span></span>](#callingclientsoutsidehub)
    - [<span data-ttu-id="98d9e-153">La gestion de l’appartenance au groupe</span><span class="sxs-lookup"><span data-stu-id="98d9e-153">Managing group membership</span></span>](#managinggroupsoutsidehub)
- [<span data-ttu-id="98d9e-154">Comment activer le suivi</span><span class="sxs-lookup"><span data-stu-id="98d9e-154">How to enable tracing</span></span>](#tracing)
- [<span data-ttu-id="98d9e-155">Comment personnaliser le pipeline de concentrateurs</span><span class="sxs-lookup"><span data-stu-id="98d9e-155">How to customize the Hubs pipeline</span></span>](#hubpipeline)

<span data-ttu-id="98d9e-156">Pour plus d’informations sur comment les clients du programme, consultez les ressources suivantes :</span><span class="sxs-lookup"><span data-stu-id="98d9e-156">For documentation on how to program clients, see the following resources:</span></span>

- [<span data-ttu-id="98d9e-157">Guide d’API concentrateurs SignalR - Client JavaScript</span><span class="sxs-lookup"><span data-stu-id="98d9e-157">SignalR Hubs API Guide - JavaScript Client</span></span>](hubs-api-guide-javascript-client.md)
- [<span data-ttu-id="98d9e-158">Guide d’API concentrateurs SignalR - Client .NET</span><span class="sxs-lookup"><span data-stu-id="98d9e-158">SignalR Hubs API Guide - .NET Client</span></span>](hubs-api-guide-net-client.md)

<span data-ttu-id="98d9e-159">Les composants serveur pour SignalR 2 sont uniquement disponibles dans .NET 4.5.</span><span class="sxs-lookup"><span data-stu-id="98d9e-159">The server components for SignalR 2 are only available in .NET 4.5.</span></span> <span data-ttu-id="98d9e-160">Serveurs exécutant .NET 4.0 doivent utiliser version 1.x de SignalR.</span><span class="sxs-lookup"><span data-stu-id="98d9e-160">Servers running .NET 4.0 must use SignalR v1.x.</span></span>

<a id="route"></a>

## <a name="how-to-register-signalr-middleware"></a><span data-ttu-id="98d9e-161">Comment inscrire SignalR intergiciel (middleware)</span><span class="sxs-lookup"><span data-stu-id="98d9e-161">How to register SignalR middleware</span></span>

<span data-ttu-id="98d9e-162">Pour définir l’itinéraire que les clients utiliseront pour se connecter à votre concentrateur, appelez le `MapSignalR` méthode lorsque l’application démarre.</span><span class="sxs-lookup"><span data-stu-id="98d9e-162">To define the route that clients will use to connect to your Hub, call the `MapSignalR` method when the application starts.</span></span> <span data-ttu-id="98d9e-163">`MapSignalR`est un [méthode d’extension](https://msdn.microsoft.com/en-us/library/vstudio/bb383977.aspx) pour la `OwinExtensions` classe.</span><span class="sxs-lookup"><span data-stu-id="98d9e-163">`MapSignalR` is an [extension method](https://msdn.microsoft.com/en-us/library/vstudio/bb383977.aspx) for the `OwinExtensions` class.</span></span> <span data-ttu-id="98d9e-164">L’exemple suivant montre comment définir l’itinéraire de concentrateurs SignalR à l’aide d’une classe de démarrage OWIN.</span><span class="sxs-lookup"><span data-stu-id="98d9e-164">The following example shows how to define the SignalR Hubs route using an OWIN startup class.</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample1.cs)]

<span data-ttu-id="98d9e-165">Si vous ajoutez des fonctionnalités de SignalR pour une application ASP.NET MVC, assurez-vous que l’itinéraire SignalR est ajouté avant les autres itinéraires.</span><span class="sxs-lookup"><span data-stu-id="98d9e-165">If you are adding SignalR functionality to an ASP.NET MVC application, make sure that the SignalR route is added before the other routes.</span></span> <span data-ttu-id="98d9e-166">Pour plus d’informations, consultez [didacticiel : mise en route avec SignalR 2 et MVC 5](../getting-started/tutorial-getting-started-with-signalr-and-mvc.md).</span><span class="sxs-lookup"><span data-stu-id="98d9e-166">For more information, see [Tutorial: Getting Started with SignalR 2 and MVC 5](../getting-started/tutorial-getting-started-with-signalr-and-mvc.md).</span></span>

<a id="signalrurl"></a>

### <a name="the-signalr-url"></a><span data-ttu-id="98d9e-167">L’URL /signalr</span><span class="sxs-lookup"><span data-stu-id="98d9e-167">The /signalr URL</span></span>

<span data-ttu-id="98d9e-168">Par défaut, l’URL de routage qui les clients utiliseront pour se connecter à votre concentrateur est « / signalr ».</span><span class="sxs-lookup"><span data-stu-id="98d9e-168">By default, the route URL which clients will use to connect to your Hub is "/signalr".</span></span> <span data-ttu-id="98d9e-169">(Ne confondez pas cette URL avec l’URL « concentrateurs signalr / / », qui est le fichier JavaScript généré automatiquement.</span><span class="sxs-lookup"><span data-stu-id="98d9e-169">(Don't confuse this URL with the "/signalr/hubs" URL, which is for the automatically generated JavaScript file.</span></span> <span data-ttu-id="98d9e-170">Pour plus d’informations sur le proxy généré, consultez [Guide API de concentrateurs SignalR - JavaScript Client - du proxy généré et ce qu’il fait pour vous](hubs-api-guide-javascript-client.md#genproxy).)</span><span class="sxs-lookup"><span data-stu-id="98d9e-170">For more information about the generated proxy, see [SignalR Hubs API Guide - JavaScript Client - The generated proxy and what it does for you](hubs-api-guide-javascript-client.md#genproxy).)</span></span>

<span data-ttu-id="98d9e-171">Il peut y avoir des circonstances exceptionnelles qui rendent cette URL de base n’est pas utilisable pour SignalR ; par exemple, vous avez un dossier dans votre projet nommé *signalr* et vous ne souhaitez pas modifier le nom.</span><span class="sxs-lookup"><span data-stu-id="98d9e-171">There might be extraordinary circumstances that make this base URL not usable for SignalR; for example, you have a folder in your project named *signalr* and you don't want to change the name.</span></span> <span data-ttu-id="98d9e-172">Dans ce cas, vous pouvez modifier l’URL de base, comme indiqué dans les exemples suivants (remplacez « / signalr » dans l’exemple de code avec l’URL de votre choix).</span><span class="sxs-lookup"><span data-stu-id="98d9e-172">In that case, you can change the base URL, as shown in the following examples (replace "/signalr" in the sample code with your desired URL).</span></span>

<span data-ttu-id="98d9e-173">**Code de serveur qui spécifie l’URL**</span><span class="sxs-lookup"><span data-stu-id="98d9e-173">**Server code that specifies the URL**</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample2.cs?highlight=1)]

<span data-ttu-id="98d9e-174">**Code JavaScript client qui spécifie l’URL (avec le proxy généré)**</span><span class="sxs-lookup"><span data-stu-id="98d9e-174">**JavaScript client code that specifies the URL (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-server/samples/sample3.js?highlight=1)]

<span data-ttu-id="98d9e-175">**Code JavaScript client qui spécifie l’URL (sans le proxy généré)**</span><span class="sxs-lookup"><span data-stu-id="98d9e-175">**JavaScript client code that specifies the URL (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-server/samples/sample4.js?highlight=1)]

<span data-ttu-id="98d9e-176">**Code client .NET qui spécifie l’URL**</span><span class="sxs-lookup"><span data-stu-id="98d9e-176">**.NET client code that specifies the URL**</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample5.cs?highlight=1)]

<a id="options"></a>

### <a name="configuring-signalr-options"></a><span data-ttu-id="98d9e-177">Configuration des Options de SignalR</span><span class="sxs-lookup"><span data-stu-id="98d9e-177">Configuring SignalR Options</span></span>

<span data-ttu-id="98d9e-178">Les surcharges de la `MapSignalR` méthode permettent de spécifier une URL personnalisée, un résolveur de dépendance personnalisée et les options suivantes :</span><span class="sxs-lookup"><span data-stu-id="98d9e-178">Overloads of the `MapSignalR` method enable you to specify a custom URL, a custom dependency resolver, and the following options:</span></span>

- <span data-ttu-id="98d9e-179">Activer les appels inter-domaines à partir de clients de navigateur à l’aide de CORS ou JSONP.</span><span class="sxs-lookup"><span data-stu-id="98d9e-179">Enable cross-domain calls using CORS or JSONP from browser clients.</span></span>

    <span data-ttu-id="98d9e-180">En général, si le navigateur charge une page à partir de `http://contoso.com`, la connexion SignalR est dans le même domaine, `http://contoso.com/signalr`.</span><span class="sxs-lookup"><span data-stu-id="98d9e-180">Typically if the browser loads a page from `http://contoso.com`, the SignalR connection is in the same domain, at `http://contoso.com/signalr`.</span></span> <span data-ttu-id="98d9e-181">Si la page à partir de `http://contoso.com` établit une connexion à `http://fabrikam.com/signalr`, qui est une connexion entre domaines.</span><span class="sxs-lookup"><span data-stu-id="98d9e-181">If the page from `http://contoso.com` makes a connection to `http://fabrikam.com/signalr`, that is a cross-domain connection.</span></span> <span data-ttu-id="98d9e-182">Pour des raisons de sécurité, les connexions inter-domaines sont désactivées par défaut.</span><span class="sxs-lookup"><span data-stu-id="98d9e-182">For security reasons, cross-domain connections are disabled by default.</span></span> <span data-ttu-id="98d9e-183">Pour plus d’informations, consultez [ASP.NET SignalR concentrateurs API Guide - JavaScript Client - comment établir une connexion entre domaines](hubs-api-guide-javascript-client.md#crossdomain).</span><span class="sxs-lookup"><span data-stu-id="98d9e-183">For more information, see [ASP.NET SignalR Hubs API Guide - JavaScript Client - How to establish a cross-domain connection](hubs-api-guide-javascript-client.md#crossdomain).</span></span>
- <span data-ttu-id="98d9e-184">Activer les messages d’erreur détaillés.</span><span class="sxs-lookup"><span data-stu-id="98d9e-184">Enable detailed error messages.</span></span>

    <span data-ttu-id="98d9e-185">Lorsque des erreurs se produisent, le comportement par défaut de SignalR consiste à envoyer aux clients un message de notification sans plus d’informations sur ce qui est arrivé.</span><span class="sxs-lookup"><span data-stu-id="98d9e-185">When errors occur, the default behavior of SignalR is to send to clients a notification message without details about what happened.</span></span> <span data-ttu-id="98d9e-186">Envoi des informations détaillées sur les clients n’est pas recommandée dans la production, car les utilisateurs malveillants peuvent être en mesure d’utiliser les informations à des attaques contre votre application.</span><span class="sxs-lookup"><span data-stu-id="98d9e-186">Sending detailed error information to clients is not recommended in production, because malicious users might be able to use the information in attacks against your application.</span></span> <span data-ttu-id="98d9e-187">Pour le dépannage, vous pouvez utiliser cette option pour activer temporairement le rapport d’erreurs plus détaillées.</span><span class="sxs-lookup"><span data-stu-id="98d9e-187">For troubleshooting, you can use this option to temporarily enable more informative error reporting.</span></span>
- <span data-ttu-id="98d9e-188">Désactiver les fichiers de proxy JavaScript générés automatiquement.</span><span class="sxs-lookup"><span data-stu-id="98d9e-188">Disable automatically generated JavaScript proxy files.</span></span>

    <span data-ttu-id="98d9e-189">Par défaut, un fichier JavaScript avec les serveurs proxy pour les classes de votre Hub est généré en réponse à l’URL « concentrateurs signalr / / ».</span><span class="sxs-lookup"><span data-stu-id="98d9e-189">By default, a JavaScript file with proxies for your Hub classes is generated in response to the URL "/signalr/hubs".</span></span> <span data-ttu-id="98d9e-190">Si vous ne souhaitez pas utiliser les proxy JavaScript, ou si vous souhaitez générer ce fichier manuellement et faire référence à un fichier physique de vos clients, vous pouvez utiliser cette option pour désactiver la génération de proxy.</span><span class="sxs-lookup"><span data-stu-id="98d9e-190">If you don't want to use the JavaScript proxies, or if you want to generate this file manually and refer to a physical file in your clients, you can use this option to disable proxy generation.</span></span> <span data-ttu-id="98d9e-191">Pour plus d’informations, consultez [Guide API de concentrateurs SignalR - JavaScript Client - proxy généré de la création d’un fichier physique pour SignalR](hubs-api-guide-javascript-client.md#manualproxy).</span><span class="sxs-lookup"><span data-stu-id="98d9e-191">For more information, see [SignalR Hubs API Guide - JavaScript Client - How to create a physical file for the SignalR generated proxy](hubs-api-guide-javascript-client.md#manualproxy).</span></span>

<span data-ttu-id="98d9e-192">L’exemple suivant montre comment spécifier l’URL de connexion SignalR et ces options dans un appel à la `MapSignalR` (méthode).</span><span class="sxs-lookup"><span data-stu-id="98d9e-192">The following example shows how to specify the SignalR connection URL and these options in a call to the `MapSignalR` method.</span></span> <span data-ttu-id="98d9e-193">Pour spécifier une URL personnalisée, remplacez « / signalr » dans l’exemple par l’URL que vous souhaitez utiliser.</span><span class="sxs-lookup"><span data-stu-id="98d9e-193">To specify a custom URL, replace "/signalr" in the example with the URL that you want to use.</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample6.cs)]

<a id="hubclass"></a>

## <a name="how-to-create-and-use-hub-classes"></a><span data-ttu-id="98d9e-194">Comment créer et utiliser des classes de Hub</span><span class="sxs-lookup"><span data-stu-id="98d9e-194">How to create and use Hub classes</span></span>

<span data-ttu-id="98d9e-195">Pour créer un concentrateur, créez une classe qui dérive de [Microsoft.Aspnet.Signalr.Hub](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.hub(v=vs.111).aspx).</span><span class="sxs-lookup"><span data-stu-id="98d9e-195">To create a Hub, create a class that derives from [Microsoft.Aspnet.Signalr.Hub](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.hub(v=vs.111).aspx).</span></span> <span data-ttu-id="98d9e-196">L’exemple suivant montre une classe simple de Hub pour une application de conversation.</span><span class="sxs-lookup"><span data-stu-id="98d9e-196">The following example shows a simple Hub class for a chat application.</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample7.cs)]

<span data-ttu-id="98d9e-197">Dans cet exemple, un client connecté peut appeler le `NewContosoChatMessage` (méthode), et dans ce cas, les données reçues sont envoyées à tous les clients connectés.</span><span class="sxs-lookup"><span data-stu-id="98d9e-197">In this example, a connected client can call the `NewContosoChatMessage` method, and when it does, the data received is broadcasted to all connected clients.</span></span>

<a id="transience"></a>

### <a name="hub-object-lifetime"></a><span data-ttu-id="98d9e-198">Durée de vie Hub</span><span class="sxs-lookup"><span data-stu-id="98d9e-198">Hub object lifetime</span></span>

<span data-ttu-id="98d9e-199">Vous n’instanciez la classe de concentrateur ou appeler ses méthodes à partir de votre propre code sur le serveur ; tout ce qui est réalisée pour vous par le pipeline de concentrateurs SignalR.</span><span class="sxs-lookup"><span data-stu-id="98d9e-199">You don't instantiate the Hub class or call its methods from your own code on the server; all that is done for you by the SignalR Hubs pipeline.</span></span> <span data-ttu-id="98d9e-200">SignalR crée une nouvelle instance de votre classe de concentrateur chaque fois qu’il faut gérer une opération de concentrateur par exemple quand un client se connecte, se déconnecte ou effectue une appel de méthode sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="98d9e-200">SignalR creates a new instance of your Hub class each time it needs to handle a Hub operation such as when a client connects, disconnects, or makes a method call to the server.</span></span>

<span data-ttu-id="98d9e-201">Étant donné que les instances de la classe de concentrateur sont temporaires, vous ne pouvez pas les utiliser pour gérer l’état à partir d’un appel de méthode à l’autre.</span><span class="sxs-lookup"><span data-stu-id="98d9e-201">Because instances of the Hub class are transient, you can't use them to maintain state from one method call to the next.</span></span> <span data-ttu-id="98d9e-202">Chaque fois que le serveur reçoit un appel de méthode à partir d’un client, une nouvelle instance de votre processus de classe de concentrateur le message.</span><span class="sxs-lookup"><span data-stu-id="98d9e-202">Each time the server receives a method call from a client, a new instance of your Hub class processes the message.</span></span> <span data-ttu-id="98d9e-203">Pour conserver l’état via plusieurs connexions et les appels de méthode, utilisez une autre méthode, tel qu’une base de données, ou une variable statique sur la classe de concentrateur ou une autre classe ne dérive pas de `Hub`.</span><span class="sxs-lookup"><span data-stu-id="98d9e-203">To maintain state through multiple connections and method calls, use some other method such as a database, or a static variable on the Hub class, or a different class that does not derive from `Hub`.</span></span> <span data-ttu-id="98d9e-204">Si vous conserver les données en mémoire, à l’aide d’une méthode comme une variable statique sur la classe de concentrateur, les données seront perdues lors du recyclage du domaine d’application.</span><span class="sxs-lookup"><span data-stu-id="98d9e-204">If you persist data in memory, using a method such as a static variable on the Hub class, the data will be lost when the app domain recycles.</span></span>

<span data-ttu-id="98d9e-205">Si vous souhaitez envoyer des messages aux clients de votre propre code qui s’exécute en dehors de la classe de concentrateur, vous ne peut pas le faire en instanciant une instance de classe de concentrateur, mais vous pouvez le faire en obtenant une référence à l’objet de contexte SignalR pour votre classe de concentrateur.</span><span class="sxs-lookup"><span data-stu-id="98d9e-205">If you want to send messages to clients from your own code that runs outside the Hub class, you can't do it by instantiating a Hub class instance, but you can do it by getting a reference to the SignalR context object for your Hub class.</span></span> <span data-ttu-id="98d9e-206">Pour plus d’informations, consultez [comment appeler des méthodes de client et de gérer des groupes à l’extérieur de la classe de concentrateur](#callfromoutsidehub) plus loin dans cette rubrique.</span><span class="sxs-lookup"><span data-stu-id="98d9e-206">For more information, see [How to call client methods and manage groups from outside the Hub class](#callfromoutsidehub) later in this topic.</span></span>

<a id="hubnames"></a>

### <a name="camel-casing-of-hub-names-in-javascript-clients"></a><span data-ttu-id="98d9e-207">Casse mixte des noms de concentrateur dans les clients JavaScript</span><span class="sxs-lookup"><span data-stu-id="98d9e-207">Camel-casing of Hub names in JavaScript clients</span></span>

<span data-ttu-id="98d9e-208">Par défaut, les clients JavaScript font référence à des concentrateurs à l’aide d’une version de casse mixte du nom de classe.</span><span class="sxs-lookup"><span data-stu-id="98d9e-208">By default, JavaScript clients refer to Hubs by using a camel-cased version of the class name.</span></span> <span data-ttu-id="98d9e-209">SignalR effectue automatiquement cette modification afin que le code JavaScript peut être conforme aux conventions de JavaScript.</span><span class="sxs-lookup"><span data-stu-id="98d9e-209">SignalR automatically makes this change so that JavaScript code can conform to JavaScript conventions.</span></span> <span data-ttu-id="98d9e-210">L’exemple précédent est désigné en tant que `contosoChatHub` dans le code JavaScript.</span><span class="sxs-lookup"><span data-stu-id="98d9e-210">The previous example would be referred to as `contosoChatHub` in JavaScript code.</span></span>

<span data-ttu-id="98d9e-211">**Serveur**</span><span class="sxs-lookup"><span data-stu-id="98d9e-211">**Server**</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample8.cs?highlight=1)]

<span data-ttu-id="98d9e-212">**Client JavaScript à l’aide du proxy généré**</span><span class="sxs-lookup"><span data-stu-id="98d9e-212">**JavaScript client using generated proxy**</span></span>

[!code-javascript[Main](hubs-api-guide-server/samples/sample9.js?highlight=1)]

<span data-ttu-id="98d9e-213">Si vous souhaitez spécifier un autre nom pour les clients, ajoutez le `HubName` attribut.</span><span class="sxs-lookup"><span data-stu-id="98d9e-213">If you want to specify a different name for clients to use, add the `HubName` attribute.</span></span> <span data-ttu-id="98d9e-214">Lorsque vous utilisez un `HubName` d’attribut, il n’existe aucun changement de nom en casse mixte sur les clients JavaScript.</span><span class="sxs-lookup"><span data-stu-id="98d9e-214">When you use a `HubName` attribute, there is no name change to camel case on JavaScript clients.</span></span>

<span data-ttu-id="98d9e-215">**Serveur**</span><span class="sxs-lookup"><span data-stu-id="98d9e-215">**Server**</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample10.cs?highlight=1)]

<span data-ttu-id="98d9e-216">**Client JavaScript à l’aide du proxy généré**</span><span class="sxs-lookup"><span data-stu-id="98d9e-216">**JavaScript client using generated proxy**</span></span>

[!code-javascript[Main](hubs-api-guide-server/samples/sample11.js?highlight=1)]

<a id="multiplehubs"></a>

### <a name="multiple-hubs"></a><span data-ttu-id="98d9e-217">Plusieurs concentrateurs</span><span class="sxs-lookup"><span data-stu-id="98d9e-217">Multiple Hubs</span></span>

<span data-ttu-id="98d9e-218">Vous pouvez définir plusieurs classes de Hub dans une application.</span><span class="sxs-lookup"><span data-stu-id="98d9e-218">You can define multiple Hub classes in an application.</span></span> <span data-ttu-id="98d9e-219">Lorsque vous procédez ainsi, la connexion est partagée, mais les groupes sont distincts :</span><span class="sxs-lookup"><span data-stu-id="98d9e-219">When you do that, the connection is shared but groups are separate:</span></span>

- <span data-ttu-id="98d9e-220">Tous les clients utiliseront la même URL pour établir une connexion SignalR avec votre service (« / signalr » ou l’URL personnalisée si vous avez défini un), et que la connexion est utilisée pour tous les Hubs définis par le service.</span><span class="sxs-lookup"><span data-stu-id="98d9e-220">All clients will use the same URL to establish a SignalR connection with your service ("/signalr" or your custom URL if you specified one), and that connection is used for all Hubs defined by the service.</span></span>

    <span data-ttu-id="98d9e-221">Il n’existe aucune différence de performances pour plusieurs concentrateurs par rapport à la définition de toutes les fonctionnalités de concentrateur dans une classe unique.</span><span class="sxs-lookup"><span data-stu-id="98d9e-221">There is no performance difference for multiple Hubs compared to defining all Hub functionality in a single class.</span></span>
- <span data-ttu-id="98d9e-222">Tous les concentrateurs d’Obtient les mêmes informations de demande HTTP.</span><span class="sxs-lookup"><span data-stu-id="98d9e-222">All Hubs get the same HTTP request information.</span></span>

    <span data-ttu-id="98d9e-223">Étant donné que tous les concentrateurs partagent la même connexion, les informations de demande HTTP uniquement le serveur obtient sont ce qui est fourni dans la requête HTTP d’origine qui établit la connexion de SignalR.</span><span class="sxs-lookup"><span data-stu-id="98d9e-223">Since all Hubs share the same connection, the only HTTP request information that the server gets is what comes in the original HTTP request that establishes the SignalR connection.</span></span> <span data-ttu-id="98d9e-224">Si vous utilisez la demande de connexion pour transmettre des informations à partir du client au serveur en spécifiant une chaîne de requête, vous ne peut pas fournir des chaînes de requête différents aux concentrateurs différents.</span><span class="sxs-lookup"><span data-stu-id="98d9e-224">If you use the connection request to pass information from the client to the server by specifying a query string, you can't provide different query strings to different Hubs.</span></span> <span data-ttu-id="98d9e-225">Tous les concentrateurs recevront les mêmes informations.</span><span class="sxs-lookup"><span data-stu-id="98d9e-225">All Hubs will receive the same information.</span></span>
- <span data-ttu-id="98d9e-226">Le fichier de proxies JavaScript généré contiendra des proxys pour tous les Hubs dans un seul fichier.</span><span class="sxs-lookup"><span data-stu-id="98d9e-226">The generated JavaScript proxies file will contain proxies for all Hubs in one file.</span></span>

    <span data-ttu-id="98d9e-227">Pour plus d’informations sur les proxies JavaScript, consultez [Guide API de concentrateurs SignalR - JavaScript Client - du proxy généré et ce qu’il fait pour vous](hubs-api-guide-javascript-client.md#genproxy).</span><span class="sxs-lookup"><span data-stu-id="98d9e-227">For information about JavaScript proxies, see [SignalR Hubs API Guide - JavaScript Client - The generated proxy and what it does for you](hubs-api-guide-javascript-client.md#genproxy).</span></span>
- <span data-ttu-id="98d9e-228">Les groupes sont définis dans les concentrateurs.</span><span class="sxs-lookup"><span data-stu-id="98d9e-228">Groups are defined within Hubs.</span></span>

    <span data-ttu-id="98d9e-229">Dans SignalR, que vous pouvez définir des groupes pour diffuser à des sous-ensembles de clients connectés nommés.</span><span class="sxs-lookup"><span data-stu-id="98d9e-229">In SignalR you can define named groups to broadcast to subsets of connected clients.</span></span> <span data-ttu-id="98d9e-230">Les groupes sont gérées séparément pour chaque concentrateur.</span><span class="sxs-lookup"><span data-stu-id="98d9e-230">Groups are maintained separately for each Hub.</span></span> <span data-ttu-id="98d9e-231">Par exemple, un groupe nommé « Administrateurs » inclut un ensemble de clients pour votre `ContosoChatHub` rapporte classe et le même nom de groupe à un autre ensemble de clients pour votre `StockTickerHub` classe.</span><span class="sxs-lookup"><span data-stu-id="98d9e-231">For example, a group named "Administrators" would include one set of clients for your `ContosoChatHub` class, and the same group name would refer to a different set of clients for your `StockTickerHub` class.</span></span>

<a id="stronglytypedhubs"></a>
### <a name="strongly-typed-hubs"></a><span data-ttu-id="98d9e-232">Concentrateurs de fortement typé</span><span class="sxs-lookup"><span data-stu-id="98d9e-232">Strongly-Typed Hubs</span></span>

<span data-ttu-id="98d9e-233">Pour définir une interface pour les méthodes de concentrateur votre client peut référence (et activer Intellisense dans vos méthodes de concentrateur), dérivez votre concentrateur `Hub<T>` (introduite dans SignalR 2.1) plutôt que `Hub`:</span><span class="sxs-lookup"><span data-stu-id="98d9e-233">To define an interface for your hub methods that your client can reference (and enable Intellisense on your hub methods), derive your hub from `Hub<T>` (introduced in SignalR 2.1) rather than `Hub`:</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample12.cs)]

<a id="hubmethods"></a>

## <a name="how-to-define-methods-in-the-hub-class-that-clients-can-call"></a><span data-ttu-id="98d9e-234">Comment définir des méthodes dans la classe de concentrateur que les clients peuvent appeler.</span><span class="sxs-lookup"><span data-stu-id="98d9e-234">How to define methods in the Hub class that clients can call</span></span>

<span data-ttu-id="98d9e-235">Pour exposer une méthode sur le concentrateur que vous souhaitez pouvoir être appelée à partir du client, déclarez une méthode publique, comme indiqué dans les exemples suivants.</span><span class="sxs-lookup"><span data-stu-id="98d9e-235">To expose a method on the Hub that you want to be callable from the client, declare a public method, as shown in the following examples.</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample13.cs?highlight=3)]

[!code-csharp[Main](hubs-api-guide-server/samples/sample14.cs?highlight=3)]

<span data-ttu-id="98d9e-236">Vous pouvez spécifier un type de retour et des paramètres, y compris les types complexes et les tableaux, comme vous le feriez dans n’importe quelle méthode c#.</span><span class="sxs-lookup"><span data-stu-id="98d9e-236">You can specify a return type and parameters, including complex types and arrays, as you would in any C# method.</span></span> <span data-ttu-id="98d9e-237">Toutes les données dans les paramètres de réception ou de retourner à l’appelant sont communiquées entre le client et le serveur à l’aide de JSON et SignalR gère la liaison d’objets complexes et les tableaux d’objets automatiquement.</span><span class="sxs-lookup"><span data-stu-id="98d9e-237">Any data that you receive in parameters or return to the caller is communicated between the client and the server by using JSON, and SignalR handles the binding of complex objects and arrays of objects automatically.</span></span>

<a id="methodnames"></a>

### <a name="camel-casing-of-method-names-in-javascript-clients"></a><span data-ttu-id="98d9e-238">Casse mixte des noms de méthode dans les clients JavaScript</span><span class="sxs-lookup"><span data-stu-id="98d9e-238">Camel-casing of method names in JavaScript clients</span></span>

<span data-ttu-id="98d9e-239">Par défaut, les clients JavaScript font référence aux méthodes de concentrateur à l’aide d’une version de casse mixte du nom de la méthode.</span><span class="sxs-lookup"><span data-stu-id="98d9e-239">By default, JavaScript clients refer to Hub methods by using a camel-cased version of the method name.</span></span> <span data-ttu-id="98d9e-240">SignalR effectue automatiquement cette modification afin que le code JavaScript peut être conforme aux conventions de JavaScript.</span><span class="sxs-lookup"><span data-stu-id="98d9e-240">SignalR automatically makes this change so that JavaScript code can conform to JavaScript conventions.</span></span>

<span data-ttu-id="98d9e-241">**Serveur**</span><span class="sxs-lookup"><span data-stu-id="98d9e-241">**Server**</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample15.cs?highlight=1)]

<span data-ttu-id="98d9e-242">**Client JavaScript à l’aide du proxy généré**</span><span class="sxs-lookup"><span data-stu-id="98d9e-242">**JavaScript client using generated proxy**</span></span>

[!code-javascript[Main](hubs-api-guide-server/samples/sample16.js?highlight=1)]

<span data-ttu-id="98d9e-243">Si vous souhaitez spécifier un autre nom pour les clients, ajoutez le `HubMethodName` attribut.</span><span class="sxs-lookup"><span data-stu-id="98d9e-243">If you want to specify a different name for clients to use, add the `HubMethodName` attribute.</span></span>

<span data-ttu-id="98d9e-244">**Serveur**</span><span class="sxs-lookup"><span data-stu-id="98d9e-244">**Server**</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample17.cs?highlight=1)]

<span data-ttu-id="98d9e-245">**Client JavaScript à l’aide du proxy généré**</span><span class="sxs-lookup"><span data-stu-id="98d9e-245">**JavaScript client using generated proxy**</span></span>

[!code-javascript[Main](hubs-api-guide-server/samples/sample18.js?highlight=1)]

<a id="asyncmethods"></a>

### <a name="when-to-execute-asynchronously"></a><span data-ttu-id="98d9e-246">Quand exécuter de façon asynchrone</span><span class="sxs-lookup"><span data-stu-id="98d9e-246">When to execute asynchronously</span></span>

<span data-ttu-id="98d9e-247">Si la méthode sera être longue ou effectuer le travail qui serait impliquent en attente, telles que la recherche d’une base de données ou un appel de service web, la méthode de concentrateur asynchrone en retournant un [tâche](https://msdn.microsoft.com/en-us/library/system.threading.tasks.task.aspx) (à la place de `void` retourner) ou [ Tâche&lt;T&gt; ](https://msdn.microsoft.com/en-us/library/dd321424.aspx) objet (à la place de `T` type de retour).</span><span class="sxs-lookup"><span data-stu-id="98d9e-247">If the method will be long-running or has to do work that would involve waiting, such as a database lookup or a web service call, make the Hub method asynchronous by returning a [Task](https://msdn.microsoft.com/en-us/library/system.threading.tasks.task.aspx) (in place of `void` return) or [Task&lt;T&gt;](https://msdn.microsoft.com/en-us/library/dd321424.aspx) object (in place of `T` return type).</span></span> <span data-ttu-id="98d9e-248">Lorsque vous retournez un `Task` objet à partir de la méthode SignalR attend le `Task` se termine, puis il transmet le résultat désencapsulé au client, il n’existe aucune différence dans la façon dont vous codez l’appel de méthode dans le client.</span><span class="sxs-lookup"><span data-stu-id="98d9e-248">When you return a `Task` object from the method, SignalR waits for the `Task` to complete, and then it sends the unwrapped result back to the client, so there is no difference in how you code the method call in the client.</span></span>

<span data-ttu-id="98d9e-249">Effectue une méthode de concentrateur asynchrone évite de bloquer la connexion lorsqu’il utilise le transport WebSocket.</span><span class="sxs-lookup"><span data-stu-id="98d9e-249">Making a Hub method asynchronous avoids blocking the connection when it uses the WebSocket transport.</span></span> <span data-ttu-id="98d9e-250">Lorsqu’une méthode de concentrateur exécute de façon synchrone et le transport est WebSocket, les appels suivants de méthodes sur le concentrateur du même client sont bloquées jusqu'à la fin de la méthode de concentrateur.</span><span class="sxs-lookup"><span data-stu-id="98d9e-250">When a Hub method executes synchronously and the transport is WebSocket, subsequent invocations of methods on the Hub from the same client are blocked until the Hub method completes.</span></span>

<span data-ttu-id="98d9e-251">L’exemple suivant montre la même méthode codé pour s’exécuter de façon synchrone ou asynchrone, suivi par le code JavaScript client qui fonctionne pour appeler des versions.</span><span class="sxs-lookup"><span data-stu-id="98d9e-251">The following example shows the same method coded to run synchronously or asynchronously, followed by JavaScript client code that works for calling either version.</span></span>

<span data-ttu-id="98d9e-252">**Synchrone**</span><span class="sxs-lookup"><span data-stu-id="98d9e-252">**Synchronous**</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample19.cs)]

<span data-ttu-id="98d9e-253">**Asynchrone**</span><span class="sxs-lookup"><span data-stu-id="98d9e-253">**Asynchronous**</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample20.cs?highlight=1,7-8)]

<span data-ttu-id="98d9e-254">**Client JavaScript à l’aide du proxy généré**</span><span class="sxs-lookup"><span data-stu-id="98d9e-254">**JavaScript client using generated proxy**</span></span>

[!code-javascript[Main](hubs-api-guide-server/samples/sample21.js)]

<span data-ttu-id="98d9e-255">Pour plus d’informations sur l’utilisation de méthodes asynchrones dans ASP.NET 4.5, consultez [à l’aide de méthodes asynchrones dans ASP.NET MVC 4](../../../mvc/overview/performance/using-asynchronous-methods-in-aspnet-mvc-4.md).</span><span class="sxs-lookup"><span data-stu-id="98d9e-255">For more information about how to use asynchronous methods in ASP.NET 4.5, see [Using Asynchronous Methods in ASP.NET MVC 4](../../../mvc/overview/performance/using-asynchronous-methods-in-aspnet-mvc-4.md).</span></span>

<a id="overloads"></a>

### <a name="defining-overloads"></a><span data-ttu-id="98d9e-256">Définir des surcharges</span><span class="sxs-lookup"><span data-stu-id="98d9e-256">Defining Overloads</span></span>

<span data-ttu-id="98d9e-257">Si vous souhaitez définir de surcharges d’une méthode, le nombre de paramètres dans chaque surcharge doit être différent.</span><span class="sxs-lookup"><span data-stu-id="98d9e-257">If you want to define overloads for a method, the number of parameters in each overload must be different.</span></span> <span data-ttu-id="98d9e-258">Si vous différencier une surcharge simplement en spécifiant les types de paramètres différents, votre classe de concentrateur est compilés mais le service SignalR lève une exception au moment de l’exécution lorsque les clients essaient pour appeler l’une des surcharges.</span><span class="sxs-lookup"><span data-stu-id="98d9e-258">If you differentiate an overload just by specifying different parameter types, your Hub class will compile but the SignalR service will throw an exception at run time when clients try to call one of the overloads.</span></span>

<a id="progress"></a>
### <a name="reporting-progress-from-hub-method-invocations"></a><span data-ttu-id="98d9e-259">Signaler la progression à partir des appels de méthode de concentrateur</span><span class="sxs-lookup"><span data-stu-id="98d9e-259">Reporting progress from hub method invocations</span></span>

<span data-ttu-id="98d9e-260">SignalR 2.1 prend en charge la [modèle de rapport de progression](https://blogs.msdn.com/b/dotnet/archive/2012/06/06/async-in-4-5-enabling-progress-and-cancellation-in-async-apis.aspx) introduite dans .NET 4.5.</span><span class="sxs-lookup"><span data-stu-id="98d9e-260">SignalR 2.1 adds support for the [progress reporting pattern](https://blogs.msdn.com/b/dotnet/archive/2012/06/06/async-in-4-5-enabling-progress-and-cancellation-in-async-apis.aspx) introduced in .NET 4.5.</span></span> <span data-ttu-id="98d9e-261">Pour implémenter le rapport de progression, définissez un `IProgress<T>` paramètre pour la méthode de concentrateur qui permettre accéder à votre client :</span><span class="sxs-lookup"><span data-stu-id="98d9e-261">To implement progress reporting, define an `IProgress<T>` parameter for your hub method that your client can access:</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample22.cs)]

<span data-ttu-id="98d9e-262">Lorsque vous écrivez une méthode de serveur longue, il est important d’utiliser un modèle de programmation asynchrone comme Async / Await plutôt qu’un blocage du thread de concentrateur.</span><span class="sxs-lookup"><span data-stu-id="98d9e-262">When writing a long-running server method, it is important to use an asynchronous programming pattern like Async/ Await rather than blocking the hub thread.</span></span>

<a id="callfromhub"></a>

## <a name="how-to-call-client-methods-from-the-hub-class"></a><span data-ttu-id="98d9e-263">Comment appeler des méthodes de client à partir de la classe de concentrateur</span><span class="sxs-lookup"><span data-stu-id="98d9e-263">How to call client methods from the Hub class</span></span>

<span data-ttu-id="98d9e-264">Pour appeler des méthodes de client à partir du serveur, utilisez le `Clients` propriété dans une méthode dans votre classe de concentrateur.</span><span class="sxs-lookup"><span data-stu-id="98d9e-264">To call client methods from the server, use the `Clients` property in a method in your Hub class.</span></span> <span data-ttu-id="98d9e-265">L’exemple suivant montre le code de serveur qui appelle `addNewMessageToPage` sur tous les clients connectés et le code client qui définit la méthode dans un client JavaScript.</span><span class="sxs-lookup"><span data-stu-id="98d9e-265">The following example shows server code that calls `addNewMessageToPage` on all connected clients, and client code that defines the method in a JavaScript client.</span></span>

<span data-ttu-id="98d9e-266">**Serveur**</span><span class="sxs-lookup"><span data-stu-id="98d9e-266">**Server**</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample23.cs?highlight=5)]

<span data-ttu-id="98d9e-267">**Client JavaScript à l’aide du proxy généré**</span><span class="sxs-lookup"><span data-stu-id="98d9e-267">**JavaScript client using generated proxy**</span></span>

[!code-html[Main](hubs-api-guide-server/samples/sample24.html?highlight=1)]

<span data-ttu-id="98d9e-268">Impossible d’obtenir une valeur de retour à partir d’une méthode du client ; la syntaxe `int x = Clients.All.add(1,1)` ne fonctionne pas.</span><span class="sxs-lookup"><span data-stu-id="98d9e-268">You can't get a return value from a client method; syntax such as `int x = Clients.All.add(1,1)` does not work.</span></span>

<span data-ttu-id="98d9e-269">Vous pouvez spécifier les types complexes et des tableaux pour les paramètres.</span><span class="sxs-lookup"><span data-stu-id="98d9e-269">You can specify complex types and arrays for the parameters.</span></span> <span data-ttu-id="98d9e-270">L’exemple suivant passe un type complexe au client dans un paramètre de méthode.</span><span class="sxs-lookup"><span data-stu-id="98d9e-270">The following example passes a complex type to the client in a method parameter.</span></span>

<span data-ttu-id="98d9e-271">**Code de serveur qui appelle une méthode du client à l’aide d’un objet complexe**</span><span class="sxs-lookup"><span data-stu-id="98d9e-271">**Server code that calls a client method using a complex object**</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample25.cs?highlight=3)]

<span data-ttu-id="98d9e-272">**Code de serveur qui définit l’objet complexe**</span><span class="sxs-lookup"><span data-stu-id="98d9e-272">**Server code that defines the complex object**</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample26.cs?highlight=1)]

<span data-ttu-id="98d9e-273">**Client JavaScript à l’aide du proxy généré**</span><span class="sxs-lookup"><span data-stu-id="98d9e-273">**JavaScript client using generated proxy**</span></span>

[!code-javascript[Main](hubs-api-guide-server/samples/sample27.js?highlight=2-3)]

<a id="selectingclients"></a>

### <a name="selecting-which-clients-will-receive-the-rpc"></a><span data-ttu-id="98d9e-274">En sélectionnant les clients qui reçoivent l’appel RPC</span><span class="sxs-lookup"><span data-stu-id="98d9e-274">Selecting which clients will receive the RPC</span></span>

<span data-ttu-id="98d9e-275">La propriété retourne de Clients un [HubConnectionContext](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.hubs.hubconnectioncontext(v=vs.111).aspx) objet qui fournit plusieurs options permettant de spécifier les clients qui recevront le RPC :</span><span class="sxs-lookup"><span data-stu-id="98d9e-275">The Clients property returns a [HubConnectionContext](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.hubs.hubconnectioncontext(v=vs.111).aspx) object that provides several options for specifying which clients will receive the RPC:</span></span>

- <span data-ttu-id="98d9e-276">Tous les clients connectés.</span><span class="sxs-lookup"><span data-stu-id="98d9e-276">All connected clients.</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample28.cs)]
- <span data-ttu-id="98d9e-277">Client appelant.</span><span class="sxs-lookup"><span data-stu-id="98d9e-277">Only the calling client.</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample29.cs)]
- <span data-ttu-id="98d9e-278">Tous les clients sauf des clients.</span><span class="sxs-lookup"><span data-stu-id="98d9e-278">All clients except the calling client.</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample30.cs)]
- <span data-ttu-id="98d9e-279">Un client spécifique identifié par l’ID de connexion.</span><span class="sxs-lookup"><span data-stu-id="98d9e-279">A specific client identified by connection ID.</span></span>

    [!code-css[Main](hubs-api-guide-server/samples/sample31.css)]

    <span data-ttu-id="98d9e-280">Cet exemple appelle `addContosoChatMessageToPage` sur le client appelant et a le même effet que l’utilisation de `Clients.Caller`.</span><span class="sxs-lookup"><span data-stu-id="98d9e-280">This example calls `addContosoChatMessageToPage` on the calling client and has the same effect as using `Clients.Caller`.</span></span>
- <span data-ttu-id="98d9e-281">Tous les clients connectés à l’exception des clients spécifiés, identifiés par l’ID de connexion.</span><span class="sxs-lookup"><span data-stu-id="98d9e-281">All connected clients except the specified clients, identified by connection ID.</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample32.cs)]
- <span data-ttu-id="98d9e-282">Tous les clients connectés dans un groupe spécifié.</span><span class="sxs-lookup"><span data-stu-id="98d9e-282">All connected clients in a specified group.</span></span>

    [!code-css[Main](hubs-api-guide-server/samples/sample33.css)]
- <span data-ttu-id="98d9e-283">Tous les clients connectés dans un groupe spécifié, à l’exception des clients spécifiés, identifié par l’ID de connexion.</span><span class="sxs-lookup"><span data-stu-id="98d9e-283">All connected clients in a specified group except the specified clients, identified by connection ID.</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample34.cs)]
- <span data-ttu-id="98d9e-284">Tous les clients connectés dans un groupe spécifié à l’exception du client appelant.</span><span class="sxs-lookup"><span data-stu-id="98d9e-284">All connected clients in a specified group except the calling client.</span></span>

    [!code-css[Main](hubs-api-guide-server/samples/sample35.css)]
- <span data-ttu-id="98d9e-285">Un utilisateur spécifique, identifié par l’ID d’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="98d9e-285">A specific user, identified by userId.</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample36.cs)]

    <span data-ttu-id="98d9e-286">Par défaut, il s’agit de `IPrincipal.Identity.Name`, mais cela peut être modifié par [l’inscription d’une implémentation de IUserIdProvider avec l’hôte global](mapping-users-to-connections.md#IUserIdProvider).</span><span class="sxs-lookup"><span data-stu-id="98d9e-286">By default, this is `IPrincipal.Identity.Name`, but this can be changed by [registering an implementation of IUserIdProvider with the global host](mapping-users-to-connections.md#IUserIdProvider).</span></span>
- <span data-ttu-id="98d9e-287">Tous les clients et les groupes dans une liste des ID de connexion.</span><span class="sxs-lookup"><span data-stu-id="98d9e-287">All clients and groups in a list of connection IDs.</span></span>

    [!code-css[Main](hubs-api-guide-server/samples/sample37.css)]
- <span data-ttu-id="98d9e-288">Une liste de groupes.</span><span class="sxs-lookup"><span data-stu-id="98d9e-288">A list of groups.</span></span>

    [!code-css[Main](hubs-api-guide-server/samples/sample38.css)]
- <span data-ttu-id="98d9e-289">Un utilisateur par nom.</span><span class="sxs-lookup"><span data-stu-id="98d9e-289">A user by name.</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample39.cs)]
- <span data-ttu-id="98d9e-290">Liste des noms d’utilisateur (introduite dans SignalR 2.1).</span><span class="sxs-lookup"><span data-stu-id="98d9e-290">A list of user names (introduced in SignalR 2.1).</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample40.cs)]

<a id="dynamicmethodnames"></a>

### <a name="no-compile-time-validation-for-method-names"></a><span data-ttu-id="98d9e-291">Aucune validation de la compilation pour les noms de méthode</span><span class="sxs-lookup"><span data-stu-id="98d9e-291">No compile-time validation for method names</span></span>

<span data-ttu-id="98d9e-292">Le nom de la méthode que vous spécifiez est interprété comme un objet dynamique, ce qui signifie qu'aucune IntelliSense ou la validation lors de la compilation.</span><span class="sxs-lookup"><span data-stu-id="98d9e-292">The method name that you specify is interpreted as a dynamic object, which means there is no IntelliSense or compile-time validation for it.</span></span> <span data-ttu-id="98d9e-293">L’expression est évaluée au moment de l’exécution.</span><span class="sxs-lookup"><span data-stu-id="98d9e-293">The expression is evaluated at run time.</span></span> <span data-ttu-id="98d9e-294">Lorsque l’appel de méthode s’exécute, SignalR envoie le nom de méthode et les valeurs de paramètre au client, et si le client possède une méthode qui correspond au nom que la méthode est appelée et les valeurs de paramètre sont passés à ce dernier.</span><span class="sxs-lookup"><span data-stu-id="98d9e-294">When the method call executes, SignalR sends the method name and the parameter values to the client, and if the client has a method that matches the name, that method is called and the parameter values are passed to it.</span></span> <span data-ttu-id="98d9e-295">Si aucune méthode correspondante n’est trouvée sur le client, aucune erreur n’est déclenché.</span><span class="sxs-lookup"><span data-stu-id="98d9e-295">If no matching method is found on the client, no error is raised.</span></span> <span data-ttu-id="98d9e-296">Pour plus d’informations sur le format des données SignalR transmet au client en arrière-plan lorsque vous appelez une méthode du client, consultez [Introduction à SignalR](../getting-started/introduction-to-signalr.md).</span><span class="sxs-lookup"><span data-stu-id="98d9e-296">For information about the format of the data that SignalR transmits to the client behind the scenes when you call a client method, see [Introduction to SignalR](../getting-started/introduction-to-signalr.md).</span></span>

<a id="caseinsensitive"></a>

### <a name="case-insensitive-method-name-matching"></a><span data-ttu-id="98d9e-297">Une correspondance de nom sans respecter la casse (méthode)</span><span class="sxs-lookup"><span data-stu-id="98d9e-297">Case-insensitive method name matching</span></span>

<span data-ttu-id="98d9e-298">Correspondance de nom de méthode respecte la casse.</span><span class="sxs-lookup"><span data-stu-id="98d9e-298">Method name matching is case-insensitive.</span></span> <span data-ttu-id="98d9e-299">Par exemple, `Clients.All.addContosoChatMessageToPage` sur le serveur s’exécute `AddContosoChatMessageToPage`, `addcontosochatmessagetopage`, ou `addContosoChatMessageToPage` sur le client.</span><span class="sxs-lookup"><span data-stu-id="98d9e-299">For example, `Clients.All.addContosoChatMessageToPage` on the server will execute `AddContosoChatMessageToPage`, `addcontosochatmessagetopage`, or `addContosoChatMessageToPage` on the client.</span></span>

<a id="asyncclient"></a>

### <a name="asynchronous-execution"></a><span data-ttu-id="98d9e-300">Exécution asynchrone</span><span class="sxs-lookup"><span data-stu-id="98d9e-300">Asynchronous execution</span></span>

<span data-ttu-id="98d9e-301">La méthode que vous appelez s’exécute de façon asynchrone.</span><span class="sxs-lookup"><span data-stu-id="98d9e-301">The method that you call executes asynchronously.</span></span> <span data-ttu-id="98d9e-302">Tout code qui vient après qu’un appel de méthode à un client s’exécute immédiatement sans attendre que SignalR terminer la transmission de données aux clients, sauf si vous spécifiez que les lignes suivantes du code doivent attendre pour l’exécution de la méthode.</span><span class="sxs-lookup"><span data-stu-id="98d9e-302">Any code that comes after a method call to a client will execute immediately without waiting for SignalR to finish transmitting data to clients unless you specify that the subsequent lines of code should wait for method completion.</span></span> <span data-ttu-id="98d9e-303">L’exemple de code suivant montre comment exécuter deux méthodes de client de façon séquentielle.</span><span class="sxs-lookup"><span data-stu-id="98d9e-303">The following code example shows how to execute two client methods sequentially.</span></span>

<span data-ttu-id="98d9e-304">**À l’aide d’Await (.NET 4.5)**</span><span class="sxs-lookup"><span data-stu-id="98d9e-304">**Using Await (.NET 4.5)**</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample41.cs?highlight=1,3)]

<span data-ttu-id="98d9e-305">Si vous utilisez `await` pour attendre qu’une méthode de client se termine avant l’exécution de la ligne de code suivante, qui ne signifie pas que que les clients réellement le message avant l’exécution de la ligne de code suivante.</span><span class="sxs-lookup"><span data-stu-id="98d9e-305">If you use `await` to wait until a client method finishes before the next line of code executes, that does not mean that clients will actually receive the message before the next line of code executes.</span></span> <span data-ttu-id="98d9e-306">« Exécution » d’un appel de méthode client signifie uniquement que SignalR a effectué toutes les opérations nécessaires pour envoyer le message.</span><span class="sxs-lookup"><span data-stu-id="98d9e-306">"Completion" of a client method call only means that SignalR has done everything necessary to send the message.</span></span> <span data-ttu-id="98d9e-307">Si vous avez besoin que les clients ont reçu le message de vérification, vous devez programmer ce mécanisme.</span><span class="sxs-lookup"><span data-stu-id="98d9e-307">If you need verification that clients received the message, you have to program that mechanism yourself.</span></span> <span data-ttu-id="98d9e-308">Par exemple, vous pourriez coder un `MessageReceived` méthode du concentrateur et dans le `addContosoChatMessageToPage` méthode sur le client, vous pouvez appeler `MessageReceived` après l’installation quelle que soit opérationnelle, vous devez effectuer sur le client.</span><span class="sxs-lookup"><span data-stu-id="98d9e-308">For example, you could code a `MessageReceived` method on the Hub, and in the `addContosoChatMessageToPage` method on the client you could call `MessageReceived` after you do whatever work you need to do on the client.</span></span> <span data-ttu-id="98d9e-309">Dans `MessageReceived` dans le concentrateur, vous pouvez effectuer le travail dépend de réception du client réel et le traitement de l’appel de méthode d’origine.</span><span class="sxs-lookup"><span data-stu-id="98d9e-309">In `MessageReceived` in the Hub you can do whatever work depends on actual client reception and processing of the original method call.</span></span>

### <a name="how-to-use-a-string-variable-as-the-method-name"></a><span data-ttu-id="98d9e-310">Comment utiliser une variable de chaîne en tant que nom de la méthode</span><span class="sxs-lookup"><span data-stu-id="98d9e-310">How to use a string variable as the method name</span></span>

<span data-ttu-id="98d9e-311">Si vous souhaitez appeler une méthode du client à l’aide d’une variable de chaîne en tant que nom de la méthode, casté `Clients.All` (ou `Clients.Others`, `Clients.Caller`, etc.) à `IClientProxy` , puis appelez [Invoke (methodName, args...) ](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.hubs.iclientproxy.invoke(v=vs.111).aspx).</span><span class="sxs-lookup"><span data-stu-id="98d9e-311">If you want to invoke a client method by using a string variable as the method name, cast `Clients.All` (or `Clients.Others`, `Clients.Caller`, etc.) to `IClientProxy` and then call [Invoke(methodName, args...)](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.hubs.iclientproxy.invoke(v=vs.111).aspx).</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample42.cs)]

<a id="groupsfromhub"></a>

## <a name="how-to-manage-group-membership-from-the-hub-class"></a><span data-ttu-id="98d9e-312">Comment gérer l’appartenance au groupe à partir de la classe de concentrateur</span><span class="sxs-lookup"><span data-stu-id="98d9e-312">How to manage group membership from the Hub class</span></span>

<span data-ttu-id="98d9e-313">Groupes dans SignalR fournissent une méthode pour diffuser des messages à des sous-ensembles spécifiés de clients connectés.</span><span class="sxs-lookup"><span data-stu-id="98d9e-313">Groups in SignalR provide a method for broadcasting messages to specified subsets of connected clients.</span></span> <span data-ttu-id="98d9e-314">Un groupe peut avoir n’importe quel nombre de clients, et un client peut être un membre de n’importe quel nombre de groupes.</span><span class="sxs-lookup"><span data-stu-id="98d9e-314">A group can have any number of clients, and a client can be a member of any number of groups.</span></span>

<span data-ttu-id="98d9e-315">Pour gérer l’appartenance au groupe, utilisez le [ajouter](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.igroupmanager.add(v=vs.111).aspx) et [supprimer](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.igroupmanager.remove(v=vs.111).aspx) méthodes fournies par le `Groups` propriété de la classe de concentrateur.</span><span class="sxs-lookup"><span data-stu-id="98d9e-315">To manage group membership, use the [Add](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.igroupmanager.add(v=vs.111).aspx) and [Remove](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.igroupmanager.remove(v=vs.111).aspx) methods provided by the `Groups` property of the Hub class.</span></span> <span data-ttu-id="98d9e-316">L’exemple suivant illustre la `Groups.Add` et `Groups.Remove` méthodes utilisées dans les méthodes de concentrateur qui sont appelées par le code client, suivi par le code JavaScript client qui les appelle.</span><span class="sxs-lookup"><span data-stu-id="98d9e-316">The following example shows the `Groups.Add` and `Groups.Remove` methods used in Hub methods that are called by client code, followed by JavaScript client code that calls them.</span></span>

<span data-ttu-id="98d9e-317">**Serveur**</span><span class="sxs-lookup"><span data-stu-id="98d9e-317">**Server**</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample43.cs?highlight=5,10)]

<span data-ttu-id="98d9e-318">**Client JavaScript à l’aide du proxy généré**</span><span class="sxs-lookup"><span data-stu-id="98d9e-318">**JavaScript client using generated proxy**</span></span>

[!code-javascript[Main](hubs-api-guide-server/samples/sample44.js)]

[!code-javascript[Main](hubs-api-guide-server/samples/sample45.js)]

<span data-ttu-id="98d9e-319">Vous n’êtes pas obligé de créer explicitement des groupes.</span><span class="sxs-lookup"><span data-stu-id="98d9e-319">You don't have to explicitly create groups.</span></span> <span data-ttu-id="98d9e-320">En effet un groupe est créé automatiquement la première fois que vous spécifiez son nom dans un appel à `Groups.Add`, et il est supprimé lorsque vous supprimez la dernière connexion de l’appartenance à elle.</span><span class="sxs-lookup"><span data-stu-id="98d9e-320">In effect a group is automatically created the first time you specify its name in a call to `Groups.Add`, and it is deleted when you remove the last connection from membership in it.</span></span>

<span data-ttu-id="98d9e-321">Il n’existe aucune API pour obtenir une liste de l’appartenance au groupe ou une liste de groupes.</span><span class="sxs-lookup"><span data-stu-id="98d9e-321">There is no API for getting a group membership list or a list of groups.</span></span> <span data-ttu-id="98d9e-322">SignalR envoie des messages pour les clients et les groupes basés sur un [modèle de pub/sub](http://en.wikipedia.org/wiki/Publish/subscribe), et le serveur ne conserve pas les listes de groupes ou des appartenances aux groupes.</span><span class="sxs-lookup"><span data-stu-id="98d9e-322">SignalR sends messages to clients and groups based on a [pub/sub model](http://en.wikipedia.org/wiki/Publish/subscribe), and the server does not maintain lists of groups or group memberships.</span></span> <span data-ttu-id="98d9e-323">Cela permet d’optimiser l’évolutivité, car chaque fois que vous ajoutez un nœud à une batterie de serveurs web, tout état SignalR tient à jour doit être propagée vers le nouveau nœud.</span><span class="sxs-lookup"><span data-stu-id="98d9e-323">This helps maximize scalability, because whenever you add a node to a web farm, any state that SignalR maintains has to be propagated to the new node.</span></span>

<a id="asyncgroupmethods"></a>

### <a name="asynchronous-execution-of-add-and-remove-methods"></a><span data-ttu-id="98d9e-324">Exécution asynchrone de méthodes Add et Remove</span><span class="sxs-lookup"><span data-stu-id="98d9e-324">Asynchronous execution of Add and Remove methods</span></span>

<span data-ttu-id="98d9e-325">Le `Groups.Add` et `Groups.Remove` méthodes exécutent de façon asynchrone.</span><span class="sxs-lookup"><span data-stu-id="98d9e-325">The `Groups.Add` and `Groups.Remove` methods execute asynchronously.</span></span> <span data-ttu-id="98d9e-326">Si vous souhaitez ajouter un client à un groupe et d’envoyer immédiatement un message au client en utilisant le groupe, vous devez vous assurer que le `Groups.Add` méthode se termine en premier.</span><span class="sxs-lookup"><span data-stu-id="98d9e-326">If you want to add a client to a group and immediately send a message to the client by using the group, you have to make sure that the `Groups.Add` method finishes first.</span></span> <span data-ttu-id="98d9e-327">L’exemple de code suivant montre comment procéder.</span><span class="sxs-lookup"><span data-stu-id="98d9e-327">The following code example shows how to do that.</span></span>

<span data-ttu-id="98d9e-328">**Ajout d’un client à un groupe et que le client de messagerie puis**</span><span class="sxs-lookup"><span data-stu-id="98d9e-328">**Adding a client to a group and then messaging that client**</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample46.cs?highlight=1,3)]

<a id="grouppersistence"></a>

### <a name="group-membership-persistence"></a><span data-ttu-id="98d9e-329">Persistance de l’appartenance au groupe</span><span class="sxs-lookup"><span data-stu-id="98d9e-329">Group membership persistence</span></span>

<span data-ttu-id="98d9e-330">SignalR effectue le suivi des connexions, pas les utilisateurs, par conséquent, si vous souhaitez qu’un utilisateur soit dans le même groupe chaque fois que l’utilisateur établit une connexion, vous devez appeler `Groups.Add` chaque fois que l’utilisateur établit une nouvelle connexion.</span><span class="sxs-lookup"><span data-stu-id="98d9e-330">SignalR tracks connections, not users, so if you want a user to be in the same group every time the user establishes a connection, you have to call `Groups.Add` every time the user establishes a new connection.</span></span>

<span data-ttu-id="98d9e-331">Après une perte de connectivité temporaire, parfois SignalR peut restaurer la connexion automatiquement.</span><span class="sxs-lookup"><span data-stu-id="98d9e-331">After a temporary loss of connectivity, sometimes SignalR can restore the connection automatically.</span></span> <span data-ttu-id="98d9e-332">Dans ce cas, SignalR restaure la même connexion, ne pas l’établissement d’une nouvelle connexion, et par conséquent, les appartenances de groupe du client est automatiquement restauré.</span><span class="sxs-lookup"><span data-stu-id="98d9e-332">In that case, SignalR is restoring the same connection, not establishing a new connection, and so the client's group membership is automatically restored.</span></span> <span data-ttu-id="98d9e-333">Cela est possible même lorsque l’arrêt temporaire est le résultat d’un redémarrage du serveur ou l’échec, car l’état de connexion pour chaque client, y compris les appartenances de groupe, est un aller-retour vers le client.</span><span class="sxs-lookup"><span data-stu-id="98d9e-333">This is possible even when the temporary break is the result of a server reboot or failure, because connection state for each client, including group memberships, is round-tripped to the client.</span></span> <span data-ttu-id="98d9e-334">Si un serveur tombe en panne et est remplacé par un nouveau serveur avant l’expiration de la connexion, un client peut se reconnecter automatiquement au nouveau serveur et réinscrire dans des groupes, de qu'il est membre.</span><span class="sxs-lookup"><span data-stu-id="98d9e-334">If a server goes down and is replaced by a new server before the connection times out, a client can reconnect automatically to the new server and re-enroll in groups it is a member of.</span></span>

<span data-ttu-id="98d9e-335">Lorsqu’une connexion ne peut pas être restaurée automatiquement après une perte de connectivité, ou lorsque la connexion arrive à expiration, ou lorsque le client se déconnecte (par exemple, lorsqu’un navigateur accède à une nouvelle page), les appartenances aux groupes sont perdues.</span><span class="sxs-lookup"><span data-stu-id="98d9e-335">When a connection can't be restored automatically after a loss of connectivity, or when the connection times out, or when the client disconnects (for example, when a browser navigates to a new page), group memberships are lost.</span></span> <span data-ttu-id="98d9e-336">La prochaine fois que l’utilisateur se connecte sera une nouvelle connexion.</span><span class="sxs-lookup"><span data-stu-id="98d9e-336">The next time the user connects will be a new connection.</span></span> <span data-ttu-id="98d9e-337">Pour gérer l’appartenance au groupe lorsque le même utilisateur établit une nouvelle connexion, votre application doit suivre les associations entre les utilisateurs et groupes et restaurer les appartenances aux groupes chaque fois qu’un utilisateur établit une nouvelle connexion.</span><span class="sxs-lookup"><span data-stu-id="98d9e-337">To maintain group memberships when the same user establishes a new connection, your application has to track the associations between users and groups, and restore group memberships each time a user establishes a new connection.</span></span>

<span data-ttu-id="98d9e-338">Pour plus d’informations sur les connexions et les reconnexions, consultez [comment gérer les événements de durée de vie de connexion dans la classe de concentrateur](#connectionlifetime) plus loin dans cette rubrique.</span><span class="sxs-lookup"><span data-stu-id="98d9e-338">For more information about connections and reconnections, see [How to handle connection lifetime events in the Hub class](#connectionlifetime) later in this topic.</span></span>

<a id="singleusergroups"></a>

### <a name="single-user-groups"></a><span data-ttu-id="98d9e-339">Groupes d’utilisateur unique</span><span class="sxs-lookup"><span data-stu-id="98d9e-339">Single-user groups</span></span>

<span data-ttu-id="98d9e-340">Applications qui utilisent généralement des SignalR doivent effectuer le suivi des associations entre les utilisateurs et connexions afin de déterminer l’utilisateur qui a envoyé un message et les utilisateurs doivent recevoir un message.</span><span class="sxs-lookup"><span data-stu-id="98d9e-340">Applications that use SignalR typically have to keep track of the associations between users and connections in order to know which user has sent a message and which user(s) should be receiving a message.</span></span> <span data-ttu-id="98d9e-341">Pour cela, les groupes sont utilisés dans un des deux modèles couramment utilisés.</span><span class="sxs-lookup"><span data-stu-id="98d9e-341">Groups are used in one of the two commonly used patterns for doing that.</span></span>

- <span data-ttu-id="98d9e-342">Groupes d’utilisateur unique.</span><span class="sxs-lookup"><span data-stu-id="98d9e-342">Single-user groups.</span></span>

    <span data-ttu-id="98d9e-343">Vous pouvez spécifier le nom d’utilisateur en tant que le nom du groupe et ajoutez l’ID de connexion actuel au groupe chaque fois que l’utilisateur se connecte ou se reconnecte.</span><span class="sxs-lookup"><span data-stu-id="98d9e-343">You can specify the user name as the group name, and add the current connection ID to the group every time the user connects or reconnects.</span></span> <span data-ttu-id="98d9e-344">Pour envoyer des messages à l’utilisateur que vous envoyez au groupe.</span><span class="sxs-lookup"><span data-stu-id="98d9e-344">To send messages to the user you send to the group.</span></span> <span data-ttu-id="98d9e-345">L’inconvénient de cette méthode est que le groupe ne vous fournit un moyen de savoir si l’utilisateur est en ligne ou hors connexion.</span><span class="sxs-lookup"><span data-stu-id="98d9e-345">A disadvantage of this method is that the group doesn't provide you with a way to find out if the user is online or offline.</span></span>
- <span data-ttu-id="98d9e-346">Effectuer le suivi des associations entre les noms d’utilisateur et ID de connexion.</span><span class="sxs-lookup"><span data-stu-id="98d9e-346">Track associations between user names and connection IDs.</span></span>

    <span data-ttu-id="98d9e-347">Vous pouvez stocker une association entre chaque nom d’utilisateur et un ou plusieurs ID de connexion dans un dictionnaire ou d’une base de données et mettre à jour les données stockées chaque fois que l’utilisateur se connecte ou se déconnecte.</span><span class="sxs-lookup"><span data-stu-id="98d9e-347">You can store an association between each user name and one or more connection IDs in a dictionary or database, and update the stored data each time the user connects or disconnects.</span></span> <span data-ttu-id="98d9e-348">Pour envoyer des messages à l’utilisateur, vous spécifiez l’ID de connexion.</span><span class="sxs-lookup"><span data-stu-id="98d9e-348">To send messages to the user you specify the connection IDs.</span></span> <span data-ttu-id="98d9e-349">L’inconvénient de cette méthode est qu’il faut plus de mémoire.</span><span class="sxs-lookup"><span data-stu-id="98d9e-349">A disadvantage of this method is that it takes more memory.</span></span>

<a id="connectionlifetime"></a>

## <a name="how-to-handle-connection-lifetime-events-in-the-hub-class"></a><span data-ttu-id="98d9e-350">Comment gérer les événements de durée de vie de connexion dans la classe de concentrateur</span><span class="sxs-lookup"><span data-stu-id="98d9e-350">How to handle connection lifetime events in the Hub class</span></span>

<span data-ttu-id="98d9e-351">Les raisons courantes pour la gestion des événements de durée de vie de connexion sont de savoir si un utilisateur est connecté ou non et pour effectuer le suivi de l’association entre les noms d’utilisateur et ID de connexion.</span><span class="sxs-lookup"><span data-stu-id="98d9e-351">Typical reasons for handling connection lifetime events are to keep track of whether a user is connected or not, and to keep track of the association between user names and connection IDs.</span></span> <span data-ttu-id="98d9e-352">Pour exécuter votre propre code lorsque les clients se connecteront ou déconnectent, remplacer le `OnConnected`, `OnDisconnected`, et `OnReconnected` méthodes virtuelles du concentrateur de classe, comme indiqué dans l’exemple suivant.</span><span class="sxs-lookup"><span data-stu-id="98d9e-352">To run your own code when clients connect or disconnect, override the `OnConnected`, `OnDisconnected`, and `OnReconnected` virtual methods of the Hub class, as shown in the following example.</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample47.cs?highlight=3,14,22)]

<a id="onreconnected"></a>

### <a name="when-onconnected-ondisconnected-and-onreconnected-are-called"></a><span data-ttu-id="98d9e-353">Lorsque les OnConnected, OnDisconnected et OnReconnected sont appelées</span><span class="sxs-lookup"><span data-stu-id="98d9e-353">When OnConnected, OnDisconnected, and OnReconnected are called</span></span>

<span data-ttu-id="98d9e-354">Chaque fois qu’un navigateur accède à une nouvelle page, une nouvelle connexion doit être établie, ce qui signifie que SignalR exécutera la `OnDisconnected` méthode suivie par le `OnConnected` (méthode).</span><span class="sxs-lookup"><span data-stu-id="98d9e-354">Each time a browser navigates to a new page, a new connection has to be established, which means SignalR will execute the `OnDisconnected` method followed by the `OnConnected` method.</span></span> <span data-ttu-id="98d9e-355">SignalR crée toujours un nouvel ID de connexion lorsqu’une nouvelle connexion est établie.</span><span class="sxs-lookup"><span data-stu-id="98d9e-355">SignalR always creates a new connection ID when a new connection is established.</span></span>

<span data-ttu-id="98d9e-356">Le `OnReconnected` méthode est appelée en cas d’un arrêt temporaire de connectivité SignalR peut récupérer automatiquement à partir, par exemple lorsqu’un câble est temporairement déconnecté et reconnecté avant l’expiration de la connexion. Le `OnDisconnected` méthode est appelée lorsque le client est déconnecté et SignalR ne peut pas se reconnecter automatiquement, par exemple quand un navigateur accède à une nouvelle page.</span><span class="sxs-lookup"><span data-stu-id="98d9e-356">The `OnReconnected` method is called when there has been a temporary break in connectivity that SignalR can automatically recover from, such as when a cable is temporarily disconnected and reconnected before the connection times out. The `OnDisconnected` method is called when the client is disconnected and SignalR can't automatically reconnect, such as when a browser navigates to a new page.</span></span> <span data-ttu-id="98d9e-357">Par conséquent, une séquence possible d’événements pour un client donné est `OnConnected`, `OnReconnected`, `OnDisconnected`; ou `OnConnected`, `OnDisconnected`.</span><span class="sxs-lookup"><span data-stu-id="98d9e-357">Therefore, a possible sequence of events for a given client is `OnConnected`, `OnReconnected`, `OnDisconnected`; or `OnConnected`, `OnDisconnected`.</span></span> <span data-ttu-id="98d9e-358">Vous ne voyez pas la séquence `OnConnected`, `OnDisconnected`, `OnReconnected` pour une connexion donnée.</span><span class="sxs-lookup"><span data-stu-id="98d9e-358">You won't see the sequence `OnConnected`, `OnDisconnected`, `OnReconnected` for a given connection.</span></span>

<span data-ttu-id="98d9e-359">Le `OnDisconnected` méthode n’est pas appelée dans certains scénarios, tels que lorsqu’un serveur tombe en panne ou le domaine d’application obtient recyclé.</span><span class="sxs-lookup"><span data-stu-id="98d9e-359">The `OnDisconnected` method doesn't get called in some scenarios, such as when a server goes down or the App Domain gets recycled.</span></span> <span data-ttu-id="98d9e-360">Lorsqu’un autre serveur est en ligne ou le domaine d’application se termine son recyclage, certains clients peuvent être en mesure de se reconnecter et de déclencher la `OnReconnected` événement.</span><span class="sxs-lookup"><span data-stu-id="98d9e-360">When another server comes on line or the App Domain completes its recycle, some clients may be able to reconnect and fire the `OnReconnected` event.</span></span>

<span data-ttu-id="98d9e-361">Pour plus d’informations, consultez [compréhension et gestion des événements de durée de vie de connexion dans SignalR](handling-connection-lifetime-events.md).</span><span class="sxs-lookup"><span data-stu-id="98d9e-361">For more information, see [Understanding and Handling Connection Lifetime Events in SignalR](handling-connection-lifetime-events.md).</span></span>

<a id="nocallerstate"></a>

### <a name="caller-state-not-populated"></a><span data-ttu-id="98d9e-362">État de l’appelant ne pas remplie</span><span class="sxs-lookup"><span data-stu-id="98d9e-362">Caller state not populated</span></span>

<span data-ttu-id="98d9e-363">Les méthodes de gestionnaire de connexions durée de vie des événements sont appelés à partir du serveur, ce qui signifie que les États que vous placez dans le `state` objet sur le client ne sera pas renseigné dans le `Caller` propriété sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="98d9e-363">The connection lifetime event handler methods are called from the server, which means that any state that you put in the `state` object on the client will not be populated in the `Caller` property on the server.</span></span> <span data-ttu-id="98d9e-364">Pour plus d’informations sur la `state` objet et la `Caller` propriété, consultez [comment passer l’état entre les clients et de la classe de concentrateur](#passstate) plus loin dans cette rubrique.</span><span class="sxs-lookup"><span data-stu-id="98d9e-364">For information about the `state` object and the `Caller` property, see [How to pass state between clients and the Hub class](#passstate) later in this topic.</span></span>

<a id="contextproperty"></a>

## <a name="how-to-get-information-about-the-client-from-the-context-property"></a><span data-ttu-id="98d9e-365">Comment obtenir des informations sur le client à partir de la propriété de contexte</span><span class="sxs-lookup"><span data-stu-id="98d9e-365">How to get information about the client from the Context property</span></span>

<span data-ttu-id="98d9e-366">Pour obtenir plus d’informations sur le client, utilisez le `Context` propriété de la classe de concentrateur.</span><span class="sxs-lookup"><span data-stu-id="98d9e-366">To get information about the client, use the `Context` property of the Hub class.</span></span> <span data-ttu-id="98d9e-367">Le `Context` propriété retourne un [HubCallerContext](https://msdn.microsoft.com/en-us/library/jj890883(v=vs.111).aspx) objet qui fournit l’accès aux informations suivantes :</span><span class="sxs-lookup"><span data-stu-id="98d9e-367">The `Context` property returns a [HubCallerContext](https://msdn.microsoft.com/en-us/library/jj890883(v=vs.111).aspx) object which provides access to the following information:</span></span>

- <span data-ttu-id="98d9e-368">L’ID de connexion du client appelant.</span><span class="sxs-lookup"><span data-stu-id="98d9e-368">The connection ID of the calling client.</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample48.cs?highlight=1)]

    <span data-ttu-id="98d9e-369">L’ID de connexion est un GUID qui est affecté par SignalR (vous ne pouvez pas spécifier la valeur dans votre propre code).</span><span class="sxs-lookup"><span data-stu-id="98d9e-369">The connection ID is a GUID that is assigned by SignalR (you can't specify the value in your own code).</span></span> <span data-ttu-id="98d9e-370">Il existe un ID de connexion pour chaque connexion et le même QU'ID est utilisé par tous les concentrateurs si vous avez plusieurs concentrateurs dans votre application.</span><span class="sxs-lookup"><span data-stu-id="98d9e-370">There is one connection ID for each connection, and the same connection ID is used by all Hubs if you have multiple Hubs in your application.</span></span>
- <span data-ttu-id="98d9e-371">Données d’en-tête HTTP.</span><span class="sxs-lookup"><span data-stu-id="98d9e-371">HTTP header data.</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample49.cs?highlight=1)]

    <span data-ttu-id="98d9e-372">Vous pouvez également obtenir des en-têtes HTTP à partir de `Context.Headers`.</span><span class="sxs-lookup"><span data-stu-id="98d9e-372">You can also get HTTP headers from `Context.Headers`.</span></span> <span data-ttu-id="98d9e-373">La raison de plusieurs références à la même chose est que `Context.Headers` a été créé en premier lieu, le `Context.Request` propriété a été ajoutée à une version ultérieure, et `Context.Headers` a été conservée pour compatibilité descendante.</span><span class="sxs-lookup"><span data-stu-id="98d9e-373">The reason for multiple references to the same thing is that `Context.Headers` was created first, the `Context.Request` property was added later, and `Context.Headers` was retained for backward compatibility.</span></span>
- <span data-ttu-id="98d9e-374">Données de chaîne de requête.</span><span class="sxs-lookup"><span data-stu-id="98d9e-374">Query string data.</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample50.cs?highlight=1)]

    <span data-ttu-id="98d9e-375">Vous pouvez également obtenir des données de chaîne de requête `Context.QueryString`.</span><span class="sxs-lookup"><span data-stu-id="98d9e-375">You can also get query string data from `Context.QueryString`.</span></span>

    <span data-ttu-id="98d9e-376">La chaîne de requête que vous obtenez dans cette propriété est celui qui a été utilisé avec la requête HTTP qui a établi la connexion de SignalR.</span><span class="sxs-lookup"><span data-stu-id="98d9e-376">The query string that you get in this property is the one that was used with the HTTP request that established the SignalR connection.</span></span> <span data-ttu-id="98d9e-377">Vous pouvez ajouter des paramètres de chaîne de requête dans le client en configurant la connexion, ce qui constitue un moyen pratique pour passer des données sur le client à partir du client au serveur.</span><span class="sxs-lookup"><span data-stu-id="98d9e-377">You can add query string parameters in the client by configuring the connection, which is a convenient way to pass data about the client from the client to the server.</span></span> <span data-ttu-id="98d9e-378">L’exemple suivant montre une façon d’ajouter une chaîne de requête dans un client JavaScript lorsque vous utilisez le proxy généré.</span><span class="sxs-lookup"><span data-stu-id="98d9e-378">The following example shows one way to add a query string in a JavaScript client when you use the generated proxy.</span></span>

    [!code-javascript[Main](hubs-api-guide-server/samples/sample51.js?highlight=1)]

    <span data-ttu-id="98d9e-379">Pour plus d’informations sur la définition des paramètres de chaîne de requête, consultez les guides d’API pour le [JavaScript](hubs-api-guide-javascript-client.md) et [.NET](hubs-api-guide-net-client.md) les clients.</span><span class="sxs-lookup"><span data-stu-id="98d9e-379">For more information about setting query string parameters, see the API guides for the [JavaScript](hubs-api-guide-javascript-client.md) and [.NET](hubs-api-guide-net-client.md) clients.</span></span>

    <span data-ttu-id="98d9e-380">Vous pouvez trouver le mode de transport utilisé pour la connexion dans les données de chaîne de requête, ainsi que certaines autres valeurs utilisées en interne par SignalR :</span><span class="sxs-lookup"><span data-stu-id="98d9e-380">You can find the transport method used for the connection in the query string data, along with some other values used internally by SignalR:</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample52.cs)]

    <span data-ttu-id="98d9e-381">La valeur de `transportMethod` sera « webSockets », « serverSentEvents », « foreverFrame » ou « longPolling ».</span><span class="sxs-lookup"><span data-stu-id="98d9e-381">The value of `transportMethod` will be "webSockets", "serverSentEvents", "foreverFrame" or "longPolling".</span></span> <span data-ttu-id="98d9e-382">Notez que si vous vérifiez cette valeur la `OnConnected` méthode de gestionnaire d’événements, dans certains scénarios, vous pouvez initialement obtenir une valeur de transport qui n’est pas la méthode de transport négociée final pour la connexion.</span><span class="sxs-lookup"><span data-stu-id="98d9e-382">Note that if you check this value in the `OnConnected` event handler method, in some scenarios you might initially get a transport value that is not the final negotiated transport method for the connection.</span></span> <span data-ttu-id="98d9e-383">Dans ce cas, la méthode lève une exception et est appelée plus tard lorsque le mode de transport finale est établi.</span><span class="sxs-lookup"><span data-stu-id="98d9e-383">In that case the method will throw an exception and will be called again later when the final transport method is established.</span></span>
- <span data-ttu-id="98d9e-384">Cookies.</span><span class="sxs-lookup"><span data-stu-id="98d9e-384">Cookies.</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample53.cs?highlight=1)]

    <span data-ttu-id="98d9e-385">Vous pouvez également obtenir les cookies à partir de `Context.RequestCookies`.</span><span class="sxs-lookup"><span data-stu-id="98d9e-385">You can also get cookies from `Context.RequestCookies`.</span></span>
- <span data-ttu-id="98d9e-386">Informations sur l’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="98d9e-386">User information.</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample54.cs?highlight=1)]
- <span data-ttu-id="98d9e-387">L’objet HttpContext pour la demande :</span><span class="sxs-lookup"><span data-stu-id="98d9e-387">The HttpContext object for the request :</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample55.cs?highlight=1)]

    <span data-ttu-id="98d9e-388">Utilisez cette méthode au lieu d’obtenir `HttpContext.Current` pour obtenir le `HttpContext` objet pour la connexion de SignalR.</span><span class="sxs-lookup"><span data-stu-id="98d9e-388">Use this method instead of getting `HttpContext.Current` to get the `HttpContext` object for the SignalR connection.</span></span>

<a id="passstate"></a>

## <a name="how-to-pass-state-between-clients-and-the-hub-class"></a><span data-ttu-id="98d9e-389">Comment passer l’état entre les clients et de la classe de concentrateur</span><span class="sxs-lookup"><span data-stu-id="98d9e-389">How to pass state between clients and the Hub class</span></span>

<span data-ttu-id="98d9e-390">Le proxy client fournit un `state` objet dans lequel vous pouvez stocker les données que vous souhaitez transmettre au serveur avec chaque appel de méthode.</span><span class="sxs-lookup"><span data-stu-id="98d9e-390">The client proxy provides a `state` object in which you can store data that you want to be transmitted to the server with each method call.</span></span> <span data-ttu-id="98d9e-391">Sur le serveur, vous pouvez accéder à ces données dans le `Clients.Caller` propriété dans les méthodes de concentrateur qui sont appelées par les clients.</span><span class="sxs-lookup"><span data-stu-id="98d9e-391">On the server you can access this data in the `Clients.Caller` property in Hub methods that are called by clients.</span></span> <span data-ttu-id="98d9e-392">Le `Clients.Caller` propriété n’est pas remplie pour les méthodes du Gestionnaire d’événements connexion durée de vie `OnConnected`, `OnDisconnected`, et `OnReconnected`.</span><span class="sxs-lookup"><span data-stu-id="98d9e-392">The `Clients.Caller` property is not populated for the connection lifetime event handler methods `OnConnected`, `OnDisconnected`, and `OnReconnected`.</span></span>

<span data-ttu-id="98d9e-393">Création ou mise à jour des données dans le `state` objet et le `Clients.Caller` propriété fonctionne dans les deux sens.</span><span class="sxs-lookup"><span data-stu-id="98d9e-393">Creating or updating data in the `state` object and the `Clients.Caller` property works in both directions.</span></span> <span data-ttu-id="98d9e-394">Vous pouvez mettre à jour les valeurs dans le serveur et qu’ils sont transmis au client.</span><span class="sxs-lookup"><span data-stu-id="98d9e-394">You can update values in the server and they are passed back to the client.</span></span>

<span data-ttu-id="98d9e-395">L’exemple suivant montre le code JavaScript client qui stocke l’état de transmission au serveur à chaque appel de méthode.</span><span class="sxs-lookup"><span data-stu-id="98d9e-395">The following example shows JavaScript client code that stores state for transmission to the server with every method call.</span></span>

[!code-javascript[Main](hubs-api-guide-server/samples/sample56.js?highlight=1-2)]

<span data-ttu-id="98d9e-396">L’exemple suivant montre le code équivalent dans un client .NET.</span><span class="sxs-lookup"><span data-stu-id="98d9e-396">The following example shows the equivalent code in a .NET client.</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample57.cs?highlight=1-2)]

<span data-ttu-id="98d9e-397">Dans votre classe de concentrateur, vous pouvez accéder à ces données dans le `Clients.Caller` propriété.</span><span class="sxs-lookup"><span data-stu-id="98d9e-397">In your Hub class, you can access this data in the `Clients.Caller` property.</span></span> <span data-ttu-id="98d9e-398">L’exemple suivant montre le code qui Récupère l’état dans l’exemple précédent.</span><span class="sxs-lookup"><span data-stu-id="98d9e-398">The following example shows code that retrieves the state referred to in the previous example.</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample58.cs?highlight=3-4)]

> [!NOTE]
> <span data-ttu-id="98d9e-399">Ce mécanisme pour rendre persistant l’état n’est pas destiné pour de grandes quantités de données, depuis tout ce dont vous placer dans le `state` ou `Clients.Caller` propriété est un aller-retour avec chaque appel de méthode.</span><span class="sxs-lookup"><span data-stu-id="98d9e-399">This mechanism for persisting state is not intended for large amounts of data, since everything you put in the `state` or `Clients.Caller` property is round-tripped with every method invocation.</span></span> <span data-ttu-id="98d9e-400">Il est utile pour les éléments plus petits, tels que les noms d’utilisateur ou de compteurs.</span><span class="sxs-lookup"><span data-stu-id="98d9e-400">It's useful for smaller items such as user names or counters.</span></span>


<span data-ttu-id="98d9e-401">Dans VB.NET ou dans un concentrateur fortement typée, l’objet d’état de l’appelant ne peut pas être accessibles via `Clients.Caller`; au lieu de cela, utilisez `Clients.CallerState` (introduite dans SignalR 2.1) :</span><span class="sxs-lookup"><span data-stu-id="98d9e-401">In VB.NET or in a strongly-typed hub, the caller state object can't be accessed through `Clients.Caller`; instead, use `Clients.CallerState` (introduced in SignalR 2.1):</span></span>

<span data-ttu-id="98d9e-402">**À l’aide de CallerState en c#**</span><span class="sxs-lookup"><span data-stu-id="98d9e-402">**Using CallerState in C#**</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample59.cs?highlight=3-4)]

<span data-ttu-id="98d9e-403">**À l’aide de CallerState en Visual Basic**</span><span class="sxs-lookup"><span data-stu-id="98d9e-403">**Using CallerState in Visual Basic**</span></span>

[!code-vb[Main](hubs-api-guide-server/samples/sample60.vb)]

<a id="handleErrors"></a>

## <a name="how-to-handle-errors-in-the-hub-class"></a><span data-ttu-id="98d9e-404">Comment gérer les erreurs dans la classe de concentrateur</span><span class="sxs-lookup"><span data-stu-id="98d9e-404">How to handle errors in the Hub class</span></span>

<span data-ttu-id="98d9e-405">Pour gérer les erreurs qui se produisent dans vos méthodes de classe de concentrateur, utilisez une ou plusieurs des méthodes suivantes :</span><span class="sxs-lookup"><span data-stu-id="98d9e-405">To handle errors that occur in your Hub class methods, use one or more of the following methods:</span></span>

- <span data-ttu-id="98d9e-406">Encapsuler votre code de la méthode dans les blocs try-catch et les journaux de l’objet exception.</span><span class="sxs-lookup"><span data-stu-id="98d9e-406">Wrap your method code in try-catch blocks and log the exception object.</span></span> <span data-ttu-id="98d9e-407">À des fins de débogage, vous pouvez envoyer l’exception au client, mais pour la sécurité de raisons d’envoyer des informations détaillées sur les clients en production ne sont pas recommandées.</span><span class="sxs-lookup"><span data-stu-id="98d9e-407">For debugging purposes you can send the exception to the client, but for security reasons sending detailed information to clients in production is not recommended.</span></span>
- <span data-ttu-id="98d9e-408">Créer un module de pipeline concentrateurs qui gère la [OnIncomingError](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.hubs.hubpipelinemodule.onincomingerror(v=vs.111).aspx) (méthode).</span><span class="sxs-lookup"><span data-stu-id="98d9e-408">Create a Hubs pipeline module that handles the [OnIncomingError](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.hubs.hubpipelinemodule.onincomingerror(v=vs.111).aspx) method.</span></span> <span data-ttu-id="98d9e-409">L’exemple suivant montre un module de pipeline qui enregistre les erreurs, suivis par le code dans Startup.cs qui injecte le module dans le pipeline de concentrateurs.</span><span class="sxs-lookup"><span data-stu-id="98d9e-409">The following example shows a pipeline module that logs errors, followed by code in Startup.cs that injects the module into the Hubs pipeline.</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample61.cs)]

    [!code-csharp[Main](hubs-api-guide-server/samples/sample62.cs?highlight=4)]
- <span data-ttu-id="98d9e-410">Utilisez la `HubException` classe (introduite dans SignalR 2).</span><span class="sxs-lookup"><span data-stu-id="98d9e-410">Use the `HubException` class (introduced in SignalR 2).</span></span> <span data-ttu-id="98d9e-411">Cette erreur peut être levée à partir de n’importe quel appel de concentrateur.</span><span class="sxs-lookup"><span data-stu-id="98d9e-411">This error can be thrown from any hub invocation.</span></span> <span data-ttu-id="98d9e-412">Le `HubError` le constructeur prend un message de chaîne et un objet pour le stockage des données d’erreur supplémentaires.</span><span class="sxs-lookup"><span data-stu-id="98d9e-412">The `HubError` constructor takes a string message, and an object for storing extra error data.</span></span> <span data-ttu-id="98d9e-413">SignalR auto-sérialiser l’exception et l’envoyer au client, où il sera utilisé pour refuser ou de faire échouer l’appel de méthode de concentrateur.</span><span class="sxs-lookup"><span data-stu-id="98d9e-413">SignalR will auto-serialize the exception and send it to the client, where it will be used to reject or fail the hub method invocation.</span></span>

    <span data-ttu-id="98d9e-414">Les exemples de code suivants montrent comment lever une `HubException` pendant un appel de concentrateur et comment gérer l’exception sur les clients JavaScript et .NET.</span><span class="sxs-lookup"><span data-stu-id="98d9e-414">The following code samples demonstrate how to throw a `HubException` during a Hub invocation, and how to handle the exception on JavaScript and .NET clients.</span></span>

    <span data-ttu-id="98d9e-415">**Code de serveur illustrant la classe HubException**</span><span class="sxs-lookup"><span data-stu-id="98d9e-415">**Server code demonstrating the HubException class**</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample63.cs)]

    <span data-ttu-id="98d9e-416">**Code JavaScript client qui montre la réponse à la levée d’une HubException dans un concentrateur**</span><span class="sxs-lookup"><span data-stu-id="98d9e-416">**JavaScript client code demonstrating response to throwing a HubException in a hub**</span></span>

    [!code-html[Main](hubs-api-guide-server/samples/sample64.html)]

    <span data-ttu-id="98d9e-417">**Code client .NET qui montre la réponse à la levée d’une HubException dans un concentrateur**</span><span class="sxs-lookup"><span data-stu-id="98d9e-417">**.NET client code demonstrating response to throwing a HubException in a hub**</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample65.cs)]

<span data-ttu-id="98d9e-418">Pour plus d’informations sur les modules du pipeline Hub, consultez [comment personnaliser le pipeline de concentrateurs](#hubpipeline) plus loin dans cette rubrique.</span><span class="sxs-lookup"><span data-stu-id="98d9e-418">For more information about Hub pipeline modules, see [How to customize the Hubs pipeline](#hubpipeline) later in this topic.</span></span>

<a id="tracing"></a>

## <a name="how-to-enable-tracing"></a><span data-ttu-id="98d9e-419">Comment activer le suivi</span><span class="sxs-lookup"><span data-stu-id="98d9e-419">How to enable tracing</span></span>

<span data-ttu-id="98d9e-420">Pour activer le suivi du côté serveur, ajouter un élément system.diagnostics à votre fichier Web.config, comme indiqué dans cet exemple :</span><span class="sxs-lookup"><span data-stu-id="98d9e-420">To enable server-side tracing, add a system.diagnostics element to your Web.config file, as shown in this example:</span></span>

[!code-html[Main](hubs-api-guide-server/samples/sample66.html?highlight=17-72)]

<span data-ttu-id="98d9e-421">Lorsque vous exécutez l’application dans Visual Studio, vous pouvez afficher les journaux dans le **sortie** fenêtre.</span><span class="sxs-lookup"><span data-stu-id="98d9e-421">When you run the application in Visual Studio, you can view the logs in the **Output** window.</span></span>

<a id="callfromoutsidehub"></a>

## <a name="how-to-call-client-methods-and-manage-groups-from-outside-the-hub-class"></a><span data-ttu-id="98d9e-422">Comment appeler des méthodes de client et de gérer des groupes à l’extérieur de la classe de concentrateur</span><span class="sxs-lookup"><span data-stu-id="98d9e-422">How to call client methods and manage groups from outside the Hub class</span></span>

<span data-ttu-id="98d9e-423">Pour appeler des méthodes de client à partir d’une autre classe que votre classe de concentrateur, obtenir une référence à l’objet de contexte SignalR pour le concentrateur et l’utiliser pour appeler des méthodes sur le client ou gérer des groupes.</span><span class="sxs-lookup"><span data-stu-id="98d9e-423">To call client methods from a different class than your Hub class, get a reference to the SignalR context object for the Hub and use that to call methods on the client or manage groups.</span></span>

<span data-ttu-id="98d9e-424">L’exemple suivant `StockTicker` classe obtient l’objet de contexte, il stocke dans une instance de la classe, stocke l’instance de classe dans une propriété statique et utilise le contexte de l’instance de la classe singleton pour appeler le `updateStockPrice` sur les clients (méthode) connecté à un concentrateur nommé `StockTickerHub`.</span><span class="sxs-lookup"><span data-stu-id="98d9e-424">The following sample `StockTicker` class gets the context object, stores it in an instance of the class, stores the class instance in a static property, and uses the context from the singleton class instance to call the `updateStockPrice` method on clients that are connected to a Hub named `StockTickerHub`.</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample67.cs?highlight=8,24)]

<span data-ttu-id="98d9e-425">Si vous avez besoin d’utiliser les contexte plusieurs fois dans un objet de longue durée, obtenir la référence une seule fois et enregistrez il plutôt que de l’obtenir chaque fois.</span><span class="sxs-lookup"><span data-stu-id="98d9e-425">If you need to use the context multiple-times in a long-lived object, get the reference once and save it rather than getting it again each time.</span></span> <span data-ttu-id="98d9e-426">Obtenir le contexte qu’une seule fois garantit que SignalR envoie des messages aux clients dans l’ordre dans lequel vos méthodes de concentrateur rendre client appels de méthode.</span><span class="sxs-lookup"><span data-stu-id="98d9e-426">Getting the context once ensures that SignalR sends messages to clients in the same sequence in which your Hub methods make client method invocations.</span></span> <span data-ttu-id="98d9e-427">Pour obtenir un didacticiel qui montre comment utiliser le contexte de SignalR pour un concentrateur, consultez [serveur de diffusion avec ASP.NET SignalR](../getting-started/tutorial-server-broadcast-with-signalr.md).</span><span class="sxs-lookup"><span data-stu-id="98d9e-427">For a tutorial that shows how to use the SignalR context for a Hub, see [Server Broadcast with ASP.NET SignalR](../getting-started/tutorial-server-broadcast-with-signalr.md).</span></span>

<a id="callingclientsoutsidehub"></a>

### <a name="calling-client-methods"></a><span data-ttu-id="98d9e-428">Appel de méthodes de client</span><span class="sxs-lookup"><span data-stu-id="98d9e-428">Calling client methods</span></span>

<span data-ttu-id="98d9e-429">Vous pouvez spécifier les clients qui recevront le RPC, mais vous avez moins d’options que lorsque vous appelez à partir d’une classe de concentrateur.</span><span class="sxs-lookup"><span data-stu-id="98d9e-429">You can specify which clients will receive the RPC, but you have fewer options than when you call from a Hub class.</span></span> <span data-ttu-id="98d9e-430">La raison en est que le contexte n’est pas associé à un appel particulier à partir d’un client, donc toutes les méthodes qui requièrent des connaissances de l’ID de connexion en cours, telles que `Clients.Others`, ou `Clients.Caller`, ou `Clients.OthersInGroup`, ne sont pas disponibles.</span><span class="sxs-lookup"><span data-stu-id="98d9e-430">The reason for this is that the context is not associated with a particular call from a client, so any methods that require knowledge of the current connection ID, such as `Clients.Others`, or `Clients.Caller`, or `Clients.OthersInGroup`, are not available.</span></span> <span data-ttu-id="98d9e-431">Les options ci-dessous sont disponibles :</span><span class="sxs-lookup"><span data-stu-id="98d9e-431">The following options are available:</span></span>

- <span data-ttu-id="98d9e-432">Tous les clients connectés.</span><span class="sxs-lookup"><span data-stu-id="98d9e-432">All connected clients.</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample68.cs)]
- <span data-ttu-id="98d9e-433">Un client spécifique identifié par l’ID de connexion.</span><span class="sxs-lookup"><span data-stu-id="98d9e-433">A specific client identified by connection ID.</span></span>

    [!code-css[Main](hubs-api-guide-server/samples/sample69.css)]
- <span data-ttu-id="98d9e-434">Tous les clients connectés à l’exception des clients spécifiés, identifiés par l’ID de connexion.</span><span class="sxs-lookup"><span data-stu-id="98d9e-434">All connected clients except the specified clients, identified by connection ID.</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample70.cs)]
- <span data-ttu-id="98d9e-435">Tous les clients connectés dans un groupe spécifié.</span><span class="sxs-lookup"><span data-stu-id="98d9e-435">All connected clients in a specified group.</span></span>

    [!code-css[Main](hubs-api-guide-server/samples/sample71.css)]
- <span data-ttu-id="98d9e-436">Tous les clients connectés dans un groupe spécifié, à l’exception des clients spécifiés, identifié par l’ID de connexion.</span><span class="sxs-lookup"><span data-stu-id="98d9e-436">All connected clients in a specified group except specified clients, identified by connection ID.</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample72.cs)]

<span data-ttu-id="98d9e-437">Si vous appelez dans votre classe non-Hub à partir des méthodes dans votre classe de concentrateur, vous pouvez entrer l’ID de connexion en cours et l’utiliser avec `Clients.Client`, `Clients.AllExcept`, ou `Clients.Group` pour simuler `Clients.Caller`, `Clients.Others`, ou `Clients.OthersInGroup`.</span><span class="sxs-lookup"><span data-stu-id="98d9e-437">If you are calling into your non-Hub class from methods in your Hub class, you can pass in the current connection ID and use that with `Clients.Client`, `Clients.AllExcept`, or `Clients.Group` to simulate `Clients.Caller`, `Clients.Others`, or `Clients.OthersInGroup`.</span></span> <span data-ttu-id="98d9e-438">Dans l’exemple suivant, la `MoveShapeHub` classe passe l’ID de connexion à la `Broadcaster` classe afin que la `Broadcaster` classe capable de simuler `Clients.Others`.</span><span class="sxs-lookup"><span data-stu-id="98d9e-438">In the following example, the `MoveShapeHub` class passes the connection ID to the `Broadcaster` class so that the `Broadcaster` class can simulate `Clients.Others`.</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample73.cs?highlight=12,36)]

<a id="managinggroupsoutsidehub"></a>

### <a name="managing-group-membership"></a><span data-ttu-id="98d9e-439">La gestion de l’appartenance au groupe</span><span class="sxs-lookup"><span data-stu-id="98d9e-439">Managing group membership</span></span>

<span data-ttu-id="98d9e-440">Pour la gestion des groupes, vous avez les mêmes options comme vous le feriez dans une classe de concentrateur.</span><span class="sxs-lookup"><span data-stu-id="98d9e-440">For managing groups you have the same options as you do in a Hub class.</span></span>

- <span data-ttu-id="98d9e-441">Ajouter un client à un groupe</span><span class="sxs-lookup"><span data-stu-id="98d9e-441">Add a client to a group</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample74.cs)]
- <span data-ttu-id="98d9e-442">Supprimer un client à partir d’un groupe</span><span class="sxs-lookup"><span data-stu-id="98d9e-442">Remove a client from a group</span></span>

    [!code-css[Main](hubs-api-guide-server/samples/sample75.css)]

<a id="hubpipeline"></a>

## <a name="how-to-customize-the-hubs-pipeline"></a><span data-ttu-id="98d9e-443">Comment personnaliser le pipeline de concentrateurs</span><span class="sxs-lookup"><span data-stu-id="98d9e-443">How to customize the Hubs pipeline</span></span>

<span data-ttu-id="98d9e-444">SignalR vous permet d’injecter votre propre code dans le pipeline de concentrateur.</span><span class="sxs-lookup"><span data-stu-id="98d9e-444">SignalR enables you to inject your own code into the Hub pipeline.</span></span> <span data-ttu-id="98d9e-445">L’exemple suivant montre un module de pipeline de concentrateur personnalisé qui enregistre chaque appel de méthode entrant reçu du client et l’appel de méthode sortant est appelée sur le client :</span><span class="sxs-lookup"><span data-stu-id="98d9e-445">The following example shows a custom Hub pipeline module that logs each incoming method call received from the client and outgoing method call invoked on the client:</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample76.cs)]

<span data-ttu-id="98d9e-446">Le code suivant dans le *Startup.cs* fichier enregistre le module à s’exécuter dans le pipeline de Hub :</span><span class="sxs-lookup"><span data-stu-id="98d9e-446">The following code in the *Startup.cs* file registers the module to run in the Hub pipeline:</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample77.cs?highlight=3)]

<span data-ttu-id="98d9e-447">Il existe de nombreuses méthodes différentes que vous pouvez substituer.</span><span class="sxs-lookup"><span data-stu-id="98d9e-447">There are many different methods that you can override.</span></span> <span data-ttu-id="98d9e-448">Pour obtenir la liste complète, consultez [HubPipelineModule méthodes](https://msdn.microsoft.com/en-us/library/jj918633(v=vs.111).aspx).</span><span class="sxs-lookup"><span data-stu-id="98d9e-448">For a complete list, see [HubPipelineModule Methods](https://msdn.microsoft.com/en-us/library/jj918633(v=vs.111).aspx).</span></span>
