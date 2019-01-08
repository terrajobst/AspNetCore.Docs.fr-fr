---
uid: signalr/overview/getting-started/tutorial-getting-started-with-signalr
title: 'Tutoriel : Conversation en temps réel avec SignalR 2 | Microsoft Docs'
author: pfletcher
description: Ce didacticiel montre comment utiliser SignalR pour créer une application de conversation en temps réel. Vous ajoutez SignalR à une application de web ASP.NET vide.
ms.author: riande
ms.date: 01/02/2019
ms.assetid: a8b3b778-f009-4369-85c7-e90f9878d8b4
msc.legacyurl: /signalr/overview/getting-started/tutorial-getting-started-with-signalr
msc.type: authoredcontent
ms.topic: tutorial
ms.openlocfilehash: aa015abc47bb2450e04e167c0404aaa1d119ba2c
ms.sourcegitcommit: 97d7a00bd39c83a8f6bccb9daa44130a509f75ce
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/08/2019
ms.locfileid: "54098622"
---
# <a name="tutorial-real-time-chat-with-signalr-2"></a>Tutoriel : Conversation en temps réel avec SignalR 2

Ce didacticiel vous montre comment utiliser SignalR pour créer une application de conversation en temps réel. Vous ajoutez des SignalR à une application de web ASP.NET vide et que vous créez une page HTML pour envoyer et afficher des messages.

Dans ce didacticiel, vous :

> [!div class="checklist"]
> * Configurer le projet
> * Exécuter l’exemple
> * Examinez le code

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

## <a name="prerequisites"></a>Prérequis

* [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/) avec la **ASP.NET et développement web** charge de travail.

## <a name="set-up-the-project"></a>Configurer le projet

Cette section montre comment utiliser Visual Studio 2017 et SignalR 2 pour créer une application web ASP.NET vide, ajouter SignalR et créer l’application de conversation.

1. Dans Visual Studio, créez une Application Web ASP.NET.

    ![Créer le web](tutorial-getting-started-with-signalr/_static/image2.png)

1. Dans le **nouveau projet ASP.NET - SignalRChat** fenêtre, laissez le champ **vide** sélectionné et sélectionnez **OK**.

1. Dans **l’Explorateur de solutions**, cliquez sur le projet et sélectionnez **ajouter** > **un nouvel élément**.

1. Dans **ajouter un nouvel élément - SignalRChat**, sélectionnez **installé** > **Visual C#**   >  **Web**  >  **SignalR** , puis sélectionnez **classe de concentrateur SignalR (v2)**.

1. Nommez la classe *ChatHub* et ajoutez-le au projet.

    Cette étape crée la *ChatHub.cs* fichier de classe et ajoute un ensemble de fichiers de script et les références d’assembly qui prennent en charge SignalR au projet.

1. Remplacez le code dans le nouveau *ChatHub.cs* fichier de classe avec ce code :

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample1.cs)]

1. Dans **l’Explorateur de solutions**, cliquez sur le projet et sélectionnez **ajouter** > **un nouvel élément**.

1. Dans **ajouter un nouvel élément - SignalRChat** sélectionnez **installé** > **Visual C#**   >  **Web** , puis Sélectionnez **classe de démarrage OWIN**.

1. Nommez la classe *démarrage* et ajoutez-le au projet.

1. Dans **l’Explorateur de solutions**, cliquez sur le projet et sélectionnez **ajouter** > **HTML Page**.

1. Nommez la nouvelle page *index* et sélectionnez **OK**.

1. Dans **l’Explorateur de solutions**, avec le bouton droit de la page HTML que vous avez créé et sélectionnez **définir comme Page de démarrage**.

1. Remplacez le code par défaut dans la page HTML avec ce code :

    [!code-html[Main](tutorial-getting-started-with-signalr/samples/sample3.html)]

1. Dans **l’Explorateur de solutions**, développez **Scripts**.

    Bibliothèques de scripts pour jQuery et SignalR sont visibles dans le projet.

    > [!IMPORTANT]
    > Le Gestionnaire de package peut avoir installé une version ultérieure des scripts SignalR.

1. Vérifier que les références de script dans le bloc de code correspondent aux versions des fichiers de script dans le projet.

    Références de script à partir du bloc de code d’origine :
    ```html
    <!--Script references. -->
    <!--Reference the jQuery library. -->
    <script src="Scripts/jquery-3.1.1.min.js" ></script>
    <!--Reference the SignalR library. -->
    <script src="Scripts/jquery.signalR-2.2.1.min.js"></script>
    ```

1. Si elles ne correspondent pas, mettre à jour le *.html* fichier.

1. Dans la barre de menus, sélectionnez **fichier** > **Enregistrer tout**.

## <a name="run-the-sample"></a>Exécuter l’exemple

