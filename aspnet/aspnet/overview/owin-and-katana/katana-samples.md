---
uid: aspnet/overview/owin-and-katana/katana-samples
title: Exemples Katana | Microsoft Docs
author: microsoft
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/17/2014
ms.topic: article
ms.assetid: bec04f5d-2638-4417-b288-97c58c8d6379
ms.technology: ''
msc.legacyurl: /aspnet/overview/owin-and-katana/katana-samples
msc.type: authoredcontent
ms.openlocfilehash: e81da1e650d8dfd24a3e0fda6aa42b7f360ce12d
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37393317"
---
<a name="katana-samples"></a>Exemples Katana
====================
par [Microsoft](https://github.com/microsoft)

## <a name="katana-samples"></a>Exemples Katana

**ASP.NET achemine exemple** | [Code Source](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/AspNetRoutes/ReadMe.txt)  
Dans certaines applications, vous devez raccorder les composants OWIN dans la table de routage Asp.Net côte à côte avec les composants non OWIN. Cet exemple montre comment utiliser les méthodes d’extension RouteCollection MapOwinPath et MapOwinRoute fournies par Microsoft.Owin.Host.SystemWeb.

**Création de branches Pipelines exemple** | [Code Source](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/BranchingPipelines/ReadMe.txt)  
Pipelines de traitement de la demande OWIN n’êtes pas obligé d’être linéaire, ils peuvent comprendre des branches pour traiter les demandes de différentes façons. Cet exemple montre comment construire un pipeline de branchement basé sur les chemins d’accès de la demande ou d’autres données de requête tels que les en-têtes. Ces composants sont disponibles dans le package nuget de Microsoft.Owin.Mapping.

**Exemple de serveur personnalisé** | [Code Source](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/CustomServer/MyCustomServer/CustomServer.cs)   
Montre comment utiliser un serveur OWIN personnalisé lors de l’auto-hébergement OWIN.

**Incorporé exemple** | [Code Source](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/Embedded/ReadMe.txt)  
Certains serveurs OWIN peuvent être exécutés à l’intérieur de votre propre processus (&quot;auto-hébergé&quot;). Cet exemple montre comment démarrer une application OWIN à l’aide des outils fournis par le package nuget Microsoft.Owin.Hosting.

**Exemple HelloWorld** | [Code Source](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/HelloWorld/ReadMe.txt)  
OWIN est un abstraction d’API qui permet la portabilité des applications sur différents serveurs du serveur HTTP. Cet exemple montre comment écrire une application Hello World à l’aide de certaines **wrappers simples** autour de l’abstraction de OWIN brutes et les exécuter sur un serveur web, tel que ASP.NET.

**Exemple Hello World brutes OWIN** | [Code Source](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/HelloWorldRawOwin/ReadMe.txt)  
Cet exemple montre comment écrire une application Hello World à l’aide du **brutes** abstraction de OWIN et l’exécuter sur un serveur web comme Asp.Net.

**Exemple de SignalR** | [Code Source](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/SignalR/Program.cs)  
Montre comment l’auto-hébergement de SignalR à l’aide d’OWIN / Katana. Pour plus d’informations sur d’auto-hébergement de SignalR, consultez [didacticiel : auto-hébergement de SignalR](../../../signalr/overview/deployment/tutorial-signalr-self-host.md).

**Exemple de fichiers statiques** | [Code Source](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/StaticFilesSample/Startup.cs)   
Montre comment prendre en charge des requêtes HTTP pour les fichiers statiques à l’aide d’OWIN / Katana.

**API Web** | [Code Source](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/WebApi/ReadMe.txt)   
Cet exemple montre comment héberger OWIN dans IIS et ajouter des API Web au pipeline OWIN.

**Exemple de Socket de Web** | [Code Source](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/WebSocketSample/WebSocketServer/Startup.cs)   
Montre comment prendre en charge Web Sockets dans OWIN à l’aide de la [System.Net.WebSockets.WebSocket](https://msdn.microsoft.com/library/system.net.websockets.websocket(v=vs.110).aspx) classe.
