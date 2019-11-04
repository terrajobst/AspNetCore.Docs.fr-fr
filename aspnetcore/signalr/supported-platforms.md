---
title: Plateformes prises en charge par ASP.NET Core Signalr
author: bradygaster
description: En savoir plus sur les plateformes prises en charge pour ASP.NET Core Signalr.
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 11/01/2019
uid: signalr/supported-platforms
ms.openlocfilehash: 1be7a307710e6e522c0088fd1ca01da11a13eda1
ms.sourcegitcommit: 77c8be22d5e88dd710f42c739748869f198865dd
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/01/2019
ms.locfileid: "73426979"
---
# <a name="aspnet-core-signalr-supported-platforms"></a><span data-ttu-id="65f43-103">Plateformes prises en charge par ASP.NET Core Signalr</span><span class="sxs-lookup"><span data-stu-id="65f43-103">ASP.NET Core SignalR supported platforms</span></span>

## <a name="server-system-requirements"></a><span data-ttu-id="65f43-104">Configuration système requise pour le serveur</span><span class="sxs-lookup"><span data-stu-id="65f43-104">Server system requirements</span></span>

<span data-ttu-id="65f43-105">Signalr pour ASP.NET Core prend en charge toute plateforme de serveur prise en charge par ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="65f43-105">SignalR for ASP.NET Core supports any server platform that ASP.NET Core supports.</span></span>

## <a name="javascript-client"></a><span data-ttu-id="65f43-106">Client JavaScript</span><span class="sxs-lookup"><span data-stu-id="65f43-106">JavaScript client</span></span>

