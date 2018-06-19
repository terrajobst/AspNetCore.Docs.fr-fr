---
title: Utiliser les modèles d’applications monopages avec ASP.NET Core
author: SteveSandersonMS
description: Découvrez comment installer et bien démarrer avec les modèles de projet d’application à page unique ASP.NET Core.
manager: wpickett
monikerRange: '>= aspnetcore-2.0'
ms.author: scaddie
ms.custom: mvc
ms.date: 02/21/2018
ms.devlang: csharp
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: spa/index
ms.openlocfilehash: f7bb23e9001c7606c3e622bf4575a4debec56ccd
ms.sourcegitcommit: 466300d32f8c33e64ee1b419a2cbffe702863cdf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/27/2018
ms.locfileid: "34555558"
---
# <a name="use-the-single-page-application-templates-with-aspnet-core"></a>Utiliser les modèles d’applications monopages avec ASP.NET Core

::: moniker range="= aspnetcore-2.0"

> [!NOTE]
> Le kit SDK .NET Core 2.0.x publié inclut des modèles de projet plus anciens pour Angular, React et React avec Redux. Cette documentation ne traite pas de ces modèles de projet plus anciens. Cette documentation concerne les derniers modèles Angular, React et React avec Redux que vous pouvez installer manuellement dans ASP.NET Core 2.0. Les modèles sont inclus par défaut dans ASP.NET Core 2.1.

::: moniker-end

## <a name="prerequisites"></a>Prérequis

* [!INCLUDE [](~/includes/net-core-sdk-download-link.md)]
* [Node.js](https://nodejs.org), version 6 ou ultérieure

## <a name="installation"></a>Installation

::: moniker range=">= aspnetcore-2.1"

Les modèles sont déjà installés avec ASP.NET Core 2.1.

::: moniker-end

::: moniker range="= aspnetcore-2.0"

Si vous avez ASP.NET Core 2.0, exécutez la commande suivante pour installer les modèles ASP.NET Core mis à jour pour Angular, React et React avec Redux :

```console
dotnet new --install Microsoft.DotNet.Web.Spa.ProjectTemplates::2.0.0
```

::: moniker-end

## <a name="use-the-templates"></a>Utiliser les modèles

* [Utiliser le modèle de projet Angular](xref:spa/angular)
* [Utiliser le modèle de projet React](xref:spa/react)
* [Utiliser le modèle de projet React avec Redux](xref:spa/react-with-redux)
