---
uid: signalr/overview/older-versions/signalr-1x-hubs-api-guide-javascript-client
title: "Guide d’API concentrateurs SignalR 1.x - Client JavaScript | Documents Microsoft"
author: pfletcher
description: "Ce document fournit une introduction à l’utilisation de l’API de Hubs pour SignalR version 1.1, les clients JavaScript, telles que les navigateurs et du Windows Store (WinJS) applic..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/17/2013
ms.topic: article
ms.assetid: dcd4593b-1118-418a-af71-d12ff33fb36d
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/older-versions/signalr-1x-hubs-api-guide-javascript-client
msc.type: authoredcontent
ms.openlocfilehash: f92470b2022f343cfd6d822abb255dc19947b4d1
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/24/2018
---
<a name="signalr-1x-hubs-api-guide---javascript-client"></a><span data-ttu-id="19a2c-103">Guide d’API concentrateurs SignalR 1.x - Client JavaScript</span><span class="sxs-lookup"><span data-stu-id="19a2c-103">SignalR 1.x Hubs API Guide - JavaScript Client</span></span>
====================
<span data-ttu-id="19a2c-104">par [Patrick Fletcher](https://github.com/pfletcher), [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="19a2c-104">by [Patrick Fletcher](https://github.com/pfletcher), [Tom Dykstra](https://github.com/tdykstra)</span></span>

> <span data-ttu-id="19a2c-105">Ce document fournit une introduction à l’utilisation de l’API de Hubs pour SignalR version 1.1, les clients JavaScript, telles que des navigateurs et des applications du Windows Store (WinJS).</span><span class="sxs-lookup"><span data-stu-id="19a2c-105">This document provides an introduction to using the Hubs API for SignalR version 1.1 in JavaScript clients, such as browsers and Windows Store (WinJS) applications.</span></span>
> 
> <span data-ttu-id="19a2c-106">L’API de concentrateurs SignalR vous permet de vous permettent d’effectuer des appels de procédure distante (RPC) à partir d’un serveur pour les clients connectés et à partir de clients sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="19a2c-106">The SignalR Hubs API enables you to make remote procedure calls (RPCs) from a server to connected clients and from clients to the server.</span></span> <span data-ttu-id="19a2c-107">Dans le code serveur, vous définissez les méthodes qui peuvent être appelées par les clients, et vous appelez des méthodes qui s’exécutent sur le client.</span><span class="sxs-lookup"><span data-stu-id="19a2c-107">In server code, you define methods that can be called by clients, and you call methods that run on the client.</span></span> <span data-ttu-id="19a2c-108">Dans le code client, vous définissez les méthodes qui peuvent être appelées à partir du serveur, et vous appelez des méthodes qui s’exécutent sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="19a2c-108">In client code, you define methods that can be called from the server, and you call methods that run on the server.</span></span> <span data-ttu-id="19a2c-109">SignalR prend en charge de tous les éléments client-serveur pour vous.</span><span class="sxs-lookup"><span data-stu-id="19a2c-109">SignalR takes care of all of the client-to-server plumbing for you.</span></span>
> 
> <span data-ttu-id="19a2c-110">SignalR offre également une API de niveau inférieur appelée connexions persistantes.</span><span class="sxs-lookup"><span data-stu-id="19a2c-110">SignalR also offers a lower-level API called Persistent Connections.</span></span> <span data-ttu-id="19a2c-111">Pour obtenir une présentation SignalR, des concentrateurs et des connexions persistantes, ou pour obtenir un didacticiel qui montre comment générer une application SignalR complète, consultez [SignalR - mise en route](../getting-started/index.md).</span><span class="sxs-lookup"><span data-stu-id="19a2c-111">For an introduction to SignalR, Hubs, and Persistent Connections, or for a tutorial that shows how to build a complete SignalR application, see [SignalR - Getting Started](../getting-started/index.md).</span></span>


## <a name="overview"></a><span data-ttu-id="19a2c-112">Vue d'ensemble</span><span class="sxs-lookup"><span data-stu-id="19a2c-112">Overview</span></span>

<span data-ttu-id="19a2c-113">Ce document contient les sections suivantes :</span><span class="sxs-lookup"><span data-stu-id="19a2c-113">This document contains the following sections:</span></span>

- [<span data-ttu-id="19a2c-114">Le proxy généré et ce qu’il fait pour vous</span><span class="sxs-lookup"><span data-stu-id="19a2c-114">The generated proxy and what it does for you</span></span>](#genproxy)

    - [<span data-ttu-id="19a2c-115">Quand utiliser le proxy généré</span><span class="sxs-lookup"><span data-stu-id="19a2c-115">When to use the generated proxy</span></span>](#cantusegenproxy)
- [<span data-ttu-id="19a2c-116">Programme d’installation du client</span><span class="sxs-lookup"><span data-stu-id="19a2c-116">Client Setup</span></span>](#clientsetup)

    - [<span data-ttu-id="19a2c-117">Comment référencer le proxy généré de manière dynamique</span><span class="sxs-lookup"><span data-stu-id="19a2c-117">How to reference the dynamically generated proxy</span></span>](#dynamicproxy)
    - [<span data-ttu-id="19a2c-118">Comment créer un fichier physique pour SignalR de proxy généré</span><span class="sxs-lookup"><span data-stu-id="19a2c-118">How to create a physical file for the SignalR generated proxy</span></span>](#manualproxy)
- [<span data-ttu-id="19a2c-119">Comment établir une connexion</span><span class="sxs-lookup"><span data-stu-id="19a2c-119">How to establish a connection</span></span>](#establishconnection)

    - [<span data-ttu-id="19a2c-120">$. connection.hub est le même que $.hubConnection() crée l’objet</span><span class="sxs-lookup"><span data-stu-id="19a2c-120">$.connection.hub is the same object that $.hubConnection() creates</span></span>](#connequivalence)
    - [<span data-ttu-id="19a2c-121">Exécution asynchrone de la méthode de démarrage</span><span class="sxs-lookup"><span data-stu-id="19a2c-121">Asynchronous execution of the start method</span></span>](#asyncstart)
- [<span data-ttu-id="19a2c-122">Comment établir une connexion entre domaines</span><span class="sxs-lookup"><span data-stu-id="19a2c-122">How to establish a cross-domain connection</span></span>](#crossdomain)
- [<span data-ttu-id="19a2c-123">Comment configurer la connexion</span><span class="sxs-lookup"><span data-stu-id="19a2c-123">How to configure the connection</span></span>](#configureconnection)

    - [<span data-ttu-id="19a2c-124">Comment spécifier des paramètres de chaîne de requête</span><span class="sxs-lookup"><span data-stu-id="19a2c-124">How to specify query string parameters</span></span>](#querystring)
    - [<span data-ttu-id="19a2c-125">Comment spécifier le mode de transport</span><span class="sxs-lookup"><span data-stu-id="19a2c-125">How to specify the transport method</span></span>](#transport)
- [<span data-ttu-id="19a2c-126">Comment obtenir un proxy pour une classe de concentrateur</span><span class="sxs-lookup"><span data-stu-id="19a2c-126">How to get a proxy for a Hub class</span></span>](#getproxy)
- [<span data-ttu-id="19a2c-127">Comment définir des méthodes sur le client que le serveur peut appeler.</span><span class="sxs-lookup"><span data-stu-id="19a2c-127">How to define methods on the client that the server can call</span></span>](#callclient)
- [<span data-ttu-id="19a2c-128">Comment appeler des méthodes de serveur à partir du client</span><span class="sxs-lookup"><span data-stu-id="19a2c-128">How to call server methods from the client</span></span>](#callserver)
- [<span data-ttu-id="19a2c-129">Comment gérer les événements de durée de vie de connexion</span><span class="sxs-lookup"><span data-stu-id="19a2c-129">How to handle connection lifetime events</span></span>](#connectionlifetime)
- [<span data-ttu-id="19a2c-130">Comment gérer les erreurs</span><span class="sxs-lookup"><span data-stu-id="19a2c-130">How to handle errors</span></span>](#handleerrors)
- [<span data-ttu-id="19a2c-131">Comment activer la journalisation côté client</span><span class="sxs-lookup"><span data-stu-id="19a2c-131">How to enable client-side logging</span></span>](#logging)

<span data-ttu-id="19a2c-132">Pour plus d’informations sur la façon de programmer le serveur ou les clients .NET, consultez les ressources suivantes :</span><span class="sxs-lookup"><span data-stu-id="19a2c-132">For documentation on how to program the server or .NET clients, see the following resources:</span></span>

- [<span data-ttu-id="19a2c-133">Guide d’API concentrateurs SignalR - serveur</span><span class="sxs-lookup"><span data-stu-id="19a2c-133">SignalR Hubs API Guide - Server</span></span>](../guide-to-the-api/hubs-api-guide-server.md)
- [<span data-ttu-id="19a2c-134">Guide d’API concentrateurs SignalR - Client .NET</span><span class="sxs-lookup"><span data-stu-id="19a2c-134">SignalR Hubs API Guide - .NET Client</span></span>](../guide-to-the-api/hubs-api-guide-net-client.md)

<span data-ttu-id="19a2c-135">Des liens vers des rubriques de référence de l’API sont à la version de .NET 4.5 de l’API.</span><span class="sxs-lookup"><span data-stu-id="19a2c-135">Links to API Reference topics are to the .NET 4.5 version of the API.</span></span> <span data-ttu-id="19a2c-136">Si vous utilisez le .NET 4, consultez [la version de .NET 4 des rubriques API](https://msdn.microsoft.com/library/jj891075(v=vs.100).aspx).</span><span class="sxs-lookup"><span data-stu-id="19a2c-136">If you're using .NET 4, see [the .NET 4 version of the API topics](https://msdn.microsoft.com/library/jj891075(v=vs.100).aspx).</span></span>

<a id="genproxy"></a>

## <a name="the-generated-proxy-and-what-it-does-for-you"></a><span data-ttu-id="19a2c-137">Le proxy généré et ce qu’il fait pour vous</span><span class="sxs-lookup"><span data-stu-id="19a2c-137">The generated proxy and what it does for you</span></span>

<span data-ttu-id="19a2c-138">Vous pouvez programmer un client JavaScript pour communiquer avec un service SignalR avec ou sans un proxy SignalR génère pour vous.</span><span class="sxs-lookup"><span data-stu-id="19a2c-138">You can program a JavaScript client to communicate with a SignalR service with or without a proxy that SignalR generates for you.</span></span> <span data-ttu-id="19a2c-139">Ce que fait le serveur proxy pour vous est de simplifier la syntaxe du code que vous utilisez pour vous connecter, les méthodes d’écriture que le serveur appelle, et appelez des méthodes sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="19a2c-139">What the proxy does for you is simplify the syntax of the code you use to connect, write methods that the server calls, and call methods on the server.</span></span>

<span data-ttu-id="19a2c-140">Lorsque vous écrivez du code pour appeler des méthodes de serveur, le proxy généré vous permet d’utiliser la syntaxe qui semble que vous s’exécutaient une fonction locale : vous pouvez écrire `serverMethod(arg1, arg2)` au lieu de `invoke('serverMethod', arg1, arg2)`.</span><span class="sxs-lookup"><span data-stu-id="19a2c-140">When you write code to call server methods, the generated proxy enables you to use syntax that looks as though you were executing a local function: you can write `serverMethod(arg1, arg2)` instead of `invoke('serverMethod', arg1, arg2)`.</span></span> <span data-ttu-id="19a2c-141">La syntaxe de proxy généré permet également une erreur côté client immédiate et intelligible si vous orthographiez mal le nom d’une méthode de serveur.</span><span class="sxs-lookup"><span data-stu-id="19a2c-141">The generated proxy syntax also enables an immediate and intelligible client-side error if you mistype a server method name.</span></span> <span data-ttu-id="19a2c-142">Et si vous créez manuellement le fichier qui définit les serveurs proxy, vous pouvez également obtenir de prise en charge IntelliSense pour l’écriture de code qui appelle des méthodes de serveur.</span><span class="sxs-lookup"><span data-stu-id="19a2c-142">And if you manually create the file that defines the proxies, you can also get IntelliSense support for writing code that calls server methods.</span></span>

<span data-ttu-id="19a2c-143">Par exemple, supposons que vous disposez de la classe de concentrateur suivante sur le serveur :</span><span class="sxs-lookup"><span data-stu-id="19a2c-143">For example, suppose you have the following Hub class on the server:</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample1.cs?highlight=1,3,5)]

<span data-ttu-id="19a2c-144">Les exemples de code suivants montrent ce qui se présente le code JavaScript, pour appeler le `NewContosoChatMessage` méthode sur le serveur et de recevoir des appels de la `addContosoChatMessageToPage` méthode à partir du serveur.</span><span class="sxs-lookup"><span data-stu-id="19a2c-144">The following code examples show what JavaScript code looks like for invoking the `NewContosoChatMessage` method on the server and receiving invocations of the `addContosoChatMessageToPage` method from the server.</span></span>

<span data-ttu-id="19a2c-145">**Avec le proxy généré**</span><span class="sxs-lookup"><span data-stu-id="19a2c-145">**With the generated proxy**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample2.js?highlight=1-2,8)]

<span data-ttu-id="19a2c-146">**Sans le proxy généré**</span><span class="sxs-lookup"><span data-stu-id="19a2c-146">**Without the generated proxy**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample3.js?highlight=2-3,9)]

<a id="cantusegenproxy"></a>

### <a name="when-to-use-the-generated-proxy"></a><span data-ttu-id="19a2c-147">Quand utiliser le proxy généré</span><span class="sxs-lookup"><span data-stu-id="19a2c-147">When to use the generated proxy</span></span>

<span data-ttu-id="19a2c-148">Si vous souhaitez inscrire une méthode de client que le serveur appelle plusieurs gestionnaires d’événements, vous ne pouvez pas utiliser le proxy généré.</span><span class="sxs-lookup"><span data-stu-id="19a2c-148">If you want to register multiple event handlers for a client method that the server calls, you can't use the generated proxy.</span></span> <span data-ttu-id="19a2c-149">Dans le cas contraire, vous pouvez choisir d’utiliser le proxy généré ou non en fonction de vos préférences de codage.</span><span class="sxs-lookup"><span data-stu-id="19a2c-149">Otherwise, you can choose to use the generated proxy or not based on your coding preference.</span></span> <span data-ttu-id="19a2c-150">Si vous choisissez de ne pas l’utiliser, vous n’êtes pas obligé de faire référence à l’URL « / concentrateurs signalr » dans un `script` élément dans votre code client.</span><span class="sxs-lookup"><span data-stu-id="19a2c-150">If you choose not to use it, you don't have to reference the "signalr/hubs" URL in a `script` element in your client code.</span></span>

<a id="clientsetup"></a>

## <a name="client-setup"></a><span data-ttu-id="19a2c-151">Programme d’installation du client</span><span class="sxs-lookup"><span data-stu-id="19a2c-151">Client setup</span></span>

<span data-ttu-id="19a2c-152">Un client JavaScript nécessite des références aux jQuery et le fichier JavaScript de base SignalR.</span><span class="sxs-lookup"><span data-stu-id="19a2c-152">A JavaScript client requires references to jQuery and the SignalR core JavaScript file.</span></span> <span data-ttu-id="19a2c-153">La version de jQuery doit être 1.6.4 ou versions ultérieures principales, comme 1.7.2, 1.8.2 ou 1.9.1.</span><span class="sxs-lookup"><span data-stu-id="19a2c-153">The jQuery version must be 1.6.4 or major later versions, such as 1.7.2, 1.8.2, or 1.9.1.</span></span> <span data-ttu-id="19a2c-154">Si vous décidez d’utiliser le proxy généré, vous devez également une référence au fichier JavaScript proxy SignalR généré.</span><span class="sxs-lookup"><span data-stu-id="19a2c-154">If you decide to use the generated proxy, you also need a reference to the SignalR generated proxy JavaScript file.</span></span> <span data-ttu-id="19a2c-155">L’exemple suivant montre ce que les références peut se présenter comme dans une page HTML qui utilise le proxy généré.</span><span class="sxs-lookup"><span data-stu-id="19a2c-155">The following example shows what the references might look like in an HTML page that uses the generated proxy.</span></span>

[!code-html[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample4.html)]

<span data-ttu-id="19a2c-156">Ces références doivent être inclus dans cet ordre : jQuery first, last, SignalR core après cela et les proxys de SignalR.</span><span class="sxs-lookup"><span data-stu-id="19a2c-156">These references must be included in this order: jQuery first, SignalR core after that, and SignalR proxies last.</span></span>

<a id="dynamicproxy"></a>

### <a name="how-to-reference-the-dynamically-generated-proxy"></a><span data-ttu-id="19a2c-157">Comment référencer le proxy généré de manière dynamique</span><span class="sxs-lookup"><span data-stu-id="19a2c-157">How to reference the dynamically generated proxy</span></span>

<span data-ttu-id="19a2c-158">Dans l’exemple précédent, la référence au proxy SignalR généré est au code JavaScript généré dynamiquement, pas à un fichier physique.</span><span class="sxs-lookup"><span data-stu-id="19a2c-158">In the preceding example, the reference to the SignalR generated proxy is to dynamically generated JavaScript code, not to a physical file.</span></span> <span data-ttu-id="19a2c-159">SignalR crée le code JavaScript pour le proxy à la volée et le fournit au client en réponse à l’URL « concentrateurs signalr / / ».</span><span class="sxs-lookup"><span data-stu-id="19a2c-159">SignalR creates the JavaScript code for the proxy on the fly and serves it to the client in response to the "/signalr/hubs" URL.</span></span> <span data-ttu-id="19a2c-160">Si vous avez spécifié une autre URL de base pour les connexions SignalR sur le serveur dans votre `MapHubs` , l’URL du fichier proxy généré dynamiquement est votre URL personnalisée avec « / concentrateurs » ajouté à la fin.</span><span class="sxs-lookup"><span data-stu-id="19a2c-160">If you specified a different base URL for SignalR connections on the server in your `MapHubs` method, the URL for the dynamically generated proxy file is your custom URL with "/hubs" appended to it.</span></span>

> [!NOTE]
> <span data-ttu-id="19a2c-161">Pour les clients JavaScript de Windows 8 (Windows Store), utilisez le fichier de proxy physique au lieu de celle générée dynamiquement.</span><span class="sxs-lookup"><span data-stu-id="19a2c-161">For Windows 8 (Windows Store) JavaScript clients, use the physical proxy file instead of the dynamically generated one.</span></span> <span data-ttu-id="19a2c-162">Pour plus d’informations, consultez [proxy généré de la création d’un fichier physique pour SignalR](#manualproxy) plus loin dans cette rubrique.</span><span class="sxs-lookup"><span data-stu-id="19a2c-162">For more information, see [How to create a physical file for the SignalR generated proxy](#manualproxy) later in this topic.</span></span>


<span data-ttu-id="19a2c-163">Dans une vue ASP.NET MVC 4 Razor, utilisez le tilde pour faire référence à la racine de l’application dans votre référence de fichier proxy :</span><span class="sxs-lookup"><span data-stu-id="19a2c-163">In an ASP.NET MVC 4 Razor view, use the tilde to refer to the application root in your proxy file reference:</span></span>

[!code-html[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample5.html)]

<span data-ttu-id="19a2c-164">Pour plus d’informations sur l’utilisation de SignalR dans MVC 4, consultez [prise en main SignalR et MVC 4](tutorial-getting-started-with-signalr-and-mvc-4.md).</span><span class="sxs-lookup"><span data-stu-id="19a2c-164">For more information about using SignalR in MVC 4, see [Getting Started with SignalR and MVC 4](tutorial-getting-started-with-signalr-and-mvc-4.md).</span></span>

<span data-ttu-id="19a2c-165">Dans une vue ASP.NET MVC 3 Razor, utilisez `Url.Content` pour des références de fichier proxy :</span><span class="sxs-lookup"><span data-stu-id="19a2c-165">In an ASP.NET MVC 3 Razor view, use `Url.Content` for your proxy file reference:</span></span>

[!code-cshtml[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample6.cshtml)]

<span data-ttu-id="19a2c-166">Dans une application ASP.NET Web Forms, utilisez `ResolveClientUrl` pour les proxys de référence de fichier ou enregistrez-le via ScriptManager à l’aide d’une application racine chemin d’accès relatif (commence par un tilde) :</span><span class="sxs-lookup"><span data-stu-id="19a2c-166">In an ASP.NET Web Forms application, use `ResolveClientUrl` for your proxies file reference or register it via the ScriptManager using an app root relative path (starting with a tilde):</span></span>

[!code-aspx[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample7.aspx)]

<span data-ttu-id="19a2c-167">En règle générale, utilisez la même méthode pour spécifier l’URL « concentrateurs signalr / / » que vous utilisez pour les fichiers CSS ou JavaScript.</span><span class="sxs-lookup"><span data-stu-id="19a2c-167">As a general rule, use the same method for specifying the "/signalr/hubs" URL that you use for CSS or JavaScript files.</span></span> <span data-ttu-id="19a2c-168">Si vous spécifiez une URL sans l’aide d’un tilde, dans certains scénarios de votre application fonctionnera correctement lorsque vous testez dans Visual Studio à l’aide d’IIS Express, mais échoue avec une erreur 404 lorsque vous déployez vers IIS complet.</span><span class="sxs-lookup"><span data-stu-id="19a2c-168">If you specify a URL without using a tilde, in some scenarios your application will work correctly when you test in Visual Studio using IIS Express but will fail with a 404 error when you deploy to full IIS.</span></span> <span data-ttu-id="19a2c-169">Pour plus d’informations, consultez **résolution des références aux ressources de niveau racine** dans [serveurs Web dans Visual Studio pour les projets Web ASP.NET](https://msdn.microsoft.com/library/58wxa9w5.aspx) sur le site MSDN.</span><span class="sxs-lookup"><span data-stu-id="19a2c-169">For more information, see **Resolving References to Root-Level Resources** in [Web Servers in Visual Studio for ASP.NET Web Projects](https://msdn.microsoft.com/library/58wxa9w5.aspx) on the MSDN site.</span></span>

<span data-ttu-id="19a2c-170">Lorsque vous exécutez un projet web dans Visual Studio 2012 en mode débogage, et si vous utilisez Internet Explorer comme navigateur, vous pouvez voir le fichier proxy dans **l’Explorateur de solutions** sous **Documents de Script**, comme illustré dans le illustration suivante.</span><span class="sxs-lookup"><span data-stu-id="19a2c-170">When you run a web project in Visual Studio 2012 in debug mode, and if you use Internet Explorer as your browser, you can see the proxy file in **Solution Explorer** under **Script Documents**, as shown in the following illustration.</span></span>

![Fichier proxy généré de JavaScript dans l’Explorateur de solutions](signalr-1x-hubs-api-guide-javascript-client/_static/image1.png)

<span data-ttu-id="19a2c-172">Pour afficher le contenu du fichier, double-cliquez sur **concentrateurs**.</span><span class="sxs-lookup"><span data-stu-id="19a2c-172">To see the contents of the file, double-click **hubs**.</span></span> <span data-ttu-id="19a2c-173">Si vous n’utilisez pas Visual Studio 2012 et Internet Explorer, ou si vous n’êtes pas en mode débogage, vous pouvez également obtenir le contenu du fichier en accédant à l’URL « concentrateurs signalR / / ».</span><span class="sxs-lookup"><span data-stu-id="19a2c-173">If you are not using Visual Studio 2012 and Internet Explorer, or if you are not in debug mode, you can also get the contents of the file by browsing to the "/signalR/hubs" URL.</span></span> <span data-ttu-id="19a2c-174">Par exemple, si votre site est en cours d’exécution à `http://localhost:56699`, accédez à `http://localhost:56699/SignalR/hubs` dans votre navigateur.</span><span class="sxs-lookup"><span data-stu-id="19a2c-174">For example, if your site is running at `http://localhost:56699`, go to `http://localhost:56699/SignalR/hubs` in your browser.</span></span>

<a id="manualproxy"></a>

### <a name="how-to-create-a-physical-file-for-the-signalr-generated-proxy"></a><span data-ttu-id="19a2c-175">Comment créer un fichier physique pour SignalR de proxy généré</span><span class="sxs-lookup"><span data-stu-id="19a2c-175">How to create a physical file for the SignalR generated proxy</span></span>

<span data-ttu-id="19a2c-176">Comme alternative au proxy généré de manière dynamique, vous pouvez créer un fichier physique qui a le code proxy et référencer ce fichier.</span><span class="sxs-lookup"><span data-stu-id="19a2c-176">As an alternative to the dynamically generated proxy, you can create a physical file that has the proxy code and reference that file.</span></span> <span data-ttu-id="19a2c-177">Vous pourriez le faire pour contrôler la mise en cache ou le comportement de regroupement ou d’utiliser IntelliSense lorsque vous codez des appels aux méthodes de serveur.</span><span class="sxs-lookup"><span data-stu-id="19a2c-177">You might want to do that for control over caching or bundling behavior, or to get IntelliSense when you are coding calls to server methods.</span></span>

<span data-ttu-id="19a2c-178">Pour créer un fichier proxy, procédez comme suit :</span><span class="sxs-lookup"><span data-stu-id="19a2c-178">To create a proxy file, perform the following steps:</span></span>

1. <span data-ttu-id="19a2c-179">Installer le [Microsoft.AspNet.SignalR.Utils](https://nuget.org/packages/Microsoft.AspNet.SignalR.Utils/) package NuGet.</span><span class="sxs-lookup"><span data-stu-id="19a2c-179">Install the [Microsoft.AspNet.SignalR.Utils](https://nuget.org/packages/Microsoft.AspNet.SignalR.Utils/) NuGet package.</span></span>
2. <span data-ttu-id="19a2c-180">Ouvrez une invite de commandes et accédez à la *outils* dossier qui contient le fichier SignalR.exe.</span><span class="sxs-lookup"><span data-stu-id="19a2c-180">Open a command prompt and browse to the *tools* folder that contains the SignalR.exe file.</span></span> <span data-ttu-id="19a2c-181">Le dossier Outils est à l’emplacement suivant :</span><span class="sxs-lookup"><span data-stu-id="19a2c-181">The tools folder is at the following location:</span></span>

    `[your solution folder]\packages\Microsoft.AspNet.SignalR.Utils.1.0.1\tools`
3. <span data-ttu-id="19a2c-182">Entrez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="19a2c-182">Enter the following command:</span></span>

    `signalr ghp /path:[path to the .dll that contains your Hub class]`

    <span data-ttu-id="19a2c-183">Le chemin d’accès à votre *.dll* est généralement le *bin* dans votre dossier de projet.</span><span class="sxs-lookup"><span data-stu-id="19a2c-183">The path to your *.dll* is typically the *bin* folder in your project folder.</span></span>

    <span data-ttu-id="19a2c-184">Cette commande crée un fichier nommé *server.js* dans le même dossier que *signalr.exe*.</span><span class="sxs-lookup"><span data-stu-id="19a2c-184">This command creates a file named *server.js* in the same folder as *signalr.exe*.</span></span>
4. <span data-ttu-id="19a2c-185">Placez le *server.js* de fichiers dans un dossier approprié à votre projet, renommez-la en fonction de votre application et ajouter une référence à celui-ci à la place de la référence « concentrateurs signalr / ».</span><span class="sxs-lookup"><span data-stu-id="19a2c-185">Put the *server.js* file in an appropriate folder in your project, rename it as appropriate for your application, and add a reference to it in place of the "signalr/hubs" reference.</span></span>

<a id="establishconnection"></a>

## <a name="how-to-establish-a-connection"></a><span data-ttu-id="19a2c-186">Comment établir une connexion</span><span class="sxs-lookup"><span data-stu-id="19a2c-186">How to establish a connection</span></span>

<span data-ttu-id="19a2c-187">Avant de pouvoir établir une connexion, vous devez créer un objet de connexion, créer un proxy et inscrire les gestionnaires d’événements pour les méthodes qui peuvent être appelées à partir du serveur.</span><span class="sxs-lookup"><span data-stu-id="19a2c-187">Before you can establish a connection, you have to create a connection object, create a proxy, and register event handlers for methods that can be called from the server.</span></span> <span data-ttu-id="19a2c-188">Lorsque les gestionnaires d’événements et de proxy sont configurés, établir la connexion en appelant le `start` (méthode).</span><span class="sxs-lookup"><span data-stu-id="19a2c-188">When the proxy and event handlers are set up, establish the connection by calling the `start` method.</span></span>

<span data-ttu-id="19a2c-189">Si vous utilisez le proxy généré, il est inutile de créer l’objet de connexion dans votre propre code, car le code proxy généré le fait pour vous.</span><span class="sxs-lookup"><span data-stu-id="19a2c-189">If you are using the generated proxy, you don't have to create the connection object in your own code because the generated proxy code does it for you.</span></span>

<a id="nogenconnection"></a>

<span data-ttu-id="19a2c-190">**Établir une connexion (avec le proxy généré)**</span><span class="sxs-lookup"><span data-stu-id="19a2c-190">**Establish a connection (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample8.js?highlight=5)]

<span data-ttu-id="19a2c-191">**Établir une connexion (sans le proxy généré)**</span><span class="sxs-lookup"><span data-stu-id="19a2c-191">**Establish a connection (without the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample9.js?highlight=1,6)]

<span data-ttu-id="19a2c-192">L’exemple de code utilise la valeur par défaut « / signalr « URL pour se connecter à votre service de SignalR.</span><span class="sxs-lookup"><span data-stu-id="19a2c-192">The sample code uses the default "/signalr" URL to connect to your SignalR service.</span></span> <span data-ttu-id="19a2c-193">Pour plus d’informations sur la façon de spécifier une autre URL de base, consultez [ASP.NET SignalR concentrateurs API Guide - Server - l’URL /signalr](../guide-to-the-api/hubs-api-guide-server.md#signalrurl).</span><span class="sxs-lookup"><span data-stu-id="19a2c-193">For information about how to specify a different base URL, see [ASP.NET SignalR Hubs API Guide - Server - The /signalr URL](../guide-to-the-api/hubs-api-guide-server.md#signalrurl).</span></span>

> [!NOTE]
> <span data-ttu-id="19a2c-194">Normalement, vous inscrivez les gestionnaires d’événements avant d’appeler le `start` méthode pour établir la connexion.</span><span class="sxs-lookup"><span data-stu-id="19a2c-194">Normally you register event handlers before calling the `start` method to establish the connection.</span></span> <span data-ttu-id="19a2c-195">Si vous souhaitez enregistrer certains gestionnaires d’événements après avoir établi la connexion, vous pouvez le faire, mais vous devez vous inscrire au moins un de vos gestionnaires d’événements avant d’appeler le `start` (méthode).</span><span class="sxs-lookup"><span data-stu-id="19a2c-195">If you want to register some event handlers after establishing the connection, you can do that, but you must register at least one of your event handler(s) before calling the `start` method.</span></span> <span data-ttu-id="19a2c-196">Une des raisons sont qu’il peut y avoir de nombreux concentrateurs dans une application, mais vous ne souhaitez pas déclencher le `OnConnected` événement sur chaque Hub si vous vous apprêtez uniquement à utiliser pour un d’eux.</span><span class="sxs-lookup"><span data-stu-id="19a2c-196">One reason for this is that there can be many Hubs in an application, but you wouldn't want to trigger the `OnConnected` event on every Hub if you are only going to use to one of them.</span></span> <span data-ttu-id="19a2c-197">Lorsque la connexion est établie, la présence d’une méthode du client sur le proxy d’un concentrateur est ce qui indique à SignalR pour déclencher le `OnConnected` événement.</span><span class="sxs-lookup"><span data-stu-id="19a2c-197">When the connection is established, the presence of a client method on a Hub's proxy is what tells SignalR to trigger the `OnConnected` event.</span></span> <span data-ttu-id="19a2c-198">Si vous n’inscrivez les gestionnaires d’événements avant d’appeler le `start` (méthode), vous serez en mesure d’appeler des méthodes sur le concentrateur, mais de concentrateur `OnConnected` méthode ne sera pas appelée et aucune méthode client ne sera appelée à partir du serveur.</span><span class="sxs-lookup"><span data-stu-id="19a2c-198">If you don't register any event handlers before calling the `start` method, you will be able to invoke methods on the Hub, but the Hub's `OnConnected` method won't be called and no client methods will be invoked from the server.</span></span>


<a id="connequivalence"></a>

### <a name="connectionhub-is-the-same-object-that-hubconnection-creates"></a><span data-ttu-id="19a2c-199">$. connection.hub est le même que $.hubConnection() crée l’objet</span><span class="sxs-lookup"><span data-stu-id="19a2c-199">$.connection.hub is the same object that $.hubConnection() creates</span></span>

<span data-ttu-id="19a2c-200">Comme vous pouvez voir des exemples, lorsque vous utilisez le proxy généré, `$.connection.hub` fait référence à l’objet de connexion.</span><span class="sxs-lookup"><span data-stu-id="19a2c-200">As you can see from the examples, when you use the generated proxy, `$.connection.hub` refers to the connection object.</span></span> <span data-ttu-id="19a2c-201">Il s’agit du même objet que vous obtenez en appelant `$.hubConnection()` lorsque vous n’utilisez pas le proxy généré.</span><span class="sxs-lookup"><span data-stu-id="19a2c-201">This is the same object that you get by calling `$.hubConnection()` when you aren't using the generated proxy.</span></span> <span data-ttu-id="19a2c-202">Le code proxy généré crée la connexion pour vous en exécutant l’instruction suivante :</span><span class="sxs-lookup"><span data-stu-id="19a2c-202">The generated proxy code creates the connection for you by executing the following statement:</span></span>

![Création d’une connexion dans le fichier proxy généré](signalr-1x-hubs-api-guide-javascript-client/_static/image3.png)

<span data-ttu-id="19a2c-204">Lorsque vous utilisez le proxy généré, vous pouvez faire tout ce avec `$.connection.hub` que vous pouvez faire avec un objet de connexion lorsque vous n’utilisez pas le proxy généré.</span><span class="sxs-lookup"><span data-stu-id="19a2c-204">When you're using the generated proxy, you can do anything with `$.connection.hub` that you can do with a connection object when you're not using the generated proxy.</span></span>

<a id="asyncstart"></a>

### <a name="asynchronous-execution-of-the-start-method"></a><span data-ttu-id="19a2c-205">Exécution asynchrone de la méthode de démarrage</span><span class="sxs-lookup"><span data-stu-id="19a2c-205">Asynchronous execution of the start method</span></span>

<span data-ttu-id="19a2c-206">Le `start` méthode s’exécute de façon asynchrone.</span><span class="sxs-lookup"><span data-stu-id="19a2c-206">The `start` method executes asynchronously.</span></span> <span data-ttu-id="19a2c-207">Elle retourne un [jQuery différé objet](http://api.jquery.com/category/deferred-object/), ce qui signifie que vous pouvez ajouter des fonctions de rappel en appelant des méthodes comme `pipe`, `done`, et `fail`.</span><span class="sxs-lookup"><span data-stu-id="19a2c-207">It returns a [jQuery Deferred object](http://api.jquery.com/category/deferred-object/), which means that you can add callback functions by calling methods such as `pipe`, `done`, and `fail`.</span></span> <span data-ttu-id="19a2c-208">Si vous disposez du code que vous souhaitez exécuter une fois la connexion établie, tel qu’un appel à une méthode de serveur, placez ce code dans une fonction de rappel ou l’appeler à partir d’une fonction de rappel.</span><span class="sxs-lookup"><span data-stu-id="19a2c-208">If you have code that you want to execute after the connection is established, such as a call to a server method, put that code in a callback function or call it from a callback function.</span></span> <span data-ttu-id="19a2c-209">Le `.done` méthode de rappel est exécutée après la connexion a été établie, et une fois que tout code que vous avez votre `OnConnected` fin de la méthode de gestionnaire d’événements sur le serveur de l’exécution.</span><span class="sxs-lookup"><span data-stu-id="19a2c-209">The `.done` callback method is executed after the connection has been established, and after any code that you have in your `OnConnected` event handler method on the server finishes executing.</span></span>

<span data-ttu-id="19a2c-210">Si vous placez l’instruction « Maintenant connecté » de l’exemple précédent en tant que la ligne suivante de code après le `start` appel de méthode (pas dans un `.done` rappel), le `console.log` ligne s’exécute avant que la connexion est établie, comme indiqué dans l’exemple suivant exemple :</span><span class="sxs-lookup"><span data-stu-id="19a2c-210">If you put the "Now connected" statement from the preceding example as the next line of code after the `start` method call (not in a `.done` callback), the `console.log` line will execute before the connection is established, as shown in the following example:</span></span>

![Mauvaise façon d’écrire du code qui s’exécute après que la connexion est établie.](signalr-1x-hubs-api-guide-javascript-client/_static/image5.png)

<a id="crossdomain"></a>

## <a name="how-to-establish-a-cross-domain-connection"></a><span data-ttu-id="19a2c-212">Comment établir une connexion entre domaines</span><span class="sxs-lookup"><span data-stu-id="19a2c-212">How to establish a cross-domain connection</span></span>

<span data-ttu-id="19a2c-213">En général, si le navigateur charge une page à partir de `http://contoso.com`, la connexion SignalR est dans le même domaine, `http://contoso.com/signalr`.</span><span class="sxs-lookup"><span data-stu-id="19a2c-213">Typically if the browser loads a page from `http://contoso.com`, the SignalR connection is in the same domain, at `http://contoso.com/signalr`.</span></span> <span data-ttu-id="19a2c-214">Si la page à partir de `http://contoso.com` établit une connexion à `http://fabrikam.com/signalr`, qui est une connexion entre domaines.</span><span class="sxs-lookup"><span data-stu-id="19a2c-214">If the page from `http://contoso.com` makes a connection to `http://fabrikam.com/signalr`, that is a cross-domain connection.</span></span> <span data-ttu-id="19a2c-215">Pour des raisons de sécurité, les connexions inter-domaines sont désactivées par défaut.</span><span class="sxs-lookup"><span data-stu-id="19a2c-215">For security reasons, cross-domain connections are disabled by default.</span></span> <span data-ttu-id="19a2c-216">Pour établir une connexion entre domaines, vérifiez que les connexions entre les domaines sont activées sur le serveur et spécifiez l’URL de connexion lorsque vous créez l’objet de connexion.</span><span class="sxs-lookup"><span data-stu-id="19a2c-216">To establish a cross-domain connection, make sure that cross-domain connections are enabled on the server, and specify the connection URL when you create the connection object.</span></span> <span data-ttu-id="19a2c-217">SignalR utilise la technologie appropriée pour les connexions entre domaines, tels que [JSONP](http://en.wikipedia.org/wiki/JSONP) ou [CORS](http://en.wikipedia.org/wiki/Cross-origin_resource_sharing).</span><span class="sxs-lookup"><span data-stu-id="19a2c-217">SignalR will use the appropriate technology for cross-domain connections, such as [JSONP](http://en.wikipedia.org/wiki/JSONP) or [CORS](http://en.wikipedia.org/wiki/Cross-origin_resource_sharing).</span></span>

<span data-ttu-id="19a2c-218">Sur le serveur, activez les connexions entre domaines en sélectionnant cette option lorsque vous appelez le `MapHubs` (méthode).</span><span class="sxs-lookup"><span data-stu-id="19a2c-218">On the server, enable cross-domain connections by selecting that option when you call the `MapHubs` method.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample10.cs?highlight=2)]

<span data-ttu-id="19a2c-219">Sur le client, spécifiez l’URL lorsque vous créez l’objet de connexion (sans le proxy généré) ou avant d’appeler la méthode start (avec le proxy généré).</span><span class="sxs-lookup"><span data-stu-id="19a2c-219">On the client, specify the URL when you create the connection object (without the generated proxy) or before you call the start method (with the generated proxy).</span></span>

<span data-ttu-id="19a2c-220">**Code client qui spécifie une connexion entre domaines (avec le proxy généré)**</span><span class="sxs-lookup"><span data-stu-id="19a2c-220">**Client code that specifies a cross-domain connection (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample11.js?highlight=1)]

<span data-ttu-id="19a2c-221">**Code client qui spécifie une connexion entre domaines (sans le proxy généré)**</span><span class="sxs-lookup"><span data-stu-id="19a2c-221">**Client code that specifies a cross-domain connection (without the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample12.js?highlight=1)]

<span data-ttu-id="19a2c-222">Lorsque vous utilisez la `$.hubConnection` constructeur, il est inutile d’inclure `signalr` dans l’URL, car il est ajouté automatiquement (sauf si vous spécifiez `useDefaultUrl` en tant que `false`).</span><span class="sxs-lookup"><span data-stu-id="19a2c-222">When you use the `$.hubConnection` constructor, you don't have to include `signalr` in the URL because it is added automatically (unless you specify `useDefaultUrl` as `false`).</span></span>

<span data-ttu-id="19a2c-223">Vous pouvez créer plusieurs connexions à différents points de terminaison.</span><span class="sxs-lookup"><span data-stu-id="19a2c-223">You can create multiple connections to different endpoints.</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample13.js)]

> [!NOTE] 
> 
> - <span data-ttu-id="19a2c-224">Ne définissez pas `jQuery.support.cors` à true dans votre code.</span><span class="sxs-lookup"><span data-stu-id="19a2c-224">Don't set `jQuery.support.cors` to true in your code.</span></span>
> 
>     ![Ne définissez pas jQuery.support.cors sur true](signalr-1x-hubs-api-guide-javascript-client/_static/image7.png)
> 
>     <span data-ttu-id="19a2c-226">SignalR gère l’utilisation de JSONP ou CORS.</span><span class="sxs-lookup"><span data-stu-id="19a2c-226">SignalR handles the use of JSONP or CORS.</span></span> <span data-ttu-id="19a2c-227">Paramètre `jQuery.support.cors` à true désactive JSONP car elle force SignalR à assumer le navigateur prend en charge CORS.</span><span class="sxs-lookup"><span data-stu-id="19a2c-227">Setting `jQuery.support.cors` to true disables JSONP because it causes SignalR to assume the browser supports CORS.</span></span>
> - <span data-ttu-id="19a2c-228">Lorsque vous vous connectez à une URL localhost, Internet Explorer 10 ne considèrent comme une connexion entre domaines, pour l’application fonctionne localement avec Internet Explorer 10 même si vous n’avez pas activé les connexions entre domaines sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="19a2c-228">When you're connecting to a localhost URL, Internet Explorer 10 won't consider it a cross-domain connection, so the application will work locally with IE 10 even if you haven't enabled cross-domain connections on the server.</span></span>
> - <span data-ttu-id="19a2c-229">Pour plus d’informations sur l’utilisation de connexions inter-domaines avec Internet Explorer 9, consultez [ce thread StackOverflow](http://stackoverflow.com/questions/13573397/siganlr-ie9-cross-domain-request-dont-work).</span><span class="sxs-lookup"><span data-stu-id="19a2c-229">For information about using cross-domain connections with Internet Explorer 9, see [this StackOverflow thread](http://stackoverflow.com/questions/13573397/siganlr-ie9-cross-domain-request-dont-work).</span></span>
> - <span data-ttu-id="19a2c-230">Pour plus d’informations sur l’utilisation de connexions inter-domaines avec Chrome, consultez [ce thread StackOverflow](http://stackoverflow.com/questions/15467373/signalr-1-0-1-cross-domain-request-cors-with-chrome).</span><span class="sxs-lookup"><span data-stu-id="19a2c-230">For information about using cross-domain connections with Chrome, see [this StackOverflow thread](http://stackoverflow.com/questions/15467373/signalr-1-0-1-cross-domain-request-cors-with-chrome).</span></span>
> - <span data-ttu-id="19a2c-231">L’exemple de code utilise la valeur par défaut « / signalr « URL pour se connecter à votre service de SignalR.</span><span class="sxs-lookup"><span data-stu-id="19a2c-231">The sample code uses the default "/signalr" URL to connect to your SignalR service.</span></span> <span data-ttu-id="19a2c-232">Pour plus d’informations sur la façon de spécifier une autre URL de base, consultez [ASP.NET SignalR concentrateurs API Guide - Server - l’URL /signalr](../guide-to-the-api/hubs-api-guide-server.md#signalrurl).</span><span class="sxs-lookup"><span data-stu-id="19a2c-232">For information about how to specify a different base URL, see [ASP.NET SignalR Hubs API Guide - Server - The /signalr URL](../guide-to-the-api/hubs-api-guide-server.md#signalrurl).</span></span>


<a id="configureconnection"></a>

## <a name="how-to-configure-the-connection"></a><span data-ttu-id="19a2c-233">Comment configurer la connexion</span><span class="sxs-lookup"><span data-stu-id="19a2c-233">How to configure the connection</span></span>

<span data-ttu-id="19a2c-234">Avant d’établir une connexion, vous pouvez spécifier des paramètres de chaîne de requête ou spécifier le mode de transport.</span><span class="sxs-lookup"><span data-stu-id="19a2c-234">Before you establish a connection, you can specify query string parameters or specify the transport method.</span></span>

<a id="querystring"></a>

### <a name="how-to-specify-query-string-parameters"></a><span data-ttu-id="19a2c-235">Comment spécifier des paramètres de chaîne de requête</span><span class="sxs-lookup"><span data-stu-id="19a2c-235">How to specify query string parameters</span></span>

<span data-ttu-id="19a2c-236">Si vous souhaitez envoyer des données sur le serveur lorsque le client se connecte, vous pouvez ajouter des paramètres de chaîne de requête à l’objet de connexion.</span><span class="sxs-lookup"><span data-stu-id="19a2c-236">If you want to send data to the server when the client connects, you can add query string parameters to the connection object.</span></span> <span data-ttu-id="19a2c-237">Les exemples suivants montrent comment définir un paramètre de chaîne de requête dans le code client.</span><span class="sxs-lookup"><span data-stu-id="19a2c-237">The following examples show how to set a query string parameter in client code.</span></span>

<span data-ttu-id="19a2c-238">**Définir une valeur de chaîne de requête avant d’appeler la méthode start (avec le proxy généré)**</span><span class="sxs-lookup"><span data-stu-id="19a2c-238">**Set a query string value before calling the start method (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample14.js?highlight=1)]

<span data-ttu-id="19a2c-239">**Définir une valeur de chaîne de requête avant d’appeler la méthode start (sans le proxy généré)**</span><span class="sxs-lookup"><span data-stu-id="19a2c-239">**Set a query string value before calling the start method (without the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample15.js?highlight=2)]

<span data-ttu-id="19a2c-240">L’exemple suivant montre comment lire un paramètre de chaîne de requête dans le code serveur.</span><span class="sxs-lookup"><span data-stu-id="19a2c-240">The following example shows how to read a query string parameter in server code.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample16.cs?highlight=5)]

<a id="transport"></a>

### <a name="how-to-specify-the-transport-method"></a><span data-ttu-id="19a2c-241">Comment spécifier le mode de transport</span><span class="sxs-lookup"><span data-stu-id="19a2c-241">How to specify the transport method</span></span>

<span data-ttu-id="19a2c-242">Dans le cadre du processus de connexion, un client SignalR est normalement négocie avec le serveur pour déterminer le transport meilleure prise en charge par le serveur et client.</span><span class="sxs-lookup"><span data-stu-id="19a2c-242">As part of the process of connecting, a SignalR client normally negotiates with the server to determine the best transport that is supported by both server and client.</span></span> <span data-ttu-id="19a2c-243">Si vous savez déjà que vous souhaitez utiliser le transport, vous pouvez ignorer ce processus de négociation en spécifiant le mode de transport lorsque vous appelez le `start` (méthode).</span><span class="sxs-lookup"><span data-stu-id="19a2c-243">If you already know which transport you want to use, you can bypass this negotiation process by specifying the transport method when you call the `start` method.</span></span>

<span data-ttu-id="19a2c-244">**Code client qui spécifie le mode de transport (avec le proxy généré)**</span><span class="sxs-lookup"><span data-stu-id="19a2c-244">**Client code that specifies the transport method (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample17.js?highlight=1)]

<span data-ttu-id="19a2c-245">**Code client qui spécifie le mode de transport (sans le proxy généré)**</span><span class="sxs-lookup"><span data-stu-id="19a2c-245">**Client code that specifies the transport method (without the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample18.js?highlight=2)]

<span data-ttu-id="19a2c-246">En guise d’alternative, vous pouvez spécifier plusieurs méthodes de transport dans l’ordre dans lequel vous souhaitez SignalR à les essayer :</span><span class="sxs-lookup"><span data-stu-id="19a2c-246">As an alternative, you can specify multiple transport methods in the order in which you want SignalR to try them:</span></span>

<span data-ttu-id="19a2c-247">**Code client qui spécifie un schéma de secours de transport personnalisé (avec le proxy généré)**</span><span class="sxs-lookup"><span data-stu-id="19a2c-247">**Client code that specifies a custom transport fallback scheme (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample19.js?highlight=1)]

<span data-ttu-id="19a2c-248">**Code client qui spécifie un schéma de secours de transport personnalisés (sans le proxy généré)**</span><span class="sxs-lookup"><span data-stu-id="19a2c-248">**Client code that specifies a custom transport fallback scheme (without the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample20.js?highlight=2)]

<span data-ttu-id="19a2c-249">Vous pouvez utiliser les valeurs suivantes pour spécifier le mode de transport :</span><span class="sxs-lookup"><span data-stu-id="19a2c-249">You can use the following values for specifying the transport method:</span></span>

- <span data-ttu-id="19a2c-250">"webSockets"</span><span class="sxs-lookup"><span data-stu-id="19a2c-250">"webSockets"</span></span>
- <span data-ttu-id="19a2c-251">"foreverFrame"</span><span class="sxs-lookup"><span data-stu-id="19a2c-251">"foreverFrame"</span></span>
- <span data-ttu-id="19a2c-252">"serverSentEvents"</span><span class="sxs-lookup"><span data-stu-id="19a2c-252">"serverSentEvents"</span></span>
- <span data-ttu-id="19a2c-253">"longPolling"</span><span class="sxs-lookup"><span data-stu-id="19a2c-253">"longPolling"</span></span>

<span data-ttu-id="19a2c-254">Les exemples suivants montrent comment déterminer quelle méthode de transport est utilisé par une connexion.</span><span class="sxs-lookup"><span data-stu-id="19a2c-254">The following examples show how to find out which transport method is being used by a connection.</span></span>

<span data-ttu-id="19a2c-255">**Code client qui affiche le mode de transport utilisé par une connexion (avec le proxy généré)**</span><span class="sxs-lookup"><span data-stu-id="19a2c-255">**Client code that displays the transport method used by a connection (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample21.js?highlight=2)]

<span data-ttu-id="19a2c-256">**Code client qui affiche le mode de transport utilisé par une connexion (sans le proxy généré)**</span><span class="sxs-lookup"><span data-stu-id="19a2c-256">**Client code that displays the transport method used by a connection (without the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample22.js?highlight=3)]

<span data-ttu-id="19a2c-257">Pour plus d’informations sur la vérification de la méthode de transport dans le code serveur, consultez [ASP.NET SignalR concentrateurs API Guide - Server - comment obtenir des informations sur le client à partir de la propriété de contexte](../guide-to-the-api/hubs-api-guide-server.md#contextproperty).</span><span class="sxs-lookup"><span data-stu-id="19a2c-257">For information about how to check the transport method in server code, see [ASP.NET SignalR Hubs API Guide - Server - How to get information about the client from the Context property](../guide-to-the-api/hubs-api-guide-server.md#contextproperty).</span></span> <span data-ttu-id="19a2c-258">Pour plus d’informations sur les transports et de secours, consultez [Introduction à SignalR - Transports et secours](../getting-started/introduction-to-signalr.md#transports).</span><span class="sxs-lookup"><span data-stu-id="19a2c-258">For more information about transports and fallbacks, see [Introduction to SignalR - Transports and Fallbacks](../getting-started/introduction-to-signalr.md#transports).</span></span>

<a id="getproxy"></a>

## <a name="how-to-get-a-proxy-for-a-hub-class"></a><span data-ttu-id="19a2c-259">Comment obtenir un proxy pour une classe de concentrateur</span><span class="sxs-lookup"><span data-stu-id="19a2c-259">How to get a proxy for a Hub class</span></span>

<span data-ttu-id="19a2c-260">Chaque objet de connexion que vous créez encapsule des informations sur une connexion à un service de SignalR qui contient une ou plusieurs classes de concentrateur.</span><span class="sxs-lookup"><span data-stu-id="19a2c-260">Each connection object that you create encapsulates information about a connection to a SignalR service that contains one or more Hub classes.</span></span> <span data-ttu-id="19a2c-261">Pour communiquer avec une classe de concentrateur, vous utilisez un objet proxy que vous créez vous-même (si vous n’utilisez pas le proxy généré) ou qui est généré pour vous.</span><span class="sxs-lookup"><span data-stu-id="19a2c-261">To communicate with a Hub class, you use a proxy object which you create yourself (if you're not using the generated proxy) or which is generated for you.</span></span>

<span data-ttu-id="19a2c-262">Sur le client le nom du proxy est une version de casse mixte du nom de classe du concentrateur.</span><span class="sxs-lookup"><span data-stu-id="19a2c-262">On the client the proxy name is a camel-cased version of the Hub class name.</span></span> <span data-ttu-id="19a2c-263">SignalR effectue automatiquement cette modification afin que le code JavaScript peut être conforme aux conventions de JavaScript.</span><span class="sxs-lookup"><span data-stu-id="19a2c-263">SignalR automatically makes this change so that JavaScript code can conform to JavaScript conventions.</span></span>

<span data-ttu-id="19a2c-264">**Classe de concentrateur sur le serveur**</span><span class="sxs-lookup"><span data-stu-id="19a2c-264">**Hub class on server**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample23.cs?highlight=1)]

<span data-ttu-id="19a2c-265">**Obtenir une référence au proxy client généré pour le concentrateur**</span><span class="sxs-lookup"><span data-stu-id="19a2c-265">**Get a reference to the generated client proxy for the Hub**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample24.js?highlight=1)]

<span data-ttu-id="19a2c-266">**Créer le proxy client pour la classe de concentrateur (sans proxy généré)**</span><span class="sxs-lookup"><span data-stu-id="19a2c-266">**Create client proxy for the Hub class (without generated proxy)**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample25.cs?highlight=1)]

<span data-ttu-id="19a2c-267">Si vous ajoutez votre classe Hub avec un `HubName` d’attribut, utilisez le nom exact sans changement de casse.</span><span class="sxs-lookup"><span data-stu-id="19a2c-267">If you decorate your Hub class with a `HubName` attribute, use the exact name without changing case.</span></span>

<span data-ttu-id="19a2c-268">**Classe de concentrateur sur le serveur avec l’attribut de HubName**</span><span class="sxs-lookup"><span data-stu-id="19a2c-268">**Hub class on server with HubName attribute**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample26.cs?highlight=1)]

