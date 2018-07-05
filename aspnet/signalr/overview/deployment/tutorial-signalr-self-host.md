---
uid: signalr/overview/deployment/tutorial-signalr-self-host
title: 'Didacticiel : Auto-hébergement de SignalR | Microsoft Docs'
author: pfletcher
description: Ce didacticiel montre comment créer un serveur de SignalR 2 auto-hébergé et comment s’y connecter avec un client JavaScript. Versions des logiciels utilisées dans le didacticiel V...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: 400db427-27af-4f2f-abf0-5486d5e024b5
ms.technology: dotnet-signalr
msc.legacyurl: /signalr/overview/deployment/tutorial-signalr-self-host
msc.type: authoredcontent
ms.openlocfilehash: 34e307499c7dabcd2afc73780649d79e68c10fdc
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37378996"
---
<a name="tutorial-signalr-self-host"></a>Didacticiel : Auto-hébergement de SignalR
====================
par [Patrick Fletcher](https://github.com/pfletcher)

[Télécharger le projet terminé](http://code.msdn.microsoft.com/SignalR-Self-Host-Sample-6da0f383)

> Ce didacticiel montre comment créer un serveur de SignalR 2 auto-hébergé et comment s’y connecter avec un client JavaScript.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versions des logiciels utilisées dans le didacticiel
> 
> 
> - [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - .NET 4.5
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
> ## <a name="questions-and-comments"></a>Questions et commentaires
> 
> Veuillez laisser des commentaires sur la façon dont vous avez apprécié ce didacticiel et ce que nous pouvions améliorer dans les commentaires en bas de la page. Si vous avez des questions qui ne sont pas directement liées à ce didacticiel, vous pouvez les publier à le [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) ou [StackOverflow.com](http://stackoverflow.com/).


## <a name="overview"></a>Vue d'ensemble

Un serveur SignalR est généralement hébergé dans une application ASP.NET dans IIS, mais il peut également être auto-hébergé (comme dans une application console ou un service de Windows) à l’aide de la bibliothèque Self-host. Cette bibliothèque, comme tous SignalR 2, est basée sur OWIN ([Open Web Interface pour .NET](http://owin.org)). OWIN définit une abstraction entre les serveurs web de .NET et des applications web. OWIN dissocie de l’application web à partir du serveur, ce qui rend OWIN idéale pour l’hébergement automatique d’une application web dans votre propre processus, en dehors d’IIS.

Voici quelques raisons pour ne pas d’hébergement dans IIS :

- Environnements où IIS n’est pas disponible ou souhaitable, par exemple une batterie de serveurs existante sans IIS.
- La surcharge de performances d’IIS doit être évitée.
- Fonctionnalité de SignalR consiste à être ajouté à une application existante qui s’exécute dans un Service Windows, rôle de travail Azure ou autre processus.

Si une solution est en cours de développement en tant que Self-host pour des raisons de performances, il est recommandé pour le test également l’application hébergée dans IIS pour déterminer l’amélioration des performances.

Ce didacticiel contient les sections suivantes :

- [Création du serveur](#server)
- [L’accès au serveur avec un client JavaScript](#js)

<a id="server"></a>

## <a name="creating-the-server"></a>Création du serveur

Dans ce didacticiel, vous allez créer un serveur qui est hébergé dans une application console, mais le serveur peut être hébergé dans une forme quelconque de processus, tel qu’un service de Windows ou le rôle de travail Azure. Pour l’exemple de code pour l’hébergement d’un serveur de SignalR dans un Service Windows, consultez [SignalR Self-Hosting dans un Service Windows](https://code.msdn.microsoft.com/SignalR-self-hosted-in-6ff7e6c3).

1. Ouvrez Visual Studio 2013 avec des privilèges d’administrateur. Sélectionnez **fichier**, **nouveau projet**. Sélectionnez **Windows** sous le **Visual C#** nœud dans le **modèles** volet, puis sélectionnez le **Application Console** modèle. Nommez le nouveau projet « SignalRSelfHost » et cliquez sur **OK**.

    ![](tutorial-signalr-self-host/_static/image1.png)
2. Ouvrez la console de gestionnaire de package de bibliothèque en sélectionnant **outils**, **Library Package Manager**, **Console du Gestionnaire de Package**.
3. Dans la console Gestionnaire de package, entrez la commande suivante :

    [!code-powershell[Main](tutorial-signalr-self-host/samples/sample1.ps1)]

    Cette commande ajoute les bibliothèques de SignalR 2 auto-héberger au projet.
4. Dans la console Gestionnaire de package, entrez la commande suivante :

    [!code-powershell[Main](tutorial-signalr-self-host/samples/sample2.ps1)]

    Cette commande ajoute la bibliothèque Microsoft.Owin.Cors au projet. Cette bibliothèque est utilisée pour la prise en charge de domaines, qui est requis pour les applications qui hébergent SignalR et un client de la page web dans des domaines différents. Dans la mesure où vous hébergerez le serveur SignalR et le client web sur des ports différents, cela signifie qu’inter-domaines doit être activé pour la communication entre ces composants.
5. Remplacez le contenu de Program.cs par le code suivant.

    [!code-csharp[Main](tutorial-signalr-self-host/samples/sample3.cs)]

    Le code ci-dessus comprend trois classes :

    - **Programme**, y compris le **Main** méthode définissant le chemin d’accès principal d’exécution. Dans cette méthode, une application web de type **démarrage** est démarré à l’URL spécifiée (`http://localhost:8080`). Si la sécurité est requise sur le point de terminaison, SSL peut être implémenté. Consultez [Comment : configurer un Port avec un certificat SSL](https://msdn.microsoft.com/library/ms733791.aspx) pour plus d’informations.
    - **Démarrage**, la classe contenant la configuration pour le serveur de SignalR (la seule configuration de ce didacticiel utilise est l’appel à `UseCors`) et l’appel à `MapSignalR`, ce qui crée des itinéraires pour tous les objets Hub dans le projet.
    - **Monconcentrateur**, la classe de concentrateur SignalR l’application fournira aux clients. Cette classe a une méthode unique, **envoyer**, que les clients appelleront pour diffuser un message à tous les autres clients connectés.
6. Compilez et exécutez l'application. L’adresse que le serveur est en cours d’exécution doit afficher dans une fenêtre de console.

    ![](tutorial-signalr-self-host/_static/image2.png)
7. Si l’exécution échoue avec l’exception `System.Reflection.TargetInvocationException was unhandled`, vous devez redémarrer Visual Studio avec des privilèges d’administrateur.
8. Arrêter l’application avant de passer à la section suivante.

<a id="js"></a>

## <a name="accessing-the-server-with-a-javascript-client"></a>L’accès au serveur avec un client JavaScript

Dans cette section, vous allez utiliser le même client JavaScript à partir de la [didacticiel mise en route](../getting-started/tutorial-getting-started-with-signalr.md). Nous veillerons à ce seulement une modification au client, qui consiste à définir explicitement l’URL du hub. Avec une application auto-hébergée, le serveur nécessairement peut-être pas à la même adresse en tant que l’URL de connexion (en raison des proxys inverses et les équilibreurs de charge), par conséquent, l’URL doit être défini explicitement.

1. Dans **l’Explorateur de solutions**, avec le bouton droit sur la solution et sélectionnez **ajouter**, **nouveau projet**. Sélectionnez le **Web** nœud, puis sélectionnez le **Application Web ASP.NET** modèle. Nommez le projet « JavascriptClient » et cliquez sur **OK**.

    ![](tutorial-signalr-self-host/_static/image3.png)
2. Sélectionnez le **vide** modèle et laissez les options restantes désélectionnées. Sélectionnez **créer le projet**.

    ![](tutorial-signalr-self-host/_static/image4.png)
3. Dans la console Gestionnaire de package, sélectionnez le projet « JavascriptClient » dans le **projet par défaut** liste déroulante et exécutez la commande suivante :

    [!code-powershell[Main](tutorial-signalr-self-host/samples/sample4.ps1)]

    Cette commande installe les bibliothèques de SignalR et JQuery dont vous aurez besoin dans le client.
4. Avec le bouton droit sur votre projet, puis sélectionnez **ajouter**, **un nouvel élément**. Sélectionnez le **Web** nœud et sélectionner la HTML Page. Nommez la page **Default.html**.

    ![](tutorial-signalr-self-host/_static/image5.png)
5. Remplacez le contenu de la nouvelle page HTML par le code suivant. Vérifiez que les références de script ici correspondent les scripts dans le dossier Scripts du projet.

    [!code-html[Main](tutorial-signalr-self-host/samples/sample5.html?highlight=31-32)]

    Le code suivant (mis en surbrillance dans l’exemple de code ci-dessus) est l’ajout que vous avez apportées au client utilisé dans le didacticiel bien regardais (en plus de mise à niveau le code vers SignalR version 2 Bêta). Cette ligne de code définit explicitement l’URL de connexion de base pour SignalR sur le serveur.

    [!code-javascript[Main](tutorial-signalr-self-host/samples/sample6.js)]
6. Avec le bouton droit sur la solution, puis sélectionnez **définir les projets de démarrage...** . Sélectionnez le **plusieurs projets de démarrage** case d’option et définissez des deux projets **Action** à **Démarrer**.

    ![](tutorial-signalr-self-host/_static/image6.png)
7. Avec le bouton droit sur « Default.html » et sélectionnez **définir comme Page de démarrage**.
8. Exécutez l'application. Lancement la page et le serveur. Vous devrez peut-être recharger la page web (ou sélectionnez **continuer** dans le débogueur) si la page se charge avant que le serveur est démarré.
9. Dans le navigateur, fournissez un nom d’utilisateur lorsque vous y êtes invité. Copier l’URL de la page dans une autre fenêtre ou un onglet de navigateur et fournir un nom d’utilisateur différent. Vous serez en mesure d’envoyer des messages à partir du volet d’un navigateur à l’autre, comme dans le didacticiel de mise en route.
