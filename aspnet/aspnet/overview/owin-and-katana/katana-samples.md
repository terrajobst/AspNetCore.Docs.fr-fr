---
uid: aspnet/overview/owin-and-katana/katana-samples
title: Exemples Katana | Microsoft Docs
author: microsoft
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/17/2014
ms.topic: article
ms.assetid: bec04f5d-2638-4417-b288-97c58c8d6379
ms.technology: ''
msc.legacyurl: /aspnet/overview/owin-and-katana/katana-samples
msc.type: authoredcontent
ms.openlocfilehash: e81da1e650d8dfd24a3e0fda6aa42b7f360ce12d
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37393317"
---
<a name="katana-samples"></a><span data-ttu-id="4bd22-102">Exemples Katana</span><span class="sxs-lookup"><span data-stu-id="4bd22-102">Katana Samples</span></span>
====================
<span data-ttu-id="4bd22-103">par [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="4bd22-103">by [Microsoft](https://github.com/microsoft)</span></span>

## <a name="katana-samples"></a><span data-ttu-id="4bd22-104">Exemples Katana</span><span class="sxs-lookup"><span data-stu-id="4bd22-104">Katana Samples</span></span>

<span data-ttu-id="4bd22-105">**ASP.NET achemine exemple** | [Code Source](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/AspNetRoutes/ReadMe.txt)</span><span class="sxs-lookup"><span data-stu-id="4bd22-105">**ASP.NET Routes Sample** | [Source Code](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/AspNetRoutes/ReadMe.txt)</span></span>  
<span data-ttu-id="4bd22-106">Dans certaines applications, vous devez raccorder les composants OWIN dans la table de routage Asp.Net côte à côte avec les composants non OWIN.</span><span class="sxs-lookup"><span data-stu-id="4bd22-106">In some applications you will want to hook up OWIN components in the Asp.Net route table side by side with non-OWIN components.</span></span> <span data-ttu-id="4bd22-107">Cet exemple montre comment utiliser les méthodes d’extension RouteCollection MapOwinPath et MapOwinRoute fournies par Microsoft.Owin.Host.SystemWeb.</span><span class="sxs-lookup"><span data-stu-id="4bd22-107">This sample shows how to use the RouteCollection extension methods MapOwinPath and MapOwinRoute provided by Microsoft.Owin.Host.SystemWeb.</span></span>

<span data-ttu-id="4bd22-108">**Création de branches Pipelines exemple** | [Code Source](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/BranchingPipelines/ReadMe.txt)</span><span class="sxs-lookup"><span data-stu-id="4bd22-108">**Branching Pipelines Sample** | [Source Code](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/BranchingPipelines/ReadMe.txt)</span></span>  
<span data-ttu-id="4bd22-109">Pipelines de traitement de la demande OWIN n’êtes pas obligé d’être linéaire, ils peuvent comprendre des branches pour traiter les demandes de différentes façons.</span><span class="sxs-lookup"><span data-stu-id="4bd22-109">OWIN request processing pipelines do not need to be linear, they can be branched to process requests in different ways.</span></span> <span data-ttu-id="4bd22-110">Cet exemple montre comment construire un pipeline de branchement basé sur les chemins d’accès de la demande ou d’autres données de requête tels que les en-têtes.</span><span class="sxs-lookup"><span data-stu-id="4bd22-110">This sample shows how to construct a branching pipeline based on request paths or other request data such as headers.</span></span> <span data-ttu-id="4bd22-111">Ces composants sont disponibles dans le package nuget de Microsoft.Owin.Mapping.</span><span class="sxs-lookup"><span data-stu-id="4bd22-111">These components are available in the Microsoft.Owin.Mapping nuget package.</span></span>

<span data-ttu-id="4bd22-112">**Exemple de serveur personnalisé** | [Code Source](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/CustomServer/MyCustomServer/CustomServer.cs) </span><span class="sxs-lookup"><span data-stu-id="4bd22-112">**Custom Server Sample** | [Source Code](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/CustomServer/MyCustomServer/CustomServer.cs) </span></span>  
<span data-ttu-id="4bd22-113">Montre comment utiliser un serveur OWIN personnalisé lors de l’auto-hébergement OWIN.</span><span class="sxs-lookup"><span data-stu-id="4bd22-113">Shows how to use a custom OWIN server when self-hosting OWIN.</span></span>

<span data-ttu-id="4bd22-114">**Incorporé exemple** | [Code Source](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/Embedded/ReadMe.txt)</span><span class="sxs-lookup"><span data-stu-id="4bd22-114">**Embedded Sample** | [Source Code](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/Embedded/ReadMe.txt)</span></span>  
<span data-ttu-id="4bd22-115">Certains serveurs OWIN peuvent être exécutés à l’intérieur de votre propre processus (&quot;auto-hébergé&quot;).</span><span class="sxs-lookup"><span data-stu-id="4bd22-115">Some OWIN servers can be run inside of your own process (&quot;self-hosted&quot;).</span></span> <span data-ttu-id="4bd22-116">Cet exemple montre comment démarrer une application OWIN à l’aide des outils fournis par le package nuget Microsoft.Owin.Hosting.</span><span class="sxs-lookup"><span data-stu-id="4bd22-116">This sample shows how to start an OWIN application using the tools provided by the Microsoft.Owin.Hosting nuget package.</span></span>

<span data-ttu-id="4bd22-117">**Exemple HelloWorld** | [Code Source](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/HelloWorld/ReadMe.txt)</span><span class="sxs-lookup"><span data-stu-id="4bd22-117">**HelloWorld Sample** | [Source Code](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/HelloWorld/ReadMe.txt)</span></span>  
<span data-ttu-id="4bd22-118">OWIN est un abstraction d’API qui permet la portabilité des applications sur différents serveurs du serveur HTTP.</span><span class="sxs-lookup"><span data-stu-id="4bd22-118">OWIN is a HTTP server API abstraction that enables application portability across various servers.</span></span> <span data-ttu-id="4bd22-119">Cet exemple montre comment écrire une application Hello World à l’aide de certaines **wrappers simples** autour de l’abstraction de OWIN brutes et les exécuter sur un serveur web, tel que ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="4bd22-119">This sample demonstrates how to write a Hello World application using some **simple wrappers** around the raw OWIN abstraction and run it on a web server like ASP.NET.</span></span>

<span data-ttu-id="4bd22-120">**Exemple Hello World brutes OWIN** | [Code Source](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/HelloWorldRawOwin/ReadMe.txt)</span><span class="sxs-lookup"><span data-stu-id="4bd22-120">**Hello World Raw OWIN Sample** | [Source Code](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/HelloWorldRawOwin/ReadMe.txt)</span></span>  
<span data-ttu-id="4bd22-121">Cet exemple montre comment écrire une application Hello World à l’aide du **brutes** abstraction de OWIN et l’exécuter sur un serveur web comme Asp.Net.</span><span class="sxs-lookup"><span data-stu-id="4bd22-121">This sample demonstrates how to write a Hello World application using the **raw** OWIN abstraction and run it on a web server like Asp.Net.</span></span>

<span data-ttu-id="4bd22-122">**Exemple de SignalR** | [Code Source](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/SignalR/Program.cs)</span><span class="sxs-lookup"><span data-stu-id="4bd22-122">**SignalR Sample** | [Source Code](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/SignalR/Program.cs)</span></span>  
<span data-ttu-id="4bd22-123">Montre comment l’auto-hébergement de SignalR à l’aide d’OWIN / Katana.</span><span class="sxs-lookup"><span data-stu-id="4bd22-123">Shows how to self-host SignalR using OWIN / Katana.</span></span> <span data-ttu-id="4bd22-124">Pour plus d’informations sur d’auto-hébergement de SignalR, consultez [didacticiel : auto-hébergement de SignalR](../../../signalr/overview/deployment/tutorial-signalr-self-host.md).</span><span class="sxs-lookup"><span data-stu-id="4bd22-124">For more info about self-hosting SignalR, see [Tutorial: SignalR Self-Host](../../../signalr/overview/deployment/tutorial-signalr-self-host.md).</span></span>

<span data-ttu-id="4bd22-125">**Exemple de fichiers statiques** | [Code Source](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/StaticFilesSample/Startup.cs) </span><span class="sxs-lookup"><span data-stu-id="4bd22-125">**Static Files Sample** | [Source Code](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/StaticFilesSample/Startup.cs) </span></span>  
<span data-ttu-id="4bd22-126">Montre comment prendre en charge des requêtes HTTP pour les fichiers statiques à l’aide d’OWIN / Katana.</span><span class="sxs-lookup"><span data-stu-id="4bd22-126">Shows how to support HTTP requests for static files using OWIN / Katana.</span></span>

<span data-ttu-id="4bd22-127">**API Web** | [Code Source](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/WebApi/ReadMe.txt) </span><span class="sxs-lookup"><span data-stu-id="4bd22-127">**Web API** | [Source Code](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/WebApi/ReadMe.txt) </span></span>  
<span data-ttu-id="4bd22-128">Cet exemple montre comment héberger OWIN dans IIS et ajouter des API Web au pipeline OWIN.</span><span class="sxs-lookup"><span data-stu-id="4bd22-128">This sample shows how to host OWIN in IIS and add Web API to the OWIN pipeline.</span></span>

<span data-ttu-id="4bd22-129">**Exemple de Socket de Web** | [Code Source](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/WebSocketSample/WebSocketServer/Startup.cs) </span><span class="sxs-lookup"><span data-stu-id="4bd22-129">**Web Socket Sample** | [Source Code](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/WebSocketSample/WebSocketServer/Startup.cs) </span></span>  
<span data-ttu-id="4bd22-130">Montre comment prendre en charge Web Sockets dans OWIN à l’aide de la [System.Net.WebSockets.WebSocket](https://msdn.microsoft.com/library/system.net.websockets.websocket(v=vs.110).aspx) classe.</span><span class="sxs-lookup"><span data-stu-id="4bd22-130">Shows how to support Web Sockets in OWIN by using the [System.Net.WebSockets.WebSocket](https://msdn.microsoft.com/library/system.net.websockets.websocket(v=vs.110).aspx) class.</span></span>
