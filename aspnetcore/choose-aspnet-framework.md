---
title: Choisir entre ASP.NET et ASP.NET Core
author: rick-anderson
description: Découvrez comment choisir entre ASP.NET et ASP.NET Core.
manager: wpickett
ms.author: riande
ms.date: 03/14/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/choose-between-aspnet-and-aspnetcore
ms.openlocfilehash: e6ac9f54ef623895b81eea33d90791e5f0ad5120
ms.sourcegitcommit: 71b93b42cbce8a9b1a12c4d88391e75a4dfb6162
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/20/2018
---
# <a name="choose-between-aspnet-and-aspnet-core"></a><span data-ttu-id="86800-103">Choisir entre ASP.NET et ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="86800-103">Choose between ASP.NET and ASP.NET Core</span></span>

<span data-ttu-id="86800-104">Quelle que soit l’application web que vous créez, ASP.NET a une solution pour vous : depuis les applications web d’entreprise ciblant Windows Server jusqu’aux microservices ciblant des conteneurs Linux, et tout ce qui se trouve entre les deux.</span><span class="sxs-lookup"><span data-stu-id="86800-104">No matter the web app you're creating, ASP.NET has a solution for you: from enterprise web apps targeting Windows Server, to small microservices targeting Linux containers, and everything in between.</span></span>

## <a name="aspnet-core"></a><span data-ttu-id="86800-105">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="86800-105">ASP.NET Core</span></span>

<span data-ttu-id="86800-106">ASP.NET Core est un framework open source multiplateforme qui permet de créer des applications web cloud modernes sur Windows, macOS et Linux.</span><span class="sxs-lookup"><span data-stu-id="86800-106">ASP.NET Core is an open-source, cross-platform framework for building modern, cloud-based web apps on Windows, macOS, or Linux.</span></span>

## <a name="aspnet"></a><span data-ttu-id="86800-107">ASP.NET</span><span class="sxs-lookup"><span data-stu-id="86800-107">ASP.NET</span></span>

<span data-ttu-id="86800-108">ASP.NET est un framework mature qui offre tous les services nécessaires pour créer des applications web serveur d’entreprise sur Windows.</span><span class="sxs-lookup"><span data-stu-id="86800-108">ASP.NET is a mature framework that provides all the services needed to build enterprise-class, server-based web apps on Windows.</span></span>

## <a name="which-one-is-right-for-me"></a><span data-ttu-id="86800-109">Laquelle des deux est adaptée à mes besoins ?</span><span class="sxs-lookup"><span data-stu-id="86800-109">Which one is right for me?</span></span>

