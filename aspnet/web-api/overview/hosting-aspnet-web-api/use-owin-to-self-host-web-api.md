---
uid: web-api/overview/hosting-aspnet-web-api/use-owin-to-self-host-web-api
title: Utiliser OWIN pour auto-héberger ASP.NET Web API 2 | Microsoft Docs
author: rick-anderson
description: Ce didacticiel montre comment héberger des API Web ASP.NET dans une application console, à l’aide d’OWIN pour auto-héberger l’infrastructure API Web. Open Web Interface pour .NET (OWIN) d...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/09/2013
ms.topic: article
ms.assetid: a90a04ce-9d07-43ad-8250-8a92fb2bd3d5
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/hosting-aspnet-web-api/use-owin-to-self-host-web-api
msc.type: authoredcontent
ms.openlocfilehash: 73757b50c15c6c65dbde4b61179b2d453673cfad
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37389558"
---
<a name="use-owin-to-self-host-aspnet-web-api-2"></a>Utiliser OWIN pour auto-héberger ASP.NET Web API 2
====================
par [Kanchan Mehrotra](https://twitter.com/kanchanmeh)

> Ce didacticiel montre comment héberger des API Web ASP.NET dans une application console, à l’aide d’OWIN pour auto-héberger l’infrastructure API Web.
> 
> [Open Web Interface pour .NET](http://owin.org) (OWIN) définit une abstraction entre les serveurs web de .NET et des applications web. OWIN dissocie de l’application web à partir du serveur, ce qui rend OWIN idéale pour l’hébergement automatique d’une application web dans votre propre processus, en dehors d’IIS.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versions des logiciels utilisées dans le didacticiel
> 
> 
> - [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads) (fonctionne également avec Visual Studio 2012)
> - Web API 2


> [!NOTE]
> Vous trouverez le code source complet pour ce didacticiel à [aspnet.codeplex.com](https://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OwinSelfhostSample/ReadMe.txt).


## <a name="create-a-console-application"></a>Créez une Application Console

Sur le **fichier** menu, cliquez sur **New**, puis cliquez sur **projet**. À partir de **modèles installés**, sous Visual c#, cliquez sur **Windows** puis cliquez sur **Application Console**. Nommez le projet « OwinSelfhostSample » et cliquez sur **OK**.

[![](use-owin-to-self-host-web-api/_static/image2.png)](use-owin-to-self-host-web-api/_static/image1.png)

## <a name="add-the-web-api-and-owin-packages"></a>Ajouter les API Web et les Packages OWIN

À partir de la **outils** menu, cliquez sur **Library Package Manager**, puis cliquez sur **Console du Gestionnaire de Package**. Dans la fenêtre de Console du Gestionnaire de Package, entrez la commande suivante :

`Install-Package Microsoft.AspNet.WebApi.OwinSelfHost`

Cela installera le package de selfhost WebAPI OWIN et tous les packages OWIN requis.

[![](use-owin-to-self-host-web-api/_static/image4.png)](use-owin-to-self-host-web-api/_static/image3.png)

## <a name="configure-web-api-for-self-host"></a>Configurer les API Web pour l’auto-hébergement

Dans l’Explorateur de solutions, cliquez avec le bouton droit sur le projet et sélectionnez **ajouter** / **classe** pour ajouter une nouvelle classe. Nommez la classe `Startup`.

![](use-owin-to-self-host-web-api/_static/image5.png)

Remplacez tout le code réutilisable dans ce fichier avec les éléments suivants :

[!code-csharp[Main](use-owin-to-self-host-web-api/samples/sample1.cs)]

## <a name="add-a-web-api-controller"></a>Ajouter un contrôleur d’API Web

Ensuite, ajoutez une classe de contrôleur d’API Web. Dans l’Explorateur de solutions, cliquez avec le bouton droit sur le projet et sélectionnez **ajouter** / **classe** pour ajouter une nouvelle classe. Nommez la classe `ValuesController`.

Remplacez tout le code réutilisable dans ce fichier avec les éléments suivants :

[!code-csharp[Main](use-owin-to-self-host-web-api/samples/sample2.cs)]

## <a name="start-the-owin-host-and-make-a-request-using-httpclient"></a>Démarrer l’hôte OWIN et effectuer une demande à l’aide de HttpClient

Remplacez tout le code réutilisable dans le fichier Program.cs par le code suivant :

[!code-csharp[Main](use-owin-to-self-host-web-api/samples/sample3.cs)]

## <a name="running-the-application"></a>Exécution de l'application

Pour exécuter l’application, appuyez sur F5 dans Visual Studio. La sortie doit se présenter comme suit :

[!code-console[Main](use-owin-to-self-host-web-api/samples/sample4.cmd)]

![](use-owin-to-self-host-web-api/_static/image6.png)

## <a name="additional-resources"></a>Ressources supplémentaires

[Vue d’ensemble du projet Katana](../../../aspnet/overview/owin-and-katana/an-overview-of-project-katana.md)

[Héberger des API Web ASP.NET dans un rôle Worker Azure](host-aspnet-web-api-in-an-azure-worker-role.md)
