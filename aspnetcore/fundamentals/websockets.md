---
title: Prise en charge des WebSockets dans ASP.NET Core
author: tdykstra
description: "Découvrez comment commencer à utiliser les WebSockets dans ASP.NET Core."
manager: wpickett
ms.author: tdykstra
ms.date: 03/25/2017
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/websockets
ms.openlocfilehash: 306eca28b9f1f66e1ccaf185ccae87db8dea1b01
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2018
---
# <a name="introduction-to-websockets-in-aspnet-core"></a>Introduction aux WebSockets dans ASP.NET Core

Par [Tom Dykstra](https://github.com/tdykstra) et [Andrew Stanton-Nurse](https://github.com/anurse)

Cet article explique comment commencer avec les WebSockets dans ASP.NET Core. [WebSocket](https://wikipedia.org/wiki/WebSocket) est un protocole qui autorise des canaux de communication persistants bidirectionnels sur les connexions TCP. Il est utilisé pour des applications comme le tchat, les transactions boursières, les jeux, partout où vous voulez des fonctionnalités en temps réel dans une application web.

[Affichez ou téléchargez un exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/websockets/sample) ([procédure de téléchargement](xref:tutorials/index#how-to-download-a-sample)). Pour plus d’informations, consultez la section [Étapes suivantes](#next-steps).


## <a name="prerequisites"></a>Prérequis

* ASP.NET Core 1.1 (ne s’exécute pas sur la version 1.0)
* Un système d’exploitation sur lequel ASP.NET Core s’exécute :
  
  * Windows 7/Windows Server 2008 et ultérieur
  * Linux
  * macOS

* **Exception** : Si votre application s’exécute sur Windows avec IIS, ou avec WebListener, vous devez utiliser :

  * Windows 8/Windows Server 2012 ou ultérieur
  * IIS 8/IIS 8 Express
  * WebSocket doit être activé dans IIS

* Pour connaître les navigateurs pris en charge, consultez http://caniuse.com/#feat=websockets.

## <a name="when-to-use-it"></a>Quand l'utiliser

Utilisez des WebSockets quand vous devez travailler directement avec une connexion de socket. Par exemple, vous pouvez avoir besoin des meilleures performances possibles pour un jeu en temps réel.

[ASP.NET SignalR](https://docs.microsoft.com/aspnet/signalr/overview/getting-started/introduction-to-signalr) fournit un modèle d’application plus avancé pour les fonctionnalités en temps réel, mais il s’exécute uniquement sur ASP.NET, pas sur ASP.NET Core. Une version Core de SignalR est en cours de développement. Pour suivre sa progression, consultez le [dépôt GitHub pour SignalR Core](https://github.com/aspnet/SignalR).

Si vous ne voulez pas attendre SignalR Core, vous pouvez utiliser directement des WebSockets maintenant. Toutefois, vous devrez peut-être développer des fonctionnalités que SignalR fournirait, notamment les suivantes :

* Prise en charge d’un plus grand nombre de versions de navigateur à l’aide d’un retour automatique à d’autres méthodes de transport.
* Reconnexion automatique en cas de perte de connexion.
* Prise en charge des clients appelant des méthodes sur le serveur, ou vice versa.
* Prise en charge de la mise à l’échelle sur plusieurs serveurs.

## <a name="how-to-use-it"></a>Utilisation

* Installez le package [Microsoft.AspNetCore.WebSockets](https://www.nuget.org/packages/Microsoft.AspNetCore.WebSockets/).
* Configurez l’intergiciel (middleware).
* Acceptez les requêtes WebSocket.
* Envoyez et recevez des messages.

### <a name="configure-the-middleware"></a>Configurer l’intergiciel (middleware)

Ajoutez l’intergiciel des WebSockets dans la méthode `Configure` de la classe `Startup`.

[!code-csharp[](websockets/sample/Startup.cs?name=UseWebSockets)]

Les paramètres suivants peuvent être configurés :

* `KeepAliveInterval` : fréquence d’envoi de frames de « ping » au client pour garantir que les proxys maintiennent la connexion ouverte.
* `ReceiveBufferSize` : taille de la mémoire tampon utilisée pour recevoir des données. Seuls les utilisateurs expérimentés doivent changer ce paramètre, pour l’optimisation des performances en fonction de la taille de leurs données.

[!code-csharp[](websockets/sample/Startup.cs?name=UseWebSocketsOptions)]

### <a name="accept-websocket-requests"></a>Accepter les requêtes WebSocket

Ultérieurement dans le cycle de vie de la requête (plus loin dans la méthode `Configure` ou dans une action MVC, par exemple), vérifiez s’il s’agit d’une requête WebSocket et acceptez-la.

Cet exemple est extrait d’une partie ultérieure de la méthode `Configure`.

[!code-csharp[](websockets/sample/Startup.cs?name=AcceptWebSocket&highlight=7)]

Une requête WebSocket peut figurer sur toute URL, mais cet exemple de code accepte uniquement les requêtes pour `/ws`.

### <a name="send-and-receive-messages"></a>Envoyer et recevoir des messages

La méthode `AcceptWebSocketAsync` met à niveau la connexion TCP vers une connexion WebSocket et vous donne un objet [WebSocket](https://docs.microsoft.com/dotnet/core/api/system.net.websockets.websocket). Utilisez l’objet WebSocket pour envoyer et recevoir des messages.

Le code indiqué précédemment qui accepte la requête WebSocket passe l’objet `WebSocket` à une méthode `Echo`. Voici la méthode `Echo`. Le code reçoit un message et renvoie immédiatement le même message. Il reste dans une boucle qui effectue cette action jusqu’à ce que le client ferme la connexion. 

[!code-csharp[](websockets/sample/Startup.cs?name=Echo)]

Quand vous acceptez le WebSocket avant de commencer cette boucle, le pipeline de l’intergiciel se termine.  À la fermeture du socket, le pipeline se déroule. Autrement dit, la requête cesse de progresser dans le pipeline quand vous acceptez un WebSocket, exactement comme quand vous atteignez une action MVC, par exemple.  Mais quand vous terminez cette boucle et que vous fermez le socket, la requête repart en arrière dans le pipeline.

## <a name="next-steps"></a>Étapes suivantes

[L’exemple d’application](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/websockets/sample) qui accompagne cet article est une application d’écho simple. Elle comporte une page web qui établit des connexions WebSocket, et le serveur renvoie simplement au client tous les messages qu’il reçoit. Exécutez-la à partir d’une invite de commandes (elle n’est pas configurée pour s’exécuter à partir de Visual Studio avec IIS Express) et accédez à http://localhost:5000. La page web affiche l’état de la connexion en haut à gauche :

![État initial de la page web](websockets/_static/start.png)

Sélectionnez **Connect** (Connexion) pour envoyer une requête WebSocket à l’URL affichée.  Entrez un message de test, puis sélectionnez **Send** (Envoyer). Quand vous avez terminé, sélectionnez **Close Socket** (Fermer le socket). La section **Communication Log** (Journal des communications) signale chaque action d’ouverture, d’envoi et de fermeture en temps réel.

![État initial de la page web](websockets/_static/end.png)
