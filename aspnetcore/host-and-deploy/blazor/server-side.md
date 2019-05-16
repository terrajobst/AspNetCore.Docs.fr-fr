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
# <a name="host-and-deploy-blazor-server-side"></a><span data-ttu-id="ee7d8-103">Héberger et déployer Blazor côté serveur</span><span class="sxs-lookup"><span data-stu-id="ee7d8-103">Host and deploy Blazor server-side</span></span>

<span data-ttu-id="ee7d8-104">Par [Luke Latham](https://github.com/guardrex), [Rainer Stropek](https://www.timecockpit.com) et [Daniel Roth](https://github.com/danroth27)</span><span class="sxs-lookup"><span data-stu-id="ee7d8-104">By [Luke Latham](https://github.com/guardrex), [Rainer Stropek](https://www.timecockpit.com), and [Daniel Roth](https://github.com/danroth27)</span></span>

## <a name="host-configuration-values"></a><span data-ttu-id="ee7d8-105">Valeurs de configuration de l’hôte</span><span class="sxs-lookup"><span data-stu-id="ee7d8-105">Host configuration values</span></span>

<span data-ttu-id="ee7d8-106">Les applications côté serveur qui utilisent le [modèle d’hébergement côté serveur](xref:blazor/hosting-models#server-side) peuvent accepter des [valeurs de configuration d’hôte générique](xref:fundamentals/host/generic-host#host-configuration).</span><span class="sxs-lookup"><span data-stu-id="ee7d8-106">Server-side apps that use the [server-side hosting model](xref:blazor/hosting-models#server-side) can accept [Generic Host configuration values](xref:fundamentals/host/generic-host#host-configuration).</span></span>

## <a name="deployment"></a><span data-ttu-id="ee7d8-107">Déploiement</span><span class="sxs-lookup"><span data-stu-id="ee7d8-107">Deployment</span></span>

<span data-ttu-id="ee7d8-108">Avec le [modèle d’hébergement côté serveur](xref:blazor/hosting-models#server-side), Blazor est exécuté sur le serveur à partir d’une application ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="ee7d8-108">With the [server-side hosting model](xref:blazor/hosting-models#server-side), Blazor is executed on the server from within an ASP.NET Core app.</span></span> <span data-ttu-id="ee7d8-109">Les mises à jour de l’interface utilisateur, la gestion des événements et les appels JavaScript sont gérés par le biais d’une connexion [SignalR](xref:signalr/introduction).</span><span class="sxs-lookup"><span data-stu-id="ee7d8-109">UI updates, event handling, and JavaScript calls are handled over a [SignalR](xref:signalr/introduction) connection.</span></span>

<span data-ttu-id="ee7d8-110">Un serveur web capable d’héberger une application ASP.NET Core est nécessaire.</span><span class="sxs-lookup"><span data-stu-id="ee7d8-110">A web server capable of hosting an ASP.NET Core app is required.</span></span> <span data-ttu-id="ee7d8-111">Visual Studio inclut le modèle de projet **Blazor (côté serveur)** (modèle `blazorserverside` lorsque vous utilisez la commande [dotnet new](/dotnet/core/tools/dotnet-new)).</span><span class="sxs-lookup"><span data-stu-id="ee7d8-111">Visual Studio includes the **Blazor (server-side)** project template (`blazorserverside` template when using the [dotnet new](/dotnet/core/tools/dotnet-new) command).</span></span>

<!--

**INSERT: Concerns are the same as publishing an ASP.NET Core SignalR app**

**INSERT: Content on the Azure SignalR Service**

**INSERT: Manually turn on WebSockets support**

-->

## <a name="additional-resources"></a><span data-ttu-id="ee7d8-112">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="ee7d8-112">Additional resources</span></span>

* <xref:signalr/introduction>
* <xref:host-and-deploy/index>
* <xref:tutorials/publish-to-azure-webapp-using-vs>
* [<span data-ttu-id="ee7d8-113">Déployer la préversion d’ASP.NET Core sur Azure App Service</span><span class="sxs-lookup"><span data-stu-id="ee7d8-113">Deploy ASP.NET Core preview release to Azure App Service</span></span>](xref:host-and-deploy/azure-apps/index#deploy-aspnet-core-preview-release-to-azure-app-service)
