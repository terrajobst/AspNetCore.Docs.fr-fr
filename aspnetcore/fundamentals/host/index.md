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
# <a name="host-in-aspnet-core"></a>Héberger dans ASP.NET Core

Les applications .NET configurent et lancent un *hôte*. L’hôte est responsable de la gestion du démarrage et de la durée de vie des applications. Deux API d’hôte sont disponibles :

* [Hôte web](xref:fundamentals/host/web-host) &ndash; convient à l’hébergement d’applications web.
* [Hôte générique](xref:fundamentals/host/generic-host) (ASP.NET Core 2.1 ou ultérieur) &ndash; convient à l’hébergement d’applications autres que les applications web (par exemple celles qui exécutent des tâches en arrière-plan). Dans une version ultérieure, l’hôte générique conviendra à l’hébergement de tout type d’application, y compris les applications web. L’hôte générique remplacera à terme l’hôte web.

À l’heure actuelle, les développeurs doivent utiliser l’[hôte web](xref:fundamentals/host/web-host) basé sur [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) pour l’hébergement d’applications ASP.NET Core.
