---
title: Plates-formes ASP.NET Core éblouissantes prises en charge
author: guardrex
description: En savoir plus sur les plateformes prises en charge pour ASP.NET Core éblouissant.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 09/23/2019
uid: blazor/supported-platforms
ms.openlocfilehash: b769ee175cde7c9a613d7fb70949de129ca428d3
ms.sourcegitcommit: 79eeb17604b536e8f34641d1e6b697fb9a2ee21f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/24/2019
ms.locfileid: "71211566"
---
# <a name="aspnet-core-blazor-supported-platforms"></a>Plates-formes ASP.NET Core éblouissantes prises en charge

Par [Luke Latham](https://github.com/guardrex)

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

## <a name="browser-requirements"></a>Exigences liées au navigateur

### <a name="blazor-webassembly"></a>WebAssembly Blazor

| Visiteur                          | Version               |
| -------------------------------- | :-------------------: |
| Microsoft Edge                   | Actuelle               |
| Mozilla Firefox                  | Actuelle               |
| Google Chrome, y compris Android | Actuelle               |
| Safari, y compris iOS            | Actuelle               |
| Microsoft Internet Explorer      | Non pris en charge&dagger; |

&dagger;Microsoft Internet Explorer ne prend pas en charge [webassembly](https://webassembly.org).

### <a name="blazor-server"></a>Serveur Blazor

| Visiteur                          | Version    |
| -------------------------------- | :--------: |
| Microsoft Edge                   | Actuelle    |
| Mozilla Firefox                  | Actuelle    |
| Google Chrome, y compris Android | Actuelle    |
| Safari, y compris iOS            | Actuelle    |
| Microsoft Internet Explorer      | 11&dagger; |

&dagger;Des polyremplissages supplémentaires sont nécessaires (par exemple, des promesses peuvent être ajoutées via un bundle [Polyfill.IO](https://polyfill.io/v3/) ).

## <a name="additional-resources"></a>Ressources supplémentaires

* <xref:blazor/hosting-models>
