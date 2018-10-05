---
title: Plateformes prises en charge par ASP.NET Core SignalR
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
# <a name="aspnet-core-signalr-supported-platforms"></a>Plateformes prises en charge par ASP.NET Core SignalR

## <a name="server-system-requirements"></a>Configuration requise du système serveur

SignalR pour ASP.NET Core prend en charge n’importe quelle plateforme de serveur qu'ASP.NET Core prend en charge.

## <a name="javascript-client"></a>Client JavaScript

Le [client JavaScript](https://www.npmjs.com/package/@aspnet/signalr) s’exécute sur NodeJS 8 et versions ultérieures et les navigateurs suivants :

| Visiteur | Version |
| ------- | ------- |
| Microsoft Edge | actuelle |
| Mozilla Firefox | actuelle |
| Google Chrome ; inclut Android | actuelle |
| Safari ; inclut iOS | actuelle |
| Microsoft Internet Explorer | 11 |
 
## <a name="net-client"></a>Client .NET

Le [client .NET](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR/) s’exécute sur n’importe quelle plateforme de serveur pris en charge par ASP.NET Core.

Lorsque le serveur s’exécute IIS, le transport WebSocket requiert IIS 8.0 ou version ultérieure, sur Windows Server 2012 ou version ultérieure. Autres transports sont pris en charge sur toutes les plateformes.

## <a name="java-client"></a>Client Java

Le [client Java](https://search.maven.org/artifact/com.microsoft.aspnet/signalr) prend en charge de Java 8 et versions ultérieures.

## <a name="unsupported-clients"></a>Clients non pris en charge

Les clients suivants sont disponibles mais sont expérimentales ou non officiels. Ils ne prennent pas en charge maintenant et ne peuvent pas jamais être pris en charge.

* [Client C++](https://github.com/aspnet/SignalR/tree/master/clients/cpp)

* [Client SWIFT](https://github.com/moozzyk/SignalR-Client-Swift)
