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

Le [client .NET](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR/) s’exécute sur n’importe quelle plateforme prise en charge par ASP.NET Core. Par exemple, [les développeurs Xamarin peuvent utiliser SignalR](https://github.com/aspnet/Announcements/issues/305) pour la création d’applications Android à l’aide de Xamarin.Android 8.4.0.1 et versions ultérieures et les applications iOS à l’aide de Xamarin.iOS 11.14.0.4 et versions ultérieures.

Si le serveur exécute IIS, le transport WebSocket requiert IIS 8.0 ou version ultérieure sur Windows Server 2012 ou version ultérieure. Autres transports sont pris en charge sur toutes les plateformes.

## <a name="java-client"></a>Client Java

Le [client Java](https://search.maven.org/artifact/com.microsoft.aspnet/signalr) prend en charge de Java 8 et versions ultérieures.

## <a name="unsupported-clients"></a>Clients non pris en charge

Les clients suivants sont disponibles mais sont expérimentales ou non officiels. Ils ne sont pas actuellement pris en charge et ne peuvent jamais être.

* [Client C++](https://github.com/aspnet/SignalR/tree/master/clients/cpp)

* [Client SWIFT](https://github.com/moozzyk/SignalR-Client-Swift)
