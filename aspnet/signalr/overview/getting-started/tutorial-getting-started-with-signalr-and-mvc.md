---
uid: signalr/overview/getting-started/tutorial-getting-started-with-signalr-and-mvc
title: 'Tutoriel : Conversation en temps réel avec SignalR 2 et MVC 5. | Microsoft Docs'
author: pfletcher
description: Ce didacticiel montre comment utiliser ASP.NET SignalR 2 pour créer une application de conversation en temps réel. Vous ajoutez SignalR à une application MVC 5.
ms.author: riande
ms.date: 01/02/2019
ms.assetid: 80bfe5fb-bdfc-41fe-ac43-2132e5d69fac
msc.legacyurl: /signalr/overview/getting-started/tutorial-getting-started-with-signalr-and-mvc
msc.type: authoredcontent
ms.topic: tutorial
ms.openlocfilehash: eb4b7e1403f4070d65702b756bf98c5294c7fb17
ms.sourcegitcommit: 97d7a00bd39c83a8f6bccb9daa44130a509f75ce
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/08/2019
ms.locfileid: "54098602"
---
# <a name="tutorial-real-time-chat-with-signalr-2-and-mvc-5"></a>Tutoriel : Conversation en temps réel avec SignalR 2 et MVC 5

Ce didacticiel montre comment utiliser ASP.NET SignalR 2 pour créer une application de conversation en temps réel. Vous ajoutez des SignalR à une application MVC 5 et que vous créez une vue de conversation pour envoyer et afficher les messages.

Dans ce didacticiel, vous :

> [!div class="checklist"]
> * Configurer le projet
> * Exécuter l’exemple
> * Examinez le code

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

## <a name="prerequisites"></a>Prérequis

* [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/) avec la **ASP.NET et développement web** charge de travail.

## <a name="set-up-the-project"></a>Configurer le projet

Cette section montre comment utiliser Visual Studio 2017 et SignalR 2 pour créer une application ASP.NET MVC 5 vide, ajoutez la bibliothèque SignalR et créer l’application de conversation.

1. Dans Visual Studio, créez une application c# ASP.NET qui cible .NET Framework 4.5, nommez-le SignalRChat et cliquez sur OK.

    ![Créer le web](tutorial-getting-started-with-signalr-and-mvc/_static/image1.png)

1. Dans **nouvelle Application Web ASP.NET - SignalRMvcChat**, sélectionnez **MVC** , puis sélectionnez **modifier l’authentification**.

1. Dans **modifier l’authentification**, sélectionnez **aucune authentification** et cliquez sur **OK**.

    ![Sélectionnez Aucune authentification](tutorial-getting-started-with-signalr-and-mvc/_static/image2.png)

1. Dans **nouvelle Application Web ASP.NET - SignalRMvcChat**, sélectionnez **OK**.

1. Dans **l’Explorateur de solutions**, cliquez sur le projet et sélectionnez **ajouter** > **un nouvel élément**.

1. Dans **ajouter un nouvel élément - SignalRChat**, sélectionnez **installé** > **Visual C#**   >  **Web**  >  **SignalR** , puis sélectionnez **classe de concentrateur SignalR (v2)**.

1. Nommez la classe *ChatHub* et ajoutez-le au projet.

    Cette étape crée la *ChatHub.cs* fichier de classe et ajoute un ensemble de fichiers de script et les références d’assembly qui prennent en charge SignalR au projet.

1. Remplacez le code dans le nouveau *ChatHub.cs* fichier de classe avec ce code :

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample1.cs)]

1. Dans **l’Explorateur de solutions**, cliquez sur le projet et sélectionnez **ajouter** > **classe**.

1. Nommez la nouvelle classe *démarrage* et ajoutez-le au projet.

1. Remplacez le code dans le *Startup.cs* fichier de classe avec ce code :

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample2.cs)]

1. Dans **l’Explorateur de solutions**, sélectionnez **contrôleurs** > **HomeController.cs**.

1. Ajoutez la méthode suivante à la *HomeController.cs*.

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample3.cs)]

    Cette méthode retourne le **Chat** vue que vous créez dans une étape ultérieure.

1. Dans **l’Explorateur de solutions**, avec le bouton droit **vues** > **accueil**, puis sélectionnez **ajouter**  >    **Vue**.

1. Dans **ajouter une vue**, nommez la nouvelle vue **Chat** et sélectionnez **ajouter**.

1. Remplacez le contenu de **Chat.cshtml** avec ce code :

    [!code-cshtml[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample4.cshtml)]

1. Dans **l’Explorateur de solutions**, développez **Scripts**.

    Bibliothèques de scripts pour jQuery et SignalR sont visibles dans le projet.

    > [!IMPORTANT]
    > Le Gestionnaire de package peut avoir installé une version ultérieure des scripts SignalR.

1. Vérifier que les références de script dans le bloc de code correspondent aux versions des fichiers de script dans le projet.

    Références de script à partir du bloc de code d’origine :

    ```cshtml
    <!--Script references. -->
    <!--The jQuery library is required and is referenced by default in _Layout.cshtml. -->
    <!--Reference the SignalR library. -->
    <script src="~/Scripts/jquery.signalR-2.1.0.min.js"></script>
    ```

1. Si elles ne correspondent pas, mettre à jour le *.cshtml* fichier.

1. Dans la barre de menus, sélectionnez **fichier** > **Enregistrer tout**.

## <a name="run-the-sample"></a>Exécuter l’exemple

