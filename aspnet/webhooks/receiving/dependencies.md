---
uid: webhooks/receiving/dependencies
title: Dépendances de récepteur ASP.NET WebHooks | Documents Microsoft
author: rick-anderson
description: Dépendances du récepteur et injection de dépendances dans ASP.NET WebHooks.
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/17/2012
ms.topic: article
ms.assetid: 5125e483-c2bb-435b-8cd1-21d3499bfaaf
ms.technology: ''
ms.prod: .net-framework
ms.openlocfilehash: f9726c746c8934594e26f2871f9b867c192374bb
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/10/2017
ms.locfileid: "26529908"
---
# <a name="aspnet-webhooks-receiver-dependencies"></a><span data-ttu-id="2f127-103">Dépendances de récepteur ASP.NET WebHooks</span><span class="sxs-lookup"><span data-stu-id="2f127-103">ASP.NET WebHooks receiver dependencies</span></span>

<span data-ttu-id="2f127-104">Microsoft ASP.NET WebHooks est conçue avec l’injection de dépendances à l’esprit.</span><span class="sxs-lookup"><span data-stu-id="2f127-104">Microsoft ASP.NET WebHooks is designed with dependency injection in mind.</span></span> <span data-ttu-id="2f127-105">La plupart des dépendances dans le système peut être remplacé par les implémentations alternatives à l’aide d’un moteur d’injection de dépendance.</span><span class="sxs-lookup"><span data-stu-id="2f127-105">Most dependencies in the system can be replaced with alternative implementations using a dependency injection engine.</span></span>

<span data-ttu-id="2f127-106">Consultez [DependencyScopeExtensions](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/Extensions/DependencyScopeExtensions.cs) pour obtenir la liste de dépendances de récepteur.</span><span class="sxs-lookup"><span data-stu-id="2f127-106">Please see [DependencyScopeExtensions](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/Extensions/DependencyScopeExtensions.cs) for a list of receiver dependencies.</span></span> <span data-ttu-id="2f127-107">Si aucune dépendance n’a été inscrit, une implémentation par défaut est utilisée.</span><span class="sxs-lookup"><span data-stu-id="2f127-107">If no dependency has been registered, a default implementation is used.</span></span> <span data-ttu-id="2f127-108">Consultez [ReceiverServices](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/Services/ReceiverServices.cs) pour obtenir la liste des implémentations par défaut.</span><span class="sxs-lookup"><span data-stu-id="2f127-108">Please see [ReceiverServices](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/Services/ReceiverServices.cs) for a list of default implementations.</span></span>
