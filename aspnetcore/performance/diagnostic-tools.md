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
# <a name="performance-diagnostic-tools"></a><span data-ttu-id="a1930-103">Outils de Diagnostic de performances</span><span class="sxs-lookup"><span data-stu-id="a1930-103">Performance Diagnostic Tools</span></span>

<span data-ttu-id="a1930-104">Par [Mike Rousos](https://github.com/mjrousos)</span><span class="sxs-lookup"><span data-stu-id="a1930-104">By [Mike Rousos](https://github.com/mjrousos)</span></span>

<span data-ttu-id="a1930-105">Cet article répertorie les outils pour diagnostiquer les problèmes de performances dans ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="a1930-105">This article lists tools for diagnosing performance issues in ASP.NET Core.</span></span>

## <a name="visual-studio-diagnostic-tools"></a><span data-ttu-id="a1930-106">Outils de Diagnostic de Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a1930-106">Visual Studio Diagnostic Tools</span></span>

<span data-ttu-id="a1930-107">Le [les outils de profilage et de diagnostics](/visualstudio/profiling) intégrés à Visual Studio sont un bon point de départ à examiner les problèmes de performances.</span><span class="sxs-lookup"><span data-stu-id="a1930-107">The [profiling and diagnostic tools](/visualstudio/profiling) built into Visual Studio are a good place to start investigating performance issues.</span></span> <span data-ttu-id="a1930-108">Ces outils sont puissants et pratique d’utiliser à partir de l’environnement de développement Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="a1930-108">These tools are powerful and convenient to use from the Visual Studio development environment.</span></span> <span data-ttu-id="a1930-109">Les outils permet d’analyser l’utilisation du processeur, utilisation de la mémoire et les événements de performances dans les applications ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="a1930-109">The tooling allows analysis of CPU usage, memory usage, and performance events in ASP.NET Core apps.</span></span>

<span data-ttu-id="a1930-110">Informations supplémentaires sont disponibles dans [documentation de Visual Studio](/visualstudio/profiling/profiling-overview).</span><span class="sxs-lookup"><span data-stu-id="a1930-110">More information is available in [Visual Studio documentation](/visualstudio/profiling/profiling-overview).</span></span>

## <a name="application-insights"></a><span data-ttu-id="a1930-111">Application Insights</span><span class="sxs-lookup"><span data-stu-id="a1930-111">Application Insights</span></span>

<span data-ttu-id="a1930-112">[Application Insights](/azure/application-insights/app-insights-overview) fournit des données de performances détaillées pour votre application.</span><span class="sxs-lookup"><span data-stu-id="a1930-112">[Application Insights](/azure/application-insights/app-insights-overview) provides in-depth performance data for your app.</span></span> <span data-ttu-id="a1930-113">Application Insights collecte automatiquement les données sur les taux de réponse, taux d’échec, les temps de réponse de dépendance et bien plus encore.</span><span class="sxs-lookup"><span data-stu-id="a1930-113">Application Insights automatically collects data on response rates, failure rates, dependency response times, and more.</span></span> <span data-ttu-id="a1930-114">Application Insights prend en charge la journalisation des événements personnalisés et les mesures spécifiques à votre application.</span><span class="sxs-lookup"><span data-stu-id="a1930-114">Application Insights supports logging custom events and metrics specific to your app.</span></span>

<span data-ttu-id="a1930-115">Application Insights peuvent être utilisés dans des environnements de diverses :</span><span class="sxs-lookup"><span data-stu-id="a1930-115">Application Insights can be used in a variety environments:</span></span>

* <span data-ttu-id="a1930-116">Optimisé pour travailler dans Azure.</span><span class="sxs-lookup"><span data-stu-id="a1930-116">Optimized to work in Azure.</span></span>
* <span data-ttu-id="a1930-117">Fonctionne en production, de développement et de mise en lots.</span><span class="sxs-lookup"><span data-stu-id="a1930-117">Works in production, development, and staging.</span></span>
* <span data-ttu-id="a1930-118">Fonctionne localement à partir de [Visual Studio](/azure/application-insights/app-insights-visual-studio) ou dans d’autres environnements d’hébergement.</span><span class="sxs-lookup"><span data-stu-id="a1930-118">Works locally from [Visual Studio](/azure/application-insights/app-insights-visual-studio) or in other hosting environments.</span></span>

<span data-ttu-id="a1930-119">Pour plus d’informations, voir [Application Insights pour ASP.NET Core](/azure/application-insights/app-insights-asp-net-core).</span><span class="sxs-lookup"><span data-stu-id="a1930-119">For more information, see [Application Insights for ASP.NET Core](/azure/application-insights/app-insights-asp-net-core).</span></span>

## <a name="perfview"></a><span data-ttu-id="a1930-120">PerfView</span><span class="sxs-lookup"><span data-stu-id="a1930-120">PerfView</span></span>

<span data-ttu-id="a1930-121">[PerfView](https://github.com/Microsoft/perfview) est un outil d’analyse de performances créé par l’équipe .NET spécifiquement pour diagnostiquer les problèmes de performances de .NET.</span><span class="sxs-lookup"><span data-stu-id="a1930-121">[PerfView](https://github.com/Microsoft/perfview) is a performance analysis tool created by the .NET team specifically for diagnosing .NET performance issues.</span></span> <span data-ttu-id="a1930-122">PerfView permet l’analyse de processeur l’utilisation, mémoire et GC comportement, les événements de performance et temps horloge.</span><span class="sxs-lookup"><span data-stu-id="a1930-122">PerfView allows analysis of CPU usage, memory and GC behavior, performance events, and wall clock time.</span></span>

<span data-ttu-id="a1930-123">Plus d’informations sur PerfView et la prise en main avec [didacticiels vidéo PerfView](http://channel9.msdn.com/Series/PerfView-Tutorial) ou en lisant le guide de l’utilisateur disponible dans l’outil ou [sur GitHub](https://github.com/Microsoft/perfview).</span><span class="sxs-lookup"><span data-stu-id="a1930-123">You can learn more about PerfView and how to get started with [PerfView video tutorials](http://channel9.msdn.com/Series/PerfView-Tutorial) or by reading the user's guide available in the tool or [on GitHub](https://github.com/Microsoft/perfview).</span></span>

## <a name="perfcollect"></a><span data-ttu-id="a1930-124">PerfCollect</span><span class="sxs-lookup"><span data-stu-id="a1930-124">PerfCollect</span></span>

<span data-ttu-id="a1930-125">PerfView est un outil d’analyse des performances utile pour les scénarios de .NET, il s’exécute uniquement sous Windows vous ne pouvez pas l’utiliser pour recueillir des traces à partir d’applications ASP.NET Core en cours d’exécution dans les environnements Linux.</span><span class="sxs-lookup"><span data-stu-id="a1930-125">While PerfView is a useful performance analysis tool for .NET scenarios, it only runs on Windows so you can't use it to collect traces from ASP.NET Core apps running in Linux environments.</span></span>

<span data-ttu-id="a1930-126">[PerfCollect](https://github.com/dotnet/coreclr/blob/master/Documentation/project-docs/linux-performance-tracing.md) est un script bash qui utilise des outils de profilage de Linux natif ([Perf](https://perf.wiki.kernel.org/index.php/Main_Page) et [LTTng](https://lttng.org/)) pour collecter les traces sur Linux qui peut être analysée par PerfView.</span><span class="sxs-lookup"><span data-stu-id="a1930-126">[PerfCollect](https://github.com/dotnet/coreclr/blob/master/Documentation/project-docs/linux-performance-tracing.md) is a bash script that uses native Linux profiling tools ([Perf](https://perf.wiki.kernel.org/index.php/Main_Page) and [LTTng](https://lttng.org/)) to collect traces on Linux that can be analyzed by PerfView.</span></span> <span data-ttu-id="a1930-127">PerfCollect est utile lorsque des problèmes de performances s’affichent dans les environnements Linux où PerfView ne peut pas être utilisée directement.</span><span class="sxs-lookup"><span data-stu-id="a1930-127">PerfCollect is useful when performance problems show up in Linux environments where PerfView can't be used directly.</span></span> <span data-ttu-id="a1930-128">Au lieu de cela, PerfCollect peut recueillir des traces à partir d’applications .NET Core qui sont ensuite analysées sur un ordinateur Windows à l’aide de PerfView.</span><span class="sxs-lookup"><span data-stu-id="a1930-128">Instead, PerfCollect can collect traces from .NET Core apps that are then analyzed on a Windows computer using PerfView.</span></span>

<span data-ttu-id="a1930-129">Vous trouverez plus d’informations sur l’installation et la prise en main PerfCollect [sur GitHub](https://github.com/dotnet/coreclr/blob/master/Documentation/project-docs/linux-performance-tracing.md).</span><span class="sxs-lookup"><span data-stu-id="a1930-129">More information about how to install and get started with PerfCollect is available [on GitHub](https://github.com/dotnet/coreclr/blob/master/Documentation/project-docs/linux-performance-tracing.md).</span></span>