| <span data-ttu-id="86800-110">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="86800-110">ASP.NET Core</span></span> | <span data-ttu-id="86800-111">ASP.NET</span><span class="sxs-lookup"><span data-stu-id="86800-111">ASP.NET</span></span> |
|---|---|
|<span data-ttu-id="86800-112">Générer pour Windows, macOS ou Linux</span><span class="sxs-lookup"><span data-stu-id="86800-112">Build for Windows, macOS, or Linux</span></span>|<span data-ttu-id="86800-113">Générer pour Windows</span><span class="sxs-lookup"><span data-stu-id="86800-113">Build for Windows</span></span>|
|<span data-ttu-id="86800-114">Nous vous recommandons d’utiliser les [pages Razor](xref:mvc/razor-pages/index) pour créer une interface utilisateur web à partir d’ASP.NET Core 2.</span><span class="sxs-lookup"><span data-stu-id="86800-114">[Razor Pages](xref:mvc/razor-pages/index) is the recommended approach to create a Web UI as of ASP.NET Core 2.x.</span></span> <span data-ttu-id="86800-115">Voir aussi [MVC](xref:mvc/overview), [API web](xref:tutorials/first-web-api) et [SignalR](xref:signalr/introduction).</span><span class="sxs-lookup"><span data-stu-id="86800-115">See also [MVC](xref:mvc/overview), [Web API](xref:tutorials/first-web-api), and [SignalR](xref:signalr/introduction).</span></span>|<span data-ttu-id="86800-116">Utiliser [Web Forms](/aspnet/web-forms), [SignalR](/aspnet/signalr), [MVC](/aspnet/mvc), [API Web](/aspnet/web-api/), ou [Pages Web](/aspnet/web-pages)</span><span class="sxs-lookup"><span data-stu-id="86800-116">Use [Web Forms](/aspnet/web-forms), [SignalR](/aspnet/signalr), [MVC](/aspnet/mvc), [Web API](/aspnet/web-api/), or [Web Pages](/aspnet/web-pages)</span></span>|
|<span data-ttu-id="86800-117">Plusieurs versions par machine</span><span class="sxs-lookup"><span data-stu-id="86800-117">Multiple versions per machine</span></span>|<span data-ttu-id="86800-118">Une seule version par machine</span><span class="sxs-lookup"><span data-stu-id="86800-118">One version per machine</span></span>|
|<span data-ttu-id="86800-119">Développer avec Visual Studio, [Visual Studio pour Mac](https://www.visualstudio.com/vs/visual-studio-mac/) ou [Visual Studio Code](https://code.visualstudio.com/) en utilisant C# ou F#</span><span class="sxs-lookup"><span data-stu-id="86800-119">Develop with Visual Studio, [Visual Studio for Mac](https://www.visualstudio.com/vs/visual-studio-mac/), or [Visual Studio Code](https://code.visualstudio.com/) using C# or F#</span></span>|<span data-ttu-id="86800-120">Développer avec Visual Studio en utilisant C#, VB ou F#</span><span class="sxs-lookup"><span data-stu-id="86800-120">Develop with Visual Studio using C#, VB, or F#</span></span>|
|<span data-ttu-id="86800-121">Performances supérieures à celles d’ASP.NET</span><span class="sxs-lookup"><span data-stu-id="86800-121">Higher performance than ASP.NET</span></span>|<span data-ttu-id="86800-122">Bonnes performances</span><span class="sxs-lookup"><span data-stu-id="86800-122">Good performance</span></span>|
|[<span data-ttu-id="86800-123">Choisir le runtime .NET Framework ou .NET Core</span><span class="sxs-lookup"><span data-stu-id="86800-123">Choose .NET Framework or .NET Core runtime</span></span>](/dotnet/articles/standard/choosing-core-framework-server)|<span data-ttu-id="86800-124">Utiliser le runtime .NET Framework</span><span class="sxs-lookup"><span data-stu-id="86800-124">Use .NET Framework runtime</span></span>|

## <a name="aspnet-core-scenarios"></a><span data-ttu-id="86800-125">Scénarios ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="86800-125">ASP.NET Core scenarios</span></span>

<!-- update link to Razor Pages mvc movie series when done -->
* <span data-ttu-id="86800-126">Nous vous recommandons d’utiliser les [pages Razor](xref:mvc/razor-pages/index) pour créer une interface utilisateur web à partir d’ASP.NET Core 2.</span><span class="sxs-lookup"><span data-stu-id="86800-126">[Razor Pages](xref:mvc/razor-pages/index) is the recommended approach to create a Web UI as of ASP.NET Core 2.x.</span></span>
* [<span data-ttu-id="86800-127">Sites web</span><span class="sxs-lookup"><span data-stu-id="86800-127">Websites</span></span>](xref:tutorials/first-mvc-app/index)
* [<span data-ttu-id="86800-128">API</span><span class="sxs-lookup"><span data-stu-id="86800-128">APIs</span></span>](xref:tutorials/first-web-api)
* [<span data-ttu-id="86800-129">Temps réel</span><span class="sxs-lookup"><span data-stu-id="86800-129">Real-time</span></span>](xref:signalr/index)

## <a name="aspnet-scenarios"></a><span data-ttu-id="86800-130">Scénarios ASP.NET</span><span class="sxs-lookup"><span data-stu-id="86800-130">ASP.NET scenarios</span></span>

* [<span data-ttu-id="86800-131">Sites web</span><span class="sxs-lookup"><span data-stu-id="86800-131">Websites</span></span>](/aspnet/mvc)
* [<span data-ttu-id="86800-132">API</span><span class="sxs-lookup"><span data-stu-id="86800-132">APIs</span></span>](/aspnet/web-api)
* [<span data-ttu-id="86800-133">Temps réel</span><span class="sxs-lookup"><span data-stu-id="86800-133">Real-time</span></span>](/aspnet/signalr)

## <a name="resources"></a><span data-ttu-id="86800-134">Ressources</span><span class="sxs-lookup"><span data-stu-id="86800-134">Resources</span></span>

* [<span data-ttu-id="86800-135">Présentation d’ASP.NET</span><span class="sxs-lookup"><span data-stu-id="86800-135">Introduction to ASP.NET</span></span>](/aspnet/overview)
* [<span data-ttu-id="86800-136">Présentation d’ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="86800-136">Introduction to ASP.NET Core</span></span>](xref:index)
