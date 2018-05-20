---
title: Prise en charge des WebSockets dans ASP.NET Core
author: rick-anderson
description: Découvrez comment commencer à utiliser les WebSockets dans ASP.NET Core.
manager: wpickett
ms.author: tdykstra
ms.custom: mvc
ms.date: 02/15/2018
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/websockets
ms.openlocfilehash: ede8064b5e77024b843357d4715869b3495b9147
ms.sourcegitcommit: a19261eb82b948af6e4a1664fcfb8dabb16150e3
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/14/2018
---
# <a name="websockets-support-in-aspnet-core"></a>Prise en charge des WebSockets dans ASP.NET Core

Par [Tom Dykstra](https://github.com/tdykstra) et [Andrew Stanton-Nurse](https://github.com/anurse)

Cet article explique comment commencer avec les WebSockets dans ASP.NET Core. [WebSocket](https://wikipedia.org/wiki/WebSocket) ([RFC 6455](https://tools.ietf.org/html/rfc6455)) est un protocole qui autorise des canaux de communication persistants bidirectionnels sur les connexions TCP. Il est utilisé dans les applications qui bénéficient de communications rapides et en temps réel, comme les applications de conversation, de tableau de bord et de jeu.

[Affichez ou téléchargez un exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/websockets/sample) ([procédure de téléchargement](xref:tutorials/index#how-to-download-a-sample)). Pour plus d’informations, consultez la section [Étapes suivantes](#next-steps).

## <a name="prerequisites"></a>Prérequis

* ASP.NET Core 1.1 ou ultérieur
* Tout système d’exploitation prenant en charge ASP.NET Core :
  
  * Windows 7 / Windows Server 2008 ou ultérieur
  * Linux
  * macOS
  
* Si l’application s’exécute sur Windows avec IIS :

  * Windows 8/Windows Server 2012 ou ultérieur
  * IIS 8/IIS 8 Express
  * WebSocket doit être activé dans IIS (consultez la section [Prise en charge d’IIS/IIS Express](#iisiis-express-support).)
  
* Si l’application s’exécute sur [HTTP.sys](xref:fundamentals/servers/httpsys) :

  * Windows 8/Windows Server 2012 ou ultérieur

* Pour les navigateurs pris en charge, consultez https://caniuse.com/#feat=websockets.

## <a name="when-to-use-websockets"></a>Quand utiliser WebSocket

Utilisez WebSocket pour travailler directement avec une connexion de socket. Par exemple, utilisez WebSocket de façon à obtenir les meilleures performances possibles pour un jeu en temps réel.

[ASP.NET SignalR](/aspnet/signalr/overview/getting-started/introduction-to-signalr) fournit un modèle d’application plus avancé pour les fonctionnalités temps réel, mais il s’exécute seulement sur ASP.NET 4.x, et pas sur ASP.NET Core. Une version ASP.NET Core de SignalR est prévue avec la publication d’ASP.NET Core 2.1. Consultez [ASP.NET Core 2.1 high-level planning](https://github.com/aspnet/Announcements/issues/288).

D’ici là, vous pouvez utiliser WebSocket. Cependant, des fonctionnalités fournies par SignalR doivent être fournies et prises en charge par le développeur. Exemple :

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

Ajoutez le middleware WebSocket dans la méthode `Configure` de la classe `Startup` :

[!code-csharp[](websockets/sample/Startup.cs?name=UseWebSockets)]

Les paramètres suivants peuvent être configurés :

* `KeepAliveInterval` : fréquence d’envoi de frames de « ping » au client pour garantir que les proxys maintiennent la connexion ouverte.
* `ReceiveBufferSize` : taille de la mémoire tampon utilisée pour recevoir des données. Seuls les utilisateurs avancés peuvent être amenés à changer ce paramètre, pour l’optimisation des performances en fonction de la taille des données.

[!code-csharp[](websockets/sample/Startup.cs?name=UseWebSocketsOptions)]

### <a name="accept-websocket-requests"></a>Accepter les requêtes WebSocket

Ultérieurement dans le cycle de vie de la requête (plus loin dans la méthode `Configure` ou dans une action MVC, par exemple), vérifiez s’il s’agit d’une requête WebSocket et acceptez-la.

L’exemple suivant est extrait d’une partie ultérieure de la méthode `Configure` :

[!code-csharp[](websockets/sample/Startup.cs?name=AcceptWebSocket&highlight=7)]

Une requête WebSocket peut figurer sur toute URL, mais cet exemple de code accepte uniquement les requêtes pour `/ws`.

### <a name="send-and-receive-messages"></a>Envoyer et recevoir des messages

La méthode `AcceptWebSocketAsync` met à niveau la connexion TCP vers une connexion WebSocket et fournit un objet [WebSocket](/dotnet/core/api/system.net.websockets.websocket). Utilisez l’objet `WebSocket` pour envoyer et recevoir des messages.

Le code montré précédemment qui accepte la requête WebSocket passe l’objet `WebSocket` à une méthode `Echo`. Le code reçoit un message et renvoie immédiatement le même message. Les messages sont envoyés et reçus dans une boucle jusqu’à ce que le client ferme la connexion :

[!code-csharp[](websockets/sample/Startup.cs?name=Echo)]

Quand vous acceptez le WebSocket avant de commencer cette boucle, le pipeline de middlewares se termine. À la fermeture du socket, le pipeline se déroule. Autrement dit, la requête cesse d’avancer dans le pipeline quand le WebSocket est accepté. Quand la boucle est terminée et que le socket est fermé, la requête recule dans le pipeline.

## <a name="iisiis-express-support"></a>Prise en charge d’IIS/IIS Express

Windows Server 2012 ou ultérieur et Windows 8 ou ultérieur avec IIS/IIS Express 8 ou ultérieure prennent en charge le protocole WebSocket.

Pour activer la prise en charge du protocole WebSocket sur Windows Server 2012 ou ultérieur :

1. Utilisez l’Assistant **Ajouter des rôles et des fonctionnalités** par le biais du menu **Gérer** ou du lien dans **Gestionnaire de serveur**.
1. Sélectionnez **Installation basée sur un rôle ou une fonctionnalité**. Sélectionnez **Suivant**.
1. Sélectionnez le serveur approprié (le serveur local est sélectionné par défaut). Sélectionnez **Suivant**.
1. Développez **Serveur web (IIS)** dans l’arborescence **Rôles**, développez **Serveur Web**, puis développez **Développement d’applications**.
1. Sélectionnez **Protocole WebSocket**. Sélectionnez **Suivant**.
1. Si vous n’avez pas besoin d’autres fonctionnalités, sélectionnez **Suivant**.
1. Cliquez sur **Installer**.
1. Une fois l’installation terminée, sélectionnez **Fermer** pour quitter l’Assistant.

Pour activer la prise en charge du protocole WebSocket sur Windows 8 ou ultérieur :

1. Accédez à **Panneau de configuration** > **Programmes** > **Programmes et fonctionnalités** > **Activer ou désactiver des fonctionnalités Windows** (à gauche de l’écran).
1. Ouvrez les nœuds suivants : **Internet Information Services** > **Services World Wide Web** > **Fonctionnalités de développement d’applications**.
1. Sélectionnez la fonctionnalité **Protocole WebSocket**. Sélectionnez **OK**.

**Désactiver WebSocket lors de l’utilisation de socket.io sur node.js**

Si vous utilisez la prise en charge de WebSocket dans [socket.io](https://socket.io/) sur [Node.js](https://nodejs.org/), désactivez le module WebSocket IIS par défaut en utilisant l’élément `webSocket` dans *web.config* ou dans *applicationHost.config*. Si cette étape n’est pas effectuée, le module WebSocket IIS tente de gérer la communication WebSocket, au lieu de celle de Node.js et de l’application.

```xml
<system.webServer>
  <webSocket enabled="false" />
</system.webServer>
```

## <a name="next-steps"></a>Étapes suivantes

[L’exemple d’application](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/websockets/sample) qui accompagne cet article est une application d’écho. Elle comporte une page web qui établit des connexions WebSocket, et le serveur renvoie au client les messages qu’il reçoit. Exécutez l’application à partir d’une invite de commandes (elle n’est pas configurée pour s’exécuter à partir de Visual Studio avec IIS Express) et accédez à http://localhost:5000. La page web affiche l’état de la connexion en haut à gauche :

![État initial de la page web](websockets/_static/start.png)

Sélectionnez **Connect** (Connexion) pour envoyer une requête WebSocket à l’URL affichée. Entrez un message de test, puis sélectionnez **Send** (Envoyer). Quand vous avez terminé, sélectionnez **Close Socket** (Fermer le socket). La section **Communication Log** (Journal des communications) signale chaque action d’ouverture, d’envoi et de fermeture en temps réel.

![État initial de la page web](websockets/_static/end.png)
