---
uid: signalr/overview/getting-started/tutorial-high-frequency-realtime-with-signalr
title: 'Tutoriel : Créer des applications en temps réel haute fréquence avec SignalR 2 | Microsoft Docs'
author: bradygaster
description: Ce didacticiel montre comment créer une application web qui utilise ASP.NET SignalR pour fournir des fonctionnalités de messagerie à fréquence élevée.
ms.author: bradyg
ms.date: 01/22/2019
ms.assetid: 9f969dda-78ea-4329-b1e3-e51c02210a2b
msc.legacyurl: /signalr/overview/getting-started/tutorial-high-frequency-realtime-with-signalr
msc.type: authoredcontent
ms.topic: tutorial
ms.openlocfilehash: 44aaa2b0c059de310e963f642fa56c2f00a7e443
ms.sourcegitcommit: ebf4e5a7ca301af8494edf64f85d4a8deb61d641
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/24/2019
ms.locfileid: "54836725"
---
# <a name="tutorial-create-high-frequency-real-time-app-with-signalr-2"></a>Tutoriel : Créer des applications en temps réel haute fréquence avec SignalR 2

Ce didacticiel montre comment créer une application web qui utilise ASP.NET SignalR 2 pour fournir des fonctionnalités de messagerie à fréquence élevée. Dans ce cas, « échanges à fréquence élevée » signifie que le serveur envoie des mises à jour à un taux fixe. Vous envoyez 10 messages par seconde.

L’application que vous créez affiche une forme que les utilisateurs peuvent faire glisser. Le serveur met à jour la position de la forme dans tous les navigateurs connectés pour correspondre à la position de la forme déplacée à l’aide de mises à jour a expiré.

Concepts présentés dans ce didacticiel ont des applications dans des jeux en temps réel et autres applications de simulation.

Dans ce didacticiel, vous avez effectué les actions suivantes :

> [!div class="checklist"]
> * Configurer le projet
> * Créer l’application de base
> * Mapper vers le hub au démarrage de l’application
> * Ajouter le client
> * Exécuter l'application
> * Ajouter la boucle de client
> * Ajouter la boucle de serveur
> * Ajouter des animations fluides

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

## <a name="prerequisites"></a>Prérequis

* [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/) avec la **ASP.NET et développement web** charge de travail.

## <a name="set-up-the-project"></a>Configurer le projet

Dans cette section, vous créez le projet dans Visual Studio 2017.

Cette section montre comment utiliser Visual Studio 2017 pour créer une Application de Web ASP.NET vide et ajouter les bibliothèques de SignalR et jQuery.UI.

1. Dans Visual Studio, créez une Application Web ASP.NET.

    ![Créer le web](tutorial-high-frequency-realtime-with-signalr/_static/image1.png)

1. Dans le **nouvelle Application Web ASP.NET - MoveShapeDemo** fenêtre, laissez le champ **vide** sélectionné et sélectionnez **OK**.

1. Dans **l’Explorateur de solutions**, cliquez sur le projet et sélectionnez **ajouter** > **un nouvel élément**.

1. Dans **ajouter un nouvel élément - MoveShapeDemo**, sélectionnez **installé** > **Visual C#**   >  **Web**  >  **SignalR** , puis sélectionnez **classe de concentrateur SignalR (v2)**.

1. Nommez la classe *MoveShapeHub* et ajoutez-le au projet.

    Cette étape crée la *MoveShapeHub.cs* fichier de classe. Simultanément, il ajoute un ensemble de fichiers de script et les références d’assembly qui prennent en charge SignalR au projet.

1. Sélectionnez **outils** > **Gestionnaire de Package NuGet** > **Console du Gestionnaire de Package**.

1. Dans **Console du Gestionnaire de Package**, exécutez la commande suivante :

    ```console
    Install-Package jQuery.UI.Combined
    ```

    La commande installe la bibliothèque jQuery UI. Vous l’utilisez pour animer la forme.

