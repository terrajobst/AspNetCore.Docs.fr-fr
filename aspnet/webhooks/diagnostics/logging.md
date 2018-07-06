---
uid: webhooks/diagnostics/logging
title: WebHooks ASP.NET journalisation | Microsoft Docs
author: rick-anderson
description: Procédure à suivre la journalisation dans ASP.NET WebHooks.
ms.author: aspnetcontent
ms.date: 01/17/2012
ms.assetid: f71bc442-5f80-481b-a32c-a0ec18dee9d6
ms.openlocfilehash: 65e4d49474034406be835eb31378c81ba0706da3
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37828453"
---
# <a name="aspnet-webhooks-logging"></a>WebHooks ASP.NET journalisation

Microsoft WebHooks ASP.NET utilise l’enregistrement comme un moyen de signalement de problèmes et des problèmes. Par défaut les journaux sont écrits à l’aide de [System.Diagnostics.Trace](https://msdn.microsoft.com/library/system.diagnostics.trace) où ils peuvent être administrés à l’aide de [écouteurs de suivi](https://msdn.microsoft.com/library/system.diagnostics.tracelistener.aspx) comme n’importe quel autre flux de journal.

Lorsque vous déployez votre Application Web en tant qu’une application Web Azure, les journaux sont récupérées automatiquement et peuvent être gérés avec n’importe quel autre [System.Diagnostics.Trace](https://msdn.microsoft.com/library/system.diagnostics.trace) journalisation. Pour plus d’informations, consultez [activer la journalisation des diagnostics pour les applications web dans Azure App Service](https://azure.microsoft.com/documentation/articles/web-sites-enable-diagnostic-log/)

En outre, les journaux peuvent provenir directement à l’intérieur de Visual Studio, comme décrit dans [dépanner une application web dans Azure App Service à l’aide de Visual Studio](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/#webserverlogs).

## <a name="redirecting-logs"></a>Redirection des journaux

Au lieu d’écrire des journaux à [System.Diagnostics.Trace](https://msdn.microsoft.com/library/system.diagnostics.trace), il est possible de fournir une implémentation de journalisation autre pouvant se connecter directement à un gestionnaire de journal comme [Log4Net](http://logging.apache.org/log4net/) et [NLog ](http://nlog-project.org/). Il vous suffit de fournir une implémentation de [ILogger](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Common/Diagnostics/ILogger.cs) et inscrivez-le avec un moteur de l’injection de dépendance de votre choix et il est sélectionnée par Microsoft ASP.NET WebHooks. Consultez [l’Injection de dépendances dans ASP.NET Web API 2](https://www.asp.net/web-api/overview/advanced/dependency-injection) pour plus d’informations.
