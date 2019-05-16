---
title: Héberger et déployer Blazor côté serveur
author: guardrex
description: Découvrez comment héberger et déployer une application Blazor côté serveur avec ASP.NET Core.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 04/26/2019
uid: host-and-deploy/blazor/server-side
ms.openlocfilehash: 8e44be09a4cceba2509f3e86abf3ce5fd2d077bd
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/27/2019
ms.locfileid: "64887764"
---
# <a name="host-and-deploy-blazor-server-side"></a>Héberger et déployer Blazor côté serveur

Par [Luke Latham](https://github.com/guardrex), [Rainer Stropek](https://www.timecockpit.com) et [Daniel Roth](https://github.com/danroth27)

## <a name="host-configuration-values"></a>Valeurs de configuration de l’hôte

Les applications côté serveur qui utilisent le [modèle d’hébergement côté serveur](xref:blazor/hosting-models#server-side) peuvent accepter des [valeurs de configuration d’hôte générique](xref:fundamentals/host/generic-host#host-configuration).

## <a name="deployment"></a>Déploiement

Avec le [modèle d’hébergement côté serveur](xref:blazor/hosting-models#server-side), Blazor est exécuté sur le serveur à partir d’une application ASP.NET Core. Les mises à jour de l’interface utilisateur, la gestion des événements et les appels JavaScript sont gérés par le biais d’une connexion [SignalR](xref:signalr/introduction).

Un serveur web capable d’héberger une application ASP.NET Core est nécessaire. Visual Studio inclut le modèle de projet **Blazor (côté serveur)** (modèle `blazorserverside` lorsque vous utilisez la commande [dotnet new](/dotnet/core/tools/dotnet-new)).

<!--

**INSERT: Concerns are the same as publishing an ASP.NET Core SignalR app**

**INSERT: Content on the Azure SignalR Service**

**INSERT: Manually turn on WebSockets support**

-->

## <a name="additional-resources"></a>Ressources supplémentaires

* <xref:signalr/introduction>
* <xref:host-and-deploy/index>
* <xref:tutorials/publish-to-azure-webapp-using-vs>
* [Déployer la préversion d’ASP.NET Core sur Azure App Service](xref:host-and-deploy/azure-apps/index#deploy-aspnet-core-preview-release-to-azure-app-service)
