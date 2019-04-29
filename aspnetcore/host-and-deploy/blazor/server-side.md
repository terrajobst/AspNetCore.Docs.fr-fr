---
title: Héberger et déployer Blazor côté serveur
author: guardrex
description: Découvrez comment héberger et déployer une application Blazor côté serveur avec ASP.NET Core.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 04/15/2019
uid: host-and-deploy/blazor/server-side
ms.openlocfilehash: 940020ee44d72d50395aad64bc924413c1bbecfb
ms.sourcegitcommit: 017b673b3c700d2976b77201d0ac30172e2abc87
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/16/2019
ms.locfileid: "59614659"
---
# <a name="host-and-deploy-blazor-server-side"></a><span data-ttu-id="490e2-103">Héberger et déployer Blazor côté serveur</span><span class="sxs-lookup"><span data-stu-id="490e2-103">Host and deploy Blazor server-side</span></span>

<span data-ttu-id="490e2-104">Par [Luke Latham](https://github.com/guardrex), [Rainer Stropek](https://www.timecockpit.com) et [Daniel Roth](https://github.com/danroth27)</span><span class="sxs-lookup"><span data-stu-id="490e2-104">By [Luke Latham](https://github.com/guardrex), [Rainer Stropek](https://www.timecockpit.com), and [Daniel Roth](https://github.com/danroth27)</span></span>

## <a name="host-configuration-values"></a><span data-ttu-id="490e2-105">Valeurs de configuration de l’hôte</span><span class="sxs-lookup"><span data-stu-id="490e2-105">Host configuration values</span></span>

<span data-ttu-id="490e2-106">Les applications côté serveur qui utilisent le [modèle d’hébergement côté serveur](xref:blazor/hosting-models#server-side-hosting-model) peuvent accepter des [valeurs de configuration d’hôte générique](xref:fundamentals/host/generic-host#host-configuration).</span><span class="sxs-lookup"><span data-stu-id="490e2-106">Server-side apps that use the [server-side hosting model](xref:blazor/hosting-models#server-side-hosting-model) can accept [Generic Host configuration values](xref:fundamentals/host/generic-host#host-configuration).</span></span>

## <a name="deployment"></a><span data-ttu-id="490e2-107">Déploiement</span><span class="sxs-lookup"><span data-stu-id="490e2-107">Deployment</span></span>

<span data-ttu-id="490e2-108">Avec le [modèle d’hébergement côté serveur](xref:blazor/hosting-models#server-side-hosting-model), Blazor est exécuté sur le serveur à partir d’une application ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="490e2-108">With the [server-side hosting model](xref:blazor/hosting-models#server-side-hosting-model), Blazor is executed on the server from within an ASP.NET Core app.</span></span> <span data-ttu-id="490e2-109">Les mises à jour de l’interface utilisateur, la gestion des événements et les appels JavaScript sont gérés par le biais d’une connexion [SignalR](xref:signalr/introduction).</span><span class="sxs-lookup"><span data-stu-id="490e2-109">UI updates, event handling, and JavaScript calls are handled over a [SignalR](xref:signalr/introduction) connection.</span></span>

<span data-ttu-id="490e2-110">L’application est incluse avec l’application ASP.NET Core dans la sortie publiée, et les deux applications sont déployées ensemble.</span><span class="sxs-lookup"><span data-stu-id="490e2-110">The app is included with the ASP.NET Core app in the published output, and the two apps are deployed together.</span></span> <span data-ttu-id="490e2-111">Un serveur web capable d’héberger une application ASP.NET Core est nécessaire.</span><span class="sxs-lookup"><span data-stu-id="490e2-111">A web server that's capable of hosting an ASP.NET Core app is required.</span></span> <span data-ttu-id="490e2-112">Pour un déploiement côté serveur, Visual Studio inclut le modèle de projet **Razor Components** (modèle `razorcomponents` quand vous utilisez la commande [dotnet new](/dotnet/core/tools/dotnet-new)).</span><span class="sxs-lookup"><span data-stu-id="490e2-112">For a server-side deployment, Visual Studio includes the **Razor Components** project template (`razorcomponents` template when using the [dotnet new](/dotnet/core/tools/dotnet-new) command).</span></span>

<!--

**INSERT: Concerns are the same as publishing an ASP.NET Core SignalR app**

**INSERT: Content on the Azure SignalR Service**

**INSERT: Manually turn on WebSockets support**

-->

<span data-ttu-id="490e2-113">Pour plus d’informations sur l’hébergement et le déploiement d’applications ASP.NET Core, consultez <xref:host-and-deploy/index>.</span><span class="sxs-lookup"><span data-stu-id="490e2-113">For more information on ASP.NET Core app hosting and deployment, see <xref:host-and-deploy/index>.</span></span>

<span data-ttu-id="490e2-114">Pour plus d’informations concernant le déploiement sur Azure App Service, consultez <xref:tutorials/publish-to-azure-webapp-using-vs>.</span><span class="sxs-lookup"><span data-stu-id="490e2-114">For information on deploying to Azure App Service, see <xref:tutorials/publish-to-azure-webapp-using-vs>.</span></span>
