---
uid: signalr/overview/performance/signalr-performance
title: SignalR performances | Documents Microsoft
author: pfletcher
description: Performances de SignalR
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: 3751f5e7-59db-4be0-a290-50abc24e5c84
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/performance/signalr-performance
msc.type: authoredcontent
ms.openlocfilehash: dec2602e47fbcb838643a506a7e3feebda9d9c81
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/10/2017
---
<a name="signalr-performance"></a><span data-ttu-id="32330-103">Performances de SignalR</span><span class="sxs-lookup"><span data-stu-id="32330-103">SignalR Performance</span></span>
====================
<span data-ttu-id="32330-104">par [Patrick Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="32330-104">by [Patrick Fletcher](https://github.com/pfletcher)</span></span>

> <span data-ttu-id="32330-105">Cette rubrique décrit comment concevoir pour, mesurer et améliorer les performances dans une application SignalR.</span><span class="sxs-lookup"><span data-stu-id="32330-105">This topic describes how to design for, measure, and improve performance in a SignalR application.</span></span>
> 
> ## <a name="software-versions-used-in-this-topic"></a><span data-ttu-id="32330-106">Versions du logiciel utilisées dans cette rubrique</span><span class="sxs-lookup"><span data-stu-id="32330-106">Software versions used in this topic</span></span>
> 
> 
> - [<span data-ttu-id="32330-107">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="32330-107">Visual Studio 2013</span></span>](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - <span data-ttu-id="32330-108">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="32330-108">.NET 4.5</span></span>
> - <span data-ttu-id="32330-109">SignalR version 2</span><span class="sxs-lookup"><span data-stu-id="32330-109">SignalR version 2</span></span>
>   
> 
> 
> ## <a name="previous-versions-of-this-topic"></a><span data-ttu-id="32330-110">Versions précédentes de cette rubrique</span><span class="sxs-lookup"><span data-stu-id="32330-110">Previous versions of this topic</span></span>
> 
> <span data-ttu-id="32330-111">Pour plus d’informations sur les versions antérieures de SignalR, consultez [SignalR des Versions antérieures](../older-versions/index.md).</span><span class="sxs-lookup"><span data-stu-id="32330-111">For information about earlier versions of SignalR, see [SignalR Older Versions](../older-versions/index.md).</span></span>
> 
> ## <a name="questions-and-comments"></a><span data-ttu-id="32330-112">Questions et des commentaires</span><span class="sxs-lookup"><span data-stu-id="32330-112">Questions and comments</span></span>
> 
> <span data-ttu-id="32330-113">Veuillez laisser des commentaires sur la façon dont vous avez aimé ce didacticiel et nous pouvons améliorer dans les commentaires en bas de la page.</span><span class="sxs-lookup"><span data-stu-id="32330-113">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="32330-114">Si vous avez des questions qui ne sont pas directement liées à ce didacticiel, vous pouvez les valider pour le [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) ou [StackOverflow.com](http://stackoverflow.com/).</span><span class="sxs-lookup"><span data-stu-id="32330-114">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>


<span data-ttu-id="32330-115">Pour une présentation récente sur SignalR performances et mise à l’échelle, consultez [mise à l’échelle Web en temps réel avec ASP.NET SignalR](https://channel9.msdn.com/Events/Build/2013/3-502).</span><span class="sxs-lookup"><span data-stu-id="32330-115">For a recent presentation on SignalR performance and scaling, see [Scaling the Real-time Web with ASP.NET SignalR](https://channel9.msdn.com/Events/Build/2013/3-502).</span></span>

<span data-ttu-id="32330-116">Cette rubrique contient les sections suivantes :</span><span class="sxs-lookup"><span data-stu-id="32330-116">This topic contains the following sections:</span></span>

- [<span data-ttu-id="32330-117">Considérations de conception</span><span class="sxs-lookup"><span data-stu-id="32330-117">Design considerations</span></span>](#design)
- [<span data-ttu-id="32330-118">Réglage de votre serveur SignalR pour des performances</span><span class="sxs-lookup"><span data-stu-id="32330-118">Tuning your SignalR server for performance</span></span>](#tuning)
- [<span data-ttu-id="32330-119">Résolution des problèmes de performances</span><span class="sxs-lookup"><span data-stu-id="32330-119">Troubleshooting performance issues</span></span>](#troubleshooting)
- [<span data-ttu-id="32330-120">À l’aide des compteurs de performance SignalR</span><span class="sxs-lookup"><span data-stu-id="32330-120">Using SignalR performance counters</span></span>](#perfcounters)
- [<span data-ttu-id="32330-121">À l’aide d’autres compteurs de performances</span><span class="sxs-lookup"><span data-stu-id="32330-121">Using other performance counters</span></span>](#othercounters)
- [<span data-ttu-id="32330-122">Autres ressources</span><span class="sxs-lookup"><span data-stu-id="32330-122">Other resources</span></span>](#otherresources)

<a id="design"></a>

## <a name="design-considerations"></a><span data-ttu-id="32330-123">Considérations de conception</span><span class="sxs-lookup"><span data-stu-id="32330-123">Design considerations</span></span>

<span data-ttu-id="32330-124">Cette section décrit les modèles qui peuvent être implémentées pendant la conception d’une application SignalR, pour vous assurer que les performances ne soit pas entravé en générant le trafic réseau inutile.</span><span class="sxs-lookup"><span data-stu-id="32330-124">This section describes patterns that can be implemented during the design of a SignalR application, to ensure that performance is not being hindered by generating unnecessary network traffic.</span></span>

### <a name="throttling-message-frequency"></a><span data-ttu-id="32330-125">Limitation de la fréquence des messages</span><span class="sxs-lookup"><span data-stu-id="32330-125">Throttling message frequency</span></span>

<span data-ttu-id="32330-126">Même dans une application qui envoie des messages à une fréquence élevée (par exemple, une application de jeu en temps réel), la plupart des applications n’avez pas besoin d’envoyer plusieurs messages par seconde.</span><span class="sxs-lookup"><span data-stu-id="32330-126">Even in an application that sends out messages at a high frequency (such as a realtime gaming application), most applications don't need to send more than a few messages a second.</span></span> <span data-ttu-id="32330-127">Pour réduire la quantité de trafic qui génère de chaque client, une boucle de message peut être implémentée que les files d’attente et envoie des messages ne dépasse pas fréquemment un taux fixe (autrement dit, un certain nombre de messages est envoyée par seconde, s’il existe des messages pendant cette période dans terval envoi).</span><span class="sxs-lookup"><span data-stu-id="32330-127">To reduce the amount of traffic that each client generates, a message loop can be implemented that queues and sends out messages no more frequently than a fixed rate (that is, up to a certain number of messages will be sent every second, if there are messages in that time interval to be sent).</span></span> <span data-ttu-id="32330-128">Pour un exemple d’application qui limite les messages à un certain taux (à partir de client et serveur), consultez [en temps réel de haute fréquence avec SignalR](../getting-started/tutorial-high-frequency-realtime-with-signalr.md).</span><span class="sxs-lookup"><span data-stu-id="32330-128">For a sample application that throttles messages to a certain rate (from both client and server), see [High-Frequency Realtime with SignalR](../getting-started/tutorial-high-frequency-realtime-with-signalr.md).</span></span>

### <a name="reducing-message-size"></a><span data-ttu-id="32330-129">La réduction de taille de message</span><span class="sxs-lookup"><span data-stu-id="32330-129">Reducing message size</span></span>

<span data-ttu-id="32330-130">Vous pouvez réduire la taille d’un message SignalR en réduisant la taille de vos objets sérialisés.</span><span class="sxs-lookup"><span data-stu-id="32330-130">You can reduce the size of a SignalR message by reducing the size of your serialized objects.</span></span> <span data-ttu-id="32330-131">Dans le code serveur, si vous envoyez un objet qui contient les propriétés que vous n’avez pas besoin d’être transmis, empêcher ces propriétés en cours de sérialisation à l’aide de la `JsonIgnore` attribut.</span><span class="sxs-lookup"><span data-stu-id="32330-131">In server code, if you're sending an object that contains properties that don't need to be transmitted, prevent those properties from being serialized by using the `JsonIgnore` attribute.</span></span> <span data-ttu-id="32330-132">Les noms des propriétés sont également stockées dans le message. les noms de propriétés peuvent être abrégés à l’aide de la `JsonProperty` attribut.</span><span class="sxs-lookup"><span data-stu-id="32330-132">The names of properties are also stored in the message; the names of properties can be shortened using the `JsonProperty` attribute.</span></span> <span data-ttu-id="32330-133">L’exemple de code suivant illustre comment exclure une propriété d’être envoyées au client et raccourcir les noms de propriété :</span><span class="sxs-lookup"><span data-stu-id="32330-133">The following code sample demonstrates how to exclude a property from being sent to the client, and how to shorten property names:</span></span>

<span data-ttu-id="32330-134">**Code de serveur .NET qui montre l’attribut JsonIgnore pour exclure des données envoyées au client et l’attribut JsonProperty pour réduire la taille de message**</span><span class="sxs-lookup"><span data-stu-id="32330-134">**.NET server code that demonstrates the JsonIgnore attribute to exclude data from being sent to the client, and the JsonProperty attribute to reduce message size**</span></span>

[!code-csharp[Main](signalr-performance/samples/sample1.cs?highlight=5,7,10)]

<span data-ttu-id="32330-135">Afin de conserver la lisibilité facilité de maintenance dans le code client, les noms de propriété abrégé peuvent être remappé vers convivial noms après la réception du message.</span><span class="sxs-lookup"><span data-stu-id="32330-135">In order to retain readability/ maintainability in the client code, the abbreviated property names can be remapped to human-friendly names after the message is received.</span></span> <span data-ttu-id="32330-136">L’exemple de code suivant illustre une façon possible de remappage des noms raccourcis à plus longues, en définissant un contrat de message (mappage) et à l’aide de la `reMap` fonction à appliquer le contrat à la classe de message optimisée :</span><span class="sxs-lookup"><span data-stu-id="32330-136">The following code sample demonstrates one possible way of remapping shortened names to longer ones, by defining a message contract (mapping), and using the `reMap` function to apply the contract to the optimized message class:</span></span>

<span data-ttu-id="32330-137">**Code JavaScript côté client qui remappe réduit les noms de propriété aux noms contrôlable de visu**</span><span class="sxs-lookup"><span data-stu-id="32330-137">**Client-side JavaScript code that remaps shortened property names to human-readable names**</span></span>

[!code-javascript[Main](signalr-performance/samples/sample2.js)]

<span data-ttu-id="32330-138">Noms peuvent être abrégés dans les messages à partir du client au serveur, à l’aide de la même méthode.</span><span class="sxs-lookup"><span data-stu-id="32330-138">Names can be shortened in messages from the client to the server as well, using the same method.</span></span>

<span data-ttu-id="32330-139">En diminuant l’encombrement de mémoire (autrement dit, la quantité de mémoire utilisée pour le message) du message d’objet peut également améliorer les performances.</span><span class="sxs-lookup"><span data-stu-id="32330-139">Reducing the memory footprint (that is, the amount of memory used for the message) of the message object can also improve performance.</span></span> <span data-ttu-id="32330-140">Par exemple, si la plage complète d’un `int` n’est pas nécessaire, un `short` ou `byte` peut être utilisé à la place.</span><span class="sxs-lookup"><span data-stu-id="32330-140">For example, if the full range of an `int` is not needed, a `short` or `byte` can be used instead.</span></span>

<span data-ttu-id="32330-141">Étant donné que les messages sont stockés dans le bus de messages dans la mémoire du serveur, ce qui réduit la taille des messages peut également résoudre les problèmes de mémoire de serveur.</span><span class="sxs-lookup"><span data-stu-id="32330-141">Since messages are stored in the message bus in server memory, reducing the size of messages can also address server memory issues.</span></span>

<a id="tuning"></a>

### <a name="tuning-your-signalr-server-for-performance"></a><span data-ttu-id="32330-142">Réglage de votre serveur SignalR pour des performances</span><span class="sxs-lookup"><span data-stu-id="32330-142">Tuning your SignalR server for performance</span></span>

<span data-ttu-id="32330-143">Les paramètres de configuration suivants peuvent être utilisés pour paramétrer votre serveur pour de meilleures performances dans une application SignalR.</span><span class="sxs-lookup"><span data-stu-id="32330-143">The following configuration settings can be used to tune your server for better performance in a SignalR application.</span></span> <span data-ttu-id="32330-144">Pour obtenir des informations générales sur la façon d’améliorer les performances dans une application ASP.NET, consultez [amélioration des performances ASP.NET](https://msdn.microsoft.com/en-us/library/ff647787.aspx).</span><span class="sxs-lookup"><span data-stu-id="32330-144">For general information on how to improve performance in an ASP.NET application, see [Improving ASP.NET Performance](https://msdn.microsoft.com/en-us/library/ff647787.aspx).</span></span>

<span data-ttu-id="32330-145">**Paramètres de configuration SignalR**</span><span class="sxs-lookup"><span data-stu-id="32330-145">**SignalR configuration settings**</span></span>

- <span data-ttu-id="32330-146">**DefaultMessageBufferSize**: par défaut, SignalR conserve 1 000 messages en mémoire par le concentrateur par connexion.</span><span class="sxs-lookup"><span data-stu-id="32330-146">**DefaultMessageBufferSize**: By default, SignalR retains 1000 messages in memory per hub per connection.</span></span> <span data-ttu-id="32330-147">Si les messages volumineux sont utilisés, cela peut créer des problèmes de mémoire qui peuvent être Soulagés par la réduction de cette valeur.</span><span class="sxs-lookup"><span data-stu-id="32330-147">If large messages are being used, this may create memory issues which can be alleviated by reducing this value.</span></span> <span data-ttu-id="32330-148">Ce paramètre peut être défini dans le `Application_Start` Gestionnaire d’événements dans une application ASP.NET, ou dans le `Configuration` méthode d’une classe de démarrage OWIN dans une application auto-hébergée.</span><span class="sxs-lookup"><span data-stu-id="32330-148">This setting can be set in the `Application_Start` event handler in an ASP.NET application, or in the `Configuration` method of an OWIN startup class in a self-hosted application.</span></span> <span data-ttu-id="32330-149">L’exemple suivant montre comment réduire cette valeur afin de réduire l’encombrement mémoire de votre application afin de réduire la quantité de mémoire du serveur :</span><span class="sxs-lookup"><span data-stu-id="32330-149">The following sample demonstrates how to reduce this value in order to reduce the memory footprint of your application in order to reduce the amount of server memory used:</span></span>

    <span data-ttu-id="32330-150">**Code de serveur .NET dans Startup.cs pour diminuer la taille de mémoire tampon de message par défaut**</span><span class="sxs-lookup"><span data-stu-id="32330-150">**.NET server code in Startup.cs for decreasing default message buffer size**</span></span>

    [!code-csharp[Main](signalr-performance/samples/sample3.cs)]

<span data-ttu-id="32330-151">**Paramètres de configuration IIS**</span><span class="sxs-lookup"><span data-stu-id="32330-151">**IIS configuration settings**</span></span>

- <span data-ttu-id="32330-152">**Nombre maximal de demandes simultanées par application**: augmenter le nombre de IIS simultanées demandes augmente les ressources serveur disponibles pour le traitement des demandes.</span><span class="sxs-lookup"><span data-stu-id="32330-152">**Max concurrent requests per application**: Increasing the number of concurrent IIS requests will increase server resources available for serving requests.</span></span> <span data-ttu-id="32330-153">La valeur par défaut est 5000 ; Pour augmenter ce paramètre, exécutez les commandes suivantes dans une invite de commandes avec élévation de privilèges :</span><span class="sxs-lookup"><span data-stu-id="32330-153">The default value is 5000; to increase this setting, execute the following commands in an elevated command prompt:</span></span>

    [!code-console[Main](signalr-performance/samples/sample4.cmd)]
- <span data-ttu-id="32330-154">**ApplicationPool QueueLength**: il s’agit du nombre maximal de demandes que Http.sys files d’attente du pool d’applications.</span><span class="sxs-lookup"><span data-stu-id="32330-154">**ApplicationPool QueueLength**: This is the maximum number of requests that Http.sys queues for the application pool.</span></span> <span data-ttu-id="32330-155">Lorsque la file d’attente est pleine, les nouvelles demandes reçoivent la réponse 503 « Service indisponible ».</span><span class="sxs-lookup"><span data-stu-id="32330-155">When the queue is full, new requests receive a 503 "Service Unavailable" response.</span></span> <span data-ttu-id="32330-156">La valeur par défaut est 1000.</span><span class="sxs-lookup"><span data-stu-id="32330-156">The default value is 1000.</span></span>

    <span data-ttu-id="32330-157">Raccourcir la longueur de file d’attente pour le processus de travail dans le pool d’applications qui héberge votre application sera économiser des ressources mémoire.</span><span class="sxs-lookup"><span data-stu-id="32330-157">Shortening the queue length for the worker process in the application pool hosting your application will conserve memory resources.</span></span> <span data-ttu-id="32330-158">Pour plus d’informations, consultez [gestion paramétrage et configuration des Pools d’applications](https://technet.microsoft.com/en-us/library/cc745955.aspx).</span><span class="sxs-lookup"><span data-stu-id="32330-158">For more information, see [Managing, Tuning, and Configuring Application Pools](https://technet.microsoft.com/en-us/library/cc745955.aspx).</span></span>

<span data-ttu-id="32330-159">**Paramètres de configuration ASP.NET**</span><span class="sxs-lookup"><span data-stu-id="32330-159">**ASP.NET configuration settings**</span></span>

<span data-ttu-id="32330-160">Cette section inclut des paramètres de configuration qui peuvent être définies dans le `aspnet.config` fichier.</span><span class="sxs-lookup"><span data-stu-id="32330-160">This section includes configuration settings that can be set in the `aspnet.config` file.</span></span> <span data-ttu-id="32330-161">Ce fichier se trouve dans un des deux emplacements, en fonction de la plateforme :</span><span class="sxs-lookup"><span data-stu-id="32330-161">This file is found in one of two locations, depending on platform:</span></span>

- `%windir%\Microsoft.NET\Framework\v4.0.30319\aspnet.config`
- `%windir%\Microsoft.NET\Framework64\v4.0.30319\aspnet.config`

<span data-ttu-id="32330-162">Les paramètres ASP.NET qui peuvent améliorer les performances de SignalR sont les suivantes :</span><span class="sxs-lookup"><span data-stu-id="32330-162">ASP.NET settings that may improve SignalR performance include the following:</span></span>

- <span data-ttu-id="32330-163">**Nombre maximal de demandes simultané par UC**: augmentation de ce paramètre peut atténuer les goulets d’étranglement.</span><span class="sxs-lookup"><span data-stu-id="32330-163">**Maximum concurrent requests per CPU**: Increasing this setting may alleviate performance bottlenecks.</span></span> <span data-ttu-id="32330-164">Pour augmenter ce paramètre, ajoutez le paramètre de configuration suivant à la `aspnet.config` fichier :</span><span class="sxs-lookup"><span data-stu-id="32330-164">To increase this setting, add the following configuration setting to the `aspnet.config` file:</span></span>

    [!code-xml[Main](signalr-performance/samples/sample5.xml?highlight=4)]
- <span data-ttu-id="32330-165">**Limite de la file d’attente des requêtes**: lorsque le nombre total de connexions dépasse les `maxConcurrentRequestsPerCPU` définition, ASP.NET commence à l’aide d’une file d’attente de demandes de limitation.</span><span class="sxs-lookup"><span data-stu-id="32330-165">**Request Queue Limit**: When the total number of connections exceeds the `maxConcurrentRequestsPerCPU` setting, ASP.NET will start throttling requests using a queue.</span></span> <span data-ttu-id="32330-166">Pour augmenter la taille de la file d’attente, vous pouvez augmenter la `requestQueueLimit` paramètre.</span><span class="sxs-lookup"><span data-stu-id="32330-166">To increase the size of the queue, you can increase the `requestQueueLimit` setting.</span></span> <span data-ttu-id="32330-167">Pour ce faire, ajoutez le paramètre de configuration suivant à la `processModel` nœud `config/machine.config` (plutôt que `aspnet.config`) :</span><span class="sxs-lookup"><span data-stu-id="32330-167">To do this, add the following configuration setting to the `processModel` node in `config/machine.config` (rather than `aspnet.config`):</span></span>

    [!code-xml[Main](signalr-performance/samples/sample6.xml)]

<a id="troubleshooting"></a>

## <a name="troubleshooting-performance-issues"></a><span data-ttu-id="32330-168">Résolution des problèmes de performances</span><span class="sxs-lookup"><span data-stu-id="32330-168">Troubleshooting performance issues</span></span>

<span data-ttu-id="32330-169">Cette section décrit les façons de rechercher les goulots d’étranglement dans votre application.</span><span class="sxs-lookup"><span data-stu-id="32330-169">This section describes ways to find performance bottlenecks in your application.</span></span>

### <a name="verifying-that-websocket-is-being-used"></a><span data-ttu-id="32330-170">Vérification que WebSocket est utilisé</span><span class="sxs-lookup"><span data-stu-id="32330-170">Verifying that WebSocket is being used</span></span>

<span data-ttu-id="32330-171">SignalR pouvez utiliser une variété de transports pour la communication entre le client et le serveur, WebSocket offre de meilleures performances significatif et doit être utilisé pour le client et le serveur de prise en charge.</span><span class="sxs-lookup"><span data-stu-id="32330-171">While SignalR can use a variety of transports for communication between client and server, WebSocket offers a significant performance advantage, and should be used if the client and server support it.</span></span> <span data-ttu-id="32330-172">Pour déterminer si votre client et le serveur de répondre à la configuration requise pour le WebSocket, consultez [du transport et secours](../getting-started/introduction-to-signalr.md#transports).</span><span class="sxs-lookup"><span data-stu-id="32330-172">To determine if your client and server meet the requirements for WebSocket, see [Transports and Fallbacks](../getting-started/introduction-to-signalr.md#transports).</span></span> <span data-ttu-id="32330-173">Pour déterminer que le transport est utilisé dans votre application, vous pouvez utiliser les outils de développement de navigateur et examinez les journaux pour vérifier que le transport est utilisé pour la connexion.</span><span class="sxs-lookup"><span data-stu-id="32330-173">To determine what transport is being used in your application, you can use the browser developer tools, and examine the logs to see what transport is being used for the connection.</span></span> <span data-ttu-id="32330-174">Pour plus d’informations sur l’utilisation des outils de développement de navigateur dans Internet Explorer et Chrome, consultez [du transport et secours](../getting-started/introduction-to-signalr.md#transports).</span><span class="sxs-lookup"><span data-stu-id="32330-174">For information on using the browser development tools in Internet Explorer and Chrome, see [Transports and Fallbacks](../getting-started/introduction-to-signalr.md#transports).</span></span>

<a id="perfcounters"></a>

## <a name="using-signalr-performance-counters"></a><span data-ttu-id="32330-175">À l’aide des compteurs de performance SignalR</span><span class="sxs-lookup"><span data-stu-id="32330-175">Using SignalR performance counters</span></span>

<span data-ttu-id="32330-176">Cette section décrit comment activer et utiliser des compteurs de performance SignalR, trouvé dans le `Microsoft.AspNet.SignalR.Utils` package.</span><span class="sxs-lookup"><span data-stu-id="32330-176">This section describes how to enable and use SignalR performance counters, found in the `Microsoft.AspNet.SignalR.Utils` package.</span></span>

### <a name="installing-signalrexe"></a><span data-ttu-id="32330-177">Signalr.exe lors de l’installation</span><span class="sxs-lookup"><span data-stu-id="32330-177">Installing signalr.exe</span></span>

<span data-ttu-id="32330-178">Compteurs de performances peuvent être ajoutés au serveur à l’aide d’un utilitaire appelé SignalR.exe.</span><span class="sxs-lookup"><span data-stu-id="32330-178">Performance counters can be added to the server using a utility called SignalR.exe.</span></span> <span data-ttu-id="32330-179">Pour installer cet utilitaire, procédez comme suit :</span><span class="sxs-lookup"><span data-stu-id="32330-179">To install this utility, follow these steps:</span></span>

1. <span data-ttu-id="32330-180">Dans votre application Visual Studio, sélectionnez **outils**, **Gestionnaire de Package de bibliothèque**, **gérer les Packages NuGet pour la Solution...**</span><span class="sxs-lookup"><span data-stu-id="32330-180">In your Visual Studio application, select **Tools**, **Library Package Manager**, **Manage NuGet Packages for Solution...**</span></span>
2. <span data-ttu-id="32330-181">Recherchez **signalr.utils**, puis sélectionnez Installer.</span><span class="sxs-lookup"><span data-stu-id="32330-181">Search for **signalr.utils**, and select Install.</span></span>

    ![](signalr-performance/_static/image1.png)
3. <span data-ttu-id="32330-182">Acceptez le contrat de licence pour installer le package.</span><span class="sxs-lookup"><span data-stu-id="32330-182">Accept the license agreement to install the package.</span></span>
4. <span data-ttu-id="32330-183">SignalR.exe sera installé dans `<project folder>/packages/Microsoft.AspNet.SignalR.Utils.<version>/tools`.</span><span class="sxs-lookup"><span data-stu-id="32330-183">SignalR.exe will be installed to `<project folder>/packages/Microsoft.AspNet.SignalR.Utils.<version>/tools`.</span></span>

### <a name="installing-performance-counters-with-signalrexe"></a><span data-ttu-id="32330-184">Installation des compteurs de performance avec SignalR.exe</span><span class="sxs-lookup"><span data-stu-id="32330-184">Installing performance counters with SignalR.exe</span></span>

<span data-ttu-id="32330-185">Pour installer les compteurs de performance SignalR, exécutez SignalR.exe dans une invite de commandes avec élévation de privilèges avec le paramètre suivant :</span><span class="sxs-lookup"><span data-stu-id="32330-185">To install SignalR performance counters, run SignalR.exe in an elevated command prompt with the following parameter:</span></span>

[!code-console[Main](signalr-performance/samples/sample7.cmd)]

<span data-ttu-id="32330-186">Pour supprimer les compteurs de performance SignalR, exécutez SignalR.exe dans une invite de commandes avec élévation de privilèges avec le paramètre suivant :</span><span class="sxs-lookup"><span data-stu-id="32330-186">To remove SignalR performance counters, run SignalR.exe in an elevated command prompt with the following parameter:</span></span>

[!code-console[Main](signalr-performance/samples/sample8.cmd)]

### <a name="signalr-performance-counters"></a><span data-ttu-id="32330-187">Compteurs de Performance de SignalR</span><span class="sxs-lookup"><span data-stu-id="32330-187">SignalR Performance counters</span></span>

<span data-ttu-id="32330-188">Le package d’utilitaires installe des compteurs de performances suivants.</span><span class="sxs-lookup"><span data-stu-id="32330-188">The utilities package installs the following performance counters.</span></span> <span data-ttu-id="32330-189">Les compteurs « Total » mesurent le nombre d’événements depuis le dernier pool d’applications ou le serveur de redémarrer.</span><span class="sxs-lookup"><span data-stu-id="32330-189">The "Total" counters measure the number of events since the last application pool or server restart.</span></span>

<span data-ttu-id="32330-190">**Métriques de connexion**</span><span class="sxs-lookup"><span data-stu-id="32330-190">**Connection metrics**</span></span>

<span data-ttu-id="32330-191">Les métriques suivantes mesurent les événements de durée de vie de connexion qui se produisent.</span><span class="sxs-lookup"><span data-stu-id="32330-191">The following metrics measure the connection lifetime events that occur.</span></span> <span data-ttu-id="32330-192">Pour plus d’informations, consultez [compréhension et gestion des événements de durée de vie de connexion](../guide-to-the-api/handling-connection-lifetime-events.md).</span><span class="sxs-lookup"><span data-stu-id="32330-192">For more information, see [Understanding and Handling Connection Lifetime Events](../guide-to-the-api/handling-connection-lifetime-events.md).</span></span>

- <span data-ttu-id="32330-193">**Connexions connectées**</span><span class="sxs-lookup"><span data-stu-id="32330-193">**Connections Connected**</span></span>
- <span data-ttu-id="32330-194">**Connexions reconnectées**</span><span class="sxs-lookup"><span data-stu-id="32330-194">**Connections Reconnected**</span></span>
- <span data-ttu-id="32330-195">**Connexions déconnecté**</span><span class="sxs-lookup"><span data-stu-id="32330-195">**Connections Disconnected**</span></span>
- <span data-ttu-id="32330-196">**Connexions en cours**</span><span class="sxs-lookup"><span data-stu-id="32330-196">**Connections Current**</span></span>

<span data-ttu-id="32330-197">**Métriques de message**</span><span class="sxs-lookup"><span data-stu-id="32330-197">**Message metrics**</span></span>

<span data-ttu-id="32330-198">Les métriques suivantes mesurent le trafic des messages généré par SignalR.</span><span class="sxs-lookup"><span data-stu-id="32330-198">The following metrics measure the message traffic generated by SignalR.</span></span>

- <span data-ttu-id="32330-199">**Nombre Total de Messages reçus connexion**</span><span class="sxs-lookup"><span data-stu-id="32330-199">**Connection Messages Received Total**</span></span>
- <span data-ttu-id="32330-200">**Nombre Total de Messages de connexion envoyés**</span><span class="sxs-lookup"><span data-stu-id="32330-200">**Connection Messages Sent Total**</span></span>
- <span data-ttu-id="32330-201">**Connexion de Messages reçus/s**</span><span class="sxs-lookup"><span data-stu-id="32330-201">**Connection Messages Received/Sec**</span></span>
- <span data-ttu-id="32330-202">**Connexion de Messages envoyés/s**</span><span class="sxs-lookup"><span data-stu-id="32330-202">**Connection Messages Sent/Sec**</span></span>

<span data-ttu-id="32330-203">**Métriques de bus de messages**</span><span class="sxs-lookup"><span data-stu-id="32330-203">**Message bus metrics**</span></span>

<span data-ttu-id="32330-204">Les métriques suivantes mesurent le trafic via le bus de messages SignalR interne, la file d’attente dans laquelle tous les messages de SignalR entrants et sortants sont placés.</span><span class="sxs-lookup"><span data-stu-id="32330-204">The following metrics measure traffic through the internal SignalR message bus, the queue in which all incoming and outgoing SignalR messages are placed.</span></span> <span data-ttu-id="32330-205">Un message est **publié** lorsqu’il est envoyé ou de diffusion.</span><span class="sxs-lookup"><span data-stu-id="32330-205">A message is **Published** when it is sent or broadcast.</span></span> <span data-ttu-id="32330-206">A **abonné** dans ce contexte est un abonnement sur le bus de messages ; il doit être égal au nombre de clients plus le serveur lui-même.</span><span class="sxs-lookup"><span data-stu-id="32330-206">A **Subscriber** in this context is a subscription on the message bus; this should equal the number of clients plus the server itself.</span></span> <span data-ttu-id="32330-207">Un **allouée de travail** est un composant qui envoie des données pour les connexions actives ; un **travail occupé** est une tâche qui envoie un message activement.</span><span class="sxs-lookup"><span data-stu-id="32330-207">An **Allocated Worker** is a component that sends data to active connections; a **Busy Worker** is one that is actively sending a message.</span></span>

- <span data-ttu-id="32330-208">**Total reçu des Messages de Bus de messages**</span><span class="sxs-lookup"><span data-stu-id="32330-208">**Message Bus Messages Received Total**</span></span>
- <span data-ttu-id="32330-209">**Bus de messages des Messages reçus/s**</span><span class="sxs-lookup"><span data-stu-id="32330-209">**Message Bus Messages Received/Sec**</span></span>
- <span data-ttu-id="32330-210">**Bus de messages Messages publiés Total**</span><span class="sxs-lookup"><span data-stu-id="32330-210">**Message Bus Messages Published Total**</span></span>
- <span data-ttu-id="32330-211">**Bus de messages des Messages publiés par seconde**</span><span class="sxs-lookup"><span data-stu-id="32330-211">**Message Bus Messages Published/Sec**</span></span>
- <span data-ttu-id="32330-212">**Message Bus abonnés actuel**</span><span class="sxs-lookup"><span data-stu-id="32330-212">**Message Bus Subscribers Current**</span></span>
- <span data-ttu-id="32330-213">**Nombre Total d’abonnés message Bus**</span><span class="sxs-lookup"><span data-stu-id="32330-213">**Message Bus Subscribers Total**</span></span>
- <span data-ttu-id="32330-214">**Les abonnés de Bus de messages/s**</span><span class="sxs-lookup"><span data-stu-id="32330-214">**Message Bus Subscribers/Sec**</span></span>
- <span data-ttu-id="32330-215">**Bus de messages allouée travailleurs**</span><span class="sxs-lookup"><span data-stu-id="32330-215">**Message Bus Allocated Workers**</span></span>
- <span data-ttu-id="32330-216">**Threads de travail occupés de Bus de messages**</span><span class="sxs-lookup"><span data-stu-id="32330-216">**Message Bus Busy Workers**</span></span>
- <span data-ttu-id="32330-217">**Actuel de rubriques de Bus de messages**</span><span class="sxs-lookup"><span data-stu-id="32330-217">**Message Bus Topics Current**</span></span>

<span data-ttu-id="32330-218">**Métriques de l’erreur**</span><span class="sxs-lookup"><span data-stu-id="32330-218">**Error metrics**</span></span>

<span data-ttu-id="32330-219">Les métriques suivantes mesurent les erreurs générées par le trafic des messages SignalR.</span><span class="sxs-lookup"><span data-stu-id="32330-219">The following metrics measure errors generated by SignalR message traffic.</span></span> <span data-ttu-id="32330-220">**Résolution de concentrateur** erreurs se produisent quand un concentrateur ou une méthode de concentrateur ne peut pas être résolue.</span><span class="sxs-lookup"><span data-stu-id="32330-220">**Hub Resolution** errors occur when a hub or hub method cannot be resolved.</span></span> <span data-ttu-id="32330-221">**Appel de concentrateur** erreurs sont des exceptions levées lors de l’appel d’une méthode de concentrateur.</span><span class="sxs-lookup"><span data-stu-id="32330-221">**Hub Invocation** errors are exceptions thrown while invoking a hub method.</span></span> <span data-ttu-id="32330-222">**Transport** erreurs sont des erreurs de connexion levées lors d’une demande HTTP ou d’une réponse.</span><span class="sxs-lookup"><span data-stu-id="32330-222">**Transport** errors are connection errors thrown during an HTTP request or response.</span></span>

- <span data-ttu-id="32330-223">**Erreurs : Total de toutes les**</span><span class="sxs-lookup"><span data-stu-id="32330-223">**Errors: All Total**</span></span>
- <span data-ttu-id="32330-224">**Erreurs : All/s**</span><span class="sxs-lookup"><span data-stu-id="32330-224">**Errors: All/Sec**</span></span>
- <span data-ttu-id="32330-225">**Erreurs : Total de résolution Hub**</span><span class="sxs-lookup"><span data-stu-id="32330-225">**Errors: Hub Resolution Total**</span></span>
- <span data-ttu-id="32330-226">**Erreurs : Résolution de concentrateur par seconde**</span><span class="sxs-lookup"><span data-stu-id="32330-226">**Errors: Hub Resolution/Sec**</span></span>
- <span data-ttu-id="32330-227">**Erreurs : Total de l’appel Hub**</span><span class="sxs-lookup"><span data-stu-id="32330-227">**Errors: Hub Invocation Total**</span></span>
- <span data-ttu-id="32330-228">**Erreurs : Appel de concentrateur par seconde**</span><span class="sxs-lookup"><span data-stu-id="32330-228">**Errors: Hub Invocation/Sec**</span></span>
- <span data-ttu-id="32330-229">**Erreurs : Total de Transport**</span><span class="sxs-lookup"><span data-stu-id="32330-229">**Errors: Transport Total**</span></span>
- <span data-ttu-id="32330-230">**Erreurs : Transport par seconde**</span><span class="sxs-lookup"><span data-stu-id="32330-230">**Errors: Transport/Sec**</span></span>

<a id="scaleout_metrics"></a>

<span data-ttu-id="32330-231">**Métriques de montée en puissance parallèle**</span><span class="sxs-lookup"><span data-stu-id="32330-231">**Scaleout metrics**</span></span>

<span data-ttu-id="32330-232">Les métriques suivantes mesurent le trafic et les erreurs générées par le fournisseur de montée en puissance parallèle.</span><span class="sxs-lookup"><span data-stu-id="32330-232">The following metrics measure traffic and errors generated by the scaleout provider.</span></span> <span data-ttu-id="32330-233">A **flux** dans ce contexte est une unité d’échelle utilisée par le fournisseur de montée en charge ; il s’agit une table si SQL Server est utilisée, une rubrique si Service Bus est utilisé et un abonnement si Redis est utilisé.</span><span class="sxs-lookup"><span data-stu-id="32330-233">A **Stream** in this context is a scale unit used by the scaleout provider; this is a table if SQL Server is used, a Topic if Service Bus is used, and a Subscription if Redis is used.</span></span> <span data-ttu-id="32330-234">Chaque flux garantit ordonnée en lecture et les opérations d’écriture ; un seul flux étant un goulot d’étranglement potentiel de mise à l’échelle, le nombre de flux peut être augmenté pour aider à réduire ce goulot d’étranglement.</span><span class="sxs-lookup"><span data-stu-id="32330-234">Each stream ensures ordered read and write operations; a single stream is a potential scale bottleneck, so the number of streams can be increased to help reduce that bottleneck.</span></span> <span data-ttu-id="32330-235">Si plusieurs flux de données est utilisés, SignalR distribue automatiquement des messages de (partition) dans ces flux d’une manière qui garantit que les messages envoyés à partir d’une connexion donnée sont dans l’ordre.</span><span class="sxs-lookup"><span data-stu-id="32330-235">If multiple streams are used, SignalR will automatically distribute (shard) messages across these streams in a way that ensures messages sent from any given connection are in order.</span></span>

<span data-ttu-id="32330-236">Le [MaxQueueLength](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.messaging.scaleoutconfiguration.maxqueuelength(v=vs.118).aspx) paramètre détermine la longueur de la file d’envoi de montée en puissance parallèle géré par SignalR.</span><span class="sxs-lookup"><span data-stu-id="32330-236">The [MaxQueueLength](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.messaging.scaleoutconfiguration.maxqueuelength(v=vs.118).aspx) setting controls the length of the scaleout send queue maintained by SignalR.</span></span> <span data-ttu-id="32330-237">Lui affectant une valeur supérieure à 0 place tous les messages dans une file d’attente d’envoi à envoyer à la fois au fond de panier de messagerie configuré.</span><span class="sxs-lookup"><span data-stu-id="32330-237">Setting it to a value greater than 0 will place all messages in a send queue to be sent one at a time to the configured messaging backplane.</span></span> <span data-ttu-id="32330-238">Si la taille de la file d’attente dépasse la longueur configurée, les appels suivants à envoyer va échouent immédiatement avec un [InvalidOperationException](https://msdn.microsoft.com/en-us/library/system.invalidoperationexception(v=vs.118).aspx) jusqu'à ce que le nombre de messages dans la file d’attente est inférieure à celle du paramètre à nouveau.</span><span class="sxs-lookup"><span data-stu-id="32330-238">If the size of the queue goes above the configured length, subsequent calls to send will fail immediately with an [InvalidOperationException](https://msdn.microsoft.com/en-us/library/system.invalidoperationexception(v=vs.118).aspx) until the number of messages in the queue is less than the setting again.</span></span> <span data-ttu-id="32330-239">File d’attente est désactivée par défaut, car les fonds de panier implémentés ont généralement leur propre queuing ou le contrôle de flux en place.</span><span class="sxs-lookup"><span data-stu-id="32330-239">Queueing is disabled by default because the implemented backplanes generally have their own queuing or flow-control in place.</span></span> <span data-ttu-id="32330-240">Dans le cas de SQL Server, le regroupement de connexions limite efficacement le nombre d’envois continue à un moment donné.</span><span class="sxs-lookup"><span data-stu-id="32330-240">In the case of SQL Server, connection pooling effectively limits the number of sends going on at any one time.</span></span>

<span data-ttu-id="32330-241">Par défaut, un seul flux est utilisé pour SQL Server et de Redis, cinq flux sont utilisés pour Service Bus et file d’attente est désactivée, mais ces paramètres peuvent être modifiés via la configuration de SQL Server et Service Bus :</span><span class="sxs-lookup"><span data-stu-id="32330-241">By default, only one stream is used for SQL Server and Redis, five streams are used for Service Bus, and queueing is disabled, but these settings can be changed through configuration on SQL Server and Service Bus:</span></span>

<span data-ttu-id="32330-242">**Code de serveur .NET pour la configuration de longueur de file d’attente et de nombre de la table pour fond de panier de SQL Server**</span><span class="sxs-lookup"><span data-stu-id="32330-242">**.NET Server Code for configuring table count and queue length for SQL Server backplane**</span></span>

[!code-csharp[Main](signalr-performance/samples/sample9.cs)]

<span data-ttu-id="32330-243">**Code de serveur .NET pour la configuration de longueur de file d’attente et de nombre de rubrique pour Service Bus de fond de panier**</span><span class="sxs-lookup"><span data-stu-id="32330-243">**.NET Server Code for configuring topic count and queue length for Service Bus backplane**</span></span>

[!code-csharp[Main](signalr-performance/samples/sample10.cs)]

<span data-ttu-id="32330-244">A **mise en mémoire tampon** flux de données est celui qui a entré un état d’erreur ; lorsque le flux de données est dans l’état par défaut, tous les messages envoyés au fond de panier échoue immédiatement jusqu'à ce que le flux de données n’est plus défaillante.</span><span class="sxs-lookup"><span data-stu-id="32330-244">A **Buffering** stream is one that has entered a faulted state; when the stream is in the faulted state, all messages sent to the backplane will fail immediately until the stream is no longer faulting.</span></span> <span data-ttu-id="32330-245">Le **longueur de file d’attente d’envoi** est le nombre de messages qui ont été validées mais pas encore été envoyées.</span><span class="sxs-lookup"><span data-stu-id="32330-245">The **Send Queue Length** is the number of messages that have been posted but not yet sent.</span></span>

- <span data-ttu-id="32330-246">**Bus de messages de montée en charge les Messages reçus/s**</span><span class="sxs-lookup"><span data-stu-id="32330-246">**Scaleout Message Bus Messages Received/Sec**</span></span>
- <span data-ttu-id="32330-247">**Nombre Total de flux de montée en puissance parallèle**</span><span class="sxs-lookup"><span data-stu-id="32330-247">**Scaleout Streams Total**</span></span>
- <span data-ttu-id="32330-248">**Ouvrir des flux de montée en puissance parallèle**</span><span class="sxs-lookup"><span data-stu-id="32330-248">**Scaleout Streams Open**</span></span>
- <span data-ttu-id="32330-249">**Mise en mémoire tampon de flux de montée en puissance parallèle**</span><span class="sxs-lookup"><span data-stu-id="32330-249">**Scaleout Streams Buffering**</span></span>
- <span data-ttu-id="32330-250">**Total des erreurs de montée en puissance parallèle**</span><span class="sxs-lookup"><span data-stu-id="32330-250">**Scaleout Errors Total**</span></span>
- <span data-ttu-id="32330-251">**Erreurs de montée en puissance parallèle par seconde**</span><span class="sxs-lookup"><span data-stu-id="32330-251">**Scaleout Errors/Sec**</span></span>
- <span data-ttu-id="32330-252">**Longueur de file d’attente d’envoi de montée en puissance parallèle**</span><span class="sxs-lookup"><span data-stu-id="32330-252">**Scaleout Send Queue Length**</span></span>

<span data-ttu-id="32330-253">Pour plus d’informations sur ce que ces compteurs sont de mesure, consultez [SignalR Scaleout avec Azure Service Bus](scaleout-with-windows-azure-service-bus.md).</span><span class="sxs-lookup"><span data-stu-id="32330-253">For more information on what these counters are measuring, see [SignalR Scaleout with Azure Service Bus](scaleout-with-windows-azure-service-bus.md).</span></span>

<a id="othercounters"></a>

## <a name="using-other-performance-counters"></a><span data-ttu-id="32330-254">À l’aide d’autres compteurs de performances</span><span class="sxs-lookup"><span data-stu-id="32330-254">Using other performance counters</span></span>

<span data-ttu-id="32330-255">Les compteurs de performances suivants peuvent également être utiles pour surveiller les performances de votre application.</span><span class="sxs-lookup"><span data-stu-id="32330-255">The following performance counters may also be useful in monitoring your application's performance.</span></span>

<span data-ttu-id="32330-256">**Mémoire**</span><span class="sxs-lookup"><span data-stu-id="32330-256">**Memory**</span></span>

- <span data-ttu-id="32330-257">Mémoire CLR .NET\\nombre d’octets dans tous les segments de mémoire (pour w3wp)</span><span class="sxs-lookup"><span data-stu-id="32330-257">.NET CLR Memory\\# bytes in all Heaps (for w3wp)</span></span>

<span data-ttu-id="32330-258">**ASP.NET**</span><span class="sxs-lookup"><span data-stu-id="32330-258">**ASP.NET**</span></span>

- <span data-ttu-id="32330-259">ASP. net\requêtes en cours</span><span class="sxs-lookup"><span data-stu-id="32330-259">ASP.NET\Requests Current</span></span>
- <span data-ttu-id="32330-260">ASP.NET\Queued</span><span class="sxs-lookup"><span data-stu-id="32330-260">ASP.NET\Queued</span></span>
- <span data-ttu-id="32330-261">ASP.NET\Rejected</span><span class="sxs-lookup"><span data-stu-id="32330-261">ASP.NET\Rejected</span></span>

<span data-ttu-id="32330-262">**PROCESSEUR**</span><span class="sxs-lookup"><span data-stu-id="32330-262">**CPU**</span></span>

- <span data-ttu-id="32330-263">Temps de processeur Information\Processor</span><span class="sxs-lookup"><span data-stu-id="32330-263">Processor Information\Processor Time</span></span>

<span data-ttu-id="32330-264">**TCP/IP**</span><span class="sxs-lookup"><span data-stu-id="32330-264">**TCP/IP**</span></span>

- <span data-ttu-id="32330-265">TCPv6/connexions établies</span><span class="sxs-lookup"><span data-stu-id="32330-265">TCPv6/Connections Established</span></span>
- <span data-ttu-id="32330-266">TCPv4/connexions établies</span><span class="sxs-lookup"><span data-stu-id="32330-266">TCPv4/Connections Established</span></span>

<span data-ttu-id="32330-267">**Le Service Web**</span><span class="sxs-lookup"><span data-stu-id="32330-267">**Web Service**</span></span>

- <span data-ttu-id="32330-268">Connexions Web service web\connexions en cours</span><span class="sxs-lookup"><span data-stu-id="32330-268">Web Service\Current Connections</span></span>
- <span data-ttu-id="32330-269">Connexions Web Service\Maximum</span><span class="sxs-lookup"><span data-stu-id="32330-269">Web Service\Maximum Connections</span></span>

<span data-ttu-id="32330-270">**Thread**</span><span class="sxs-lookup"><span data-stu-id="32330-270">**Threading**</span></span>

- <span data-ttu-id="32330-271">.NET verrous et Threads CLR\\# de Threads actuels logiques</span><span class="sxs-lookup"><span data-stu-id="32330-271">.NET CLR Locks And Threads\\# of current logical Threads</span></span>
- <span data-ttu-id="32330-272">.NET verrous et Threads CLR\\# de Threads actuels physiques</span><span class="sxs-lookup"><span data-stu-id="32330-272">.NET CLR Locks And Threads\\# of current physical Threads</span></span>

<a id="otherresources"></a>

## <a name="other-resources"></a><span data-ttu-id="32330-273">Autres ressources</span><span class="sxs-lookup"><span data-stu-id="32330-273">Other Resources</span></span>

<span data-ttu-id="32330-274">Pour plus d’informations sur les performances d’ASP.NET la surveillance et de paramétrage, consultez les rubriques suivantes :</span><span class="sxs-lookup"><span data-stu-id="32330-274">For more information on ASP.NET performance monitoring and tuning, see the following topics:</span></span>

- <span data-ttu-id="32330-275">[Vue d’ensemble des performances ASP.NET](https://msdn.microsoft.com/en-us/library/cc668225(v=vs.100).aspx)</span><span class="sxs-lookup"><span data-stu-id="32330-275">[ASP.NET Performance Overview](https://msdn.microsoft.com/en-us/library/cc668225(v=vs.100).aspx)</span></span>
- [<span data-ttu-id="32330-276">Utilisation de Thread ASP.NET sur IIS 6.0, IIS 7.0 et IIS 7.5</span><span class="sxs-lookup"><span data-stu-id="32330-276">ASP.NET Thread Usage on IIS 7.5, IIS 7.0, and IIS 6.0</span></span>](https://blogs.msdn.com/b/tmarq/archive/2007/07/21/asp-net-thread-usage-on-iis-7-0-and-6-0.aspx)
- [<span data-ttu-id="32330-277">&lt;applicationPool&gt; élément (paramètres Web)</span><span class="sxs-lookup"><span data-stu-id="32330-277">&lt;applicationPool&gt; Element (Web Settings)</span></span>](https://msdn.microsoft.com/en-us/library/dd560842.aspx)
