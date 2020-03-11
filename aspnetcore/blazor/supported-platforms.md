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
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78658857"
---
# <a name="aspnet-core-blazor-supported-platforms"></a>Plates-formes ASP.NET Core éblouissantes prises en charge

Par [Luke Latham](https://github.com/guardrex)

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

## <a name="browser-requirements"></a>Configuration requise des navigateurs

### <a name="blazor-webassembly"></a>WebAssembly Blazor

| Browser                          | Version               |
| -------------------------------- | :-------------------: |
| Microsoft Edge                   | Current               |
| Mozilla Firefox                  | Current               |
| Google Chrome, y compris Android | Current               |
| Safari, y compris iOS            | Current               |
| Microsoft Internet Explorer      | &dagger; non pris en charge |

&dagger;Microsoft Internet Explorer ne prend pas en charge [Webassembly](https://webassembly.org).

### <a name="blazor-server"></a>Serveur Blazor

| Browser                          | Version    |
| -------------------------------- | :--------: |
| Microsoft Edge                   | Current    |
| Mozilla Firefox                  | Current    |
| Google Chrome, y compris Android | Current    |
| Safari, y compris iOS            | Current    |
| Microsoft Internet Explorer      | 11&dagger; |

&dagger;des polyremplissages supplémentaires sont nécessaires (par exemple, des promesses peuvent être ajoutées via un bundle [Polyfill.IO](https://polyfill.io/v3/) ).

## <a name="additional-resources"></a>Ressources supplémentaires

* <xref:blazor/hosting-models>
