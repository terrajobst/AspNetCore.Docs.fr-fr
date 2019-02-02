---
uid: web-api/overview/hosting-aspnet-web-api/use-owin-to-self-host-web-api
title: Utiliser OWIN pour auto-héberger l’API Web ASP.NET | Microsoft Docs
author: rick-anderson
description: Ce didacticiel montre comment héberger des API Web ASP.NET dans une application console, à l’aide d’OWIN pour auto-héberger l’infrastructure API Web. Open Web Interface pour .NET (OWIN) d...
ms.author: riande
ms.date: 07/09/2013
ms.assetid: a90a04ce-9d07-43ad-8250-8a92fb2bd3d5
msc.legacyurl: /web-api/overview/hosting-aspnet-web-api/use-owin-to-self-host-web-api
msc.type: authoredcontent
ms.openlocfilehash: 59ce24aa47ca590fbe9b617dbbe8bc6b3711849e
ms.sourcegitcommit: ed76cc752966c604a795fbc56d5a71d16ded0b58
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/02/2019
ms.locfileid: "55667386"
---
<a name="use-owin-to-self-host-aspnet-web-api"></a>Utiliser OWIN pour auto-héberger l’API Web ASP.NET 
====================

> Ce didacticiel montre comment héberger des API Web ASP.NET dans une application console, à l’aide d’OWIN pour auto-héberger l’infrastructure API Web.
>
> [Open Web Interface pour .NET](http://owin.org) (OWIN) définit une abstraction entre les serveurs web de .NET et des applications web. OWIN dissocie de l’application web à partir du serveur, ce qui rend OWIN idéale pour l’hébergement automatique d’une application web dans votre propre processus, en dehors d’IIS.
>
> ## <a name="software-versions-used-in-the-tutorial"></a>Versions des logiciels utilisées dans le didacticiel
>
>
> - [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/) 
> - API 5.2.7 Web


> [!NOTE]
> Vous trouverez le code source complet pour ce didacticiel à [aspnet.codeplex.com](https://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OwinSelfhostSample/ReadMe.txt).


## <a name="create-a-console-application"></a>Créer une application console

Sur le **fichier** menu, **New**, puis sélectionnez **projet**. À partir de **installé**, sous **Visual C#** , sélectionnez **Windows Desktop** , puis sélectionnez **application Console (.Net Framework)**. Nommez le projet « OwinSelfhostSample » et sélectionnez **OK**.

[![](use-owin-to-self-host-web-api/_static/image7.png)](use-owin-to-self-host-web-api/_static/image7.png)

## <a name="add-the-web-api-and-owin-packages"></a>Ajoutez les packages de l’API Web et la bibliothèque OWIN

À partir de la **outils** menu, sélectionnez **Gestionnaire de Package NuGet**, puis sélectionnez **Console du Gestionnaire de Package**. Dans la fenêtre de Console du Gestionnaire de Package, entrez la commande suivante :

`Install-Package Microsoft.AspNet.WebApi.OwinSelfHost`

Cela installera le package de selfhost WebAPI OWIN et tous les packages OWIN requis.

[![](use-owin-to-self-host-web-api/_static/image4.png)](use-owin-to-self-host-web-api/_static/image3.png)

## <a name="configure-web-api-for-self-host"></a>Configurer les API Web pour Self-host

Dans l’Explorateur de solutions, cliquez sur le projet et sélectionnez **ajouter** / **classe** pour ajouter une nouvelle classe. Nommez la classe `Startup`.

![](use-owin-to-self-host-web-api/_static/image5.png)

Remplacez tout le code réutilisable dans ce fichier avec les éléments suivants :

[!code-csharp[Main](use-owin-to-self-host-web-api/samples/sample1.cs)]

## <a name="add-a-web-api-controller"></a>Ajouter un contrôleur d’API Web

Ensuite, ajoutez une classe de contrôleur d’API Web. Dans l’Explorateur de solutions, cliquez sur le projet et sélectionnez **ajouter** / **classe** pour ajouter une nouvelle classe. Nommez la classe `ValuesController`.

Remplacez tout le code réutilisable dans ce fichier avec les éléments suivants :

[!code-csharp[Main](use-owin-to-self-host-web-api/samples/sample2.cs)]

## <a name="start-the-owin-host-and-make-a-request-with-httpclient"></a>Démarrer l’hôte OWIN et faire une demande avec HttpClient

Remplacez tout le code réutilisable dans le fichier Program.cs par le code suivant :

[!code-csharp[Main](use-owin-to-self-host-web-api/samples/sample3.cs)]

## <a name="run-the-application"></a>Exécuter l'application

Pour exécuter l’application, appuyez sur F5 dans Visual Studio. La sortie doit se présenter comme suit :

[!code-console[Main](use-owin-to-self-host-web-api/samples/sample4.cmd)]

![](use-owin-to-self-host-web-api/_static/image6.png)

## <a name="additional-resources"></a>Ressources supplémentaires

[Vue d’ensemble du projet Katana](../../../aspnet/overview/owin-and-katana/an-overview-of-project-katana.md)

[Héberger des API Web ASP.NET dans un rôle Worker Azure](host-aspnet-web-api-in-an-azure-worker-role.md)
