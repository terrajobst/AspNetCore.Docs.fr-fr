---
title: Choisir entre ASP.NET 4.x et ASP.NET Core
author: rick-anderson
description: Explique ASP.NET Core par rapport à ASP.NET 4.x, et comment choisir entre les deux.
ms.author: riande
ms.custom: seodec18
ms.date: 09/11/2018
uid: fundamentals/choose-between-aspnet-and-aspnetcore
ms.openlocfilehash: eb216bdac7dd029c3d985f2edd9e70eb91f42883
ms.sourcegitcommit: a16352c1c88a71770ab3922200a8cd148fb278a6
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/13/2018
ms.locfileid: "53335340"
---
# <a name="choose-between-aspnet-4x-and-aspnet-core"></a><span data-ttu-id="19e6e-103">Choisir entre ASP.NET 4.x et ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="19e6e-103">Choose between ASP.NET 4.x and ASP.NET Core</span></span>

<span data-ttu-id="19e6e-104">ASP.NET Core est une refonte d’ASP.NET 4.x.</span><span class="sxs-lookup"><span data-stu-id="19e6e-104">ASP.NET Core is a redesign of ASP.NET 4.x.</span></span> <span data-ttu-id="19e6e-105">Cet article liste les différences existant entre eux.</span><span class="sxs-lookup"><span data-stu-id="19e6e-105">This article lists the differences between them.</span></span>

## <a name="aspnet-core"></a><span data-ttu-id="19e6e-106">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="19e6e-106">ASP.NET Core</span></span>

<span data-ttu-id="19e6e-107">ASP.NET Core est un framework open source multiplateforme qui permet de créer des applications web cloud modernes sur Windows, macOS et Linux.</span><span class="sxs-lookup"><span data-stu-id="19e6e-107">ASP.NET Core is an open-source, cross-platform framework for building modern, cloud-based web apps on Windows, macOS, or Linux.</span></span>

[!INCLUDE[](~/includes/benefits.md)]

## <a name="aspnet-4x"></a><span data-ttu-id="19e6e-108">ASP.NET 4.x</span><span class="sxs-lookup"><span data-stu-id="19e6e-108">ASP.NET 4.x</span></span>

<span data-ttu-id="19e6e-109">ASP.NET 4.x est un framework abouti qui offre les services nécessaires pour créer sur Windows des applications web, basées sur serveur et destinées à l’entreprise.</span><span class="sxs-lookup"><span data-stu-id="19e6e-109">ASP.NET 4.x is a mature framework that provides the services needed to build enterprise-grade, server-based web apps on Windows.</span></span>

## <a name="framework-selection"></a><span data-ttu-id="19e6e-110">Sélection du Framework</span><span class="sxs-lookup"><span data-stu-id="19e6e-110">Framework selection</span></span>

<span data-ttu-id="19e6e-111">Le tableau suivant compare ASP.NET Core à ASP.NET 4.x.</span><span class="sxs-lookup"><span data-stu-id="19e6e-111">The following table compares ASP.NET Core to ASP.NET 4.x.</span></span>