1. Dans **l’Explorateur de solutions**, développez le nœud de Scripts.

    ![Références de bibliothèque de script](tutorial-high-frequency-realtime-with-signalr/_static/image2.png)

    Bibliothèques de scripts pour jQuery, jQueryUI et SignalR sont visibles dans le projet.

## <a name="create-the-base-application"></a>Créer l’application de base

Dans cette section, vous créez une application de navigateur. L’application envoie l’emplacement de la forme sur le serveur lors de chaque événement mouse move. Le serveur diffuse ces informations pour tous les autres clients connectés en temps réel. Vous en savoir plus sur cette application dans les sections suivantes.

1. Ouvrez le *MoveShapeHub.cs* fichier.

1. Remplacez le code dans le *MoveShapeHub.cs* fichier avec ce code :

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample1.cs)]

1. Enregistrez le fichier.

Le `MoveShapeHub` classe est une implémentation d’un concentrateur SignalR. Comme dans le [bien démarrer avec SignalR](tutorial-getting-started-with-signalr.md) didacticiel, le concentrateur a une méthode d’appeler directement les clients. Dans ce cas, le client envoie un objet avec la nouvelle X et Y des coordonnées de la forme sur le serveur. Ces coordonnées obtient diffusées à tous les autres clients connectés. SignalR sérialise automatiquement cet objet à l’aide de JSON.

L’application envoie la `ShapeModel` objet au client. Il possède des membres pour stocker la position de la forme. La version de l’objet sur le serveur a également un membre pour suivre les données de client sont stockées. Cet objet empêche le serveur d’envoyer des données d’un client vers lui-même. Ce membre utilise le `JsonIgnore` attribut pour empêcher l’application de sérialiser les données et de l’envoyer au client.

## <a name="map-to-the-hub-when-app-starts"></a>Mapper vers le hub au démarrage de l’application

Ensuite, vous configurez le mappage au hub lorsque l’application démarre. SignalR 2, l’ajout d’une classe de démarrage OWIN crée le mappage.

1. Dans **l’Explorateur de solutions**, cliquez sur le projet et sélectionnez **ajouter** > **un nouvel élément**.

1. Dans **ajouter un nouvel élément - MoveShapeDemo** sélectionnez **installé** > **Visual C#**   >  **Web** , puis Sélectionnez **classe de démarrage OWIN**.

1. Nommez la classe *démarrage* et sélectionnez **OK**.

1. Remplacez le code par défaut dans le *Startup.cs* fichier avec ce code :

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample2.cs)]

La classe de démarrage OWIN appelle `MapSignalR` lorsque l’application s’exécute le `Configuration` (méthode). L’application ajoute la classe de démarrage de OWIN traiter à l’aide de la `OwinStartup` attribut d’assembly.

## <a name="add-the-client"></a>Ajouter le client

Ajouter la page HTML pour le client.

1. Dans **l’Explorateur de solutions**, cliquez sur le projet et sélectionnez **ajouter** > **HTML Page**.

1. Nommez la page **par défaut** et sélectionnez **OK**.

1. Dans **l’Explorateur de solutions**, avec le bouton droit *Default.html* et sélectionnez **définir comme Page de démarrage**.

1. Remplacez le code par défaut dans le *Default.html* fichier avec ce code :

    [!code-html[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample3.html?highlight=14-16)]

1. Dans **l’Explorateur de solutions**, développez **Scripts**.

    Bibliothèques de scripts pour jQuery et SignalR sont visibles dans le projet.

    > [!IMPORTANT]
    > Le Gestionnaire de package installe une version ultérieure des scripts SignalR.

1. Mettre à jour les références de script dans le bloc de code pour qu’elles correspondent aux versions des fichiers de script dans le projet.

Ce code HTML et JavaScript crée une croix rouge `div` appelée `shape`. Il active le comportement de glissement de la forme à l’aide de la bibliothèque jQuery et utilise le `drag` événement à la position de la forme d’envoi au serveur.

