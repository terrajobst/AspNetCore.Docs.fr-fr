---
title: Choisir entre ASP.NET et ASP.NET Core
author: rick-anderson
description: Découvrez comment choisir entre ASP.NET et ASP.NET Core.
ms.author: riande
ms.date: 05/11/2018
uid: fundamentals/choose-between-aspnet-and-aspnetcore
ms.openlocfilehash: 6d759c0bc5e5c7d32d6c14786db6ba9fe7a2f1e8
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/20/2018
ms.locfileid: "36297228"
---
# <a name="choose-between-aspnet-and-aspnet-core"></a><span data-ttu-id="e1e41-103">Choisir entre ASP.NET et ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e1e41-103">Choose between ASP.NET and ASP.NET Core</span></span>

<span data-ttu-id="e1e41-104">Quelle que soit l’application web que vous créez, ASP.NET a une solution pour vous : depuis les applications web d’entreprise ciblant Windows Server jusqu’aux microservices ciblant des conteneurs Linux, et tout ce qui se trouve entre les deux.</span><span class="sxs-lookup"><span data-stu-id="e1e41-104">No matter the web app you're creating, ASP.NET has a solution for you: from enterprise web apps targeting Windows Server, to small microservices targeting Linux containers, and everything in between.</span></span>

## <a name="aspnet-core"></a><span data-ttu-id="e1e41-105">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e1e41-105">ASP.NET Core</span></span>

<span data-ttu-id="e1e41-106">ASP.NET Core est un framework open source multiplateforme qui permet de créer des applications web cloud modernes sur Windows, macOS et Linux.</span><span class="sxs-lookup"><span data-stu-id="e1e41-106">ASP.NET Core is an open-source, cross-platform framework for building modern, cloud-based web apps on Windows, macOS, or Linux.</span></span>

## <a name="aspnet"></a><span data-ttu-id="e1e41-107">ASP.NET</span><span class="sxs-lookup"><span data-stu-id="e1e41-107">ASP.NET</span></span>

<span data-ttu-id="e1e41-108">ASP.NET est un framework mature qui offre tous les services nécessaires pour créer des applications web serveur d’entreprise sur Windows.</span><span class="sxs-lookup"><span data-stu-id="e1e41-108">ASP.NET is a mature framework that provides all the services needed to build enterprise-grade, server-based web apps on Windows.</span></span>

## <a name="framework-selection"></a><span data-ttu-id="e1e41-109">Sélection du Framework</span><span class="sxs-lookup"><span data-stu-id="e1e41-109">Framework selection</span></span>

<span data-ttu-id="e1e41-110">Passez en revue le tableau ci-dessous pour déterminer le framework qui convient le mieux à vos besoins.</span><span class="sxs-lookup"><span data-stu-id="e1e41-110">Review the table below to determine which framework is most appropriate for your needs.</span></span>