<span data-ttu-id="19a2c-269">**Obtenir une référence au proxy client généré pour le concentrateur**</span><span class="sxs-lookup"><span data-stu-id="19a2c-269">**Get a reference to the generated client proxy for the Hub**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample27.js?highlight=1)]

<span data-ttu-id="19a2c-270">**Créer le proxy client pour la classe de concentrateur (sans proxy généré)**</span><span class="sxs-lookup"><span data-stu-id="19a2c-270">**Create client proxy for the Hub class (without generated proxy)**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample28.cs?highlight=1)]

<a id="callclient"></a>

## <a name="how-to-define-methods-on-the-client-that-the-server-can-call"></a><span data-ttu-id="19a2c-271">Comment définir des méthodes sur le client que le serveur peut appeler.</span><span class="sxs-lookup"><span data-stu-id="19a2c-271">How to define methods on the client that the server can call</span></span>

<span data-ttu-id="19a2c-272">Pour définir une méthode que le serveur peut appeler à partir d’un concentrateur, ajoutez un gestionnaire d’événements pour le concentrateur proxy à l’aide de la `client` propriété du proxy généré ou appel de la `on` méthode si vous n’utilisez pas le proxy généré.</span><span class="sxs-lookup"><span data-stu-id="19a2c-272">To define a method that the server can call from a Hub, add an event handler to the Hub proxy by using the `client` property of the generated proxy, or call the `on` method if you aren't using the generated proxy.</span></span> <span data-ttu-id="19a2c-273">Les paramètres peuvent être des objets complexes.</span><span class="sxs-lookup"><span data-stu-id="19a2c-273">The parameters can be complex objects.</span></span>

