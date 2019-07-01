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
# <a name="host-and-deploy-blazor-server-side"></a><span data-ttu-id="1480f-103">Héberger et déployer Blazor côté serveur</span><span class="sxs-lookup"><span data-stu-id="1480f-103">Host and deploy Blazor server-side</span></span>

<span data-ttu-id="1480f-104">Par [Luke Latham](https://github.com/guardrex), [Rainer Stropek](https://www.timecockpit.com) et [Daniel Roth](https://github.com/danroth27)</span><span class="sxs-lookup"><span data-stu-id="1480f-104">By [Luke Latham](https://github.com/guardrex), [Rainer Stropek](https://www.timecockpit.com), and [Daniel Roth](https://github.com/danroth27)</span></span>

## <a name="host-configuration-values"></a><span data-ttu-id="1480f-105">Valeurs de configuration de l’hôte</span><span class="sxs-lookup"><span data-stu-id="1480f-105">Host configuration values</span></span>

<span data-ttu-id="1480f-106">Les applications côté serveur qui utilisent le [modèle d’hébergement côté serveur](xref:blazor/hosting-models#server-side) peuvent accepter des [valeurs de configuration d’hôte générique](xref:fundamentals/host/generic-host#host-configuration).</span><span class="sxs-lookup"><span data-stu-id="1480f-106">Server-side apps that use the [server-side hosting model](xref:blazor/hosting-models#server-side) can accept [Generic Host configuration values](xref:fundamentals/host/generic-host#host-configuration).</span></span>

## <a name="deployment"></a><span data-ttu-id="1480f-107">Déploiement</span><span class="sxs-lookup"><span data-stu-id="1480f-107">Deployment</span></span>

<span data-ttu-id="1480f-108">Avec le [modèle d’hébergement côté serveur](xref:blazor/hosting-models#server-side), Blazor est exécuté sur le serveur à partir d’une application ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="1480f-108">With the [server-side hosting model](xref:blazor/hosting-models#server-side), Blazor is executed on the server from within an ASP.NET Core app.</span></span> <span data-ttu-id="1480f-109">Les mises à jour de l’interface utilisateur, la gestion des événements et les appels JavaScript sont gérés par le biais d’une connexion [SignalR](xref:signalr/introduction).</span><span class="sxs-lookup"><span data-stu-id="1480f-109">UI updates, event handling, and JavaScript calls are handled over a [SignalR](xref:signalr/introduction) connection.</span></span>

<span data-ttu-id="1480f-110">Un serveur web capable d’héberger une application ASP.NET Core est nécessaire.</span><span class="sxs-lookup"><span data-stu-id="1480f-110">A web server capable of hosting an ASP.NET Core app is required.</span></span> <span data-ttu-id="1480f-111">Visual Studio inclut le modèle de projet **Blazor (côté serveur)** (modèle `blazorserverside` lorsque vous utilisez la commande [dotnet new](/dotnet/core/tools/dotnet-new)).</span><span class="sxs-lookup"><span data-stu-id="1480f-111">Visual Studio includes the **Blazor (server-side)** project template (`blazorserverside` template when using the [dotnet new](/dotnet/core/tools/dotnet-new) command).</span></span>

## <a name="connection-scale-out"></a><span data-ttu-id="1480f-112">Monter en charge la connexion</span><span class="sxs-lookup"><span data-stu-id="1480f-112">Connection scale out</span></span>

<span data-ttu-id="1480f-113">Les applications côté serveur Blazor nécessitent une seule connexion SignalR active pour chaque utilisateur.</span><span class="sxs-lookup"><span data-stu-id="1480f-113">Blazor server-side apps require one active SignalR connection for each user.</span></span> <span data-ttu-id="1480f-114">Un déploiement de Blazor côté serveur de production nécessite une solution pour prendre en charge autant de connexions simultanées que requis par l’application.</span><span class="sxs-lookup"><span data-stu-id="1480f-114">A production Blazor server-side deployment requires a solution for supporting as many concurrent connections as required by the app.</span></span> <span data-ttu-id="1480f-115">Le [Azure SignalR Service](/azure/azure-signalr/) gère la mise à l’échelle des connexions et est recommandé comme solution de mise à l’échelle pour les applications côté serveur Blazor.</span><span class="sxs-lookup"><span data-stu-id="1480f-115">The [Azure SignalR Service](/azure/azure-signalr/) handles the scaling of connections and is recommended as a scaling solution for Blazor server-side apps.</span></span> <span data-ttu-id="1480f-116">Pour plus d’informations, consultez <xref:signalr/publish-to-azure-web-app>.</span><span class="sxs-lookup"><span data-stu-id="1480f-116">For more information, see <xref:signalr/publish-to-azure-web-app>.</span></span>

## <a name="signalr-configuration"></a><span data-ttu-id="1480f-117">Configuration SignalR</span><span class="sxs-lookup"><span data-stu-id="1480f-117">SignalR configuration</span></span>

<span data-ttu-id="1480f-118">SignalR est configuré par ASP.NET Core pour les scénarios côté serveur Blazor les plus courants.</span><span class="sxs-lookup"><span data-stu-id="1480f-118">SignalR is configured by ASP.NET Core for the most common Blazor server-side scenarios.</span></span> <span data-ttu-id="1480f-119">Pour les scénarios avancés et personnalisés, consultez les articles SignalR dans la section [Ressources supplémentaires](#additional-resources).</span><span class="sxs-lookup"><span data-stu-id="1480f-119">For custom and advanced scenarios, consult the SignalR articles in the [Additional resources](#additional-resources) section.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="1480f-120">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="1480f-120">Additional resources</span></span>

* <xref:signalr/introduction>
* [<span data-ttu-id="1480f-121">Documentation Azure SignalR Service</span><span class="sxs-lookup"><span data-stu-id="1480f-121">Azure SignalR Service Documentation</span></span>](/azure/azure-signalr/)
* [<span data-ttu-id="1480f-122">Démarrage rapide : Créer une salle de conversation à l'aide de SignalR Service</span><span class="sxs-lookup"><span data-stu-id="1480f-122">Quickstart: Create a chat room by using SignalR Service</span></span>](/azure/azure-signalr/signalr-quickstart-dotnet-core)
* <xref:host-and-deploy/index>
* <xref:tutorials/publish-to-azure-webapp-using-vs>
* [<span data-ttu-id="1480f-123">Déployer la préversion d’ASP.NET Core sur Azure App Service</span><span class="sxs-lookup"><span data-stu-id="1480f-123">Deploy ASP.NET Core preview release to Azure App Service</span></span>](xref:host-and-deploy/azure-apps/index#deploy-aspnet-core-preview-release-to-azure-app-service)
