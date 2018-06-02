---
title: Bien démarrer avec les pages Razor dans ASP.NET Core sur macOS avec Visual Studio pour Mac
author: rick-anderson
description: Découvrez comment bien démarrer avec les pages Razor dans ASP.NET Core à l’aide de Visual Studio pour Mac.
manager: wpickett
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 07/27/2017
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/razor-pages-mac/razor-pages-start
ms.openlocfilehash: 2da9b34f797758c02132d5cf6cc2f2fb2fe6f05a
ms.sourcegitcommit: c79fd3592f444d58e17518914f8873d0a11219c0
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/18/2018
---
# <a name="get-started-with-razor-pages-in-aspnet-core-on-macos-with-visual-studio-for-mac"></a>Bien démarrer avec les pages Razor dans ASP.NET Core sur macOS avec Visual Studio pour Mac

Par [Rick Anderson](https://twitter.com/RickAndMSFT)

Ce didacticiel décrit les principes fondamentaux liés à la génération d’une application web de pages Razor dans ASP.NET Core. Nous vous recommandons de revoir [Présentation des pages Razor](xref:mvc/razor-pages/index) avant de commencer ce didacticiel. L’utilisation de pages Razor est la méthode recommandée pour générer l’IU d’applications web dans ASP.NET Core.

## <a name="prerequisites"></a>Prérequis

[!INCLUDE [](~/includes/net-core-prereqs-macos.md)]

## <a name="create-a-razor-web-app"></a>Créer une application web Razor

À partir d’un terminal, exécutez les commandes suivantes :

```console
dotnet new razor -o RazorPagesMovie
cd RazorPagesMovie
dotnet run
```

Les commandes précédentes utilisent le [CLI .NET Core](https://docs.microsoft.com/dotnet/core/tools/dotnet) pour créer et exécuter un projet de pages Razor. Ouvrez un navigateur sur http://localhost:5000 pour voir l’application.

![Page d’accueil ou page d’index](../razor-pages/razor-pages-start/_static/home.png)

[!INCLUDE [razor-pages-start](../../includes/RP/razor-pages-start.md)]

## <a name="open-the-project"></a>Ouvrir le projet

Appuyez sur Ctrl+C pour arrêter l’application.

Dans Visual Studio, sélectionnez **Fichier > Ouvrir**, puis sélectionnez le fichier *RazorPagesMovie.csproj*.

### <a name="launch-the-app"></a>Lancer l’application

Dans Visual Studio, sélectionnez **Exécuter > Exécuter sans débogage** pour lancer l’application. Visual Studio démarre [Kestrel](xref:fundamentals/servers/kestrel), lance un navigateur, puis accède à `http://localhost:5000`.

Dans le prochain didacticiel, nous allons ajouter un modèle au projet.

> [!div class="step-by-step"]
> [Suivant : Ajout d’un modèle](xref:tutorials/razor-pages-mac/model)
