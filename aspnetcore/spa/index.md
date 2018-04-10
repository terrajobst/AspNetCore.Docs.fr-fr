---
title: Utiliser les modèles d’applications monopages avec ASP.NET Core
author: SteveSandersonMS
description: Découvrez comment installer et bien démarrer avec les modèles de projet d’application à page unique ASP.NET Core.
manager: wpickett
ms.author: scaddie
ms.custom: mvc
ms.date: 02/21/2018
ms.devlang: csharp
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: spa/index
ms.openlocfilehash: eda4817de007f3c3184b2ba6ed6c97989ff17da5
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/06/2018
---
# <a name="use-the-single-page-application-templates-with-aspnet-core"></a>Utiliser les modèles d’applications monopages avec ASP.NET Core

> [!NOTE]
> Le kit SDK .NET Core 2.0.x publié inclut des modèles de projet plus anciens pour Angular, React et React avec Redux. Cette documentation ne traite pas de ces modèles de projet plus anciens. Cette documentation concerne les derniers modèles Angular, React et React avec Redux que vous pouvez installer manuellement dans ASP.NET Core 2.0. Les modèles sont inclus par défaut dans ASP.NET Core 2.1.

## <a name="prerequisites"></a>Prérequis

* [!INCLUDE [](~/includes/net-core-sdk-download-link.md)]
* [Node.js](https://nodejs.org), version 6 ou ultérieure

## <a name="installation"></a>Installation

Si vous avez ASP.NET Core 2.0, exécutez la commande suivante pour installer les modèles ASP.NET Core mis à jour pour Angular, React et React avec Redux :

```console
dotnet new --install Microsoft.DotNet.Web.Spa.ProjectTemplates::2.0.0
```

## <a name="use-the-templates"></a>Utiliser les modèles

- [Utiliser le modèle de projet Angular](xref:spa/angular)
- [Utiliser le modèle de projet React](xref:spa/react)
- [Utiliser le modèle de projet React avec Redux](xref:spa/react-with-redux)
