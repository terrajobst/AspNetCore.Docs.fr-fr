---
title: Bien démarrer avec les pages Razor dans ASP.NET Core sur macOS avec Visual Studio pour Mac
author: rick-anderson
description: Découvrez comment bien démarrer avec les pages Razor dans ASP.NET Core à l’aide de Visual Studio pour Mac.
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.custom: mvc
ms.date: 10/24/2018
uid: tutorials/razor-pages-mac/razor-pages-start
ms.openlocfilehash: 8a4cc6c52684558ce9176f98205e439096e88cbf
ms.sourcegitcommit: 4d74644f11e0dac52b4510048490ae731c691496
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50089649"
---
# <a name="get-started-with-razor-pages-in-aspnet-core-on-macos-with-visual-studio-for-mac"></a>Bien démarrer avec les pages Razor dans ASP.NET Core sur macOS avec Visual Studio pour Mac

Par [Rick Anderson](https://twitter.com/RickAndMSFT)

Ce didacticiel décrit les principes fondamentaux liés à la génération d’une application web de pages Razor dans ASP.NET Core. Nous vous recommandons de revoir [Présentation des pages Razor](xref:razor-pages/index) avant de commencer ce didacticiel. L’utilisation de pages Razor est la méthode recommandée pour générer l’IU d’applications web dans ASP.NET Core.

## <a name="prerequisites"></a>Prérequis

[!INCLUDE [](~/includes/net-core-prereqs-macos.md)]

## <a name="create-a-razor-web-app"></a>Créer une application web Razor

À partir d’un terminal, exécutez les commandes suivantes :

::: moniker range=">= aspnetcore-2.1"

```console
dotnet new webapp -o RazorPagesMovie
cd RazorPagesMovie
dotnet run
```

::: moniker-end

::: moniker range="= aspnetcore-2.0"

```console
dotnet new razor -o RazorPagesMovie
cd RazorPagesMovie
dotnet run
```

::: moniker-end

Les commandes précédentes utilisent le [CLI .NET Core](/dotnet/core/tools/dotnet) pour créer et exécuter un projet de pages Razor. Ouvrez un navigateur sur http://localhost:5000 pour voir l’application.

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