<span data-ttu-id="19a2c-274">Ajouter le Gestionnaire d’événements avant d’appeler le `start` méthode pour établir la connexion.</span><span class="sxs-lookup"><span data-stu-id="19a2c-274">Add the event handler before you call the `start` method to establish the connection.</span></span> <span data-ttu-id="19a2c-275">(Si vous souhaitez ajouter des gestionnaires d’événements après avoir appelé la `start` (méthode), consultez la note de [comment établir une connexion](#establishconnection) plus haut dans ce document et utilisez la syntaxe indiquée pour la définition d’une méthode sans utiliser le proxy généré.)</span><span class="sxs-lookup"><span data-stu-id="19a2c-275">(If you want to add event handlers after calling the `start` method, see the note in [How to establish a connection](#establishconnection) earlier in this document, and use the syntax shown for defining a method without using the generated proxy.)</span></span>

<span data-ttu-id="19a2c-276">Correspondance de nom de méthode respecte la casse.</span><span class="sxs-lookup"><span data-stu-id="19a2c-276">Method name matching is case-insensitive.</span></span> <span data-ttu-id="19a2c-277">Par exemple, `Clients.All.addContosoChatMessageToPage` sur le serveur s’exécute `AddContosoChatMessageToPage`, `addContosoChatMessageToPage`, ou `addcontosochatmessagetopage` sur le client.</span><span class="sxs-lookup"><span data-stu-id="19a2c-277">For example, `Clients.All.addContosoChatMessageToPage` on the server will execute `AddContosoChatMessageToPage`, `addContosoChatMessageToPage`, or `addcontosochatmessagetopage` on the client.</span></span>

<span data-ttu-id="19a2c-278">**Définir la méthode sur le client (avec le proxy généré)**</span><span class="sxs-lookup"><span data-stu-id="19a2c-278">**Define method on client (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample29.js?highlight=2)]

<span data-ttu-id="19a2c-279">**Autre manière de définir la méthode sur le client (avec le proxy généré)**</span><span class="sxs-lookup"><span data-stu-id="19a2c-279">**Alternate way to define method on client (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample30.js?highlight=1-2)]