<span data-ttu-id="65f43-107">Le [client JavaScript](https://www.npmjs.com/package/@aspnet/signalr) s’exécute sur NodeJS 8 et versions ultérieures, ainsi que sur les navigateurs suivants :</span><span class="sxs-lookup"><span data-stu-id="65f43-107">The [JavaScript client](https://www.npmjs.com/package/@aspnet/signalr) runs on NodeJS 8 and later versions and the following browsers:</span></span>

| <span data-ttu-id="65f43-108">Visiteur</span><span class="sxs-lookup"><span data-stu-id="65f43-108">Browser</span></span>                         | <span data-ttu-id="65f43-109">Version</span><span class="sxs-lookup"><span data-stu-id="65f43-109">Version</span></span>         |
| ------------------------------- | --------------- |
| <span data-ttu-id="65f43-110">Microsoft Edge</span><span class="sxs-lookup"><span data-stu-id="65f43-110">Microsoft Edge</span></span>                  | <span data-ttu-id="65f43-111">&dagger; actuel</span><span class="sxs-lookup"><span data-stu-id="65f43-111">Current&dagger;</span></span> |
| <span data-ttu-id="65f43-112">Mozilla Firefox</span><span class="sxs-lookup"><span data-stu-id="65f43-112">Mozilla Firefox</span></span>                 | <span data-ttu-id="65f43-113">&dagger; actuel</span><span class="sxs-lookup"><span data-stu-id="65f43-113">Current&dagger;</span></span> |
| <span data-ttu-id="65f43-114">Google Chrome ; comprend Android</span><span class="sxs-lookup"><span data-stu-id="65f43-114">Google Chrome; includes Android</span></span> | <span data-ttu-id="65f43-115">&dagger; actuel</span><span class="sxs-lookup"><span data-stu-id="65f43-115">Current&dagger;</span></span> |
| <span data-ttu-id="65f43-116">Safari comprend iOS</span><span class="sxs-lookup"><span data-stu-id="65f43-116">Safari; includes iOS</span></span>            | <span data-ttu-id="65f43-117">&dagger; actuel</span><span class="sxs-lookup"><span data-stu-id="65f43-117">Current&dagger;</span></span> |
| <span data-ttu-id="65f43-118">Microsoft Internet Explorer</span><span class="sxs-lookup"><span data-stu-id="65f43-118">Microsoft Internet Explorer</span></span>     | <span data-ttu-id="65f43-119">11</span><span class="sxs-lookup"><span data-stu-id="65f43-119">11</span></span>              |

<span data-ttu-id="65f43-120">&dagger;*en cours* fait référence à la dernière version du navigateur.</span><span class="sxs-lookup"><span data-stu-id="65f43-120">&dagger;*Current* refers to the latest version of the browser.</span></span>

## <a name="net-client"></a><span data-ttu-id="65f43-121">Client .NET</span><span class="sxs-lookup"><span data-stu-id="65f43-121">.NET client</span></span>

<span data-ttu-id="65f43-122">Le [client .net](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR/) s’exécute sur n’importe quelle plateforme prise en charge par ASP.net core.</span><span class="sxs-lookup"><span data-stu-id="65f43-122">The [.NET client](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR/) runs on any platform supported by ASP.NET Core.</span></span> <span data-ttu-id="65f43-123">Par exemple, [les développeurs Xamarin peuvent utiliser signalr](https://github.com/aspnet/Announcements/issues/305) pour créer des applications Android à l’aide de Xamarin. Android 8.4.0.1 et versions ultérieures et des applications iOS à l’aide de Xamarin. iOS 11.14.0.4 et versions ultérieures.</span><span class="sxs-lookup"><span data-stu-id="65f43-123">For example, [Xamarin developers can use SignalR](https://github.com/aspnet/Announcements/issues/305) for building Android apps using Xamarin.Android 8.4.0.1 and later and iOS apps using Xamarin.iOS 11.14.0.4 and later.</span></span>

<span data-ttu-id="65f43-124">Si le serveur exécute IIS, le transport WebSockets requiert IIS 8,0 ou une version ultérieure sur Windows Server 2012 ou version ultérieure.</span><span class="sxs-lookup"><span data-stu-id="65f43-124">If the server runs IIS, the WebSockets transport requires IIS 8.0 or later on Windows Server 2012 or later.</span></span> <span data-ttu-id="65f43-125">D’autres transports sont pris en charge sur toutes les plateformes.</span><span class="sxs-lookup"><span data-stu-id="65f43-125">Other transports are supported on all platforms.</span></span>

## <a name="java-client"></a><span data-ttu-id="65f43-126">Client Java</span><span class="sxs-lookup"><span data-stu-id="65f43-126">Java client</span></span>

<span data-ttu-id="65f43-127">Le [client Java](https://search.maven.org/artifact/com.microsoft.aspnet/signalr) prend en charge Java 8 et versions ultérieures.</span><span class="sxs-lookup"><span data-stu-id="65f43-127">The [Java client](https://search.maven.org/artifact/com.microsoft.aspnet/signalr) supports Java 8 and later versions.</span></span>

## <a name="unsupported-clients"></a><span data-ttu-id="65f43-128">Clients non pris en charge</span><span class="sxs-lookup"><span data-stu-id="65f43-128">Unsupported clients</span></span>

<span data-ttu-id="65f43-129">Les clients suivants sont disponibles, mais sont expérimentaux ou non officiels.</span><span class="sxs-lookup"><span data-stu-id="65f43-129">The following clients are available but are experimental or unofficial.</span></span> <span data-ttu-id="65f43-130">Ils ne sont actuellement pas pris en charge et peuvent ne jamais être.</span><span class="sxs-lookup"><span data-stu-id="65f43-130">They aren't currently supported and may never be.</span></span>

* [<span data-ttu-id="65f43-131">C++client</span><span class="sxs-lookup"><span data-stu-id="65f43-131">C++ client</span></span>](https://github.com/aspnet/SignalR/tree/master/clients/cpp)

* [<span data-ttu-id="65f43-132">Client SWIFT</span><span class="sxs-lookup"><span data-stu-id="65f43-132">Swift client</span></span>](https://github.com/moozzyk/SignalR-Client-Swift)
