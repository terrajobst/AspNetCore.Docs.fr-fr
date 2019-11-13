---
title: Plateformes prises en charge par ASP.NET Core Blazor
author: guardrex
description: En savoir plus sur les plateformes prises en charge pour ASP.NET Core Blazor.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 10/15/2019
no-loc:
- Blazor
uid: blazor/supported-platforms
ms.openlocfilehash: de51296cc8785474e1c1406cfd5d4e5bd4050172
ms.sourcegitcommit: 3fc3020961e1289ee5bf5f3c365ce8304d8ebf19
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/12/2019
ms.locfileid: "73962732"
---
# <a name="aspnet-core-opno-locblazor-supported-platforms"></a>Plateformes prises en charge par ASP.NET Core Blazor

Par [Luke Latham](https://github.com/guardrex)

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

## <a name="browser-requirements"></a>Contraintes liées au navigateur

### <a name="opno-locblazor-webassembly"></a>Blazor webassembly

| Visiteur                          | Version               |
| -------------------------------- | :-------------------: |
| Microsoft Edge                   | Actuelle               |
| Mozilla Firefox                  | Actuelle               |
| Google Chrome, y compris Android | Actuelle               |
| Safari, y compris iOS            | Actuelle               |
| Microsoft Internet Explorer      | &dagger; non pris en charge |

&dagger;Microsoft Internet Explorer ne prend pas en charge [Webassembly](https://webassembly.org).

### <a name="opno-locblazor-server"></a>Serveur de Blazor

| Visiteur                          | Version    |
| -------------------------------- | :--------: |
| Microsoft Edge                   | Actuelle    |
| Mozilla Firefox                  | Actuelle    |
| Google Chrome, y compris Android | Actuelle    |
| Safari, y compris iOS            | Actuelle    |
| Microsoft Internet Explorer      | 11&dagger; |

&dagger;des polyremplissages supplémentaires sont nécessaires (par exemple, des promesses peuvent être ajoutées via un bundle [Polyfill.IO](https://polyfill.io/v3/) ).

## <a name="additional-resources"></a>Ressources supplémentaires

* <xref:blazor/hosting-models>