<span data-ttu-id="19a2c-280">**Définir la méthode sur le client (sans le proxy généré, ou lorsque vous ajoutez après avoir appelé la méthode de démarrage)**</span><span class="sxs-lookup"><span data-stu-id="19a2c-280">**Define method on client (without the generated proxy, or when adding after calling the start method)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample31.js?highlight=3)]

<span data-ttu-id="19a2c-281">**Code de serveur qui appelle la méthode du client**</span><span class="sxs-lookup"><span data-stu-id="19a2c-281">**Server code that calls the client method**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample32.cs?highlight=5)]

<span data-ttu-id="19a2c-282">Les exemples suivants incluent un objet complexe comme un paramètre de méthode.</span><span class="sxs-lookup"><span data-stu-id="19a2c-282">The following examples include a complex object as a method parameter.</span></span>

<span data-ttu-id="19a2c-283">**Définir la méthode sur le client qui prend un objet complexe (avec le proxy généré)**</span><span class="sxs-lookup"><span data-stu-id="19a2c-283">**Define method on client that takes a complex object (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample33.js?highlight=2-3)]

<span data-ttu-id="19a2c-284">**Définir la méthode sur le client qui prend un objet complexe (sans le proxy généré)**</span><span class="sxs-lookup"><span data-stu-id="19a2c-284">**Define method on client that takes a complex object (without the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample34.js?highlight=3-4)]

