---
title: Plateformes prises en charge par ASP.NET Core SignalR
author: tdykstra
description: En savoir plus sur les plateformes prises en charge pour ASP.NET Core SignalR.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 10/31/2018
uid: signalr/supported-platforms
ms.openlocfilehash: 773f6c020dbb2982911e177b55855473c750d52a
ms.sourcegitcommit: fc2486ddbeb15ab4969168d99b3fe0fbe91e8661
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/01/2018
ms.locfileid: "50758178"
---
# <a name="aspnet-core-signalr-supported-platforms"></a><span data-ttu-id="7afbb-103">Plateformes prises en charge par ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="7afbb-103">ASP.NET Core SignalR supported platforms</span></span>

## <a name="server-system-requirements"></a><span data-ttu-id="7afbb-104">Configuration requise du système serveur</span><span class="sxs-lookup"><span data-stu-id="7afbb-104">Server system requirements</span></span>

<span data-ttu-id="7afbb-105">SignalR pour ASP.NET Core prend en charge n’importe quelle plateforme de serveur ASP.NET Core prend en charge.</span><span class="sxs-lookup"><span data-stu-id="7afbb-105">SignalR for ASP.NET Core supports any server platform that ASP.NET Core supports.</span></span>

## <a name="javascript-client"></a><span data-ttu-id="7afbb-106">Client JavaScript</span><span class="sxs-lookup"><span data-stu-id="7afbb-106">JavaScript client</span></span>

<span data-ttu-id="7afbb-107">Le [client JavaScript](https://www.npmjs.com/package/@aspnet/signalr) s’exécute sur NodeJS 8 et versions ultérieures et les navigateurs suivants :</span><span class="sxs-lookup"><span data-stu-id="7afbb-107">The [JavaScript client](https://www.npmjs.com/package/@aspnet/signalr) runs on NodeJS 8 and later versions and the following browsers:</span></span>

| <span data-ttu-id="7afbb-108">Visiteur</span><span class="sxs-lookup"><span data-stu-id="7afbb-108">Browser</span></span>                         | <span data-ttu-id="7afbb-109">Version</span><span class="sxs-lookup"><span data-stu-id="7afbb-109">Version</span></span> |
| ------------------------------- | ------- |
| <span data-ttu-id="7afbb-110">Microsoft Edge</span><span class="sxs-lookup"><span data-stu-id="7afbb-110">Microsoft Edge</span></span>                  | <span data-ttu-id="7afbb-111">actuelle</span><span class="sxs-lookup"><span data-stu-id="7afbb-111">current</span></span> |
| <span data-ttu-id="7afbb-112">Mozilla Firefox</span><span class="sxs-lookup"><span data-stu-id="7afbb-112">Mozilla Firefox</span></span>                 | <span data-ttu-id="7afbb-113">actuelle</span><span class="sxs-lookup"><span data-stu-id="7afbb-113">current</span></span> |
| <span data-ttu-id="7afbb-114">Google Chrome ; inclut Android</span><span class="sxs-lookup"><span data-stu-id="7afbb-114">Google Chrome; includes Android</span></span> | <span data-ttu-id="7afbb-115">actuelle</span><span class="sxs-lookup"><span data-stu-id="7afbb-115">current</span></span> |
| <span data-ttu-id="7afbb-116">Safari ; inclut iOS</span><span class="sxs-lookup"><span data-stu-id="7afbb-116">Safari; includes iOS</span></span>            | <span data-ttu-id="7afbb-117">actuelle</span><span class="sxs-lookup"><span data-stu-id="7afbb-117">current</span></span> |
| <span data-ttu-id="7afbb-118">Microsoft Internet Explorer</span><span class="sxs-lookup"><span data-stu-id="7afbb-118">Microsoft Internet Explorer</span></span>     | <span data-ttu-id="7afbb-119">11</span><span class="sxs-lookup"><span data-stu-id="7afbb-119">11</span></span>      |
 
## <a name="net-client"></a><span data-ttu-id="7afbb-120">Client .NET</span><span class="sxs-lookup"><span data-stu-id="7afbb-120">.NET client</span></span>

<span data-ttu-id="7afbb-121">Le [client .NET](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR/) s’exécute sur n’importe quelle plateforme de serveur pris en charge par ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="7afbb-121">The [.NET client](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR/) runs on any server platform supported by ASP.NET Core.</span></span>

<span data-ttu-id="7afbb-122">Si le serveur exécute IIS, le transport WebSocket requiert IIS 8.0 ou version ultérieure sur Windows Server 2012 ou version ultérieure.</span><span class="sxs-lookup"><span data-stu-id="7afbb-122">If the server runs IIS, the WebSockets transport requires IIS 8.0 or higher on Windows Server 2012 or higher.</span></span> <span data-ttu-id="7afbb-123">Autres transports sont pris en charge sur toutes les plateformes.</span><span class="sxs-lookup"><span data-stu-id="7afbb-123">Other transports are supported on all platforms.</span></span>

## <a name="java-client"></a><span data-ttu-id="7afbb-124">Client Java</span><span class="sxs-lookup"><span data-stu-id="7afbb-124">Java client</span></span>

<span data-ttu-id="7afbb-125">Le [client Java](https://search.maven.org/artifact/com.microsoft.aspnet/signalr) prend en charge de Java 8 et versions ultérieures.</span><span class="sxs-lookup"><span data-stu-id="7afbb-125">The [Java client](https://search.maven.org/artifact/com.microsoft.aspnet/signalr) supports Java 8 and later versions.</span></span>

## <a name="unsupported-clients"></a><span data-ttu-id="7afbb-126">Clients non pris en charge</span><span class="sxs-lookup"><span data-stu-id="7afbb-126">Unsupported clients</span></span>

<span data-ttu-id="7afbb-127">Les clients suivants sont disponibles mais sont expérimentales ou non officiels.</span><span class="sxs-lookup"><span data-stu-id="7afbb-127">The following clients are available but are experimental or unofficial.</span></span> <span data-ttu-id="7afbb-128">Ils ne sont pas actuellement pris en charge et ne peuvent jamais être.</span><span class="sxs-lookup"><span data-stu-id="7afbb-128">They aren't currently supported and may never be.</span></span>

* [<span data-ttu-id="7afbb-129">Client C++</span><span class="sxs-lookup"><span data-stu-id="7afbb-129">C++ client</span></span>](https://github.com/aspnet/SignalR/tree/master/clients/cpp)

* [<span data-ttu-id="7afbb-130">Client SWIFT</span><span class="sxs-lookup"><span data-stu-id="7afbb-130">Swift client</span></span>](https://github.com/moozzyk/SignalR-Client-Swift)
