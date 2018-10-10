---
uid: signalr/overview/getting-started/tutorial-getting-started-with-signalr-and-mvc
title: 'Didacticiel : Bien démarrer avec SignalR 2 et MVC 5 | Microsoft Docs'
author: pfletcher
description: Ce didacticiel montre comment utiliser ASP.NET SignalR 2 pour créer une application de conversation en temps réel. Vous ajouter SignalR à une application MVC 5 et créer une vue de la conversation...
ms.author: riande
ms.date: 06/10/2014
ms.assetid: 80bfe5fb-bdfc-41fe-ac43-2132e5d69fac
msc.legacyurl: /signalr/overview/getting-started/tutorial-getting-started-with-signalr-and-mvc
msc.type: authoredcontent
ms.openlocfilehash: a58b95adfb5d0165887b95abd3247d3a829aa882
ms.sourcegitcommit: a4dcca4f1cb81227c5ed3c92dc0e28be6e99447b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/10/2018
ms.locfileid: "48912226"
---
<a name="tutorial-getting-started-with-signalr-2-and-mvc-5"></a>Didacticiel : Bien démarrer avec SignalR 2 et MVC 5
====================
par [Patrick Fletcher](https://github.com/pfletcher), [Tim Teebken](https://github.com/timlt)

[Télécharger le projet terminé](http://code.msdn.microsoft.com/Getting-Started-with-c366b2f3)

> Ce didacticiel montre comment utiliser ASP.NET SignalR 2 pour créer une application de conversation en temps réel. Vous ajouter SignalR à une application MVC 5 et créer une vue de conversation pour envoyer et afficher des messages.
>
> ## <a name="software-versions-used-in-the-tutorial"></a>Versions des logiciels utilisées dans le didacticiel
>
>
> - [Visual Studio 2013](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)
> - .NET 4.5
> - MVC 5
> - SignalR version 2
>
>
>
> ## <a name="using-visual-studio-2012-with-this-tutorial"></a>À l’aide de Visual Studio 2012 avec ce didacticiel
>
>
> Pour utiliser Visual Studio 2012 avec ce didacticiel, procédez comme suit :
>
> - Mise à jour votre [Gestionnaire de Package](http://docs.nuget.org/docs/start-here/installing-nuget) vers la dernière version.
> - Installer le [Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx).
> - Dans le programme Web Platform Installer, recherchez et installez **ASP.NET et Web Tools 2013.1 pour Visual Studio 2012**. Cela installera les modèles Visual Studio pour les classes de SignalR comme **Hub**.
> - Certains modèles (tels que **classe de démarrage OWIN**) ne sera pas disponible ; dans ce cas, utilisez un fichier de classe à la place.
>
>
> ## <a name="tutorial-versions"></a>Versions de didacticiels
>
> Pour plus d’informations sur les versions antérieures de SignalR, consultez [les Versions antérieures de SignalR](../older-versions/index.md).
>
> ## <a name="questions-and-comments"></a>Questions et commentaires
>
> Veuillez laisser des commentaires sur la façon dont vous avez apprécié ce didacticiel et ce que nous pouvions améliorer dans les commentaires en bas de la page. Si vous avez des questions qui ne sont pas directement liées à ce didacticiel, vous pouvez les publier à le [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) ou [StackOverflow.com](http://stackoverflow.com/).


## <a name="overview"></a>Vue d'ensemble

Ce didacticiel vous présente au développement d’applications web en temps réel avec ASP.NET SignalR 2 et ASP.NET MVC 5. Ce didacticiel utilise le même code d’application de conversation que la [didacticiel SignalR mise en route](tutorial-getting-started-with-signalr.md), mais montre comment ajouter à une application MVC 5.

Dans cette rubrique, vous allez apprendre les tâches de développement SignalR suivantes :

- Ajout de la bibliothèque de SignalR à une application MVC 5.
- Création de hub et les classes de démarrage OWIN pour envoyer le contenu aux clients.
- À l’aide de la bibliothèque jQuery SignalR dans une page web pour envoyer des messages et d’afficher les mises à jour à partir du concentrateur.

La capture d’écran suivante montre l’application de conversation terminée en cours d’exécution dans un navigateur.

![Instances de conversation](tutorial-getting-started-with-signalr-and-mvc/_static/image1.png)

Sections :

- [Configurer le projet](#setup)
- [Exécuter l’exemple](#run)
- [Examinez le Code](#code)
- [Étapes suivantes](#next)

<a id="setup"></a>

## <a name="set-up-the-project"></a>Configurer le projet

Conditions préalables :

- Visual Studio 2013. Si vous n’avez pas Visual Studio, consultez [téléchargements ASP.NET](https://www.asp.net/downloads) pour obtenir le Visual Studio 2013 Express outil de développement gratuit.

Cette section montre comment créer une application ASP.NET MVC 5, ajoutez la bibliothèque SignalR et créer l’application de conversation.

1. Dans Visual Studio, créez une application c# ASP.NET qui cible .NET Framework 4.5, nommez-le SignalRChat et cliquez sur OK.

    ![Créer le web](tutorial-getting-started-with-signalr-and-mvc/_static/image2.png)
2. Dans le `New ASP.NET Project` boîte de dialogue, puis sélectionnez **MVC**, puis cliquez sur **modifier l’authentification**.

    ![Créer le web](tutorial-getting-started-with-signalr-and-mvc/_static/image3.png)
3. Sélectionnez **aucune authentification** dans le **modifier l’authentification** boîte de dialogue, puis cliquez sur **OK**.

    ![Sélectionnez Aucune authentification](tutorial-getting-started-with-signalr-and-mvc/_static/image4.png)

    > [!NOTE]
    > Si vous sélectionnez un fournisseur d’authentification pour votre application, un `Startup.cs` classe sera créée pour vous, vous ne devez pas créer votre propre `Startup.cs` classe à l’étape 10 ci-dessous.
4. Cliquez sur **OK** dans le **nouveau projet ASP.NET** boîte de dialogue.
5. Ouvrez le **Outils > Gestionnaire de Package NuGet > Console du Gestionnaire de Package** et exécutez la commande suivante. Cette étape ajoute au projet un ensemble de fichiers de script et les références d’assembly qui activent les fonctionnalités de SignalR.

    `install-package Microsoft.AspNet.SignalR`
6. Dans **l’Explorateur de solutions**, développez le dossier Scripts. Notez que les bibliothèques de scripts pour SignalR ont été ajoutés au projet.

    ![Dossier de scripts](tutorial-getting-started-with-signalr-and-mvc/_static/image5.png)
7. Dans **l’Explorateur de solutions**, cliquez sur le projet, sélectionnez **ajouter | Nouveau dossier**, et ajoutez un nouveau dossier nommé **Hubs**.
8. Cliquez sur le **Hubs** dossier, cliquez sur **ajouter | Un nouvel élément**, sélectionnez le **Visual C# | Web | SignalR** nœud dans le **installé** volet, sélectionnez **classe de concentrateur SignalR (v2)** dans le volet central et créez un nouveau hub nommé **ChatHub.cs**. Vous utiliserez cette classe comme un concentrateur de serveur SignalR qui envoie des messages à tous les clients.

    ![Créer nouveau concentrateur](tutorial-getting-started-with-signalr-and-mvc/_static/image6.png)
9. Remplacez le code dans le **ChatHub** classe par le code suivant.

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample1.cs)]
10. Créer une nouvelle classe appelée Startup.cs. Modifier le contenu du fichier à ce qui suit.

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample2.cs)]
11. Modifier le `HomeController` classe trouvée dans **Controllers/HomeController.cs** et ajoutez la méthode suivante à la classe. Cette méthode retourne le **Chat** vue que vous allez créer dans une étape ultérieure.

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample3.cs)]
12. Cliquez sur le **vues/accueil** dossier, puis sélectionnez **ajouter... | Vue**.
13. Dans le **ajouter une vue** boîte de dialogue, nom de la nouvelle vue **Chat**.

    ![Ajouter une vue](tutorial-getting-started-with-signalr-and-mvc/_static/image7.png)
14. Remplacez le contenu de **Chat.cshtml** avec le code suivant.

    > [!IMPORTANT]
    > Lorsque vous ajoutez SignalR et autres bibliothèques de scripts à votre projet Visual Studio, le Gestionnaire de Package peut installer une version du fichier de script de SignalR est plus récente que la version indiquée dans cette rubrique. Assurez-vous que la référence de script dans votre code correspond à la version de la bibliothèque de scripts installée dans votre projet.

    [!code-cshtml[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample4.cshtml)]
15. **Enregistrer tous les** pour le projet.

<a id="run"></a>

## <a name="run-the-sample"></a>Exécuter l’exemple

1. Appuyez sur F5 pour exécuter le projet en mode débogage.
2. Dans la ligne d’adresse de navigateur, ajoutez **chat/home/** à l’URL de la page par défaut pour le projet. La page de la conversation se charge dans une instance du navigateur et des invites pour un nom d’utilisateur.

    ![Entrer un nom d'utilisateur](tutorial-getting-started-with-signalr-and-mvc/_static/image8.png)
3. Entrez un nom d’utilisateur.
4. Copiez l’URL de la ligne d’adresse du navigateur et l’utiliser pour ouvrir les deux autres instances de navigateur. Dans chaque instance du navigateur, entrez un nom d’utilisateur unique.
5. Dans chaque instance du navigateur, ajoutez un commentaire et cliquez sur **envoyer**. Les commentaires doivent s’afficher dans toutes les instances de navigateur.

    > [!NOTE]
    > Cette application de conversation simple ne conserve pas le contexte de la discussion sur le serveur. Le hub diffuse des commentaires à tous les utilisateurs actuels. Les utilisateurs qui accèdent à la conversation ultérieurement verrez messages ajoutés à partir du moment qu'où ils joignent.
6. La capture d’écran suivante montre l’application de conversation en cours d’exécution dans un navigateur.

    ![Navigateurs de conversation](tutorial-getting-started-with-signalr-and-mvc/_static/image9.png)
7. Dans **l’Explorateur de solutions**, inspecter la **Documents de Script** nœud pour l’application en cours d’exécution. Ce nœud est visible en mode débogage si vous utilisez Internet Explorer comme navigateur. Il existe un fichier de script nommé **hubs** que la bibliothèque SignalR génère dynamiquement lors de l’exécution. Ce fichier gère la communication entre le script de jQuery et le code côté serveur. Si vous utilisez un navigateur autre que Internet Explorer, vous pouvez également accéder à la dynamique **hubs** fichier en y accédant directement, par exemple http://mywebsite/signalr/hubs.

<a id="code"></a>

## <a name="examine-the-code"></a>Examinez le Code

L’application de conversation SignalR montre deux tâches de développement SignalR base : création d’un hub en tant que l’objet principal de coordination sur le serveur et à l’aide de la bibliothèque jQuery de SignalR pour envoyer et recevoir des messages.

### <a name="signalr-hubs"></a>Concentrateurs SignalR

Dans l’exemple de code la **ChatHub** classe dérive de la **Microsoft.AspNet.SignalR.Hub** classe. Dérivation à partir de la **Hub** classe est un moyen utile pour créer une application de SignalR. Vous pouvez créer des méthodes publiques sur votre classe de concentrateur et ensuite accéder à ces méthodes en les appelant à partir de scripts dans une page web.

Dans le code de la conversation, les clients appellent le **ChatHub.Send** méthode pour envoyer un nouveau message. Le concentrateur à son tour envoie le message à tous les clients en appelant **Clients.All.addNewMessageToPage**.

Le **envoyer** méthode illustre plusieurs concepts de hub :

- Déclarer des méthodes publiques sur un concentrateur afin que les clients peuvent appeler les.
- Utilisez le **Microsoft.AspNet.SignalR.Hub.Clients** propriété pour accéder à tous les clients connectés à ce concentrateur.
- Appeler une fonction sur le client (tel que le `addNewMessageToPage` (fonction)) pour mettre à jour des clients.

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample5.cs)]

### <a name="signalr-and-jquery"></a>SignalR et jQuery

Le **Chat.cshtml** afficher le fichier dans l’exemple de code montre comment utiliser la bibliothèque jQuery de SignalR pour communiquer avec un concentrateur SignalR. Les tâches essentielles dans le code sont création d’une référence pour le proxy généré automatiquement pour le hub, déclarant une fonction que le serveur peut appeler pour transmettre le contenu aux clients et le démarrage d’une connexion pour envoyer des messages au concentrateur.

Le code suivant déclare une référence à un concentrateur proxy.

[!code-javascript[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample6.js)]

> [!NOTE]
> Dans JavaScript, la référence à la classe de serveur et de ses membres est en casse mixte. L’exemple de code fait référence à celle de C# **ChatHub** classe dans JavaScript en tant que **chatHub**. Si vous souhaitez faire référence à la `ChatHub` classe dans jQuery avec Pascal conventionnel mise en majuscules comme vous le feriez en c#, modifiez le fichier de classe ChatHub.cs. Ajouter un `using` instruction pour référencer le `Microsoft.AspNet.SignalR.Hubs` espace de noms. Ajoutez ensuite le `HubName` attribut le `ChatHub` de classe, par exemple `[HubName("ChatHub")]`. Enfin, mettez à jour de votre référence de jQuery à la `ChatHub` classe.


Le code suivant montre comment créer une fonction de rappel dans le script. La classe de concentrateur sur le serveur appelle cette fonction pour envoyer des mises à jour de contenu à chaque client. L’appel facultatif à la `htmlEncode` affiche la fonction moyen HTML encode le contenu du message avant de les afficher dans la page, comme un moyen d’empêcher l’injection de script.

[!code-html[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample7.html)]

Le code suivant montre comment ouvrir une connexion avec le hub. Le code démarre la connexion et passe une fonction pour gérer l’événement de clic sur le **envoyer** bouton dans la page de la conversation.

> [!NOTE]
> Cette approche garantit que la connexion est établie avant que le Gestionnaire d’événements s’exécute.


[!code-javascript[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample8.js)]

<a id="next"></a>

## <a name="next-steps"></a>Étapes suivantes

Vous avez appris que SignalR est une infrastructure pour générer des applications web en temps réel. Vous avez également appris à plusieurs tâches de développement de SignalR : comment ajouter SignalR à une application ASP.NET, comment créer une classe de concentrateur et comment envoyer et recevoir des messages à partir du concentrateur.

Pour une procédure pas à pas sur la façon de déployer l’exemple d’application SignalR dans Azure, consultez [à l’aide de SignalR avec Web Apps dans Azure App Service](../deployment/using-signalr-with-azure-web-sites.md). Pour plus d’informations sur la façon de déployer un projet web Visual Studio sur un Site Web de Windows Azure, consultez [créer une application web ASP.NET dans Azure App Service](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-get-started/).

Pour en savoir plus les concepts de développements SignalR plus avancés, consultez les sites suivants pour le code source de SignalR et de ressources :

- [Projet de SignalR](http://signalr.net)
- [SignalR Github et exemples](https://github.com/SignalR/SignalR)
- [Wiki de SignalR](https://github.com/SignalR/SignalR/wiki)
