---
uid: aspnet/overview/owin-and-katana/getting-started-with-owin-and-katana
title: Bien démarrer avec OWIN et Katana | Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
ms.date: 09/27/2013
ms.assetid: 6dae249f-5ac6-4f6e-bc49-13bcd5a54a70
msc.legacyurl: /aspnet/overview/owin-and-katana/getting-started-with-owin-and-katana
msc.type: authoredcontent
ms.openlocfilehash: 58a3d234867821d5e23cce2f01e105dfab88ac33
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37826657"
---
<a name="getting-started-with-owin-and-katana"></a>Bien démarrer avec OWIN et Katana
====================
par [Mike Wasson](https://github.com/MikeWasson)

[Open Web Interface pour .NET (OWIN)](http://owin.org/) définit une abstraction entre les serveurs web de .NET et des applications web. En séparant le serveur web à partir de l’application, OWIN rend plus facile de créer des intergiciels (middleware) pour le développement web .NET. En outre, OWIN facilite pour porter les applications web vers d’autres hôtes&#8212;auto-hébergement, par exemple, dans un service Windows ou tout autre processus.

OWIN est une spécification appartenant à la Communauté, pas une implémentation. Le projet Katana est un ensemble de composants OWIN open source développée par Microsoft. Pour obtenir une vue d’ensemble de OWIN et Katana, consultez [une vue d’ensemble du projet Katana](an-overview-of-project-katana.md). Dans cet article, j’ai sera lancez-vous dans le code pour commencer.

Ce didacticiel utilise [Visual Studio 2013 Release Candidate](https://go.microsoft.com/fwlink/?LinkId=306566), mais vous pouvez également utiliser Visual Studio 2012. Quelques étapes sont différentes dans Visual Studio 2012, je remarque ci-dessous.

## <a name="host-owin-in-iis"></a>Héberger OWIN dans IIS

Dans cette section, nous allons héberger OWIN dans IIS. Cette option vous donne la flexibilité et la composabilité d’un pipeline OWIN avec l’ensemble des fonctionnalités matures de IIS. Utilisez cette option, l’application OWIN s’exécute dans le pipeline de demande ASP.NET.

Tout d’abord, créez un nouveau projet d’Application Web ASP.NET. (Dans Visual Studio 2012, utilisez le type de projet d’Application Web ASP.NET vide).

![](getting-started-with-owin-and-katana/_static/image1.png)

Dans le **nouveau projet ASP.NET** boîte de dialogue, sélectionnez le **vide** modèle.

![](getting-started-with-owin-and-katana/_static/image2.png)

### <a name="add-nuget-packages"></a>Ajout de Packages NuGet

Ensuite, ajoutez les packages NuGet nécessaires. À partir de la **outils** menu, sélectionnez **Library Package Manager**, puis sélectionnez **Console du Gestionnaire de Package**. Dans la fenêtre de Console du Gestionnaire de Package, tapez la commande suivante :

`install-package Microsoft.Owin.Host.SystemWeb –Pre`

![](getting-started-with-owin-and-katana/_static/image3.png)

### <a name="add-a-startup-class"></a>Ajoutez une classe de démarrage

Ensuite, ajoutez une classe de démarrage OWIN. Dans l’Explorateur de solutions, cliquez sur le projet et sélectionnez **ajouter**, puis sélectionnez **un nouvel élément**. Dans le **ajouter un nouvel élément** boîte de dialogue, sélectionnez **classe de démarrage Owin**. Pour plus d’informations sur la configuration de la classe de démarrage, consultez [détection de classe de démarrage OWIN](owin-startup-class-detection.md).

![](getting-started-with-owin-and-katana/_static/image4.png)

Ajoutez le code suivant à la méthode `Startup1.Configuration` :

[!code-csharp[Main](getting-started-with-owin-and-katana/samples/sample1.cs?highlight=3)]

Ce code ajoute un simple morceau de l’intergiciel (middleware) au pipeline OWIN, implémenté comme une fonction qui reçoit un **Microsoft.Owin.IOwinContext** instance. Lorsque le serveur reçoit une requête HTTP, le pipeline OWIN appelle l’intergiciel (middleware). L’intergiciel (middleware) définit le type de contenu pour la réponse et écrit le corps de réponse.

> [!NOTE]
> Le modèle de classe de démarrage OWIN est disponible dans Visual Studio 2013. Si vous utilisez Visual Studio 2012, ajoutez simplement une classe vide nommée `Startup1`et collez le code suivant :


[!code-csharp[Main](getting-started-with-owin-and-katana/samples/sample2.cs)]

### <a name="run-the-application"></a>Exécution de l'application

Appuyez sur F5 pour commencer le débogage. Visual Studio ouvre une fenêtre de navigateur à `http://localhost:*port*/`. La page doit ressembler à ce qui suit :

![](getting-started-with-owin-and-katana/_static/image5.png)

## <a name="self-host-owin-in-a-console-application"></a>L’auto-hébergement OWIN dans une Application Console

Il est facile de convertir cette application à partir de l’hébergement IIS pour l’auto-hébergement dans un processus personnalisé. Avec hébergement IIS, IIS sert à la fois le serveur HTTP et que le processus qui héberge le service. Avec l’auto-hébergement, votre application crée le processus et utilise le **HttpListener** classe en tant que le serveur HTTP.

Dans Visual Studio, créez une nouvelle application de console. Dans la fenêtre de Console du Gestionnaire de Package, tapez la commande suivante :

`Install-Package Microsoft.Owin.SelfHost -Pre`

Ajouter un `Startup1` classe à partir de la partie 1 de ce didacticiel au projet. Vous n’avez pas besoin de modifier cette classe.

Implémenter l’application `Main` méthode comme suit.

[!code-csharp[Main](getting-started-with-owin-and-katana/samples/sample3.cs)]

Lorsque vous exécutez l’application console, le serveur commence à écouter les `http://localhost:9000`. Si vous accédez à cette adresse dans un navigateur web, vous verrez la page « Hello world ».

![](getting-started-with-owin-and-katana/_static/image6.png)

## <a name="add-owin-diagnostics"></a>Ajouter des tests de diagnostic OWIN

Le package Microsoft.Owin.Diagnostics contient un intergiciel (middleware) qui intercepte les exceptions non gérées et affiche une page HTML avec les détails de l’erreur. Cette page même manière que la page d’erreur ASP.NET qui est parfois appelée le «[écran jaune de la mort](http://en.wikipedia.org/wiki/Yellow_Screen_of_Death#Yellow)» (YSOD). Comme le YSOD, la page d’erreur Katana est utile lors du développement, mais il est conseillé de le désactiver en mode de production.

Pour installer le package de diagnostic dans votre projet, tapez la commande suivante dans la fenêtre de Console du Gestionnaire de Package :

`install-package Microsoft.Owin.Diagnostics –Pre`

Modifier le code dans votre `Startup1.Configuration` méthode comme suit :

[!code-csharp[Main](getting-started-with-owin-and-katana/samples/sample4.cs?highlight=4,9-12)]

Maintenant utiliser CTRL + F5 pour exécuter l’application sans débogage, afin que Visual Studio s’arrête pas sur l’exception. L’application comporte comme avant, jusqu'à ce que vous accédez à `http://localhost/fail`, moment où l’application lève l’exception. L’intergiciel (middleware) page d’erreur interceptera l’exception et afficher une page HTML avec des informations sur l’erreur. Vous pouvez cliquer sur les onglets pour afficher la pile de chaîne de requête, les cookies, en-tête de demande et les variables d’environnement OWIN.

![](getting-started-with-owin-and-katana/_static/image7.png)

## <a name="next-steps"></a>Étapes suivantes

- [Détection de classe de démarrage OWIN](owin-startup-class-detection.md)
- [Utiliser OWIN pour auto-héberger l’API Web ASP.NET](../../../web-api/overview/hosting-aspnet-web-api/use-owin-to-self-host-web-api.md)
- [Utiliser OWIN pour auto-héberger SignalR](../../../signalr/overview/deployment/tutorial-signalr-self-host.md)
