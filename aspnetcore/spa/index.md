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
# <a name="use-the-single-page-application-templates-with-aspnet-core"></a><span data-ttu-id="a73e7-103">Utiliser les modèles d’applications monopages avec ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="a73e7-103">Use the Single Page Application templates with ASP.NET Core</span></span>

> [!NOTE]
> <span data-ttu-id="a73e7-104">Le kit SDK .NET Core 2.0.x publié inclut des modèles de projet plus anciens pour Angular, React et React avec Redux.</span><span class="sxs-lookup"><span data-stu-id="a73e7-104">The released .NET Core 2.0.x SDK includes older project templates for Angular, React, and React with Redux.</span></span> <span data-ttu-id="a73e7-105">Cette documentation ne traite pas de ces modèles de projet plus anciens.</span><span class="sxs-lookup"><span data-stu-id="a73e7-105">This documentation isn't about those older project templates.</span></span> <span data-ttu-id="a73e7-106">Cette documentation concerne les derniers modèles Angular, React et React avec Redux que vous pouvez installer manuellement dans ASP.NET Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="a73e7-106">This documentation is for the latest Angular, React, and React with Redux templates, which can be installed manually into ASP.NET Core 2.0.</span></span> <span data-ttu-id="a73e7-107">Les modèles sont inclus par défaut dans ASP.NET Core 2.1.</span><span class="sxs-lookup"><span data-stu-id="a73e7-107">The templates are included by default with ASP.NET Core 2.1.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="a73e7-108">Prérequis</span><span class="sxs-lookup"><span data-stu-id="a73e7-108">Prerequisites</span></span>

* [!INCLUDE [](~/includes/net-core-sdk-download-link.md)]
* <span data-ttu-id="a73e7-109">[Node.js](https://nodejs.org), version 6 ou ultérieure</span><span class="sxs-lookup"><span data-stu-id="a73e7-109">[Node.js](https://nodejs.org), version 6 or later</span></span>

## <a name="installation"></a><span data-ttu-id="a73e7-110">Installation</span><span class="sxs-lookup"><span data-stu-id="a73e7-110">Installation</span></span>

<span data-ttu-id="a73e7-111">Si vous avez ASP.NET Core 2.0, exécutez la commande suivante pour installer les modèles ASP.NET Core mis à jour pour Angular, React et React avec Redux :</span><span class="sxs-lookup"><span data-stu-id="a73e7-111">If you have ASP.NET Core 2.0, run the following command to install the updated ASP.NET Core templates for Angular, React, and React with Redux:</span></span>

```console
dotnet new --install Microsoft.DotNet.Web.Spa.ProjectTemplates::2.0.0
```

## <a name="use-the-templates"></a><span data-ttu-id="a73e7-112">Utiliser les modèles</span><span class="sxs-lookup"><span data-stu-id="a73e7-112">Use the templates</span></span>

- [<span data-ttu-id="a73e7-113">Utiliser le modèle de projet Angular</span><span class="sxs-lookup"><span data-stu-id="a73e7-113">Use the Angular project template</span></span>](xref:spa/angular)
- [<span data-ttu-id="a73e7-114">Utiliser le modèle de projet React</span><span class="sxs-lookup"><span data-stu-id="a73e7-114">Use the React project template</span></span>](xref:spa/react)
- [<span data-ttu-id="a73e7-115">Utiliser le modèle de projet React avec Redux</span><span class="sxs-lookup"><span data-stu-id="a73e7-115">Use the React with Redux project template</span></span>](xref:spa/react-with-redux)
