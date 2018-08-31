---
uid: visual-studio/overview/2017/optimize-build-perf
title: Optimiser les performances de génération de solution
author: AngelosP
description: Optimiser les performances de génération de solution
ms.author: riande
ms.date: 08/29/2018
msc.type: authoredcontent
ms.openlocfilehash: c1a5cf5e59374b4c0dd7150c5dd62fbde42af555
ms.sourcegitcommit: ecf2cd4e0613569025b28e12de3baa21d86d4258
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/30/2018
ms.locfileid: "43312139"
---
# <a name="optimize-build-performance-for-solution"></a><span data-ttu-id="fda41-103">Optimiser les performances de génération de solution</span><span class="sxs-lookup"><span data-stu-id="fda41-103">Optimize build performance for solution</span></span>

<span data-ttu-id="fda41-104">15.8 de 2017 Visual Studio ou versions ultérieures incluent un élément de menu : **Build** > **Compilation ASP.NET** > **optimiser les performances de génération de Solution**.</span><span class="sxs-lookup"><span data-stu-id="fda41-104">Visual Studio 2017 15.8 or later include a menu item: **Build** > **ASP.NET Compilation** > **Optimize Build Performance for Solution**.</span></span>

![capture d’écran de l’élément de menu Nouveau](optimize-build-perf/_static/optimize-build-performance-for-solution.png)

<span data-ttu-id="fda41-106">ASP.NET compile ses vues lors de l’exécution, ce qui signifie qu’un projet ASP.NET s’accompagne d’une copie du compilateur.</span><span class="sxs-lookup"><span data-stu-id="fda41-106">ASP.NET compiles its views at runtime, which means that an ASP.NET project carries with it a copy of the compiler.</span></span> <span data-ttu-id="fda41-107">Toutefois sur un ordinateur de développeur lors de la copie du compilateur ne correspond pas à copier de Visual Studio, les performances de génération sont affecté l’ordre de 1 à 3 secondes, la génération incrémentielle.</span><span class="sxs-lookup"><span data-stu-id="fda41-107">However on a developer machine when the copy of the compiler doesn't match Visual Studio's copy, build performance is impacted on the order of 1-3 seconds per incremental build.</span></span> <span data-ttu-id="fda41-108">Cette fonctionnalité met à jour la copie de votre projet du compilateur pour faire correspondre de Visual Studio, ce qui améliore généralement les builds incrémentielles.</span><span class="sxs-lookup"><span data-stu-id="fda41-108">This feature updates your project's copy of the compiler to match Visual Studio's, which usually speeds up incremental builds.</span></span>

<span data-ttu-id="fda41-109">**Cela s’applique à ASP.NET Framework 4.7.1 ou uniquement les projets plus tard, il ne s’applique pas à ASP.NET Core.**</span><span class="sxs-lookup"><span data-stu-id="fda41-109">**This is applicable to ASP.NET Framework 4.7.1 or later projects only, it does not apply to ASP.NET Core.**</span></span>
