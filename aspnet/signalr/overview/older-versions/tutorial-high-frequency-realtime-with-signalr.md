---
uid: signalr/overview/older-versions/tutorial-high-frequency-realtime-with-signalr
title: En temps réel haute fréquence avec SignalR 1.x | Microsoft Docs
author: bradygaster
description: Ce didacticiel montre comment créer une application web qui utilise ASP.NET SignalR pour fournir des fonctionnalités de messagerie à fréquence élevée. Fréquence élevée de messagerie dans...
ms.author: bradyg
ms.date: 04/16/2013
ms.assetid: ad2a5da5-2e79-40ea-bc84-028d327f5982
msc.legacyurl: /signalr/overview/older-versions/tutorial-high-frequency-realtime-with-signalr
msc.type: authoredcontent
ms.openlocfilehash: 6df35a420a0733003808a12d065b03f08ef56dd9
ms.sourcegitcommit: ebf4e5a7ca301af8494edf64f85d4a8deb61d641
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/24/2019
ms.locfileid: "54837921"
---
<a name="high-frequency-realtime-with-signalr-1x"></a>En temps réel haute fréquence avec SignalR 1.x
====================
par [Patrick Fletcher](https://github.com/pfletcher)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> Ce didacticiel montre comment créer une application web qui utilise ASP.NET SignalR pour fournir des fonctionnalités de messagerie à fréquence élevée. Échanges à fréquence élevée signifie dans ce cas de mises à jour qui sont envoyés à un taux fixe ; dans le cas de cette application, jusqu'à 10 messages par seconde.
> 
> L’application que vous allez créer dans ce didacticiel affiche une forme que les utilisateurs peuvent faire glisser. La position de la forme dans tous les autres navigateurs connectés sera ensuite être mis à jour pour correspondre à la position de la forme déplacée à l’aide de mises à jour a expiré.
> 
> Concepts présentés dans ce didacticiel ont des applications dans des jeux en temps réel et autres applications de simulation.
> 
> Commentaires sur le didacticiel sont les bienvenus. Si vous avez des questions qui ne sont pas directement liées à ce didacticiel, vous pouvez les publier à le [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) ou [StackOverflow.com](http://stackoverflow.com).


## <a name="overview"></a>Vue d'ensemble

Ce didacticiel montre comment créer une application qui partage l’état d’un objet avec d’autres navigateurs en temps réel. L’application que nous allons créer est appelée MoveShape. La page MoveShape affiche un élément HTML Div que l’utilisateur peut faire glisser ; Lorsque l’utilisateur fait glisser la balise Div, sa nouvelle position sera envoyée au serveur, qui vous indique toutes les autres clients connectés pour mettre à jour la position de la forme pour faire correspondre.

![La fenêtre d’application](tutorial-high-frequency-realtime-with-signalr/_static/image1.png)

L’application créée dans ce didacticiel est basée sur une démonstration par Damian Edwards. Vous voyez une vidéo contenant cette démonstration [ici](https://channel9.msdn.com/Series/Building-Web-Apps-with-ASP-NET-Jump-Start/Building-Web-Apps-with-ASPNET-Jump-Start-08-Real-time-Communication-with-SignalR).

Le didacticiel démarrera en montrant comment envoyer des messages SignalR à partir de chaque événement qui se déclenche lorsque la forme est glissée. Chaque client connecté met ensuite à jour la position de la version locale de la forme chaque fois qu’un message est reçu.

Bien que l’application fonctionne à l’aide de cette méthode, cela n’est pas un modèle de programmation recommandé, car il n’y aurait aucune limite supérieure au nombre de messages bien envoyés, afin que les clients et le serveur peuvent obtenir submergés par les messages et de performances se dégraderont . L’animation affichée sur le client serait également disjoint, comme la forme est déplacée instantanément par chaque méthode, plutôt que déplacement sans heurts vers chaque nouvel emplacement. Les sections suivantes de ce didacticiel va vous montrer comment créer une fonction de minuteur qui limite le taux maximal auquel les messages sont envoyés par le client ou le serveur et comment déplacer la forme sans heurts entre les emplacements. La version finale de l’application créée dans ce didacticiel peut être téléchargée à partir de [galerie de Code](https://code.msdn.microsoft.com/SignalR-MoveShape-demo-3366dac6).

Ce didacticiel contient les sections suivantes :

- [Composants requis](#prerequisites)
- [Créer le projet](#createtheproject)
- [Ajoutez les packages ASP.NET SignalR et JQuery.UI NuGet](#nugetpackages)
- [Créer l’application de base](#baseapp)
- [Ajouter la boucle de client](#clientloop)
- [Ajouter la boucle de serveur](#serverloop)
- [Ajouter des animations fluides sur le client](#animation)
- [Étapes supplémentaires](#furthersteps)

<a id="prerequisites"></a>

## <a name="prerequisites"></a>Prérequis

Ce didacticiel requiert Visual Studio 2012 ou Visual Studio 2010. Si Visual Studio 2010 est utilisé, le projet utilise .NET Framework 4, plutôt que .NET Framework 4.5.

Si vous utilisez Visual Studio 2012, il est recommandé d’installer le [mise à jour ASP.NET et Web Tools 2012.2](https://go.microsoft.com/fwlink/?LinkId=282650). Cette mise à jour contient de nouvelles fonctionnalités telles que des améliorations apportées aux fonctionnalités de publication, nouveau et les nouveaux modèles.

Si vous avez Visual Studio 2010, assurez-vous que [NuGet](https://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c) est installé.

<a id="createtheproject"></a>

## <a name="create-the-project"></a>Créer le projet

Dans cette section, nous allons créer le projet dans Visual Studio.

1. À partir de la **fichier** menu, cliquez sur **nouveau projet**.
2. Dans le **nouveau projet** boîte de dialogue, développez **c#** sous **modèles** et sélectionnez **Web**.
3. Sélectionnez le **Application Web ASP.NET vide** modèle, nommez le projet *MoveShapeDemo*, puis cliquez sur **OK**.

    ![Création du nouveau projet](tutorial-high-frequency-realtime-with-signalr/_static/image2.png)

<a id="nugetpackages"></a>

## <a name="add-the-signalr-and-jqueryui-nuget-packages"></a>Ajouter les Packages NuGet de JQuery.UI SignalR

Vous pouvez ajouter des fonctionnalités de SignalR à un projet en installant un package NuGet. Ce didacticiel utilise également le package JQuery.UI permettant d’attribuer la forme pour faire glisser et animée.

1. Cliquez sur **outils | Gestionnaire de Package NuGet | Console du Gestionnaire de package**.
2. Entrez la commande suivante dans le Gestionnaire de package.

    [!code-powershell[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample1.ps1)]

    Le package de SignalR installe un nombre d’autres packages NuGet en tant que dépendances. Lorsque l’installation est terminée. vous disposez de tous les composants serveur et client requises pour utiliser SignalR dans une application ASP.NET.
3. Entrez la commande suivante dans la console du Gestionnaire de package pour installer les packages de JQuery et JQuery.UI.

    [!code-powershell[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample2.ps1)]

<a id="baseapp"></a>

## <a name="create-the-base-application"></a>Créer l’application de base

Dans cette section, nous allons créer une application de navigateur qui envoie l’emplacement de la forme sur le serveur lors de chaque événement mouse move. Le serveur diffuse ensuite ces informations pour tous les autres clients connectés qu’elles sont reçues. Nous allons développer sur cette application dans les sections suivantes.

1. Dans **l’Explorateur de solutions**, avec le bouton droit sur le projet et sélectionnez **ajouter**, **classe...** . Nommez la classe **MoveShapeHub** et cliquez sur **ajouter**.
2. Remplacez le code dans le nouveau **MoveShapeHub** classe par le code suivant.

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample3.cs)]

    Le `MoveShapeHub` classe ci-dessus est une implémentation d’un concentrateur SignalR. Comme dans le [bien démarrer avec SignalR](index.md) didacticiel, le concentrateur a une méthode qui appellent directement les clients. Dans ce cas, le client envoie un objet contenant les nouvelles coordonnées X et Y de la forme sur le serveur, puis obtient diffusé à tous les autres clients connectés. SignalR sérialise automatiquement cet objet à l’aide de JSON.

    L’objet qui sera envoyé au client (`ShapeModel`) contient des membres pour stocker la position de la forme. La version de l’objet sur le serveur contient également un membre pour suivre les données de client sont stockées, afin que leurs propres données ne soit envoyé à un client donné. Ce membre utilise le `JsonIgnore` attribut pour éviter qu’il est sérialisé et envoyé au client.
3. Ensuite, nous allons configurer le hub au démarrage de l’application. Dans **l’Explorateur de solutions**, cliquez sur le projet, puis cliquez sur **ajouter | Classe d’Application globale**. Acceptez le nom par défaut *Global* et cliquez sur **OK**.

    ![Ajouter la classe d’Application globale](tutorial-high-frequency-realtime-with-signalr/_static/image3.png)
4. Ajoutez le code suivant `using` instruction après avoir fourni **à l’aide de** instructions dans la classe Global.asax.cs.

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample4.cs)]
5. Ajoutez la ligne suivante de code dans le `Application_Start` méthode de la classe Global pour inscrire l’itinéraire par défaut pour SignalR.

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample5.cs)]

    Votre fichier global.asax doit se présenter comme suit :

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample6.cs)]
6. Ensuite, nous allons ajouter le client. Dans **l’Explorateur de solutions**, cliquez sur le projet, puis cliquez sur **ajouter | Un nouvel élément**. Dans le **ajouter un nouvel élément** boîte de dialogue, sélectionnez **Html Page**. Donner un nom approprié à la page (comme **Default.html**) et cliquez sur **ajouter**.
7. Dans **l’Explorateur de solutions**, avec le bouton droit de la page que vous venez de créer, puis cliquez sur **définir comme Page de démarrage**.
8. Remplacez le code par défaut dans la page HTML par l’extrait de code suivant.

    > [!NOTE]
    > Vérifiez que le script fait référence ci-dessous correspondent aux packages ajoutés à votre projet dans le dossier Scripts. Dans Visual Studio 2010, la version de JQuery et SignalR ajouté au projet peut ne pas correspond au numéro de version ci-dessous.

    [!code-html[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample7.html)]

    Le code HTML et JavaScript ci-dessus crée un Div rouge appelé forme, Active le comportement de glissement de la forme à l’aide de la bibliothèque jQuery et utilise la forme `drag` événement à la position de la forme d’envoi au serveur.
9. Démarrez l’application en appuyant sur F5. Copiez l’URL de la page et collez-le dans une deuxième fenêtre de navigateur. Faites glisser la forme de l’une des fenêtres de navigateur ; la forme dans l’autre fenêtre de navigateur doit déplacer.

    ![La fenêtre d’application](tutorial-high-frequency-realtime-with-signalr/_static/image4.png)

<a id="clientloop"></a>

## <a name="add-the-client-loop"></a>Ajouter la boucle de client

Étant donné que l’envoi de l’emplacement de la forme sur chaque événement mouse move créera un montant n’est pas nécessaire de trafic réseau, les messages à partir du client doivent être limitées. Nous allons utiliser le code javascript `setInterval` fonction pour configurer une boucle qui envoie des informations sur la nouvelle position sur le serveur à un taux fixe. Cette boucle est une représentation très basique d’une « boucle de jeu », une fonction appelée à plusieurs reprises qui gère toutes les fonctionnalités d’un jeu ou une autre simulation.

1. Mettre à jour le code client dans la page HTML pour correspondre à l’extrait de code suivant.

    [!code-html[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample8.html)]

    La mise à jour ci-dessus ajoute le `updateServerModel` (fonction), qui est appelée sur une fréquence fixe. Cette fonction envoie les données de la position sur le serveur chaque fois que le `moved` indicateur indique qu’il existe de nouvelles données de position à envoyer.
2. Démarrez l’application en appuyant sur F5. Copiez l’URL de la page et collez-le dans une deuxième fenêtre de navigateur. Faites glisser la forme de l’une des fenêtres de navigateur ; la forme dans l’autre fenêtre de navigateur doit déplacer. Étant donné que le nombre de messages qui sont envoyés au serveur est limité, l’animation n’apparaître pas aussi fluide que dans la section précédente.

    ![La fenêtre d’application](tutorial-high-frequency-realtime-with-signalr/_static/image5.png)

<a id="serverloop"></a>

## <a name="add-the-server-loop"></a>Ajouter la boucle de serveur

Dans l’application actuelle, les messages envoyés à partir du serveur au client accède souvent qu’ils sont reçus. Cela pose un problème similaire, comme observé sur le client ; messages peuvent être envoyés plus souvent qu’ils sont nécessaires, et la connexion peut devenir submergée par conséquent. Cette section décrit comment mettre à jour le serveur pour implémenter une minuterie qui limite le taux des messages sortants.

1. Remplacez le contenu de `MoveShapeHub.cs` par l’extrait de code suivant.

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample9.cs)]

    Le code ci-dessus développe le client pour ajouter le `Broadcaster` (classe), ce qui limite la sortie des messages à l’aide de la `Timer` classe du .NET Framework.

    Étant donné que le hub lui-même est transitoire (elle est créée chaque fois qu’il est nécessaire), le `Broadcaster` sera créé comme un singleton. Initialisation tardive (introduite dans .NET 4) est utilisée pour différer sa création jusqu'à ce qu’il est nécessaire, en garantissant que la première instance de concentrateur est complètement créée avant le démarrage de la minuterie.

    L’appel à les clients' `UpdateShape` fonction est ensuite déplacée hors du concentrateur `UpdateModel` (méthode), afin qu’il ne soit plus appelée dès que les messages entrants sont reçus. Au lieu de cela, les messages pour les clients seront envoyés à un débit de 25 appels par seconde, géré par le `_broadcastLoop` minuteur depuis la `Broadcaster` classe.

    Enfin, au lieu d’appeler la méthode du client à partir du hub directement, le `Broadcaster` classe a besoin d’obtenir une référence au cours d’exécution hub (`_hubContext`) à l’aide de la `GlobalHost`.
2. Démarrez l’application en appuyant sur F5. Copiez l’URL de la page et collez-le dans une deuxième fenêtre de navigateur. Faites glisser la forme de l’une des fenêtres de navigateur ; la forme dans l’autre fenêtre de navigateur doit déplacer. Il y aura pas d’une différence visible dans le navigateur à partir de la section précédente, mais le nombre de messages qui sont envoyés au client est limité.

    ![La fenêtre d’application](tutorial-high-frequency-realtime-with-signalr/_static/image6.png)

<a id="animation"></a>

## <a name="add-smooth-animation-on-the-client"></a>Ajouter des animations fluides sur le client

L’application est presque terminée, mais nous pourrions éventuellement effectuer plus améliorer, dans le mouvement de la forme sur le client lorsqu’il est déplacé en réponse aux messages du serveur. Au lieu de définir la position de la forme vers le nouvel emplacement donné par le serveur, nous allons utiliser la bibliothèque JQuery UI `animate` (fonction) pour déplacer la forme sans heurts entre sa position actuelle et nouvelle.

1. Mettre à jour le client `updateShape` méthode à rechercher comme le code en surbrillance ci-dessous :

    [!code-html[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample10.html?highlight=35-42)]

    Le code ci-dessus déplace la forme de l’ancien emplacement vers le nouveau donné par le serveur au cours de l’intervalle d’animation (dans ce cas, il s’agit de 100 millisecondes). Toute animation précédente est en cours d’exécution sur la forme est effacée avant que la nouvelle animation démarre.
2. Démarrez l’application en appuyant sur F5. Copiez l’URL de la page et collez-le dans une deuxième fenêtre de navigateur. Faites glisser la forme de l’une des fenêtres de navigateur ; la forme dans l’autre fenêtre de navigateur doit déplacer. Le déplacement de la forme dans l’autre fenêtre doit apparaître moins saccadé comme son déplacement est interpolé progressivement, plutôt que définie une seule fois par message entrant.

    ![La fenêtre d’application](tutorial-high-frequency-realtime-with-signalr/_static/image7.png)

<a id="furthersteps"></a>

## <a name="further-steps"></a>Étapes supplémentaires

Dans ce didacticiel, vous avez appris comment programmer une application SignalR qui envoie des messages à fréquence élevée entre les clients et serveurs. Ce paradigme de communication est utile pour le développement de jeux en ligne et autres simulations, tel que [le jeu de ShootR créé avec SignalR](http://shootr.signalr.net).

L’application complète créée dans ce didacticiel peut être téléchargée à partir de [galerie de Code](https://code.msdn.microsoft.com/SignalR-MoveShape-demo-3366dac6).

Pour en savoir plus sur les concepts de développement SignalR, visitez les sites suivants pour le code source de SignalR et de ressources :

- [Projet de SignalR](http://signalr.net)
- [SignalR Github et exemples](https://github.com/SignalR/SignalR)
- [Wiki de SignalR](https://github.com/SignalR/SignalR/wiki)
