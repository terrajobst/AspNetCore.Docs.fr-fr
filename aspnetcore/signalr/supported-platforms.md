---
title: Plateformes prises en charge par ASP.NET Core SignalR
author: bradygaster
description: En savoir plus sur les plateformes prises en charge pour ASP.NET Core SignalR.
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 11/21/2019
no-loc:
- SignalR
uid: signalr/supported-platforms
ms.openlocfilehash: 9b9cf1d57d61c333c485f23b7ab952c66814d2aa
ms.sourcegitcommit: 3e503ef510008e77be6dd82ee79213c9f7b97607
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/22/2019
ms.locfileid: "74317463"
---
# <a name="aspnet-core-opno-locsignalr-supported-platforms"></a><span data-ttu-id="c4fc9-103">Plateformes prises en charge par ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="c4fc9-103">ASP.NET Core SignalR supported platforms</span></span>

## <a name="server-system-requirements"></a><span data-ttu-id="c4fc9-104">Configuration système requise pour le serveur</span><span class="sxs-lookup"><span data-stu-id="c4fc9-104">Server system requirements</span></span>

SignalR<span data-ttu-id="c4fc9-105"> pour ASP.NET Core prend en charge toutes les plateformes de serveur prises en charge par ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="c4fc9-105"> for ASP.NET Core supports any server platform that ASP.NET Core supports.</span></span>

## <a name="javascript-client"></a><span data-ttu-id="c4fc9-106">Client JavaScript</span><span class="sxs-lookup"><span data-stu-id="c4fc9-106">JavaScript client</span></span>

<span data-ttu-id="c4fc9-107">Le [client JavaScript](xref:signalr/javascript-client) s’exécute sur NodeJS 8 et versions ultérieures, ainsi que sur les navigateurs suivants :</span><span class="sxs-lookup"><span data-stu-id="c4fc9-107">The [JavaScript client](xref:signalr/javascript-client) runs on NodeJS 8 and later versions and the following browsers:</span></span>

| <span data-ttu-id="c4fc9-108">Navigateur</span><span class="sxs-lookup"><span data-stu-id="c4fc9-108">Browser</span></span>                         | <span data-ttu-id="c4fc9-109">Version</span><span class="sxs-lookup"><span data-stu-id="c4fc9-109">Version</span></span>         |
| ------------------------------- | --------------- |
| <span data-ttu-id="c4fc9-110">Microsoft Edge</span><span class="sxs-lookup"><span data-stu-id="c4fc9-110">Microsoft Edge</span></span>                  | <span data-ttu-id="c4fc9-111">&dagger; actuel</span><span class="sxs-lookup"><span data-stu-id="c4fc9-111">Current&dagger;</span></span> |
| <span data-ttu-id="c4fc9-112">Mozilla Firefox</span><span class="sxs-lookup"><span data-stu-id="c4fc9-112">Mozilla Firefox</span></span>                 | <span data-ttu-id="c4fc9-113">&dagger; actuel</span><span class="sxs-lookup"><span data-stu-id="c4fc9-113">Current&dagger;</span></span> |
| <span data-ttu-id="c4fc9-114">Google Chrome ; comprend Android</span><span class="sxs-lookup"><span data-stu-id="c4fc9-114">Google Chrome; includes Android</span></span> | <span data-ttu-id="c4fc9-115">&dagger; actuel</span><span class="sxs-lookup"><span data-stu-id="c4fc9-115">Current&dagger;</span></span> |
| <span data-ttu-id="c4fc9-116">Safari comprend iOS</span><span class="sxs-lookup"><span data-stu-id="c4fc9-116">Safari; includes iOS</span></span>            | <span data-ttu-id="c4fc9-117">&dagger; actuel</span><span class="sxs-lookup"><span data-stu-id="c4fc9-117">Current&dagger;</span></span> |
| <span data-ttu-id="c4fc9-118">Microsoft Internet Explorer</span><span class="sxs-lookup"><span data-stu-id="c4fc9-118">Microsoft Internet Explorer</span></span>     | <span data-ttu-id="c4fc9-119">11</span><span class="sxs-lookup"><span data-stu-id="c4fc9-119">11</span></span>              |

