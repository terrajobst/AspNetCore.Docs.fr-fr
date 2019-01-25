---
uid: signalr/overview/testing-and-debugging/enabling-signalr-tracing
title: Activation du traçage SignalR | Microsoft Docs
author: bradygaster
description: Ce document décrit comment activer et configurer le traçage des clients et serveurs de SignalR. Le suivi vous permet de vous permettent d’afficher des informations sur les événements de diagnostic...
ms.author: bradyg
ms.date: 08/08/2014
ms.assetid: 30060acb-be3e-4347-996f-3870f0c37829
msc.legacyurl: /signalr/overview/testing-and-debugging/enabling-signalr-tracing
msc.type: authoredcontent
ms.openlocfilehash: c6515e3653798ef50e2d2dcb7354ffed407a190e
ms.sourcegitcommit: ebf4e5a7ca301af8494edf64f85d4a8deb61d641
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/24/2019
ms.locfileid: "54836465"
---
<a name="enabling-signalr-tracing"></a><span data-ttu-id="b0c40-104">Activation du traçage SignalR</span><span class="sxs-lookup"><span data-stu-id="b0c40-104">Enabling SignalR Tracing</span></span>
====================
<span data-ttu-id="b0c40-105">par [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="b0c40-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> <span data-ttu-id="b0c40-106">Ce document décrit comment activer et configurer le traçage des clients et serveurs de SignalR.</span><span class="sxs-lookup"><span data-stu-id="b0c40-106">This document describes how to enable and configure tracing for SignalR servers and clients.</span></span> <span data-ttu-id="b0c40-107">Traçage permet d’afficher des informations sur les événements de diagnostic dans votre application de SignalR.</span><span class="sxs-lookup"><span data-stu-id="b0c40-107">Tracing enables you to view diagnostic information about events in your SignalR application.</span></span>
>
> <span data-ttu-id="b0c40-108">Cette rubrique a été écrit à l’origine par Patrick Fletcher.</span><span class="sxs-lookup"><span data-stu-id="b0c40-108">This topic was originally written by Patrick Fletcher.</span></span>
>
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="b0c40-109">Versions des logiciels utilisées dans le didacticiel</span><span class="sxs-lookup"><span data-stu-id="b0c40-109">Software versions used in the tutorial</span></span>
>
>
> - [<span data-ttu-id="b0c40-110">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="b0c40-110">Visual Studio 2013</span></span>](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)
> - <span data-ttu-id="b0c40-111">.NET Framework 4.5</span><span class="sxs-lookup"><span data-stu-id="b0c40-111">.NET Framework 4.5</span></span>
> - <span data-ttu-id="b0c40-112">SignalR version 2</span><span class="sxs-lookup"><span data-stu-id="b0c40-112">SignalR version 2</span></span>
>
>
>
> ## <a name="questions-and-comments"></a><span data-ttu-id="b0c40-113">Questions et commentaires</span><span class="sxs-lookup"><span data-stu-id="b0c40-113">Questions and comments</span></span>
>
> <span data-ttu-id="b0c40-114">Veuillez laisser des commentaires sur la façon dont vous avez apprécié ce didacticiel et ce que nous pouvions améliorer dans les commentaires en bas de la page.</span><span class="sxs-lookup"><span data-stu-id="b0c40-114">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="b0c40-115">Si vous avez des questions qui ne sont pas directement liées à ce didacticiel, vous pouvez les publier à le [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) ou [StackOverflow.com](http://stackoverflow.com/).</span><span class="sxs-lookup"><span data-stu-id="b0c40-115">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>


<span data-ttu-id="b0c40-116">Lorsque le traçage est activé, une application de SignalR crée des entrées de journal des événements.</span><span class="sxs-lookup"><span data-stu-id="b0c40-116">When tracing is enabled, a SignalR application creates log entries for events.</span></span> <span data-ttu-id="b0c40-117">Vous pouvez consigner les événements provenant du client et le serveur.</span><span class="sxs-lookup"><span data-stu-id="b0c40-117">You can log events from both the client and the server.</span></span> <span data-ttu-id="b0c40-118">Le suivi sur la connexion du serveur de journaux, fournisseur de montée en puissance parallèle et événements de bus de messages.</span><span class="sxs-lookup"><span data-stu-id="b0c40-118">Tracing on the server logs connection, scaleout provider, and message bus events.</span></span> <span data-ttu-id="b0c40-119">Le suivi sur les événements de connexion de journaux de clients.</span><span class="sxs-lookup"><span data-stu-id="b0c40-119">Tracing on the client logs connection events.</span></span> <span data-ttu-id="b0c40-120">Dans SignalR 2.1 et versions ultérieures, le suivi sur le client enregistre le contenu complet des messages d’appel de concentrateur.</span><span class="sxs-lookup"><span data-stu-id="b0c40-120">In SignalR 2.1 and later, tracing on the client logs the full content of hub invocation messages.</span></span>

## <a name="contents"></a><span data-ttu-id="b0c40-121">Sommaire</span><span class="sxs-lookup"><span data-stu-id="b0c40-121">Contents</span></span>

- [<span data-ttu-id="b0c40-122">L’activation du suivi sur le serveur</span><span class="sxs-lookup"><span data-stu-id="b0c40-122">Enabling tracing on the server</span></span>](#server)

    - [<span data-ttu-id="b0c40-123">Journalisation des événements de serveur dans des fichiers texte</span><span class="sxs-lookup"><span data-stu-id="b0c40-123">Logging server events to text files</span></span>](#server_text)
    - [<span data-ttu-id="b0c40-124">Journalisation des événements de serveur dans le journal des événements</span><span class="sxs-lookup"><span data-stu-id="b0c40-124">Logging server events to the event log</span></span>](#server_eventlog)
- [<span data-ttu-id="b0c40-125">L’activation du traçage dans le client .NET (applications de bureau de Windows)</span><span class="sxs-lookup"><span data-stu-id="b0c40-125">Enabling tracing in the .NET client (Windows Desktop apps)</span></span>](#net_client)

    - [<span data-ttu-id="b0c40-126">Journalisation des événements de client de bureau dans la console</span><span class="sxs-lookup"><span data-stu-id="b0c40-126">Logging Desktop client events to the console</span></span>](#desktop_console)
    - [<span data-ttu-id="b0c40-127">Journalisation des événements de client de bureau dans un fichier texte</span><span class="sxs-lookup"><span data-stu-id="b0c40-127">Logging Desktop client events to a text file</span></span>](#desktop_text)
- [<span data-ttu-id="b0c40-128">L’activation du traçage dans les clients Windows Phone 8</span><span class="sxs-lookup"><span data-stu-id="b0c40-128">Enabling tracing in Windows Phone 8 clients</span></span>](#phone)

    - [<span data-ttu-id="b0c40-129">Journalisation des événements du client Windows Phone à l’interface utilisateur</span><span class="sxs-lookup"><span data-stu-id="b0c40-129">Logging Windows Phone client events to the UI</span></span>](#phone_ui)
    - [<span data-ttu-id="b0c40-130">Journalisation des événements du client Windows Phone à la console de débogage</span><span class="sxs-lookup"><span data-stu-id="b0c40-130">Logging Windows Phone client events to the debug console</span></span>](#phone_debug)
- [<span data-ttu-id="b0c40-131">L’activation du traçage dans le client JavaScript</span><span class="sxs-lookup"><span data-stu-id="b0c40-131">Enabling tracing in the JavaScript client</span></span>](#javascript)

<a id="server"></a>
## <a name="enabling-tracing-on-the-server"></a><span data-ttu-id="b0c40-132">L’activation du suivi sur le serveur</span><span class="sxs-lookup"><span data-stu-id="b0c40-132">Enabling tracing on the server</span></span>

<span data-ttu-id="b0c40-133">Vous activez le traçage sur le serveur dans le fichier de configuration de l’application (App.config ou Web.config en fonction du type de projet.) Vous spécifiez les catégories d’événements que vous souhaitez journaliser.</span><span class="sxs-lookup"><span data-stu-id="b0c40-133">You enable tracing on the server within the application's configuration file (either App.config or Web.config depending on the type of project.) You specify which categories of events you want to log.</span></span> <span data-ttu-id="b0c40-134">Dans le fichier de configuration, vous spécifiez également s’il faut enregistrer les événements dans un fichier texte, le journal des événements Windows ou un journal personnalisé à l’aide d’une implémentation de [TraceListener](https://msdn.microsoft.com/library/system.diagnostics.tracelistener(v=vs.110).aspx).</span><span class="sxs-lookup"><span data-stu-id="b0c40-134">In the configuration file, you also specify whether to log the events to a text file, the Windows event log, or a custom log using an implementation of [TraceListener](https://msdn.microsoft.com/library/system.diagnostics.tracelistener(v=vs.110).aspx).</span></span>

<span data-ttu-id="b0c40-135">Les catégories d’événements de serveur incluent les types de messages suivants :</span><span class="sxs-lookup"><span data-stu-id="b0c40-135">The server event categories include the following sorts of messages:</span></span>

| <span data-ttu-id="b0c40-136">Source</span><span class="sxs-lookup"><span data-stu-id="b0c40-136">Source</span></span> | <span data-ttu-id="b0c40-137">Messages</span><span class="sxs-lookup"><span data-stu-id="b0c40-137">Messages</span></span> |
| --- | --- |
| <span data-ttu-id="b0c40-138">SignalR.SqlMessageBus</span><span class="sxs-lookup"><span data-stu-id="b0c40-138">SignalR.SqlMessageBus</span></span> | <span data-ttu-id="b0c40-139">Le programme d’installation fournisseur de Bus de messages SQL montée en puissance parallèle, opération de base de données, erreur et les événements de délai d’attente</span><span class="sxs-lookup"><span data-stu-id="b0c40-139">SQL Message Bus scaleout provider setup, database operation, error, and timeout events</span></span> |
| <span data-ttu-id="b0c40-140">SignalR.ServiceBusMessageBus</span><span class="sxs-lookup"><span data-stu-id="b0c40-140">SignalR.ServiceBusMessageBus</span></span> | <span data-ttu-id="b0c40-141">Création de rubrique service bus montée en puissance parallèle fournisseur et abonnement, erreurs et événements de messagerie</span><span class="sxs-lookup"><span data-stu-id="b0c40-141">Service bus scaleout provider topic creation and subscription, error, and messaging events</span></span> |
| <span data-ttu-id="b0c40-142">SignalR.RedisMessageBus</span><span class="sxs-lookup"><span data-stu-id="b0c40-142">SignalR.RedisMessageBus</span></span> | <span data-ttu-id="b0c40-143">Événements de connexion, déconnexion et d’erreur de fournisseur de montée en puissance parallèle de redis</span><span class="sxs-lookup"><span data-stu-id="b0c40-143">Redis scaleout provider connection, disconnection, and error events</span></span> |
| <span data-ttu-id="b0c40-144">SignalR.ScaleoutMessageBus</span><span class="sxs-lookup"><span data-stu-id="b0c40-144">SignalR.ScaleoutMessageBus</span></span> | <span data-ttu-id="b0c40-145">Événements de messagerie de montée en puissance parallèle</span><span class="sxs-lookup"><span data-stu-id="b0c40-145">Scaleout messaging events</span></span> |
| <span data-ttu-id="b0c40-146">SignalR.Transports.WebSocketTransport</span><span class="sxs-lookup"><span data-stu-id="b0c40-146">SignalR.Transports.WebSocketTransport</span></span> | <span data-ttu-id="b0c40-147">Événements de connexion, déconnexion, messagerie et d’erreur de transport WebSocket</span><span class="sxs-lookup"><span data-stu-id="b0c40-147">WebSocket transport connection, disconnection, messaging, and error events</span></span> |
| <span data-ttu-id="b0c40-148">SignalR.Transports.ServerSentEventsTransport</span><span class="sxs-lookup"><span data-stu-id="b0c40-148">SignalR.Transports.ServerSentEventsTransport</span></span> | <span data-ttu-id="b0c40-149">Événements de connexion, déconnexion, messagerie et d’erreur de transport ServerSentEvents</span><span class="sxs-lookup"><span data-stu-id="b0c40-149">ServerSentEvents transport connection, disconnection, messaging, and error events</span></span> |
| <span data-ttu-id="b0c40-150">SignalR.Transports.ForeverFrameTransport</span><span class="sxs-lookup"><span data-stu-id="b0c40-150">SignalR.Transports.ForeverFrameTransport</span></span> | <span data-ttu-id="b0c40-151">Événements de connexion, déconnexion, messagerie et d’erreur de transport ForeverFrame</span><span class="sxs-lookup"><span data-stu-id="b0c40-151">ForeverFrame transport connection, disconnection, messaging, and error events</span></span> |
| <span data-ttu-id="b0c40-152">SignalR.Transports.LongPollingTransport</span><span class="sxs-lookup"><span data-stu-id="b0c40-152">SignalR.Transports.LongPollingTransport</span></span> | <span data-ttu-id="b0c40-153">Événements de connexion, déconnexion, messagerie et d’erreur de transport LongPolling</span><span class="sxs-lookup"><span data-stu-id="b0c40-153">LongPolling transport connection, disconnection, messaging, and error events</span></span> |
| <span data-ttu-id="b0c40-154">SignalR.Transports.TransportHeartBeat</span><span class="sxs-lookup"><span data-stu-id="b0c40-154">SignalR.Transports.TransportHeartBeat</span></span> | <span data-ttu-id="b0c40-155">Événements de connexion, déconnexion et keepalive de transport</span><span class="sxs-lookup"><span data-stu-id="b0c40-155">Transport connection, disconnection, and keepalive events</span></span> |
| <span data-ttu-id="b0c40-156">SignalR.ReflectedHubDescriptorProvider</span><span class="sxs-lookup"><span data-stu-id="b0c40-156">SignalR.ReflectedHubDescriptorProvider</span></span> | <span data-ttu-id="b0c40-157">Événements de découverte de hub</span><span class="sxs-lookup"><span data-stu-id="b0c40-157">Hub discovery events</span></span> |

<a id="server_text"></a>
### <a name="logging-server-events-to-text-files"></a><span data-ttu-id="b0c40-158">Journalisation des événements de serveur dans des fichiers texte</span><span class="sxs-lookup"><span data-stu-id="b0c40-158">Logging server events to text files</span></span>

<span data-ttu-id="b0c40-159">Le code suivant montre comment activer le suivi pour chaque catégorie d’événement.</span><span class="sxs-lookup"><span data-stu-id="b0c40-159">The following code shows how to enable tracing for each category of event.</span></span> <span data-ttu-id="b0c40-160">Cet exemple configure l’application pour consigner des événements dans des fichiers texte.</span><span class="sxs-lookup"><span data-stu-id="b0c40-160">This sample configures the application to log events to text files.</span></span>

<span data-ttu-id="b0c40-161">**Code de serveur XML pour l’activation du suivi**</span><span class="sxs-lookup"><span data-stu-id="b0c40-161">**XML server code for enabling tracing**</span></span>

[!code-html[Main](enabling-signalr-tracing/samples/sample1.html)]

<span data-ttu-id="b0c40-162">Dans le code ci-dessus, le `SignalRSwitch` entrée spécifie le [TraceLevel](https://msdn.microsoft.com/library/system.diagnostics.tracelevel(v=vs.110).aspx) utilisés pour les événements envoyés dans le journal spécifié.</span><span class="sxs-lookup"><span data-stu-id="b0c40-162">In the code above, the `SignalRSwitch` entry specifies the [TraceLevel](https://msdn.microsoft.com/library/system.diagnostics.tracelevel(v=vs.110).aspx) used for events sent to the specified log.</span></span> <span data-ttu-id="b0c40-163">Dans ce cas, il est défini `Verbose` ce qui signifie que tous les messages de traçage et de débogage sont enregistrés.</span><span class="sxs-lookup"><span data-stu-id="b0c40-163">In this case, it is set to `Verbose` which means all debugging and tracing messages are logged.</span></span>

<span data-ttu-id="b0c40-164">La sortie suivante montre les entrées lors de la `transports.log.txt` fichier pour une application utilisant le fichier de configuration ci-dessus.</span><span class="sxs-lookup"><span data-stu-id="b0c40-164">The following output shows entries from the `transports.log.txt` file for an application using the above configuration file.</span></span> <span data-ttu-id="b0c40-165">Il montre une nouvelle connexion, une connexion supprimée et les événements de pulsation de transport.</span><span class="sxs-lookup"><span data-stu-id="b0c40-165">It shows a new connection, a removed connection, and transport heartbeat events.</span></span>

[!code-console[Main](enabling-signalr-tracing/samples/sample2.cmd)]

<a id="server_eventlog"></a>
### <a name="logging-server-events-to-the-event-log"></a><span data-ttu-id="b0c40-166">Journalisation des événements de serveur dans le journal des événements</span><span class="sxs-lookup"><span data-stu-id="b0c40-166">Logging server events to the event log</span></span>

<span data-ttu-id="b0c40-167">Pour enregistrer les événements dans le journal des événements plutôt que d’un fichier texte, modifiez les valeurs pour les entrées dans le `sharedListeners` nœud.</span><span class="sxs-lookup"><span data-stu-id="b0c40-167">To log events to the event log rather than a text file, change the values for the entries in the `sharedListeners` node.</span></span> <span data-ttu-id="b0c40-168">Le code suivant montre comment enregistrer les événements de serveur dans le journal des événements :</span><span class="sxs-lookup"><span data-stu-id="b0c40-168">The following code shows how to log server events to the event log:</span></span>

<span data-ttu-id="b0c40-169">**Code de serveur XML pour la journalisation des événements dans le journal des événements**</span><span class="sxs-lookup"><span data-stu-id="b0c40-169">**XML server code for logging events to the event log**</span></span>

[!code-xml[Main](enabling-signalr-tracing/samples/sample3.xml)]

<span data-ttu-id="b0c40-170">Les événements sont consignés dans le journal des applications et sont disponibles via l’Observateur d’événements, comme indiqué ci-dessous :</span><span class="sxs-lookup"><span data-stu-id="b0c40-170">The events are logged in the Application log, and are available through the Event Viewer, as shown below:</span></span>

![Afficher les journaux de SignalR l’Observateur d’événements](enabling-signalr-tracing/_static/image1.png)

> [!NOTE]
> <span data-ttu-id="b0c40-172">Le journal des événements, définissez la **TraceLevel** à **erreur** pour que le nombre de messages reste gérable.</span><span class="sxs-lookup"><span data-stu-id="b0c40-172">When using the event log, set the **TraceLevel** to **Error** to keep the number of messages manageable.</span></span>

<a id="net_client"></a>
## <a name="enabling-tracing-in-the-net-client-windows-desktop-apps"></a><span data-ttu-id="b0c40-173">L’activation du traçage dans le client .NET (applications de bureau de Windows)</span><span class="sxs-lookup"><span data-stu-id="b0c40-173">Enabling tracing in the .NET client (Windows Desktop apps)</span></span>

<span data-ttu-id="b0c40-174">Le client .NET peut se connecter à des événements à la console, un fichier texte, ou dans un journal personnalisé à l’aide d’une implémentation de [TextWriter](https://msdn.microsoft.com/library/system.io.textwriter.aspx).</span><span class="sxs-lookup"><span data-stu-id="b0c40-174">The .NET client can log events to the console, a text file, or to a custom log using an implementation of [TextWriter](https://msdn.microsoft.com/library/system.io.textwriter.aspx).</span></span>

<span data-ttu-id="b0c40-175">Pour activer la journalisation dans le client .NET, définissez la connexion `TraceLevel` propriété à un [TraceLevels](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.tracelevels(v=vs.118).aspx) valeur et le `TraceWriter` propriété valide [TextWriter](https://msdn.microsoft.com/library/system.io.textwriter.aspx) instance.</span><span class="sxs-lookup"><span data-stu-id="b0c40-175">To enable logging in the .NET client, set the connection's `TraceLevel` property to a [TraceLevels](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.tracelevels(v=vs.118).aspx) value, and the `TraceWriter` property to a valid [TextWriter](https://msdn.microsoft.com/library/system.io.textwriter.aspx) instance.</span></span>

<a id="desktop_console"></a>
### <a name="logging-desktop-client-events-to-the-console"></a><span data-ttu-id="b0c40-176">Journalisation des événements de client de bureau dans la console</span><span class="sxs-lookup"><span data-stu-id="b0c40-176">Logging Desktop client events to the console</span></span>

<span data-ttu-id="b0c40-177">Le code c# suivant montre comment enregistrer les événements dans le client .NET dans la console :</span><span class="sxs-lookup"><span data-stu-id="b0c40-177">The following C# code shows how to log events in the .NET client to the console:</span></span>

[!code-csharp[Main](enabling-signalr-tracing/samples/sample4.cs?highlight=2-3)]

<a id="desktop_text"></a>
### <a name="logging-desktop-client-events-to-a-text-file"></a><span data-ttu-id="b0c40-178">Journalisation des événements de client de bureau dans un fichier texte</span><span class="sxs-lookup"><span data-stu-id="b0c40-178">Logging Desktop client events to a text file</span></span>

<span data-ttu-id="b0c40-179">Le code c# suivant montre comment enregistrer les événements dans le client .NET dans un fichier texte :</span><span class="sxs-lookup"><span data-stu-id="b0c40-179">The following C# code shows how to log events in the .NET client to a text file:</span></span>

[!code-csharp[Main](enabling-signalr-tracing/samples/sample5.cs?highlight=4-5)]

<span data-ttu-id="b0c40-180">La sortie suivante montre les entrées lors de la `ClientLog.txt` fichier pour une application utilisant le fichier de configuration ci-dessus.</span><span class="sxs-lookup"><span data-stu-id="b0c40-180">The following output shows entries from the `ClientLog.txt` file for an application using the above configuration file.</span></span> <span data-ttu-id="b0c40-181">Elle indique le client qui se connecte au serveur et le hub d’appeler une méthode client appelé `addMessage`:</span><span class="sxs-lookup"><span data-stu-id="b0c40-181">It shows the client connecting to the server, and the hub invoking a client method called `addMessage`:</span></span>

[!code-console[Main](enabling-signalr-tracing/samples/sample6.cmd)]

<a id="phone"></a>
## <a name="enabling-tracing-in-windows-phone-8-clients"></a><span data-ttu-id="b0c40-182">L’activation du traçage dans les clients Windows Phone 8</span><span class="sxs-lookup"><span data-stu-id="b0c40-182">Enabling tracing in Windows Phone 8 clients</span></span>

<span data-ttu-id="b0c40-183">Applications de SignalR pour les applications Windows Phone utilisent le même client .NET en tant qu’applications de bureau, mais [Console.Out](https://msdn.microsoft.com/library/system.console.out(v=vs.110).aspx) et l’écriture dans un fichier avec [StreamWriter](https://msdn.microsoft.com/library/system.io.streamwriter(v=vs.110).aspx) ne sont pas disponibles.</span><span class="sxs-lookup"><span data-stu-id="b0c40-183">SignalR applications for Windows Phone apps use the same .NET client as desktop apps, but [Console.Out](https://msdn.microsoft.com/library/system.console.out(v=vs.110).aspx) and writing to a file with [StreamWriter](https://msdn.microsoft.com/library/system.io.streamwriter(v=vs.110).aspx) are not available.</span></span> <span data-ttu-id="b0c40-184">Au lieu de cela, vous devez créer une implémentation personnalisée de [TextWriter](https://msdn.microsoft.com/library/system.io.textwriter(v=vs.110).aspx) pour le suivi.</span><span class="sxs-lookup"><span data-stu-id="b0c40-184">Instead, you need to create a custom implementation of [TextWriter](https://msdn.microsoft.com/library/system.io.textwriter(v=vs.110).aspx) for tracing.</span></span>

<a id="phone_ui"></a>
### <a name="logging-windows-phone-client-events-to-the-ui"></a><span data-ttu-id="b0c40-185">Journalisation des événements du client Windows Phone à l’interface utilisateur</span><span class="sxs-lookup"><span data-stu-id="b0c40-185">Logging Windows Phone client events to the UI</span></span>

<span data-ttu-id="b0c40-186">Le [SignalR codebase](https://github.com/SignalR/SignalR/archive/master.zip) inclut un exemple de Windows Phone qui écrit la sortie de trace dans un [TextBlock](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.aspx) l’aide d’un [TextWriter](https://msdn.microsoft.com/library/system.io.textwriter(v=vs.110).aspx) implémentation appelée `TextBlockWriter`.</span><span class="sxs-lookup"><span data-stu-id="b0c40-186">The [SignalR codebase](https://github.com/SignalR/SignalR/archive/master.zip) includes a Windows Phone sample that writes trace output to a [TextBlock](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.aspx) using a custom [TextWriter](https://msdn.microsoft.com/library/system.io.textwriter(v=vs.110).aspx) implementation called `TextBlockWriter`.</span></span> <span data-ttu-id="b0c40-187">Cette classe peut être trouvée dans le **samples/Microsoft.AspNet.SignalR.Client.WP8.Samples** projet.</span><span class="sxs-lookup"><span data-stu-id="b0c40-187">This class can be found in the **samples/Microsoft.AspNet.SignalR.Client.WP8.Samples** project.</span></span> <span data-ttu-id="b0c40-188">Lors de la création d’une instance de `TextBlockWriter`, transmettez actuel [SynchronizationContext](https://msdn.microsoft.com/library/system.threading.synchronizationcontext(v=vs.110).aspx)et un [StackPanel](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.stackpanel.aspx) où il créera un [TextBlock](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.aspx) à utiliser pour la trace sortie :</span><span class="sxs-lookup"><span data-stu-id="b0c40-188">When creating an instance of `TextBlockWriter`, pass in the current [SynchronizationContext](https://msdn.microsoft.com/library/system.threading.synchronizationcontext(v=vs.110).aspx), and a [StackPanel](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.stackpanel.aspx) where it will create a [TextBlock](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.aspx) to use for trace output:</span></span>

[!code-csharp[Main](enabling-signalr-tracing/samples/sample7.cs)]

<span data-ttu-id="b0c40-189">La sortie de trace sera ensuite être écrites dans un nouveau [TextBlock](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.aspx) créé dans le [StackPanel](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.stackpanel.aspx) vous avez passé dans :</span><span class="sxs-lookup"><span data-stu-id="b0c40-189">The trace output will then be written to a new [TextBlock](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.aspx) created in the [StackPanel](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.stackpanel.aspx) you passed in:</span></span>

![](enabling-signalr-tracing/_static/image2.png)

<a id="phone_debug"></a>
### <a name="logging-windows-phone-client-events-to-the-debug-console"></a><span data-ttu-id="b0c40-190">Journalisation des événements du client Windows Phone à la console de débogage</span><span class="sxs-lookup"><span data-stu-id="b0c40-190">Logging Windows Phone client events to the debug console</span></span>

<span data-ttu-id="b0c40-191">Pour envoyer la sortie de la console de débogage plutôt que l’interface utilisateur, créer une implémentation de [TextWriter](https://msdn.microsoft.com/library/system.io.textwriter(v=vs.110).aspx) qui écrit dans la fenêtre de débogage et affectez-le à de votre connexion [TraceWriter](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.connection.tracewriter(v=vs.118).aspx) propriété :</span><span class="sxs-lookup"><span data-stu-id="b0c40-191">To send output to the debug console rather than the UI, create an implementation of [TextWriter](https://msdn.microsoft.com/library/system.io.textwriter(v=vs.110).aspx) that writes to the debug window, and assign it to your connection's [TraceWriter](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.connection.tracewriter(v=vs.118).aspx) property:</span></span>

[!code-csharp[Main](enabling-signalr-tracing/samples/sample8.cs)]

<span data-ttu-id="b0c40-192">Informations de trace seront ensuite être écrites dans la fenêtre de débogage dans Visual Studio :</span><span class="sxs-lookup"><span data-stu-id="b0c40-192">Trace information will then be written to the debug window in Visual Studio:</span></span>

![](enabling-signalr-tracing/_static/image3.png)

<a id="javascript"></a>
## <a name="enabling-tracing-in-the-javascript-client"></a><span data-ttu-id="b0c40-193">L’activation du traçage dans le client JavaScript</span><span class="sxs-lookup"><span data-stu-id="b0c40-193">Enabling tracing in the JavaScript client</span></span>

<span data-ttu-id="b0c40-194">Pour activer la journalisation côté client sur une connexion, définissez la `logging` propriété sur l’objet de connexion avant d’appeler le `start` méthode pour établir la connexion.</span><span class="sxs-lookup"><span data-stu-id="b0c40-194">To enable client-side logging on a connection, set the `logging` property on the connection object before you call the `start` method to establish the connection.</span></span>

<span data-ttu-id="b0c40-195">**Code JavaScript client pour l’activation du suivi pour la console du navigateur (avec le proxy généré)**</span><span class="sxs-lookup"><span data-stu-id="b0c40-195">**Client JavaScript code for enabling tracing to the browser console (with the generated proxy)**</span></span>

[!code-javascript[Main](enabling-signalr-tracing/samples/sample9.js?highlight=1)]

<span data-ttu-id="b0c40-196">**Code JavaScript client pour l’activation du suivi pour la console du navigateur (sans le proxy généré)**</span><span class="sxs-lookup"><span data-stu-id="b0c40-196">**Client JavaScript code for enabling tracing to the browser console (without the generated proxy)**</span></span>

[!code-javascript[Main](enabling-signalr-tracing/samples/sample10.js?highlight=2)]

<span data-ttu-id="b0c40-197">Lorsque le traçage est activé, le client JavaScript consigne les événements dans la console du navigateur.</span><span class="sxs-lookup"><span data-stu-id="b0c40-197">When tracing is enabled, the JavaScript client logs events to the browser console.</span></span> <span data-ttu-id="b0c40-198">Pour accéder à la console du navigateur, consultez [Transports surveillance](../getting-started/introduction-to-signalr.md#MonitoringTransports).</span><span class="sxs-lookup"><span data-stu-id="b0c40-198">To access the browser console, see [Monitoring Transports](../getting-started/introduction-to-signalr.md#MonitoringTransports).</span></span>

<span data-ttu-id="b0c40-199">La capture d’écran suivante montre un client SignalR JavaScript avec le suivi est activé.</span><span class="sxs-lookup"><span data-stu-id="b0c40-199">The following screenshot shows a SignalR JavaScript client with tracing enabled.</span></span> <span data-ttu-id="b0c40-200">Il montre la connexion et les événements d’appel de concentrateur dans la console du navigateur :</span><span class="sxs-lookup"><span data-stu-id="b0c40-200">It shows connection and hub invocation events in the browser console:</span></span>

![Événements de traçage SignalR dans la console du navigateur](enabling-signalr-tracing/_static/image4.png)
