---
uid: aspnet/overview/owin-and-katana/katana-samples
title: Exemples de Katana | Documents Microsoft
author: microsoft
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/17/2014
ms.topic: article
ms.assetid: bec04f5d-2638-4417-b288-97c58c8d6379
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/owin-and-katana/katana-samples
msc.type: authoredcontent
ms.openlocfilehash: 5540164bda8db31c4e78b49ecb7f7c573acca013
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/10/2017
---
<a name="katana-samples"></a><span data-ttu-id="e92a9-102">Exemples de Katana</span><span class="sxs-lookup"><span data-stu-id="e92a9-102">Katana Samples</span></span>
====================
<span data-ttu-id="e92a9-103">par [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="e92a9-103">by [Microsoft](https://github.com/microsoft)</span></span>

## <a name="katana-samples"></a><span data-ttu-id="e92a9-104">Exemples de Katana</span><span class="sxs-lookup"><span data-stu-id="e92a9-104">Katana Samples</span></span>

<span data-ttu-id="e92a9-105">**ASP.NET achemine exemple** | [Code Source](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/AspNetRoutes/ReadMe.txt)</span><span class="sxs-lookup"><span data-stu-id="e92a9-105">**ASP.NET Routes Sample** | [Source Code](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/AspNetRoutes/ReadMe.txt)</span></span>  
<span data-ttu-id="e92a9-106">Dans certaines applications, vous devez connecter les composants OWIN dans la table d’itinéraires Asp.Net côte à côte avec les composants non-OWIN.</span><span class="sxs-lookup"><span data-stu-id="e92a9-106">In some applications you will want to hook up OWIN components in the Asp.Net route table side by side with non-OWIN components.</span></span> <span data-ttu-id="e92a9-107">Cet exemple montre comment utiliser les méthodes d’extension RouteCollection MapOwinPath et MapOwinRoute fournie par Microsoft.Owin.Host.SystemWeb.</span><span class="sxs-lookup"><span data-stu-id="e92a9-107">This sample shows how to use the RouteCollection extension methods MapOwinPath and MapOwinRoute provided by Microsoft.Owin.Host.SystemWeb.</span></span>

<span data-ttu-id="e92a9-108">**Créer une branche de Pipelines exemple** | [Code Source](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/BranchingPipelines/ReadMe.txt)</span><span class="sxs-lookup"><span data-stu-id="e92a9-108">**Branching Pipelines Sample** | [Source Code](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/BranchingPipelines/ReadMe.txt)</span></span>  
<span data-ttu-id="e92a9-109">Pipelines de traitement de la demande OWIN n’avez pas besoin d’être linéaire, ils peuvent être connecté pour traiter les demandes de différentes façons.</span><span class="sxs-lookup"><span data-stu-id="e92a9-109">OWIN request processing pipelines do not need to be linear, they can be branched to process requests in different ways.</span></span> <span data-ttu-id="e92a9-110">Cet exemple montre comment construire un pipeline de branchement basé sur les chemins d’accès de la demande ou d’autres données de la demande tels que les en-têtes.</span><span class="sxs-lookup"><span data-stu-id="e92a9-110">This sample shows how to construct a branching pipeline based on request paths or other request data such as headers.</span></span> <span data-ttu-id="e92a9-111">Ces composants sont disponibles dans le package nuget Microsoft.Owin.Mapping.</span><span class="sxs-lookup"><span data-stu-id="e92a9-111">These components are available in the Microsoft.Owin.Mapping nuget package.</span></span>

<span data-ttu-id="e92a9-112">**Exemple de serveur personnalisé** | [Code Source](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/CustomServer/MyCustomServer/CustomServer.cs) </span><span class="sxs-lookup"><span data-stu-id="e92a9-112">**Custom Server Sample** | [Source Code](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/CustomServer/MyCustomServer/CustomServer.cs) </span></span>  
<span data-ttu-id="e92a9-113">Montre comment utiliser un serveur OWIN personnalisé lorsque l’auto-hébergement OWIN.</span><span class="sxs-lookup"><span data-stu-id="e92a9-113">Shows how to use a custom OWIN server when self-hosting OWIN.</span></span>

<span data-ttu-id="e92a9-114">**Incorporé exemple** | [Code Source](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/Embedded/ReadMe.txt)</span><span class="sxs-lookup"><span data-stu-id="e92a9-114">**Embedded Sample** | [Source Code](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/Embedded/ReadMe.txt)</span></span>  
<span data-ttu-id="e92a9-115">Certains serveurs OWIN peuvent être exécutés à l’intérieur de votre propre processus (&quot;auto-hébergé&quot;).</span><span class="sxs-lookup"><span data-stu-id="e92a9-115">Some OWIN servers can be run inside of your own process (&quot;self-hosted&quot;).</span></span> <span data-ttu-id="e92a9-116">Cet exemple montre comment démarrer une application OWIN à l’aide des outils fournis par le package nuget Microsoft.Owin.Hosting.</span><span class="sxs-lookup"><span data-stu-id="e92a9-116">This sample shows how to start an OWIN application using the tools provided by the Microsoft.Owin.Hosting nuget package.</span></span>

<span data-ttu-id="e92a9-117">**Exemple HelloWorld** | [Code Source](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/HelloWorld/ReadMe.txt)</span><span class="sxs-lookup"><span data-stu-id="e92a9-117">**HelloWorld Sample** | [Source Code](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/HelloWorld/ReadMe.txt)</span></span>  
<span data-ttu-id="e92a9-118">OWIN est un abstraction d’API qui permet la portabilité des applications sur différents serveurs du serveur HTTP.</span><span class="sxs-lookup"><span data-stu-id="e92a9-118">OWIN is a HTTP server API abstraction that enables application portability across various servers.</span></span> <span data-ttu-id="e92a9-119">Cet exemple montre comment écrire une application Hello World utilisant certaines **wrappers simples** autour de l’abstraction de OWIN brutes et les exécuter sur un serveur web telles que ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="e92a9-119">This sample demonstrates how to write a Hello World application using some **simple wrappers** around the raw OWIN abstraction and run it on a web server like ASP.NET.</span></span>

<span data-ttu-id="e92a9-120">**Hello World (exemple) brut OWIN** | [Code Source](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/HelloWorldRawOwin/ReadMe.txt)</span><span class="sxs-lookup"><span data-stu-id="e92a9-120">**Hello World Raw OWIN Sample** | [Source Code](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/HelloWorldRawOwin/ReadMe.txt)</span></span>  
<span data-ttu-id="e92a9-121">Cet exemple montre comment écrire une application Hello World utilisant le **brutes** abstraction de OWIN et l’exécuter sur un serveur web comme Asp.Net.</span><span class="sxs-lookup"><span data-stu-id="e92a9-121">This sample demonstrates how to write a Hello World application using the **raw** OWIN abstraction and run it on a web server like Asp.Net.</span></span>

<span data-ttu-id="e92a9-122">**Exemple de SignalR** | [Code Source](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/SignalR/Program.cs)</span><span class="sxs-lookup"><span data-stu-id="e92a9-122">**SignalR Sample** | [Source Code](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/SignalR/Program.cs)</span></span>  
<span data-ttu-id="e92a9-123">Montre comment l’auto-hébergement SignalR à l’aide de OWIN / Katana.</span><span class="sxs-lookup"><span data-stu-id="e92a9-123">Shows how to self-host SignalR using OWIN / Katana.</span></span> <span data-ttu-id="e92a9-124">Pour plus d’informations sur SignalR d’auto-hébergement, consultez [didacticiel : SignalR auto-hébergement](../../../signalr/overview/deployment/tutorial-signalr-self-host.md).</span><span class="sxs-lookup"><span data-stu-id="e92a9-124">For more info about self-hosting SignalR, see [Tutorial: SignalR Self-Host](../../../signalr/overview/deployment/tutorial-signalr-self-host.md).</span></span>

<span data-ttu-id="e92a9-125">**Exemple de fichiers statiques** | [Code Source](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/StaticFilesSample/Startup.cs) </span><span class="sxs-lookup"><span data-stu-id="e92a9-125">**Static Files Sample** | [Source Code](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/StaticFilesSample/Startup.cs) </span></span>  
<span data-ttu-id="e92a9-126">Montre comment prendre en charge les requêtes HTTP pour les fichiers statiques à l’aide de OWIN / Katana.</span><span class="sxs-lookup"><span data-stu-id="e92a9-126">Shows how to support HTTP requests for static files using OWIN / Katana.</span></span>

<span data-ttu-id="e92a9-127">**Web API** | [Code Source](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/WebApi/ReadMe.txt) </span><span class="sxs-lookup"><span data-stu-id="e92a9-127">**Web API** | [Source Code](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/WebApi/ReadMe.txt) </span></span>  
<span data-ttu-id="e92a9-128">Cet exemple montre comment héberger OWIN dans IIS et ajouter des API Web au pipeline OWIN.</span><span class="sxs-lookup"><span data-stu-id="e92a9-128">This sample shows how to host OWIN in IIS and add Web API to the OWIN pipeline.</span></span>

<span data-ttu-id="e92a9-129">**Exemple de Socket de Web** | [Code Source](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/WebSocketSample/WebSocketServer/Startup.cs) </span><span class="sxs-lookup"><span data-stu-id="e92a9-129">**Web Socket Sample** | [Source Code](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/WebSocketSample/WebSocketServer/Startup.cs) </span></span>  
<span data-ttu-id="e92a9-130">Montre comment prendre en charge de WebSocket dans OWIN à l’aide de la [System.Net.WebSockets.WebSocket](https://msdn.microsoft.com/en-us/library/system.net.websockets.websocket(v=vs.110).aspx) classe.</span><span class="sxs-lookup"><span data-stu-id="e92a9-130">Shows how to support Web Sockets in OWIN by using the [System.Net.WebSockets.WebSocket](https://msdn.microsoft.com/en-us/library/system.net.websockets.websocket(v=vs.110).aspx) class.</span></span>
