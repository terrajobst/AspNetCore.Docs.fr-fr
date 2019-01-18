---
uid: signalr/overview/guide-to-the-api/hubs-api-guide-javascript-client
title: Guide de l’API ASP.NET SignalR Hubs - Client JavaScript | Microsoft Docs
author: pfletcher
description: Ce document fournit une introduction à l’utilisation de l’API de Hubs pour SignalR version 2 dans les clients JavaScript, telles que les navigateurs et en cours de Windows Store (WinJS)...
ms.author: riande
ms.date: 01/15/2019
ms.assetid: a9fd4dc0-1b96-4443-82ca-932a5b4a8ea4
msc.legacyurl: /signalr/overview/guide-to-the-api/hubs-api-guide-javascript-client
msc.type: authoredcontent
ms.openlocfilehash: 12d675b6a2f2f6acdd8c3a5d0d27b5ad2fb1efc4
ms.sourcegitcommit: 184ba5b44d1c393076015510ac842b77bc9d4d93
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/18/2019
ms.locfileid: "54396309"
---
<a name="aspnet-signalr-hubs-api-guide---javascript-client"></a><span data-ttu-id="ea510-103">Guide de l’API ASP.NET SignalR Hubs - Client JavaScript</span><span class="sxs-lookup"><span data-stu-id="ea510-103">ASP.NET SignalR Hubs API Guide - JavaScript Client</span></span>
====================

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> <span data-ttu-id="ea510-104">Ce document fournit une introduction à l’utilisation de l’API de Hubs pour SignalR version 2 dans les clients JavaScript, telles que des navigateurs et des applications du Windows Store (WinJS).</span><span class="sxs-lookup"><span data-stu-id="ea510-104">This document provides an introduction to using the Hubs API for SignalR version 2 in JavaScript clients, such as browsers and Windows Store (WinJS) applications.</span></span>
>
> <span data-ttu-id="ea510-105">L’API de concentrateurs SignalR vous permet de vous permettent d’effectuer des appels de procédure distante (RPC) à partir d’un serveur aux clients connectés et à partir de clients sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="ea510-105">The SignalR Hubs API enables you to make remote procedure calls (RPCs) from a server to connected clients and from clients to the server.</span></span> <span data-ttu-id="ea510-106">Dans le code serveur, vous définissez des méthodes qui peuvent être appelées par les clients, et vous appelez des méthodes qui s’exécutent sur le client.</span><span class="sxs-lookup"><span data-stu-id="ea510-106">In server code, you define methods that can be called by clients, and you call methods that run on the client.</span></span> <span data-ttu-id="ea510-107">Dans le code client, vous définissez des méthodes qui peuvent être appelées à partir du serveur, et vous appelez des méthodes qui s’exécutent sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="ea510-107">In client code, you define methods that can be called from the server, and you call methods that run on the server.</span></span> <span data-ttu-id="ea510-108">SignalR s’occupe de tous les éléments client-serveur pour vous.</span><span class="sxs-lookup"><span data-stu-id="ea510-108">SignalR takes care of all of the client-to-server plumbing for you.</span></span>
>
> <span data-ttu-id="ea510-109">SignalR offre également une API de niveau inférieur appelée connexions persistantes.</span><span class="sxs-lookup"><span data-stu-id="ea510-109">SignalR also offers a lower-level API called Persistent Connections.</span></span> <span data-ttu-id="ea510-110">Pour une introduction à SignalR Hubs et connexions persistantes, consultez [Introduction à SignalR](../getting-started/introduction-to-signalr.md).</span><span class="sxs-lookup"><span data-stu-id="ea510-110">For an introduction to SignalR, Hubs, and Persistent Connections, see [Introduction to SignalR](../getting-started/introduction-to-signalr.md).</span></span>
>
> ## <a name="software-versions-used-in-this-topic"></a><span data-ttu-id="ea510-111">Versions des logiciels utilisées dans cette rubrique</span><span class="sxs-lookup"><span data-stu-id="ea510-111">Software versions used in this topic</span></span>
>
>
> - [<span data-ttu-id="ea510-112">Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="ea510-112">Visual Studio 2017</span></span>](https://visualstudio.microsoft.com/downloads/)
> - <span data-ttu-id="ea510-113">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="ea510-113">.NET 4.5</span></span>
> - <span data-ttu-id="ea510-114">SignalR version 2</span><span class="sxs-lookup"><span data-stu-id="ea510-114">SignalR version 2</span></span>
>
>
>
> ## <a name="previous-versions-of-this-topic"></a><span data-ttu-id="ea510-115">Versions précédentes de cette rubrique</span><span class="sxs-lookup"><span data-stu-id="ea510-115">Previous versions of this topic</span></span>
>
> <span data-ttu-id="ea510-116">Pour plus d’informations sur les versions antérieures de SignalR, consultez [les Versions antérieures de SignalR](../older-versions/index.md).</span><span class="sxs-lookup"><span data-stu-id="ea510-116">For information about earlier versions of SignalR, see [SignalR Older Versions](../older-versions/index.md).</span></span>
>
> ## <a name="questions-and-comments"></a><span data-ttu-id="ea510-117">Questions et commentaires</span><span class="sxs-lookup"><span data-stu-id="ea510-117">Questions and comments</span></span>
>
> <span data-ttu-id="ea510-118">Veuillez laisser des commentaires sur la façon dont vous avez apprécié ce didacticiel et ce que nous pouvions améliorer dans les commentaires en bas de la page.</span><span class="sxs-lookup"><span data-stu-id="ea510-118">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="ea510-119">Si vous avez des questions qui ne sont pas directement liées à ce didacticiel, vous pouvez les publier à le [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) ou [StackOverflow.com](http://stackoverflow.com/).</span><span class="sxs-lookup"><span data-stu-id="ea510-119">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>

## <a name="overview"></a><span data-ttu-id="ea510-120">Vue d'ensemble</span><span class="sxs-lookup"><span data-stu-id="ea510-120">Overview</span></span>

<span data-ttu-id="ea510-121">Ce document contient les sections suivantes :</span><span class="sxs-lookup"><span data-stu-id="ea510-121">This document contains the following sections:</span></span>

- [<span data-ttu-id="ea510-122">Le proxy généré et ce qu’il fait pour vous</span><span class="sxs-lookup"><span data-stu-id="ea510-122">The generated proxy and what it does for you</span></span>](#genproxy)

    - [<span data-ttu-id="ea510-123">Quand utiliser le proxy généré</span><span class="sxs-lookup"><span data-stu-id="ea510-123">When to use the generated proxy</span></span>](#cantusegenproxy)
- [<span data-ttu-id="ea510-124">Programme d’installation du client</span><span class="sxs-lookup"><span data-stu-id="ea510-124">Client Setup</span></span>](#clientsetup)

    - [<span data-ttu-id="ea510-125">Comment font référence au proxy généré de manière dynamique</span><span class="sxs-lookup"><span data-stu-id="ea510-125">How to reference the dynamically generated proxy</span></span>](#dynamicproxy)
    - [<span data-ttu-id="ea510-126">Comment créer un fichier physique pour SignalR de proxy généré</span><span class="sxs-lookup"><span data-stu-id="ea510-126">How to create a physical file for the SignalR generated proxy</span></span>](#manualproxy)
- [<span data-ttu-id="ea510-127">Comment établir une connexion</span><span class="sxs-lookup"><span data-stu-id="ea510-127">How to establish a connection</span></span>](#establishconnection)

    - [<span data-ttu-id="ea510-128">$. connection.hub est le même objet crée ce $.hubConnection()</span><span class="sxs-lookup"><span data-stu-id="ea510-128">$.connection.hub is the same object that $.hubConnection() creates</span></span>](#connequivalence)
    - [<span data-ttu-id="ea510-129">Exécution asynchrone de la méthode start</span><span class="sxs-lookup"><span data-stu-id="ea510-129">Asynchronous execution of the start method</span></span>](#asyncstart)
- [<span data-ttu-id="ea510-130">Comment établir une connexion entre domaines</span><span class="sxs-lookup"><span data-stu-id="ea510-130">How to establish a cross-domain connection</span></span>](#crossdomain)
- [<span data-ttu-id="ea510-131">Comment configurer la connexion</span><span class="sxs-lookup"><span data-stu-id="ea510-131">How to configure the connection</span></span>](#configureconnection)

    - [<span data-ttu-id="ea510-132">Comment spécifier des paramètres de chaîne de requête</span><span class="sxs-lookup"><span data-stu-id="ea510-132">How to specify query string parameters</span></span>](#querystring)
    - [<span data-ttu-id="ea510-133">Comment spécifier le mode de transport</span><span class="sxs-lookup"><span data-stu-id="ea510-133">How to specify the transport method</span></span>](#transport)
- [<span data-ttu-id="ea510-134">Comment obtenir un proxy pour une classe de concentrateur</span><span class="sxs-lookup"><span data-stu-id="ea510-134">How to get a proxy for a Hub class</span></span>](#getproxy)
- [<span data-ttu-id="ea510-135">Comment définir des méthodes sur le client que le serveur peut appeler.</span><span class="sxs-lookup"><span data-stu-id="ea510-135">How to define methods on the client that the server can call</span></span>](#callclient)
- [<span data-ttu-id="ea510-136">Comment appeler des méthodes de serveur à partir du client</span><span class="sxs-lookup"><span data-stu-id="ea510-136">How to call server methods from the client</span></span>](#callserver)
- [<span data-ttu-id="ea510-137">Comment gérer les événements de durée de vie de connexion</span><span class="sxs-lookup"><span data-stu-id="ea510-137">How to handle connection lifetime events</span></span>](#connectionlifetime)
- [<span data-ttu-id="ea510-138">Comment gérer les erreurs</span><span class="sxs-lookup"><span data-stu-id="ea510-138">How to handle errors</span></span>](#handleerrors)
- [<span data-ttu-id="ea510-139">Comment activer la journalisation côté client</span><span class="sxs-lookup"><span data-stu-id="ea510-139">How to enable client-side logging</span></span>](#logging)

<span data-ttu-id="ea510-140">Pour obtenir une documentation sur la façon de programmer le serveur ou les clients .NET, consultez les ressources suivantes :</span><span class="sxs-lookup"><span data-stu-id="ea510-140">For documentation on how to program the server or .NET clients, see the following resources:</span></span>

- [<span data-ttu-id="ea510-141">Guide de l’API SignalR Hubs - serveur</span><span class="sxs-lookup"><span data-stu-id="ea510-141">SignalR Hubs API Guide - Server</span></span>](hubs-api-guide-server.md)
- [<span data-ttu-id="ea510-142">Guide de l’API SignalR Hubs - Client .NET</span><span class="sxs-lookup"><span data-stu-id="ea510-142">SignalR Hubs API Guide - .NET Client</span></span>](hubs-api-guide-net-client.md)

<span data-ttu-id="ea510-143">Le composant de serveur SignalR 2 est uniquement disponible sur .NET 4.5 (s’il existe un client .NET pour SignalR 2 sur .NET 4.0).</span><span class="sxs-lookup"><span data-stu-id="ea510-143">The SignalR 2 server component is only available on .NET 4.5 (though there is a .NET client for SignalR 2 on .NET 4.0).</span></span>

<a id="genproxy"></a>

## <a name="the-generated-proxy-and-what-it-does-for-you"></a><span data-ttu-id="ea510-144">Le proxy généré et ce qu’il fait pour vous</span><span class="sxs-lookup"><span data-stu-id="ea510-144">The generated proxy and what it does for you</span></span>

<span data-ttu-id="ea510-145">Vous pouvez programmer un client JavaScript pour communiquer avec un service de SignalR avec ou sans un proxy qui génère de SignalR pour vous.</span><span class="sxs-lookup"><span data-stu-id="ea510-145">You can program a JavaScript client to communicate with a SignalR service with or without a proxy that SignalR generates for you.</span></span> <span data-ttu-id="ea510-146">Ce que fait le serveur proxy pour vous est de simplifier la syntaxe du code que vous utilisez pour vous connecter, les méthodes d’écriture que le serveur appelle, et appelez des méthodes sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="ea510-146">What the proxy does for you is simplify the syntax of the code you use to connect, write methods that the server calls, and call methods on the server.</span></span>

<span data-ttu-id="ea510-147">Lorsque vous écrivez du code pour appeler des méthodes de serveur, le proxy généré vous permet d’utiliser la syntaxe semble que vous exécutaient une fonction locale : vous pouvez écrire `serverMethod(arg1, arg2)` au lieu de `invoke('serverMethod', arg1, arg2)`.</span><span class="sxs-lookup"><span data-stu-id="ea510-147">When you write code to call server methods, the generated proxy enables you to use syntax that looks as though you were executing a local function: you can write `serverMethod(arg1, arg2)` instead of `invoke('serverMethod', arg1, arg2)`.</span></span> <span data-ttu-id="ea510-148">La syntaxe de proxy généré permet également une erreur côté client immédiate et intelligible si vous orthographiez mal un nom de méthode de serveur.</span><span class="sxs-lookup"><span data-stu-id="ea510-148">The generated proxy syntax also enables an immediate and intelligible client-side error if you mistype a server method name.</span></span> <span data-ttu-id="ea510-149">Et si vous créez manuellement le fichier qui définit les serveurs proxy, vous pouvez également obtenir prise en charge IntelliSense pour l’écriture de code qui appelle des méthodes de serveur.</span><span class="sxs-lookup"><span data-stu-id="ea510-149">And if you manually create the file that defines the proxies, you can also get IntelliSense support for writing code that calls server methods.</span></span>

<span data-ttu-id="ea510-150">Par exemple, supposons que vous disposez de la classe de concentrateur suivante sur le serveur :</span><span class="sxs-lookup"><span data-stu-id="ea510-150">For example, suppose you have the following Hub class on the server:</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample1.cs?highlight=1,3,5)]

<span data-ttu-id="ea510-151">Les exemples de code suivants montrent quoi JavaScript code ressemble pour appeler le `NewContosoChatMessage` méthode sur le serveur et la réception des appels de la `addContosoChatMessageToPage` méthode à partir du serveur.</span><span class="sxs-lookup"><span data-stu-id="ea510-151">The following code examples show what JavaScript code looks like for invoking the `NewContosoChatMessage` method on the server and receiving invocations of the `addContosoChatMessageToPage` method from the server.</span></span>

<span data-ttu-id="ea510-152">**Avec le proxy généré**</span><span class="sxs-lookup"><span data-stu-id="ea510-152">**With the generated proxy**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample2.js?highlight=1-2,8)]

<span data-ttu-id="ea510-153">**Sans le proxy généré**</span><span class="sxs-lookup"><span data-stu-id="ea510-153">**Without the generated proxy**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample3.js?highlight=2-3,9)]

<a id="cantusegenproxy"></a>

### <a name="when-to-use-the-generated-proxy"></a><span data-ttu-id="ea510-154">Quand utiliser le proxy généré</span><span class="sxs-lookup"><span data-stu-id="ea510-154">When to use the generated proxy</span></span>

<span data-ttu-id="ea510-155">Si vous souhaitez inscrire plusieurs gestionnaires d’événements pour une méthode de client qui appelle le serveur, vous ne pouvez pas utiliser le proxy généré.</span><span class="sxs-lookup"><span data-stu-id="ea510-155">If you want to register multiple event handlers for a client method that the server calls, you can't use the generated proxy.</span></span> <span data-ttu-id="ea510-156">Sinon, vous pouvez choisir d’utiliser le proxy généré ou pas selon votre préférence de codage.</span><span class="sxs-lookup"><span data-stu-id="ea510-156">Otherwise, you can choose to use the generated proxy or not based on your coding preference.</span></span> <span data-ttu-id="ea510-157">Si vous choisissez de ne pas l’utiliser, vous n’êtes pas obligé de référencer l’URL « signalr/hubs » dans un `script` élément dans votre code client.</span><span class="sxs-lookup"><span data-stu-id="ea510-157">If you choose not to use it, you don't have to reference the "signalr/hubs" URL in a `script` element in your client code.</span></span>

<a id="clientsetup"></a>

## <a name="client-setup"></a><span data-ttu-id="ea510-158">Programme d’installation du client</span><span class="sxs-lookup"><span data-stu-id="ea510-158">Client setup</span></span>

<span data-ttu-id="ea510-159">Un client JavaScript nécessite des références aux jQuery et le fichier de JavaScript SignalR core.</span><span class="sxs-lookup"><span data-stu-id="ea510-159">A JavaScript client requires references to jQuery and the SignalR core JavaScript file.</span></span> <span data-ttu-id="ea510-160">La version de jQuery doit être 1.6.4 ou les versions ultérieures principales, telles que 1.7.2, 1.8.2 ou 1.9.1.</span><span class="sxs-lookup"><span data-stu-id="ea510-160">The jQuery version must be 1.6.4 or major later versions, such as 1.7.2, 1.8.2, or 1.9.1.</span></span> <span data-ttu-id="ea510-161">Si vous décidez d’utiliser le proxy généré, vous devez également une référence au proxy SignalR généré fichier JavaScript.</span><span class="sxs-lookup"><span data-stu-id="ea510-161">If you decide to use the generated proxy, you also need a reference to the SignalR generated proxy JavaScript file.</span></span> <span data-ttu-id="ea510-162">L’exemple suivant montre ce que les références peut se présenter comme dans une page HTML qui utilise le proxy généré.</span><span class="sxs-lookup"><span data-stu-id="ea510-162">The following example shows what the references might look like in an HTML page that uses the generated proxy.</span></span>

[!code-html[Main](hubs-api-guide-javascript-client/samples/sample4.html)]

<span data-ttu-id="ea510-163">Ces références doivent être inclus dans cet ordre : jQuery, SignalR core après cela et les proxys de SignalR prénom.</span><span class="sxs-lookup"><span data-stu-id="ea510-163">These references must be included in this order: jQuery first, SignalR core after that, and SignalR proxies last.</span></span>

<a id="dynamicproxy"></a>

### <a name="how-to-reference-the-dynamically-generated-proxy"></a><span data-ttu-id="ea510-164">Comment font référence au proxy généré de manière dynamique</span><span class="sxs-lookup"><span data-stu-id="ea510-164">How to reference the dynamically generated proxy</span></span>

<span data-ttu-id="ea510-165">Dans l’exemple précédent, la référence au proxy SignalR généré est au code JavaScript généré dynamiquement, pas à un fichier physique.</span><span class="sxs-lookup"><span data-stu-id="ea510-165">In the preceding example, the reference to the SignalR generated proxy is to dynamically generated JavaScript code, not to a physical file.</span></span> <span data-ttu-id="ea510-166">SignalR crée le code JavaScript pour le proxy à la volée et il sert au client en réponse à l’URL « / signalr hubs ».</span><span class="sxs-lookup"><span data-stu-id="ea510-166">SignalR creates the JavaScript code for the proxy on the fly and serves it to the client in response to the "/signalr/hubs" URL.</span></span> <span data-ttu-id="ea510-167">Si vous avez spécifié une autre URL de base pour les connexions SignalR sur le serveur dans votre `MapSignalR` (méthode), l’URL du fichier proxy généré dynamiquement est votre URL personnalisée avec « / hubs » est ajoutée.</span><span class="sxs-lookup"><span data-stu-id="ea510-167">If you specified a different base URL for SignalR connections on the server in your `MapSignalR` method, the URL for the dynamically generated proxy file is your custom URL with "/hubs" appended to it.</span></span>

> [!NOTE]
> <span data-ttu-id="ea510-168">Pour les clients Windows 8 (Windows Store) JavaScript, utilisez le fichier de proxy physique au lieu de celle générée dynamiquement.</span><span class="sxs-lookup"><span data-stu-id="ea510-168">For Windows 8 (Windows Store) JavaScript clients, use the physical proxy file instead of the dynamically generated one.</span></span> <span data-ttu-id="ea510-169">Pour plus d’informations, consultez [proxy généré de la création d’un fichier physique pour SignalR](#manualproxy) plus loin dans cette rubrique.</span><span class="sxs-lookup"><span data-stu-id="ea510-169">For more information, see [How to create a physical file for the SignalR generated proxy](#manualproxy) later in this topic.</span></span>


<span data-ttu-id="ea510-170">En un ASP.NET MVC 4 ou 5 Razor, utilisez le signe tilde pour faire référence à la racine de l’application dans votre référence de fichier proxy :</span><span class="sxs-lookup"><span data-stu-id="ea510-170">In an ASP.NET MVC 4 or 5 Razor view, use the tilde to refer to the application root in your proxy file reference:</span></span>

[!code-html[Main](hubs-api-guide-javascript-client/samples/sample5.html)]

<span data-ttu-id="ea510-171">Pour plus d’informations sur l’utilisation de SignalR dans MVC 5, consultez [bien démarrer avec SignalR et MVC 5](../getting-started/tutorial-getting-started-with-signalr-and-mvc.md).</span><span class="sxs-lookup"><span data-stu-id="ea510-171">For more information about using SignalR in MVC 5, see [Getting Started with SignalR and MVC 5](../getting-started/tutorial-getting-started-with-signalr-and-mvc.md).</span></span>

<span data-ttu-id="ea510-172">Dans une vue ASP.NET MVC 3 Razor, utilisez `Url.Content` pour votre référence de fichier proxy :</span><span class="sxs-lookup"><span data-stu-id="ea510-172">In an ASP.NET MVC 3 Razor view, use `Url.Content` for your proxy file reference:</span></span>

[!code-cshtml[Main](hubs-api-guide-javascript-client/samples/sample6.cshtml)]

<span data-ttu-id="ea510-173">Dans une application ASP.NET Web Forms, utilisez `ResolveClientUrl` pour les proxys de référence de fichier ou s’inscrire par le biais de ScriptManager à l’aide d’une application racine chemin d’accès relatif (commençant par un tilde) :</span><span class="sxs-lookup"><span data-stu-id="ea510-173">In an ASP.NET Web Forms application, use `ResolveClientUrl` for your proxies file reference or register it via the ScriptManager using an app root relative path (starting with a tilde):</span></span>

[!code-aspx[Main](hubs-api-guide-javascript-client/samples/sample7.aspx)]

<span data-ttu-id="ea510-174">En règle générale, utilisez la même méthode pour spécifier l’URL « / signalr hubs » que vous utilisez pour les fichiers CSS ou JavaScript.</span><span class="sxs-lookup"><span data-stu-id="ea510-174">As a general rule, use the same method for specifying the "/signalr/hubs" URL that you use for CSS or JavaScript files.</span></span> <span data-ttu-id="ea510-175">Si vous spécifiez une URL sans utiliser un tilde, dans certains scénarios de votre application fonctionnera correctement quand vous testez dans Visual Studio à l’aide d’IIS Express, mais échoue avec une erreur 404 lorsque vous déployez vers IIS complet.</span><span class="sxs-lookup"><span data-stu-id="ea510-175">If you specify a URL without using a tilde, in some scenarios your application will work correctly when you test in Visual Studio using IIS Express but will fail with a 404 error when you deploy to full IIS.</span></span> <span data-ttu-id="ea510-176">Pour plus d’informations, consultez **résolution des références aux ressources au niveau racine** dans [serveurs Web dans Visual Studio pour les projets Web ASP.NET](https://msdn.microsoft.com/library/58wxa9w5.aspx) sur le site MSDN.</span><span class="sxs-lookup"><span data-stu-id="ea510-176">For more information, see **Resolving References to Root-Level Resources** in [Web Servers in Visual Studio for ASP.NET Web Projects](https://msdn.microsoft.com/library/58wxa9w5.aspx) on the MSDN site.</span></span>

<span data-ttu-id="ea510-177">Lorsque vous exécutez un projet web dans Visual Studio 2017 en mode débogage, et si vous utilisez Internet Explorer comme votre navigateur, vous pouvez voir le fichier de proxy dans **l’Explorateur de solutions** sous **Scripts**.</span><span class="sxs-lookup"><span data-stu-id="ea510-177">When you run a web project in Visual Studio 2017 in debug mode, and if you use Internet Explorer as your browser, you can see the proxy file in **Solution Explorer** under **Scripts**.</span></span>

<span data-ttu-id="ea510-178">Pour afficher le contenu du fichier, double-cliquez sur **hubs**.</span><span class="sxs-lookup"><span data-stu-id="ea510-178">To see the contents of the file, double-click **hubs**.</span></span> <span data-ttu-id="ea510-179">Si vous n’utilisez pas Visual Studio 2012 ou 2013 et Internet Explorer, ou si vous n’êtes pas en mode débogage, vous pouvez également obtenir le contenu du fichier en accédant à l’URL « / signalR hubs ».</span><span class="sxs-lookup"><span data-stu-id="ea510-179">If you are not using Visual Studio 2012 or 2013 and Internet Explorer, or if you are not in debug mode, you can also get the contents of the file by browsing to the "/signalR/hubs" URL.</span></span> <span data-ttu-id="ea510-180">Par exemple, si votre site est en cours d’exécution à `http://localhost:56699`, accédez à `http://localhost:56699/SignalR/hubs` dans votre navigateur.</span><span class="sxs-lookup"><span data-stu-id="ea510-180">For example, if your site is running at `http://localhost:56699`, go to `http://localhost:56699/SignalR/hubs` in your browser.</span></span>

<a id="manualproxy"></a>

### <a name="how-to-create-a-physical-file-for-the-signalr-generated-proxy"></a><span data-ttu-id="ea510-181">Comment créer un fichier physique pour SignalR de proxy généré</span><span class="sxs-lookup"><span data-stu-id="ea510-181">How to create a physical file for the SignalR generated proxy</span></span>

<span data-ttu-id="ea510-182">Comme alternative au proxy généré de manière dynamique, vous pouvez créer un fichier physique contenant le code proxy et référencer ce fichier.</span><span class="sxs-lookup"><span data-stu-id="ea510-182">As an alternative to the dynamically generated proxy, you can create a physical file that has the proxy code and reference that file.</span></span> <span data-ttu-id="ea510-183">Peut-être voulez-vous faire pour contrôler la mise en cache ou son comportement de regroupement ou d’utiliser IntelliSense lorsque vous codez des appels aux méthodes de serveur.</span><span class="sxs-lookup"><span data-stu-id="ea510-183">You might want to do that for control over caching or bundling behavior, or to get IntelliSense when you are coding calls to server methods.</span></span>

<span data-ttu-id="ea510-184">Pour créer un fichier de proxy, procédez comme suit :</span><span class="sxs-lookup"><span data-stu-id="ea510-184">To create a proxy file, perform the following steps:</span></span>

1. <span data-ttu-id="ea510-185">Installer le [Microsoft.AspNet.SignalR.Utils](https://nuget.org/packages/Microsoft.AspNet.SignalR.Utils/) package NuGet.</span><span class="sxs-lookup"><span data-stu-id="ea510-185">Install the [Microsoft.AspNet.SignalR.Utils](https://nuget.org/packages/Microsoft.AspNet.SignalR.Utils/) NuGet package.</span></span>
2. <span data-ttu-id="ea510-186">Ouvrez une invite de commandes et accédez à la *outils* dossier qui contient le fichier SignalR.exe.</span><span class="sxs-lookup"><span data-stu-id="ea510-186">Open a command prompt and browse to the *tools* folder that contains the SignalR.exe file.</span></span> <span data-ttu-id="ea510-187">Le dossier Outils est à l’emplacement suivant :</span><span class="sxs-lookup"><span data-stu-id="ea510-187">The tools folder is at the following location:</span></span>

    `[your solution folder]\packages\Microsoft.AspNet.SignalR.Utils.2.1.0\tools`
3. <span data-ttu-id="ea510-188">Entrez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="ea510-188">Enter the following command:</span></span>

    `signalr ghp /path:[path to the .dll that contains your Hub class]`

    <span data-ttu-id="ea510-189">Le chemin d’accès à votre *.dll* est généralement le *bin* dossier dans votre dossier de projet.</span><span class="sxs-lookup"><span data-stu-id="ea510-189">The path to your *.dll* is typically the *bin* folder in your project folder.</span></span>

    <span data-ttu-id="ea510-190">Cette commande crée un fichier nommé *server.js* dans le même dossier que *signalr.exe*.</span><span class="sxs-lookup"><span data-stu-id="ea510-190">This command creates a file named *server.js* in the same folder as *signalr.exe*.</span></span>
4. <span data-ttu-id="ea510-191">Placez le *server.js* de fichiers dans un dossier approprié dans votre projet, renommez-le comme il convient pour votre application et ajoutez une référence à celui-ci à la place de la référence « signalr/hubs ».</span><span class="sxs-lookup"><span data-stu-id="ea510-191">Put the *server.js* file in an appropriate folder in your project, rename it as appropriate for your application, and add a reference to it in place of the "signalr/hubs" reference.</span></span>

<a id="establishconnection"></a>

## <a name="how-to-establish-a-connection"></a><span data-ttu-id="ea510-192">Comment établir une connexion</span><span class="sxs-lookup"><span data-stu-id="ea510-192">How to establish a connection</span></span>

<span data-ttu-id="ea510-193">Avant de pouvoir établir une connexion, vous devez créer un objet de connexion, créer un proxy et inscrire des gestionnaires d’événements pour les méthodes qui peuvent être appelées à partir du serveur.</span><span class="sxs-lookup"><span data-stu-id="ea510-193">Before you can establish a connection, you have to create a connection object, create a proxy, and register event handlers for methods that can be called from the server.</span></span> <span data-ttu-id="ea510-194">Quand les gestionnaires d’événements et de proxy sont configurés, établir la connexion en appelant le `start` (méthode).</span><span class="sxs-lookup"><span data-stu-id="ea510-194">When the proxy and event handlers are set up, establish the connection by calling the `start` method.</span></span>

<span data-ttu-id="ea510-195">Si vous utilisez le proxy généré, vous n’êtes pas obligé de créer l’objet de connexion dans votre propre code, car le code proxy généré le fait pour vous.</span><span class="sxs-lookup"><span data-stu-id="ea510-195">If you are using the generated proxy, you don't have to create the connection object in your own code because the generated proxy code does it for you.</span></span>

<a id="nogenconnection"></a>

<span data-ttu-id="ea510-196">**Établir une connexion (avec le proxy généré)**</span><span class="sxs-lookup"><span data-stu-id="ea510-196">**Establish a connection (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample8.js?highlight=5)]

<span data-ttu-id="ea510-197">**Établir une connexion (sans le proxy généré)**</span><span class="sxs-lookup"><span data-stu-id="ea510-197">**Establish a connection (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample9.js?highlight=1,6)]

<span data-ttu-id="ea510-198">L’exemple de code utilise la valeur par défaut « / signalr « URL pour se connecter à votre service de SignalR.</span><span class="sxs-lookup"><span data-stu-id="ea510-198">The sample code uses the default "/signalr" URL to connect to your SignalR service.</span></span> <span data-ttu-id="ea510-199">Pour plus d’informations sur la façon de spécifier une autre URL de base, consultez [Guide de l’API ASP.NET SignalR Hubs - Server - URL /signalr](hubs-api-guide-server.md#signalrurl).</span><span class="sxs-lookup"><span data-stu-id="ea510-199">For information about how to specify a different base URL, see [ASP.NET SignalR Hubs API Guide - Server - The /signalr URL](hubs-api-guide-server.md#signalrurl).</span></span>

<span data-ttu-id="ea510-200">Par défaut, l’emplacement de concentrateur est le serveur actuel ; Si vous vous connectez à un autre serveur, spécifiez l’URL avant d’appeler le `start` méthode, comme indiqué dans l’exemple suivant :</span><span class="sxs-lookup"><span data-stu-id="ea510-200">By default, the hub location is the current server; if you are connecting to a different server, specify the URL before calling the `start` method, as shown in the following example:</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample10.js)]

> [!NOTE]
> <span data-ttu-id="ea510-201">Normalement, vous inscrivez les gestionnaires d’événements avant d’appeler le `start` méthode pour établir la connexion.</span><span class="sxs-lookup"><span data-stu-id="ea510-201">Normally you register event handlers before calling the `start` method to establish the connection.</span></span> <span data-ttu-id="ea510-202">Si vous souhaitez inscrire des gestionnaires d’événements après avoir établi la connexion, vous pouvez le faire, mais vous devez vous inscrire au moins un de vos gestionnaires d’événements avant d’appeler le `start` (méthode).</span><span class="sxs-lookup"><span data-stu-id="ea510-202">If you want to register some event handlers after establishing the connection, you can do that, but you must register at least one of your event handler(s) before calling the `start` method.</span></span> <span data-ttu-id="ea510-203">Une des raisons sont qu’il peut y avoir de nombreux concentrateurs dans une application, mais vous ne voudriez déclencher le `OnConnected` événement sur chaque Hub si vous souhaitez uniquement utiliser pour un d’eux.</span><span class="sxs-lookup"><span data-stu-id="ea510-203">One reason for this is that there can be many Hubs in an application, but you wouldn't want to trigger the `OnConnected` event on every Hub if you are only going to use to one of them.</span></span> <span data-ttu-id="ea510-204">Lorsque la connexion est établie, la présence d’une méthode de client sur proxy d’un concentrateur est ce qui indique à SignalR pour déclencher le `OnConnected` événement.</span><span class="sxs-lookup"><span data-stu-id="ea510-204">When the connection is established, the presence of a client method on a Hub's proxy is what tells SignalR to trigger the `OnConnected` event.</span></span> <span data-ttu-id="ea510-205">Si vous n’enregistrez pas les gestionnaires d’événements avant d’appeler le `start` (méthode), vous serez en mesure d’appeler des méthodes sur le concentrateur, mais le Hub `OnConnected` méthode n’est pas appelée et aucune méthode client ne sera appelée à partir du serveur.</span><span class="sxs-lookup"><span data-stu-id="ea510-205">If you don't register any event handlers before calling the `start` method, you will be able to invoke methods on the Hub, but the Hub's `OnConnected` method won't be called and no client methods will be invoked from the server.</span></span>


<a id="connequivalence"></a>

### <a name="connectionhub-is-the-same-object-that-hubconnection-creates"></a><span data-ttu-id="ea510-206">$. connection.hub est le même objet crée ce $.hubConnection()</span><span class="sxs-lookup"><span data-stu-id="ea510-206">$.connection.hub is the same object that $.hubConnection() creates</span></span>

<span data-ttu-id="ea510-207">Comme vous pouvez le voir dans les exemples, lorsque vous utilisez le proxy généré, `$.connection.hub` fait référence à l’objet de connexion.</span><span class="sxs-lookup"><span data-stu-id="ea510-207">As you can see from the examples, when you use the generated proxy, `$.connection.hub` refers to the connection object.</span></span> <span data-ttu-id="ea510-208">Il s’agit du même objet que vous obtenez en appelant `$.hubConnection()` lorsque vous n’utilisez pas le proxy généré.</span><span class="sxs-lookup"><span data-stu-id="ea510-208">This is the same object that you get by calling `$.hubConnection()` when you aren't using the generated proxy.</span></span> <span data-ttu-id="ea510-209">Le code proxy généré crée la connexion pour vous en exécutant l’instruction suivante :</span><span class="sxs-lookup"><span data-stu-id="ea510-209">The generated proxy code creates the connection for you by executing the following statement:</span></span>

![Création d’une connexion dans le fichier proxy généré](hubs-api-guide-javascript-client/_static/image3.png)

<span data-ttu-id="ea510-211">Lorsque vous utilisez le proxy généré, vous pouvez effectuer quoi que ce soit avec `$.connection.hub` que vous pouvez faire avec un objet de connexion lorsque vous n’utilisez pas le proxy généré.</span><span class="sxs-lookup"><span data-stu-id="ea510-211">When you're using the generated proxy, you can do anything with `$.connection.hub` that you can do with a connection object when you're not using the generated proxy.</span></span>

<a id="asyncstart"></a>

### <a name="asynchronous-execution-of-the-start-method"></a><span data-ttu-id="ea510-212">Exécution asynchrone de la méthode start</span><span class="sxs-lookup"><span data-stu-id="ea510-212">Asynchronous execution of the start method</span></span>

<span data-ttu-id="ea510-213">Le `start` méthode s’exécute de façon asynchrone.</span><span class="sxs-lookup"><span data-stu-id="ea510-213">The `start` method executes asynchronously.</span></span> <span data-ttu-id="ea510-214">Elle retourne un [jQuery différé objet](http://api.jquery.com/category/deferred-object/), ce qui signifie que vous pouvez ajouter des fonctions de rappel en appelant des méthodes comme `pipe`, `done`, et `fail`.</span><span class="sxs-lookup"><span data-stu-id="ea510-214">It returns a [jQuery Deferred object](http://api.jquery.com/category/deferred-object/), which means that you can add callback functions by calling methods such as `pipe`, `done`, and `fail`.</span></span> <span data-ttu-id="ea510-215">Si vous avez le code que vous souhaitez exécuter une fois la connexion est établie, tel qu’un appel à une méthode de serveur, placez ce code dans une fonction de rappel ou appeler à partir d’une fonction de rappel.</span><span class="sxs-lookup"><span data-stu-id="ea510-215">If you have code that you want to execute after the connection is established, such as a call to a server method, put that code in a callback function or call it from a callback function.</span></span> <span data-ttu-id="ea510-216">Le `.done` méthode de rappel est exécutée une fois que la connexion a été établie, et une fois que tout code que vous avez dans votre `OnConnected` méthode de gestionnaire d’événements sur le serveur termine son exécution.</span><span class="sxs-lookup"><span data-stu-id="ea510-216">The `.done` callback method is executed after the connection has been established, and after any code that you have in your `OnConnected` event handler method on the server finishes executing.</span></span>

<span data-ttu-id="ea510-217">Si vous placez l’instruction « Désormais connecté » de l’exemple précédent en tant que la ligne suivante du code après le `start` appel de méthode (pas dans un `.done` rappel), le `console.log` ligne s’exécute avant que la connexion est établie, comme indiqué dans l’exemple suivant exemple :</span><span class="sxs-lookup"><span data-stu-id="ea510-217">If you put the "Now connected" statement from the preceding example as the next line of code after the `start` method call (not in a `.done` callback), the `console.log` line will execute before the connection is established, as shown in the following example:</span></span>

![Mauvaise façon d’écrire du code qui s’exécute après que la connexion est établie.](hubs-api-guide-javascript-client/_static/image5.png)

<a id="crossdomain"></a>

## <a name="how-to-establish-a-cross-domain-connection"></a><span data-ttu-id="ea510-219">Comment établir une connexion entre domaines</span><span class="sxs-lookup"><span data-stu-id="ea510-219">How to establish a cross-domain connection</span></span>

<span data-ttu-id="ea510-220">En général, si le navigateur charge d’une page `http://contoso.com`, la connexion de SignalR est dans le même domaine, `http://contoso.com/signalr`.</span><span class="sxs-lookup"><span data-stu-id="ea510-220">Typically if the browser loads a page from `http://contoso.com`, the SignalR connection is in the same domain, at `http://contoso.com/signalr`.</span></span> <span data-ttu-id="ea510-221">Si la page à partir de `http://contoso.com` établit une connexion à `http://fabrikam.com/signalr`, qui est une connexion entre domaines.</span><span class="sxs-lookup"><span data-stu-id="ea510-221">If the page from `http://contoso.com` makes a connection to `http://fabrikam.com/signalr`, that is a cross-domain connection.</span></span> <span data-ttu-id="ea510-222">Pour des raisons de sécurité, les connexions inter-domaines sont désactivées par défaut.</span><span class="sxs-lookup"><span data-stu-id="ea510-222">For security reasons, cross-domain connections are disabled by default.</span></span>

<span data-ttu-id="ea510-223">Dans SignalR 1.x, des requêtes entre domaines ont été contrôlé par un indicateur EnableCrossDomain unique.</span><span class="sxs-lookup"><span data-stu-id="ea510-223">In SignalR 1.x, cross domain requests were controlled by a single EnableCrossDomain flag.</span></span> <span data-ttu-id="ea510-224">Cet indicateur contrôlé demandes JSONP et CORS.</span><span class="sxs-lookup"><span data-stu-id="ea510-224">This flag controlled both JSONP and CORS requests.</span></span> <span data-ttu-id="ea510-225">Pour une flexibilité accrue, tous les CORS prennent en charge a été supprimée à partir du composant serveur de SignalR (clients JavaScript toujours utiliseront CORS normalement s’il est détecté que le navigateur prend en charge), et nouvel intergiciel (middleware) OWIN soit devenue disponible pour prendre en charge ces scénarios.</span><span class="sxs-lookup"><span data-stu-id="ea510-225">For greater flexibility, all CORS support has been removed from the server component of SignalR (JavaScript clients still use CORS normally if it is detected that the browser supports it), and new OWIN middleware has been made available to support these scenarios.</span></span>

<span data-ttu-id="ea510-226">Si JSONP est requise sur le client (pour prendre en charge les demandes inter-domaines dans les navigateurs plus anciens), il doit être activée explicitement en définissant `EnableJSONP` sur le `HubConfiguration` objet `true`, comme illustré ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="ea510-226">If JSONP is required on the client (to support cross-domain requests in older browsers), it will need to be enabled explicitly by setting `EnableJSONP` on the `HubConfiguration` object to `true`, as shown below.</span></span> <span data-ttu-id="ea510-227">JSONP est désactivée par défaut, car il est moins sécurisé que CORS.</span><span class="sxs-lookup"><span data-stu-id="ea510-227">JSONP is disabled by default, as it is less secure than CORS.</span></span>

<span data-ttu-id="ea510-228">**Ajout de Microsoft.Owin.Cors à votre projet :** Pour installer cette bibliothèque, exécutez la commande suivante dans la Console du Gestionnaire de Package :</span><span class="sxs-lookup"><span data-stu-id="ea510-228">**Adding Microsoft.Owin.Cors to your project:** To install this library, run the following command in the Package Manager Console:</span></span>

`Install-Package Microsoft.Owin.Cors`

<span data-ttu-id="ea510-229">Cette commande ajoutera le 2.1.0 version du package à votre projet.</span><span class="sxs-lookup"><span data-stu-id="ea510-229">This command will add the 2.1.0 version of the package to your project.</span></span>

### <a name="calling-usecors"></a><span data-ttu-id="ea510-230">Appeler UseCors</span><span class="sxs-lookup"><span data-stu-id="ea510-230">Calling UseCors</span></span>

 <span data-ttu-id="ea510-231">L’extrait de code suivant montre comment implémenter des connexions inter-domaines dans SignalR 2.</span><span class="sxs-lookup"><span data-stu-id="ea510-231">The following code snippet demonstrates how to implement cross-domain connections in SignalR 2.</span></span>

<span data-ttu-id="ea510-232">**Implémentation de demandes inter-domaines dans SignalR 2**</span><span class="sxs-lookup"><span data-stu-id="ea510-232">**Implementing cross-domain requests in SignalR 2**</span></span>

<span data-ttu-id="ea510-233">Le code suivant montre comment activer CORS ou JSONP dans un projet de SignalR 2.</span><span class="sxs-lookup"><span data-stu-id="ea510-233">The following code demonstrates how to enable CORS or JSONP in a SignalR 2 project.</span></span> <span data-ttu-id="ea510-234">Cet exemple de code utilise `Map` et `RunSignalR` au lieu de `MapSignalR`, de sorte que l’intergiciel (middleware) CORS s’exécute uniquement pour les demandes de SignalR qui nécessitent la prise en charge CORS (plutôt que pour tout le trafic sur le chemin spécifié dans `MapSignalR`.) Carte peut également être utilisée pour n’importe quel autre intergiciel (middleware) qui doit s’exécuter pour un préfixe d’URL spécifique, plutôt que pour l’application entière.</span><span class="sxs-lookup"><span data-stu-id="ea510-234">This code sample uses `Map` and `RunSignalR` instead of `MapSignalR`, so that the CORS middleware runs only for the SignalR requests that require CORS support (rather than for all traffic at the path specified in `MapSignalR`.) Map can also be used for any other middleware that needs to run for a specific URL prefix, rather than for the entire application.</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample11.cs)]

> [!NOTE]
>
> - <span data-ttu-id="ea510-235">Ne définissez pas `jQuery.support.cors` sur true dans votre code.</span><span class="sxs-lookup"><span data-stu-id="ea510-235">Don't set `jQuery.support.cors` to true in your code.</span></span>
>
>     ![Ne définissez pas jQuery.support.cors sur true](hubs-api-guide-javascript-client/_static/image7.png)
>
>     <span data-ttu-id="ea510-237">SignalR gère l’utilisation de CORS.</span><span class="sxs-lookup"><span data-stu-id="ea510-237">SignalR handles the use of CORS.</span></span> <span data-ttu-id="ea510-238">Paramètre `jQuery.support.cors` à la valeur true désactive JSONP, car elle force SignalR à assumer le navigateur prend en charge CORS.</span><span class="sxs-lookup"><span data-stu-id="ea510-238">Setting `jQuery.support.cors` to true disables JSONP because it causes SignalR to assume the browser supports CORS.</span></span>
> - <span data-ttu-id="ea510-239">Lorsque vous vous connectez à une URL localhost, Internet Explorer 10 ne considérez-la comme une connexion entre domaines, pour l’application fonctionne localement avec IE 10 même si vous n’avez pas activé les connexions inter-domaines sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="ea510-239">When you're connecting to a localhost URL, Internet Explorer 10 won't consider it a cross-domain connection, so the application will work locally with IE 10 even if you haven't enabled cross-domain connections on the server.</span></span>
> - <span data-ttu-id="ea510-240">Pour plus d’informations sur l’utilisation de connexions inter-domaines avec Internet Explorer 9, consultez [ce thread Stack Overflow](http://stackoverflow.com/questions/13573397/siganlr-ie9-cross-domain-request-dont-work).</span><span class="sxs-lookup"><span data-stu-id="ea510-240">For information about using cross-domain connections with Internet Explorer 9, see [this StackOverflow thread](http://stackoverflow.com/questions/13573397/siganlr-ie9-cross-domain-request-dont-work).</span></span>
> - <span data-ttu-id="ea510-241">Pour plus d’informations sur l’utilisation de connexions inter-domaines avec Chrome, consultez [ce thread Stack Overflow](http://stackoverflow.com/questions/15467373/signalr-1-0-1-cross-domain-request-cors-with-chrome).</span><span class="sxs-lookup"><span data-stu-id="ea510-241">For information about using cross-domain connections with Chrome, see [this StackOverflow thread](http://stackoverflow.com/questions/15467373/signalr-1-0-1-cross-domain-request-cors-with-chrome).</span></span>
> - <span data-ttu-id="ea510-242">L’exemple de code utilise la valeur par défaut « / signalr « URL pour se connecter à votre service de SignalR.</span><span class="sxs-lookup"><span data-stu-id="ea510-242">The sample code uses the default "/signalr" URL to connect to your SignalR service.</span></span> <span data-ttu-id="ea510-243">Pour plus d’informations sur la façon de spécifier une autre URL de base, consultez [Guide de l’API ASP.NET SignalR Hubs - Server - URL /signalr](hubs-api-guide-server.md#signalrurl).</span><span class="sxs-lookup"><span data-stu-id="ea510-243">For information about how to specify a different base URL, see [ASP.NET SignalR Hubs API Guide - Server - The /signalr URL](hubs-api-guide-server.md#signalrurl).</span></span>


<a id="configureconnection"></a>

## <a name="how-to-configure-the-connection"></a><span data-ttu-id="ea510-244">Comment configurer la connexion</span><span class="sxs-lookup"><span data-stu-id="ea510-244">How to configure the connection</span></span>

<span data-ttu-id="ea510-245">Avant d’établir une connexion, vous pouvez spécifier des paramètres de chaîne de requête ou spécifier le mode de transport.</span><span class="sxs-lookup"><span data-stu-id="ea510-245">Before you establish a connection, you can specify query string parameters or specify the transport method.</span></span>

<a id="querystring"></a>

### <a name="how-to-specify-query-string-parameters"></a><span data-ttu-id="ea510-246">Comment spécifier des paramètres de chaîne de requête</span><span class="sxs-lookup"><span data-stu-id="ea510-246">How to specify query string parameters</span></span>

<span data-ttu-id="ea510-247">Si vous souhaitez envoyer des données au serveur lorsque le client se connecte, vous pouvez ajouter des paramètres de chaîne de requête à l’objet de connexion.</span><span class="sxs-lookup"><span data-stu-id="ea510-247">If you want to send data to the server when the client connects, you can add query string parameters to the connection object.</span></span> <span data-ttu-id="ea510-248">Les exemples suivants montrent comment définir un paramètre de chaîne de requête dans le code client.</span><span class="sxs-lookup"><span data-stu-id="ea510-248">The following examples show how to set a query string parameter in client code.</span></span>

<span data-ttu-id="ea510-249">**Définir une valeur de chaîne de requête avant d’appeler la méthode start (avec le proxy généré)**</span><span class="sxs-lookup"><span data-stu-id="ea510-249">**Set a query string value before calling the start method (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample12.js?highlight=1)]

<span data-ttu-id="ea510-250">**Définir une valeur de chaîne de requête avant d’appeler la méthode start (sans le proxy généré)**</span><span class="sxs-lookup"><span data-stu-id="ea510-250">**Set a query string value before calling the start method (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample13.js?highlight=2)]

<span data-ttu-id="ea510-251">L’exemple suivant montre comment lire un paramètre de chaîne de requête dans le code serveur.</span><span class="sxs-lookup"><span data-stu-id="ea510-251">The following example shows how to read a query string parameter in server code.</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample14.cs?highlight=5)]

<a id="transport"></a>

### <a name="how-to-specify-the-transport-method"></a><span data-ttu-id="ea510-252">Comment spécifier le mode de transport</span><span class="sxs-lookup"><span data-stu-id="ea510-252">How to specify the transport method</span></span>

<span data-ttu-id="ea510-253">Dans le cadre du processus de connexion, un client SignalR est normalement négocie avec le serveur pour déterminer le meilleur transport qui est pris en charge par le serveur et client.</span><span class="sxs-lookup"><span data-stu-id="ea510-253">As part of the process of connecting, a SignalR client normally negotiates with the server to determine the best transport that is supported by both server and client.</span></span> <span data-ttu-id="ea510-254">Si vous connaissez déjà le transport à utiliser, vous pouvez ignorer ce processus de négociation en spécifiant le mode de transport lorsque vous appelez le `start` (méthode).</span><span class="sxs-lookup"><span data-stu-id="ea510-254">If you already know which transport you want to use, you can bypass this negotiation process by specifying the transport method when you call the `start` method.</span></span>

<span data-ttu-id="ea510-255">**Code client qui spécifie le mode de transport (avec le proxy généré)**</span><span class="sxs-lookup"><span data-stu-id="ea510-255">**Client code that specifies the transport method (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample15.js?highlight=1)]

<span data-ttu-id="ea510-256">**Code client qui spécifie le mode de transport (sans le proxy généré)**</span><span class="sxs-lookup"><span data-stu-id="ea510-256">**Client code that specifies the transport method (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample16.js?highlight=2)]

<span data-ttu-id="ea510-257">Comme alternative, vous pouvez spécifier plusieurs méthodes de transport dans l’ordre dans lequel vous souhaitez SignalR pour les essayer :</span><span class="sxs-lookup"><span data-stu-id="ea510-257">As an alternative, you can specify multiple transport methods in the order in which you want SignalR to try them:</span></span>

<span data-ttu-id="ea510-258">**Code client qui spécifie un schéma de secours de transport personnalisés (avec le proxy généré)**</span><span class="sxs-lookup"><span data-stu-id="ea510-258">**Client code that specifies a custom transport fallback scheme (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample17.js?highlight=1)]

<span data-ttu-id="ea510-259">**Code client qui spécifie un schéma de secours de transport personnalisés (sans le proxy généré)**</span><span class="sxs-lookup"><span data-stu-id="ea510-259">**Client code that specifies a custom transport fallback scheme (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample18.js?highlight=2)]

<span data-ttu-id="ea510-260">Vous pouvez utiliser les valeurs suivantes pour spécifier le mode de transport :</span><span class="sxs-lookup"><span data-stu-id="ea510-260">You can use the following values for specifying the transport method:</span></span>

- <span data-ttu-id="ea510-261">"webSockets"</span><span class="sxs-lookup"><span data-stu-id="ea510-261">"webSockets"</span></span>
- <span data-ttu-id="ea510-262">« foreverFrame »</span><span class="sxs-lookup"><span data-stu-id="ea510-262">"foreverFrame"</span></span>
- <span data-ttu-id="ea510-263">"serverSentEvents"</span><span class="sxs-lookup"><span data-stu-id="ea510-263">"serverSentEvents"</span></span>
- <span data-ttu-id="ea510-264">« longPolling »</span><span class="sxs-lookup"><span data-stu-id="ea510-264">"longPolling"</span></span>

<span data-ttu-id="ea510-265">Les exemples suivants montrent comment savoir quelle méthode de transport est utilisé par une connexion.</span><span class="sxs-lookup"><span data-stu-id="ea510-265">The following examples show how to find out which transport method is being used by a connection.</span></span>

<span data-ttu-id="ea510-266">**Code client qui affiche le mode de transport utilisé par une connexion (avec le proxy généré)**</span><span class="sxs-lookup"><span data-stu-id="ea510-266">**Client code that displays the transport method used by a connection (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample19.js?highlight=2)]

<span data-ttu-id="ea510-267">**Code client qui affiche le mode de transport utilisé par une connexion (sans le proxy généré)**</span><span class="sxs-lookup"><span data-stu-id="ea510-267">**Client code that displays the transport method used by a connection (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample20.js?highlight=3)]

<span data-ttu-id="ea510-268">Pour plus d’informations sur la vérification de la méthode de transport dans le code serveur, consultez [Guide de l’API ASP.NET SignalR Hubs - Server - comment obtenir des informations sur le client à partir de la propriété de contexte](hubs-api-guide-server.md#contextproperty).</span><span class="sxs-lookup"><span data-stu-id="ea510-268">For information about how to check the transport method in server code, see [ASP.NET SignalR Hubs API Guide - Server - How to get information about the client from the Context property](hubs-api-guide-server.md#contextproperty).</span></span> <span data-ttu-id="ea510-269">Pour plus d’informations sur les transports et les solutions de secours, consultez [Introduction à SignalR - Transports et les solutions de secours](../getting-started/introduction-to-signalr.md#transports).</span><span class="sxs-lookup"><span data-stu-id="ea510-269">For more information about transports and fallbacks, see [Introduction to SignalR - Transports and Fallbacks](../getting-started/introduction-to-signalr.md#transports).</span></span>

<a id="getproxy"></a>

## <a name="how-to-get-a-proxy-for-a-hub-class"></a><span data-ttu-id="ea510-270">Comment obtenir un proxy pour une classe de concentrateur</span><span class="sxs-lookup"><span data-stu-id="ea510-270">How to get a proxy for a Hub class</span></span>

<span data-ttu-id="ea510-271">Chaque objet de connexion que vous créez encapsule des informations sur une connexion à un service de SignalR qui contient une ou plusieurs classes de concentrateur.</span><span class="sxs-lookup"><span data-stu-id="ea510-271">Each connection object that you create encapsulates information about a connection to a SignalR service that contains one or more Hub classes.</span></span> <span data-ttu-id="ea510-272">Pour communiquer avec une classe de concentrateur, vous utilisez un objet proxy que vous créez vous-même (si vous n’utilisez pas le proxy généré) ou qui est généré pour vous.</span><span class="sxs-lookup"><span data-stu-id="ea510-272">To communicate with a Hub class, you use a proxy object which you create yourself (if you're not using the generated proxy) or which is generated for you.</span></span>

<span data-ttu-id="ea510-273">Sur le client, le nom du proxy est une version de casse mixte du nom de classe du concentrateur.</span><span class="sxs-lookup"><span data-stu-id="ea510-273">On the client the proxy name is a camel-cased version of the Hub class name.</span></span> <span data-ttu-id="ea510-274">SignalR crée automatiquement ce changement afin que le code JavaScript peut être conforme aux conventions de JavaScript.</span><span class="sxs-lookup"><span data-stu-id="ea510-274">SignalR automatically makes this change so that JavaScript code can conform to JavaScript conventions.</span></span>

<span data-ttu-id="ea510-275">**Classe de concentrateur sur le serveur**</span><span class="sxs-lookup"><span data-stu-id="ea510-275">**Hub class on server**</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample21.cs?highlight=1)]

<span data-ttu-id="ea510-276">**Obtenir une référence au proxy client généré pour le Hub**</span><span class="sxs-lookup"><span data-stu-id="ea510-276">**Get a reference to the generated client proxy for the Hub**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample22.js?highlight=1)]

<span data-ttu-id="ea510-277">**Créer le proxy client pour la classe de concentrateur (sans proxy généré)**</span><span class="sxs-lookup"><span data-stu-id="ea510-277">**Create client proxy for the Hub class (without generated proxy)**</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample23.cs?highlight=1)]

<span data-ttu-id="ea510-278">Si vous décorez votre classe Hub avec un `HubName` d’attribut, utilisez le nom exact sans changement de casse.</span><span class="sxs-lookup"><span data-stu-id="ea510-278">If you decorate your Hub class with a `HubName` attribute, use the exact name without changing case.</span></span>

<span data-ttu-id="ea510-279">**Classe de concentrateur sur le serveur avec l’attribut de HubName**</span><span class="sxs-lookup"><span data-stu-id="ea510-279">**Hub class on server with HubName attribute**</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample24.cs?highlight=1)]

