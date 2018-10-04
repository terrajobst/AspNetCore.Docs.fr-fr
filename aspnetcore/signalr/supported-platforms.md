---
title: ASP.NET Core SignalR plateformes prises en charge
author: tdykstra
description: Plateformes prises en charge pour ASP.NET Core SignalR
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 09/26/2018
uid: signalr/supported-platforms
ms.openlocfilehash: d6d74a55d35ddb34a6f66a171bfe3f343dd61b63
ms.sourcegitcommit: 7b4e3936feacb1a8fcea7802aab3e2ea9c8af5b4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/04/2018
ms.locfileid: "48577624"
---
# <a name="aspnet-core-signalr-supported-platforms"></a><span data-ttu-id="9c89d-103">ASP.NET Core SignalR plateformes prises en charge</span><span class="sxs-lookup"><span data-stu-id="9c89d-103">ASP.NET Core SignalR supported platforms</span></span>

## <a name="server-system-requirements"></a><span data-ttu-id="9c89d-104">Configuration requise du système serveur</span><span class="sxs-lookup"><span data-stu-id="9c89d-104">Server system requirements</span></span>

<span data-ttu-id="9c89d-105">SignalR pour ASP.NET Core prend en charge n’importe quelle plateforme de serveur QU'ASP.NET Core prend en charge.</span><span class="sxs-lookup"><span data-stu-id="9c89d-105">SignalR for ASP.NET Core supports any server platform ASP.NET Core supports.</span></span>

## <a name="javascript-client"></a><span data-ttu-id="9c89d-106">Client JavaScript</span><span class="sxs-lookup"><span data-stu-id="9c89d-106">JavaScript client</span></span>

<span data-ttu-id="9c89d-107">Le [client JavaScript](https://www.npmjs.com/package/@aspnet/signalr) s’exécute sur NodeJS 8 et versions ultérieures et les navigateurs suivants :</span><span class="sxs-lookup"><span data-stu-id="9c89d-107">The [JavaScript client](https://www.npmjs.com/package/@aspnet/signalr) runs on NodeJS 8 and later versions and the following browsers:</span></span>

| <span data-ttu-id="9c89d-108">Visiteur</span><span class="sxs-lookup"><span data-stu-id="9c89d-108">Browser</span></span> | <span data-ttu-id="9c89d-109">Version</span><span class="sxs-lookup"><span data-stu-id="9c89d-109">Version</span></span> |
| ------- | ------- |
| <span data-ttu-id="9c89d-110">Microsoft Edge</span><span class="sxs-lookup"><span data-stu-id="9c89d-110">Microsoft Edge</span></span> | <span data-ttu-id="9c89d-111">actuels</span><span class="sxs-lookup"><span data-stu-id="9c89d-111">current</span></span> |
| <span data-ttu-id="9c89d-112">Mozilla Firefox</span><span class="sxs-lookup"><span data-stu-id="9c89d-112">Mozilla Firefox</span></span> | <span data-ttu-id="9c89d-113">actuels</span><span class="sxs-lookup"><span data-stu-id="9c89d-113">current</span></span> |
| <span data-ttu-id="9c89d-114">Google Chrome ; inclut Android</span><span class="sxs-lookup"><span data-stu-id="9c89d-114">Google Chrome; includes Android</span></span> | <span data-ttu-id="9c89d-115">actuels</span><span class="sxs-lookup"><span data-stu-id="9c89d-115">current</span></span> |
| <span data-ttu-id="9c89d-116">Safari ; inclut iOS</span><span class="sxs-lookup"><span data-stu-id="9c89d-116">Safari; includes iOS</span></span> | <span data-ttu-id="9c89d-117">actuels</span><span class="sxs-lookup"><span data-stu-id="9c89d-117">current</span></span> |
| <span data-ttu-id="9c89d-118">Microsoft Internet Explorer</span><span class="sxs-lookup"><span data-stu-id="9c89d-118">Microsoft Internet Explorer</span></span> | <span data-ttu-id="9c89d-119">11</span><span class="sxs-lookup"><span data-stu-id="9c89d-119">11</span></span> |
 
## <a name="net-client"></a><span data-ttu-id="9c89d-120">Client .NET</span><span class="sxs-lookup"><span data-stu-id="9c89d-120">.NET client</span></span>

<span data-ttu-id="9c89d-121">Le [client .NET](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR/) s’exécute sur n’importe quelle plateforme de serveur pris en charge par ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="9c89d-121">The [.NET client](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR/) runs on any server platform supported by ASP.NET Core.</span></span>

<span data-ttu-id="9c89d-122">Lorsque le serveur s’exécute IIS, le transport WebSocket requiert IIS 8.0 ou version ultérieure, sur Windows Server 2012 ou version ultérieure.</span><span class="sxs-lookup"><span data-stu-id="9c89d-122">When the server runs IIS, the WebSockets transport requires IIS 8.0 or higher, on Windows Server 2012 or higher.</span></span> <span data-ttu-id="9c89d-123">Autres transports sont pris en charge sur toutes les plateformes.</span><span class="sxs-lookup"><span data-stu-id="9c89d-123">Other transports are supported on all platforms.</span></span>

## <a name="java-client"></a><span data-ttu-id="9c89d-124">Client Java</span><span class="sxs-lookup"><span data-stu-id="9c89d-124">Java client</span></span>

<span data-ttu-id="9c89d-125">Le [client Java](https://search.maven.org/artifact/com.microsoft.aspnet/signalr) prend en charge de Java 8 et versions ultérieures.</span><span class="sxs-lookup"><span data-stu-id="9c89d-125">The [Java client](https://search.maven.org/artifact/com.microsoft.aspnet/signalr) supports Java 8 and later versions.</span></span>

## <a name="unsupported-clients"></a><span data-ttu-id="9c89d-126">Clients non pris en charge</span><span class="sxs-lookup"><span data-stu-id="9c89d-126">Unsupported clients</span></span>

<span data-ttu-id="9c89d-127">Les clients suivants sont disponibles mais sont expérimentales ou non officiels.</span><span class="sxs-lookup"><span data-stu-id="9c89d-127">The following clients are available but are experimental or unofficial.</span></span> <span data-ttu-id="9c89d-128">Ils ne prennent pas en charge maintenant et ne peuvent pas jamais être pris en charge.</span><span class="sxs-lookup"><span data-stu-id="9c89d-128">They are not supported now and may not ever be supported.</span></span>

* [<span data-ttu-id="9c89d-129">Client C++</span><span class="sxs-lookup"><span data-stu-id="9c89d-129">C++ client</span></span>](https://github.com/aspnet/SignalR/tree/master/clients/cpp)

* [<span data-ttu-id="9c89d-130">Client SWIFT</span><span class="sxs-lookup"><span data-stu-id="9c89d-130">Swift client</span></span>](https://github.com/moozzyk/SignalR-Client-Swift)