| <span data-ttu-id="19e6e-112">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="19e6e-112">ASP.NET Core</span></span> | <span data-ttu-id="19e6e-113">ASP.NET 4.x</span><span class="sxs-lookup"><span data-stu-id="19e6e-113">ASP.NET 4.x</span></span> |
|---|---|
|<span data-ttu-id="19e6e-114">Générer pour Windows, macOS ou Linux</span><span class="sxs-lookup"><span data-stu-id="19e6e-114">Build for Windows, macOS, or Linux</span></span>|<span data-ttu-id="19e6e-115">Générer pour Windows</span><span class="sxs-lookup"><span data-stu-id="19e6e-115">Build for Windows</span></span>|
|<span data-ttu-id="19e6e-116">Nous vous recommandons d’utiliser les [pages Razor](xref:razor-pages/index) pour créer une interface utilisateur web à partir d’ASP.NET Core 2.</span><span class="sxs-lookup"><span data-stu-id="19e6e-116">[Razor Pages](xref:razor-pages/index) is the recommended approach to create a Web UI as of ASP.NET Core 2.x.</span></span> <span data-ttu-id="19e6e-117">Voir aussi [MVC](xref:mvc/overview), [API web](xref:tutorials/first-web-api) et [SignalR](xref:signalr/introduction).</span><span class="sxs-lookup"><span data-stu-id="19e6e-117">See also [MVC](xref:mvc/overview), [Web API](xref:tutorials/first-web-api), and [SignalR](xref:signalr/introduction).</span></span>|<span data-ttu-id="19e6e-118">Utilisez [Web Forms](/aspnet/web-forms), [SignalR](/aspnet/signalr), [MVC](/aspnet/mvc), [Web API](/aspnet/web-api/), [WebHooks](/aspnet/webhooks/) ou [Pages Web](/aspnet/web-pages)</span><span class="sxs-lookup"><span data-stu-id="19e6e-118">Use [Web Forms](/aspnet/web-forms), [SignalR](/aspnet/signalr), [MVC](/aspnet/mvc), [Web API](/aspnet/web-api/), [WebHooks](/aspnet/webhooks/), or [Web Pages](/aspnet/web-pages)</span></span>|
|<span data-ttu-id="19e6e-119">Plusieurs versions par machine</span><span class="sxs-lookup"><span data-stu-id="19e6e-119">Multiple versions per machine</span></span>|<span data-ttu-id="19e6e-120">Une seule version par machine</span><span class="sxs-lookup"><span data-stu-id="19e6e-120">One version per machine</span></span>|
|<span data-ttu-id="19e6e-121">Développer avec Visual Studio, [Visual Studio pour Mac](https://www.visualstudio.com/vs/visual-studio-mac/) ou [Visual Studio Code](https://code.visualstudio.com/) en utilisant C# ou F#</span><span class="sxs-lookup"><span data-stu-id="19e6e-121">Develop with Visual Studio, [Visual Studio for Mac](https://www.visualstudio.com/vs/visual-studio-mac/), or [Visual Studio Code](https://code.visualstudio.com/) using C# or F#</span></span>|<span data-ttu-id="19e6e-122">Développer avec Visual Studio en utilisant C#, VB ou F#</span><span class="sxs-lookup"><span data-stu-id="19e6e-122">Develop with Visual Studio using C#, VB, or F#</span></span>|
|<span data-ttu-id="19e6e-123">Performances supérieures à celles d’ASP.NET 4.x</span><span class="sxs-lookup"><span data-stu-id="19e6e-123">Higher performance than ASP.NET 4.x</span></span>|<span data-ttu-id="19e6e-124">Bonnes performances</span><span class="sxs-lookup"><span data-stu-id="19e6e-124">Good performance</span></span>|
|[<span data-ttu-id="19e6e-125">Choisir le runtime .NET Framework ou .NET Core</span><span class="sxs-lookup"><span data-stu-id="19e6e-125">Choose .NET Framework or .NET Core runtime</span></span>](/dotnet/standard/choosing-core-framework-server)|<span data-ttu-id="19e6e-126">Utiliser le runtime .NET Framework</span><span class="sxs-lookup"><span data-stu-id="19e6e-126">Use .NET Framework runtime</span></span>|

<span data-ttu-id="19e6e-127">Consultez [ASP.NET Core ciblant le .NET Framework](xref:index#target-framework) pour plus d’informations sur la prise en charge d’ASP.NET Core 2.x sur le .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="19e6e-127">See [ASP.NET Core targeting .NET Framework](xref:index#target-framework) for information on ASP.NET Core 2.x support on .NET Framework.</span></span>

## <a name="aspnet-core-scenarios"></a><span data-ttu-id="19e6e-128">Scénarios ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="19e6e-128">ASP.NET Core scenarios</span></span>

* <span data-ttu-id="19e6e-129">Nous vous recommandons d’utiliser les [pages Razor](xref:razor-pages/index) pour créer une interface utilisateur web à partir d’ASP.NET Core 2.</span><span class="sxs-lookup"><span data-stu-id="19e6e-129">[Razor Pages](xref:razor-pages/index) is the recommended approach to create a Web UI as of ASP.NET Core 2.x.</span></span>
* [<span data-ttu-id="19e6e-130">Sites web</span><span class="sxs-lookup"><span data-stu-id="19e6e-130">Websites</span></span>](xref:tutorials/first-mvc-app/index)
* [<span data-ttu-id="19e6e-131">API</span><span class="sxs-lookup"><span data-stu-id="19e6e-131">APIs</span></span>](xref:tutorials/first-web-api)
* [<span data-ttu-id="19e6e-132">Temps réel</span><span class="sxs-lookup"><span data-stu-id="19e6e-132">Real-time</span></span>](xref:signalr/index)
* [<span data-ttu-id="19e6e-133">Déployer une application ASP.NET Core sur Azure</span><span class="sxs-lookup"><span data-stu-id="19e6e-133">Deploy an ASP.NET Core app to Azure</span></span>](/azure/app-service/app-service-web-get-started-dotnet)

## <a name="aspnet-4x-scenarios"></a><span data-ttu-id="19e6e-134">Scénarios ASP.NET 4.x</span><span class="sxs-lookup"><span data-stu-id="19e6e-134">ASP.NET 4.x scenarios</span></span>

* [<span data-ttu-id="19e6e-135">Sites web</span><span class="sxs-lookup"><span data-stu-id="19e6e-135">Websites</span></span>](/aspnet/mvc)
* [<span data-ttu-id="19e6e-136">API</span><span class="sxs-lookup"><span data-stu-id="19e6e-136">APIs</span></span>](/aspnet/web-api)
* [<span data-ttu-id="19e6e-137">Temps réel</span><span class="sxs-lookup"><span data-stu-id="19e6e-137">Real-time</span></span>](/aspnet/signalr)
* [<span data-ttu-id="19e6e-138">Créer une application web ASP.NET 4.x dans Azure</span><span class="sxs-lookup"><span data-stu-id="19e6e-138">Create an ASP.NET 4.x web app in Azure</span></span>](/azure/app-service/app-service-web-get-started-dotnet-framework)

## <a name="additional-resources"></a><span data-ttu-id="19e6e-139">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="19e6e-139">Additional resources</span></span>

* [<span data-ttu-id="19e6e-140">Présentation d’ASP.NET</span><span class="sxs-lookup"><span data-stu-id="19e6e-140">Introduction to ASP.NET</span></span>](/aspnet/overview)
* [<span data-ttu-id="19e6e-141">Présentation d’ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="19e6e-141">Introduction to ASP.NET Core</span></span>](xref:index)
* <xref:host-and-deploy/azure-apps/index>
