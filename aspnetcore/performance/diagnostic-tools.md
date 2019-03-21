---
title: Outils de diagnostic des performances
author: mjrousos
description: Outils utiles pour diagnostiquer les problèmes de performances dans les applications ASP.NET Core.
monikerRange: '>= aspnetcore-1.1'
ms.author: riande
ms.date: 12/07/2018
uid: performance/diagnostic-tools
ms.openlocfilehash: 0fb9944d1082090f4f58f641a26ee96286920456
ms.sourcegitcommit: 5f299daa7c8102d56a63b214b9a34cc4bc87bc42
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/19/2019
ms.locfileid: "58210000"
---
# <a name="performance-diagnostic-tools"></a>Outils de Diagnostic de performances

Par [Mike Rousos](https://github.com/mjrousos)

Cet article répertorie les outils pour diagnostiquer les problèmes de performances dans ASP.NET Core.

## <a name="visual-studio-diagnostic-tools"></a>Outils de Diagnostic de Visual Studio

Le [les outils de profilage et de diagnostics](/visualstudio/profiling) intégrés à Visual Studio sont un bon point de départ à examiner les problèmes de performances. Ces outils sont puissants et pratique d’utiliser à partir de l’environnement de développement Visual Studio. Les outils permet d’analyser l’utilisation du processeur, utilisation de la mémoire et les événements de performances dans les applications ASP.NET Core. Intégré en cours rend facile de profilage au moment du développement.

Informations supplémentaires sont disponibles dans [documentation de Visual Studio](/visualstudio/profiling/profiling-overview).

## <a name="application-insights"></a>Application Insights

[Application Insights](/azure/application-insights/app-insights-overview) fournit des données de performances détaillées pour votre application. Application Insights collecte automatiquement les données sur les taux de réponse, taux d’échec, les temps de réponse de dépendance et bien plus encore. Application Insights prend en charge la journalisation des événements personnalisés et les mesures spécifiques à votre application.

Azure Application Insights offre plusieurs moyens de donner des informations sur les applications analysées :

- [Cartographie d’application](/azure/application-insights/app-insights-app-map) – permet des baisses de performances ou réactives Échec sur tous les composants d’applications distribuées.
- [Panneau de métriques dans le portail Application Insights](/azure/application-insights/app-insights-metrics-explorer?toc=/azure/azure-monitor/toc.json) montre les valeurs observées et nombres d’événements.
- [Panneau de performances dans le portail Application Insights](/azure/application-insights/app-insights-tutorial-performance):

  - Affiche les détails de performances pour différentes opérations dans l’application analysée.
  - Permet l’exploration en une seule opération à vérifier toutes les parties/dépendances qui contribuent à une longue durée.
  - Profiler peut être appelée à partir d’ici à collecter les performances traces à la demande.

- [De Azure Application Insights Profiler](/azure/azure-monitor/app/profiler) permet régulière et à la demande sur le profilage des applications .NET.  Azure indique portail capturé des traces de performances avec les piles d’appels et les chemins d’accès à chaud. Les fichiers de trace peuvent également être téléchargés pour une analyse plus approfondie à l’aide de PerfView.

Application Insights peuvent être utilisés dans des environnements de diverses :

- Optimisé pour travailler dans Azure.
- Fonctionne en production, de développement et de mise en lots.
- Fonctionne localement à partir de [Visual Studio](/azure/application-insights/app-insights-visual-studio) ou dans d’autres environnements d’hébergement.

Pour plus d’informations, voir [Application Insights pour ASP.NET Core](/azure/application-insights/app-insights-asp-net-core).

## <a name="perfview"></a>PerfView

[PerfView](https://github.com/Microsoft/perfview) est un outil d’analyse de performances créé par l’équipe .NET spécifiquement pour diagnostiquer les problèmes de performances de .NET. PerfView permet l’analyse de processeur l’utilisation, mémoire et GC comportement, les événements de performance et temps horloge.

Plus d’informations sur PerfView et la prise en main avec [didacticiels vidéo PerfView](http://channel9.msdn.com/Series/PerfView-Tutorial) ou en lisant le guide de l’utilisateur disponible dans l’outil ou [sur GitHub](https://github.com/Microsoft/perfview).

## <a name="windows-performance-toolkit"></a>Windows Performance Toolkit

[Windows Performance Toolkit](/windows-hardware/test/wpt/) (WPT) se compose de deux composants : Enregistreur de Performance Windows (WPR) et Windows Performance Analyzer (WPA). Les outils de produisent des profils de performances approfondies de systèmes d’exploitation de Windows et d’applications. WPT a de nombreuses façons de visualiser les données, mais sa collecte des données sont moins puissantes que de PerfView.

## <a name="perfcollect"></a>PerfCollect

PerfView est un outil d’analyse des performances utile pour les scénarios de .NET, il s’exécute uniquement sous Windows vous ne pouvez pas l’utiliser pour recueillir des traces à partir d’applications ASP.NET Core en cours d’exécution dans les environnements Linux.

[PerfCollect](https://github.com/dotnet/coreclr/blob/master/Documentation/project-docs/linux-performance-tracing.md) est un script bash qui utilise des outils de profilage de Linux natif ([Perf](https://perf.wiki.kernel.org/index.php/Main_Page) et [LTTng](https://lttng.org/)) pour collecter les traces sur Linux qui peut être analysée par PerfView. PerfCollect est utile lorsque des problèmes de performances s’affichent dans les environnements Linux où PerfView ne peut pas être utilisée directement. Au lieu de cela, PerfCollect peut recueillir des traces à partir d’applications .NET Core qui sont ensuite analysées sur un ordinateur Windows à l’aide de PerfView.

Vous trouverez plus d’informations sur l’installation et la prise en main PerfCollect [sur GitHub](https://github.com/dotnet/coreclr/blob/master/Documentation/project-docs/linux-performance-tracing.md).

## <a name="other-third-party-performance-tools"></a>Autres outils de performances tiers

La liste suivante décrit des outils de performances tiers qui sont utiles dans l’analyse des performances des applications .NET Core.

- [MiniProfiler](https://miniprofiler.com/)
- dotTrace et dotMemory de JetBrains
- VTune d’Intel