1. Dans la barre d’outils, activez **le débogage de Script** , puis sélectionnez le bouton lecture pour exécuter l’exemple en mode débogage.

    ![Entrer un nom d'utilisateur](tutorial-getting-started-with-signalr/_static/image3.png)

1. Lorsque le navigateur s’ouvre, entrez un nom pour votre identité de conversation.

1. Copiez l’URL à partir du navigateur, ouvrez les deux autres navigateurs et collez l’URL dans la barre d’adresse.

1. Dans chaque navigateur, entrez un nom unique.

1. Maintenant, ajoutez un commentaire, puis sélectionnez **envoyer**. Répétez que dans les autres navigateurs. Les commentaires s’affichent en temps réel.

    > [!NOTE]
    > Cette application de conversation simple ne conserve pas le contexte de la discussion sur le serveur. Le hub diffuse des commentaires à tous les utilisateurs actuels. Les utilisateurs qui accèdent à la conversation ultérieurement verrez messages ajoutés à partir du moment qu'où ils joignent.

    Voyez comment l’application de conversation s’exécute dans trois différents navigateurs. Lorsque Tom, Anand et Susan envoient des messages, tous les navigateurs mettre à jour en temps réel :

    ![Tous les trois navigateurs affichent le même historique de conversation](tutorial-getting-started-with-signalr/_static/image4.png)

1. Dans **l’Explorateur de solutions**, inspecter la **Documents de Script** nœud pour l’application en cours d’exécution. Il existe un fichier de script nommé *hubs* qui génère de la bibliothèque SignalR lors de l’exécution. Ce fichier gère la communication entre le script de jQuery et le code côté serveur.

    ![script de hubs généré automatiquement dans le nœud Documents de Script](tutorial-getting-started-with-signalr/_static/image5.png)

## <a name="examine-the-code"></a>Examinez le Code

L’application SignalRChat montre deux tâches de développement SignalR base. Il vous montre comment créer un hub. Le serveur utilise ce hub en tant que l’objet principal de coordination. Le hub utilise la bibliothèque jQuery de SignalR pour envoyer et recevoir des messages.

### <a name="signalr-hubs-in-the-chathubcs"></a>Concentrateurs SignalR dans les ChatHub.cs

Dans l’exemple de code ci-dessus, le `ChatHub` classe dérive de la `Microsoft.AspNet.SignalR.Hub` classe. Dérivation à partir de la `Hub` classe est un moyen utile pour créer une application de SignalR. Vous pouvez créer des méthodes publiques sur votre classe de concentrateur et ensuite utiliser ces méthodes en les appelant à partir de scripts dans une page web.

Dans le code de la conversation, les clients appellent le `ChatHub.Send` méthode pour envoyer un nouveau message. Le concentrateur envoie ensuite le message à tous les clients en appelant `Clients.All.broadcastMessage`.

Le `Send` méthode illustre plusieurs concepts de hub :

* Déclarer des méthodes publiques sur un concentrateur afin que les clients peuvent appeler les.

* Utilisez le `Microsoft.AspNet.SignalR.Hub.Clients` propriété dynamique à communiquer avec tous les clients connectés à ce concentrateur.

* Appeler une fonction sur le client (comme le `broadcastMessage` (fonction)) pour mettre à jour des clients.

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample4.cs)]

### <a name="signalr-and-jquery-in-the-indexhtml"></a>SignalR et jQuery dans le fichier index.html

Le *index.html* page dans l’exemple de code montre comment utiliser la bibliothèque jQuery de SignalR pour communiquer avec un concentrateur SignalR. Le code effectue de nombreuses tâches importantes. Elle déclare un proxy vers le hub de référence, déclare une fonction que le serveur peut appeler pour transmettre le contenu aux clients, et il démarre une connexion pour envoyer des messages au concentrateur.

[!code-javascript[Main](tutorial-getting-started-with-signalr/samples/sample5.js)]

> [!NOTE]
> Dans JavaScript la référence à la classe server et ses membres doit être une casse mixte. Les références d’exemple de code la C# *ChatHub* classe dans JavaScript en tant que `chatHub`.

Dans ce bloc de code, vous créez une fonction de rappel dans le script.

[!code-html[Main](tutorial-getting-started-with-signalr/samples/sample6.html)]

La classe de concentrateur sur le serveur appelle cette fonction pour envoyer des mises à jour de contenu à chaque client. Les deux lignes que HTML à encoder le contenu avant de les afficher sont facultatifs et afficher un bon moyen d’empêcher l’injection de script.

Ce code ouvre une connexion avec le hub.

[!code-javascript[Main](tutorial-getting-started-with-signalr/samples/sample7.js)]

> [!NOTE]
> Cette approche garantit que le code établit une connexion avant que le Gestionnaire d’événements s’exécute.

Le code démarre la connexion et passe une fonction pour gérer l’événement de clic sur le **envoyer** bouton dans la page HTML.

## <a name="additional-resources"></a>Ressources supplémentaires

Pour en savoir plus sur SignalR, consultez les ressources suivantes :

* [Projet de SignalR](http://signalr.net)

* [SignalR Github et exemples](https://github.com/SignalR/SignalR)

* [Wiki de SignalR](https://github.com/SignalR/SignalR/wiki)

## <a name="next-steps"></a>Étapes suivantes

Dans ce didacticiel vous :

> [!div class="checklist"]
> * Configurer le projet
> * Exécuter l’exemple
> * Examiner le code

Passez à l’article suivant pour apprendre à utiliser SignalR et MVC 5.
> [!div class="nextstepaction"]
> [SignalR 2 et MVC 5](tutorial-getting-started-with-signalr-and-mvc.md)