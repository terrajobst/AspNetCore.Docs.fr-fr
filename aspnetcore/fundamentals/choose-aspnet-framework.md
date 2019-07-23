---
title: Choisir entre ASP.NET 4.x et ASP.NET Core
author: rick-anderson
description: Explique ASP.NET Core par rapport à ASP.NET 4.x, et comment choisir entre les deux.
ms.author: riande
ms.custom: mvc, seodec18
ms.date: 07/15/2019
uid: fundamentals/choose-between-aspnet-and-aspnetcore
ms.openlocfilehash: 9e093e83a1f6367cbb244076a8351644244f9874
ms.sourcegitcommit: 7e00e8236ca4eabf058f07020a5a3882daf7564f
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/16/2019
ms.locfileid: "68239206"
---
# <a name="choose-between-aspnet-4x-and-aspnet-core"></a><span data-ttu-id="783f9-103">Choisir entre ASP.NET 4.x et ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="783f9-103">Choose between ASP.NET 4.x and ASP.NET Core</span></span>

<span data-ttu-id="783f9-104">ASP.NET Core est une refonte d’ASP.NET 4.x.</span><span class="sxs-lookup"><span data-stu-id="783f9-104">ASP.NET Core is a redesign of ASP.NET 4.x.</span></span> <span data-ttu-id="783f9-105">Cet article liste les différences existant entre eux.</span><span class="sxs-lookup"><span data-stu-id="783f9-105">This article lists the differences between them.</span></span>

## <a name="aspnet-core"></a><span data-ttu-id="783f9-106">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="783f9-106">ASP.NET Core</span></span>

<span data-ttu-id="783f9-107">ASP.NET Core est un framework open source multiplateforme qui permet de créer des applications web cloud modernes sur Windows, macOS et Linux.</span><span class="sxs-lookup"><span data-stu-id="783f9-107">ASP.NET Core is an open-source, cross-platform framework for building modern, cloud-based web apps on Windows, macOS, or Linux.</span></span>

[!INCLUDE[](~/includes/benefits.md)]

## <a name="aspnet-4x"></a><span data-ttu-id="783f9-108">ASP.NET 4.x</span><span class="sxs-lookup"><span data-stu-id="783f9-108">ASP.NET 4.x</span></span>

<span data-ttu-id="783f9-109">ASP.NET 4.x est un framework abouti qui offre les services nécessaires pour créer sur Windows des applications web, basées sur serveur et destinées à l’entreprise.</span><span class="sxs-lookup"><span data-stu-id="783f9-109">ASP.NET 4.x is a mature framework that provides the services needed to build enterprise-grade, server-based web apps on Windows.</span></span>

## <a name="framework-selection"></a><span data-ttu-id="783f9-110">Sélection du Framework</span><span class="sxs-lookup"><span data-stu-id="783f9-110">Framework selection</span></span>

<span data-ttu-id="783f9-111">Le tableau suivant compare ASP.NET Core à ASP.NET 4.x.</span><span class="sxs-lookup"><span data-stu-id="783f9-111">The following table compares ASP.NET Core to ASP.NET 4.x.</span></span>

