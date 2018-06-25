---
title: Présentation d’ASP.NET Core MVC sur macOS, Linux ou Windows
author: rick-anderson
description: Découvrez comment bien démarrer avec ASP.NET Core MVC et Visual Studio Code sur macOS, Linux et Windows
ms.author: riande
ms.date: 07/07/2017
uid: tutorials/first-mvc-app-xplat/start-mvc
ms.openlocfilehash: b25ee29541e345a3bf6700b67d992409c02b183a
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/20/2018
ms.locfileid: "36275273"
---
# <a name="introduction-to-aspnet-core-mvc-on-macos-linux-or-windows"></a>Présentation d’ASP.NET Core MVC sur macOS, Linux ou Windows

Par [Rick Anderson](https://twitter.com/RickAndMSFT)

Ce didacticiel présente les principes de base de la génération d’une application web ASP.NET Core MVC à l’aide de [Visual Studio Code](https://code.visualstudio.com) (VS Code). Il part du principe que vous connaissez déjà VS Code. Pour plus d’informations, consultez [Bien démarrer avec VS Code](https://code.visualstudio.com/docs) et [Aide de Visual Studio Code](#visual-studio-code-help). 

[!INCLUDE [consider RP](../../includes/razor.md)]

Il existe trois versions de ce didacticiel :

* macOS : [Créer une application ASP.NET Core MVC avec Visual Studio pour Mac](xref:tutorials/first-mvc-app-mac/start-mvc)
* Windows : [Créer une application ASP.NET Core MVC avec Visual Studio](xref:tutorials/first-mvc-app/start-mvc)
* macOS, Linux et Windows : [Créer une application ASP.NET Core MVC avec Visual Studio Code](xref:tutorials/first-mvc-app-xplat/start-mvc) 

## <a name="prerequisites"></a>Prérequis

[!INCLUDE [](~/includes/net-core-prereqs-vscode.md)]

## <a name="create-a-web-app-with-dotnet"></a>Créer une application web avec dotnet

À partir d’un terminal, exécutez les commandes suivantes :

```console
mkdir MvcMovie
cd MvcMovie
dotnet new mvc
```

Ouvrez le dossier *MvcMovie* dans Visual Studio Code (VS Code), puis sélectionnez le fichier *Startup.cs*.

- Sélectionnez **Oui** quand une boîte de dialogue **Avertissement** s’affiche en indiquant le message suivant : « Les composants nécessaires à la build et au débogage sont manquants dans 'MvcMovie'. Faut-il les ajouter ? »
- Sélectionnez **Restaurer** quand une boîte de dialogue **Informations** s’affiche en indiquant qu’il existe des dépendances non résolues.

![VS Code avec le message d’avertissement indiquant « Les composants nécessaires à la build et au débogage sont manquants dans 'MvcMovie'. Faut-il les ajouter ? » Ne plus demander, Pas maintenant, Oui et également Info - Il existe des dépendances non résolues - Restaurer - Fermer](../web-api-vsc/_static/vsc_restore.png)

Appuyez sur **Déboguer** (F5) pour générer et exécuter le programme.

![application en cours d’exécution](../first-mvc-app/start-mvc/_static/1.png)

VS Code démarre le serveur web [Kestrel](xref:fundamentals/servers/kestrel) et exécute votre application. Notez que la barre d’adresse affiche `localhost:5000`, et non quelque chose comme `example.com`. En effet, `localhost` est le nom d’hôte standard de votre ordinateur local.

Le modèle par défaut donne des liens **Accueil, À propos** et **Contact** fonctionnels. L’image de navigateur ci-dessus ne montre pas ces liens. En fonction de la taille de votre navigateur, vous devrez peut-être cliquer sur l’icône de navigation pour les afficher.

![Icône de navigation en haut à droite](../first-mvc-app/start-mvc/_static/2.png)

Dans la prochaine partie de ce didacticiel, nous allons découvrir MVC et commencer à écrire du code.

## <a name="visual-studio-code-help"></a>Aide de Visual Studio Code

- [Prise en main](https://code.visualstudio.com/docs)
- [Débogage](https://code.visualstudio.com/docs/editor/debugging)
- [Terminal intégré](https://code.visualstudio.com/docs/editor/integrated-terminal)
- [Raccourcis clavier](https://code.visualstudio.com/docs/getstarted/keybindings#_keyboard-shortcuts-reference)

  - [Raccourcis clavier macOS](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-macos.pdf)
  - [Raccourcis clavier Linux](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-linux.pdf)
  - [Raccourcis clavier Windows](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-windows.pdf)

> [!div class="step-by-step"]
> [Suivant - Ajouter un contrôleur](adding-controller.md)