## <a name="run-the-app"></a>Exécuter l'application

Vous pouvez exécuter l’application à se'e il fonctionne. Lorsque vous faites glisser la forme autour d’une fenêtre de navigateur, la forme déplace trop dans les autres navigateurs.

1. Dans la barre d’outils, activez **le débogage de Script** , puis sélectionnez le bouton lecture pour exécuter l’application en mode débogage.

    ![Capture d’écran de l’utilisateur sous tension en sélectionnant play et de débogage.](tutorial-high-frequency-realtime-with-signalr/_static/image3.png)

    Une fenêtre de navigateur s’ouvre avec la forme rouge dans le coin supérieur droit.

1. Copiez l’URL de la page.

1. Ouvrir un autre navigateur et collez l’URL dans la barre d’adresses.

1. Faites glisser la forme de l’une des fenêtres de navigateur. La forme dans l’autre fenêtre de navigateur suit.

Alors que l’application les fonctions à l’aide de cette méthode, il n’est pas un modèle de programmation recommandé. Il n’existe aucune limite supérieure au nombre de messages envoyés de l’obtention. Par conséquent, les clients et le serveur obtient submergés par les messages et les performances se dégradent. En outre, l’application affiche une animation disjoint sur le client. Cette animation saccadée se produit, car la forme déplace instantanément à chaque méthode. Il est préférable si la forme se déplace correctement vers chaque nouvel emplacement. Ensuite, vous allez apprendre à résoudre ces problèmes.

## <a name="add-the-client-loop"></a>Ajouter la boucle de client

Envoi de l’emplacement de la forme sur chaque événement mouse move crée une quantité inutile de trafic réseau. L’application doit limiter les messages à partir du client.

Utiliser le code javascript `setInterval` fonction pour configurer une boucle qui envoie des informations sur la nouvelle position sur le serveur à un taux fixe. Cette boucle est une représentation de base d’une « boucle de jeu ». C’est une fonction appelée à plusieurs reprises qui gère toutes les fonctionnalités d’un jeu.

1. Remplacez le code client dans le *Default.html* fichier avec ce code :

    [!code-html[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample4.html?highlight=14-16)]

    > [!IMPORTANT]
    > Vous devez remplacer les références de script à nouveau. Ils doivent correspondre les versions des scripts dans le projet.

    Ce nouveau code ajoute la `updateServerModel` (fonction). Elle est appelée sur une fréquence fixe. La fonction envoie les données de la position sur le serveur chaque fois que le `moved` indicateur indique qu’il existe de nouvelles données de position à envoyer.

1. Sélectionnez le bouton lecture pour démarrer l’application

1. Copiez l’URL de la page.

1. Ouvrir un autre navigateur et collez l’URL dans la barre d’adresses.

1. Faites glisser la forme de l’une des fenêtres de navigateur. La forme dans l’autre fenêtre de navigateur suit.

Dans la mesure où l’application limite le nombre de messages qui sont envoyés au serveur, l’animation ne s’affiche pas aussi fluide a fait dans un premier temps.

## <a name="add-the-server-loop"></a>Ajouter la boucle de serveur

Dans l’application actuelle, les messages envoyés à partir du serveur au client accède souvent lors de leur réception. Ce trafic réseau pose un problème similaire comme nous voir sur le client.

L’application peut envoyer des messages plus souvent qu’ils sont nécessaires. La connexion peut par conséquent devenir submergée. Cette section décrit comment mettre à jour le serveur pour ajouter un minuteur qui limite le taux des messages sortants.

1. Remplacez le contenu de `MoveShapeHub.cs` avec ce code :

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample5.cs)]

1. Sélectionnez le bouton lecture pour démarrer l’application.

1. Copiez l’URL de la page.

1. Ouvrir un autre navigateur et collez l’URL dans la barre d’adresses.

1. Faites glisser la forme de l’une des fenêtres de navigateur.

