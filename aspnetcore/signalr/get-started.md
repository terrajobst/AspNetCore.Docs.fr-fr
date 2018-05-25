---
title: Prise en main SignalR sur ASP.NET Core
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
ms.openlocfilehash: fb448f22db5bb624083f07cefaad29d6d58331ef
ms.sourcegitcommit: 300a1127957dcdbce1b6ad79a7b9dc676f571510
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/23/2018
---
# <a name="get-started-with-signalr-on-aspnet-core"></a>Prise en main SignalR sur ASP.NET Core

Par [Rachel Appel](https://twitter.com/rachelappel)

Ce didacticiel explique les principes fondamentaux de la création d’une application en temps réel à l’aide de SignalR pour ASP.NET Core.

   ![Solution](get-started/_static/signalr-get-started-finished.png)

Ce didacticiel présente les tâches de développement SignalR suivantes :

> [!div class="checklist"]
> * Créez un SignalR sur l’application web ASP.NET Core.
> * Créer un concentrateur SignalR pour transmettre le contenu aux clients.
> * Modifier la `Startup` classe et de configurer l’application.

[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/get-started/sample/) ([procédure de téléchargement](xref:tutorials/index#how-to-download-a-sample))

# <a name="prerequisites"></a>Prérequis

Installer les logiciels suivants :

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* [Kit de développement .NET core 2.1.0 RC 1](https://www.microsoft.com/net/download/dotnet-core/sdk-2.1.300-rc1) ou version ultérieure
* [Visual Studio 2017](https://www.visualstudio.com/downloads/) 15,7 ou version ultérieure avec le **développement web ASP.NET et** la charge de travail
* [npm](https://www.npmjs.com/get-npm)

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

* [Kit de développement .NET core 2.1.0 RC 1](https://www.microsoft.com/net/download/dotnet-core/sdk-2.1.300-rc1) ou version ultérieure
* [Visual Studio Code](https://code.visualstudio.com/download)
* [C# pour le Code de Visual Studio](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)
* [npm](https://www.npmjs.com/get-npm)

-----

## <a name="create-an-aspnet-core-project-that-hosts-signalr-client-and-server"></a>Créez un projet ASP.NET Core qui héberge le serveur et client SignalR

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio/)

1. Utilisez le **fichier** > **nouveau projet** menu option et choisissez **Application ASP.NET Core Web**. Nommez le projet *SignalRChat*.

   ![Boîte de dialogue Nouveau projet dans Visual Studio](get-started/_static/signalr-new-project-dialog.png)

2. Sélectionnez **Web Application** pour créer un projet à l’aide des Pages Razor. Puis sélectionnez **OK**. Assurez-vous que **ASP.NET Core 2.1** est sélectionné dans le sélecteur de framework, si SignalR s’exécute sur des versions antérieures de .NET.

   ![Boîte de dialogue Nouveau projet dans Visual Studio](get-started/_static/signalr-new-project-choose-type.png)

Visual Studio inclut la `Microsoft.AspNetCore.SignalR` package contenant ses bibliothèques du serveur dans le cadre de son **Application ASP.NET Core Web** modèle. Toutefois, la bibliothèque cliente JavaScript pour SignalR doit être installée à l’aide de *npm*.

3. Exécutez les commandes suivantes le **Package Manager Console** fenêtre, à partir de la racine du projet :

    ```console
    npm init -y
    npm install @aspnet/signalr
    ```     

4. Copie le *signalr.js* à partir du fichier *node_modules\\ @aspnet\signalr\dist\browser*  à la *lib* dossier dans votre projet.

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code/)

1. À partir de la **Terminal Server intégré**, exécutez la commande suivante :

    ```console
    dotnet new razor -o SignalRChat
    ```

2. Installer la bibliothèque cliente JavaScript à l’aide *npm*.

    ```console
    npm init -y
    npm install @aspnet/signalr
    ```

3. Copie le *signalr.js* à partir du fichier *node_modules\\ @aspnet\signalr\dist\browser*  à la *lib* dossier dans votre projet.

-----

## <a name="create-the-signalr-hub"></a>Création du Hub SignalR

Un concentrateur est une classe qui sert d’un pipeline de haut niveau qui permet au client et le serveur appeler des méthodes sur eux.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio/)

1. Ajoutez une classe au projet en choisissant **fichier** > **nouveau** > **fichier** et en sélectionnant **classe Visual c#**.

2. Hériter de `Microsoft.AspNetCore.SignalR.Hub`. La `Hub` classe contient des propriétés et des événements pour la gestion des connexions et des groupes, ainsi que des données d’envoi et de réception.

3. Créer le `SendMessage` méthode qui envoie un message à tous les clients de conversation connecté. Notez qu’il renvoie un [tâche](https://msdn.microsoft.com/library/system.threading.tasks.task(v=vs.110).aspx), car SignalR est asynchrone. Code asynchrone évolue mieux.

   [!code-csharp[Startup](get-started/sample/Hubs/ChatHub.cs)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code/)

1. Ouvrez le *SignalRChat* dossier dans le Code de Visual Studio.

2. Ajoutez une classe au projet en sélectionnant **fichier** > **nouveau fichier** à partir du menu.

3. Hériter de `Microsoft.AspNetCore.SignalR.Hub`. La `Hub` classe contient des propriétés et des événements pour la gestion des connexions et les groupes, ainsi que d’envoyer et recevoir des données vers les clients.

4. Ajoutez une méthode `SendMessage` à la classe. Le `SendMessage` méthode envoie un message à tous les clients de conversation connecté. Notez qu’il renvoie un [tâche](/dotnet/api/system.threading.tasks.task), car SignalR est asynchrone. Code asynchrone évolue mieux.

   [!code-csharp[Startup](get-started/sample/Hubs/ChatHub.cs?range=6-12)]

-----

## <a name="configure-the-project-to-use-signalr"></a>Configurer le projet pour utiliser SignalR

Le serveur de SignalR doit être configuré de sorte qu’il sache pour transmettre les demandes à SignalR.

1. Pour configurer un projet SignalR, modifiez du projet `Startup.ConfigureServices` (méthode).

   `services.AddSignalR` Ajoute SignalR dans le cadre de la [intergiciel (middleware)](xref:fundamentals/middleware/index) pipeline.

2. Configurer des itinéraires à vos concentrateurs à l’aide de `UseSignalR`.


   [!code-csharp[Startup](get-started/sample/Startup.cs?highlight=37,57-60)]


## <a name="create-the-signalr-client-code"></a>Créer le code de client SignalR

1. Remplacez le contenu de *Pages\Index.cshtml* avec le code suivant :

   [!code-cshtml[Index](get-started/sample/Pages/Index.cshtml)]

   Le code HTML précédent affiche le nom et les champs de message et un bouton d’envoi. Notez les références de script en bas : une référence à SignalR et *chat.js*.

2. Ajouter un fichier JavaScript nommé *chat.js*, à la *wwwroot\js* dossier. Ajoutez-y le code suivant :

   [!code-javascript[Index](get-started/sample/wwwroot/js/chat.js)]

## <a name="run-the-app"></a>Exécuter l'application

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

1. Sélectionnez **déboguer** > **démarrer sans débogage** pour lancer un navigateur et de charger le site Web localement. Copiez l’URL de la barre d’adresses.

1. Ouvrez une autre instance du navigateur (n’importe quel navigateur) et collez l’URL dans la barre d’adresses.

1. Choisissez un navigateur, entrez un nom et un message, puis cliquez sur le **envoyer** bouton. Le nom et le message sont affichés sur les deux pages instantanément.

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

1. Appuyez sur **Déboguer** (F5) pour générer et exécuter le programme. Le programme en cours d’exécution, une fenêtre de navigateur s’ouvre.

1. Ouvrir une autre fenêtre de navigateur et de charger le site Web localement dans celui-ci.

1. Choisissez un navigateur, entrez un nom et un message, puis cliquez sur le **envoyer** bouton. Le nom et le message sont affichés sur les deux pages instantanément.

---

  ![Solution](get-started/_static/signalr-get-started-finished.png)

## <a name="related-resources"></a>Ressources connexes

[Introduction à ASP.NET Core SignalR](introduction.md)
