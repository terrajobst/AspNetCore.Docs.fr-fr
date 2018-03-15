---
title: Prise en main SignalR sur ASP.NET Core
author: rachelappel
description: "Découvrez les principes fondamentaux de la création d’une application en temps réel à l’aide de SignalR pour ASP.NET Core."
manager: wpickett
ms.author: rachelap
ms.custom: mvc
ms.date: 03/06/2018
ms.prod: aspnet-core
ms.technology: dotnet-signalr
ms.topic: tutorial
uid: signalr/get-started-signalr-core
ms.openlocfilehash: 79af59fc8c2ada71d764ada95a431e10f4f00f27
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/15/2018
---
# <a name="tutorial-get-started-with-signalr-for-aspnet-core"></a>Didacticiel : Prise en main SignalR pour ASP.NET Core

Par [Rachel Appel](https://twitter.com/rachelappel)

Ce didacticiel explique les principes fondamentaux de la création d’une application en temps réel à l’aide de SignalR pour ASP.NET Core.

   ![Solution](get-started-signalr-core/_static/signalr-get-started-finished.png)

[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/get-started-signalr-core/sample/) ([procédure de téléchargement](xref:tutorials/index#how-to-download-a-sample))

Ce didacticiel présente les tâches de développement SignalR suivantes :

> [!div class="checklist"]
> * Créer une application de web ASP.NET Core.
> * Créer un concentrateur SignalR pour transmettre le contenu aux clients.
> * Utilisez la bibliothèque SignalR JavaScript pour envoyer des messages et d’afficher les mises à jour à partir du concentrateur.

## <a name="prerequisites"></a>Prérequis

Installer les logiciels suivants :

* [Afficher un aperçu de .NET core 2.1.0 1 SDK](https://www.microsoft.com/net/download/dotnet-core/sdk-2.1.300-preview1) ou version ultérieure
* [Visual Studio 2017](https://www.visualstudio.com/downloads/) 15,6 ou version ultérieure avec la charge de travail de développement ASP.NET et web
* [npm](https://www.npmjs.com/get-npm)

## <a name="create-an-aspnet-core-project-that-hosts-signalr-client-and-server"></a>Créez un projet ASP.NET Core qui héberge le serveur et client SignalR

1. Utilisez le **fichier** > **nouveau projet** menu option et choisissez **Application ASP.NET Core Web**. Attribuez un nom au projet `SignalRChat`.

  ![Boîte de dialogue Nouveau projet dans Visual Studio](get-started-signalr-core/_static/signalr-new-project-dialog.png)

2. Sélectionnez **Web Application** pour créer un projet à l’aide des Pages Razor. Puis sélectionnez **Ok**. Assurez-vous que **ASP.NET Core 2.1** est sélectionné dans le sélecteur de framework, si SignalR s’exécute sur des versions antérieures de .NET.

  ![Boîte de dialogue Nouveau projet dans Visual Studio](get-started-signalr-core/_static/signalr-new-project-choose-type.png)

  Les bibliothèques qui hébergent le code de SignalR côté serveur sont inclus dans le modèle de projet. Installer le JavaScript côté client séparément avec [npm](https://www.npmjs.com/).

  ```console
   npm install @aspnet/signalr
  ```

3. Copie le *signalr.js* de *node_modules\\ @aspnet\signalr\dist\browser*  à la *wwwroot\lib* dossier dans votre projet.

## <a name="create-the-signalr-hub"></a>Création du Hub SignalR

Un concentrateur est une classe qui sert d’un pipeline de haut niveau qui permet au client et le serveur appeler des méthodes sur eux.

1. Ajoutez une classe au projet en choisissant **fichier** > **nouveau** > **fichier** et en sélectionnant **classe Visual c#**. 

1. Hériter de `Microsoft.AspNetCore.SignalR.Hub`. La `Hub` classe contient des propriétés et des événements pour la gestion des connexions et des groupes, ainsi que des données d’envoi et de réception.

1. Créer le `Send` méthode qui envoie un message à tous les clients de conversation connecté. Notez qu’il renvoie un `Task`, car SignalR est asynchrone. Code asynchrone évolue mieux.

  [!code-csharp[Startup](get-started-signalr-core/sample/Hubs/ChatHub.cs?range=7-14)]

## <a name="configure-the-project-to-use-signalr"></a>Configurer le projet pour utiliser SignalR

Le serveur de SignalR doit être configuré de sorte qu’il sache pour transmettre les demandes à SignalR.

1. Pour configurer un projet SignalR, modifiez le `ConfigureServices` (méthode) de l’application `Startup` classe en insérant un appel à `services.AddSignalR`.

  `services.AddSignalR` Ajoute SignalR dans le cadre de la [intergiciel (middleware) ASP.NET Core](xref:fundamentals/middleware/index) pipeline.

1. Configurer des itinéraires à vos concentrateurs à l’aide de `UseSignalR`.

  [!code-csharp[Startup](get-started-signalr-core/sample/Startup.cs?highlight=22,40-43)]

## <a name="create-the-signalr-client-code"></a>Créer le code de client SignalR

1. Remplacez le contenu de *Pages\Index.cshtml* avec le code suivant :

  [!code-cshtml[Index](get-started-signalr-core/sample/Pages/Index.cshtml)]

  Le code HTML précédent affiche le nom et les champs de message et un bouton d’envoi. Notez les références de script en bas : une référence à SignalR et *chat.js*.

1. Ajouter un fichier JavaScript à la *wwwroot\js* dossier nommé *chat.js* et ajoutez-y le code suivant :

  [!code-javascript[Index](get-started-signalr-core/sample/wwwroot/js/chat.js)]

## <a name="run-the-app"></a>Exécuter l'application

1. Sélectionnez **déboguer** > **démarrer sans débogage** pour lancer un navigateur et de charger le site Web localement. Copiez l’URL de la barre d’adresses.

1. Ouvrez une autre instance du navigateur (n’importe quel navigateur) et collez l’URL dans la barre d’adresses.

1. Choisissez un navigateur, entrez un nom et un message, puis cliquez sur le **envoyer** bouton. Le nom et le message sont affichés sur les deux pages instantanément.

  ![Solution](get-started-signalr-core/_static/signalr-get-started-finished.png)
