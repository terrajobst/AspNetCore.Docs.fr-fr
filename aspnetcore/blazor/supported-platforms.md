---
title: Plates-formes ASP.NET Core éblouissantes prises en charge
author: guardrex
description: En savoir plus sur les plateformes prises en charge pour ASP.NET Core éblouissant.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 07/01/2019
uid: blazor/supported-platforms
ms.openlocfilehash: 042fbb1b2c7f92b7dc6443319f3f195a12a55adc
ms.sourcegitcommit: 092061c4f6ef46ed2165fa84de6273d3786fb97e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/13/2019
ms.locfileid: "70963882"
---
# <a name="aspnet-core-blazor-supported-platforms"></a>Plates-formes ASP.NET Core éblouissantes prises en charge

Par [Luke Latham](https://github.com/guardrex)

## <a name="browser-requirements"></a>Exigences liées au navigateur

### <a name="blazor-webassembly"></a>Webassembly éblouissant

| Visiteur                          | Version               |
| -------------------------------- | :-------------------: |
| Microsoft Edge                   | Actuelle               |
| Mozilla Firefox                  | Actuelle               |
| Google Chrome, y compris Android | Actuelle               |
| Safari, y compris iOS            | Actuelle               |
| Microsoft Internet Explorer      | Non pris en charge&dagger; |

&dagger;Microsoft Internet Explorer ne prend pas en charge [webassembly](https://webassembly.org).

### <a name="blazor-server"></a>Serveur éblouissant

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
