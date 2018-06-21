---
title: Utiliser les modèles d’applications monopages avec ASP.NET Core
author: SteveSandersonMS
description: Découvrez comment installer et bien démarrer avec les modèles de projet d’application à page unique ASP.NET Core.
monikerRange: '>= aspnetcore-2.0'
ms.author: scaddie
ms.custom: mvc
ms.date: 02/21/2018
uid: spa/index
ms.openlocfilehash: ab164ae5d2df47739ec04b32cd21835ffdf9f87f
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/20/2018
ms.locfileid: "36291442"
---
# <a name="use-the-single-page-application-templates-with-aspnet-core"></a><span data-ttu-id="b2f00-103">Utiliser les modèles d’applications monopages avec ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="b2f00-103">Use the Single Page Application templates with ASP.NET Core</span></span>

::: moniker range="= aspnetcore-2.0"

> [!NOTE]
> <span data-ttu-id="b2f00-104">Le kit SDK .NET Core 2.0.x publié inclut des modèles de projet plus anciens pour Angular, React et React avec Redux.</span><span class="sxs-lookup"><span data-stu-id="b2f00-104">The released .NET Core 2.0.x SDK includes older project templates for Angular, React, and React with Redux.</span></span> <span data-ttu-id="b2f00-105">Cette documentation ne traite pas de ces modèles de projet plus anciens.</span><span class="sxs-lookup"><span data-stu-id="b2f00-105">This documentation isn't about those older project templates.</span></span> <span data-ttu-id="b2f00-106">Cette documentation concerne les derniers modèles Angular, React et React avec Redux que vous pouvez installer manuellement dans ASP.NET Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="b2f00-106">This documentation is for the latest Angular, React, and React with Redux templates, which can be installed manually into ASP.NET Core 2.0.</span></span> <span data-ttu-id="b2f00-107">Les modèles sont inclus par défaut dans ASP.NET Core 2.1.</span><span class="sxs-lookup"><span data-stu-id="b2f00-107">The templates are included by default with ASP.NET Core 2.1.</span></span>

::: moniker-end

## <a name="prerequisites"></a><span data-ttu-id="b2f00-108">Prérequis</span><span class="sxs-lookup"><span data-stu-id="b2f00-108">Prerequisites</span></span>

* [!INCLUDE [](~/includes/net-core-sdk-download-link.md)]
* <span data-ttu-id="b2f00-109">[Node.js](https://nodejs.org), version 6 ou ultérieure</span><span class="sxs-lookup"><span data-stu-id="b2f00-109">[Node.js](https://nodejs.org), version 6 or later</span></span>

## <a name="installation"></a><span data-ttu-id="b2f00-110">Installation</span><span class="sxs-lookup"><span data-stu-id="b2f00-110">Installation</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="b2f00-111">Les modèles sont déjà installés avec ASP.NET Core 2.1.</span><span class="sxs-lookup"><span data-stu-id="b2f00-111">The templates are already installed with ASP.NET Core 2.1.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="b2f00-112">Si vous avez ASP.NET Core 2.0, exécutez la commande suivante pour installer les modèles ASP.NET Core mis à jour pour Angular, React et React avec Redux :</span><span class="sxs-lookup"><span data-stu-id="b2f00-112">If you have ASP.NET Core 2.0, run the following command to install the updated ASP.NET Core templates for Angular, React, and React with Redux:</span></span>

```console
dotnet new --install Microsoft.DotNet.Web.Spa.ProjectTemplates::2.0.0
```

::: moniker-end

## <a name="use-the-templates"></a><span data-ttu-id="b2f00-113">Utiliser les modèles</span><span class="sxs-lookup"><span data-stu-id="b2f00-113">Use the templates</span></span>

* [<span data-ttu-id="b2f00-114">Utiliser le modèle de projet Angular</span><span class="sxs-lookup"><span data-stu-id="b2f00-114">Use the Angular project template</span></span>](xref:spa/angular)
* [<span data-ttu-id="b2f00-115">Utiliser le modèle de projet React</span><span class="sxs-lookup"><span data-stu-id="b2f00-115">Use the React project template</span></span>](xref:spa/react)
* [<span data-ttu-id="b2f00-116">Utiliser le modèle de projet React avec Redux</span><span class="sxs-lookup"><span data-stu-id="b2f00-116">Use the React with Redux project template</span></span>](xref:spa/react-with-redux)