| <span data-ttu-id="e1e41-111">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e1e41-111">ASP.NET Core</span></span> | <span data-ttu-id="e1e41-112">ASP.NET</span><span class="sxs-lookup"><span data-stu-id="e1e41-112">ASP.NET</span></span> |
|---|---|
|<span data-ttu-id="e1e41-113">Générer pour Windows, macOS ou Linux</span><span class="sxs-lookup"><span data-stu-id="e1e41-113">Build for Windows, macOS, or Linux</span></span>|<span data-ttu-id="e1e41-114">Générer pour Windows</span><span class="sxs-lookup"><span data-stu-id="e1e41-114">Build for Windows</span></span>|
|<span data-ttu-id="e1e41-115">Nous vous recommandons d’utiliser les [pages Razor](xref:razor-pages/index) pour créer une interface utilisateur web à partir d’ASP.NET Core 2.</span><span class="sxs-lookup"><span data-stu-id="e1e41-115">[Razor Pages](xref:razor-pages/index) is the recommended approach to create a Web UI as of ASP.NET Core 2.x.</span></span> <span data-ttu-id="e1e41-116">Voir aussi [MVC](xref:mvc/overview), [API web](xref:tutorials/first-web-api) et [SignalR](xref:signalr/introduction).</span><span class="sxs-lookup"><span data-stu-id="e1e41-116">See also [MVC](xref:mvc/overview), [Web API](xref:tutorials/first-web-api), and [SignalR](xref:signalr/introduction).</span></span>|<span data-ttu-id="e1e41-117">Utilisez [Web Forms](/aspnet/web-forms), [SignalR](/aspnet/signalr), [MVC](/aspnet/mvc), [Web API](/aspnet/web-api/), [WebHooks](/aspnet/webhooks/) ou [Pages Web](/aspnet/web-pages)</span><span class="sxs-lookup"><span data-stu-id="e1e41-117">Use [Web Forms](/aspnet/web-forms), [SignalR](/aspnet/signalr), [MVC](/aspnet/mvc), [Web API](/aspnet/web-api/), [WebHooks](/aspnet/webhooks/), or [Web Pages](/aspnet/web-pages)</span></span>|
|<span data-ttu-id="e1e41-118">Plusieurs versions par machine</span><span class="sxs-lookup"><span data-stu-id="e1e41-118">Multiple versions per machine</span></span>|<span data-ttu-id="e1e41-119">Une seule version par machine</span><span class="sxs-lookup"><span data-stu-id="e1e41-119">One version per machine</span></span>|
|<span data-ttu-id="e1e41-120">Développer avec Visual Studio, [Visual Studio pour Mac](https://www.visualstudio.com/vs/visual-studio-mac/) ou [Visual Studio Code](https://code.visualstudio.com/) en utilisant C# ou F#</span><span class="sxs-lookup"><span data-stu-id="e1e41-120">Develop with Visual Studio, [Visual Studio for Mac](https://www.visualstudio.com/vs/visual-studio-mac/), or [Visual Studio Code](https://code.visualstudio.com/) using C# or F#</span></span>|<span data-ttu-id="e1e41-121">Développer avec Visual Studio en utilisant C#, VB ou F#</span><span class="sxs-lookup"><span data-stu-id="e1e41-121">Develop with Visual Studio using C#, VB, or F#</span></span>|
|<span data-ttu-id="e1e41-122">Performances supérieures à celles d’ASP.NET</span><span class="sxs-lookup"><span data-stu-id="e1e41-122">Higher performance than ASP.NET</span></span>|<span data-ttu-id="e1e41-123">Bonnes performances</span><span class="sxs-lookup"><span data-stu-id="e1e41-123">Good performance</span></span>|
|[<span data-ttu-id="e1e41-124">Choisir le runtime .NET Framework ou .NET Core</span><span class="sxs-lookup"><span data-stu-id="e1e41-124">Choose .NET Framework or .NET Core runtime</span></span>](/dotnet/articles/standard/choosing-core-framework-server)|<span data-ttu-id="e1e41-125">Utiliser le runtime .NET Framework</span><span class="sxs-lookup"><span data-stu-id="e1e41-125">Use .NET Framework runtime</span></span>|

## <a name="aspnet-core-scenarios"></a><span data-ttu-id="e1e41-126">Scénarios ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e1e41-126">ASP.NET Core scenarios</span></span>

* <span data-ttu-id="e1e41-127">Nous vous recommandons d’utiliser les [pages Razor](xref:razor-pages/index) pour créer une interface utilisateur web à partir d’ASP.NET Core 2.</span><span class="sxs-lookup"><span data-stu-id="e1e41-127">[Razor Pages](xref:razor-pages/index) is the recommended approach to create a Web UI as of ASP.NET Core 2.x.</span></span>
* [<span data-ttu-id="e1e41-128">Sites web</span><span class="sxs-lookup"><span data-stu-id="e1e41-128">Websites</span></span>](xref:tutorials/first-mvc-app/index)
* [<span data-ttu-id="e1e41-129">API</span><span class="sxs-lookup"><span data-stu-id="e1e41-129">APIs</span></span>](xref:tutorials/first-web-api)
* [<span data-ttu-id="e1e41-130">Temps réel</span><span class="sxs-lookup"><span data-stu-id="e1e41-130">Real-time</span></span>](xref:signalr/index)

## <a name="aspnet-scenarios"></a><span data-ttu-id="e1e41-131">Scénarios ASP.NET</span><span class="sxs-lookup"><span data-stu-id="e1e41-131">ASP.NET scenarios</span></span>

* [<span data-ttu-id="e1e41-132">Sites web</span><span class="sxs-lookup"><span data-stu-id="e1e41-132">Websites</span></span>](/aspnet/mvc)
* [<span data-ttu-id="e1e41-133">API</span><span class="sxs-lookup"><span data-stu-id="e1e41-133">APIs</span></span>](/aspnet/web-api)
* [<span data-ttu-id="e1e41-134">Temps réel</span><span class="sxs-lookup"><span data-stu-id="e1e41-134">Real-time</span></span>](/aspnet/signalr)

## <a name="resources"></a><span data-ttu-id="e1e41-135">Ressources</span><span class="sxs-lookup"><span data-stu-id="e1e41-135">Resources</span></span>

* [<span data-ttu-id="e1e41-136">Présentation d’ASP.NET</span><span class="sxs-lookup"><span data-stu-id="e1e41-136">Introduction to ASP.NET</span></span>](/aspnet/overview)
* [<span data-ttu-id="e1e41-137">Présentation d’ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e1e41-137">Introduction to ASP.NET Core</span></span>](xref:index)
