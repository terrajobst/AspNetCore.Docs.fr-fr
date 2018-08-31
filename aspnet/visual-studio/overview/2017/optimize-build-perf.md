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
# <a name="optimize-build-performance-for-solution"></a>Optimiser les performances de génération de solution

15.8 de 2017 Visual Studio ou versions ultérieures incluent un élément de menu : **Build** > **Compilation ASP.NET** > **optimiser les performances de génération de Solution**.

![capture d’écran de l’élément de menu Nouveau](optimize-build-perf/_static/optimize-build-performance-for-solution.png)

ASP.NET compile ses vues lors de l’exécution, ce qui signifie qu’un projet ASP.NET s’accompagne d’une copie du compilateur. Toutefois sur un ordinateur de développeur lors de la copie du compilateur ne correspond pas à copier de Visual Studio, les performances de génération sont affecté l’ordre de 1 à 3 secondes, la génération incrémentielle. Cette fonctionnalité met à jour la copie de votre projet du compilateur pour faire correspondre de Visual Studio, ce qui améliore généralement les builds incrémentielles.

**Cela s’applique à ASP.NET Framework 4.7.1 ou uniquement les projets plus tard, il ne s’applique pas à ASP.NET Core.**
