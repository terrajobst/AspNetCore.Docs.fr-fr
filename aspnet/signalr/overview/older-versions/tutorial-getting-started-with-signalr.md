---
uid: signalr/overview/older-versions/tutorial-getting-started-with-signalr
title: 'Didacticiel : Bien démarrer avec SignalR 1.x | Microsoft Docs'
author: pfletcher
description: Utiliser ASP.NET SignalR pour créer une application de conversation en temps réel dans une page HTML.
ms.author: aspnetcontent
ms.date: 02/18/2013
ms.assetid: fdc3599a-5217-44c1-951f-0eec9812dce7
msc.legacyurl: /signalr/overview/older-versions/tutorial-getting-started-with-signalr
msc.type: authoredcontent
ms.openlocfilehash: 13d33ff7e3cfff996a9849cfccfcc43754c8234e
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37838625"
---
<a name="tutorial-getting-started-with-signalr-1x"></a>Didacticiel : Bien démarrer avec SignalR 1.x
====================
par [Patrick Fletcher](https://github.com/pfletcher), [Tim Teebken](https://github.com/timlt)

> Ce didacticiel montre comment utiliser SignalR pour créer une application de conversation en temps réel. Vous ajouter SignalR à une application de web ASP.NET vide et créer une page HTML pour envoyer et afficher des messages.


## <a name="overview"></a>Vue d'ensemble

Ce didacticiel présente le développement de SignalR en montrant comment créer une application de service de conversation simple basée sur navigateur. Vous ajoute la bibliothèque SignalR à une application de web ASP.NET vide, créez une classe de hub pour envoyer des messages aux clients et créer une page HTML qui permet aux utilisateurs d’envoyer et recevoir des messages de conversation. Pour obtenir un didacticiel similaire qui montre comment créer une application de conversation dans MVC 4 à l’aide d’une vue MVC, consultez [bien démarrer avec SignalR et MVC 4](index.md).

> [!NOTE]
> Ce didacticiel utilise la version finale (1.x) de SignalR. Pour plus d’informations sur les modifications entre SignalR 1.x et 2.0, consultez [SignalR la mise à niveau les projets 1.x](../releases/upgrading-signalr-1x-projects-to-20.md).

SignalR est une bibliothèque de .NET open source pour la création d’applications web qui requièrent l’intervention de l’utilisateur en direct ou de mises à jour des données en temps réel. Exemples : applications des réseaux sociaux, jeux multi-utilisateur, météo de collaboration et de news, entreprise ou les applications financières de mise à jour. Il s’agit souvent d’applications en temps réel.

SignalR simplifie le processus de génération d’applications en temps réel. Il inclut une bibliothèque de serveur ASP.NET et une bibliothèque de client JavaScript pour le rendre plus facile à gérer les connexions client-serveur et d’envoyer des mises à jour de contenu aux clients. Vous pouvez ajouter la bibliothèque SignalR à une application ASP.NET existante pour obtenir des fonctionnalités en temps réel.

Le didacticiel présente les tâches de développement SignalR suivantes :

- Ajout de la bibliothèque de SignalR pour une application web ASP.NET.
- Création d’une classe de hub pour envoyer le contenu vers les clients.
- À l’aide de la bibliothèque jQuery SignalR dans une page web pour envoyer des messages et d’afficher les mises à jour à partir du concentrateur.

La capture d’écran suivante montre l’application de conversation en cours d’exécution dans un navigateur. Chaque nouvel utilisateur peut publier des commentaires et voir les commentaires ajoutés après que l’utilisateur rejoint la conversation.

![Instances de conversation](tutorial-getting-started-with-signalr/_static/image1.png)

Sections :

- [Configurer le projet](#setup)
- [Exécuter l’exemple](#run)
- [Examinez le Code](#code)
- [Étapes suivantes](#next)

<a id="setup"></a>

## <a name="set-up-the-project"></a>Configurer le projet

Cette section montre comment créer une application web ASP.NET vide, ajouter SignalR et créer l’application de conversation.

Conditions préalables :

- Visual Studio 2010 SP1 ou 2012. Si vous n’avez pas Visual Studio, consultez [téléchargements ASP.NET](https://www.asp.net/downloads) pour obtenir le Visual Studio 2012 Express outil de développement gratuit.
- [Microsoft ASP.NET et Web Tools 2012.2](https://go.microsoft.com/fwlink/?LinkId=279941). Pour Visual Studio 2012, ce programme d’installation ajoute les nouvelles fonctionnalités ASP.NET, y compris les modèles de SignalR pour Visual Studio. Pour Visual Studio 2010 SP1, un programme d’installation n’est pas disponible, mais vous pouvez suivre le didacticiel en installant le package NuGet de SignalR comme décrit dans les étapes d’installation.

Les étapes suivantes utilisent Visual Studio 2012 pour créer une Application Web ASP.NET vide et ajouter la bibliothèque SignalR :

1. Dans Visual Studio, créez une Application Web ASP.NET vide.

    ![Créer le site web vide](tutorial-getting-started-with-signalr/_static/image2.png)
2. Ouvrez le **Console du Gestionnaire de Package** en sélectionnant **outils | Gestionnaire de Package de bibliothèque | Console du Gestionnaire de package**. Entrez la commande suivante dans la fenêtre de console :

    `Install-Package Microsoft.AspNet.SignalR -Version 1.1.3`

    Cette commande installe la dernière version de SignalR 1.x.
3. Dans **l’Explorateur de solutions**, cliquez sur le projet, sélectionnez **ajouter | Classe**. Nommez la nouvelle classe **ChatHub**.
4. Dans **l’Explorateur de solutions** développez le nœud de Scripts. Bibliothèques de scripts pour jQuery et SignalR sont visibles dans le projet.

    ![Références de bibliothèque](tutorial-getting-started-with-signalr/_static/image3.png)
5. Remplacez le code dans le **ChatHub** classe par le code suivant.

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample1.cs)]
6. Dans **l’Explorateur de solutions**, cliquez sur le projet, puis cliquez sur **ajouter | Un nouvel élément**. Dans le **ajouter un nouvel élément** boîte de dialogue, sélectionnez **classe d’Application globale** et cliquez sur **ajouter**.

    ![Ajouter global](tutorial-getting-started-with-signalr/_static/image4.png)
7. Ajoutez le code suivant `using` instructions après avoir fourni `using` instructions dans la classe Global.asax.cs.

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample2.cs)]
8. Ajoutez la ligne suivante de code dans le `Application_Start` méthode de la classe Global pour inscrire l’itinéraire par défaut pour les concentrateurs SignalR.

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample3.cs)]
9. Dans **l’Explorateur de solutions**, cliquez sur le projet, puis cliquez sur **ajouter | Un nouvel élément**. Dans le **ajouter un nouvel élément** boîte de dialogue, sélectionnez Html Page et cliquez sur **ajouter**.
10. Dans **l’Explorateur de solutions**, avec le bouton droit de la page HTML que vous venez de créer, puis cliquez sur **définir comme Page de démarrage**.
11. Remplacez le code par défaut dans la page HTML par le code suivant.

    [!code-html[Main](tutorial-getting-started-with-signalr/samples/sample4.html)]
12. **Enregistrer tous les** pour le projet.

<a id="run"></a>

## <a name="run-the-sample"></a>Exécuter l’exemple

1. Appuyez sur F5 pour exécuter le projet en mode débogage. La page HTML se charge dans une instance du navigateur et des invites pour un nom d’utilisateur.

    ![Entrer un nom d'utilisateur](tutorial-getting-started-with-signalr/_static/image5.png)
2. Entrez un nom d’utilisateur.
3. Copiez l’URL de la ligne d’adresse du navigateur et l’utiliser pour ouvrir les deux autres instances de navigateur. Dans chaque instance du navigateur, entrez un nom d’utilisateur unique.
4. Dans chaque instance du navigateur, ajoutez un commentaire et cliquez sur **envoyer**. Les commentaires doivent s’afficher dans toutes les instances de navigateur.

    > [!NOTE]
    > Cette application de conversation simple ne conserve pas le contexte de la discussion sur le serveur. Le hub diffuse des commentaires à tous les utilisateurs actuels. Les utilisateurs qui accèdent à la conversation ultérieurement verrez messages ajoutés à partir du moment qu'où ils joignent.

    La capture d’écran suivante montre l’application de conversation en cours d’exécution dans les trois instances de navigateur, qui sont mis à jour lorsqu’une instance envoie un message :

    ![Navigateurs de conversation](tutorial-getting-started-with-signalr/_static/image6.png)
5. Dans **l’Explorateur de solutions**, inspecter la **Documents de Script** nœud pour l’application en cours d’exécution. Il existe un fichier de script nommé **hubs** que la bibliothèque SignalR génère dynamiquement lors de l’exécution. Ce fichier gère la communication entre le script de jQuery et le code côté serveur.

    ![Script de hub généré](tutorial-getting-started-with-signalr/_static/image7.png)

<a id="code"></a>

## <a name="examine-the-code"></a>Examinez le Code

L’application de conversation SignalR montre deux tâches de développement SignalR base : création d’un hub en tant que l’objet principal de coordination sur le serveur et à l’aide de la bibliothèque jQuery de SignalR pour envoyer et recevoir des messages.

### <a name="signalr-hubs"></a>Concentrateurs SignalR

Dans l’exemple de code la **ChatHub** classe dérive de la **Microsoft.AspNet.SignalR.Hub** classe. Dérivation à partir de la **Hub** classe est un moyen utile pour créer une application de SignalR. Vous pouvez créer des méthodes publiques sur votre classe de concentrateur et ensuite accéder à ces méthodes en les appelant à partir de scripts jQuery dans une page web.

Dans le code de la conversation, les clients appellent le **ChatHub.Send** méthode pour envoyer un nouveau message. Le concentrateur à son tour envoie le message à tous les clients en appelant **Clients.All.broadcastMessage**.

Le **envoyer** méthode illustre plusieurs concepts de hub :

- Déclarer des méthodes publiques sur un concentrateur afin que les clients peuvent appeler les.
- Utilisez le **Microsoft.AspNet.SignalR.Hub.Clients** propriété dynamique à accéder à tous les clients connectés à ce concentrateur.
- Appeler une fonction de jQuery sur le client (tel que le `broadcastMessage` (fonction)) pour mettre à jour des clients.

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample5.cs)]