<span data-ttu-id="ea510-280">**Obtenir une référence au proxy client généré pour le Hub**</span><span class="sxs-lookup"><span data-stu-id="ea510-280">**Get a reference to the generated client proxy for the Hub**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample25.js?highlight=1)]

<span data-ttu-id="ea510-281">**Créer le proxy client pour la classe de concentrateur (sans proxy généré)**</span><span class="sxs-lookup"><span data-stu-id="ea510-281">**Create client proxy for the Hub class (without generated proxy)**</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample26.cs?highlight=1)]

<a id="callclient"></a>

## <a name="how-to-define-methods-on-the-client-that-the-server-can-call"></a><span data-ttu-id="ea510-282">Comment définir des méthodes sur le client que le serveur peut appeler.</span><span class="sxs-lookup"><span data-stu-id="ea510-282">How to define methods on the client that the server can call</span></span>

<span data-ttu-id="ea510-283">Pour définir une méthode que le serveur peut appeler à partir d’un Hub, ajoutez un gestionnaire d’événements pour le concentrateur proxy à l’aide de la `client` propriété du proxy généré ou appel le `on` méthode si vous n’utilisez pas le proxy généré.</span><span class="sxs-lookup"><span data-stu-id="ea510-283">To define a method that the server can call from a Hub, add an event handler to the Hub proxy by using the `client` property of the generated proxy, or call the `on` method if you aren't using the generated proxy.</span></span> <span data-ttu-id="ea510-284">Les paramètres peuvent être des objets complexes.</span><span class="sxs-lookup"><span data-stu-id="ea510-284">The parameters can be complex objects.</span></span>