<span data-ttu-id="19a2c-285">**Code de serveur qui définit l’objet complexe**</span><span class="sxs-lookup"><span data-stu-id="19a2c-285">**Server code that defines the complex object**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample35.cs?highlight=1)]

<span data-ttu-id="19a2c-286">**Code de serveur qui appelle la méthode du client à l’aide d’un objet complexe**</span><span class="sxs-lookup"><span data-stu-id="19a2c-286">**Server code that calls the client method using a complex object**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample36.cs?highlight=3)]

<a id="callserver"></a>

## <a name="how-to-call-server-methods-from-the-client"></a><span data-ttu-id="19a2c-287">Comment appeler des méthodes de serveur à partir du client</span><span class="sxs-lookup"><span data-stu-id="19a2c-287">How to call server methods from the client</span></span>

<span data-ttu-id="19a2c-288">Pour appeler une méthode de serveur à partir du client, utilisez le `server` propriété du proxy généré ou `invoke` méthode sur le concentrateur proxy si vous n’utilisez pas le proxy généré.</span><span class="sxs-lookup"><span data-stu-id="19a2c-288">To call a server method from the client, use the `server` property of the generated proxy or the `invoke` method on the Hub proxy if you aren't using the generated proxy.</span></span> <span data-ttu-id="19a2c-289">La valeur de retour ou les paramètres peuvent être des objets complexes.</span><span class="sxs-lookup"><span data-stu-id="19a2c-289">The return value or parameters can be complex objects.</span></span>

