---
title: Héberger dans ASP.NET Core
author: guardrex
description: Découvrez l’hôte web ASP.NET Core et l’hôte générique .NET, qui sont responsables de la gestion du démarrage et de la durée de vie des applications.
ms.author: riande
ms.custom: mvc
ms.date: 08/28/2018
uid: fundamentals/host/index
ms.openlocfilehash: 9927722b5080beb94e5628d9e7b54e6d50a5bff8
ms.sourcegitcommit: a669c4e3f42e387e214a354ac4143555602e6f66
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2018
ms.locfileid: "43336048"
---
# <a name="host-in-aspnet-core"></a><span data-ttu-id="373fe-103">Héberger dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="373fe-103">Host in ASP.NET Core</span></span>

<span data-ttu-id="373fe-104">Les applications .NET configurent et lancent un *hôte*.</span><span class="sxs-lookup"><span data-stu-id="373fe-104">.NET apps configure and launch a *host*.</span></span> <span data-ttu-id="373fe-105">L’hôte est responsable de la gestion du démarrage et de la durée de vie des applications.</span><span class="sxs-lookup"><span data-stu-id="373fe-105">The host is responsible for app startup and lifetime management.</span></span> <span data-ttu-id="373fe-106">Deux API d’hôte sont disponibles :</span><span class="sxs-lookup"><span data-stu-id="373fe-106">Two host APIs are available for use:</span></span>

* <span data-ttu-id="373fe-107">[Hôte web](xref:fundamentals/host/web-host) &ndash; convient à l’hébergement d’applications web.</span><span class="sxs-lookup"><span data-stu-id="373fe-107">[Web Host](xref:fundamentals/host/web-host) &ndash; Suitable for hosting web apps.</span></span>
* <span data-ttu-id="373fe-108">[Hôte générique](xref:fundamentals/host/generic-host) (ASP.NET Core 2.1 ou ultérieur) &ndash; convient à l’hébergement d’applications autres que les applications web (par exemple celles qui exécutent des tâches en arrière-plan).</span><span class="sxs-lookup"><span data-stu-id="373fe-108">[Generic Host](xref:fundamentals/host/generic-host) (ASP.NET Core 2.1 or later) &ndash; Suitable for hosting non-web apps (for example, apps that run background tasks).</span></span> <span data-ttu-id="373fe-109">Dans une version ultérieure, l’hôte générique conviendra à l’hébergement de tout type d’application, y compris les applications web.</span><span class="sxs-lookup"><span data-stu-id="373fe-109">In a future release, the Generic Host will be suitable for hosting any kind of app, including web apps.</span></span> <span data-ttu-id="373fe-110">L’hôte générique remplacera à terme l’hôte web.</span><span class="sxs-lookup"><span data-stu-id="373fe-110">The Generic Host will eventually replace the Web Host.</span></span>

<span data-ttu-id="373fe-111">Pour l’hébergement des *applications web* ASP.NET Core, les développeurs doivent utiliser l’hôte web basé sur <xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder>.</span><span class="sxs-lookup"><span data-stu-id="373fe-111">For hosting ASP.NET Core *web apps*, developers should use the Web Host based on <xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder>.</span></span> <span data-ttu-id="373fe-112">Pour l’hébergement des *applications non-web*, les développeurs doivent utiliser l’hôte générique basé sur <xref:Microsoft.Extensions.Hosting.HostBuilder>.</span><span class="sxs-lookup"><span data-stu-id="373fe-112">For hosting *non-web apps*, developers should use the Generic Host based on <xref:Microsoft.Extensions.Hosting.HostBuilder>.</span></span>

<xref:fundamentals/host/hosted-services>  
<span data-ttu-id="373fe-113">Découvrez comment implémenter des tâches d’arrière-plan avec des services hébergés dans ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="373fe-113">Learn how to implement background tasks with hosted services in ASP.NET Core.</span></span>

<xref:fundamentals/configuration/platform-specific-configuration>  
<span data-ttu-id="373fe-114">Découvrez comment améliorer une application ASP.NET Core à partir d’un assembly référencé ou non référencé à l’aide d’une implémentation <xref:Microsoft.AspNetCore.Hosting.IHostingStartup>.</span><span class="sxs-lookup"><span data-stu-id="373fe-114">Discover how to enhance an ASP.NET Core app from a referenced or unreferenced assembly using an <xref:Microsoft.AspNetCore.Hosting.IHostingStartup> implementation.</span></span>
