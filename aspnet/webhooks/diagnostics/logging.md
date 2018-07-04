---
uid: webhooks/diagnostics/logging
title: WebHooks ASP.NET journalisation | Microsoft Docs
author: rick-anderson
description: Procédure à suivre la journalisation dans ASP.NET WebHooks.
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/17/2012
ms.topic: article
ms.assetid: f71bc442-5f80-481b-a32c-a0ec18dee9d6
ms.technology: ''
ms.openlocfilehash: 5501d250ea6dd86c0cfddec8d9941ec16b4f57ba
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37364104"
---
# <a name="aspnet-webhooks-logging"></a><span data-ttu-id="3fe15-103">WebHooks ASP.NET journalisation</span><span class="sxs-lookup"><span data-stu-id="3fe15-103">ASP.NET WebHooks logging</span></span>

<span data-ttu-id="3fe15-104">Microsoft WebHooks ASP.NET utilise l’enregistrement comme un moyen de signalement de problèmes et des problèmes.</span><span class="sxs-lookup"><span data-stu-id="3fe15-104">Microsoft ASP.NET WebHooks uses logging as a way of reporting issues and problems.</span></span> <span data-ttu-id="3fe15-105">Par défaut les journaux sont écrits à l’aide de [System.Diagnostics.Trace](https://msdn.microsoft.com/library/system.diagnostics.trace) où ils peuvent être administrés à l’aide de [écouteurs de suivi](https://msdn.microsoft.com/library/system.diagnostics.tracelistener.aspx) comme n’importe quel autre flux de journal.</span><span class="sxs-lookup"><span data-stu-id="3fe15-105">By default logs are written using [System.Diagnostics.Trace](https://msdn.microsoft.com/library/system.diagnostics.trace) where they can be manged using [Trace Listeners](https://msdn.microsoft.com/library/system.diagnostics.tracelistener.aspx) like any other log stream.</span></span>

<span data-ttu-id="3fe15-106">Lorsque vous déployez votre Application Web en tant qu’une application Web Azure, les journaux sont récupérées automatiquement et peuvent être gérés avec n’importe quel autre [System.Diagnostics.Trace](https://msdn.microsoft.com/library/system.diagnostics.trace) journalisation.</span><span class="sxs-lookup"><span data-stu-id="3fe15-106">When deploying your Web Application as an Azure Web App, the logs are automatically picked up and can be managed together with any other [System.Diagnostics.Trace](https://msdn.microsoft.com/library/system.diagnostics.trace) logging.</span></span> <span data-ttu-id="3fe15-107">Pour plus d’informations, consultez [activer la journalisation des diagnostics pour les applications web dans Azure App Service](https://azure.microsoft.com/documentation/articles/web-sites-enable-diagnostic-log/)</span><span class="sxs-lookup"><span data-stu-id="3fe15-107">For details, please see [Enable diagnostics logging for web apps in Azure App Service](https://azure.microsoft.com/documentation/articles/web-sites-enable-diagnostic-log/)</span></span>

<span data-ttu-id="3fe15-108">En outre, les journaux peuvent provenir directement à l’intérieur de Visual Studio, comme décrit dans [dépanner une application web dans Azure App Service à l’aide de Visual Studio](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/#webserverlogs).</span><span class="sxs-lookup"><span data-stu-id="3fe15-108">In addition, logs can be obtained straight from inside Visual Studio as described in [Troubleshoot a web app in Azure App Service using Visual Studio](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/#webserverlogs).</span></span>

## <a name="redirecting-logs"></a><span data-ttu-id="3fe15-109">Redirection des journaux</span><span class="sxs-lookup"><span data-stu-id="3fe15-109">Redirecting Logs</span></span>

<span data-ttu-id="3fe15-110">Au lieu d’écrire des journaux à [System.Diagnostics.Trace](https://msdn.microsoft.com/library/system.diagnostics.trace), il est possible de fournir une implémentation de journalisation autre pouvant se connecter directement à un gestionnaire de journal comme [Log4Net](http://logging.apache.org/log4net/) et [NLog ](http://nlog-project.org/).</span><span class="sxs-lookup"><span data-stu-id="3fe15-110">Instead of writing logs to [System.Diagnostics.Trace](https://msdn.microsoft.com/library/system.diagnostics.trace), it is possible to provide an alternate logging implementation that can log directly to a log manager such as [Log4Net](http://logging.apache.org/log4net/) and [NLog](http://nlog-project.org/).</span></span> <span data-ttu-id="3fe15-111">Il vous suffit de fournir une implémentation de [ILogger](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Common/Diagnostics/ILogger.cs) et inscrivez-le avec un moteur de l’injection de dépendance de votre choix et il est sélectionnée par Microsoft ASP.NET WebHooks.</span><span class="sxs-lookup"><span data-stu-id="3fe15-111">Simply provide an implementation of [ILogger](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Common/Diagnostics/ILogger.cs) and register it with a dependency injection engine of your choice and it will get picked up by Microsoft ASP.NET WebHooks.</span></span> <span data-ttu-id="3fe15-112">Consultez [l’Injection de dépendances dans ASP.NET Web API 2](https://www.asp.net/web-api/overview/advanced/dependency-injection) pour plus d’informations.</span><span class="sxs-lookup"><span data-stu-id="3fe15-112">Please see [Dependency Injection in ASP.NET Web API 2](https://www.asp.net/web-api/overview/advanced/dependency-injection) for details.</span></span>