<span data-ttu-id="19a2c-290">Passez dans une version de casse mixte du nom de la méthode du concentrateur.</span><span class="sxs-lookup"><span data-stu-id="19a2c-290">Pass in a camel-case version of the method name on the Hub.</span></span> <span data-ttu-id="19a2c-291">SignalR effectue automatiquement cette modification afin que le code JavaScript peut être conforme aux conventions de JavaScript.</span><span class="sxs-lookup"><span data-stu-id="19a2c-291">SignalR automatically makes this change so that JavaScript code can conform to JavaScript conventions.</span></span>

<span data-ttu-id="19a2c-292">Les exemples suivants montrent comment appeler une méthode de serveur qui n’a pas une valeur de retournée et appeler une méthode de serveur qui n’a pas une valeur de retour.</span><span class="sxs-lookup"><span data-stu-id="19a2c-292">The following examples show how to call a server method that doesn't have a return value and how to call a server method that does have a return value.</span></span>

<span data-ttu-id="19a2c-293">**Méthode de serveur sans attribut HubMethodName**</span><span class="sxs-lookup"><span data-stu-id="19a2c-293">**Server method with no HubMethodName attribute**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample37.cs?highlight=3)]

<span data-ttu-id="19a2c-294">**Code de serveur qui définit l’objet complexe passées dans un paramètre**</span><span class="sxs-lookup"><span data-stu-id="19a2c-294">**Server code that defines the complex object passed in a parameter**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample38.cs)]

