---
uid: webhooks/receiving/dependencies
title: Dépendances du récepteur ASP.NET WebHooks | Microsoft Docs
author: rick-anderson
description: Dépendances du récepteur et l’injection de dépendances dans ASP.NET WebHooks.
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/17/2012
ms.topic: article
ms.assetid: 5125e483-c2bb-435b-8cd1-21d3499bfaaf
ms.technology: ''
ms.openlocfilehash: cf45e3d2e45e4b7882b42d9aa0931e18c08f76b5
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37401182"
---
# <a name="aspnet-webhooks-receiver-dependencies"></a>Dépendances du récepteur WebHooks ASP.NET

Microsoft ASP.NET WebHooks est conçu avec l’injection de dépendance à l’esprit. La plupart des dépendances dans le système peut être remplacé par d’autres implémentations à l’aide d’un moteur de l’injection de dépendance.

Consultez [DependencyScopeExtensions](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/Extensions/DependencyScopeExtensions.cs) pour obtenir la liste de dépendances du récepteur. Si aucune dépendance n’a été inscrit, une implémentation par défaut est utilisée. Consultez [ReceiverServices](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/Services/ReceiverServices.cs) pour obtenir la liste des implémentations par défaut.
