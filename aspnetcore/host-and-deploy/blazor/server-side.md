---
title: Héberger et déployer ASP.NET Core Blazor côté serveur
author: guardrex
description: Découvrez comment héberger et déployer une application Blazor côté serveur avec ASP.NET Core.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 06/11/2019
uid: host-and-deploy/blazor/server-side
ms.openlocfilehash: 8b332c2fb439e9832d604ed26f972b266eed2507
ms.sourcegitcommit: 9bb29f9ba6f0645ee8b9cabda07e3a5aa52cd659
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/26/2019
ms.locfileid: "67406118"
---
# <a name="host-and-deploy-blazor-server-side"></a>Héberger et déployer Blazor côté serveur

Par [Luke Latham](https://github.com/guardrex), [Rainer Stropek](https://www.timecockpit.com) et [Daniel Roth](https://github.com/danroth27)

## <a name="host-configuration-values"></a>Valeurs de configuration de l’hôte

Les applications côté serveur qui utilisent le [modèle d’hébergement côté serveur](xref:blazor/hosting-models#server-side) peuvent accepter des [valeurs de configuration d’hôte générique](xref:fundamentals/host/generic-host#host-configuration).

## <a name="deployment"></a>Déploiement

Avec le [modèle d’hébergement côté serveur](xref:blazor/hosting-models#server-side), Blazor est exécuté sur le serveur à partir d’une application ASP.NET Core. Les mises à jour de l’interface utilisateur, la gestion des événements et les appels JavaScript sont gérés par le biais d’une connexion [SignalR](xref:signalr/introduction).

Un serveur web capable d’héberger une application ASP.NET Core est nécessaire. Visual Studio inclut le modèle de projet **Blazor (côté serveur)** (modèle `blazorserverside` lorsque vous utilisez la commande [dotnet new](/dotnet/core/tools/dotnet-new)).

## <a name="connection-scale-out"></a>Monter en charge la connexion

Les applications côté serveur Blazor nécessitent une seule connexion SignalR active pour chaque utilisateur. Un déploiement de Blazor côté serveur de production nécessite une solution pour prendre en charge autant de connexions simultanées que requis par l’application. Le [Azure SignalR Service](/azure/azure-signalr/) gère la mise à l’échelle des connexions et est recommandé comme solution de mise à l’échelle pour les applications côté serveur Blazor. Pour plus d’informations, consultez <xref:signalr/publish-to-azure-web-app>.

## <a name="signalr-configuration"></a>Configuration SignalR

SignalR est configuré par ASP.NET Core pour les scénarios côté serveur Blazor les plus courants. Pour les scénarios avancés et personnalisés, consultez les articles SignalR dans la section [Ressources supplémentaires](#additional-resources).

## <a name="additional-resources"></a>Ressources supplémentaires

* <xref:signalr/introduction>
* [Documentation Azure SignalR Service](/azure/azure-signalr/)
* [Démarrage rapide : Créer une salle de conversation à l'aide de SignalR Service](/azure/azure-signalr/signalr-quickstart-dotnet-core)
* <xref:host-and-deploy/index>
* <xref:tutorials/publish-to-azure-webapp-using-vs>
* [Déployer la préversion d’ASP.NET Core sur Azure App Service](xref:host-and-deploy/azure-apps/index#deploy-aspnet-core-preview-release-to-azure-app-service)
