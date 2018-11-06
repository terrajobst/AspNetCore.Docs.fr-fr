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
# <a name="aspnet-core-signalr-supported-platforms"></a>Plateformes prises en charge par ASP.NET Core SignalR

## <a name="server-system-requirements"></a>Configuration requise du système serveur

SignalR pour ASP.NET Core prend en charge n’importe quelle plateforme de serveur ASP.NET Core prend en charge.

## <a name="javascript-client"></a>Client JavaScript

Le [client JavaScript](https://www.npmjs.com/package/@aspnet/signalr) s’exécute sur NodeJS 8 et versions ultérieures et les navigateurs suivants :

| Visiteur                         | Version |
| ------------------------------- | ------- |
| Microsoft Edge                  | actuelle |
| Mozilla Firefox                 | actuelle |
| Google Chrome ; inclut Android | actuelle |
| Safari ; inclut iOS            | actuelle |
| Microsoft Internet Explorer     | 11      |
 
## <a name="net-client"></a>Client .NET

Le [client .NET](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR/) s’exécute sur n’importe quelle plateforme de serveur pris en charge par ASP.NET Core.

Si le serveur exécute IIS, le transport WebSocket requiert IIS 8.0 ou version ultérieure sur Windows Server 2012 ou version ultérieure. Autres transports sont pris en charge sur toutes les plateformes.

## <a name="java-client"></a>Client Java

Le [client Java](https://search.maven.org/artifact/com.microsoft.aspnet/signalr) prend en charge de Java 8 et versions ultérieures.

## <a name="unsupported-clients"></a>Clients non pris en charge

Les clients suivants sont disponibles mais sont expérimentales ou non officiels. Ils ne sont pas actuellement pris en charge et ne peuvent jamais être.

* [Client C++](https://github.com/aspnet/SignalR/tree/master/clients/cpp)

* [Client SWIFT](https://github.com/moozzyk/SignalR-Client-Swift)
