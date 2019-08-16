---
title: Plates-formes ASP.NET Core éblouissantes prises en charge
author: guardrex
description: En savoir plus sur les plateformes prises en charge pour ASP.NET Core éblouissant.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 07/01/2019
uid: blazor/supported-platforms
ms.openlocfilehash: 01f3a55a8536feedf713e07ea3724a0bc51e7c63
ms.sourcegitcommit: 7a40c56bf6a6aaa63a7ee83a2cac9b3a1d77555e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/12/2019
ms.locfileid: "68948249"
---
# <a name="aspnet-core-blazor-supported-platforms"></a>Plates-formes ASP.NET Core éblouissantes prises en charge

Par [Luke Latham](https://github.com/guardrex)

## <a name="browser-requirements"></a>Exigences liées au navigateur

### <a name="blazor-client-side"></a>Blazor côté client

| Visiteur                          | Version               |
| -------------------------------- | :-------------------: |
| Microsoft Edge                   | Actuelle               |
| Mozilla Firefox                  | Actuelle               |
| Google Chrome, y compris Android | Actuelle               |
| Safari, y compris iOS            | Actuelle               |
| Microsoft Internet Explorer      | Non pris en charge&dagger; |

&dagger;Microsoft Internet Explorer ne prend pas en charge [webassembly](https://webassembly.org).

### <a name="blazor-server-side"></a>Blazor côté serveur

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
