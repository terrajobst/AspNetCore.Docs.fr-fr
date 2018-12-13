---
title: Outils de diagnostic des performances
author: mjrousos
description: Outils utiles pour diagnostiquer les problèmes de performances dans les applications ASP.NET Core.
monikerRange: '>= aspnetcore-1.1'
ms.author: riande
ms.date: 12/07/2018
uid: performance/diagnostic-tools
ms.openlocfilehash: 3093b7d646e4fa943334c7b1e70ddc007ab18780
ms.sourcegitcommit: 1ea1b4fc58055c62728143388562689f1ef96cb2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/13/2018
ms.locfileid: "53329198"
---
# <a name="performance-diagnostic-tools"></a>Outils de Diagnostic de performances

Par [Mike Rousos](https://github.com/mjrousos)

Cet article répertorie les outils pour diagnostiquer les problèmes de performances dans ASP.NET Core.

## <a name="visual-studio-diagnostic-tools"></a>Outils de Diagnostic de Visual Studio

Le [les outils de profilage et de diagnostics](/visualstudio/profiling) intégrés à Visual Studio sont un bon point de départ à examiner les problèmes de performances. Ces outils sont puissants et pratique d’utiliser à partir de l’environnement de développement Visual Studio. Les outils permet d’analyser l’utilisation du processeur, utilisation de la mémoire et les événements de performances dans les applications ASP.NET Core.

Informations supplémentaires sont disponibles dans [documentation de Visual Studio](/visualstudio/profiling/profiling-overview).

## <a name="application-insights"></a>Application Insights

[Application Insights](/azure/application-insights/app-insights-overview) fournit des données de performances détaillées pour votre application. Application Insights collecte automatiquement les données sur les taux de réponse, taux d’échec, les temps de réponse de dépendance et bien plus encore. Application Insights prend en charge la journalisation des événements personnalisés et les mesures spécifiques à votre application.

Application Insights peuvent être utilisés dans des environnements de diverses :

* Optimisé pour travailler dans Azure.
* Fonctionne en production, de développement et de mise en lots.
* Fonctionne localement à partir de [Visual Studio](/azure/application-insights/app-insights-visual-studio) ou dans d’autres environnements d’hébergement.

Pour plus d’informations, voir [Application Insights pour ASP.NET Core](/azure/application-insights/app-insights-asp-net-core).

## <a name="perfview"></a>PerfView

[PerfView](https://github.com/Microsoft/perfview) est un outil d’analyse de performances créé par l’équipe .NET spécifiquement pour diagnostiquer les problèmes de performances de .NET. PerfView permet l’analyse de processeur l’utilisation, mémoire et GC comportement, les événements de performance et temps horloge.

Plus d’informations sur PerfView et la prise en main avec [didacticiels vidéo PerfView](http://channel9.msdn.com/Series/PerfView-Tutorial) ou en lisant le guide de l’utilisateur disponible dans l’outil ou [sur GitHub](https://github.com/Microsoft/perfview).

## <a name="perfcollect"></a>PerfCollect

PerfView est un outil d’analyse des performances utile pour les scénarios de .NET, il s’exécute uniquement sous Windows vous ne pouvez pas l’utiliser pour recueillir des traces à partir d’applications ASP.NET Core en cours d’exécution dans les environnements Linux.

[PerfCollect](https://github.com/dotnet/coreclr/blob/master/Documentation/project-docs/linux-performance-tracing.md) est un script bash qui utilise des outils de profilage de Linux natif ([Perf](https://perf.wiki.kernel.org/index.php/Main_Page) et [LTTng](https://lttng.org/)) pour collecter les traces sur Linux qui peut être analysée par PerfView. PerfCollect est utile lorsque des problèmes de performances s’affichent dans les environnements Linux où PerfView ne peut pas être utilisée directement. Au lieu de cela, PerfCollect peut recueillir des traces à partir d’applications .NET Core qui sont ensuite analysées sur un ordinateur Windows à l’aide de PerfView.

Vous trouverez plus d’informations sur l’installation et la prise en main PerfCollect [sur GitHub](https://github.com/dotnet/coreclr/blob/master/Documentation/project-docs/linux-performance-tracing.md).