### <a name="signalr-and-jquery"></a>SignalR et jQuery

La page HTML dans l’exemple de code montre comment utiliser la bibliothèque jQuery de SignalR pour communiquer avec un concentrateur SignalR. Les tâches essentielles dans le code déclarez un proxy pour référencer le hub, la déclaration d’une fonction que le serveur peut appeler pour transmettre le contenu aux clients et le démarrage d’une connexion pour envoyer des messages au concentrateur.

Le code suivant déclare un proxy pour un concentrateur.

[!code-javascript[Main](tutorial-getting-started-with-signalr/samples/sample6.js)]

> [!NOTE]
> Dans jQuery, la référence à la classe de serveur et de ses membres est en casse mixte. L’exemple de code fait référence à celle de C# **ChatHub** classe dans jQuery comme **chatHub**.


Le code suivant est la façon dont vous créez une fonction de rappel dans le script. La classe de concentrateur sur le serveur appelle cette fonction pour envoyer des mises à jour de contenu à chaque client. Les deux lignes qu’encoder en HTML le contenu avant de les afficher sont facultatifs et affichent un moyen simple d’empêcher l’injection de script.

[!code-html[Main](tutorial-getting-started-with-signalr/samples/sample7.html)]

Le code suivant montre comment ouvrir une connexion avec le hub. Le code démarre la connexion et passe une fonction pour gérer l’événement de clic sur le **envoyer** bouton dans la page HTML.