| <span data-ttu-id="783f9-112">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="783f9-112">ASP.NET Core</span></span> | <span data-ttu-id="783f9-113">ASP.NET 4.x</span><span class="sxs-lookup"><span data-stu-id="783f9-113">ASP.NET 4.x</span></span> |
|---|---|
|<span data-ttu-id="783f9-114">Générer pour Windows, macOS ou Linux</span><span class="sxs-lookup"><span data-stu-id="783f9-114">Build for Windows, macOS, or Linux</span></span>|<span data-ttu-id="783f9-115">Générer pour Windows</span><span class="sxs-lookup"><span data-stu-id="783f9-115">Build for Windows</span></span>|
|<span data-ttu-id="783f9-116">Nous vous recommandons d’utiliser les [pages Razor](xref:razor-pages/index) pour créer une interface utilisateur web à partir d’ASP.NET Core 2.</span><span class="sxs-lookup"><span data-stu-id="783f9-116">[Razor Pages](xref:razor-pages/index) is the recommended approach to create a Web UI as of ASP.NET Core 2.x.</span></span> <span data-ttu-id="783f9-117">Voir aussi [MVC](xref:mvc/overview), [API web](xref:tutorials/first-web-api) et [SignalR](xref:signalr/introduction).</span><span class="sxs-lookup"><span data-stu-id="783f9-117">See also [MVC](xref:mvc/overview), [Web API](xref:tutorials/first-web-api), and [SignalR](xref:signalr/introduction).</span></span>|<span data-ttu-id="783f9-118">Utilisez [Web Forms](/aspnet/web-forms), [SignalR](/aspnet/signalr), [MVC](/aspnet/mvc), [Web API](/aspnet/web-api/), [WebHooks](/aspnet/webhooks/) ou [Pages Web](/aspnet/web-pages)</span><span class="sxs-lookup"><span data-stu-id="783f9-118">Use [Web Forms](/aspnet/web-forms), [SignalR](/aspnet/signalr), [MVC](/aspnet/mvc), [Web API](/aspnet/web-api/), [WebHooks](/aspnet/webhooks/), or [Web Pages](/aspnet/web-pages)</span></span>|
|<span data-ttu-id="783f9-119">Plusieurs versions par machine</span><span class="sxs-lookup"><span data-stu-id="783f9-119">Multiple versions per machine</span></span>|<span data-ttu-id="783f9-120">Une seule version par machine</span><span class="sxs-lookup"><span data-stu-id="783f9-120">One version per machine</span></span>|
|<span data-ttu-id="783f9-121">Développer avec [Visual Studio](https://visualstudio.microsoft.com/vs/), [Visual Studio pour Mac](https://visualstudio.microsoft.com/vs/mac/) ou [Visual Studio Code](https://code.visualstudio.com/) en utilisant C# ou F#</span><span class="sxs-lookup"><span data-stu-id="783f9-121">Develop with [Visual Studio](https://visualstudio.microsoft.com/vs/), [Visual Studio for Mac](https://visualstudio.microsoft.com/vs/mac/), or [Visual Studio Code](https://code.visualstudio.com/) using C# or F#</span></span>|<span data-ttu-id="783f9-122">Développer avec [Visual Studio](https://visualstudio.microsoft.com/vs/) en utilisant C#, VB ou F#</span><span class="sxs-lookup"><span data-stu-id="783f9-122">Develop with [Visual Studio](https://visualstudio.microsoft.com/vs/) using C#, VB, or F#</span></span>|
|<span data-ttu-id="783f9-123">Performances supérieures à celles d’ASP.NET 4.x</span><span class="sxs-lookup"><span data-stu-id="783f9-123">Higher performance than ASP.NET 4.x</span></span>|<span data-ttu-id="783f9-124">Bonnes performances</span><span class="sxs-lookup"><span data-stu-id="783f9-124">Good performance</span></span>|
|[<span data-ttu-id="783f9-125">Utiliser le runtime .NET Core</span><span class="sxs-lookup"><span data-stu-id="783f9-125">Use .NET Core runtime</span></span>](/dotnet/standard/choosing-core-framework-server)|<span data-ttu-id="783f9-126">Utiliser le runtime .NET Framework</span><span class="sxs-lookup"><span data-stu-id="783f9-126">Use .NET Framework runtime</span></span>|

<span data-ttu-id="783f9-127">Consultez [ASP.NET Core ciblant le .NET Framework](xref:index#target-framework) pour plus d’informations sur la prise en charge d’ASP.NET Core 2.x sur le .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="783f9-127">See [ASP.NET Core targeting .NET Framework](xref:index#target-framework) for information on ASP.NET Core 2.x support on .NET Framework.</span></span>

## <a name="aspnet-core-scenarios"></a><span data-ttu-id="783f9-128">Scénarios ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="783f9-128">ASP.NET Core scenarios</span></span>

* [<span data-ttu-id="783f9-129">Sites web</span><span class="sxs-lookup"><span data-stu-id="783f9-129">Websites</span></span>](xref:tutorials/first-mvc-app/index)
* [<span data-ttu-id="783f9-130">API</span><span class="sxs-lookup"><span data-stu-id="783f9-130">APIs</span></span>](xref:tutorials/first-web-api)
* [<span data-ttu-id="783f9-131">Temps réel</span><span class="sxs-lookup"><span data-stu-id="783f9-131">Real-time</span></span>](xref:signalr/index)
* [<span data-ttu-id="783f9-132">Déployer une application ASP.NET Core sur Azure</span><span class="sxs-lookup"><span data-stu-id="783f9-132">Deploy an ASP.NET Core app to Azure</span></span>](/azure/app-service/app-service-web-get-started-dotnet)

## <a name="aspnet-4x-scenarios"></a><span data-ttu-id="783f9-133">Scénarios ASP.NET 4.x</span><span class="sxs-lookup"><span data-stu-id="783f9-133">ASP.NET 4.x scenarios</span></span>

* [<span data-ttu-id="783f9-134">Sites web</span><span class="sxs-lookup"><span data-stu-id="783f9-134">Websites</span></span>](/aspnet/mvc)
* [<span data-ttu-id="783f9-135">API</span><span class="sxs-lookup"><span data-stu-id="783f9-135">APIs</span></span>](/aspnet/web-api)
* [<span data-ttu-id="783f9-136">Temps réel</span><span class="sxs-lookup"><span data-stu-id="783f9-136">Real-time</span></span>](/aspnet/signalr)
* [<span data-ttu-id="783f9-137">Créer une application web ASP.NET 4.x dans Azure</span><span class="sxs-lookup"><span data-stu-id="783f9-137">Create an ASP.NET 4.x web app in Azure</span></span>](/azure/app-service/app-service-web-get-started-dotnet-framework)

## <a name="additional-resources"></a><span data-ttu-id="783f9-138">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="783f9-138">Additional resources</span></span>

* [<span data-ttu-id="783f9-139">Présentation d’ASP.NET</span><span class="sxs-lookup"><span data-stu-id="783f9-139">Introduction to ASP.NET</span></span>](/aspnet/overview)
* [<span data-ttu-id="783f9-140">Présentation d’ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="783f9-140">Introduction to ASP.NET Core</span></span>](xref:index)
* <xref:host-and-deploy/azure-apps/index>
