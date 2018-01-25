---
uid: signalr/overview/older-versions/signalr-performance
title: SignalR performances (SignalR 1.x) | Documents Microsoft
author: pfletcher
description: Performances de SignalR
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/03/2013
ms.topic: article
ms.assetid: 9594d644-66b6-4223-acdd-23e29a6e4c46
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/older-versions/signalr-performance
msc.type: authoredcontent
ms.openlocfilehash: bad742af28d6c36bb1b66207c2ba09d140332449
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/24/2018
---
<a name="signalr-performance-signalr-1x"></a><span data-ttu-id="b469e-103">SignalR performances (SignalR 1.x)</span><span class="sxs-lookup"><span data-stu-id="b469e-103">SignalR Performance (SignalR 1.x)</span></span>
====================
<span data-ttu-id="b469e-104">par [Patrick Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="b469e-104">by [Patrick Fletcher](https://github.com/pfletcher)</span></span>

> <span data-ttu-id="b469e-105">Cette rubrique décrit comment concevoir pour, mesurer et améliorer les performances dans une application SignalR.</span><span class="sxs-lookup"><span data-stu-id="b469e-105">This topic describes how to design for, measure, and improve performance in a SignalR application.</span></span>


<span data-ttu-id="b469e-106">Pour une présentation récente sur SignalR performances et mise à l’échelle, consultez [mise à l’échelle Web en temps réel avec ASP.NET SignalR](https://channel9.msdn.com/Events/Build/2013/3-502).</span><span class="sxs-lookup"><span data-stu-id="b469e-106">For a recent presentation on SignalR performance and scaling, see [Scaling the Real-time Web with ASP.NET SignalR](https://channel9.msdn.com/Events/Build/2013/3-502).</span></span>

<span data-ttu-id="b469e-107">Cette rubrique contient les sections suivantes :</span><span class="sxs-lookup"><span data-stu-id="b469e-107">This topic contains the following sections:</span></span>

- [<span data-ttu-id="b469e-108">Considérations de conception</span><span class="sxs-lookup"><span data-stu-id="b469e-108">Design considerations</span></span>](#design)
- [<span data-ttu-id="b469e-109">Réglage de votre serveur SignalR pour des performances</span><span class="sxs-lookup"><span data-stu-id="b469e-109">Tuning your SignalR server for performance</span></span>](#tuning)
- [<span data-ttu-id="b469e-110">Résolution des problèmes de performances</span><span class="sxs-lookup"><span data-stu-id="b469e-110">Troubleshooting performance issues</span></span>](#troubleshooting)
- [<span data-ttu-id="b469e-111">À l’aide des compteurs de performance SignalR</span><span class="sxs-lookup"><span data-stu-id="b469e-111">Using SignalR performance counters</span></span>](#perfcounters)
- [<span data-ttu-id="b469e-112">À l’aide d’autres compteurs de performances</span><span class="sxs-lookup"><span data-stu-id="b469e-112">Using other performance counters</span></span>](#othercounters)
- [<span data-ttu-id="b469e-113">Autres ressources</span><span class="sxs-lookup"><span data-stu-id="b469e-113">Other resources</span></span>](#otherresources)

<a id="design"></a>

## <a name="design-considerations"></a><span data-ttu-id="b469e-114">Considérations de conception</span><span class="sxs-lookup"><span data-stu-id="b469e-114">Design considerations</span></span>

<span data-ttu-id="b469e-115">Cette section décrit les modèles qui peuvent être implémentées pendant la conception d’une application SignalR, pour vous assurer que les performances ne soit pas entravé en générant le trafic réseau inutile.</span><span class="sxs-lookup"><span data-stu-id="b469e-115">This section describes patterns that can be implemented during the design of a SignalR application, to ensure that performance is not being hindered by generating unnecessary network traffic.</span></span>

### <a name="throttling-message-frequency"></a><span data-ttu-id="b469e-116">Limitation de la fréquence des messages</span><span class="sxs-lookup"><span data-stu-id="b469e-116">Throttling message frequency</span></span>

<span data-ttu-id="b469e-117">Même dans une application qui envoie des messages à une fréquence élevée (par exemple, une application de jeu en temps réel), la plupart des applications n’avez pas besoin d’envoyer plusieurs messages par seconde.</span><span class="sxs-lookup"><span data-stu-id="b469e-117">Even in an application that sends out messages at a high frequency (such as a realtime gaming application), most applications don't need to send more than a few messages a second.</span></span> <span data-ttu-id="b469e-118">Pour réduire la quantité de trafic qui génère de chaque client, une boucle de message peut être implémentée que les files d’attente et envoie des messages ne dépasse pas fréquemment un taux fixe (autrement dit, un certain nombre de messages est envoyée par seconde, s’il existe des messages pendant cette période dans terval envoi).</span><span class="sxs-lookup"><span data-stu-id="b469e-118">To reduce the amount of traffic that each client generates, a message loop can be implemented that queues and sends out messages no more frequently than a fixed rate (that is, up to a certain number of messages will be sent every second, if there are messages in that time interval to be sent).</span></span> <span data-ttu-id="b469e-119">Pour un exemple d’application qui limite les messages à un certain taux (à partir de client et serveur), consultez [en temps réel de haute fréquence avec SignalR](../getting-started/tutorial-high-frequency-realtime-with-signalr.md).</span><span class="sxs-lookup"><span data-stu-id="b469e-119">For a sample application that throttles messages to a certain rate (from both client and server), see [High-Frequency Realtime with SignalR](../getting-started/tutorial-high-frequency-realtime-with-signalr.md).</span></span>

### <a name="reducing-message-size"></a><span data-ttu-id="b469e-120">La réduction de taille de message</span><span class="sxs-lookup"><span data-stu-id="b469e-120">Reducing message size</span></span>

<span data-ttu-id="b469e-121">Vous pouvez réduire la taille d’un message SignalR en réduisant la taille de vos objets sérialisés.</span><span class="sxs-lookup"><span data-stu-id="b469e-121">You can reduce the size of a SignalR message by reducing the size of your serialized objects.</span></span> <span data-ttu-id="b469e-122">Dans le code serveur, si vous envoyez un objet qui contient les propriétés que vous n’avez pas besoin d’être transmis, empêcher ces propriétés en cours de sérialisation à l’aide de la `JsonIgnore` attribut.</span><span class="sxs-lookup"><span data-stu-id="b469e-122">In server code, if you're sending an object that contains properties that don't need to be transmitted, prevent those properties from being serialized by using the `JsonIgnore` attribute.</span></span> <span data-ttu-id="b469e-123">Les noms des propriétés sont également stockées dans le message. les noms de propriétés peuvent être abrégés à l’aide de la `JsonProperty` attribut.</span><span class="sxs-lookup"><span data-stu-id="b469e-123">The names of properties are also stored in the message; the names of properties can be shortened using the `JsonProperty` attribute.</span></span> <span data-ttu-id="b469e-124">L’exemple de code suivant illustre comment exclure une propriété d’être envoyées au client et raccourcir les noms de propriété :</span><span class="sxs-lookup"><span data-stu-id="b469e-124">The following code sample demonstrates how to exclude a property from being sent to the client, and how to shorten property names:</span></span>

<span data-ttu-id="b469e-125">**Code de serveur .NET qui montre l’attribut JsonIgnore pour exclure des données envoyées au client et l’attribut JsonProperty pour réduire la taille de message**</span><span class="sxs-lookup"><span data-stu-id="b469e-125">**.NET server code that demonstrates the JsonIgnore attribute to exclude data from being sent to the client, and the JsonProperty attribute to reduce message size**</span></span>

[!code-csharp[Main](signalr-performance/samples/sample1.cs?highlight=5,7,10)]

<span data-ttu-id="b469e-126">Afin de conserver la lisibilité facilité de maintenance dans le code client, les noms de propriété abrégé peuvent être remappé vers convivial noms après la réception du message.</span><span class="sxs-lookup"><span data-stu-id="b469e-126">In order to retain readability/ maintainability in the client code, the abbreviated property names can be remapped to human-friendly names after the message is received.</span></span> <span data-ttu-id="b469e-127">L’exemple de code suivant illustre une façon possible de remappage des noms raccourcis à plus longues, en définissant un contrat de message (mappage) et à l’aide de la `reMap` fonction à appliquer le contrat à la classe de message optimisée :</span><span class="sxs-lookup"><span data-stu-id="b469e-127">The following code sample demonstrates one possible way of remapping shortened names to longer ones, by defining a message contract (mapping), and using the `reMap` function to apply the contract to the optimized message class:</span></span>

<span data-ttu-id="b469e-128">**Code JavaScript côté client qui remappe réduit les noms de propriété aux noms contrôlable de visu**</span><span class="sxs-lookup"><span data-stu-id="b469e-128">**Client-side JavaScript code that remaps shortened property names to human-readable names**</span></span>

[!code-javascript[Main](signalr-performance/samples/sample2.js)]

<span data-ttu-id="b469e-129">Noms peuvent être abrégés dans les messages à partir du client au serveur, à l’aide de la même méthode.</span><span class="sxs-lookup"><span data-stu-id="b469e-129">Names can be shortened in messages from the client to the server as well, using the same method.</span></span>

<span data-ttu-id="b469e-130">En diminuant l’encombrement de mémoire (autrement dit, la quantité de mémoire utilisée pour le message) du message d’objet peut également améliorer les performances.</span><span class="sxs-lookup"><span data-stu-id="b469e-130">Reducing the memory footprint (that is, the amount of memory used for the message) of the message object can also improve performance.</span></span> <span data-ttu-id="b469e-131">Par exemple, si la plage complète d’un `int` n’est pas nécessaire, un `short` ou `byte` peut être utilisé à la place.</span><span class="sxs-lookup"><span data-stu-id="b469e-131">For example, if the full range of an `int` is not needed, a `short` or `byte` can be used instead.</span></span>

<span data-ttu-id="b469e-132">Étant donné que les messages sont stockés dans le bus de messages dans la mémoire du serveur, ce qui réduit la taille des messages peut également résoudre les problèmes de mémoire de serveur.</span><span class="sxs-lookup"><span data-stu-id="b469e-132">Since messages are stored in the message bus in server memory, reducing the size of messages can also address server memory issues.</span></span>

<a id="tuning"></a>

### <a name="tuning-your-signalr-server-for-performance"></a><span data-ttu-id="b469e-133">Réglage de votre serveur SignalR pour des performances</span><span class="sxs-lookup"><span data-stu-id="b469e-133">Tuning your SignalR server for performance</span></span>

<span data-ttu-id="b469e-134">Les paramètres de configuration suivants peuvent être utilisés pour paramétrer votre serveur pour de meilleures performances dans une application SignalR.</span><span class="sxs-lookup"><span data-stu-id="b469e-134">The following configuration settings can be used to tune your server for better performance in a SignalR application.</span></span> <span data-ttu-id="b469e-135">Pour obtenir des informations générales sur la façon d’améliorer les performances dans une application ASP.NET, consultez [amélioration des performances ASP.NET](https://msdn.microsoft.com/library/ff647787.aspx).</span><span class="sxs-lookup"><span data-stu-id="b469e-135">For general information on how to improve performance in an ASP.NET application, see [Improving ASP.NET Performance](https://msdn.microsoft.com/library/ff647787.aspx).</span></span>

<span data-ttu-id="b469e-136">**Paramètres de configuration SignalR**</span><span class="sxs-lookup"><span data-stu-id="b469e-136">**SignalR configuration settings**</span></span>

- <span data-ttu-id="b469e-137">**DefaultMessageBufferSize**: par défaut, SignalR conserve 1 000 messages en mémoire par le concentrateur par connexion.</span><span class="sxs-lookup"><span data-stu-id="b469e-137">**DefaultMessageBufferSize**: By default, SignalR retains 1000 messages in memory per hub per connection.</span></span> <span data-ttu-id="b469e-138">Si les messages volumineux sont utilisés, cela peut créer des problèmes de mémoire qui peuvent être Soulagés par la réduction de cette valeur.</span><span class="sxs-lookup"><span data-stu-id="b469e-138">If large messages are being used, this may create memory issues which can be alleviated by reducing this value.</span></span> <span data-ttu-id="b469e-139">Ce paramètre peut être défini dans le `Application_Start` Gestionnaire d’événements dans une application ASP.NET, ou dans le `Configuration` méthode d’une classe de démarrage OWIN dans une application auto-hébergée.</span><span class="sxs-lookup"><span data-stu-id="b469e-139">This setting can be set in the `Application_Start` event handler in an ASP.NET application, or in the `Configuration` method of an OWIN startup class in a self-hosted application.</span></span> <span data-ttu-id="b469e-140">L’exemple suivant montre comment réduire cette valeur afin de réduire l’encombrement mémoire de votre application afin de réduire la quantité de mémoire du serveur :</span><span class="sxs-lookup"><span data-stu-id="b469e-140">The following sample demonstrates how to reduce this value in order to reduce the memory footprint of your application in order to reduce the amount of server memory used:</span></span>

    <span data-ttu-id="b469e-141">**Code de serveur .NET dans Global.asax pour diminuer la taille de mémoire tampon de message par défaut**</span><span class="sxs-lookup"><span data-stu-id="b469e-141">**.NET server code in Global.asax for decreasing default message buffer size**</span></span>

    [!code-csharp[Main](signalr-performance/samples/sample3.cs)]

<span data-ttu-id="b469e-142">**Paramètres de configuration IIS**</span><span class="sxs-lookup"><span data-stu-id="b469e-142">**IIS configuration settings**</span></span>

- <span data-ttu-id="b469e-143">**Nombre maximal de demandes simultanées par application**: augmenter le nombre de IIS simultanées demandes augmente les ressources serveur disponibles pour le traitement des demandes.</span><span class="sxs-lookup"><span data-stu-id="b469e-143">**Max concurrent requests per application**: Increasing the number of concurrent IIS requests will increase server resources available for serving requests.</span></span> <span data-ttu-id="b469e-144">La valeur par défaut est 5000 ; Pour augmenter ce paramètre, exécutez les commandes suivantes dans une invite de commandes avec élévation de privilèges :</span><span class="sxs-lookup"><span data-stu-id="b469e-144">The default value is 5000; to increase this setting, execute the following commands in an elevated command prompt:</span></span>

    [!code-console[Main](signalr-performance/samples/sample4.cmd)]

<span data-ttu-id="b469e-145">**Paramètres de configuration ASP.NET**</span><span class="sxs-lookup"><span data-stu-id="b469e-145">**ASP.NET configuration settings**</span></span>

<span data-ttu-id="b469e-146">Cette section inclut des paramètres de configuration qui peuvent être définies dans le `aspnet.config` fichier.</span><span class="sxs-lookup"><span data-stu-id="b469e-146">This section includes configuration settings that can be set in the `aspnet.config` file.</span></span> <span data-ttu-id="b469e-147">Ce fichier se trouve dans un des deux emplacements, en fonction de la plateforme :</span><span class="sxs-lookup"><span data-stu-id="b469e-147">This file is found in one of two locations, depending on platform:</span></span>

- `%windir%\Microsoft.NET\Framework\v4.0.30319\aspnet.config`
- `%windir%\Microsoft.NET\Framework64\v4.0.30319\aspnet.config`

<span data-ttu-id="b469e-148">Les paramètres ASP.NET qui peuvent améliorer les performances de SignalR sont les suivantes :</span><span class="sxs-lookup"><span data-stu-id="b469e-148">ASP.NET settings that may improve SignalR performance include the following:</span></span>

- <span data-ttu-id="b469e-149">**Nombre maximal de demandes simultané par UC**: augmentation de ce paramètre peut atténuer les goulets d’étranglement.</span><span class="sxs-lookup"><span data-stu-id="b469e-149">**Maximum concurrent requests per CPU**: Increasing this setting may alleviate performance bottlenecks.</span></span> <span data-ttu-id="b469e-150">Pour augmenter ce paramètre, ajoutez le paramètre de configuration suivant à la `aspnet.config` fichier :</span><span class="sxs-lookup"><span data-stu-id="b469e-150">To increase this setting, add the following configuration setting to the `aspnet.config` file:</span></span>

    [!code-xml[Main](signalr-performance/samples/sample5.xml?highlight=4)]
- <span data-ttu-id="b469e-151">**Limite de la file d’attente des requêtes**: lorsque le nombre total de connexions dépasse les `maxConcurrentRequestsPerCPU` définition, ASP.NET commence à l’aide d’une file d’attente de demandes de limitation.</span><span class="sxs-lookup"><span data-stu-id="b469e-151">**Request Queue Limit**: When the total number of connections exceeds the `maxConcurrentRequestsPerCPU` setting, ASP.NET will start throttling requests using a queue.</span></span> <span data-ttu-id="b469e-152">Pour augmenter la taille de la file d’attente, vous pouvez augmenter la `requestQueueLimit` paramètre.</span><span class="sxs-lookup"><span data-stu-id="b469e-152">To increase the size of the queue, you can increase the `requestQueueLimit` setting.</span></span> <span data-ttu-id="b469e-153">Pour ce faire, ajoutez le paramètre de configuration suivant à la `processModel` nœud `config/machine.config` (plutôt que `aspnet.config`) :</span><span class="sxs-lookup"><span data-stu-id="b469e-153">To do this, add the following configuration setting to the `processModel` node in `config/machine.config` (rather than `aspnet.config`):</span></span>

    [!code-xml[Main](signalr-performance/samples/sample6.xml)]

<a id="troubleshooting"></a>

## <a name="troubleshooting-performance-issues"></a><span data-ttu-id="b469e-154">Résolution des problèmes de performances</span><span class="sxs-lookup"><span data-stu-id="b469e-154">Troubleshooting performance issues</span></span>

<span data-ttu-id="b469e-155">Cette section décrit les façons de rechercher les goulots d’étranglement dans votre application.</span><span class="sxs-lookup"><span data-stu-id="b469e-155">This section describes ways to find performance bottlenecks in your application.</span></span>

### <a name="verifying-that-websocket-is-being-used"></a><span data-ttu-id="b469e-156">Vérification que WebSocket est utilisé</span><span class="sxs-lookup"><span data-stu-id="b469e-156">Verifying that WebSocket is being used</span></span>

<span data-ttu-id="b469e-157">SignalR pouvez utiliser une variété de transports pour la communication entre le client et le serveur, WebSocket offre de meilleures performances significatif et doit être utilisé pour le client et le serveur de prise en charge.</span><span class="sxs-lookup"><span data-stu-id="b469e-157">While SignalR can use a variety of transports for communication between client and server, WebSocket offers a significant performance advantage, and should be used if the client and server support it.</span></span> <span data-ttu-id="b469e-158">Pour déterminer si votre client et le serveur de répondre à la configuration requise pour le WebSocket, consultez [du transport et secours](../getting-started/introduction-to-signalr.md#transports).</span><span class="sxs-lookup"><span data-stu-id="b469e-158">To determine if your client and server meet the requirements for WebSocket, see [Transports and Fallbacks](../getting-started/introduction-to-signalr.md#transports).</span></span> <span data-ttu-id="b469e-159">Pour déterminer que le transport est utilisé dans votre application, vous pouvez utiliser les outils de développement de navigateur et examinez les journaux pour vérifier que le transport est utilisé pour la connexion.</span><span class="sxs-lookup"><span data-stu-id="b469e-159">To determine what transport is being used in your application, you can use the browser developer tools, and examine the logs to see what transport is being used for the connection.</span></span> <span data-ttu-id="b469e-160">Pour plus d’informations sur l’utilisation des outils de développement de navigateur dans Internet Explorer et Chrome, consultez [du transport et secours](../getting-started/introduction-to-signalr.md#transports).</span><span class="sxs-lookup"><span data-stu-id="b469e-160">For information on using the browser development tools in Internet Explorer and Chrome, see [Transports and Fallbacks](../getting-started/introduction-to-signalr.md#transports).</span></span>

<a id="perfcounters"></a>

## <a name="using-signalr-performance-counters"></a><span data-ttu-id="b469e-161">À l’aide des compteurs de performance SignalR</span><span class="sxs-lookup"><span data-stu-id="b469e-161">Using SignalR performance counters</span></span>

<span data-ttu-id="b469e-162">Cette section décrit comment activer et utiliser des compteurs de performance SignalR, trouvé dans le `Microsoft.AspNet.SignalR.Utils` package.</span><span class="sxs-lookup"><span data-stu-id="b469e-162">This section describes how to enable and use SignalR performance counters, found in the `Microsoft.AspNet.SignalR.Utils` package.</span></span>

### <a name="installing-signalrexe"></a><span data-ttu-id="b469e-163">Signalr.exe lors de l’installation</span><span class="sxs-lookup"><span data-stu-id="b469e-163">Installing signalr.exe</span></span>

<span data-ttu-id="b469e-164">Compteurs de performances peuvent être ajoutés au serveur à l’aide d’un utilitaire appelé SignalR.exe.</span><span class="sxs-lookup"><span data-stu-id="b469e-164">Peformance counters can be added to the server using a utility called SignalR.exe.</span></span> <span data-ttu-id="b469e-165">Pour installer cet utilitaire, procédez comme suit :</span><span class="sxs-lookup"><span data-stu-id="b469e-165">To install this utility, follow these steps:</span></span>

1. <span data-ttu-id="b469e-166">Dans votre application Visual Studio, sélectionnez **outils**, **Gestionnaire de Package de bibliothèque**, **gérer les Packages NuGet pour la Solution...**</span><span class="sxs-lookup"><span data-stu-id="b469e-166">In your Visual Studio application, select **Tools**, **Library Package Manager**, **Manage NuGet Packages for Solution...**</span></span>
2. <span data-ttu-id="b469e-167">Recherchez **signalr.utils**, puis sélectionnez Installer.</span><span class="sxs-lookup"><span data-stu-id="b469e-167">Search for **signalr.utils**, and select Install.</span></span>

    ![](signalr-performance/_static/image1.png)
3. <span data-ttu-id="b469e-168">Acceptez le contrat de licence pour installer le package.</span><span class="sxs-lookup"><span data-stu-id="b469e-168">Accept the license agreement to install the package.</span></span>
4. <span data-ttu-id="b469e-169">SignalR.exe sera installé dans `<project folder>/packages/Microsoft.AspNet.SignalR.Utils.<version>/tools`.</span><span class="sxs-lookup"><span data-stu-id="b469e-169">SignalR.exe will be installed to `<project folder>/packages/Microsoft.AspNet.SignalR.Utils.<version>/tools`.</span></span>

### <a name="installing-performance-counters-with-signalrexe"></a><span data-ttu-id="b469e-170">Installation des compteurs de performance avec SignalR.exe</span><span class="sxs-lookup"><span data-stu-id="b469e-170">Installing performance counters with SignalR.exe</span></span>

<span data-ttu-id="b469e-171">Pour installer les compteurs de performance SignalR, exécutez SignalR.exe dans une invite de commandes avec élévation de privilèges avec le paramètre suivant :</span><span class="sxs-lookup"><span data-stu-id="b469e-171">To install SignalR performance counters, run SignalR.exe in an elevated command prompt with the following parameter:</span></span>

[!code-console[Main](signalr-performance/samples/sample7.cmd)]

<span data-ttu-id="b469e-172">Pour supprimer les compteurs de performance SignalR, exécutez SignalR.exe dans une invite de commandes avec élévation de privilèges avec le paramètre suivant :</span><span class="sxs-lookup"><span data-stu-id="b469e-172">To remove SignalR performance counters, run SignalR.exe in an elevated command prompt with the following parameter:</span></span>

[!code-console[Main](signalr-performance/samples/sample8.cmd)]

### <a name="signalr-performance-counters"></a><span data-ttu-id="b469e-173">Compteurs de Performance de SignalR</span><span class="sxs-lookup"><span data-stu-id="b469e-173">SignalR Performance counters</span></span>

<span data-ttu-id="b469e-174">Le package d’utilitaires installe des compteurs de performances suivants.</span><span class="sxs-lookup"><span data-stu-id="b469e-174">The utilites package installs the following performance counters.</span></span> <span data-ttu-id="b469e-175">Les compteurs « Total » mesurent le nombre d’événements depuis le dernier pool d’applications ou le serveur de redémarrer.</span><span class="sxs-lookup"><span data-stu-id="b469e-175">The "Total" counters measure the number of events since the last application pool or server restart.</span></span>

<span data-ttu-id="b469e-176">**Métriques de connexion**</span><span class="sxs-lookup"><span data-stu-id="b469e-176">**Connection metrics**</span></span>

<span data-ttu-id="b469e-177">Les métriques suivantes mesurent les événements de durée de vie de connexion qui se produisent.</span><span class="sxs-lookup"><span data-stu-id="b469e-177">The following metrics measure the connection lifetime events that occur.</span></span> <span data-ttu-id="b469e-178">Pour plus d’informations, consultez [compréhension et gestion des événements de durée de vie de connexion](../guide-to-the-api/handling-connection-lifetime-events.md).</span><span class="sxs-lookup"><span data-stu-id="b469e-178">For more information, see [Understanding and Handling Connection Lifetime Events](../guide-to-the-api/handling-connection-lifetime-events.md).</span></span>

- <span data-ttu-id="b469e-179">**Connexions connectées**</span><span class="sxs-lookup"><span data-stu-id="b469e-179">**Connections Connected**</span></span>
- <span data-ttu-id="b469e-180">**Connexions reconnectées**</span><span class="sxs-lookup"><span data-stu-id="b469e-180">**Connections Reconnected**</span></span>
- <span data-ttu-id="b469e-181">**Connexions Disonnected**</span><span class="sxs-lookup"><span data-stu-id="b469e-181">**Connections Disonnected**</span></span>
- <span data-ttu-id="b469e-182">**Connexions en cours**</span><span class="sxs-lookup"><span data-stu-id="b469e-182">**Connections Current**</span></span>

<span data-ttu-id="b469e-183">**Métriques de message**</span><span class="sxs-lookup"><span data-stu-id="b469e-183">**Message metrics**</span></span>

<span data-ttu-id="b469e-184">Les métriques suivantes mesurent le trafic des messages généré par SignalR.</span><span class="sxs-lookup"><span data-stu-id="b469e-184">The following metrics measure the message traffic generated by SignalR.</span></span>

- <span data-ttu-id="b469e-185">**Nombre Total de Messages reçus connexion**</span><span class="sxs-lookup"><span data-stu-id="b469e-185">**Connection Messages Received Total**</span></span>
- <span data-ttu-id="b469e-186">**Nombre Total de Messages de connexion envoyés**</span><span class="sxs-lookup"><span data-stu-id="b469e-186">**Connection Messages Sent Total**</span></span>
- <span data-ttu-id="b469e-187">**Connexion de Messages reçus/s**</span><span class="sxs-lookup"><span data-stu-id="b469e-187">**Connection Messages Received/Sec**</span></span>
- <span data-ttu-id="b469e-188">**Connexion de Messages envoyés/s**</span><span class="sxs-lookup"><span data-stu-id="b469e-188">**Connection Messages Sent/Sec**</span></span>

<span data-ttu-id="b469e-189">**Métriques de bus de messages**</span><span class="sxs-lookup"><span data-stu-id="b469e-189">**Message bus metrics**</span></span>

<span data-ttu-id="b469e-190">Les métriques suivantes mesurent le trafic via le bus de messages SignalR interne, la file d’attente dans laquelle tous les messages de SignalR entrants et sortants sont placés.</span><span class="sxs-lookup"><span data-stu-id="b469e-190">The following metrics measure traffic through the internal SignalR message bus, the queue in which all incoming and outgoing SignalR messages are placed.</span></span> <span data-ttu-id="b469e-191">Un message est **publié** lorsqu’il est envoyé ou de diffusion.</span><span class="sxs-lookup"><span data-stu-id="b469e-191">A message is **Published** when it is sent or broadcast.</span></span> <span data-ttu-id="b469e-192">A **abonné** dans ce contexte est un abonnement sur le bus de messages ; il doit être égal au nombre de clients plus le serveur lui-même.</span><span class="sxs-lookup"><span data-stu-id="b469e-192">A **Subscriber** in this context is a subscription on the message bus; this should equal the number of clients plus the server itself.</span></span> <span data-ttu-id="b469e-193">Un **allouée de travail** est un composant qui envoie des données pour les connexions actives ; un **travail occupé** est une tâche qui envoie un message activement.</span><span class="sxs-lookup"><span data-stu-id="b469e-193">An **Allocated Worker** is a component that sends data to active connections; a **Busy Worker** is one that is actively sending a message.</span></span>

- <span data-ttu-id="b469e-194">**Total reçu des Messages de Bus de messages**</span><span class="sxs-lookup"><span data-stu-id="b469e-194">**Message Bus Messages Received Total**</span></span>
- <span data-ttu-id="b469e-195">**Bus de messages des Messages reçus/s**</span><span class="sxs-lookup"><span data-stu-id="b469e-195">**Message Bus Messages Received/Sec**</span></span>
- <span data-ttu-id="b469e-196">**Bus de messages Messages publiés Total**</span><span class="sxs-lookup"><span data-stu-id="b469e-196">**Message Bus Messages Published Total**</span></span>
- <span data-ttu-id="b469e-197">**Bus de messages des Messages publiés par seconde**</span><span class="sxs-lookup"><span data-stu-id="b469e-197">**Message Bus Messages Published/Sec**</span></span>
- <span data-ttu-id="b469e-198">**Message Bus abonnés actuel**</span><span class="sxs-lookup"><span data-stu-id="b469e-198">**Message Bus Subscribers Current**</span></span>
- <span data-ttu-id="b469e-199">**Nombre Total d’abonnés message Bus**</span><span class="sxs-lookup"><span data-stu-id="b469e-199">**Message Bus Subscribers Total**</span></span>
- <span data-ttu-id="b469e-200">**Message Bus Subscribers/Sec**</span><span class="sxs-lookup"><span data-stu-id="b469e-200">**Message Bus Subscribers/Sec**</span></span>
- <span data-ttu-id="b469e-201">**Bus de messages allouée travailleurs**</span><span class="sxs-lookup"><span data-stu-id="b469e-201">**Message Bus Allocated Workers**</span></span>
- <span data-ttu-id="b469e-202">**Threads de travail occupés de Bus de messages**</span><span class="sxs-lookup"><span data-stu-id="b469e-202">**Message Bus Busy Workers**</span></span>
- <span data-ttu-id="b469e-203">**Actuel de rubriques de Bus de messages**</span><span class="sxs-lookup"><span data-stu-id="b469e-203">**Message Bus Topics Current**</span></span>

<span data-ttu-id="b469e-204">**Métriques de l’erreur**</span><span class="sxs-lookup"><span data-stu-id="b469e-204">**Error metrics**</span></span>

<span data-ttu-id="b469e-205">Les métriques suivantes mesurent les erreurs générées par le trafic des messages SignalR.</span><span class="sxs-lookup"><span data-stu-id="b469e-205">The following metrics measure errors generated by SignalR message traffic.</span></span> <span data-ttu-id="b469e-206">**Résolution de concentrateur** erreurs se produisent quand un concentrateur ou une méthode de concentrateur ne peut pas être résolue.</span><span class="sxs-lookup"><span data-stu-id="b469e-206">**Hub Resolution** errors occur when a hub or hub method cannot be resolved.</span></span> <span data-ttu-id="b469e-207">**Appel de concentrateur** erreurs sont des exceptions levées lors de l’appel d’une méthode de concentrateur.</span><span class="sxs-lookup"><span data-stu-id="b469e-207">**Hub Invocation** errors are exceptions thrown while invoking a hub method.</span></span> <span data-ttu-id="b469e-208">**Transport** erreurs sont des erreurs de connexion levées lors d’une demande HTTP ou d’une réponse.</span><span class="sxs-lookup"><span data-stu-id="b469e-208">**Transport** errors are connection errors thrown during an HTTP request or response.</span></span>

- <span data-ttu-id="b469e-209">**Erreurs : Total de toutes les**</span><span class="sxs-lookup"><span data-stu-id="b469e-209">**Errors: All Total**</span></span>
- <span data-ttu-id="b469e-210">**Erreurs : All/s**</span><span class="sxs-lookup"><span data-stu-id="b469e-210">**Errors: All/Sec**</span></span>
- <span data-ttu-id="b469e-211">**Erreurs : Total de résolution Hub**</span><span class="sxs-lookup"><span data-stu-id="b469e-211">**Errors: Hub Resolution Total**</span></span>
- <span data-ttu-id="b469e-212">**Erreurs : Résolution de concentrateur par seconde**</span><span class="sxs-lookup"><span data-stu-id="b469e-212">**Errors: Hub Resolution/Sec**</span></span>
- <span data-ttu-id="b469e-213">**Erreurs : Total de l’appel Hub**</span><span class="sxs-lookup"><span data-stu-id="b469e-213">**Errors: Hub Invocation Total**</span></span>
- <span data-ttu-id="b469e-214">**Erreurs : Appel de concentrateur par seconde**</span><span class="sxs-lookup"><span data-stu-id="b469e-214">**Errors: Hub Invocation/Sec**</span></span>
- <span data-ttu-id="b469e-215">**Erreurs : Total de Transport**</span><span class="sxs-lookup"><span data-stu-id="b469e-215">**Errors: Transport Total**</span></span>
- <span data-ttu-id="b469e-216">**Erreurs : Transport par seconde**</span><span class="sxs-lookup"><span data-stu-id="b469e-216">**Errors: Transport/Sec**</span></span>

<span data-ttu-id="b469e-217">**Métriques de montée en puissance parallèle**</span><span class="sxs-lookup"><span data-stu-id="b469e-217">**Scaleout metrics**</span></span>

<span data-ttu-id="b469e-218">Les métriques suivantes mesurent le trafic et les erreurs générées par le fournisseur de montée en puissance parallèle.</span><span class="sxs-lookup"><span data-stu-id="b469e-218">The following metrics measure traffic and errors generated by the scaleout provider.</span></span> <span data-ttu-id="b469e-219">A **flux** dans ce contexte est une unité d’échelle utilisée par le fournisseur de montée en charge ; il s’agit une table si SQL Server est utilisée, une rubrique si Service Bus est utilisé et un abonnement si Redis est utilisé.</span><span class="sxs-lookup"><span data-stu-id="b469e-219">A **Stream** in this context is a scale unit used by the scaleout provider; this is a table if SQL Server is used, a Topic if Service Bus is used, and a Subscription if Redis is used.</span></span> <span data-ttu-id="b469e-220">Par défaut, un seul flux est utilisé, mais vous pouvez l’augmenter par la configuration sur SQL Server et Service Bus.</span><span class="sxs-lookup"><span data-stu-id="b469e-220">By default, only one stream is used, but this can be increased through configuration on SQL Server and Service Bus.</span></span> <span data-ttu-id="b469e-221">A **mise en mémoire tampon** flux de données est celui qui a entré un état d’erreur ; lorsque le flux de données est dans l’état par défaut, tous les messages envoyés au fond de panier échoue immédiatement jusqu'à ce que le flux de données n’est plus défaillante.</span><span class="sxs-lookup"><span data-stu-id="b469e-221">A **Buffering** stream is one that has entered a faulted state; when the stream is in the faulted state, all messages sent to the backplane will fail immediately until the stream is no longer faulting.</span></span> <span data-ttu-id="b469e-222">Le **longueur de file d’attente d’envoi** est le nombre de messages qui ont été validées mais pas encore été envoyées.</span><span class="sxs-lookup"><span data-stu-id="b469e-222">The **Send Queue Length** is the number of messages that have been posted but not yet sent.</span></span>

- <span data-ttu-id="b469e-223">**Bus de messages de montée en charge les Messages reçus/s**</span><span class="sxs-lookup"><span data-stu-id="b469e-223">**Scaleout Message Bus Messages Received/Sec**</span></span>
- <span data-ttu-id="b469e-224">**Nombre Total de flux de montée en puissance parallèle**</span><span class="sxs-lookup"><span data-stu-id="b469e-224">**Scaleout Streams Total**</span></span>
- <span data-ttu-id="b469e-225">**Ouvrir des flux de montée en puissance parallèle**</span><span class="sxs-lookup"><span data-stu-id="b469e-225">**Scaleout Streams Open**</span></span>
- <span data-ttu-id="b469e-226">**Mise en mémoire tampon de flux de montée en puissance parallèle**</span><span class="sxs-lookup"><span data-stu-id="b469e-226">**Scaleout Streams Buffering**</span></span>
- <span data-ttu-id="b469e-227">**Total des erreurs de montée en puissance parallèle**</span><span class="sxs-lookup"><span data-stu-id="b469e-227">**Scaleout Errors Total**</span></span>
- <span data-ttu-id="b469e-228">**Erreurs de montée en puissance parallèle par seconde**</span><span class="sxs-lookup"><span data-stu-id="b469e-228">**Scaleout Errors/Sec**</span></span>
- <span data-ttu-id="b469e-229">**Longueur de file d’attente d’envoi de montée en puissance parallèle**</span><span class="sxs-lookup"><span data-stu-id="b469e-229">**Scaleout Send Queue Length**</span></span>

<span data-ttu-id="b469e-230">Pour plus d’informations sur ce que ces compteurs sont de mesure, consultez [SignalR Scaleout avec Azure Service Bus](scaleout-with-windows-azure-service-bus.md).</span><span class="sxs-lookup"><span data-stu-id="b469e-230">For more information on what these counters are measuring, see [SignalR Scaleout with Azure Service Bus](scaleout-with-windows-azure-service-bus.md).</span></span>

<a id="othercounters"></a>

## <a name="using-other-performance-counters"></a><span data-ttu-id="b469e-231">À l’aide d’autres compteurs de performances</span><span class="sxs-lookup"><span data-stu-id="b469e-231">Using other performance counters</span></span>

<span data-ttu-id="b469e-232">Les compteurs de performances suivants peuvent également être utiles pour surveiller les performances de votre application.</span><span class="sxs-lookup"><span data-stu-id="b469e-232">The following performance counters may also be useful in monitoring your application's performance.</span></span>

<span data-ttu-id="b469e-233">**Memory**</span><span class="sxs-lookup"><span data-stu-id="b469e-233">**Memory**</span></span>

- <span data-ttu-id="b469e-234">.NET CLR mémoire Nb d’octets dans tous les segments de mémoire (pour w3wp)</span><span class="sxs-lookup"><span data-stu-id="b469e-234">.NET CLR Memory# bytes in all Heaps (for w3wp)</span></span>

<span data-ttu-id="b469e-235">**ASP.NET**</span><span class="sxs-lookup"><span data-stu-id="b469e-235">**ASP.NET**</span></span>

- <span data-ttu-id="b469e-236">ASP. net\requêtes en cours</span><span class="sxs-lookup"><span data-stu-id="b469e-236">ASP.NET\Requests Current</span></span>
- <span data-ttu-id="b469e-237">ASP.NET\Queued</span><span class="sxs-lookup"><span data-stu-id="b469e-237">ASP.NET\Queued</span></span>
- <span data-ttu-id="b469e-238">ASP.NET\Rejected</span><span class="sxs-lookup"><span data-stu-id="b469e-238">ASP.NET\Rejected</span></span>

<span data-ttu-id="b469e-239">**CPU**</span><span class="sxs-lookup"><span data-stu-id="b469e-239">**CPU**</span></span>

- <span data-ttu-id="b469e-240">Temps de processeur Information\Processor</span><span class="sxs-lookup"><span data-stu-id="b469e-240">Processor Information\Processor Time</span></span>

<span data-ttu-id="b469e-241">**TCP/IP**</span><span class="sxs-lookup"><span data-stu-id="b469e-241">**TCP/IP**</span></span>

- <span data-ttu-id="b469e-242">TCPv6/connexions établies</span><span class="sxs-lookup"><span data-stu-id="b469e-242">TCPv6/Connections Established</span></span>
- <span data-ttu-id="b469e-243">TCPv4/connexions établies</span><span class="sxs-lookup"><span data-stu-id="b469e-243">TCPv4/Connections Established</span></span>

<span data-ttu-id="b469e-244">**Le Service Web**</span><span class="sxs-lookup"><span data-stu-id="b469e-244">**Web Service**</span></span>

- <span data-ttu-id="b469e-245">Connexions Web service web\connexions en cours</span><span class="sxs-lookup"><span data-stu-id="b469e-245">Web Service\Current Connections</span></span>
- <span data-ttu-id="b469e-246">Connexions Web Service\Maximum</span><span class="sxs-lookup"><span data-stu-id="b469e-246">Web Service\Maximum Connections</span></span>

<span data-ttu-id="b469e-247">**Thread**</span><span class="sxs-lookup"><span data-stu-id="b469e-247">**Threading**</span></span>

- <span data-ttu-id="b469e-248">.NET CLR LocksAndThreads\# de Threads actuels logiques</span><span class="sxs-lookup"><span data-stu-id="b469e-248">.NET CLR LocksAndThreads\# of current logical Threads</span></span>
- <span data-ttu-id="b469e-249">.NET CLR LocksAnd Threads\# de Threads actuels physiques</span><span class="sxs-lookup"><span data-stu-id="b469e-249">.NET CLR LocksAnd Threads\# of current physical Threads</span></span>

<a id="otherresources"></a>

## <a name="other-resources"></a><span data-ttu-id="b469e-250">Autres ressources</span><span class="sxs-lookup"><span data-stu-id="b469e-250">Other Resources</span></span>

<span data-ttu-id="b469e-251">Pour plus d’informations sur les performances d’ASP.NET la surveillance et de paramétrage, consultez les rubriques suivantes :</span><span class="sxs-lookup"><span data-stu-id="b469e-251">For more information on ASP.NET performance monitoring and tuning, see the following topics:</span></span>

- <span data-ttu-id="b469e-252">[Vue d’ensemble des performances ASP.NET](https://msdn.microsoft.com/library/cc668225(v=vs.100).aspx)</span><span class="sxs-lookup"><span data-stu-id="b469e-252">[ASP.NET Performance Overview](https://msdn.microsoft.com/library/cc668225(v=vs.100).aspx)</span></span>
- [<span data-ttu-id="b469e-253">Utilisation de Thread ASP.NET sur IIS 6.0, IIS 7.0 et IIS 7.5</span><span class="sxs-lookup"><span data-stu-id="b469e-253">ASP.NET Thread Usage on IIS 7.5, IIS 7.0, and IIS 6.0</span></span>](https://blogs.msdn.com/b/tmarq/archive/2007/07/21/asp-net-thread-usage-on-iis-7-0-and-6-0.aspx)
- [<span data-ttu-id="b469e-254">&lt;applicationPool&gt; élément (paramètres Web)</span><span class="sxs-lookup"><span data-stu-id="b469e-254">&lt;applicationPool&gt; Element (Web Settings)</span></span>](https://msdn.microsoft.com/library/dd560842.aspx)
