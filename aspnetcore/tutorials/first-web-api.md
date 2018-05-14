---
title: Créer une API web avec ASP.NET Core et Visual Studio pour Windows
author: rick-anderson
description: Générer une API web avec ASP.NET Core MVC et Visual Studio pour Windows
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 04/27/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/first-web-api
ms.openlocfilehash: 962c24a7e654328df7e8893e589e45b19e87b931
ms.sourcegitcommit: 5130b3034165f5cf49d829fe7475a84aa33d2693
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="create-a-web-api-with-aspnet-core-and-visual-studio-for-windows"></a>Créer une API web avec ASP.NET Core et Visual Studio pour Windows

Par [Rick Anderson](https://twitter.com/RickAndMSFT) et [Mike Wasson](https://github.com/mikewasson)

::: moniker range="= aspnetcore-2.1"
[!INCLUDE[](~/includes/2.1.md)]
::: moniker-end

Ce didacticiel génère une API web de gestion d’une liste de tâches. Aucune interface utilisateur n’est créée.

Il existe trois versions de ce didacticiel :

* Windows : API web avec Visual Studio pour Windows (ce didacticiel)
* macOS : [API web avec Visual Studio pour Mac](xref:tutorials/first-web-api-mac)
* macOS, Linux, Windows : [API web avec Visual Studio Code](xref:tutorials/web-api-vsc)

<!-- WARNING: The code AND images in this doc are used by uid: tutorials/web-api-vsc, tutorials/first-web-api-mac and tutorials/first-web-api. If you change any code/images in this tutorial, update uid: tutorials/web-api-vsc -->

[!INCLUDE[intro to web API](../includes/webApi/intro.md)]

## <a name="prerequisites"></a>Prérequis

[!INCLUDE[](~/includes/net-core-prereqs-windows.md)]

## <a name="create-the-project"></a>Créer le projet

Suivez ces étapes dans Visual Studio :

* Dans le menu **Fichier**, sélectionnez **Nouveau** > **Projet**.
* Sélectionnez le modèle **Application web ASP.NET Core**. Nommez le projet *TodoApi*, puis cliquez sur **OK**.
* Dans la boîte de dialogue **Nouvelle application web ASP.NET Core - TodoApi**, choisissez la version ASP.NET Core. Sélectionnez le modèle **API** et cliquez sur **OK**. Ne sélectionnez **pas** **Activer la prise en charge de Docker**.

### <a name="launch-the-app"></a>Lancer l’application

Dans Visual Studio, appuyez sur Ctrl+F5 pour lancer l’application. Visual Studio lance un navigateur et accède à `http://localhost:<port>/api/values`, où `<port>` est un numéro de port choisi de manière aléatoire. Chrome, Microsoft Edge et Firefox affichent la sortie suivante :

```json
["value1","value2"]
```

### <a name="add-a-model-class"></a>Ajouter une classe de modèle

Un modèle est un objet qui représente les données de l’application. Dans le cas présent, le seul modèle est une tâche.

Dans l’Explorateur de solutions, cliquez avec le bouton droit sur le projet. Sélectionnez **Ajouter** > **Nouveau dossier**. Nommez le dossier *Models*.

> [!NOTE]
> Les classes de modèles vont n’importe où dans le projet. Le dossier *Models* est utilisé par convention pour les classes de modèles.

Dans l’Explorateur de solutions, cliquez avec le bouton droit sur le dossier *Models* et sélectionnez **Ajouter** > **Classe**. Nommez la classe *TodoItem* et cliquez sur **Ajouter**.

Ajoutez le code suivant à la classe `TodoItem` :

[!code-csharp[](first-web-api/samples/2.0/TodoApi/Models/TodoItem.cs)]

La base de données génère la valeur `Id` quand un `TodoItem` est créé.

### <a name="create-the-database-context"></a>Créer le contexte de base de données

Le *contexte de base de données* est la classe principale qui coordonne les fonctionnalités d’Entity Framework pour un modèle de données spécifié. Cette classe est créée en dérivant de la classe `Microsoft.EntityFrameworkCore.DbContext`.

Dans l’Explorateur de solutions, cliquez avec le bouton droit sur le dossier *Models* et sélectionnez **Ajouter** > **Classe**. Nommez la classe *TodoContext* et cliquez sur **Ajouter**.

Remplacez la classe par le code suivant :

[!code-csharp[](first-web-api/samples/2.0/TodoApi/Models/TodoContext.cs)]

[!INCLUDE [Register the database context](../includes/webApi/register_dbContext.md)]

### <a name="add-a-controller"></a>Ajouter un contrôleur

Dans l’Explorateur de solutions, cliquez avec le bouton droit sur le dossier *Contrôleurs*. Sélectionnez **Ajouter** > **Nouvel élément**. Dans la boîte de dialogue **Ajouter un nouvel élément**, sélectionnez le modèle **Classe de contrôleur d’API**. Nommez la classe *TodoController* et cliquez sur **Ajouter**.

![Boîte de dialogue Ajouter un nouvel élément avec contrôleur dans la zone de recherche et le contrôleur des API web sélectionné](first-web-api/_static/new_controller.png)

Remplacez la classe par le code suivant :

[!INCLUDE[code and get todo items](../includes/webApi/getTodoItems.md)]

### <a name="launch-the-app"></a>Lancer l’application

Dans Visual Studio, appuyez sur Ctrl+F5 pour lancer l’application. Visual Studio lance un navigateur et accède à `http://localhost:<port>/api/values`, où `<port>` est un numéro de port choisi de manière aléatoire. Accédez au contrôleur `Todo` à l’adresse `http://localhost:<port>/api/todo`.

[!INCLUDE[last part of web API](../includes/webApi/end.md)]

[!INCLUDE[jQuery](../includes/webApi/add-jquery.md)]

[!INCLUDE[next steps](../includes/webApi/next.md)]
