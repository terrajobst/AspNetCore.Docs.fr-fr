---
title: Héberger dans ASP.NET Core
author: guardrex
description: Découvrez l’hôte web ASP.NET Core et l’hôte générique .NET, qui sont responsables de la gestion du démarrage et de la durée de vie des applications.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 05/16/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/host/index
ms.openlocfilehash: 7ad059e39866f59040c12b7ac15e9fa3405a9aad
ms.sourcegitcommit: a66f38071e13685bbe59d48d22aa141ac702b432
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/17/2018
---
# <a name="host-in-aspnet-core"></a><span data-ttu-id="6523e-103">Héberger dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="6523e-103">Host in ASP.NET Core</span></span>

<span data-ttu-id="6523e-104">Les applications .NET configurent et lancent un *hôte*.</span><span class="sxs-lookup"><span data-stu-id="6523e-104">.NET apps configure and launch a *host*.</span></span> <span data-ttu-id="6523e-105">L’hôte est responsable de la gestion du démarrage et de la durée de vie des applications.</span><span class="sxs-lookup"><span data-stu-id="6523e-105">The host is responsible for app startup and lifetime management.</span></span> <span data-ttu-id="6523e-106">Deux API d’hôte sont disponibles :</span><span class="sxs-lookup"><span data-stu-id="6523e-106">Two host APIs are available for use:</span></span>

* <span data-ttu-id="6523e-107">[Hôte web](xref:fundamentals/host/web-host) &ndash; convient à l’hébergement d’applications web.</span><span class="sxs-lookup"><span data-stu-id="6523e-107">[Web Host](xref:fundamentals/host/web-host) &ndash; Suitable for hosting web apps.</span></span>
* <span data-ttu-id="6523e-108">[Hôte générique](xref:fundamentals/host/generic-host) (ASP.NET Core 2.1 ou ultérieur) &ndash; convient à l’hébergement d’applications autres que les applications web (par exemple celles qui exécutent des tâches en arrière-plan).</span><span class="sxs-lookup"><span data-stu-id="6523e-108">[Generic Host](xref:fundamentals/host/generic-host) (ASP.NET Core 2.1 or later) &ndash; Suitable for hosting non-web apps (for example, apps that run background tasks).</span></span> <span data-ttu-id="6523e-109">Dans une version ultérieure, l’hôte générique conviendra à l’hébergement de tout type d’application, y compris les applications web.</span><span class="sxs-lookup"><span data-stu-id="6523e-109">In a future release, the Generic Host will be suitable for hosting any kind of app, including web apps.</span></span> <span data-ttu-id="6523e-110">L’hôte générique remplacera à terme l’hôte web.</span><span class="sxs-lookup"><span data-stu-id="6523e-110">The Generic Host will eventually replace the Web Host.</span></span>

<span data-ttu-id="6523e-111">À l’heure actuelle, les développeurs doivent utiliser l’[hôte web](xref:fundamentals/host/web-host) basé sur [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) pour l’hébergement d’applications ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="6523e-111">At this time, developers should use the [Web Host](xref:fundamentals/host/web-host) based on [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) for hosting ASP.NET Core apps.</span></span>