<span data-ttu-id="ea510-285">Ajouter le Gestionnaire d’événements avant d’appeler le `start` méthode pour établir la connexion.</span><span class="sxs-lookup"><span data-stu-id="ea510-285">Add the event handler before you call the `start` method to establish the connection.</span></span> <span data-ttu-id="ea510-286">(Si vous souhaitez ajouter des gestionnaires d’événements après avoir appelé la `start` (méthode), consultez la remarque dans [comment établir une connexion](#establishconnection) précédemment dans ce document et utiliser la syntaxe indiquée pour la définition d’une méthode sans utiliser le proxy généré.)</span><span class="sxs-lookup"><span data-stu-id="ea510-286">(If you want to add event handlers after calling the `start` method, see the note in [How to establish a connection](#establishconnection) earlier in this document, and use the syntax shown for defining a method without using the generated proxy.)</span></span>

<span data-ttu-id="ea510-287">Correspondance de noms de méthode respecte la casse.</span><span class="sxs-lookup"><span data-stu-id="ea510-287">Method name matching is case-insensitive.</span></span> <span data-ttu-id="ea510-288">Par exemple, `Clients.All.addContosoChatMessageToPage` sur le serveur s’exécutera `AddContosoChatMessageToPage`, `addContosoChatMessageToPage`, ou `addcontosochatmessagetopage` sur le client.</span><span class="sxs-lookup"><span data-stu-id="ea510-288">For example, `Clients.All.addContosoChatMessageToPage` on the server will execute `AddContosoChatMessageToPage`, `addContosoChatMessageToPage`, or `addcontosochatmessagetopage` on the client.</span></span>

<span data-ttu-id="ea510-289">**Définir la méthode sur le client (avec le proxy généré)**</span><span class="sxs-lookup"><span data-stu-id="ea510-289">**Define method on client (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample27.js?highlight=2)]

<span data-ttu-id="ea510-290">**Autre manière de définir la méthode sur le client (avec le proxy généré)**</span><span class="sxs-lookup"><span data-stu-id="ea510-290">**Alternate way to define method on client (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample28.js?highlight=1-2)]