<span data-ttu-id="19a2c-295">**Code client qui appelle la méthode de serveur (avec le proxy généré)**</span><span class="sxs-lookup"><span data-stu-id="19a2c-295">**Client code that invokes the server method (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample39.js?highlight=1)]

<span data-ttu-id="19a2c-296">**Code client qui appelle la méthode de serveur (sans le proxy généré)**</span><span class="sxs-lookup"><span data-stu-id="19a2c-296">**Client code that invokes the server method (without the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample40.js?highlight=1)]

<span data-ttu-id="19a2c-297">Si vous décorée avec la méthode de concentrateur un `HubMethodName` d’attribut, utilisez ce nom sans changement de casse.</span><span class="sxs-lookup"><span data-stu-id="19a2c-297">If you decorated the Hub method with a `HubMethodName` attribute, use that name without changing case.</span></span>

<span data-ttu-id="19a2c-298">**Méthode serveur** avec un attribut HubMethodName</span><span class="sxs-lookup"><span data-stu-id="19a2c-298">**Server method** with a HubMethodName attribute</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample41.cs?highlight=3)]

<span data-ttu-id="19a2c-299">**Code client qui appelle la méthode de serveur (avec le proxy généré)**</span><span class="sxs-lookup"><span data-stu-id="19a2c-299">**Client code that invokes the server method (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample42.js?highlight=1)]

<span data-ttu-id="19a2c-300">**Code client qui appelle la méthode de serveur (sans le proxy généré)**</span><span class="sxs-lookup"><span data-stu-id="19a2c-300">**Client code that invokes the server method (without the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample43.js?highlight=1)]

<span data-ttu-id="19a2c-301">Les exemples suivants montrent comment appeler une méthode de serveur qui n’a aucune valeur de retour.</span><span class="sxs-lookup"><span data-stu-id="19a2c-301">The preceding examples show how to call a server method that has no return value.</span></span> <span data-ttu-id="19a2c-302">Les exemples suivants montrent comment appeler une méthode de serveur qui a une valeur de retour.</span><span class="sxs-lookup"><span data-stu-id="19a2c-302">The following examples show how to call a server method that has a return value.</span></span>

<span data-ttu-id="19a2c-303">**Code de serveur pour une méthode qui a une valeur de retour**</span><span class="sxs-lookup"><span data-stu-id="19a2c-303">**Server code for a method that has a return value**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample44.cs?highlight=3)]

<span data-ttu-id="19a2c-304">**La classe de Stock utilisée pour le** valeur de retour</span><span class="sxs-lookup"><span data-stu-id="19a2c-304">**The Stock class used for the** return value</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample45.cs?highlight=1)]

<span data-ttu-id="19a2c-305">**Code client qui appelle la méthode de serveur (avec le proxy généré)**</span><span class="sxs-lookup"><span data-stu-id="19a2c-305">**Client code that invokes the server method (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample46.js?highlight=2,4-5)]

<span data-ttu-id="19a2c-306">**Code client qui appelle la méthode de serveur (sans le proxy généré)**</span><span class="sxs-lookup"><span data-stu-id="19a2c-306">**Client code that invokes the server method (without the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample47.js?highlight=2,4-5)]

<a id="connectionlifetime"></a>

## <a name="how-to-handle-connection-lifetime-events"></a><span data-ttu-id="19a2c-307">Comment gérer les événements de durée de vie de connexion</span><span class="sxs-lookup"><span data-stu-id="19a2c-307">How to handle connection lifetime events</span></span>

