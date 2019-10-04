---
title: Fonctionnalités du client SignalR
author: bradygaster
description: Découvrez les fonctionnalités prises en charge par les différents clients ASP.NET Core Signalr.
ms.author: bradyg
ms.custom: mvc
ms.date: 09/18/2019
uid: signalr/client-features
ms.openlocfilehash: 6718722cdbcfae500026fcd429ca6b9de8f0f103
ms.sourcegitcommit: 73e255e846e414821b8cc20ffa3aec946735cd4e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/03/2019
ms.locfileid: "71925358"
---
# <a name="aspnet-core-signalr-client-features"></a>Fonctionnalités du client ASP.NET Core Signalr

## <a name="feature-distribution"></a>Distribution des fonctionnalités

Le tableau ci-dessous présente les fonctionnalités et la prise en charge des clients qui offrent une prise en charge en temps réel. Pour chaque fonctionnalité, la version *minimale* qui prend en charge cette fonctionnalité est indiquée. Si aucune version n’est indiquée, la fonctionnalité n’est pas prise en charge.

| Fonctionnalité | .NET | JavaScript | Java |
| ---- | :-: | :-: | :-: |
| Prise en charge du service Azure Signalr |1.0.0|1.0.0|1.0.0|
| [Streaming de serveur à client](xref:signalr/streaming)          |1.0.0|1.0.0|1.0.0|
| [Streaming client à serveur](xref:signalr/streaming)          |3.0.0|3.0.0|3.0.0|
| Reconnexion automatique ([.net](/aspnet/core/signalr/dotnet-client?view=aspnetcore-3.0&tabs=visual-studio#handle-lost-connection), [JavaScript](/aspnet/core/signalr/javascript-client?view=aspnetcore-3.0#reconnect-clients))          |3.0.0|3.0.0|❌|
| Transport WebSockets |1.0.0|1.0.0|1.0.0|
| Transport des événements envoyés par le serveur |1.0.0|1.0.0|❌|
| Transport d’interrogation longue |1.0.0|1.0.0|3.0.0|
| Protocole JSON Hub |1.0.0|1.0.0|1.0.0|
| Protocole MessagePack Hub |1.0.0|1.0.0|❌|

La prise en charge de la reconnexion automatique dans le client Java est suivie dans notre suivi des [problèmes](https://github.com/aspnet/AspNetCore/issues/8711).

## <a name="additional-resources"></a>Ressources supplémentaires

* [Prise en main de Signalr pour ASP.NET Core](xref:tutorials/signalr)
* [Plateformes prises en charge](xref:signalr/supported-platforms)
* [Hubs](xref:signalr/hubs)
* [Client JavaScript](xref:signalr/javascript-client)