<span data-ttu-id="c4fc9-120">&dagger;*en cours* fait référence à la dernière version du navigateur.</span><span class="sxs-lookup"><span data-stu-id="c4fc9-120">&dagger;*Current* refers to the latest version of the browser.</span></span>

## <a name="net-client"></a><span data-ttu-id="c4fc9-121">Client .NET</span><span class="sxs-lookup"><span data-stu-id="c4fc9-121">.NET client</span></span>

<span data-ttu-id="c4fc9-122">Le [client .NET](xref:signalr/dotnet-client) s’exécute sur n’importe quelle plateforme pouvant exécuter ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="c4fc9-122">The [.NET client](xref:signalr/dotnet-client) runs on any platform supported by ASP.NET Core.</span></span> <span data-ttu-id="c4fc9-123">Par exemple, [les développeurs Xamarin peuvent utiliser SignalR](https://github.com/aspnet/Announcements/issues/305) pour créer des applications Android à l’aide de Xamarin. Android 8.4.0.1 et versions ultérieures et des applications iOS à l’aide de Xamarin. iOS 11.14.0.4 et versions ultérieures.</span><span class="sxs-lookup"><span data-stu-id="c4fc9-123">For example, [Xamarin developers can use SignalR](https://github.com/aspnet/Announcements/issues/305) for building Android apps using Xamarin.Android 8.4.0.1 and later and iOS apps using Xamarin.iOS 11.14.0.4 and later.</span></span>

<span data-ttu-id="c4fc9-124">Si le serveur exécute IIS, le transport WebSockets requiert IIS 8,0 ou une version ultérieure sur Windows Server 2012 ou version ultérieure.</span><span class="sxs-lookup"><span data-stu-id="c4fc9-124">If the server runs IIS, the WebSockets transport requires IIS 8.0 or later on Windows Server 2012 or later.</span></span> <span data-ttu-id="c4fc9-125">Les autres transports sont pris en charge sur toutes les plateformes.</span><span class="sxs-lookup"><span data-stu-id="c4fc9-125">Other transports are supported on all platforms.</span></span>

## <a name="java-client"></a><span data-ttu-id="c4fc9-126">Client Java</span><span class="sxs-lookup"><span data-stu-id="c4fc9-126">Java client</span></span>

<span data-ttu-id="c4fc9-127">Le [client Java](xref:signalr/java-client) est disponible pour Java à partir de la version 8.</span><span class="sxs-lookup"><span data-stu-id="c4fc9-127">The [Java client](xref:signalr/java-client) supports Java 8 and later versions.</span></span>

## <a name="unsupported-clients"></a><span data-ttu-id="c4fc9-128">Clients non pris en charge</span><span class="sxs-lookup"><span data-stu-id="c4fc9-128">Unsupported clients</span></span>

<span data-ttu-id="c4fc9-129">Les clients suivants sont disponibles, mais sont expérimentaux ou non officiels.</span><span class="sxs-lookup"><span data-stu-id="c4fc9-129">The following clients are available but are experimental or unofficial.</span></span> <span data-ttu-id="c4fc9-130">Ils ne sont actuellement pas pris en charge et peuvent ne jamais l'être</span><span class="sxs-lookup"><span data-stu-id="c4fc9-130">They aren't currently supported and may never be.</span></span>

* <span data-ttu-id="c4fc9-131">[C++client](https://github.com/aspnet/SignalR/tree/master/clients/cpp)</span><span class="sxs-lookup"><span data-stu-id="c4fc9-131">[C++ client](https://github.com/aspnet/SignalR/tree/master/clients/cpp)</span></span>

* <span data-ttu-id="c4fc9-132">[Client SWIFT](https://github.com/moozzyk/SignalR-Client-Swift)</span><span class="sxs-lookup"><span data-stu-id="c4fc9-132">[Swift client](https://github.com/moozzyk/SignalR-Client-Swift)</span></span>
