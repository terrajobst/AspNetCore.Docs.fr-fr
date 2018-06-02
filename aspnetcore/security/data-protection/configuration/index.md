---
title: Configuration de la protection des données dans ASP.NET Core
author: rick-anderson
description: Consultez les rubriques qui expliquent comment configurer la protection des données dans ASP.NET Core.
manager: wpickett
ms.author: riande
ms.date: 10/12/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/data-protection/configuration/index
ms.openlocfilehash: 74fb5de465a740303872cbbc79ed2d3fafb1eea0
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/22/2018
---
# <a name="data-protection-configuration-in-aspnet-core"></a>Configuration de la protection des données dans ASP.NET Core

Consultez les rubriques suivantes pour en savoir plus sur la configuration de la protection des données dans ASP.NET Core :

* [Configurer la protection des données ASP.NET Core](xref:security/data-protection/configuration/overview)  
  Vue d’ensemble de la configuration de la protection des données ASP.NET Core.

* [Gestion et durée de vie des clés de protection des données](xref:security/data-protection/configuration/default-settings)  
  Informations sur la gestion et la durée de vie des clés de protection des données.

* [Prise en charge de la stratégie au niveau de l’ordinateur de protection des données](xref:security/data-protection/configuration/machine-wide-policy)  
  Informations sur la définition d’une stratégie par défaut au niveau de l’ordinateur pour toutes les applications qui utilisent la protection des données.

* [Scénarios non compatibles avec l’injection de dépendances pour la protection des données dans ASP.NET Core](xref:security/data-protection/configuration/non-di-scenarios)  
  Indique comment utiliser le type concret [DataProtectionProvider](/dotnet/api/Microsoft.AspNetCore.DataProtection.DataProtectionProvider) pour utiliser la protection des données sans passer par des chemins de code spécifiques à l’injection de dépendances.
