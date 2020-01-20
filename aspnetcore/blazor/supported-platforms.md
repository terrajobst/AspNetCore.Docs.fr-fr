---
title: Plateformes prises en charge par ASP.NET Core Blazor
author: guardrex
description: En savoir plus sur les plateformes prises en charge pour ASP.NET Core Blazor.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 12/18/2019
no-loc:
- Blazor
- SignalR
uid: blazor/supported-platforms
ms.openlocfilehash: 505974280b5c96ec2bcae42c6e076ab67a15bb07
ms.sourcegitcommit: 9ee99300a48c810ca6fd4f7700cd95c3ccb85972
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/17/2020
ms.locfileid: "76160130"
---
# <a name="aspnet-core-opno-locblazor-supported-platforms"></a>Plateformes prises en charge par ASP.NET Core Blazor

Par [Luke Latham](https://github.com/guardrex)

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

## <a name="browser-requirements"></a>Contraintes liées au navigateur

### <a name="opno-locblazor-webassembly"></a>Blazor webassembly

| Navigateur                          | Version               |
| -------------------------------- | :-------------------: |
| Microsoft Edge                   | Actuelle               |
| Mozilla Firefox                  | Actuelle               |
| Google Chrome, y compris Android | Actuelle               |
| Safari, y compris iOS            | Actuelle               |
| Microsoft Internet Explorer      | &dagger; non pris en charge |

&dagger;Microsoft Internet Explorer ne prend pas en charge [webassembly](https://webassembly.org).

### <a name="opno-locblazor-server"></a>Serveur de Blazor

| Navigateur                          | Version    |
| -------------------------------- | :--------: |
| Microsoft Edge                   | Actuelle    |
| Mozilla Firefox                  | Actuelle    |
| Google Chrome, y compris Android | Actuelle    |
| Safari, y compris iOS            | Actuelle    |
| Microsoft Internet Explorer      | 11&dagger; |

&dagger;des polyremplissages supplémentaires sont nécessaires (par exemple, des promesses peuvent être ajoutées via un bundle [Polyfill.IO](https://polyfill.io/v3/) ).

## <a name="additional-resources"></a>Ressources supplémentaires

* <xref:blazor/hosting-models>
