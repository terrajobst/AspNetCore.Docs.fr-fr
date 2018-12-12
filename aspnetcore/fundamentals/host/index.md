---
title: Hôte web et hôte générique dans ASP.NET Core
author: guardrex
description: Découvrez l’hôte web ASP.NET Core et l’hôte générique .NET, qui sont responsables de la gestion du démarrage et de la durée de vie des applications.
ms.author: riande
ms.custom: mvc, seodec18
ms.date: 08/28/2018
uid: fundamentals/host/index
ms.openlocfilehash: 43a68f67368ce37a1fb4032579c6c4e4c05d2719
ms.sourcegitcommit: b34b25da2ab68e6495b2460ff570468f16a9bf0d
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2018
ms.locfileid: "53284576"
---
# <a name="web-host-and-generic-host-in-aspnet-core"></a>Hôte web et hôte générique dans ASP.NET Core

Les applications .NET configurent et lancent un *hôte*. L’hôte est responsable de la gestion du démarrage et de la durée de vie des applications. Deux API d’hôte sont disponibles :

* [Hôte web](xref:fundamentals/host/web-host) &ndash; convient à l’hébergement d’applications web.
* [Hôte générique](xref:fundamentals/host/generic-host) (ASP.NET Core 2.1 ou ultérieur) &ndash; convient à l’hébergement d’applications autres que les applications web (par exemple celles qui exécutent des tâches en arrière-plan). Dans une version ultérieure, l’hôte générique conviendra à l’hébergement de tout type d’application, y compris les applications web. L’hôte générique remplacera à terme l’hôte web.

Pour l’hébergement des *applications web* ASP.NET Core, les développeurs doivent utiliser l’hôte web basé sur <xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder>. Pour l’hébergement des *applications non-web*, les développeurs doivent utiliser l’hôte générique basé sur <xref:Microsoft.Extensions.Hosting.HostBuilder>.

<xref:fundamentals/host/hosted-services>  
Découvrez comment implémenter des tâches d’arrière-plan avec des services hébergés dans ASP.NET Core.

<xref:fundamentals/configuration/platform-specific-configuration>  
Découvrez comment améliorer une application ASP.NET Core à partir d’un assembly référencé ou non référencé à l’aide d’une implémentation <xref:Microsoft.AspNetCore.Hosting.IHostingStartup>.