<span data-ttu-id="ea510-291">**Définir la méthode sur le client (sans le proxy généré, ou lorsque vous ajoutez après avoir appelé la méthode start)**</span><span class="sxs-lookup"><span data-stu-id="ea510-291">**Define method on client (without the generated proxy, or when adding after calling the start method)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample29.js?highlight=3)]

<span data-ttu-id="ea510-292">**Code de serveur qui appelle la méthode du client**</span><span class="sxs-lookup"><span data-stu-id="ea510-292">**Server code that calls the client method**</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample30.cs?highlight=5)]

<span data-ttu-id="ea510-293">Les exemples suivants incluent un objet complexe comme un paramètre de méthode.</span><span class="sxs-lookup"><span data-stu-id="ea510-293">The following examples include a complex object as a method parameter.</span></span>

<span data-ttu-id="ea510-294">**Définir la méthode sur le client qui prend un objet complexe (avec le proxy généré)**</span><span class="sxs-lookup"><span data-stu-id="ea510-294">**Define method on client that takes a complex object (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample31.js?highlight=2-3)]

<span data-ttu-id="ea510-295">**Définir la méthode sur le client qui prend un objet complexe (sans le proxy généré)**</span><span class="sxs-lookup"><span data-stu-id="ea510-295">**Define method on client that takes a complex object (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample32.js?highlight=3-4)]