<span data-ttu-id="19a2c-308">SignalR fournit des événements de durée de vie que vous pouvez gérer la connexion suivante :</span><span class="sxs-lookup"><span data-stu-id="19a2c-308">SignalR provides the following connection lifetime events that you can handle:</span></span>

- <span data-ttu-id="19a2c-309">`starting`: Déclenché avant l’envoi des données via la connexion.</span><span class="sxs-lookup"><span data-stu-id="19a2c-309">`starting`: Raised before any data is sent over the connection.</span></span>
- <span data-ttu-id="19a2c-310">`received`: Déclenché lorsque des données sont reçues sur la connexion.</span><span class="sxs-lookup"><span data-stu-id="19a2c-310">`received`: Raised when any data is received on the connection.</span></span> <span data-ttu-id="19a2c-311">Fournit les données reçues.</span><span class="sxs-lookup"><span data-stu-id="19a2c-311">Provides the received data.</span></span>
- <span data-ttu-id="19a2c-312">`connectionSlow`: Déclenché lorsque le client détecte une connexion lente ou suppression fréquemment.</span><span class="sxs-lookup"><span data-stu-id="19a2c-312">`connectionSlow`: Raised when the client detects a slow or frequently dropping connection.</span></span>
- <span data-ttu-id="19a2c-313">`reconnecting`: Déclenché lorsque le transport sous-jacent commence la reconnexion.</span><span class="sxs-lookup"><span data-stu-id="19a2c-313">`reconnecting`: Raised when the underlying transport begins reconnecting.</span></span>
- <span data-ttu-id="19a2c-314">`reconnected`: Déclenché lorsque le transport sous-jacent s’est reconnecté.</span><span class="sxs-lookup"><span data-stu-id="19a2c-314">`reconnected`: Raised when the underlying transport has reconnected.</span></span>
- <span data-ttu-id="19a2c-315">`stateChanged`: Déclenché lorsque l’état de la connexion change.</span><span class="sxs-lookup"><span data-stu-id="19a2c-315">`stateChanged`: Raised when the connection state changes.</span></span> <span data-ttu-id="19a2c-316">Fournit l’ancien état et le nouvel état (connexion, connecté, reconnexion ou Disconnected).</span><span class="sxs-lookup"><span data-stu-id="19a2c-316">Provides the old state and the new state (Connecting, Connected, Reconnecting, or Disconnected).</span></span>
- <span data-ttu-id="19a2c-317">`disconnected`: Déclenché lors de la connexion s’est déconnecté.</span><span class="sxs-lookup"><span data-stu-id="19a2c-317">`disconnected`: Raised when the connection has disconnected.</span></span>

<span data-ttu-id="19a2c-318">Par exemple, si vous souhaitez afficher les messages d’avertissement lorsqu’il existe des problèmes de connexion qui peuvent entraîner des retards notables, gérer les `connectionSlow` événement.</span><span class="sxs-lookup"><span data-stu-id="19a2c-318">For example, if you want to display warning messages when there are connection problems that might cause noticeable delays, handle the `connectionSlow` event.</span></span>

<span data-ttu-id="19a2c-319">**Gérer l’événement connectionSlow (avec le proxy généré)**</span><span class="sxs-lookup"><span data-stu-id="19a2c-319">**Handle the connectionSlow event (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample48.js?highlight=1)]

<span data-ttu-id="19a2c-320">**Gérer l’événement connectionSlow (sans le proxy généré)**</span><span class="sxs-lookup"><span data-stu-id="19a2c-320">**Handle the connectionSlow event (without the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample49.js?highlight=2)]

<span data-ttu-id="19a2c-321">Pour plus d’informations, consultez [compréhension et gestion des événements de durée de vie de connexion dans SignalR](index.md).</span><span class="sxs-lookup"><span data-stu-id="19a2c-321">For more information, see [Understanding and Handling Connection Lifetime Events in SignalR](index.md).</span></span>

<a id="handleerrors"></a>

## <a name="how-to-handle-errors"></a><span data-ttu-id="19a2c-322">Comment gérer les erreurs</span><span class="sxs-lookup"><span data-stu-id="19a2c-322">How to handle errors</span></span>

<span data-ttu-id="19a2c-323">Le client SignalR JavaScript fournit un `error` événement que vous pouvez ajouter un gestionnaire pour.</span><span class="sxs-lookup"><span data-stu-id="19a2c-323">The SignalR JavaScript client provides an `error` event that you can add a handler for.</span></span> <span data-ttu-id="19a2c-324">Vous pouvez également utiliser la méthode fail pour ajouter un gestionnaire d’erreurs qui résultent d’un appel de méthode de serveur.</span><span class="sxs-lookup"><span data-stu-id="19a2c-324">You can also use the fail method to add a handler for errors that result from a server method invocation.</span></span>

<span data-ttu-id="19a2c-325">Si vous n’activez pas explicitement des messages d’erreur détaillés sur le serveur, l’objet exception qui SignalR renvoie une erreur et contient un minimum d’informations sur l’erreur.</span><span class="sxs-lookup"><span data-stu-id="19a2c-325">If you don't explicitly enable detailed error messages on the server, the exception object that SignalR returns after an error contains minimal information about the error.</span></span> <span data-ttu-id="19a2c-326">Par exemple, si un appel à `newContosoChatMessage` échoue, le message d’erreur dans l’objet d’erreur contient «`There was an error invoking Hub method 'contosoChatHub.newContosoChatMessage'.`« envoi de messages d’erreur détaillés pour les clients en production n’est pas recommandé pour des raisons de sécurité, mais si vous souhaitez activer les messages d’erreur détaillés pour à des fins de résolution des problèmes, utilisez le code suivant sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="19a2c-326">For example, if a call to `newContosoChatMessage` fails, the error message in the error object contains "`There was an error invoking Hub method 'contosoChatHub.newContosoChatMessage'.`" Sending detailed error messages to clients in production is not recommended for security reasons, but if you want to enable detailed error messages for troubleshooting purposes, use the following code on the server.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample50.cs?highlight=2)]

<span data-ttu-id="19a2c-327">L’exemple suivant montre comment ajouter un gestionnaire pour l’événement d’erreur.</span><span class="sxs-lookup"><span data-stu-id="19a2c-327">The following example shows how to add a handler for the error event.</span></span>

<span data-ttu-id="19a2c-328">**Ajouter un gestionnaire d’erreurs (avec le proxy généré)**</span><span class="sxs-lookup"><span data-stu-id="19a2c-328">**Add an error handler (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample51.js?highlight=1)]

<span data-ttu-id="19a2c-329">**Ajouter un gestionnaire d’erreurs (sans le proxy généré)**</span><span class="sxs-lookup"><span data-stu-id="19a2c-329">**Add an error handler (without the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample52.js?highlight=2)]

<span data-ttu-id="19a2c-330">L’exemple suivant montre comment gérer une erreur à partir d’un appel de méthode.</span><span class="sxs-lookup"><span data-stu-id="19a2c-330">The following example shows how to handle an error from a method invocation.</span></span>

<span data-ttu-id="19a2c-331">**Gérer une erreur à partir d’un appel de méthode (avec le proxy généré)**</span><span class="sxs-lookup"><span data-stu-id="19a2c-331">**Handle an error from a method invocation (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample53.js?highlight=2)]

<span data-ttu-id="19a2c-332">**Gérer une erreur à partir d’un appel de méthode (sans le proxy généré)**</span><span class="sxs-lookup"><span data-stu-id="19a2c-332">**Handle an error from a method invocation (without the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample54.js?highlight=2)]

<span data-ttu-id="19a2c-333">Si un appel de méthode échoue, le `error` événement est également déclenché, de sorte que votre code dans le `error` Gestionnaire de méthode et dans le `.fail` rappel de la méthode s’exécute.</span><span class="sxs-lookup"><span data-stu-id="19a2c-333">If a method invocation fails, the `error` event is also raised, so your code in the `error` method handler and in the `.fail` method callback would execute.</span></span>

<a id="logging"></a>

## <a name="how-to-enable-client-side-logging"></a><span data-ttu-id="19a2c-334">Comment activer la journalisation côté client</span><span class="sxs-lookup"><span data-stu-id="19a2c-334">How to enable client-side logging</span></span>

<span data-ttu-id="19a2c-335">Pour activer la journalisation de côté client sur une connexion, définissez la `logging` propriété sur l’objet de connexion avant d’appeler le `start` méthode pour établir la connexion.</span><span class="sxs-lookup"><span data-stu-id="19a2c-335">To enable client-side logging on a connection, set the `logging` property on the connection object before you call the `start` method to establish the connection.</span></span>

<span data-ttu-id="19a2c-336">**Activer la journalisation (avec le proxy généré)**</span><span class="sxs-lookup"><span data-stu-id="19a2c-336">**Enable logging (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample55.js?highlight=1)]

<span data-ttu-id="19a2c-337">**Activer la journalisation (sans le proxy généré)**</span><span class="sxs-lookup"><span data-stu-id="19a2c-337">**Enable logging (without the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample56.js?highlight=2)]

<span data-ttu-id="19a2c-338">Pour afficher les journaux, ouvrez les outils de développement de votre navigateur et accédez à l’onglet de la Console. Pour obtenir un didacticiel qui montre des instructions détaillées et écran de captures qui montrent comment effectuer cette opération, consultez [avec ASP.NET Signalr - activer la journalisation de diffusion serveur](index.md).</span><span class="sxs-lookup"><span data-stu-id="19a2c-338">To see the logs, open your browser's developer tools and go to the Console tab. For a tutorial that shows step-by-step instructions and screen shots that show how to do this, see [Server Broadcast with ASP.NET Signalr - Enable Logging](index.md).</span></span>
