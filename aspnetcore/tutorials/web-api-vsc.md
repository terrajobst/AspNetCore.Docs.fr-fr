---
title: Créer une API web avec ASP.NET Core et Visual Studio Code
author: rick-anderson
description: Générer une API web sur macOS, Linux ou Windows avec ASP.NET Core MVC et Visual Studio Code
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 05/08/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/web-api-vsc
ms.openlocfilehash: 9fac4d7b3f687881eafbd63ee71f99bff3b27183
ms.sourcegitcommit: c867d7427bd4a88a78b2322e156367733b532730
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/09/2018
---
# <a name="create-a-web-api-with-aspnet-core-and-visual-studio-code"></a>Créer une API web avec ASP.NET Core et Visual Studio Code

Par [Rick Anderson](https://twitter.com/RickAndMSFT) et [Mike Wasson](https://github.com/mikewasson)

Dans ce didacticiel, créez une API web pour la gestion d’une liste de tâches. Une interface utilisateur n’est pas construite.

Il existe trois versions de ce didacticiel :

* macOS, Linux, Windows : API web avec Visual Studio Code (le présent didacticiel)
* macOS : [API web avec Visual Studio pour Mac](xref:tutorials/first-web-api-mac)
* Windows : [API web avec Visual Studio pour Windows](xref:tutorials/first-web-api)

<!-- WARNING: The code AND images in this doc are used by uid: tutorials/web-api-vsc, tutorials/first-web-api-mac and tutorials/first-web-api. If you change any code/images in this tutorial, update uid: tutorials/web-api-vsc -->

[!INCLUDE[template files](../includes/webApi/intro.md)]

## <a name="prerequisites"></a>Prérequis

[!INCLUDE[prerequisites](~/includes/net-core-prereqs-vscode.md)]

## <a name="create-the-project"></a>Créer le projet

À partir d’une console, exécutez les commandes suivantes :

```console
dotnet new webapi -o TodoApi
code TodoApi
```

Le dossier *TodoApi* s’ouvre dans Visual Studio Code (VS Code). Sélectionnez le fichier *Startup.cs*.

* Sélectionnez **Oui** quand une boîte de dialogue **Avertissement** s’affiche en indiquant le message suivant : « Les composants nécessaires à la build et au débogage sont manquants dans 'TodoApi'. Faut-il les ajouter ? »
* Sélectionnez **Restaurer** quand une boîte de dialogue **Informations** s’affiche en indiquant qu’il existe des dépendances non résolues.

<!-- uid: tutorials/first-mvc-app-xplat/start-mvc uses the pic below. If you change it, make sure it's consistent -->

![VS Code avec le message d’avertissement indiquant « Les composants nécessaires à la build et au débogage sont manquants dans 'TodoApi'. Faut-il les ajouter ?  » Ne plus demander, Pas maintenant, Oui](web-api-vsc/_static/vsc_restore.png)

Appuyez sur **Déboguer** (F5) pour générer et exécuter le programme. Dans un navigateur, accédez à http://localhost:5000/api/values. La sortie suivante s’affiche :

```json
["value1","value2"]
```

Pour obtenir des conseils sur l’utilisation de VS Code, consultez [Aide de Visual Studio Code](#visual-studio-code-help).

## <a name="add-support-for-entity-framework-core"></a>Ajouter la prise en charge d’Entity Framework Core

:::moniker range="<= aspnetcore-2.0"
La création d’un projet dans ASP.NET Core 2.0 ajoute la référence de package [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All) au fichier *TodoApi.csproj* :

[!code-xml[](first-web-api/samples/2.0/TodoApi/TodoApi.csproj?name=snippet_Metapackage&highlight=2)]
:::moniker-end
:::moniker range=">= aspnetcore-2.1"
La création d’un projet dans ASP.NET Core 2.1 ou version ultérieure ajoute la référence de package [Microsoft.AspNetCore.App](https://www.nuget.org/packages/Microsoft.AspNetCore.App) au fichier *TodoApi.csproj* :

[!code-xml[](first-web-api/samples/2.1/TodoApi/TodoApi.csproj?name=snippet_Metapackage&highlight=2)]
:::moniker-end

Il est inutile d’installer le fournisseur de base de données [Entity Framework Core InMemory](/ef/core/providers/in-memory/) séparément. Ce fournisseur de base de données permet d’utiliser Entity Framework Core avec une base de données en mémoire.

## <a name="add-a-model-class"></a>Ajouter une classe de modèle

Un modèle est un objet qui représente les données de votre application. Dans le cas présent, le seul modèle est une tâche.

Ajoutez un dossier nommé *Models*. Vous pouvez placer des classes de modèle n’importe où dans votre projet, mais le dossier *Models* est utilisé par convention.

Ajouter une classe `TodoItem` avec le code suivant :

[!code-csharp[](first-web-api/samples/2.0/TodoApi/Models/TodoItem.cs)]

La base de données génère la valeur `Id` quand un `TodoItem` est créé.

## <a name="create-the-database-context"></a>Créer le contexte de base de données

Le *contexte de base de données* est la classe principale qui coordonne les fonctionnalités d’Entity Framework pour un modèle de données spécifié. Vous créez cette classe en dérivant de la classe `Microsoft.EntityFrameworkCore.DbContext`.

Ajoutez une classe `TodoContext` dans le dossier *Models* :

[!code-csharp[](first-web-api/samples/2.0/TodoApi/Models/TodoContext.cs)]

[!INCLUDE[Register the database context](../includes/webApi/register_dbContext.md)]

## <a name="add-a-controller"></a>Ajouter un contrôleur

Dans le dossier *Contrôleurs*, créez une classe nommée `TodoController`. Remplacez son contenu par le code suivant :

[!INCLUDE[code and get todo items](../includes/webApi/getTodoItems.md)]

### <a name="launch-the-app"></a>Lancer l’application

Dans VS Code, appuyez sur F5 pour lancer l’application. Accédez à http://localhost:5000/api/todo (le contrôleur `Todo` que nous avons créé).

[!INCLUDE[jQuery](../includes/webApi/add-jquery.md)]

[!INCLUDE[last part of web API](../includes/webApi/end.md)]

## <a name="visual-studio-code-help"></a>Aide de Visual Studio Code

* [Prise en main](https://code.visualstudio.com/docs)
* [Débogage](https://code.visualstudio.com/docs/editor/debugging)
* [Terminal intégré](https://code.visualstudio.com/docs/editor/integrated-terminal)
* [Raccourcis clavier](https://code.visualstudio.com/docs/getstarted/keybindings#_keyboard-shortcuts-reference)

  * [Raccourcis clavier macOS](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-macos.pdf)
  * [Raccourcis clavier Linux](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-linux.pdf)
  * [Raccourcis clavier Windows](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-windows.pdf)

[!INCLUDE[next steps](../includes/webApi/next.md)]
