---
uid: signalr/overview/older-versions/tutorial-getting-started-with-signalr-and-mvc-4
title: 'Didacticiel : Bien démarrer avec SignalR 1.x et MVC 4 | Microsoft Docs'
author: pfletcher
description: Utiliser ASP.NET SignalR et ASP.NET MVC 4 pour créer une application de conversation en temps réel.
ms.author: riande
ms.date: 03/29/2013
ms.assetid: eeef9f73-6de3-49f9-b50b-9af22108f2ce
msc.legacyurl: /signalr/overview/older-versions/tutorial-getting-started-with-signalr-and-mvc-4
msc.type: authoredcontent
ms.openlocfilehash: 95fc3315149e07dbdb0505a2b5ab197bfedba097
ms.sourcegitcommit: a4dcca4f1cb81227c5ed3c92dc0e28be6e99447b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/10/2018
ms.locfileid: "48910874"
---
<a name="tutorial-getting-started-with-signalr-1x-and-mvc-4"></a>Didacticiel : Bien démarrer avec SignalR 1.x et MVC 4
====================
par [Patrick Fletcher](https://github.com/pfletcher), [Tim Teebken](https://github.com/timlt)

> Ce didacticiel montre comment utiliser ASP.NET SignalR pour créer une application de conversation en temps réel. Vous ajouter SignalR à une application MVC 4 et créer une vue de conversation pour envoyer et afficher des messages.


## <a name="overview"></a>Vue d'ensemble

Ce didacticiel vous présente au développement d’applications web en temps réel avec SignalR ASP.NET et ASP.NET MVC 4. Ce didacticiel utilise le même code d’application de conversation que la [didacticiel SignalR mise en route](tutorial-getting-started-with-signalr.md), mais montre comment ajouter à une application MVC 4 basée sur le modèle d’Internet.

Dans cette rubrique, vous allez apprendre les tâches de développement SignalR suivantes :

- Ajout de la bibliothèque de SignalR à une application MVC 4.
- Création d’une classe de hub pour envoyer le contenu vers les clients.
- À l’aide de la bibliothèque jQuery SignalR dans une page web pour envoyer des messages et d’afficher les mises à jour à partir du concentrateur.

La capture d’écran suivante montre l’application de conversation terminée en cours d’exécution dans un navigateur.

![Instances de conversation](tutorial-getting-started-with-signalr-and-mvc-4/_static/image2.png)

Sections :

- [Configurer le projet](#setup)
- [Exécuter l’exemple](#run)
- [Examinez le Code](#code)
- [Étapes suivantes](#next)

<a id="setup"></a>

## <a name="set-up-the-project"></a>Configurer le projet

Conditions préalables :

- Visual Studio 2010 SP1, Visual Studio 2012 ou Visual Studio 2012 Express. Si vous n’avez pas Visual Studio, consultez [téléchargements ASP.NET](https://www.asp.net/downloads) pour obtenir le Visual Studio 2012 Express outil de développement gratuit.
- Pour Visual Studio 2010, installez [ASP.NET MVC 4](https://www.microsoft.com/download/details.aspx?id=30683).

Cette section montre comment créer une application ASP.NET MVC 4, ajoutez la bibliothèque SignalR et créer l’application de conversation.

1. 1. Dans Visual Studio créer une application ASP.NET MVC 4, nommez-le SignalRChat et cliquez sur OK.

        > [!NOTE]
        > Dans Visual Studio 2010, sélectionnez **.NET Framework 4** dans le contrôle de liste déroulante de version de Framework. Code de SignalR s’exécute sur les versions du .NET Framework 4 et 4.5.

        ![Créer web mvc](tutorial-getting-started-with-signalr-and-mvc-4/_static/image3.png)
      2. Sélectionnez le modèle d’Application Internet, désactivez l’option **créer un projet de test unitaire**, puis cliquez sur OK.

         ![Créer un site internet de mvc](tutorial-getting-started-with-signalr-and-mvc-4/_static/image4.png)
      3. Ouvrez le **Outils > Gestionnaire de Package NuGet > Console du Gestionnaire de Package** et exécutez la commande suivante. Cette étape ajoute au projet un ensemble de fichiers de script et les références d’assembly qui activent les fonctionnalités de SignalR.

         `install-package Microsoft.AspNet.SignalR -Version 1.1.3`
      4. Dans **l’Explorateur de solutions** développez le dossier Scripts. Notez que les bibliothèques de scripts pour SignalR ont été ajoutés au projet.

         ![Références de bibliothèque](tutorial-getting-started-with-signalr-and-mvc-4/_static/image6.png)
      5. Dans **l’Explorateur de solutions**, cliquez sur le projet, sélectionnez **ajouter | Nouveau dossier**, et ajoutez un nouveau dossier nommé **Hubs**.
      6. Cliquez sur le **Hubs** dossier, cliquez sur **ajouter | Classe**et créez une nouvelle classe c# nommée **ChatHub.cs**. Vous utiliserez cette classe comme un concentrateur de serveur SignalR qui envoie des messages à tous les clients.

> [!NOTE]
> Si vous utilisez Visual Studio 2012 et que vous avez installé le [mise à jour ASP.NET et Web Tools 2012.2](../../../visual-studio/overview/2012/aspnet-and-web-tools-20122-release-notes-rtw.md#_Installation), vous pouvez utiliser le nouveau modèle d’élément de SignalR pour créer la classe de concentrateur. Pour ce faire, cliquez sur le **Hubs** dossier, cliquez sur **ajouter | Un nouvel élément**, sélectionnez **classe de concentrateur SignalR (v1)** et nommez la classe **ChatHub.cs**.


1. Remplacez le code dans le **ChatHub** classe par le code suivant.

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample1.cs)]
2. Ouvrez le **Global.asax** fichier pour le projet, puis ajoutez un appel à la méthode `RouteTable.Routes.MapHubs();` en tant que la première ligne de code dans le `Application_Start` (méthode). Ce code enregistre l’itinéraire par défaut pour les concentrateurs SignalR et doit être appelé avant d’inscrire tous les autres itinéraires. Fin du `Application_Start` méthode ressemble à l’exemple suivant.

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample2.cs)]
3. Modifier le `HomeController` classe trouvée dans **Controllers/HomeController.cs** et ajoutez la méthode suivante à la classe. Cette méthode retourne le **Chat** vue que vous allez créer dans une étape ultérieure.

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample3.cs)]
4. Avec le bouton droit dans le `Chat` méthode que vous venez de créer et cliquez sur **ajouter une vue** pour créer un nouveau fichier d’affichage.
5. Dans le **ajouter une vue** boîte de dialogue, vérifiez que la case à cocher est sélectionnée pour **utiliser une disposition ou la page maître** (désactivez les autres cases à cocher), puis cliquez sur **ajouter**.

    ![Ajouter une vue](tutorial-getting-started-with-signalr-and-mvc-4/_static/image8.png)
6. Modifiez le nouveau fichier d’affichage nommé **Chat.cshtml**. Après le &lt;h2&gt; balise, collez le code suivant &lt;div&gt; section et `@section scripts` bloc de code dans la page. Ce script permet à la page envoyer des messages de conversation et afficher des messages à partir du serveur. Le code complet pour la vue de conversation s’affiche dans le bloc de code suivant.

    > [!IMPORTANT]
    > Lorsque vous ajoutez SignalR et autres bibliothèques de scripts à votre projet Visual Studio, le Gestionnaire de Package peut installer des versions des scripts qui sont plus récents que les versions indiquées dans cette rubrique. Assurez-vous que les références de script dans votre code correspondent aux versions des bibliothèques de script installés dans votre projet.

    [!code-cshtml[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample4.cshtml)]
7. **Enregistrer tous les** pour le projet.

<a id="run"></a>

## <a name="run-the-sample"></a>Exécuter l’exemple

1. Appuyez sur F5 pour exécuter le projet en mode débogage.
2. Dans la ligne d’adresse de navigateur, ajoutez **chat/home/** à l’URL de la page par défaut pour le projet. La page de la conversation se charge dans une instance du navigateur et des invites pour un nom d’utilisateur.

    ![Entrer un nom d'utilisateur](tutorial-getting-started-with-signalr-and-mvc-4/_static/image9.png)
3. Entrez un nom d’utilisateur.
4. Copiez l’URL de la ligne d’adresse du navigateur et l’utiliser pour ouvrir les deux autres instances de navigateur. Dans chaque instance du navigateur, entrez un nom d’utilisateur unique.
5. Dans chaque instance du navigateur, ajoutez un commentaire et cliquez sur **envoyer**. Les commentaires doivent s’afficher dans toutes les instances de navigateur.

    > [!NOTE]
    > Cette application de conversation simple ne conserve pas le contexte de la discussion sur le serveur. Le hub diffuse des commentaires à tous les utilisateurs actuels. Les utilisateurs qui accèdent à la conversation ultérieurement verrez messages ajoutés à partir du moment qu'où ils joignent.
6. La capture d’écran suivante montre l’application de conversation en cours d’exécution dans un navigateur.

    ![Navigateurs de conversation](tutorial-getting-started-with-signalr-and-mvc-4/_static/image11.png)
7. Dans **l’Explorateur de solutions**, inspecter la **Documents de Script** nœud pour l’application en cours d’exécution. Ce nœud est visible en mode débogage si vous utilisez Internet Explorer comme navigateur. Il existe un fichier de script nommé **hubs** que la bibliothèque SignalR génère dynamiquement lors de l’exécution. Ce fichier gère la communication entre le script de jQuery et le code côté serveur. Si vous utilisez un navigateur autre que Internet Explorer, vous pouvez également accéder à la dynamique **hubs** fichier en y accédant directement, par exemple http://mywebsite/signalr/hubs.

    ![Script de hub généré](tutorial-getting-started-with-signalr-and-mvc-4/_static/image13.png)

<a id="code"></a>

## <a name="examine-the-code"></a>Examinez le Code

L’application de conversation SignalR montre deux tâches de développement SignalR base : création d’un hub en tant que l’objet principal de coordination sur le serveur et à l’aide de la bibliothèque jQuery de SignalR pour envoyer et recevoir des messages.

### <a name="signalr-hubs"></a>Concentrateurs SignalR

Dans l’exemple de code la **ChatHub** classe dérive de la **Microsoft.AspNet.SignalR.Hub** classe. Dérivation à partir de la **Hub** classe est un moyen utile pour créer une application de SignalR. Vous pouvez créer des méthodes publiques sur votre classe de concentrateur et ensuite accéder à ces méthodes en les appelant à partir de scripts jQuery dans une page web.

Dans le code de la conversation, les clients appellent le **ChatHub.Send** méthode pour envoyer un nouveau message. Le concentrateur à son tour envoie le message à tous les clients en appelant **Clients.All.addNewMessageToPage**.

Le **envoyer** méthode illustre plusieurs concepts de hub :

- Déclarer des méthodes publiques sur un concentrateur afin que les clients peuvent appeler les.
- Utilisez le **Microsoft.AspNet.SignalR.Hub.Clients** propriété pour accéder à tous les clients connectés à ce concentrateur.
- Appeler une fonction de jQuery sur le client (tel que le `addNewMessageToPage` (fonction)) pour mettre à jour des clients.

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample5.cs)]

### <a name="signalr-and-jquery"></a>SignalR et jQuery

Le **Chat.cshtml** afficher le fichier dans l’exemple de code montre comment utiliser la bibliothèque jQuery de SignalR pour communiquer avec un concentrateur SignalR. Les tâches essentielles dans le code sont création d’une référence pour le proxy généré automatiquement pour le hub, déclarant une fonction que le serveur peut appeler pour transmettre le contenu aux clients et le démarrage d’une connexion pour envoyer des messages au concentrateur.

Le code suivant déclare un proxy pour un concentrateur.

[!code-javascript[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample6.js)]

> [!NOTE]
> Dans jQuery, la référence à la classe de serveur et de ses membres est en casse mixte. L’exemple de code fait référence à celle de C# **ChatHub** classe dans jQuery comme **chatHub**. Si vous souhaitez faire référence à la `ChatHub` classe dans jQuery avec Pascal conventionnel mise en majuscules comme vous le feriez en c#, modifiez le fichier de classe ChatHub.cs. Ajouter un `using` instruction pour référencer le `Microsoft.AspNet.SignalR.Hubs` espace de noms. Ajoutez ensuite le `HubName` attribut le `ChatHub` de classe, par exemple `[HubName("ChatHub")]`. Enfin, mettez à jour de votre référence de jQuery à la `ChatHub` classe.


Le code suivant montre comment créer une fonction de rappel dans le script. La classe de concentrateur sur le serveur appelle cette fonction pour envoyer des mises à jour de contenu à chaque client. L’appel facultatif à la `htmlEncode` affiche la fonction moyen HTML encode le contenu du message avant de les afficher dans la page, comme un moyen d’empêcher l’injection de script.

[!code-html[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample7.html)]

Le code suivant montre comment ouvrir une connexion avec le hub. Le code démarre la connexion et passe une fonction pour gérer l’événement de clic sur le **envoyer** bouton dans la page de la conversation.

> [!NOTE]
> Cette approche garantit que la connexion est établie avant que le Gestionnaire d’événements s’exécute.


[!code-javascript[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample8.js)]

<a id="next"></a>

## <a name="next-steps"></a>Étapes suivantes

Vous avez appris que SignalR est une infrastructure pour générer des applications web en temps réel. Vous avez également appris à plusieurs tâches de développement de SignalR : comment ajouter SignalR à une application ASP.NET, comment créer une classe de concentrateur et comment envoyer et recevoir des messages à partir du concentrateur.

Pour en savoir plus les concepts de développements SignalR plus avancés, consultez les sites suivants pour le code source de SignalR et de ressources :

- [Projet de SignalR](http://signalr.net)
- [SignalR Github et exemples](https://github.com/SignalR/SignalR)
- [Wiki de SignalR](https://github.com/SignalR/SignalR/wiki)