Ce code développe le client pour ajouter la `Broadcaster` classe. La nouvelle classe limite les messages sortants à l’aide de la `Timer` classe du .NET Framework.

Il est judicieux d’apprendre que le hub lui-même est transitoire. Il est créé chaque fois qu’il est nécessaire. L’application crée le `Broadcaster` comme un singleton. Elle utilise l’initialisation tardive pour différer la `Broadcaster`de la création jusqu'à ce qu’il est nécessaire. Qui garantit que l’application crée la première instance de hub complètement avant de démarrer la minuterie.

L’appel à les clients' `UpdateShape` fonction est ensuite déplacée hors du concentrateur `UpdateModel` (méthode). Il ne soit plus appelée dès que l’application reçoit les messages entrants. Au lieu de cela, l’application envoie les messages aux clients à un débit de 25 appels par seconde. Le processus est géré par le `_broadcastLoop` minuteur depuis la `Broadcaster` classe.

Enfin, au lieu d’appeler la méthode du client à partir du hub directement, le `Broadcaster` classe a besoin d’obtenir une référence à l’en cours d’exécution `_hubContext` hub. Il obtient la référence avec le `GlobalHost`.

## <a name="add-smooth-animation"></a>Ajouter des animations fluides

L’application est presque terminée, mais nous pourrions éventuellement effectuer une amélioration de plus. L’application déplace la forme sur le client en réponse aux messages du serveur. Au lieu de définir la position de la forme vers le nouvel emplacement donné par le serveur, utilisez la bibliothèque JQuery UI `animate` (fonction). Il peut déplacer la forme sans heurts entre sa position actuelle et nouvelle.

1. Mettre à jour le client `updateShape` méthode dans le *Default.html* fichier ressemble le code en surbrillance :

    [!code-html[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample6.html?highlight=33-40)]

1. Sélectionnez le bouton lecture pour démarrer l’application.

1. Copiez l’URL de la page.

1. Ouvrir un autre navigateur et collez l’URL dans la barre d’adresses.

1. Faites glisser la forme de l’une des fenêtres de navigateur.

Le déplacement de la forme dans l’autre fenêtre s’affiche moins saccadé. L’application effectue une interpolation son déplacement progressivement, plutôt que définie une seule fois par message entrant.

Ce code déplace la forme de l’ancien emplacement vers le nouveau. Le serveur donne la position de la forme au cours de l’intervalle de l’animation. Dans ce cas, qui est 100 millisecondes. L’application efface toute animation précédente est en cours d’exécution sur la forme avant le démarrage de la nouvelle animation.

## <a name="get-the-code"></a>Obtenir le code

[Télécharger le projet terminé](http://code.msdn.microsoft.com/SignalR-20-MoveShape-Demo-6285b83a)

## <a name="additional-resources"></a>Ressources supplémentaires

Le paradigme de communication que vous venez d’apprendre est utile pour le développement de jeux en ligne et autres simulations, comme [le jeu de ShootR créé avec SignalR](https://shootr.azurewebsites.net/).

Pour en savoir plus sur SignalR, consultez les ressources suivantes :

* [Projet de SignalR](http://signalr.net)

* [SignalR GitHub et exemples](https://github.com/SignalR/SignalR)

* [Wiki de SignalR](https://github.com/SignalR/SignalR/wiki)

## <a name="next-steps"></a>Étapes suivantes

Dans ce didacticiel, vous avez effectué les actions suivantes :

> [!div class="checklist"]
> * Configurer le projet
> * Création de l’application de base
> * Mappé au hub au démarrage de l’application
> * Ajouté le client
> * Exécution de l’application
> * Ajouté la boucle de client
> * Ajouté la boucle de serveur
> * Ajout des animations fluides

Passez à l’article suivant pour apprendre à créer une application web qui utilise ASP.NET SignalR 2 pour fournir des fonctionnalités de diffusion de serveur.
> [!div class="nextstepaction"]
> [SignalR 2 et la diffusion de serveur](tutorial-server-broadcast-with-signalr.md)