1. Dans la barre d’outils, activez **le débogage de Script** , puis sélectionnez le bouton lecture pour exécuter l’exemple en mode débogage.

    ![Entrer un nom d'utilisateur](tutorial-getting-started-with-signalr-and-mvc/_static/image3.png)

1. Lorsque le navigateur s’ouvre, entrez un nom pour votre identité de conversation.

1. Copiez l’URL à partir du navigateur, ouvrez les deux autres navigateurs et collez l’URL dans la barre d’adresse.

1. Dans chaque navigateur, entrez un nom unique.

1. Maintenant, ajoutez un commentaire, puis sélectionnez **envoyer**. Répétez que dans les autres navigateurs. Les commentaires s’affichent en temps réel.

    > [!NOTE]
    > Cette application de conversation simple ne conserve pas le contexte de la discussion sur le serveur. Le hub diffuse des commentaires à tous les utilisateurs actuels. Les utilisateurs qui accèdent à la conversation ultérieurement verrez messages ajoutés à partir du moment qu'où ils joignent.

    Voyez comment l’application de conversation s’exécute dans trois différents navigateurs. Lorsque Tom, Anand et Susan envoient des messages, tous les navigateurs mettre à jour en temps réel :

    ![Tous les trois navigateurs affichent le même historique de conversation](tutorial-getting-started-with-signalr-and-mvc/_static/image4.png)

1. Dans **l’Explorateur de solutions**, inspecter la **Documents de Script** nœud pour l’application en cours d’exécution. Il existe un fichier de script nommé *hubs* qui génère de la bibliothèque SignalR lors de l’exécution. Ce fichier gère la communication entre le script de jQuery et le code côté serveur.

    ![script de hubs généré automatiquement dans le nœud Documents de Script](tutorial-getting-started-with-signalr-and-mvc/_static/image5.png)

## <a name="examine-the-code"></a>Examinez le Code

L’application de conversation SignalR montre deux tâches de développement SignalR base. Il vous montre comment créer un hub. Le serveur utilise ce hub en tant que l’objet principal de coordination. Le hub utilise la bibliothèque jQuery de SignalR pour envoyer et recevoir des messages.

### <a name="signalr-hubs-in-the-chathubcs"></a>Concentrateurs SignalR dans les ChatHub.cs

Dans l’exemple de code, le `ChatHub` classe dérive de la `Microsoft.AspNet.SignalR.Hub` classe. Dérivation à partir de la `Hub` classe est un moyen utile pour créer une application de SignalR. Vous pouvez créer des méthodes publiques sur votre classe de concentrateur et ensuite accéder à ces méthodes en les appelant à partir de scripts dans une page web.

Dans le code de la conversation, les clients appellent le `ChatHub.Send` méthode pour envoyer un nouveau message. Le concentrateur à son tour envoie le message à tous les clients en appelant `Clients.All.addNewMessageToPage`.

Le `Send` méthode illustre plusieurs concepts de hub :

* Déclarer des méthodes publiques sur un concentrateur afin que les clients peuvent appeler les.

* Utilisez le `Microsoft.AspNet.SignalR.Hub.Clients` propriété dynamique à communiquer avec tous les clients connectés à ce concentrateur.

* Appeler une fonction sur le client (comme le `addNewMessageToPage` (fonction)) pour mettre à jour des clients.

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample5.cs)]

### <a name="signalr-and-jquery-chatcshtml"></a>SignalR et jQuery Chat.cshtml

Le *Chat.cshtml* afficher le fichier dans l’exemple de code montre comment utiliser la bibliothèque jQuery de SignalR pour communiquer avec un concentrateur SignalR.  Le code effectue de nombreuses tâches importantes. Il crée une référence au proxy généré automatiquement pour le hub, déclare une fonction que le serveur peut appeler pour envoyer le contenu aux clients, et il démarre une connexion pour envoyer des messages au concentrateur.

[!code-javascript[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample6.js)]

> [!NOTE]
> Dans JavaScript, la référence à la classe de serveur et de ses membres est dans une casse mixte. Les références d’exemple de code la C# `ChatHub` classe dans JavaScript en tant que `chatHub`.

Dans ce bloc de code, vous créez une fonction de rappel dans le script.

[!code-html[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample7.html)]

La classe de concentrateur sur le serveur appelle cette fonction pour envoyer des mises à jour de contenu à chaque client. L’appel facultatif à la `htmlEncode` affiche la fonction moyen HTML encode le contenu du message avant de les afficher dans la page. C’est un moyen pour empêcher l’injection de script.

Ce code ouvre une connexion avec le hub.

[!code-javascript[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample8.js)]

> [!NOTE]
> Cette approche garantit que vous établissez une connexion avant que le Gestionnaire d’événements s’exécute.

Le code démarre la connexion et passe une fonction pour gérer l’événement de clic sur le **envoyer** bouton dans la page de la conversation.

## <a name="additional-resources"></a>Ressources supplémentaires

Pour en savoir plus sur SignalR, consultez les ressources suivantes :

* [Projet de SignalR](http://signalr.net)

* [SignalR GitHub et exemples](https://github.com/SignalR/SignalR)

* [Wiki de SignalR](https://github.com/SignalR/SignalR/wiki)

## <a name="next-steps"></a>Étapes suivantes

Dans ce didacticiel, vous :

> [!div class="checklist"]
> * Configurer le projet
> * Exécuter l’exemple
> * Examiner le code

Passez à l’article suivant pour apprendre à créer une application web qui utilise ASP.NET SignalR 2 pour fournir des fonctionnalités de messagerie à fréquence élevée.
> [!div class="nextstepaction"]
> [Application Web avec la messagerie à fréquence élevée](tutorial-high-frequency-realtime-with-signalr.md)