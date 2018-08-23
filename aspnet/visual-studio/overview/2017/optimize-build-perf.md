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
# <a name="optimize-build-performance-for-solution"></a>Optimiser les performances de génération de solution
15.8 de 2017 Visual Studio et ajoutée ultérieurement un nouvel élément de menu sous **Générer > ASP.NET Compilation > optimiser les performances de génération de Solution**.

![capture d’écran de l’élément de menu Nouveau](optimize-build-perf/_static/optimize-build-performance-for-solution.png)

ASP.NET compile ses vues lors de l’exécution, ce qui signifie que votre projet ASP.NET s’accompagne d’une copie du compilateur. Toutefois, sur un ordinateur de développeur lors de la copie du compilateur ne correspond pas à copier de Visual Studio, les performances de génération sont affecté l’ordre de 1 à 3 secondes, la génération incrémentielle. Cette fonctionnalité met à jour votre copie de projet du compilateur pour faire correspondre de Visual Studio qui devrait accélérer les builds incrémentielles.

Cela s’applique aux projets ASP.NET Framework uniquement, il ne s’applique pas à ASP.NET Core.
