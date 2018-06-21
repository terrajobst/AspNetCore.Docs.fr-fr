---
title: Héberger dans ASP.NET Core
author: guardrex
description: Découvrez l’hôte web ASP.NET Core et l’hôte générique .NET, qui sont responsables de la gestion du démarrage et de la durée de vie des applications.
ms.author: riande
ms.custom: mvc
ms.date: 05/16/2018
uid: fundamentals/host/index
ms.openlocfilehash: 365c679e789c07818c6eb007f40f6aef43b82c44
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/20/2018
ms.locfileid: "36276615"
---
# <a name="host-in-aspnet-core"></a><span data-ttu-id="9d0c4-103">Héberger dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="9d0c4-103">Host in ASP.NET Core</span></span>

<span data-ttu-id="9d0c4-104">Les applications .NET configurent et lancent un *hôte*.</span><span class="sxs-lookup"><span data-stu-id="9d0c4-104">.NET apps configure and launch a *host*.</span></span> <span data-ttu-id="9d0c4-105">L’hôte est responsable de la gestion du démarrage et de la durée de vie des applications.</span><span class="sxs-lookup"><span data-stu-id="9d0c4-105">The host is responsible for app startup and lifetime management.</span></span> <span data-ttu-id="9d0c4-106">Deux API d’hôte sont disponibles :</span><span class="sxs-lookup"><span data-stu-id="9d0c4-106">Two host APIs are available for use:</span></span>

* <span data-ttu-id="9d0c4-107">[Hôte web](xref:fundamentals/host/web-host) &ndash; convient à l’hébergement d’applications web.</span><span class="sxs-lookup"><span data-stu-id="9d0c4-107">[Web Host](xref:fundamentals/host/web-host) &ndash; Suitable for hosting web apps.</span></span>
* <span data-ttu-id="9d0c4-108">[Hôte générique](xref:fundamentals/host/generic-host) (ASP.NET Core 2.1 ou ultérieur) &ndash; convient à l’hébergement d’applications autres que les applications web (par exemple celles qui exécutent des tâches en arrière-plan).</span><span class="sxs-lookup"><span data-stu-id="9d0c4-108">[Generic Host](xref:fundamentals/host/generic-host) (ASP.NET Core 2.1 or later) &ndash; Suitable for hosting non-web apps (for example, apps that run background tasks).</span></span> <span data-ttu-id="9d0c4-109">Dans une version ultérieure, l’hôte générique conviendra à l’hébergement de tout type d’application, y compris les applications web.</span><span class="sxs-lookup"><span data-stu-id="9d0c4-109">In a future release, the Generic Host will be suitable for hosting any kind of app, including web apps.</span></span> <span data-ttu-id="9d0c4-110">L’hôte générique remplacera à terme l’hôte web.</span><span class="sxs-lookup"><span data-stu-id="9d0c4-110">The Generic Host will eventually replace the Web Host.</span></span>

<span data-ttu-id="9d0c4-111">Pour l’hébergement des *applications web* ASP.NET Core, les développeurs doivent utiliser l’hôte web basé sur [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder).</span><span class="sxs-lookup"><span data-stu-id="9d0c4-111">For hosting ASP.NET Core *web apps*, developers should use the Web Host based on [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder).</span></span> <span data-ttu-id="9d0c4-112">Pour l’hébergement des *applications non-web*, les développeurs doivent utiliser l’hôte générique basé sur [HostBuilder](/dotnet/api/microsoft.extensions.hosting.hostbuilder).</span><span class="sxs-lookup"><span data-stu-id="9d0c4-112">For hosting *non-web apps*, developers should use the Generic Host based on [HostBuilder](/dotnet/api/microsoft.extensions.hosting.hostbuilder).</span></span>
