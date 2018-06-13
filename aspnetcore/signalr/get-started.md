---
title: Prise en main de SignalR sur ASP.NET Core
author: rachelappel
description: Dans ce didacticiel, vous créez une application à l’aide de SignalR pour ASP.NET Core.
manager: wpickett
monikerRange: '>= aspnetcore-2.1'
ms.author: rachelap
ms.custom: mvc
ms.date: 05/22/2018
ms.prod: aspnet-core
ms.topic: tutorial
ms.technology: aspnet
uid: signalr/get-started
ms.openlocfilehash: c71d98f86c15a4c6fbbe400f912123419b4ad076
ms.sourcegitcommit: 63fb07fb3f71b32daf2c9466e132f2e7cc617163
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/10/2018
ms.locfileid: "35252202"
---
# <a name="get-started-with-signalr-on-aspnet-core"></a>Prise en main de SignalR sur ASP.NET Core

Par [Rachel Appel](https://twitter.com/rachelappel)

Ce didacticiel explique les principes fondamentaux de la création d’une application en temps réel à l’aide de SignalR pour ASP.NET Core.

   ![Solution](get-started/_static/signalr-get-started-finished.png)

Ce didacticiel présente les tâches de développement SignalR suivantes :

> [!div class="checklist"]
> * Créer un SignalR sur une application web ASP.NET Core.
> * Créer un hub SignalR pour envoyer du contenu aux clients.
> * Modifier la classe `Startup` et configurer l’application.

[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/get-started/sample/) ([procédure de téléchargement](xref:tutorials/index#how-to-download-a-sample))

# <a name="prerequisites"></a>Prérequis

Installez les logiciels suivants :


# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* [.NET core SDK 2.1 ou version ultérieure](https://www.microsoft.com/net/download/all)
* [Visual Studio 2017](https://www.visualstudio.com/downloads/) 15.7 ou version ultérieure avec la charge de travail **Développement ASP.NET et web**
* [npm](https://www.npmjs.com/get-npm)

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

* [.NET core SDK 2.1 ou version ultérieure](https://www.microsoft.com/net/download/all)
* [Visual Studio Code](https://code.visualstudio.com/download)
* [C# pour le Code de Visual Studio](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)
* [npm](https://www.npmjs.com/get-npm)

-----

## <a name="create-an-aspnet-core-project-that-hosts-signalr-client-and-server"></a>Créer un projet ASP.NET Core qui héberge le serveur et le client SignalR

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio/)

1. Utilisez l'option de menu **Fichier** > **Nouveau projet** et choisissez **Application ASP.NET Core Web**. Nommez le projet *SignalRChat*.

   ![Boîte de dialogue Nouveau projet dans Visual Studio](get-started/_static/signalr-new-project-dialog.png)

2. Sélectionnez **Web Application** pour créer un projet à l’aide des Pages Razor. Puis sélectionnez **OK**. Assurez-vous que **ASP.NET Core 2.1** est sélectionné dans le sélecteur de framework, même si SignalR peut s’exécuter sur des versions antérieures de .NET.

   ![Boîte de dialogue Nouveau projet dans Visual Studio](get-started/_static/signalr-new-project-choose-type.png)

Visual Studio inclut le package `Microsoft.AspNetCore.SignalR` contenant ses bibliothèques du serveur dans son modèle **Application web ASP.NET Core**. Toutefois, la bibliothèque cliente JavaScript pour SignalR doit être installée à l’aide de *npm*.

3. Exécutez les commandes suivantes dans la fenêtre **Console du gestionnaire de package**, à partir de la racine du projet :

    ```console
    npm init -y
    npm install @aspnet/signalr
    ```     

4. Créez un dossier nommé « signalr » à l’intérieur du dossier *lib* dans votre projet. Copiez le fichier *signalr.js* à partir de *node_modules\\ @aspnet\signalr\dist\browser*  dans ce dossier.

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code/)

1. À partir du **Terminal intégré**, exécutez la commande suivante :

    ```console
    dotnet new webapp -o SignalRChat
    ```

    [!INCLUDE[](~/includes/webapp-alias-notice.md)]

2. Installez la bibliothèque cliente JavaScript en utilisant *npm*.

    ```console
    npm init -y
    npm install @aspnet/signalr
    ```

3. Créez un dossier nommé « signalr » à l’intérieur du dossier *lib* dans votre projet. Copiez le fichier *signalr.js* à partir de *node_modules\\ @aspnet\signalr\dist\browser*  dans ce dossier.

---

## <a name="create-the-signalr-hub"></a>Création du Hub SignalR

Un hub est une classe servant de pipeline de haut niveau qui permet au client et au serveur d'appeler des méthodes l’un sur l’autre.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio/)

1. Ajoutez une classe au projet en choisissant **Fichier** > **Nouveau** > **Fichier** et en sélectionnant **Classe Visual c#**. Nommez le fichier *ChatHub*. 

2. Héritez de `Microsoft.AspNetCore.SignalR.Hub`. La classe `Hub` contient des propriétés et des événements pour la gestion des connexions et des groupes, ainsi que pour l'envoi et la réception des données.

3. Créez la méthode `SendMessage` qui envoie un message à tous les clients connectés à la conversation. Notez qu’elle renvoie une [tâche](https://msdn.microsoft.com/library/system.threading.tasks.task(v=vs.110).aspx), car SignalR est asynchrone. Le Code asynchrone évolue mieux.

   [!code-csharp[Startup](get-started/sample/Hubs/ChatHub.cs)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code/)

1. Ouvrez le dossier *SignalRChat* dans Visual Studio Code.

2. Ajoutez une classe au projet en sélectionnant **Fichier** > **Nouveau fichier** à partir du menu.

3. Héritez de `Microsoft.AspNetCore.SignalR.Hub`. La classe `Hub` contient des propriétés et des événements pour la gestion des connexions et des groupes, ainsi que pour l'envoi et la réception des données à des clients.

4. Ajoutez une méthode `SendMessage` à la classe. La méthode `SendMessage` envoie un message à tous les clients connectés à la conversation. Notez qu’elle renvoie une [tâche](/dotnet/api/system.threading.tasks.task), car SignalR est asynchrone. Le code asynchrone évolue mieux.

   [!code-csharp[Startup](get-started/sample/Hubs/ChatHub.cs?range=6-12)]

-----

## <a name="configure-the-project-to-use-signalr"></a>Configurer le projet pour utiliser SignalR

Le serveur de SignalR doit être configuré de sorte qu’il sache transmettre les demandes à SignalR.

1. Pour configurer un projet SignalR, modifiez la méthode `Startup.ConfigureServices` du projet.

   `services.AddSignalR` ajoute SignalR dans le cadre du pipeline de l'[intergiciel (middleware)](xref:fundamentals/middleware/index).

2. Configurez des routes pour vos hubs en utilisant `UseSignalR`.


   [!code-csharp[Startup](get-started/sample/Startup.cs?highlight=37,57-60)]


## <a name="create-the-signalr-client-code"></a>Créer le code de client SignalR

1. Ajoutez un fichier JavaScript nommé *chat.js* au dossier *wwwroot\js*. Ajoutez-y le code suivant :

   [!code-javascript[Index](get-started/sample/wwwroot/js/chat.js)]

2. Remplacez le contenu de *Pages\Index.cshtml* avec le code suivant :

   [!code-cshtml[Index](get-started/sample/Pages/Index.cshtml)]

   Le code HTML précédent affiche le nom et les champs de message et un bouton d’envoi. Notez les références de script en bas : une référence à SignalR et *chat.js*.


## <a name="run-the-app"></a>Exécuter l'application

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

1. Sélectionnez **Débogage** > **Démarrer sans débogage** pour lancer un navigateur et charger le site web localement. Copiez l’URL de la barre d’adresse.

1. Ouvrez une autre instance du navigateur (n’importe quel navigateur) et collez l’URL dans la barre d’adresses.

1. Choisissez un navigateur, entrez un nom et un message, puis cliquez sur le bouton **Envoyer**. Le nom et le message sont affichés sur les deux pages instantanément.

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

1. Appuyez sur **Débogage** (F5) pour générer et exécuter le programme. L’exécution du programme ouvre une fenêtre de navigateur.

1. Ouvrir une autre fenêtre de navigateur et de charger le site Web localement dans celui-ci.

1. Choisissez un navigateur, entrez un nom et un message, puis cliquez sur le bouton **Envoyer**. Le nom et le message sont affichés sur les deux pages instantanément.

---

  ![Solution](get-started/_static/signalr-get-started-finished.png)

## <a name="related-resources"></a>Ressources connexes

[Introduction à ASP.NET Core SignalR](introduction.md)
