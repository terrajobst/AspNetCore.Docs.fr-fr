---
uid: visual-studio/overview/2017/optimize-build-perf
title: Optimiser les performances de génération de solution
author: tfitzmac
description: Optimiser les performances de génération de solution
ms.author: riande
ms.date: 08/22/2018
msc.type: authoredcontent
ms.openlocfilehash: 19f190835e7477e69db470b74edac9e211fd9158
ms.sourcegitcommit: 5a2456cbf429069dc48aaa2823cde14100e4c438
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/22/2018
ms.locfileid: "41909878"
---
# <a name="optimize-build-performance-for-solution"></a><span data-ttu-id="bcf77-103">Optimiser les performances de génération de solution</span><span class="sxs-lookup"><span data-stu-id="bcf77-103">Optimize build performance for solution</span></span>
<span data-ttu-id="bcf77-104">15.8 de 2017 Visual Studio et ajoutée ultérieurement un nouvel élément de menu sous **Générer > ASP.NET Compilation > optimiser les performances de génération de Solution**.</span><span class="sxs-lookup"><span data-stu-id="bcf77-104">Visual Studio 2017 15.8 and later added a new menu item under **Build > ASP.NET Compilation > Optimize Build Performance for Solution**.</span></span>

![capture d’écran de l’élément de menu Nouveau](optimize-build-perf/_static/optimize-build-performance-for-solution.png)

<span data-ttu-id="bcf77-106">ASP.NET compile ses vues lors de l’exécution, ce qui signifie que votre projet ASP.NET s’accompagne d’une copie du compilateur.</span><span class="sxs-lookup"><span data-stu-id="bcf77-106">ASP.NET compiles its views at runtime, which means your ASP.NET project carries with it a copy of the compiler.</span></span> <span data-ttu-id="bcf77-107">Toutefois, sur un ordinateur de développeur lors de la copie du compilateur ne correspond pas à copier de Visual Studio, les performances de génération sont affecté l’ordre de 1 à 3 secondes, la génération incrémentielle.</span><span class="sxs-lookup"><span data-stu-id="bcf77-107">However, on a developer machine when the copy of the compiler doesn't match Visual Studio's copy, your build performance is impacted on the order of 1-3 seconds per incremental build.</span></span> <span data-ttu-id="bcf77-108">Cette fonctionnalité met à jour votre copie de projet du compilateur pour faire correspondre de Visual Studio qui devrait accélérer les builds incrémentielles.</span><span class="sxs-lookup"><span data-stu-id="bcf77-108">This feature will update your project's copy of the compiler to match Visual Studio's which should speed up incremental builds.</span></span>

<span data-ttu-id="bcf77-109">Cela s’applique aux projets ASP.NET Framework uniquement, il ne s’applique pas à ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="bcf77-109">This is applicable to ASP.NET Framework projects only, it does not apply to ASP.NET Core.</span></span>