<span data-ttu-id="ea510-296">**Code de serveur qui définit l’objet complexe**</span><span class="sxs-lookup"><span data-stu-id="ea510-296">**Server code that defines the complex object**</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample33.cs?highlight=1)]

<span data-ttu-id="ea510-297">**Code de serveur qui appelle la méthode du client à l’aide d’un objet complexe**</span><span class="sxs-lookup"><span data-stu-id="ea510-297">**Server code that calls the client method using a complex object**</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample34.cs?highlight=3)]

<a id="callserver"></a>

## <a name="how-to-call-server-methods-from-the-client"></a><span data-ttu-id="ea510-298">Comment appeler des méthodes de serveur à partir du client</span><span class="sxs-lookup"><span data-stu-id="ea510-298">How to call server methods from the client</span></span>

<span data-ttu-id="ea510-299">Pour appeler une méthode de serveur à partir du client, utilisez le `server` propriété du proxy généré ou le `invoke` méthode sur le concentrateur proxy si vous n’utilisez pas le proxy généré.</span><span class="sxs-lookup"><span data-stu-id="ea510-299">To call a server method from the client, use the `server` property of the generated proxy or the `invoke` method on the Hub proxy if you aren't using the generated proxy.</span></span> <span data-ttu-id="ea510-300">La valeur de retour ou paramètres peuvent être des objets complexes.</span><span class="sxs-lookup"><span data-stu-id="ea510-300">The return value or parameters can be complex objects.</span></span>

