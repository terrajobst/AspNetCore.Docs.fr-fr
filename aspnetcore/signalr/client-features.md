---
title: Fonctionnalités du client SignalR
author: bradygaster
description: Découvrez les fonctionnalités prises en charge par les différents clients ASP.NET Core Signalr.
monikerRange: '>= aspnetcore-3.0'
ms.author: bradyg
ms.custom: mvc
ms.date: 09/18/2019
uid: signalr/client-features
ms.openlocfilehash: 2d6759a5484c37aee6db3d22b3127414231605ae
ms.sourcegitcommit: 14b25156e34c82ed0495b4aff5776ac5b1950b5e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/26/2019
ms.locfileid: "71301189"
---
# <a name="aspnet-core-signalr-client-features"></a>Fonctionnalités du client ASP.NET Core Signalr

## <a name="feature-distribution"></a>Distribution des fonctionnalités

Le tableau ci-dessous présente les fonctionnalités et la prise en charge des clients qui offrent une prise en charge en temps réel.

| Fonctionnalité | .NET | JavaScript | Java |
| ---- | :-: | :-: | :-: |
| Prise en charge du service Azure Signalr |✔|✔|✔|
| [Streaming de serveur à client](xref:signalr/streaming)          |✔|✔|✔|
| [Streaming client à serveur](xref:signalr/streaming)          |✔|✔|✔|
| Reconnexion automatique ([.net](/aspnet/core/signalr/dotnet-client?view=aspnetcore-3.0&tabs=visual-studio#handle-lost-connection), [JavaScript](/aspnet/core/signalr/javascript-client?view=aspnetcore-3.0#reconnect-clients))          |✔|✔| |
| Transport WebSockets |✔|✔|✔|
| Transport des événements envoyés par le serveur |✔|✔| |
| Transport d’interrogation longue |✔|✔|✔|
| Protocole JSON Hub |✔|✔|✔|
| Protocole MessagePack Hub |✔|✔| |

La prise en charge de la reconnexion automatique dans le client Java est suivie dans notre suivi des [problèmes](https://github.com/aspnet/AspNetCore/issues/8711).

## <a name="additional-resources"></a>Ressources supplémentaires

* [Prise en main de Signalr pour ASP.NET Core](xref:tutorials/signalr)
* [Plateformes prises en charge](xref:signalr/supported-platforms)
* [Hubs](xref:signalr/hubs)
* [Client JavaScript](xref:signalr/javascript-client)
