---
title: Bien démarrer avec ASP.NET Core MVC et Visual Studio
author: rick-anderson
description: Découvrez comment bien démarrer avec ASP.NET Core MVC et Visual Studio.
ms.author: riande
ms.date: 10/07/2017
uid: tutorials/first-mvc-app/start-mvc
ms.openlocfilehash: 9d50607899058c887597a3d73198552d3ef5b020
ms.sourcegitcommit: c4572be5ebb301013a5698caf9b5572b76cb2e34
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/30/2018
ms.locfileid: "52710086"
---
# <a name="get-started-with-aspnet-core-mvc-and-visual-studio"></a>Bien démarrer avec ASP.NET Core MVC et Visual Studio

Par [Rick Anderson](https://twitter.com/RickAndMSFT)

[!INCLUDE [consider RP](~/includes/razor.md)]

Il existe trois versions de ce didacticiel :

* macOS : [Créer une application ASP.NET Core MVC avec Visual Studio pour Mac](xref:tutorials/first-mvc-app-mac/start-mvc)
* Windows : [Créer une application ASP.NET Core MVC avec Visual Studio](xref:tutorials/first-mvc-app/start-mvc)
* macOS, Linux et Windows : [Créer une application ASP.NET Core MVC avec Visual Studio Code](xref:tutorials/first-mvc-app-xplat/start-mvc)

> [!NOTE]
> Nous testons la facilité d’utilisation d’une nouvelle structure proposée pour la table des matières d’ASP.NET Core.  Si vous avez quelques minutes pour essayer un exercice de recherche de 7 différentes rubriques dans la table des matières actuelle ou proposée, [cliquez ici pour participer à l’étude](https://dpk4xbh5.optimalworkshop.com/treejack/rps16hd5).

## <a name="install-visual-studio-and-net-core"></a>Installer Visual Studio et .NET Core

::: moniker range=">= aspnetcore-2.1"

[!INCLUDE [](~/includes/net-core-prereqs-windows.md)]

## <a name="create-a-web-app"></a>Créer une application web

Dans Visual Studio, sélectionnez **Fichier > Nouveau > Projet**.

![Fichier > Nouveau > Projet](start-mvc/_static/alt_new_project.png)

Renseignez la boîte de dialogue **Nouveau projet** :

* Dans le volet gauche, cliquez sur **.NET Core**.
* Dans le volet central, cliquez sur **Application web ASP.NET Core (.NET Core)**.
* Nommez le projet « MvcMovie » (ceci est important pour que l’espace de noms corresponde quand vous copierez le code).
* Appuyez sur **OK**.

![Boîte de dialogue Nouveau projet, .Net Core dans le volet gauche, web ASP.NET Core ](start-mvc/_static/new_project2-21.png)

Renseignez la boîte de dialogue **Nouvelle application web ASP.NET Core (.NET Core) - MvcMovie** :

* Dans la zone de liste déroulante du sélecteur de version, sélectionnez **ASP.NET Core 2.1**.
* Sélectionnez **Application web (Model-View-Controller)**.
* Appuyez sur **OK**.

![Boîte de dialogue Nouveau projet, .Net Core dans le volet gauche, web ASP.NET Core ](start-mvc/_static/new_project22-21.png)

Visual Studio a utilisé un modèle par défaut pour le projet MVC que vous venez de créer. Vous disposez maintenant d’une application fonctionnelle en entrant un nom de projet et en sélectionnant quelques options. Il s’agit d’un projet de démarrage de base qui constitue un bon point de départ.

Appuyez sur **F5** pour exécuter l’application en mode débogage ou sur **Ctrl-F5** pour l’exécuter en mode non-débogage.
<!-- These images are also used by uid: tutorials/first-mvc-app-xplat/start-mvc -->
![application en cours d’exécution](start-mvc/_static/1.png)

* Visual Studio démarre [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) et exécute votre application. Notez que la barre d’adresse affiche `localhost:port#`, et non quelque chose comme `example.com`. C’est parce que `localhost` est le nom d’hôte standard de votre ordinateur local. Quand Visual Studio crée un projet web, un port aléatoire est utilisé pour le serveur web. Dans l’image ci-dessus, le numéro de port est 5000. L’URL dans le navigateur affiche `localhost:5000`. Quand vous exécutez l’application, vous voyez un autre numéro de port.
* Si vous lancez l’application avec **Ctrl+F5** (mode sans débogage), vous pouvez effectuer des modifications du code, enregistrer le fichier, actualiser le navigateur et afficher les modifications du code. De nombreux développeurs préfèrent utiliser le mode non-débogage pour lancer rapidement l’application et voir les modifications.
* Vous pouvez lancer l’application en mode débogage ou non-débogage à partir de l’élément de menu **Déboguer** :

![Menu Déboguer](start-mvc/_static/debug_menu.png)

* Vous pouvez déboguer l’application en appuyant sur le bouton **IIS Express**.

![IIS Express](start-mvc/_static/iis_express.png)

Le modèle par défaut donne des liens **Accueil, À propos** et **Contact** fonctionnels. L’image de navigateur ci-dessus ne montre pas ces liens. En fonction de la taille de votre navigateur, vous devrez peut-être cliquer sur l’icône de navigation pour les afficher.

![Icône de navigation en haut à droite](start-mvc/_static/2.png)

Si vous exécutiez l’application en mode débogage, appuyez sur **Maj-F5** pour arrêter le débogage.

Dans la prochaine partie de ce didacticiel, nous allons découvrir MVC et commencer à écrire du code.

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!INCLUDE [](~/includes/net-core-prereqs.md)]

## <a name="create-a-web-app"></a>Créer une application web

Dans Visual Studio, sélectionnez **Fichier > Nouveau > Projet**.

![Fichier > Nouveau > Projet](start-mvc/_static/alt_new_project.png)

Renseignez la boîte de dialogue **Nouveau projet** :

* Dans le volet gauche, cliquez sur **.NET Core**.
* Dans le volet central, cliquez sur **Application web ASP.NET Core (.NET Core)**.
* Nommez le projet « MvcMovie » (ceci est important pour que l’espace de noms corresponde quand vous copierez le code).
* Appuyez sur **OK**.

![Boîte de dialogue Nouveau projet, .Net Core dans le volet gauche, web ASP.NET Core ](start-mvc/_static/new_project2.png)

Renseignez la boîte de dialogue **Nouvelle application web ASP.NET Core (.NET Core) - MvcMovie** :

* Dans la zone de liste déroulante du sélecteur de version, sélectionnez **ASP.NET Core 2.-**.
* Sélectionnez **Application web (Model-View-Controller)**
* Appuyez sur **OK**.

![Boîte de dialogue Nouveau projet, .Net Core dans le volet gauche, web ASP.NET Core ](start-mvc/_static/new_project22.png)

Visual Studio a utilisé un modèle par défaut pour le projet MVC que vous venez de créer. Vous disposez maintenant d’une application fonctionnelle en entrant un nom de projet et en sélectionnant quelques options. Il s’agit d’un projet de démarrage de base qui constitue un bon point de départ.

Appuyez sur **F5** pour exécuter l’application en mode débogage ou sur **Ctrl-F5** pour l’exécuter en mode non-débogage.
<!-- These images are also used by uid: tutorials/first-mvc-app-xplat/start-mvc -->
![application en cours d’exécution](start-mvc/_static/1.png)

* Visual Studio démarre [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) et exécute votre application. Notez que la barre d’adresse affiche `localhost:port#`, et non quelque chose comme `example.com`. C’est parce que `localhost` est le nom d’hôte standard de votre ordinateur local. Quand Visual Studio crée un projet web, un port aléatoire est utilisé pour le serveur web. Dans l’image ci-dessus, le numéro de port est 5000. L’URL dans le navigateur affiche `localhost:5000`. Quand vous exécutez l’application, vous voyez un autre numéro de port.
* Si vous lancez l’application avec **Ctrl+F5** (mode sans débogage), vous pouvez effectuer des modifications du code, enregistrer le fichier, actualiser le navigateur et afficher les modifications du code. De nombreux développeurs préfèrent utiliser le mode non-débogage pour lancer rapidement l’application et voir les modifications.
* Vous pouvez lancer l’application en mode débogage ou non-débogage à partir de l’élément de menu **Déboguer** :

![Menu Déboguer](start-mvc/_static/debug_menu.png)

* Vous pouvez déboguer l’application en appuyant sur le bouton **IIS Express**.

![IIS Express](start-mvc/_static/iis_express.png)

Le modèle par défaut donne des liens **Accueil, À propos** et **Contact** fonctionnels. L’image de navigateur ci-dessus ne montre pas ces liens. En fonction de la taille de votre navigateur, vous devrez peut-être cliquer sur l’icône de navigation pour les afficher.

![Icône de navigation en haut à droite](start-mvc/_static/2.png)

Si vous exécutiez l’application en mode débogage, appuyez sur **Maj-F5** pour arrêter le débogage.

Dans la prochaine partie de ce didacticiel, nous allons découvrir MVC et commencer à écrire du code.

::: moniker-end

> [!div class="step-by-step"]
> [Next](adding-controller.md)  