<span data-ttu-id="ea510-301">Passer une version de casse mixte du nom de méthode du concentrateur.</span><span class="sxs-lookup"><span data-stu-id="ea510-301">Pass in a camel-case version of the method name on the Hub.</span></span> <span data-ttu-id="ea510-302">SignalR crée automatiquement ce changement afin que le code JavaScript peut être conforme aux conventions de JavaScript.</span><span class="sxs-lookup"><span data-stu-id="ea510-302">SignalR automatically makes this change so that JavaScript code can conform to JavaScript conventions.</span></span>

<span data-ttu-id="ea510-303">Les exemples suivants montrent comment appeler une méthode de serveur qui n’a pas une valeur de retour et comment appeler une méthode de serveur qui n’a pas une valeur de retour.</span><span class="sxs-lookup"><span data-stu-id="ea510-303">The following examples show how to call a server method that doesn't have a return value and how to call a server method that does have a return value.</span></span>

<span data-ttu-id="ea510-304">**Méthode de serveur avec aucun attribut HubMethodName**</span><span class="sxs-lookup"><span data-stu-id="ea510-304">**Server method with no HubMethodName attribute**</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample35.cs?highlight=3)]

<span data-ttu-id="ea510-305">**Code de serveur qui définit l’objet complexe passées dans un paramètre**</span><span class="sxs-lookup"><span data-stu-id="ea510-305">**Server code that defines the complex object passed in a parameter**</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample36.cs)]

