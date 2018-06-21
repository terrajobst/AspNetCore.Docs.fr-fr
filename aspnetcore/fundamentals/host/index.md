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
# <a name="host-in-aspnet-core"></a>Héberger dans ASP.NET Core

Les applications .NET configurent et lancent un *hôte*. L’hôte est responsable de la gestion du démarrage et de la durée de vie des applications. Deux API d’hôte sont disponibles :

* [Hôte web](xref:fundamentals/host/web-host) &ndash; convient à l’hébergement d’applications web.
* [Hôte générique](xref:fundamentals/host/generic-host) (ASP.NET Core 2.1 ou ultérieur) &ndash; convient à l’hébergement d’applications autres que les applications web (par exemple celles qui exécutent des tâches en arrière-plan). Dans une version ultérieure, l’hôte générique conviendra à l’hébergement de tout type d’application, y compris les applications web. L’hôte générique remplacera à terme l’hôte web.

Pour l’hébergement des *applications web* ASP.NET Core, les développeurs doivent utiliser l’hôte web basé sur [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder). Pour l’hébergement des *applications non-web*, les développeurs doivent utiliser l’hôte générique basé sur [HostBuilder](/dotnet/api/microsoft.extensions.hosting.hostbuilder).
