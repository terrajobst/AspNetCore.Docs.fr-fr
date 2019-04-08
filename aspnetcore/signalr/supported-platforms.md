---
title: Plateformes prises en charge par ASP.NET Core SignalR
author: bradygaster
description: En savoir plus sur les plateformes prises en charge pour ASP.NET Core SignalR.
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 04/06/2019
uid: signalr/supported-platforms
ms.openlocfilehash: fefaaf97de3f1fabf8f3154bf5b4ccb37195ccff
ms.sourcegitcommit: 6bde1fdf686326c080a7518a6725e56e56d8886e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2019
ms.locfileid: "59068220"
---
# <a name="aspnet-core-signalr-supported-platforms"></a><span data-ttu-id="acee6-103">Plateformes prises en charge par ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="acee6-103">ASP.NET Core SignalR supported platforms</span></span>

## <a name="server-system-requirements"></a><span data-ttu-id="acee6-104">Configuration requise du système serveur</span><span class="sxs-lookup"><span data-stu-id="acee6-104">Server system requirements</span></span>

<span data-ttu-id="acee6-105">SignalR pour ASP.NET Core prend en charge n’importe quelle plateforme de serveur ASP.NET Core prend en charge.</span><span class="sxs-lookup"><span data-stu-id="acee6-105">SignalR for ASP.NET Core supports any server platform that ASP.NET Core supports.</span></span>

## <a name="javascript-client"></a><span data-ttu-id="acee6-106">Client JavaScript</span><span class="sxs-lookup"><span data-stu-id="acee6-106">JavaScript client</span></span>

<span data-ttu-id="acee6-107">Le [client JavaScript](https://www.npmjs.com/package/@aspnet/signalr) s’exécute sur NodeJS 8 et versions ultérieures et les navigateurs suivants :</span><span class="sxs-lookup"><span data-stu-id="acee6-107">The [JavaScript client](https://www.npmjs.com/package/@aspnet/signalr) runs on NodeJS 8 and later versions and the following browsers:</span></span>

| <span data-ttu-id="acee6-108">Visiteur</span><span class="sxs-lookup"><span data-stu-id="acee6-108">Browser</span></span>                         | <span data-ttu-id="acee6-109">Version</span><span class="sxs-lookup"><span data-stu-id="acee6-109">Version</span></span> |
| ------------------------------- | ------- |
| <span data-ttu-id="acee6-110">Microsoft Edge</span><span class="sxs-lookup"><span data-stu-id="acee6-110">Microsoft Edge</span></span>                  | <span data-ttu-id="acee6-111">actuelle</span><span class="sxs-lookup"><span data-stu-id="acee6-111">current</span></span> |
| <span data-ttu-id="acee6-112">Mozilla Firefox</span><span class="sxs-lookup"><span data-stu-id="acee6-112">Mozilla Firefox</span></span>                 | <span data-ttu-id="acee6-113">actuelle</span><span class="sxs-lookup"><span data-stu-id="acee6-113">current</span></span> |
| <span data-ttu-id="acee6-114">Google Chrome ; inclut Android</span><span class="sxs-lookup"><span data-stu-id="acee6-114">Google Chrome; includes Android</span></span> | <span data-ttu-id="acee6-115">actuelle</span><span class="sxs-lookup"><span data-stu-id="acee6-115">current</span></span> |
| <span data-ttu-id="acee6-116">Safari ; inclut iOS</span><span class="sxs-lookup"><span data-stu-id="acee6-116">Safari; includes iOS</span></span>            | <span data-ttu-id="acee6-117">actuelle</span><span class="sxs-lookup"><span data-stu-id="acee6-117">current</span></span> |
| <span data-ttu-id="acee6-118">Microsoft Internet Explorer</span><span class="sxs-lookup"><span data-stu-id="acee6-118">Microsoft Internet Explorer</span></span>     | <span data-ttu-id="acee6-119">11</span><span class="sxs-lookup"><span data-stu-id="acee6-119">11</span></span>      |
 
## <a name="net-client"></a><span data-ttu-id="acee6-120">Client .NET</span><span class="sxs-lookup"><span data-stu-id="acee6-120">.NET client</span></span>

<span data-ttu-id="acee6-121">Le [client .NET](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR/) s’exécute sur n’importe quelle plateforme prise en charge par ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="acee6-121">The [.NET client](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR/) runs on any platform supported by ASP.NET Core.</span></span> <span data-ttu-id="acee6-122">Par exemple, [les développeurs Xamarin peuvent utiliser SignalR](https://github.com/aspnet/Announcements/issues/305) pour la création d’applications Android à l’aide de Xamarin.Android 8.4.0.1 et versions ultérieures et les applications iOS à l’aide de Xamarin.iOS 11.14.0.4 et versions ultérieures.</span><span class="sxs-lookup"><span data-stu-id="acee6-122">For example, [Xamarin developers can use SignalR](https://github.com/aspnet/Announcements/issues/305) for building Android apps using Xamarin.Android 8.4.0.1 and later and iOS apps using Xamarin.iOS 11.14.0.4 and later.</span></span>

<span data-ttu-id="acee6-123">Si le serveur exécute IIS, le transport WebSocket requiert IIS 8.0 ou version ultérieure sur Windows Server 2012 ou version ultérieure.</span><span class="sxs-lookup"><span data-stu-id="acee6-123">If the server runs IIS, the WebSockets transport requires IIS 8.0 or later on Windows Server 2012 or later.</span></span> <span data-ttu-id="acee6-124">Autres transports sont pris en charge sur toutes les plateformes.</span><span class="sxs-lookup"><span data-stu-id="acee6-124">Other transports are supported on all platforms.</span></span>

## <a name="java-client"></a><span data-ttu-id="acee6-125">Client Java</span><span class="sxs-lookup"><span data-stu-id="acee6-125">Java client</span></span>

<span data-ttu-id="acee6-126">Le [client Java](https://search.maven.org/artifact/com.microsoft.aspnet/signalr) prend en charge de Java 8 et versions ultérieures.</span><span class="sxs-lookup"><span data-stu-id="acee6-126">The [Java client](https://search.maven.org/artifact/com.microsoft.aspnet/signalr) supports Java 8 and later versions.</span></span>

## <a name="unsupported-clients"></a><span data-ttu-id="acee6-127">Clients non pris en charge</span><span class="sxs-lookup"><span data-stu-id="acee6-127">Unsupported clients</span></span>

<span data-ttu-id="acee6-128">Les clients suivants sont disponibles mais sont expérimentales ou non officiels.</span><span class="sxs-lookup"><span data-stu-id="acee6-128">The following clients are available but are experimental or unofficial.</span></span> <span data-ttu-id="acee6-129">Ils ne sont pas actuellement pris en charge et ne peuvent jamais être.</span><span class="sxs-lookup"><span data-stu-id="acee6-129">They aren't currently supported and may never be.</span></span>

* [<span data-ttu-id="acee6-130">Client C++</span><span class="sxs-lookup"><span data-stu-id="acee6-130">C++ client</span></span>](https://github.com/aspnet/SignalR/tree/master/clients/cpp)

* [<span data-ttu-id="acee6-131">Client SWIFT</span><span class="sxs-lookup"><span data-stu-id="acee6-131">Swift client</span></span>](https://github.com/moozzyk/SignalR-Client-Swift)