> [!NOTE]
> Cette approche garantit que la connexion est établie avant que le Gestionnaire d’événements s’exécute.


[!code-javascript[Main](tutorial-getting-started-with-signalr/samples/sample8.js)]

<a id="next"></a>

## <a name="next-steps"></a>Étapes suivantes

Vous avez appris que SignalR est une infrastructure pour générer des applications web en temps réel. Vous avez également appris à plusieurs tâches de développement de SignalR : comment ajouter SignalR à une application ASP.NET, comment créer une classe de concentrateur et comment envoyer et recevoir des messages à partir du concentrateur.

Vous pouvez proposer l’exemple d’application dans ce didacticiel ou d’autres applications de SignalR via Internet en les déployant sur un fournisseur d’hébergement. Microsoft propose d’hébergement web gratuit pour jusqu'à 10 sites web dans un gratuit [compte d’évaluation de Windows Azure](https://www.windowsazure.com/pricing/free-trial/?WT.mc_id=A443DD604). Pour une procédure pas à pas sur la façon de déployer l’exemple d’application SignalR, consultez [publier le SignalR Getting Started, exemple comme un Site Web Windows Azure](https://blogs.msdn.com/b/timlee/archive/2013/02/27/deploy-the-signalr-getting-started-sample-as-a-windows-azure-web-site.aspx). Pour plus d’informations sur la façon de déployer un projet web Visual Studio sur un Site Web de Windows Azure, consultez [déploiement d’une Application ASP.NET sur un Site Web de Windows Azure](https://docs.microsoft.com/azure/app-service-web/app-service-web-get-started-dotnet). (Remarque : le transport WebSocket est actuellement pas en charge pour les Sites Web Windows Azure. Le transport WebSocket lorsque n’est pas disponible, SignalR utilise les autres transports disponibles comme décrit dans la section de Transports de la [Introduction à SignalR rubrique](index.md).)

Pour en savoir plus les concepts de développements SignalR plus avancés, consultez les sites suivants pour le code source de SignalR et de ressources :

- [Projet de SignalR](http://signalr.net)
- [SignalR Github et exemples](https://github.com/SignalR/SignalR)
- [Wiki de SignalR](https://github.com/SignalR/SignalR/wiki)
