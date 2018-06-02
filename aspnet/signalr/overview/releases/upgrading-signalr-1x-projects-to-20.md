---
uid: signalr/overview/releases/upgrading-signalr-1x-projects-to-20
title: "La mise à niveau des projets de 1.x SignalR vers la version 2 | Documents Microsoft"
author: pfletcher
description: "Cette rubrique décrit comment mettre à niveau un projet de 1.x SignalR existant pour SignalR 2.x et comment résoudre les problèmes qui peuvent survenir pendant le processus de mise à niveau..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: adcfef99-9bc5-489d-a91b-9b7c2bc35e04
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/releases/upgrading-signalr-1x-projects-to-20
msc.type: authoredcontent
ms.openlocfilehash: e372275ae5dd4bbf354db2d02e4407f8c513b7a3
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/10/2017
---
<a name="upgrading-signalr-1x-projects-to-version-2"></a>La mise à niveau des projets de 1.x SignalR vers la version 2
====================
par [Patrick Fletcher](https://github.com/pfletcher)

> Cette rubrique décrit comment mettre à niveau un projet de 1.x SignalR existant pour SignalR 2.x et comment résoudre les problèmes qui peuvent survenir pendant le processus de mise à niveau.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versions du logiciel utilisées dans le didacticiel
> 
> 
> - [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - .NET 4.5
> - SignalR versions 1 et 2
>   
> 
> 
> ## <a name="using-visual-studio-2012-with-this-tutorial"></a>À l’aide de Visual Studio 2012 avec ce didacticiel
> 
> 
> Pour utiliser Visual Studio 2012 avec ce didacticiel, procédez comme suit :
> 
> - Mise à jour votre [Package Manager](http://docs.nuget.org/docs/start-here/installing-nuget) vers la dernière version.
> - Installer le [Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx).
> - Dans Web Platform Installer, recherchez et installez **ASP.NET et 2013.1 des outils Web pour Visual Studio 2012**. Il installe les modèles Visual Studio pour les classes de SignalR comme **Hub**.
> - Certains modèles (tel que **classe de démarrage OWIN**) ne sera pas disponible ; dans ce cas, utilisez un fichier de classe à la place.
> 
> 
> ## <a name="questions-and-comments"></a>Questions et des commentaires
> 
> Veuillez laisser des commentaires sur la façon dont vous avez aimé ce didacticiel et nous pouvons améliorer dans les commentaires en bas de la page. Si vous avez des questions qui ne sont pas directement liées à ce didacticiel, vous pouvez les valider pour le [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) ou [StackOverflow.com](http://stackoverflow.com/).


SignalR 2 offre une expérience de développement cohérent entre les plateformes de serveur à l’aide de [OWIN](http://owin.org). Cet article décrit les étapes nécessaires pour mettre à jour d’une application de 1.x SignalR vers la version 2.

Il est recommandé de mettre à niveau des applications à 2 de SignalR, SignalR 1.x sont toujours être prise en charge.

Ce didacticiel explique comment mettre à niveau une application web hébergée à SignalR 2. Les applications auto-hébergées (ceux qui hébergent un serveur dans une application console, le service Windows ou autre processus) sont désormais pris en charge sous SignalR 2. Pour plus d’informations sur la façon de commencer à créer une application auto-hébergée avec SignalR 2, consultez [didacticiel : SignalR auto-hébergement](../deployment/tutorial-signalr-self-host.md).

## <a name="contents"></a>Sommaire

Les sections suivantes décrivent les tâches impliquées dans la mise à niveau des projets de SignalR et comment résoudre les problèmes qui peuvent survenir.

- [Exemple : La mise à niveau le didacticiel mise en route pour SignalR 2](#example)
- [Dépannage des erreurs rencontrées lors de la mise à niveau](#troubleshooting)

<a id="example"></a>

## <a name="example-upgrading-the-getting-started-tutorial-application-to-signalr-2"></a>Exemple : La mise à niveau de l’application du didacticiel mise en route pour SignalR 2

Dans cette section, vous allez mettre à jour l’application créée dans le [version 1.x de SignalR du didacticiel de mise en route](../older-versions/index.md) pour utiliser 2 de SignalR.

1. Une fois que vous avez terminé le didacticiel de mise en route, avec le bouton droit sur le projet, puis sélectionnez **propriétés**. Vérifiez que le **framework cible** a la valeur **.NET Framework 4.5.**
2. Ouvrez la Console du Gestionnaire de Package. Supprimer SignalR 1.x à partir du projet à l’aide de la commande suivante :

    [!code-powershell[Main](upgrading-signalr-1x-projects-to-20/samples/sample1.ps1)]
3. Installer 2 SignalR à l’aide de la commande suivante :

    [!code-powershell[Main](upgrading-signalr-1x-projects-to-20/samples/sample2.ps1)]
4. Dans la page HTML, mettre à jour la référence de script pour SignalR correspondre à la version du script maintenant inclus dans le projet.

    [!code-html[Main](upgrading-signalr-1x-projects-to-20/samples/sample3.html)]
5. Dans la classe d’application globale, supprimez l’appel à MapHubs.

    [!code-csharp[Main](upgrading-signalr-1x-projects-to-20/samples/sample4.cs)]
6. Avec le bouton droit de la solution, puis sélectionnez **ajouter**, **un nouvel élément...** . Dans la boîte de dialogue, sélectionnez **classe de démarrage Owin**. Nommez la nouvelle classe **Startup.cs**.

    ![](upgrading-signalr-1x-projects-to-20/_static/image1.png)
7. Remplacez le contenu de Startup.cs par le code suivant :

    [!code-csharp[Main](upgrading-signalr-1x-projects-to-20/samples/sample5.cs)]

    L’attribut d’assembly ajoute la classe au processus de démarrage de Owin, qui exécute le `Configuration` méthode au démarrage de Owin. Cette action appelle à son tour le `MapSignalR` (méthode), ce qui crée des itinéraires pour tous les concentrateurs SignalR dans l’application.
8. Exécutez le projet et copiez l’URL de la page principale dans un autre navigateur ou le volet du navigateur, comme précédemment. Chaque page vous invite à entrer un nom d’utilisateur et les messages envoyés à partir de chaque page doivent être visibles dans les deux volets de navigateur.

<a id="troubleshooting"></a>

## <a name="troubleshooting-errors-encountered-during-upgrading"></a>Dépannage des erreurs rencontrées lors de la mise à niveau

Cette section décrit les problèmes qui peuvent survenir lors de la mise à niveau. Pour obtenir une liste plus complète des erreurs et les problèmes qui peuvent se produire avec une application SignalR, consultez [SignalR dépannage](../testing-and-debugging/troubleshooting.md).

### <a name="the-call-is-ambiguous-between-the-following-methods-or-properties"></a>'L’appel est ambigu entre les méthodes ou propriétés suivantes'

Cette erreur se produit si une référence à `Microsoft.AspNet.SignalR.Owin` n’est pas supprimé. Ce package est déconseillé ; la référence doit être supprimée, et la version 1.x du package SelfHost doit être désinstallée.

### <a name="hub-methods-fail-silently"></a>Méthodes de concentrateur échouent en silence

Vérifiez que les références de script dans votre client sont à jour, ainsi que le `OwinStartup` d’attributs pour votre classe de démarrage a la classe appropriée et noms d’assembly pour votre projet. En outre, essayez d’ouvrir l’adresse hubs (concentrateurs signalr / /) dans votre navigateur. toute erreur qui s’affiche propose de plus d’informations sur ce qui se passe incorrect.
