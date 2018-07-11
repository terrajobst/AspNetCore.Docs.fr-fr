---
title: Bien démarrer avec SignalR sur ASP.NET Core
author: rachelappel
description: Dans ce tutoriel, vous allez créer une application à l’aide de SignalR pour ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: rachelap
ms.custom: mvc
ms.date: 05/22/2018
uid: tutorials/signalr
ms.openlocfilehash: 6b8222ee04573ca7157b4e1125ed5a4453b2b9a9
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37830553"
---
# <a name="get-started-with-signalr-on-aspnet-core"></a>Bien démarrer avec SignalR sur ASP.NET Core

Par [Rachel Appel](https://twitter.com/rachelappel)

Ce tutoriel explique les principes fondamentaux de la création d’une application en temps réel à l’aide de SignalR pour ASP.NET Core.

   ![Solution](signalr/_static/signalr-get-started-finished.png)

Ce tutoriel présente les tâches de développement SignalR suivantes :

> [!div class="checklist"]
> * Créer une application web SignalR sur ASP.NET Core
> * Créer un hub SignalR pour transmettre le contenu aux clients
> * Modifier la classe `Startup` et configurer l’application

[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/signalr/sample) ([procédure de téléchargement](xref:tutorials/index#how-to-download-a-sample))

## <a name="prerequisites"></a>Prérequis

Installez les logiciels suivants :

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* [Kit SDK .NET Core 2.1 ou version ultérieure](https://www.microsoft.com/net/download/all)
* [Visual Studio 2017](https://www.visualstudio.com/downloads/) version 15.7.3 ou ultérieure avec la charge de travail **ASP.NET et développement web**
* [npm](https://www.npmjs.com/get-npm)

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

* [Kit SDK .NET Core 2.1 ou version ultérieure](https://www.microsoft.com/net/download/all)
* [Visual Studio Code](https://code.visualstudio.com/download)
* [C# pour Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)
* [npm](https://www.npmjs.com/get-npm)

-----

## <a name="create-an-aspnet-core-project-that-hosts-signalr-client-and-server"></a>Créer un projet ASP.NET Core qui héberge le serveur et le client SignalR

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio/)

1. Utilisez l’option de menu **Fichier** > **Nouveau projet** et choisissez **Application web ASP.NET Core**. Nommez le projet *SignalRChat*.

   ![Boîte de dialogue Nouveau projet dans Visual Studio](signalr/_static/signalr-new-project-dialog.png)

2. Sélectionnez **Application web** pour créer un projet à l’aide de Razor Pages. Sélectionnez **OK**. Vérifiez que **ASP.NET Core 2.1** est sélectionné dans le sélecteur de framework, même si SignalR s’exécute sur des versions antérieures de .NET.

   ![Boîte de dialogue Nouveau projet dans Visual Studio](signalr/_static/signalr-new-project-choose-type.png)

Visual Studio inclut le package `Microsoft.AspNetCore.SignalR` contenant ses bibliothèques de serveur dans le cadre de son modèle **Application web ASP.NET Core**. Toutefois, la bibliothèque cliente JavaScript pour SignalR doit être installée à l’aide de *npm*.

3. Exécutez les commandes suivantes dans la fenêtre **Console du Gestionnaire de package** à partir de la racine du projet :

    ```console
    npm init -y
    npm install @aspnet/signalr
    ```

4. Créez un dossier nommé « signalr » à l’intérieur du dossier *lib* dans votre projet. Copiez le fichier *signalr.js* de *node_modules\\@aspnet\signalr\dist\browser* vers ce dossier.

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code/)

1. À partir du **Terminal intégré**, exécutez la commande suivante :

    ```console
    dotnet new webapp -o SignalRChat
    ```

    [!INCLUDE[](~/includes/webapp-alias-notice.md)]

2. Installez la bibliothèque cliente JavaScript à l’aide de *npm*.

    ```console
    npm init -y
    npm install @aspnet/signalr
    ```

3. Créez un dossier nommé « signalr » à l’intérieur du dossier *lib* dans votre projet. Copiez le fichier *signalr.js* de *node_modules\\@aspnet\signalr\dist\browser* vers ce dossier.

---

## <a name="create-the-signalr-hub"></a>Créer le hub SignalR

Un hub est une classe qui sert de pipeline global permettant au client et au serveur d’appeler des méthodes de l’un à l’autre.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio/)

1. Ajoutez une classe au projet en choisissant **Fichier** > **Nouveau** > **Fichier** et en sélectionnant **Classe Visual C#**. Nommez la classe `ChatHub` et le fichier *ChatHub.cs*.

2. Héritez de `Microsoft.AspNetCore.SignalR.Hub`. La classe `Hub` contient des propriétés et des événements pour la gestion des connexions et des groupes, ainsi que l’envoi et la réception de données.

3. Créez la méthode `SendMessage` qui envoie un message à tous les clients de conversation connectés. Notez qu’elle retourne une [Tâche](https://msdn.microsoft.com/library/system.threading.tasks.task(v=vs.110).aspx), car SignalR est asynchrone. Le code asynchrone est plus scalable.

   [!code-csharp[Startup](signalr/sample/Hubs/ChatHub.cs)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code/)

1. Ouvrez le dossier *SignalRChat* dans Visual Studio Code.

2. Ajoutez une classe au projet en sélectionnant **Fichier** > **Nouveau fichier** dans le menu. Nommez la classe `ChatHub` et le fichier *ChatHub.cs*.

3. Héritez de `Microsoft.AspNetCore.SignalR.Hub`. La classe `Hub` contient des propriétés et des événements pour la gestion des connexions et des groupes, ainsi que l’envoi et la réception de données avec les clients.

4. Ajoutez une méthode `SendMessage` à la classe. La méthode `SendMessage` envoie un message à tous les clients de conversation connectés. Notez qu’elle retourne une [Tâche](/dotnet/api/system.threading.tasks.task), car SignalR est asynchrone. Le code asynchrone est plus scalable.

   [!code-csharp[Startup](signalr/sample/Hubs/ChatHub.cs)]

-----

## <a name="configure-the-project-to-use-signalr"></a>Configurer le projet pour utiliser SignalR

Vous devez configurer le serveur SignalR pour qu’il sache qu’il doit transmettre les requêtes à SignalR.

1. Pour configurer un projet SignalR, modifiez la méthode `Startup.ConfigureServices` du projet.

   `services.AddSignalR` rend les services SignalR accessibles au système d’[injection de dépendances](xref:fundamentals/dependency-injection).

1. Configurez des itinéraires vers vos hubs avec `UseSignalR` dans la méthode `Configure`. `app.UseSignalR` ajoute SignalR au pipeline d’[intergiciel (middleware)](xref:fundamentals/middleware/index).

   [!code-csharp[Startup](signalr/sample/Startup.cs?highlight=37,57-60)]

## <a name="create-the-signalr-client-code"></a>Créer le code client SignalR

1. Ajoutez un fichier JavaScript nommé *chat.js* au dossier *wwwroot\js*. Ajoutez-y le code suivant :

   [!code-javascript[Index](signalr/sample/wwwroot/js/chat.js)]

2. Remplacez le contenu de *Pages\Index.cshtml* par le code suivant :

   [!code-cshtml[Index](signalr/sample/Pages/Index.cshtml)]

   Le code HTML précédent affiche des champs de nom et de message, ainsi qu’un bouton d’envoi. Notez les références de script en bas : une référence à SignalR et *chat.js*.

## <a name="run-the-app"></a>Exécuter l'application

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

1. Sélectionnez **Déboguer** > **Démarrer sans débogage** pour lancer un navigateur et chargez le site web localement. Copiez l’URL à partir de la barre d’adresse.

1. Ouvrez une autre instance du navigateur (n’importe quel navigateur) et collez l’URL dans la barre d’adresse.

1. Choisissez l’un des navigateurs, entrez un nom et un message, puis cliquez sur le bouton **Send**. Le nom et le message sont affichés instantanément dans les deux pages.

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

1. Appuyez sur **Déboguer** (F5) pour générer et exécuter le programme. L’exécution du programme ouvre une fenêtre de navigateur.

1. Ouvrez une autre fenêtre de navigateur et chargez-y le site web localement.

1. Choisissez l’un des navigateurs, entrez un nom et un message, puis cliquez sur le bouton **Send**. Le nom et le message sont affichés instantanément dans les deux pages.

---

  ![Solution](signalr/_static/signalr-get-started-finished.png)

## <a name="related-resources"></a>Ressources connexes

[Introduction à ASP.NET Core SignalR](xref:signalr/introduction)
