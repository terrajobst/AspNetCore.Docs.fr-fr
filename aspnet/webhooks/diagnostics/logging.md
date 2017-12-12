---
uid: webhooks/diagnostics/logging
title: WebHooks ASP.NET journalisation | Documents Microsoft
author: rick-anderson
description: "Procédure à suivre la journalisation dans ASP.NET WebHooks."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/17/2012
ms.topic: article
ms.assetid: f71bc442-5f80-481b-a32c-a0ec18dee9d6
ms.technology: 
ms.prod: .net-framework
ms.openlocfilehash: 5691d5c394b4e606282ba302953e8a06e7d73860
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/10/2017
---
# <a name="aspnet-webhooks-logging"></a><span data-ttu-id="f8a1d-103">WebHooks ASP.NET journalisation</span><span class="sxs-lookup"><span data-stu-id="f8a1d-103">ASP.NET WebHooks logging</span></span>

<span data-ttu-id="f8a1d-104">Microsoft ASP.NET WebHooks utilise la journalisation comme un moyen de problèmes et des problèmes de reporting.</span><span class="sxs-lookup"><span data-stu-id="f8a1d-104">Microsoft ASP.NET WebHooks uses logging as a way of reporting issues and problems.</span></span> <span data-ttu-id="f8a1d-105">Par défaut, les journaux sont écrits à l’aide de [System.Diagnostics.Trace](https://msdn.microsoft.com/en-us/library/system.diagnostics.trace) où il peut être à l’aide de [écouteurs de traçage](https://msdn.microsoft.com/en-us/library/system.diagnostics.tracelistener.aspx) comme tout autre flux de journal.</span><span class="sxs-lookup"><span data-stu-id="f8a1d-105">By default logs are written using [System.Diagnostics.Trace](https://msdn.microsoft.com/en-us/library/system.diagnostics.trace) where they can be manged using [Trace Listeners](https://msdn.microsoft.com/en-us/library/system.diagnostics.tracelistener.aspx) like any other log stream.</span></span>

<span data-ttu-id="f8a1d-106">Lorsque vous déployez votre Application Web comme une application Web Azure, les journaux sont récupérées automatiquement et peuvent être gérés avec n’importe quel autre [System.Diagnostics.Trace](https://msdn.microsoft.com/en-us/library/system.diagnostics.trace) journalisation.</span><span class="sxs-lookup"><span data-stu-id="f8a1d-106">When deploying your Web Application as an Azure Web App, the logs are automatically picked up and can be managed together with any other [System.Diagnostics.Trace](https://msdn.microsoft.com/en-us/library/system.diagnostics.trace) logging.</span></span> <span data-ttu-id="f8a1d-107">Pour plus d’informations, consultez [activer la journalisation des diagnostics pour les applications web dans Azure App Service](https://azure.microsoft.com/en-us/documentation/articles/web-sites-enable-diagnostic-log/)</span><span class="sxs-lookup"><span data-stu-id="f8a1d-107">For details, please see [Enable diagnostics logging for web apps in Azure App Service](https://azure.microsoft.com/en-us/documentation/articles/web-sites-enable-diagnostic-log/)</span></span>

<span data-ttu-id="f8a1d-108">En outre, les journaux peuvent être obtenus directement à partir de Visual Studio, comme décrit dans [dépanner une application web dans Azure App Service à l’aide de Visual Studio](https://azure.microsoft.com/en-us/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/#webserverlogs).</span><span class="sxs-lookup"><span data-stu-id="f8a1d-108">In addition, logs can be obtained straight from inside Visual Studio as described in [Troubleshoot a web app in Azure App Service using Visual Studio](https://azure.microsoft.com/en-us/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/#webserverlogs).</span></span>

## <a name="redirecting-logs"></a><span data-ttu-id="f8a1d-109">Redirection des journaux</span><span class="sxs-lookup"><span data-stu-id="f8a1d-109">Redirecting Logs</span></span>

<span data-ttu-id="f8a1d-110">Au lieu d’écrire des journaux de [System.Diagnostics.Trace](https://msdn.microsoft.com/en-us/library/system.diagnostics.trace), il est possible de fournir une implémentation d’un autre enregistrement qui peut se connecter directement à un gestionnaire de journal comme [Log4Net](http://logging.apache.org/log4net/) et [NLog ](http://nlog-project.org/).</span><span class="sxs-lookup"><span data-stu-id="f8a1d-110">Instead of writing logs to [System.Diagnostics.Trace](https://msdn.microsoft.com/en-us/library/system.diagnostics.trace), it is possible to provide an alternate logging implementation that can log directly to a log manager such as [Log4Net](http://logging.apache.org/log4net/) and [NLog](http://nlog-project.org/).</span></span> <span data-ttu-id="f8a1d-111">Il suffit de fournir une implémentation de [ILogger](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Common/Diagnostics/ILogger.cs) il est collectée par Microsoft ASP.NET WebHooks et inscrivez-le avec un moteur d’injection de dépendance de votre choix.</span><span class="sxs-lookup"><span data-stu-id="f8a1d-111">Simply provide an implementation of [ILogger](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Common/Diagnostics/ILogger.cs) and register it with a dependency injection engine of your choice and it will get picked up by Microsoft ASP.NET WebHooks.</span></span> <span data-ttu-id="f8a1d-112">Consultez [Injection de dépendances dans ASP.NET Web API 2](https://www.asp.net/web-api/overview/advanced/dependency-injection) pour plus d’informations.</span><span class="sxs-lookup"><span data-stu-id="f8a1d-112">Please see [Dependency Injection in ASP.NET Web API 2](https://www.asp.net/web-api/overview/advanced/dependency-injection) for details.</span></span>
