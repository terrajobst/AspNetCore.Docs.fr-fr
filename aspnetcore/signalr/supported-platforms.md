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
# <a name="aspnet-core-signalr-supported-platforms"></a>Plateformes prises en charge par ASP.NET Core Signalr

## <a name="server-system-requirements"></a>Configuration système requise pour le serveur

Signalr pour ASP.NET Core prend en charge toute plateforme de serveur prise en charge par ASP.NET Core.

## <a name="javascript-client"></a>Client JavaScript

Le [client JavaScript](https://www.npmjs.com/package/@aspnet/signalr) s’exécute sur NodeJS 8 et versions ultérieures, ainsi que sur les navigateurs suivants :

| Visiteur                         | Version         |
| ------------------------------- | --------------- |
| Microsoft Edge                  | &dagger; actuel |
| Mozilla Firefox                 | &dagger; actuel |
| Google Chrome ; comprend Android | &dagger; actuel |
| Safari comprend iOS            | &dagger; actuel |
| Microsoft Internet Explorer     | 11              |

&dagger;*en cours* fait référence à la dernière version du navigateur.

## <a name="net-client"></a>Client .NET

Le [client .net](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR/) s’exécute sur n’importe quelle plateforme prise en charge par ASP.net core. Par exemple, [les développeurs Xamarin peuvent utiliser signalr](https://github.com/aspnet/Announcements/issues/305) pour créer des applications Android à l’aide de Xamarin. Android 8.4.0.1 et versions ultérieures et des applications iOS à l’aide de Xamarin. iOS 11.14.0.4 et versions ultérieures.

Si le serveur exécute IIS, le transport WebSockets requiert IIS 8,0 ou une version ultérieure sur Windows Server 2012 ou version ultérieure. D’autres transports sont pris en charge sur toutes les plateformes.

## <a name="java-client"></a>Client Java

Le [client Java](https://search.maven.org/artifact/com.microsoft.aspnet/signalr) prend en charge Java 8 et versions ultérieures.

## <a name="unsupported-clients"></a>Clients non pris en charge

Les clients suivants sont disponibles, mais sont expérimentaux ou non officiels. Ils ne sont actuellement pas pris en charge et peuvent ne jamais être.

* [C++client](https://github.com/aspnet/SignalR/tree/master/clients/cpp)

* [Client SWIFT](https://github.com/moozzyk/SignalR-Client-Swift)