<span data-ttu-id="ea510-306">**Code client qui appelle la méthode de serveur (avec le proxy généré)**</span><span class="sxs-lookup"><span data-stu-id="ea510-306">**Client code that invokes the server method (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample37.js?highlight=1)]

<span data-ttu-id="ea510-307">**Code client qui appelle la méthode de serveur (sans le proxy généré)**</span><span class="sxs-lookup"><span data-stu-id="ea510-307">**Client code that invokes the server method (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample38.js?highlight=1)]

<span data-ttu-id="ea510-308">Si vous décorée avec la méthode de concentrateur un `HubMethodName` d’attribut, utilisez ce nom sans changement de casse.</span><span class="sxs-lookup"><span data-stu-id="ea510-308">If you decorated the Hub method with a `HubMethodName` attribute, use that name without changing case.</span></span>

<span data-ttu-id="ea510-309">**Méthode serveur** avec un attribut HubMethodName</span><span class="sxs-lookup"><span data-stu-id="ea510-309">**Server method** with a HubMethodName attribute</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample39.cs?highlight=3)]

<span data-ttu-id="ea510-310">**Code client qui appelle la méthode de serveur (avec le proxy généré)**</span><span class="sxs-lookup"><span data-stu-id="ea510-310">**Client code that invokes the server method (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample40.js?highlight=1)]

<span data-ttu-id="ea510-311">**Code client qui appelle la méthode de serveur (sans le proxy généré)**</span><span class="sxs-lookup"><span data-stu-id="ea510-311">**Client code that invokes the server method (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample41.js?highlight=1)]

<span data-ttu-id="ea510-312">Les exemples précédents montrent comment appeler une méthode de serveur qui n’a aucune valeur de retour.</span><span class="sxs-lookup"><span data-stu-id="ea510-312">The preceding examples show how to call a server method that has no return value.</span></span> <span data-ttu-id="ea510-313">Les exemples suivants montrent comment appeler une méthode de serveur qui a une valeur de retour.</span><span class="sxs-lookup"><span data-stu-id="ea510-313">The following examples show how to call a server method that has a return value.</span></span>

<span data-ttu-id="ea510-314">**Code de serveur pour une méthode qui a une valeur de retour**</span><span class="sxs-lookup"><span data-stu-id="ea510-314">**Server code for a method that has a return value**</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample42.cs?highlight=3)]

<span data-ttu-id="ea510-315">**La classe de Stock utilisée pour le** valeur de retour</span><span class="sxs-lookup"><span data-stu-id="ea510-315">**The Stock class used for the** return value</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample43.cs?highlight=1)]

<span data-ttu-id="ea510-316">**Code client qui appelle la méthode de serveur (avec le proxy généré)**</span><span class="sxs-lookup"><span data-stu-id="ea510-316">**Client code that invokes the server method (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample44.js?highlight=2,4-5)]

<span data-ttu-id="ea510-317">**Code client qui appelle la méthode de serveur (sans le proxy généré)**</span><span class="sxs-lookup"><span data-stu-id="ea510-317">**Client code that invokes the server method (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample45.js?highlight=2,4-5)]

<a id="connectionlifetime"></a>

