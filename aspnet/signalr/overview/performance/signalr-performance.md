---
uid: signalr/overview/performance/signalr-performance
title: Performances de SignalR | Microsoft Docs
author: pfletcher
description: Performances de SignalR
ms.author: riande
ms.date: 06/10/2014
ms.assetid: 3751f5e7-59db-4be0-a290-50abc24e5c84
msc.legacyurl: /signalr/overview/performance/signalr-performance
msc.type: authoredcontent
ms.openlocfilehash: 269c10d7a73f181eaceac1c43ad51f3933d6711c
ms.sourcegitcommit: a4dcca4f1cb81227c5ed3c92dc0e28be6e99447b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/10/2018
ms.locfileid: "48911856"
---
<a name="signalr-performance"></a><span data-ttu-id="cabc9-103">Performances de SignalR</span><span class="sxs-lookup"><span data-stu-id="cabc9-103">SignalR Performance</span></span>
====================
<span data-ttu-id="cabc9-104">par [Patrick Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="cabc9-104">by [Patrick Fletcher](https://github.com/pfletcher)</span></span>

> <span data-ttu-id="cabc9-105">Cette rubrique décrit comment concevoir pour, mesurer et améliorer les performances dans une application de SignalR.</span><span class="sxs-lookup"><span data-stu-id="cabc9-105">This topic describes how to design for, measure, and improve performance in a SignalR application.</span></span>
>
> ## <a name="software-versions-used-in-this-topic"></a><span data-ttu-id="cabc9-106">Versions des logiciels utilisées dans cette rubrique</span><span class="sxs-lookup"><span data-stu-id="cabc9-106">Software versions used in this topic</span></span>
>
>
> - [<span data-ttu-id="cabc9-107">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="cabc9-107">Visual Studio 2013</span></span>](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)
> - <span data-ttu-id="cabc9-108">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="cabc9-108">.NET 4.5</span></span>
> - <span data-ttu-id="cabc9-109">SignalR version 2</span><span class="sxs-lookup"><span data-stu-id="cabc9-109">SignalR version 2</span></span>
>
>
>
> ## <a name="previous-versions-of-this-topic"></a><span data-ttu-id="cabc9-110">Versions précédentes de cette rubrique</span><span class="sxs-lookup"><span data-stu-id="cabc9-110">Previous versions of this topic</span></span>
>
> <span data-ttu-id="cabc9-111">Pour plus d’informations sur les versions antérieures de SignalR, consultez [les Versions antérieures de SignalR](../older-versions/index.md).</span><span class="sxs-lookup"><span data-stu-id="cabc9-111">For information about earlier versions of SignalR, see [SignalR Older Versions](../older-versions/index.md).</span></span>
>
> ## <a name="questions-and-comments"></a><span data-ttu-id="cabc9-112">Questions et commentaires</span><span class="sxs-lookup"><span data-stu-id="cabc9-112">Questions and comments</span></span>
>
> <span data-ttu-id="cabc9-113">Veuillez laisser des commentaires sur la façon dont vous avez apprécié ce didacticiel et ce que nous pouvions améliorer dans les commentaires en bas de la page.</span><span class="sxs-lookup"><span data-stu-id="cabc9-113">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="cabc9-114">Si vous avez des questions qui ne sont pas directement liées à ce didacticiel, vous pouvez les publier à le [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) ou [StackOverflow.com](http://stackoverflow.com/).</span><span class="sxs-lookup"><span data-stu-id="cabc9-114">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>


<span data-ttu-id="cabc9-115">Pour obtenir une présentation récente sur les performances de SignalR et de la mise à l’échelle, consultez [mise à l’échelle du Web en temps réel avec ASP.NET SignalR](https://channel9.msdn.com/Events/Build/2013/3-502).</span><span class="sxs-lookup"><span data-stu-id="cabc9-115">For a recent presentation on SignalR performance and scaling, see [Scaling the Real-time Web with ASP.NET SignalR](https://channel9.msdn.com/Events/Build/2013/3-502).</span></span>

<span data-ttu-id="cabc9-116">Cette rubrique contient les sections suivantes :</span><span class="sxs-lookup"><span data-stu-id="cabc9-116">This topic contains the following sections:</span></span>

- [<span data-ttu-id="cabc9-117">Considérations de conception</span><span class="sxs-lookup"><span data-stu-id="cabc9-117">Design considerations</span></span>](#design)
- [<span data-ttu-id="cabc9-118">Réglage des performances de votre serveur SignalR</span><span class="sxs-lookup"><span data-stu-id="cabc9-118">Tuning your SignalR server for performance</span></span>](#tuning)
- [<span data-ttu-id="cabc9-119">Résolution des problèmes de performances</span><span class="sxs-lookup"><span data-stu-id="cabc9-119">Troubleshooting performance issues</span></span>](#troubleshooting)
- [<span data-ttu-id="cabc9-120">À l’aide de compteurs de performances de SignalR</span><span class="sxs-lookup"><span data-stu-id="cabc9-120">Using SignalR performance counters</span></span>](#perfcounters)
- [<span data-ttu-id="cabc9-121">À l’aide d’autres compteurs de performances</span><span class="sxs-lookup"><span data-stu-id="cabc9-121">Using other performance counters</span></span>](#othercounters)
- [<span data-ttu-id="cabc9-122">Autres ressources</span><span class="sxs-lookup"><span data-stu-id="cabc9-122">Other resources</span></span>](#otherresources)

<a id="design"></a>

## <a name="design-considerations"></a><span data-ttu-id="cabc9-123">Considérations de conception</span><span class="sxs-lookup"><span data-stu-id="cabc9-123">Design considerations</span></span>

<span data-ttu-id="cabc9-124">Cette section décrit les modèles qui peuvent être implémentées lors de la conception d’une application de SignalR, pour vous assurer que les performances ne sont pas en cours freinées par générer un trafic réseau inutile.</span><span class="sxs-lookup"><span data-stu-id="cabc9-124">This section describes patterns that can be implemented during the design of a SignalR application, to ensure that performance is not being hindered by generating unnecessary network traffic.</span></span>

### <a name="throttling-message-frequency"></a><span data-ttu-id="cabc9-125">La limitation de fréquence de message</span><span class="sxs-lookup"><span data-stu-id="cabc9-125">Throttling message frequency</span></span>

<span data-ttu-id="cabc9-126">Même dans une application qui envoie des messages à une fréquence élevée (par exemple, une application de jeu en temps réel), la plupart des applications n’avez pas besoin envoyer plusieurs messages par seconde.</span><span class="sxs-lookup"><span data-stu-id="cabc9-126">Even in an application that sends out messages at a high frequency (such as a realtime gaming application), most applications don't need to send more than a few messages a second.</span></span> <span data-ttu-id="cabc9-127">Pour réduire la quantité de trafic qui génère de chaque client, une boucle de messages peut être implémentée que les files d’attente et envoie les messages ne dépasse pas fréquemment un taux fixe (autrement dit, jusqu'à un certain nombre de messages envoyés par seconde, s’il existe des messages pendant cette période dans onstruction à envoyer).</span><span class="sxs-lookup"><span data-stu-id="cabc9-127">To reduce the amount of traffic that each client generates, a message loop can be implemented that queues and sends out messages no more frequently than a fixed rate (that is, up to a certain number of messages will be sent every second, if there are messages in that time interval to be sent).</span></span> <span data-ttu-id="cabc9-128">Pour un exemple d’application qui limite les messages à un certain taux (à partir de client et serveur), consultez [en temps réel haute fréquence avec SignalR](../getting-started/tutorial-high-frequency-realtime-with-signalr.md).</span><span class="sxs-lookup"><span data-stu-id="cabc9-128">For a sample application that throttles messages to a certain rate (from both client and server), see [High-Frequency Realtime with SignalR](../getting-started/tutorial-high-frequency-realtime-with-signalr.md).</span></span>

### <a name="reducing-message-size"></a><span data-ttu-id="cabc9-129">Réduire la taille de message</span><span class="sxs-lookup"><span data-stu-id="cabc9-129">Reducing message size</span></span>

<span data-ttu-id="cabc9-130">Vous pouvez réduire la taille d’un message SignalR en réduisant la taille de vos objets sérialisés.</span><span class="sxs-lookup"><span data-stu-id="cabc9-130">You can reduce the size of a SignalR message by reducing the size of your serialized objects.</span></span> <span data-ttu-id="cabc9-131">Dans le code serveur, si vous envoyez un objet qui contient les propriétés que vous n’avez pas besoin d’être transmis, empêcher ces propriétés en cours de sérialisation à l’aide de la `JsonIgnore` attribut.</span><span class="sxs-lookup"><span data-stu-id="cabc9-131">In server code, if you're sending an object that contains properties that don't need to be transmitted, prevent those properties from being serialized by using the `JsonIgnore` attribute.</span></span> <span data-ttu-id="cabc9-132">Les noms des propriétés sont également stockés dans le message. les noms des propriétés peuvent être abrégées à l’aide de la `JsonProperty` attribut.</span><span class="sxs-lookup"><span data-stu-id="cabc9-132">The names of properties are also stored in the message; the names of properties can be shortened using the `JsonProperty` attribute.</span></span> <span data-ttu-id="cabc9-133">L’exemple de code suivant montre comment exclure une propriété d’être envoyé au client et raccourcir les noms de propriété :</span><span class="sxs-lookup"><span data-stu-id="cabc9-133">The following code sample demonstrates how to exclude a property from being sent to the client, and how to shorten property names:</span></span>

<span data-ttu-id="cabc9-134">**Code de serveur .NET qui explique l’attribut JsonIgnore à exclure des données envoyées au client et l’attribut JsonProperty pour réduire la taille de message**</span><span class="sxs-lookup"><span data-stu-id="cabc9-134">**.NET server code that demonstrates the JsonIgnore attribute to exclude data from being sent to the client, and the JsonProperty attribute to reduce message size**</span></span>

[!code-csharp[Main](signalr-performance/samples/sample1.cs?highlight=5,7,10)]

<span data-ttu-id="cabc9-135">Afin de conserver la lisibilité / la facilité de gestion dans le code client, les noms de propriété abrégée peuvent être des noms remappés à convivial une fois que le message est reçu.</span><span class="sxs-lookup"><span data-stu-id="cabc9-135">In order to retain readability/ maintainability in the client code, the abbreviated property names can be remapped to human-friendly names after the message is received.</span></span> <span data-ttu-id="cabc9-136">L’exemple de code suivant montre une façon possible de remappage des noms abrégées à plus longues, en définissant un contrat de message (mappage) et à l’aide de la `reMap` fonction à appliquer le contrat à la classe de message optimisée :</span><span class="sxs-lookup"><span data-stu-id="cabc9-136">The following code sample demonstrates one possible way of remapping shortened names to longer ones, by defining a message contract (mapping), and using the `reMap` function to apply the contract to the optimized message class:</span></span>

<span data-ttu-id="cabc9-137">**Le code JavaScript côté client qui remappe abrégés des noms de propriété aux noms contrôlable de visu**</span><span class="sxs-lookup"><span data-stu-id="cabc9-137">**Client-side JavaScript code that remaps shortened property names to human-readable names**</span></span>

[!code-javascript[Main](signalr-performance/samples/sample2.js)]

<span data-ttu-id="cabc9-138">Les noms peuvent être abrégées dans les messages à partir du client au serveur, à l’aide de la même méthode.</span><span class="sxs-lookup"><span data-stu-id="cabc9-138">Names can be shortened in messages from the client to the server as well, using the same method.</span></span>

<span data-ttu-id="cabc9-139">En réduisant l’encombrement de mémoire (autrement dit, la quantité de mémoire utilisée pour le message) du message objet peut également améliorer les performances.</span><span class="sxs-lookup"><span data-stu-id="cabc9-139">Reducing the memory footprint (that is, the amount of memory used for the message) of the message object can also improve performance.</span></span> <span data-ttu-id="cabc9-140">Par exemple, si la plage complète d’un `int` n’est pas nécessaire, un `short` ou `byte` peut être utilisé à la place.</span><span class="sxs-lookup"><span data-stu-id="cabc9-140">For example, if the full range of an `int` is not needed, a `short` or `byte` can be used instead.</span></span>

<span data-ttu-id="cabc9-141">Dans la mesure où les messages sont stockés dans le bus de messages dans la mémoire du serveur, ce qui réduit la taille des messages peut également résoudre les problèmes de mémoire de serveur.</span><span class="sxs-lookup"><span data-stu-id="cabc9-141">Since messages are stored in the message bus in server memory, reducing the size of messages can also address server memory issues.</span></span>

<a id="tuning"></a>

### <a name="tuning-your-signalr-server-for-performance"></a><span data-ttu-id="cabc9-142">Réglage des performances de votre serveur SignalR</span><span class="sxs-lookup"><span data-stu-id="cabc9-142">Tuning your SignalR server for performance</span></span>

<span data-ttu-id="cabc9-143">Les paramètres de configuration suivants peuvent être utilisés pour paramétrer votre serveur pour optimiser les performances dans une application de SignalR.</span><span class="sxs-lookup"><span data-stu-id="cabc9-143">The following configuration settings can be used to tune your server for better performance in a SignalR application.</span></span> <span data-ttu-id="cabc9-144">Pour obtenir des informations générales sur la façon d’améliorer les performances dans une application ASP.NET, consultez [amélioration des performances ASP.NET](https://msdn.microsoft.com/library/ff647787.aspx).</span><span class="sxs-lookup"><span data-stu-id="cabc9-144">For general information on how to improve performance in an ASP.NET application, see [Improving ASP.NET Performance](https://msdn.microsoft.com/library/ff647787.aspx).</span></span>

<span data-ttu-id="cabc9-145">**Paramètres de configuration de SignalR**</span><span class="sxs-lookup"><span data-stu-id="cabc9-145">**SignalR configuration settings**</span></span>

- <span data-ttu-id="cabc9-146">**DefaultMessageBufferSize**: par défaut, SignalR conserve 1 000 messages en mémoire par hub par connexion.</span><span class="sxs-lookup"><span data-stu-id="cabc9-146">**DefaultMessageBufferSize**: By default, SignalR retains 1000 messages in memory per hub per connection.</span></span> <span data-ttu-id="cabc9-147">Si les messages volumineux sont utilisées, cela peut créer des problèmes de mémoire qui peuvent être résolus en réduisant cette valeur.</span><span class="sxs-lookup"><span data-stu-id="cabc9-147">If large messages are being used, this may create memory issues which can be alleviated by reducing this value.</span></span> <span data-ttu-id="cabc9-148">Ce paramètre peut être défini dans le `Application_Start` Gestionnaire d’événements dans une application ASP.NET ou dans le `Configuration` méthode d’une classe de démarrage OWIN dans une application auto-hébergée.</span><span class="sxs-lookup"><span data-stu-id="cabc9-148">This setting can be set in the `Application_Start` event handler in an ASP.NET application, or in the `Configuration` method of an OWIN startup class in a self-hosted application.</span></span> <span data-ttu-id="cabc9-149">L’exemple suivant montre comment réduire cette valeur afin de réduire l’encombrement mémoire de votre application afin de réduire la quantité de mémoire du serveur utilisé :</span><span class="sxs-lookup"><span data-stu-id="cabc9-149">The following sample demonstrates how to reduce this value in order to reduce the memory footprint of your application in order to reduce the amount of server memory used:</span></span>

    <span data-ttu-id="cabc9-150">**Code de serveur .NET dans Startup.cs pour diminuer la taille de mémoire tampon de message par défaut**</span><span class="sxs-lookup"><span data-stu-id="cabc9-150">**.NET server code in Startup.cs for decreasing default message buffer size**</span></span>

    [!code-csharp[Main](signalr-performance/samples/sample3.cs)]

<span data-ttu-id="cabc9-151">**Paramètres de configuration IIS**</span><span class="sxs-lookup"><span data-stu-id="cabc9-151">**IIS configuration settings**</span></span>

- <span data-ttu-id="cabc9-152">**Nombre maximal de demandes simultanées par application**: augmentation du nombre de IIS simultanées demandes augmente les ressources serveur disponibles au traitement des requêtes.</span><span class="sxs-lookup"><span data-stu-id="cabc9-152">**Max concurrent requests per application**: Increasing the number of concurrent IIS requests will increase server resources available for serving requests.</span></span> <span data-ttu-id="cabc9-153">La valeur par défaut est 5000 ; Pour augmenter ce paramètre, exécutez les commandes suivantes dans une invite de commandes avec élévation de privilèges :</span><span class="sxs-lookup"><span data-stu-id="cabc9-153">The default value is 5000; to increase this setting, execute the following commands in an elevated command prompt:</span></span>

    [!code-console[Main](signalr-performance/samples/sample4.cmd)]
- <span data-ttu-id="cabc9-154">**ApplicationPool QueueLength**: il s’agit du nombre maximal de demandes que Http.sys files d’attente du pool d’applications.</span><span class="sxs-lookup"><span data-stu-id="cabc9-154">**ApplicationPool QueueLength**: This is the maximum number of requests that Http.sys queues for the application pool.</span></span> <span data-ttu-id="cabc9-155">Lorsque la file d’attente est pleine, les nouvelles demandes de recevoir une réponse de « Service indisponible » 503.</span><span class="sxs-lookup"><span data-stu-id="cabc9-155">When the queue is full, new requests receive a 503 "Service Unavailable" response.</span></span> <span data-ttu-id="cabc9-156">La valeur par défaut est 1000.</span><span class="sxs-lookup"><span data-stu-id="cabc9-156">The default value is 1000.</span></span>

    <span data-ttu-id="cabc9-157">Raccourcir la longueur de file d’attente pour le processus de travail dans le pool d’applications hébergeant votre application sera économiser les ressources de mémoire.</span><span class="sxs-lookup"><span data-stu-id="cabc9-157">Shortening the queue length for the worker process in the application pool hosting your application will conserve memory resources.</span></span> <span data-ttu-id="cabc9-158">Pour plus d’informations, consultez [gestion, paramétrage et configuration des Pools d’applications](https://technet.microsoft.com/library/cc745955.aspx).</span><span class="sxs-lookup"><span data-stu-id="cabc9-158">For more information, see [Managing, Tuning, and Configuring Application Pools](https://technet.microsoft.com/library/cc745955.aspx).</span></span>

<span data-ttu-id="cabc9-159">**Paramètres de configuration ASP.NET**</span><span class="sxs-lookup"><span data-stu-id="cabc9-159">**ASP.NET configuration settings**</span></span>

<span data-ttu-id="cabc9-160">Cette section inclut des paramètres de configuration qui peuvent être définies dans le `aspnet.config` fichier.</span><span class="sxs-lookup"><span data-stu-id="cabc9-160">This section includes configuration settings that can be set in the `aspnet.config` file.</span></span> <span data-ttu-id="cabc9-161">Ce fichier se trouve dans un des deux emplacements, en fonction de la plateforme :</span><span class="sxs-lookup"><span data-stu-id="cabc9-161">This file is found in one of two locations, depending on platform:</span></span>

- `%windir%\Microsoft.NET\Framework\v4.0.30319\aspnet.config`
- `%windir%\Microsoft.NET\Framework64\v4.0.30319\aspnet.config`

<span data-ttu-id="cabc9-162">Paramètres de ASP.NET qui peuvent améliorer les performances de SignalR sont les suivants :</span><span class="sxs-lookup"><span data-stu-id="cabc9-162">ASP.NET settings that may improve SignalR performance include the following:</span></span>

- <span data-ttu-id="cabc9-163">**Nombre maximal de requêtes simultané par UC**: augmentation de ce paramètre peut atténuer les goulets d’étranglement.</span><span class="sxs-lookup"><span data-stu-id="cabc9-163">**Maximum concurrent requests per CPU**: Increasing this setting may alleviate performance bottlenecks.</span></span> <span data-ttu-id="cabc9-164">Pour augmenter ce paramètre, ajoutez le paramètre de configuration suivant à la `aspnet.config` fichier :</span><span class="sxs-lookup"><span data-stu-id="cabc9-164">To increase this setting, add the following configuration setting to the `aspnet.config` file:</span></span>

    [!code-xml[Main](signalr-performance/samples/sample5.xml?highlight=4)]
- <span data-ttu-id="cabc9-165">**Limite de la file d’attente des requêtes**: lorsque le nombre total de connexions dépasse le `maxConcurrentRequestsPerCPU` définissant, ASP.NET commence à l’aide d’une file d’attente des demandes de limitation.</span><span class="sxs-lookup"><span data-stu-id="cabc9-165">**Request Queue Limit**: When the total number of connections exceeds the `maxConcurrentRequestsPerCPU` setting, ASP.NET will start throttling requests using a queue.</span></span> <span data-ttu-id="cabc9-166">Pour augmenter la taille de la file d’attente, vous pouvez augmenter la `requestQueueLimit` paramètre.</span><span class="sxs-lookup"><span data-stu-id="cabc9-166">To increase the size of the queue, you can increase the `requestQueueLimit` setting.</span></span> <span data-ttu-id="cabc9-167">Pour ce faire, ajoutez le paramètre de configuration suivant à la `processModel` nœud `config/machine.config` (plutôt que `aspnet.config`) :</span><span class="sxs-lookup"><span data-stu-id="cabc9-167">To do this, add the following configuration setting to the `processModel` node in `config/machine.config` (rather than `aspnet.config`):</span></span>

    [!code-xml[Main](signalr-performance/samples/sample6.xml)]

<a id="troubleshooting"></a>

## <a name="troubleshooting-performance-issues"></a><span data-ttu-id="cabc9-168">Résolution des problèmes de performances</span><span class="sxs-lookup"><span data-stu-id="cabc9-168">Troubleshooting performance issues</span></span>

<span data-ttu-id="cabc9-169">Cette section décrit les façons de rechercher les goulots d’étranglement dans votre application.</span><span class="sxs-lookup"><span data-stu-id="cabc9-169">This section describes ways to find performance bottlenecks in your application.</span></span>

### <a name="verifying-that-websocket-is-being-used"></a><span data-ttu-id="cabc9-170">Vérification que WebSocket est utilisé</span><span class="sxs-lookup"><span data-stu-id="cabc9-170">Verifying that WebSocket is being used</span></span>

<span data-ttu-id="cabc9-171">Bien que SignalR permettre utiliser une variété de transports pour la communication entre client et serveur, WebSocket offre un avantage de performances significatifs et doit être utilisé si le client et le serveur la prend en charge.</span><span class="sxs-lookup"><span data-stu-id="cabc9-171">While SignalR can use a variety of transports for communication between client and server, WebSocket offers a significant performance advantage, and should be used if the client and server support it.</span></span> <span data-ttu-id="cabc9-172">Pour déterminer si votre client et le serveur de répondre à la configuration requise pour le WebSocket, consultez [Transports et les solutions de secours](../getting-started/introduction-to-signalr.md#transports).</span><span class="sxs-lookup"><span data-stu-id="cabc9-172">To determine if your client and server meet the requirements for WebSocket, see [Transports and Fallbacks](../getting-started/introduction-to-signalr.md#transports).</span></span> <span data-ttu-id="cabc9-173">Pour déterminer que le transport est utilisé dans votre application, vous pouvez utiliser les outils de développement du navigateur et examinez les journaux pour vérifier que le transport est utilisé pour la connexion.</span><span class="sxs-lookup"><span data-stu-id="cabc9-173">To determine what transport is being used in your application, you can use the browser developer tools, and examine the logs to see what transport is being used for the connection.</span></span> <span data-ttu-id="cabc9-174">Pour plus d’informations sur l’utilisation des outils de développement de navigateur dans Internet Explorer et Chrome, consultez [Transports et les solutions de secours](../getting-started/introduction-to-signalr.md#transports).</span><span class="sxs-lookup"><span data-stu-id="cabc9-174">For information on using the browser development tools in Internet Explorer and Chrome, see [Transports and Fallbacks](../getting-started/introduction-to-signalr.md#transports).</span></span>

<a id="perfcounters"></a>

## <a name="using-signalr-performance-counters"></a><span data-ttu-id="cabc9-175">À l’aide de compteurs de performances de SignalR</span><span class="sxs-lookup"><span data-stu-id="cabc9-175">Using SignalR performance counters</span></span>

<span data-ttu-id="cabc9-176">Cette section décrit comment activer et utiliser des compteurs de performances de SignalR, trouvé dans le `Microsoft.AspNet.SignalR.Utils` package.</span><span class="sxs-lookup"><span data-stu-id="cabc9-176">This section describes how to enable and use SignalR performance counters, found in the `Microsoft.AspNet.SignalR.Utils` package.</span></span>

### <a name="installing-signalrexe"></a><span data-ttu-id="cabc9-177">L’installation signalr.exe</span><span class="sxs-lookup"><span data-stu-id="cabc9-177">Installing signalr.exe</span></span>

<span data-ttu-id="cabc9-178">Compteurs de performances peuvent être ajoutés au serveur à l’aide d’un utilitaire appelé SignalR.exe.</span><span class="sxs-lookup"><span data-stu-id="cabc9-178">Performance counters can be added to the server using a utility called SignalR.exe.</span></span> <span data-ttu-id="cabc9-179">Pour installer cet utilitaire, procédez comme suit :</span><span class="sxs-lookup"><span data-stu-id="cabc9-179">To install this utility, follow these steps:</span></span>

1. <span data-ttu-id="cabc9-180">Dans Visual Studio, sélectionnez **outils** > **Gestionnaire de Package NuGet** > **gérer les Packages NuGet pour la Solution**</span><span class="sxs-lookup"><span data-stu-id="cabc9-180">In Visual Studio, select **Tools** > **NuGet Package Manager** > **Manage NuGet Packages for Solution**</span></span>
2. <span data-ttu-id="cabc9-181">Recherchez **signalr.utils**et sélectionnez Installer.</span><span class="sxs-lookup"><span data-stu-id="cabc9-181">Search for **signalr.utils**, and select Install.</span></span>

    ![](signalr-performance/_static/image1.png)
3. <span data-ttu-id="cabc9-182">Acceptez le contrat de licence pour installer le package.</span><span class="sxs-lookup"><span data-stu-id="cabc9-182">Accept the license agreement to install the package.</span></span>
4. <span data-ttu-id="cabc9-183">SignalR.exe sera installé à `<project folder>/packages/Microsoft.AspNet.SignalR.Utils.<version>/tools`.</span><span class="sxs-lookup"><span data-stu-id="cabc9-183">SignalR.exe will be installed to `<project folder>/packages/Microsoft.AspNet.SignalR.Utils.<version>/tools`.</span></span>

### <a name="installing-performance-counters-with-signalrexe"></a><span data-ttu-id="cabc9-184">Installation des compteurs de performances avec SignalR.exe</span><span class="sxs-lookup"><span data-stu-id="cabc9-184">Installing performance counters with SignalR.exe</span></span>

<span data-ttu-id="cabc9-185">Pour installer les compteurs de performance SignalR, exécutez SignalR.exe dans une invite de commandes avec élévation de privilèges avec le paramètre suivant :</span><span class="sxs-lookup"><span data-stu-id="cabc9-185">To install SignalR performance counters, run SignalR.exe in an elevated command prompt with the following parameter:</span></span>

[!code-console[Main](signalr-performance/samples/sample7.cmd)]

<span data-ttu-id="cabc9-186">Pour supprimer les compteurs de performance SignalR, exécutez SignalR.exe dans une invite de commandes avec élévation de privilèges avec le paramètre suivant :</span><span class="sxs-lookup"><span data-stu-id="cabc9-186">To remove SignalR performance counters, run SignalR.exe in an elevated command prompt with the following parameter:</span></span>

[!code-console[Main](signalr-performance/samples/sample8.cmd)]

### <a name="signalr-performance-counters"></a><span data-ttu-id="cabc9-187">Compteurs de performances de SignalR</span><span class="sxs-lookup"><span data-stu-id="cabc9-187">SignalR Performance counters</span></span>

<span data-ttu-id="cabc9-188">Le package d’utilitaires installe des compteurs de performances suivants.</span><span class="sxs-lookup"><span data-stu-id="cabc9-188">The utilities package installs the following performance counters.</span></span> <span data-ttu-id="cabc9-189">Les compteurs « Total » mesurent le nombre d’événements dans la mesure où le dernier pool d’applications ou le serveur de redémarrer.</span><span class="sxs-lookup"><span data-stu-id="cabc9-189">The "Total" counters measure the number of events since the last application pool or server restart.</span></span>

<span data-ttu-id="cabc9-190">**Métriques de connexion**</span><span class="sxs-lookup"><span data-stu-id="cabc9-190">**Connection metrics**</span></span>

<span data-ttu-id="cabc9-191">Les métriques suivantes mesurent les événements de durée de vie de connexion qui se produisent.</span><span class="sxs-lookup"><span data-stu-id="cabc9-191">The following metrics measure the connection lifetime events that occur.</span></span> <span data-ttu-id="cabc9-192">Pour plus d’informations, consultez [compréhension et gestion des événements de durée de vie de connexion](../guide-to-the-api/handling-connection-lifetime-events.md).</span><span class="sxs-lookup"><span data-stu-id="cabc9-192">For more information, see [Understanding and Handling Connection Lifetime Events](../guide-to-the-api/handling-connection-lifetime-events.md).</span></span>

- <span data-ttu-id="cabc9-193">**Connexions connectées**</span><span class="sxs-lookup"><span data-stu-id="cabc9-193">**Connections Connected**</span></span>
- <span data-ttu-id="cabc9-194">**Connexions reconnectées**</span><span class="sxs-lookup"><span data-stu-id="cabc9-194">**Connections Reconnected**</span></span>
- <span data-ttu-id="cabc9-195">**Connexions déconnectées**</span><span class="sxs-lookup"><span data-stu-id="cabc9-195">**Connections Disconnected**</span></span>
- <span data-ttu-id="cabc9-196">**Connexions en cours**</span><span class="sxs-lookup"><span data-stu-id="cabc9-196">**Connections Current**</span></span>

<span data-ttu-id="cabc9-197">**Métriques de message**</span><span class="sxs-lookup"><span data-stu-id="cabc9-197">**Message metrics**</span></span>

<span data-ttu-id="cabc9-198">Les métriques suivantes mesurent le trafic des messages généré par SignalR.</span><span class="sxs-lookup"><span data-stu-id="cabc9-198">The following metrics measure the message traffic generated by SignalR.</span></span>

- <span data-ttu-id="cabc9-199">**Nombre Total de Messages de connexion reçus**</span><span class="sxs-lookup"><span data-stu-id="cabc9-199">**Connection Messages Received Total**</span></span>
- <span data-ttu-id="cabc9-200">**Nombre Total de Messages de connexion envoyés**</span><span class="sxs-lookup"><span data-stu-id="cabc9-200">**Connection Messages Sent Total**</span></span>
- <span data-ttu-id="cabc9-201">**Connexion de Messages reçus/s**</span><span class="sxs-lookup"><span data-stu-id="cabc9-201">**Connection Messages Received/Sec**</span></span>
- <span data-ttu-id="cabc9-202">**Connexion de Messages envoyés/s**</span><span class="sxs-lookup"><span data-stu-id="cabc9-202">**Connection Messages Sent/Sec**</span></span>

<span data-ttu-id="cabc9-203">**Métriques de bus de messages**</span><span class="sxs-lookup"><span data-stu-id="cabc9-203">**Message bus metrics**</span></span>

<span data-ttu-id="cabc9-204">Les métriques suivantes mesurent le trafic via le bus de messages SignalR interne, la file d’attente dans laquelle tous les messages de SignalR entrants et sortants sont placés.</span><span class="sxs-lookup"><span data-stu-id="cabc9-204">The following metrics measure traffic through the internal SignalR message bus, the queue in which all incoming and outgoing SignalR messages are placed.</span></span> <span data-ttu-id="cabc9-205">Un message est **publié** quand il est envoyé ou de diffusion.</span><span class="sxs-lookup"><span data-stu-id="cabc9-205">A message is **Published** when it is sent or broadcast.</span></span> <span data-ttu-id="cabc9-206">Un **abonné** dans ce contexte est un abonnement sur le bus de messages ; il doit être égal au nombre de clients plus le serveur lui-même.</span><span class="sxs-lookup"><span data-stu-id="cabc9-206">A **Subscriber** in this context is a subscription on the message bus; this should equal the number of clients plus the server itself.</span></span> <span data-ttu-id="cabc9-207">Un **travail alloué** est un composant qui envoie des données à des connexions actives ; un **travail occupé** est celui qui envoie un message activement.</span><span class="sxs-lookup"><span data-stu-id="cabc9-207">An **Allocated Worker** is a component that sends data to active connections; a **Busy Worker** is one that is actively sending a message.</span></span>

- <span data-ttu-id="cabc9-208">**Total reçu des Messages de Bus de messages**</span><span class="sxs-lookup"><span data-stu-id="cabc9-208">**Message Bus Messages Received Total**</span></span>
- <span data-ttu-id="cabc9-209">**Bus de messages Messages reçus/s**</span><span class="sxs-lookup"><span data-stu-id="cabc9-209">**Message Bus Messages Received/Sec**</span></span>
- <span data-ttu-id="cabc9-210">**Bus de messages Messages publiés Total**</span><span class="sxs-lookup"><span data-stu-id="cabc9-210">**Message Bus Messages Published Total**</span></span>
- <span data-ttu-id="cabc9-211">**Bus de messages Messages publiés par seconde**</span><span class="sxs-lookup"><span data-stu-id="cabc9-211">**Message Bus Messages Published/Sec**</span></span>
- <span data-ttu-id="cabc9-212">**Message Bus abonnés actuel**</span><span class="sxs-lookup"><span data-stu-id="cabc9-212">**Message Bus Subscribers Current**</span></span>
- <span data-ttu-id="cabc9-213">**Total des abonnés Bus de messages**</span><span class="sxs-lookup"><span data-stu-id="cabc9-213">**Message Bus Subscribers Total**</span></span>
- <span data-ttu-id="cabc9-214">**Les abonnés de Bus de messages/s**</span><span class="sxs-lookup"><span data-stu-id="cabc9-214">**Message Bus Subscribers/Sec**</span></span>
- <span data-ttu-id="cabc9-215">**Bus de messages allouée travailleurs**</span><span class="sxs-lookup"><span data-stu-id="cabc9-215">**Message Bus Allocated Workers**</span></span>
- <span data-ttu-id="cabc9-216">**Threads de travail occupés de Bus de messages**</span><span class="sxs-lookup"><span data-stu-id="cabc9-216">**Message Bus Busy Workers**</span></span>
- <span data-ttu-id="cabc9-217">**Actuel de rubriques de Bus de messages**</span><span class="sxs-lookup"><span data-stu-id="cabc9-217">**Message Bus Topics Current**</span></span>

<span data-ttu-id="cabc9-218">**Mesures d’erreur**</span><span class="sxs-lookup"><span data-stu-id="cabc9-218">**Error metrics**</span></span>

<span data-ttu-id="cabc9-219">Les métriques suivantes mesurent les erreurs générées par le trafic des messages SignalR.</span><span class="sxs-lookup"><span data-stu-id="cabc9-219">The following metrics measure errors generated by SignalR message traffic.</span></span> <span data-ttu-id="cabc9-220">**Résolution du Hub** erreurs se produisent quand un hub ou une méthode de concentrateur ne peut pas être résolu.</span><span class="sxs-lookup"><span data-stu-id="cabc9-220">**Hub Resolution** errors occur when a hub or hub method cannot be resolved.</span></span> <span data-ttu-id="cabc9-221">**Appel de concentrateur** erreurs sont des exceptions levées lors de l’appel d’une méthode de concentrateur.</span><span class="sxs-lookup"><span data-stu-id="cabc9-221">**Hub Invocation** errors are exceptions thrown while invoking a hub method.</span></span> <span data-ttu-id="cabc9-222">**Transport** erreurs sont des erreurs de connexion levées pendant une requête ou réponse HTTP.</span><span class="sxs-lookup"><span data-stu-id="cabc9-222">**Transport** errors are connection errors thrown during an HTTP request or response.</span></span>

- <span data-ttu-id="cabc9-223">**Erreurs : Total de toutes les**</span><span class="sxs-lookup"><span data-stu-id="cabc9-223">**Errors: All Total**</span></span>
- <span data-ttu-id="cabc9-224">**Erreurs : All/s**</span><span class="sxs-lookup"><span data-stu-id="cabc9-224">**Errors: All/Sec**</span></span>
- <span data-ttu-id="cabc9-225">**Erreurs : Total de résolution Hub**</span><span class="sxs-lookup"><span data-stu-id="cabc9-225">**Errors: Hub Resolution Total**</span></span>
- <span data-ttu-id="cabc9-226">**Erreurs : Résolution de concentrateur par seconde**</span><span class="sxs-lookup"><span data-stu-id="cabc9-226">**Errors: Hub Resolution/Sec**</span></span>
- <span data-ttu-id="cabc9-227">**Erreurs : Total d’appel de concentrateur**</span><span class="sxs-lookup"><span data-stu-id="cabc9-227">**Errors: Hub Invocation Total**</span></span>
- <span data-ttu-id="cabc9-228">**Erreurs : Appel de concentrateur par seconde**</span><span class="sxs-lookup"><span data-stu-id="cabc9-228">**Errors: Hub Invocation/Sec**</span></span>
- <span data-ttu-id="cabc9-229">**Erreurs : Total de Transport**</span><span class="sxs-lookup"><span data-stu-id="cabc9-229">**Errors: Transport Total**</span></span>
- <span data-ttu-id="cabc9-230">**Erreurs : Transport/s**</span><span class="sxs-lookup"><span data-stu-id="cabc9-230">**Errors: Transport/Sec**</span></span>

<a id="scaleout_metrics"></a>

<span data-ttu-id="cabc9-231">**Métriques de montée en puissance parallèle**</span><span class="sxs-lookup"><span data-stu-id="cabc9-231">**Scaleout metrics**</span></span>

<span data-ttu-id="cabc9-232">Les métriques suivantes mesurent le trafic et les erreurs générées par le fournisseur de montée en puissance parallèle.</span><span class="sxs-lookup"><span data-stu-id="cabc9-232">The following metrics measure traffic and errors generated by the scaleout provider.</span></span> <span data-ttu-id="cabc9-233">Un **Stream** dans ce contexte est une unité d’échelle utilisée par le fournisseur de montée en puissance parallèle ; il s’agit une table si SQL Server est utilisé, une rubrique si Service Bus est utilisé et un abonnement si Redis est utilisé.</span><span class="sxs-lookup"><span data-stu-id="cabc9-233">A **Stream** in this context is a scale unit used by the scaleout provider; this is a table if SQL Server is used, a Topic if Service Bus is used, and a Subscription if Redis is used.</span></span> <span data-ttu-id="cabc9-234">Chaque flux garantit ordonnée opérations de lecture et écriture ; un seul flux étant un goulot d’étranglement potentiel de mise à l’échelle, le nombre de flux peut être augmenté pour aider à réduire ce goulot d’étranglement.</span><span class="sxs-lookup"><span data-stu-id="cabc9-234">Each stream ensures ordered read and write operations; a single stream is a potential scale bottleneck, so the number of streams can be increased to help reduce that bottleneck.</span></span> <span data-ttu-id="cabc9-235">Si plusieurs flux sont utilisés, SignalR répartit automatiquement les messages (partition) entre ces flux d’une façon qui garantit que les messages envoyés à partir d’une connexion donnée sont dans l’ordre.</span><span class="sxs-lookup"><span data-stu-id="cabc9-235">If multiple streams are used, SignalR will automatically distribute (shard) messages across these streams in a way that ensures messages sent from any given connection are in order.</span></span>

<span data-ttu-id="cabc9-236">Le [MaxQueueLength](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.messaging.scaleoutconfiguration.maxqueuelength(v=vs.118).aspx) paramètre détermine la longueur de la file d’envoi de montée en puissance parallèle gérée par SignalR.</span><span class="sxs-lookup"><span data-stu-id="cabc9-236">The [MaxQueueLength](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.messaging.scaleoutconfiguration.maxqueuelength(v=vs.118).aspx) setting controls the length of the scaleout send queue maintained by SignalR.</span></span> <span data-ttu-id="cabc9-237">Lui attribuant une valeur supérieure à 0 place tous les messages dans une file d’attente d’envoi à envoyer à la fois pour le fond de panier de messagerie configuré.</span><span class="sxs-lookup"><span data-stu-id="cabc9-237">Setting it to a value greater than 0 will place all messages in a send queue to be sent one at a time to the configured messaging backplane.</span></span> <span data-ttu-id="cabc9-238">Si la taille de la file d’attente dépasse la longueur configurée, les appels suivants à envoyer va échouent immédiatement avec un [InvalidOperationException](https://msdn.microsoft.com/library/system.invalidoperationexception(v=vs.118).aspx) jusqu'à ce que le nombre de messages dans la file d’attente est inférieure à celle du paramètre à nouveau.</span><span class="sxs-lookup"><span data-stu-id="cabc9-238">If the size of the queue goes above the configured length, subsequent calls to send will fail immediately with an [InvalidOperationException](https://msdn.microsoft.com/library/system.invalidoperationexception(v=vs.118).aspx) until the number of messages in the queue is less than the setting again.</span></span> <span data-ttu-id="cabc9-239">File d’attente est désactivée par défaut, car les fonds de panier implémentées ont généralement leurs propres queuing ou le contrôle de flux en place.</span><span class="sxs-lookup"><span data-stu-id="cabc9-239">Queueing is disabled by default because the implemented backplanes generally have their own queuing or flow-control in place.</span></span> <span data-ttu-id="cabc9-240">Dans le cas de SQL Server, le regroupement de connexions limite efficacement le nombre d’envois passe à tout moment.</span><span class="sxs-lookup"><span data-stu-id="cabc9-240">In the case of SQL Server, connection pooling effectively limits the number of sends going on at any one time.</span></span>

<span data-ttu-id="cabc9-241">Par défaut, qu’un seul flux est utilisé pour SQL Server et Redis, cinq flux sont utilisés pour Service Bus et file d’attente est désactivé, mais ces paramètres peuvent être modifiés par la configuration sur SQL Server et Service Bus :</span><span class="sxs-lookup"><span data-stu-id="cabc9-241">By default, only one stream is used for SQL Server and Redis, five streams are used for Service Bus, and queueing is disabled, but these settings can be changed through configuration on SQL Server and Service Bus:</span></span>

<span data-ttu-id="cabc9-242">**Code de serveur .NET pour la configuration de longueur de nombre et de la file d’attente de la table de fond de panier de SQL Server**</span><span class="sxs-lookup"><span data-stu-id="cabc9-242">**.NET Server Code for configuring table count and queue length for SQL Server backplane**</span></span>

[!code-csharp[Main](signalr-performance/samples/sample9.cs)]

<span data-ttu-id="cabc9-243">**Code de serveur .NET pour la configuration de longueur de nombre et de la file d’attente de rubrique pour Service Bus de fond de panier**</span><span class="sxs-lookup"><span data-stu-id="cabc9-243">**.NET Server Code for configuring topic count and queue length for Service Bus backplane**</span></span>

[!code-csharp[Main](signalr-performance/samples/sample10.cs)]

<span data-ttu-id="cabc9-244">Un **mise en mémoire tampon** stream est celui qui a entré un état d’erreur ; lorsque le flux est dans l’état par défaut, tous les messages envoyés au fond de panier échouera immédiatement jusqu'à ce que le flux est défaillante n’est plus.</span><span class="sxs-lookup"><span data-stu-id="cabc9-244">A **Buffering** stream is one that has entered a faulted state; when the stream is in the faulted state, all messages sent to the backplane will fail immediately until the stream is no longer faulting.</span></span> <span data-ttu-id="cabc9-245">Le **longueur de file d’attente d’envoi** est le nombre de messages qui ont été validées mais pas encore été envoyées.</span><span class="sxs-lookup"><span data-stu-id="cabc9-245">The **Send Queue Length** is the number of messages that have been posted but not yet sent.</span></span>

- <span data-ttu-id="cabc9-246">**Bus de messages de montée en puissance parallèle des Messages reçus/s**</span><span class="sxs-lookup"><span data-stu-id="cabc9-246">**Scaleout Message Bus Messages Received/Sec**</span></span>
- <span data-ttu-id="cabc9-247">**Total des flux de montée en puissance parallèle**</span><span class="sxs-lookup"><span data-stu-id="cabc9-247">**Scaleout Streams Total**</span></span>
- <span data-ttu-id="cabc9-248">**Montée en puissance parallèle diffuse Open**</span><span class="sxs-lookup"><span data-stu-id="cabc9-248">**Scaleout Streams Open**</span></span>
- <span data-ttu-id="cabc9-249">**Mise en mémoire tampon de flux de montée en puissance parallèle**</span><span class="sxs-lookup"><span data-stu-id="cabc9-249">**Scaleout Streams Buffering**</span></span>
- <span data-ttu-id="cabc9-250">**Total des erreurs de montée en puissance parallèle**</span><span class="sxs-lookup"><span data-stu-id="cabc9-250">**Scaleout Errors Total**</span></span>
- <span data-ttu-id="cabc9-251">**Erreurs de montée en puissance parallèle par seconde**</span><span class="sxs-lookup"><span data-stu-id="cabc9-251">**Scaleout Errors/Sec**</span></span>
- <span data-ttu-id="cabc9-252">**Longueur de file d’attente d’envoi de montée en puissance parallèle**</span><span class="sxs-lookup"><span data-stu-id="cabc9-252">**Scaleout Send Queue Length**</span></span>

<span data-ttu-id="cabc9-253">Pour plus d’informations sur la mesurant des ces compteurs, consultez [SignalR Scaleout with Azure Service Bus](scaleout-with-windows-azure-service-bus.md).</span><span class="sxs-lookup"><span data-stu-id="cabc9-253">For more information on what these counters are measuring, see [SignalR Scaleout with Azure Service Bus](scaleout-with-windows-azure-service-bus.md).</span></span>

<a id="othercounters"></a>

## <a name="using-other-performance-counters"></a><span data-ttu-id="cabc9-254">À l’aide d’autres compteurs de performances</span><span class="sxs-lookup"><span data-stu-id="cabc9-254">Using other performance counters</span></span>

<span data-ttu-id="cabc9-255">Les compteurs de performances suivants peuvent également être utiles pour surveiller les performances de votre application.</span><span class="sxs-lookup"><span data-stu-id="cabc9-255">The following performance counters may also be useful in monitoring your application's performance.</span></span>

<span data-ttu-id="cabc9-256">**Mémoire**</span><span class="sxs-lookup"><span data-stu-id="cabc9-256">**Memory**</span></span>

- <span data-ttu-id="cabc9-257">Mémoire CLR .NET\\Nb. d’octets dans tous les segments de mémoire (pour w3wp)</span><span class="sxs-lookup"><span data-stu-id="cabc9-257">.NET CLR Memory\\# bytes in all Heaps (for w3wp)</span></span>

<span data-ttu-id="cabc9-258">**ASP.NET**</span><span class="sxs-lookup"><span data-stu-id="cabc9-258">**ASP.NET**</span></span>

- <span data-ttu-id="cabc9-259">ASP. net\requêtes en cours</span><span class="sxs-lookup"><span data-stu-id="cabc9-259">ASP.NET\Requests Current</span></span>
- <span data-ttu-id="cabc9-260">ASP.NET\Queued</span><span class="sxs-lookup"><span data-stu-id="cabc9-260">ASP.NET\Queued</span></span>
- <span data-ttu-id="cabc9-261">ASP.NET\Rejected</span><span class="sxs-lookup"><span data-stu-id="cabc9-261">ASP.NET\Rejected</span></span>

<span data-ttu-id="cabc9-262">**Processeur**</span><span class="sxs-lookup"><span data-stu-id="cabc9-262">**CPU**</span></span>

- <span data-ttu-id="cabc9-263">Temps de processeur Information\Processor</span><span class="sxs-lookup"><span data-stu-id="cabc9-263">Processor Information\Processor Time</span></span>

<span data-ttu-id="cabc9-264">**TCP/IP**</span><span class="sxs-lookup"><span data-stu-id="cabc9-264">**TCP/IP**</span></span>

- <span data-ttu-id="cabc9-265">TCPv6/connexions établies</span><span class="sxs-lookup"><span data-stu-id="cabc9-265">TCPv6/Connections Established</span></span>
- <span data-ttu-id="cabc9-266">TCPv4/connexions établies</span><span class="sxs-lookup"><span data-stu-id="cabc9-266">TCPv4/Connections Established</span></span>

<span data-ttu-id="cabc9-267">**Service Web**</span><span class="sxs-lookup"><span data-stu-id="cabc9-267">**Web Service**</span></span>

- <span data-ttu-id="cabc9-268">Connexions Web service web\connexions en cours</span><span class="sxs-lookup"><span data-stu-id="cabc9-268">Web Service\Current Connections</span></span>
- <span data-ttu-id="cabc9-269">Connexions de Service\Maximum Web</span><span class="sxs-lookup"><span data-stu-id="cabc9-269">Web Service\Maximum Connections</span></span>

<span data-ttu-id="cabc9-270">**Thread**</span><span class="sxs-lookup"><span data-stu-id="cabc9-270">**Threading**</span></span>

- <span data-ttu-id="cabc9-271">.NET CLR de verrous et Threads\\# de Threads actuels logiques</span><span class="sxs-lookup"><span data-stu-id="cabc9-271">.NET CLR Locks And Threads\\# of current logical Threads</span></span>
- <span data-ttu-id="cabc9-272">.NET CLR de verrous et Threads\\# de Threads actuels physiques</span><span class="sxs-lookup"><span data-stu-id="cabc9-272">.NET CLR Locks And Threads\\# of current physical Threads</span></span>

<a id="otherresources"></a>

## <a name="other-resources"></a><span data-ttu-id="cabc9-273">Autres ressources</span><span class="sxs-lookup"><span data-stu-id="cabc9-273">Other Resources</span></span>

<span data-ttu-id="cabc9-274">Pour plus d’informations sur la surveillance et réglage des performances de ASP.NET, consultez les rubriques suivantes :</span><span class="sxs-lookup"><span data-stu-id="cabc9-274">For more information on ASP.NET performance monitoring and tuning, see the following topics:</span></span>

- <span data-ttu-id="cabc9-275">[Vue d’ensemble des performances ASP.NET](https://msdn.microsoft.com/library/cc668225(v=vs.100).aspx)</span><span class="sxs-lookup"><span data-stu-id="cabc9-275">[ASP.NET Performance Overview](https://msdn.microsoft.com/library/cc668225(v=vs.100).aspx)</span></span>
- [<span data-ttu-id="cabc9-276">Utilisation de Thread ASP.NET sur IIS 7.5, IIS 7.0 et IIS 6.0</span><span class="sxs-lookup"><span data-stu-id="cabc9-276">ASP.NET Thread Usage on IIS 7.5, IIS 7.0, and IIS 6.0</span></span>](https://blogs.msdn.com/b/tmarq/archive/2007/07/21/asp-net-thread-usage-on-iis-7-0-and-6-0.aspx)
- [<span data-ttu-id="cabc9-277">&lt;applicationPool&gt; , élément (paramètres Web)</span><span class="sxs-lookup"><span data-stu-id="cabc9-277">&lt;applicationPool&gt; Element (Web Settings)</span></span>](https://msdn.microsoft.com/library/dd560842.aspx)
