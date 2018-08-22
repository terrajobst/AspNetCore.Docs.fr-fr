---
uid: webhooks/receiving/dependencies
title: Dépendances du récepteur ASP.NET WebHooks | Microsoft Docs
author: rick-anderson
description: Dépendances du récepteur et l’injection de dépendances dans ASP.NET WebHooks.
ms.author: riande
ms.date: 01/17/2012
ms.assetid: 5125e483-c2bb-435b-8cd1-21d3499bfaaf
ms.openlocfilehash: c44cfe3ed310aa728a989b108c410e8786e4f514
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41830525"
---
# <a name="aspnet-webhooks-receiver-dependencies"></a><span data-ttu-id="280a1-103">Dépendances du récepteur WebHooks ASP.NET</span><span class="sxs-lookup"><span data-stu-id="280a1-103">ASP.NET WebHooks receiver dependencies</span></span>

<span data-ttu-id="280a1-104">Microsoft ASP.NET WebHooks est conçu avec l’injection de dépendance à l’esprit.</span><span class="sxs-lookup"><span data-stu-id="280a1-104">Microsoft ASP.NET WebHooks is designed with dependency injection in mind.</span></span> <span data-ttu-id="280a1-105">La plupart des dépendances dans le système peut être remplacé par d’autres implémentations à l’aide d’un moteur de l’injection de dépendance.</span><span class="sxs-lookup"><span data-stu-id="280a1-105">Most dependencies in the system can be replaced with alternative implementations using a dependency injection engine.</span></span>

<span data-ttu-id="280a1-106">Consultez [DependencyScopeExtensions](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/Extensions/DependencyScopeExtensions.cs) pour obtenir la liste de dépendances du récepteur.</span><span class="sxs-lookup"><span data-stu-id="280a1-106">Please see [DependencyScopeExtensions](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/Extensions/DependencyScopeExtensions.cs) for a list of receiver dependencies.</span></span> <span data-ttu-id="280a1-107">Si aucune dépendance n’a été inscrit, une implémentation par défaut est utilisée.</span><span class="sxs-lookup"><span data-stu-id="280a1-107">If no dependency has been registered, a default implementation is used.</span></span> <span data-ttu-id="280a1-108">Consultez [ReceiverServices](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/Services/ReceiverServices.cs) pour obtenir la liste des implémentations par défaut.</span><span class="sxs-lookup"><span data-stu-id="280a1-108">Please see [ReceiverServices](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/Services/ReceiverServices.cs) for a list of default implementations.</span></span>