## <a name="how-to-handle-connection-lifetime-events"></a><span data-ttu-id="ea510-318">Comment gérer les événements de durée de vie de connexion</span><span class="sxs-lookup"><span data-stu-id="ea510-318">How to handle connection lifetime events</span></span>

<span data-ttu-id="ea510-319">SignalR fournit des événements de durée de vie que vous pouvez gérer la connexion suivante :</span><span class="sxs-lookup"><span data-stu-id="ea510-319">SignalR provides the following connection lifetime events that you can handle:</span></span>

- <span data-ttu-id="ea510-320">`starting`: Déclenché avant que les données sont envoyées via la connexion.</span><span class="sxs-lookup"><span data-stu-id="ea510-320">`starting`: Raised before any data is sent over the connection.</span></span>
- <span data-ttu-id="ea510-321">`received`: Déclenché lorsque toutes les données sont reçues sur la connexion.</span><span class="sxs-lookup"><span data-stu-id="ea510-321">`received`: Raised when any data is received on the connection.</span></span> <span data-ttu-id="ea510-322">Fournit les données reçues.</span><span class="sxs-lookup"><span data-stu-id="ea510-322">Provides the received data.</span></span>
- <span data-ttu-id="ea510-323">`connectionSlow`: Déclenché lorsque le client détecte une connexion lente ou suppression fréquemment.</span><span class="sxs-lookup"><span data-stu-id="ea510-323">`connectionSlow`: Raised when the client detects a slow or frequently dropping connection.</span></span>
- <span data-ttu-id="ea510-324">`reconnecting`: Déclenché lorsque le transport sous-jacent commence à se reconnecter.</span><span class="sxs-lookup"><span data-stu-id="ea510-324">`reconnecting`: Raised when the underlying transport begins reconnecting.</span></span>
- <span data-ttu-id="ea510-325">`reconnected`: Déclenché lorsque le transport sous-jacent s’est reconnecté.</span><span class="sxs-lookup"><span data-stu-id="ea510-325">`reconnected`: Raised when the underlying transport has reconnected.</span></span>
- <span data-ttu-id="ea510-326">`stateChanged`: Déclenché lorsque l’état de connexion change.</span><span class="sxs-lookup"><span data-stu-id="ea510-326">`stateChanged`: Raised when the connection state changes.</span></span> <span data-ttu-id="ea510-327">Fournit l’ancien état et le nouvel état (connexion, connecté, reconnexion ou Disconnected).</span><span class="sxs-lookup"><span data-stu-id="ea510-327">Provides the old state and the new state (Connecting, Connected, Reconnecting, or Disconnected).</span></span>
- <span data-ttu-id="ea510-328">`disconnected`: Déclenché lorsque la connexion a déconnecté.</span><span class="sxs-lookup"><span data-stu-id="ea510-328">`disconnected`: Raised when the connection has disconnected.</span></span>

<span data-ttu-id="ea510-329">Par exemple, si vous souhaitez afficher les messages d’avertissement lorsqu’il existe des problèmes de connexion qui peuvent entraîner des retards, gérer la `connectionSlow` événement.</span><span class="sxs-lookup"><span data-stu-id="ea510-329">For example, if you want to display warning messages when there are connection problems that might cause noticeable delays, handle the `connectionSlow` event.</span></span>

<span data-ttu-id="ea510-330">**Gérer l’événement connectionSlow (avec le proxy généré)**</span><span class="sxs-lookup"><span data-stu-id="ea510-330">**Handle the connectionSlow event (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample46.js?highlight=1)]

<span data-ttu-id="ea510-331">**Gérer l’événement connectionSlow (sans le proxy généré)**</span><span class="sxs-lookup"><span data-stu-id="ea510-331">**Handle the connectionSlow event (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample47.js?highlight=2)]

<span data-ttu-id="ea510-332">Pour plus d’informations, consultez [compréhension et gestion des événements de durée de vie des connexions dans SignalR](handling-connection-lifetime-events.md).</span><span class="sxs-lookup"><span data-stu-id="ea510-332">For more information, see [Understanding and Handling Connection Lifetime Events in SignalR](handling-connection-lifetime-events.md).</span></span>

<a id="handleerrors"></a>

## <a name="how-to-handle-errors"></a><span data-ttu-id="ea510-333">Comment gérer les erreurs</span><span class="sxs-lookup"><span data-stu-id="ea510-333">How to handle errors</span></span>

<span data-ttu-id="ea510-334">Le client SignalR JavaScript fournit un `error` événement que vous pouvez ajouter un gestionnaire pour.</span><span class="sxs-lookup"><span data-stu-id="ea510-334">The SignalR JavaScript client provides an `error` event that you can add a handler for.</span></span> <span data-ttu-id="ea510-335">Vous pouvez également utiliser la méthode fail pour ajouter un gestionnaire pour les erreurs qui résultent d’un appel de méthode de serveur.</span><span class="sxs-lookup"><span data-stu-id="ea510-335">You can also use the fail method to add a handler for errors that result from a server method invocation.</span></span>

<span data-ttu-id="ea510-336">Si vous n’activez explicitement les messages d’erreur détaillés sur le serveur, l’objet d’exception SignalR retourne après une erreur contient un minimum d’informations sur l’erreur.</span><span class="sxs-lookup"><span data-stu-id="ea510-336">If you don't explicitly enable detailed error messages on the server, the exception object that SignalR returns after an error contains minimal information about the error.</span></span> <span data-ttu-id="ea510-337">Par exemple, si un appel à `newContosoChatMessage` échoue, le message d’erreur dans l’objet d’erreur contient «`There was an error invoking Hub method 'contosoChatHub.newContosoChatMessage'.`« envoi de messages d’erreur détaillés aux clients en production n’est pas recommandé pour des raisons de sécurité, mais si vous souhaitez activer les messages d’erreur détaillés pour à des fins de résolution des problèmes, utilisez le code suivant sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="ea510-337">For example, if a call to `newContosoChatMessage` fails, the error message in the error object contains "`There was an error invoking Hub method 'contosoChatHub.newContosoChatMessage'.`" Sending detailed error messages to clients in production is not recommended for security reasons, but if you want to enable detailed error messages for troubleshooting purposes, use the following code on the server.</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample48.cs?highlight=2)]

<span data-ttu-id="ea510-338">L’exemple suivant montre comment ajouter un gestionnaire pour l’événement d’erreur.</span><span class="sxs-lookup"><span data-stu-id="ea510-338">The following example shows how to add a handler for the error event.</span></span>

<span data-ttu-id="ea510-339">**Ajouter un gestionnaire d’erreurs (avec le proxy généré)**</span><span class="sxs-lookup"><span data-stu-id="ea510-339">**Add an error handler (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample49.js?highlight=1)]

<span data-ttu-id="ea510-340">**Ajouter un gestionnaire d’erreurs (sans le proxy généré)**</span><span class="sxs-lookup"><span data-stu-id="ea510-340">**Add an error handler (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample50.js?highlight=2)]

<span data-ttu-id="ea510-341">L’exemple suivant montre comment gérer une erreur à partir d’un appel de méthode.</span><span class="sxs-lookup"><span data-stu-id="ea510-341">The following example shows how to handle an error from a method invocation.</span></span>

<span data-ttu-id="ea510-342">**Gérer une erreur à partir d’un appel de méthode (avec le proxy généré)**</span><span class="sxs-lookup"><span data-stu-id="ea510-342">**Handle an error from a method invocation (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample51.js?highlight=2)]

<span data-ttu-id="ea510-343">**Gérer une erreur à partir d’un appel de méthode (sans le proxy généré)**</span><span class="sxs-lookup"><span data-stu-id="ea510-343">**Handle an error from a method invocation (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample52.js?highlight=2)]

<span data-ttu-id="ea510-344">Si un appel de méthode échoue, le `error` événement est également déclenché, de sorte que votre code dans le `error` Gestionnaire de méthode et dans le `.fail` s’exécuterait de rappel de méthode.</span><span class="sxs-lookup"><span data-stu-id="ea510-344">If a method invocation fails, the `error` event is also raised, so your code in the `error` method handler and in the `.fail` method callback would execute.</span></span>

<a id="logging"></a>

## <a name="how-to-enable-client-side-logging"></a><span data-ttu-id="ea510-345">Comment activer la journalisation côté client</span><span class="sxs-lookup"><span data-stu-id="ea510-345">How to enable client-side logging</span></span>

<span data-ttu-id="ea510-346">Pour activer la journalisation côté client sur une connexion, définissez la `logging` propriété sur l’objet de connexion avant d’appeler le `start` méthode pour établir la connexion.</span><span class="sxs-lookup"><span data-stu-id="ea510-346">To enable client-side logging on a connection, set the `logging` property on the connection object before you call the `start` method to establish the connection.</span></span>

<span data-ttu-id="ea510-347">**Activer la journalisation (avec le proxy généré)**</span><span class="sxs-lookup"><span data-stu-id="ea510-347">**Enable logging (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample53.js?highlight=1)]

<span data-ttu-id="ea510-348">**Activer la journalisation (sans le proxy généré)**</span><span class="sxs-lookup"><span data-stu-id="ea510-348">**Enable logging (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample54.js?highlight=2)]

<span data-ttu-id="ea510-349">Pour afficher les journaux, ouvrir les outils de développement de votre navigateur et accédez à l’onglet de la Console. Pour obtenir un didacticiel qui montre des instructions pas à pas et l’écran de captures qui montrent comment effectuer cette opération, consultez [diffusion par le serveur avec ASP.NET Signalr - activer la journalisation](../getting-started/tutorial-server-broadcast-with-signalr.md#enable-logging).</span><span class="sxs-lookup"><span data-stu-id="ea510-349">To see the logs, open your browser's developer tools and go to the Console tab. For a tutorial that shows step-by-step instructions and screen shots that show how to do this, see [Server Broadcast with ASP.NET Signalr - Enable Logging](../getting-started/tutorial-server-broadcast-with-signalr.md#enable-logging).</span></span>
