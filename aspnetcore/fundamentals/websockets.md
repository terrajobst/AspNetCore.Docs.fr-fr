---
title: Prise en charge des WebSockets dans ASP.NET Core
author: rick-anderson
description: Découvrez comment commencer à utiliser les WebSockets dans ASP.NET Core.
monikerRange: '>= aspnetcore-1.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 01/17/2019
uid: fundamentals/websockets
ms.openlocfilehash: 76acb9c96ed5e8bbbaf39eeb6cb23307bb44fb8d
ms.sourcegitcommit: ebf4e5a7ca301af8494edf64f85d4a8deb61d641
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/24/2019
ms.locfileid: "54836856"
---
# <a name="websockets-support-in-aspnet-core"></a>Prise en charge des WebSockets dans ASP.NET Core

Par [Tom Dykstra](https://github.com/tdykstra) et [Andrew Stanton-Nurse](https://github.com/anurse)

Cet article explique comment commencer avec les WebSockets dans ASP.NET Core. [WebSocket](https://wikipedia.org/wiki/WebSocket) ([RFC 6455](https://tools.ietf.org/html/rfc6455)) est un protocole qui autorise des canaux de communication persistants bidirectionnels sur les connexions TCP. Il est utilisé dans les applications qui bénéficient de communications rapides et en temps réel, comme les applications de conversation, de tableau de bord et de jeu.

[Affichez ou téléchargez un exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/websockets/samples) ([procédure de téléchargement](xref:index#how-to-download-a-sample)). Pour plus d’informations, consultez la section [Étapes suivantes](#next-steps).

## <a name="prerequisites"></a>Prérequis

* ASP.NET Core 1.1 ou ultérieur
* Tout système d’exploitation prenant en charge ASP.NET Core :
  
  * Windows 7 / Windows Server 2008 ou ultérieur
  * Linux
  * macOS
  
* Si l’application s’exécute sur Windows avec IIS :

  * Windows 8/Windows Server 2012 ou ultérieur
  * IIS 8/IIS 8 Express
  * WebSockets doit être activé (consultez la section [Prise en charge d’IIS/IIS Express](#iisiis-express-support).)
  
* Si l’application s’exécute sur [HTTP.sys](xref:fundamentals/servers/httpsys) :

  * Windows 8/Windows Server 2012 ou ultérieur

* Pour les navigateurs pris en charge, consultez https://caniuse.com/#feat=websockets.

## <a name="when-to-use-websockets"></a>Quand utiliser WebSocket

Utilisez WebSocket pour travailler directement avec une connexion de socket. Par exemple, utilisez WebSocket de façon à obtenir les meilleures performances possibles pour un jeu en temps réel.

[ASP.NET Core SignalR](xref:signalr/introduction) est une bibliothèque qui simplifie l’ajout de fonctionnalités web en temps réel aux applications. Elle utilise des WebSockets dans la mesure du possible.

## <a name="how-to-use-websockets"></a>Comment utiliser des WebSockets ?

* Installez le package [Microsoft.AspNetCore.WebSockets](https://www.nuget.org/packages/Microsoft.AspNetCore.WebSockets/).
* Configurez l’intergiciel (middleware).
* Acceptez les requêtes WebSocket.
* Envoyez et recevez des messages.

### <a name="configure-the-middleware"></a>Configurer l’intergiciel (middleware)

Ajoutez le middleware WebSocket dans la méthode `Configure` de la classe `Startup` :

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](websockets/samples/2.x/WebSocketsSample/Startup.cs?name=UseWebSockets)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](websockets/samples/1.x/WebSocketsSample/Startup.cs?name=UseWebSockets)]

::: moniker-end

::: moniker range="< aspnetcore-2.2"

Les paramètres suivants peuvent être configurés :

* `KeepAliveInterval` : fréquence d’envoi de frames de « ping » au client pour garantir que les proxys maintiennent la connexion ouverte. La valeur par défaut est deux minutes.
* `ReceiveBufferSize` : taille de la mémoire tampon utilisée pour recevoir des données. Seuls les utilisateurs avancés peuvent être amenés à changer ce paramètre, pour l’optimisation des performances en fonction de la taille des données. La valeur par défaut est 4 Ko.

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

Les paramètres suivants peuvent être configurés :

* `KeepAliveInterval` : fréquence d’envoi de frames de « ping » au client pour garantir que les proxys maintiennent la connexion ouverte. La valeur par défaut est deux minutes.
* `ReceiveBufferSize` : taille de la mémoire tampon utilisée pour recevoir des données. Seuls les utilisateurs avancés peuvent être amenés à changer ce paramètre, pour l’optimisation des performances en fonction de la taille des données. La valeur par défaut est 4 Ko.
* `AllowedOrigins` : liste des valeurs d’en-tête Origin autorisées pour les requêtes WebSocket. Par défaut, toutes les origines sont autorisées. Pour plus d’informations, consultez « Restriction d’origine WebSocket » ci-dessous.

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](websockets/samples/2.x/WebSocketsSample/Startup.cs?name=UseWebSocketsOptions)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](websockets/samples/1.x/WebSocketsSample/Startup.cs?name=UseWebSocketsOptions)]

::: moniker-end

### <a name="accept-websocket-requests"></a>Accepter les requêtes WebSocket

Ultérieurement dans le cycle de vie de la requête (plus loin dans la méthode `Configure` ou dans une action MVC, par exemple), vérifiez s’il s’agit d’une requête WebSocket et acceptez-la.

L’exemple suivant est extrait d’une partie ultérieure de la méthode `Configure` :

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](websockets/samples/2.x/WebSocketsSample/Startup.cs?name=AcceptWebSocket&highlight=7)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](websockets/samples/1.x/WebSocketsSample/Startup.cs?name=AcceptWebSocket&highlight=7)]

::: moniker-end

Une requête WebSocket peut figurer sur toute URL, mais cet exemple de code accepte uniquement les requêtes pour `/ws`.

### <a name="send-and-receive-messages"></a>Envoyer et recevoir des messages

La méthode `AcceptWebSocketAsync` met à niveau la connexion TCP vers une connexion WebSocket et fournit un objet [WebSocket](/dotnet/core/api/system.net.websockets.websocket). Utilisez l’objet `WebSocket` pour envoyer et recevoir des messages.

Le code montré précédemment qui accepte la requête WebSocket passe l’objet `WebSocket` à une méthode `Echo`. Le code reçoit un message et renvoie immédiatement le même message. Les messages sont envoyés et reçus dans une boucle jusqu’à ce que le client ferme la connexion :

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](websockets/samples/2.x/WebSocketsSample/Startup.cs?name=Echo)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](websockets/samples/1.x/WebSocketsSample/Startup.cs?name=Echo)]

::: moniker-end

Quand vous acceptez le WebSocket avant de commencer cette boucle, le pipeline de middlewares se termine. À la fermeture du socket, le pipeline se déroule. Autrement dit, la requête cesse d’avancer dans le pipeline quand le WebSocket est accepté. Quand la boucle est terminée et que le socket est fermé, la requête recule dans le pipeline.

::: moniker range=">= aspnetcore-2.2"

### <a name="handle-client-disconnects"></a>Gérer la déconnexion du client

Le serveur n’est pas automatiquement informé lorsque le client se déconnecte en raison d’une perte de connectivité. Le serveur reçoit un message de déconnexion uniquement si le client l’envoie, ce qui n’est pas possible si la connexion Internet est perdue. Si vous souhaitez prendre des mesures quand cela se produit, définissez un délai d’attente lorsque rien n’est reçu du client dans un certain laps de temps.

Si le client n’envoie toujours pas de messages et si vous ne souhaitez pas appliquer le délai d’attente uniquement parce que la connexion devient inactive, demandez au client d’utiliser un minuteur pour envoyer un message ping toutes les X secondes. Sur le serveur, si un message n’arrive pas dans un délai de 2\*X secondes après le précédent, mettez fin à la connexion et signalez que le client s’est déconnecté. Attendez deux fois la durée de l’intervalle de temps attendu pour autoriser les délais réseau qui peuvent retarder le message ping.

### <a name="websocket-origin-restriction"></a>Restriction d’origine WebSocket

Les protections fournies par CORS ne s’appliquent pas aux WebSockets. Les navigateurs **:**

* n’effectuent pas de requêtes préalables CORS ;
* respectent les restrictions spécifiées dans les en-têtes `Access-Control` quand ils effectuent des requêtes WebSocket.

Toutefois, les navigateurs envoient l’en-tête `Origin` au moment de l’émission des requêtes WebSocket. Les applications doivent être configurées de manière à valider ces en-têtes, le but étant de vérifier que seuls les WebSockets provenant des origines attendues sont autorisés.

Si vous hébergez votre serveur sur « https://server.com» et votre client sur « https://client.com», ajoutez « https://client.com» à la liste `AllowedOrigins` pour autoriser les WebSockets.

[!code-csharp[](websockets/samples/2.x/WebSocketsSample/Startup.cs?name=UseWebSocketsOptionsAO&highlight=6-7)]

> [!NOTE]
> L’en-tête `Origin` est contrôlé par le client et, comme l’en-tête `Referer`, peut être falsifié. **N’utilisez pas** ces en-têtes comme mécanisme d’authentification.

::: moniker-end

## <a name="iisiis-express-support"></a>Prise en charge d’IIS/IIS Express

Windows Server 2012 ou ultérieur et Windows 8 ou ultérieur avec IIS/IIS Express 8 ou ultérieure prennent en charge le protocole WebSocket.

> [!NOTE]
> WebSockets sont toujours activés lorsque vous utilisez IIS Express.

### <a name="enabling-websockets-on-iis"></a>Activation de WebSockets sur IIS

Pour activer la prise en charge du protocole WebSocket sur Windows Server 2012 ou ultérieur :

> [!NOTE]
> Ces étapes ne sont pas nécessaires si vous utilisez IIS Express

1. Utilisez l’Assistant **Ajouter des rôles et des fonctionnalités** par le biais du menu **Gérer** ou du lien dans **Gestionnaire de serveur**.
1. Sélectionnez **Installation basée sur un rôle ou une fonctionnalité**. Sélectionnez **Suivant**.
1. Sélectionnez le serveur approprié (le serveur local est sélectionné par défaut). Sélectionnez **Suivant**.
1. Développez **Serveur web (IIS)** dans l’arborescence **Rôles**, développez **Serveur Web**, puis développez **Développement d’applications**.
1. Sélectionnez **Protocole WebSocket**. Sélectionnez **Suivant**.
1. Si vous n’avez pas besoin d’autres fonctionnalités, sélectionnez **Suivant**.
1. Cliquez sur **Installer**.
1. Une fois l’installation terminée, sélectionnez **Fermer** pour quitter l’Assistant.

Pour activer la prise en charge du protocole WebSocket sur Windows 8 ou ultérieur :

> [!NOTE]
> Ces étapes ne sont pas nécessaires si vous utilisez IIS Express

1. Accédez à **Panneau de configuration** > **Programmes** > **Programmes et fonctionnalités** > **Activer ou désactiver des fonctionnalités Windows** (à gauche de l’écran).
1. Ouvrez les nœuds suivants : **Internet Information Services** > **Services World Wide Web** > **Fonctionnalités de développement d’applications**.
1. Sélectionnez la fonctionnalité **Protocole WebSocket**. Sélectionnez **OK**.

### <a name="disable-websocket-when-using-socketio-on-nodejs"></a>Désactiver WebSocket lors de l’utilisation de socket.io sur Node.js

Si vous utilisez la prise en charge de WebSocket dans [socket.io](https://socket.io/) sur [Node.js](https://nodejs.org/), désactivez le module WebSocket IIS par défaut en utilisant l’élément `webSocket` dans *web.config* ou dans *applicationHost.config*. Si cette étape n’est pas effectuée, le module WebSocket IIS tente de gérer la communication WebSocket, au lieu de celle de Node.js et de l’application.

```xml
<system.webServer>
  <webSocket enabled="false" />
</system.webServer>
```

## <a name="next-steps"></a>Étapes suivantes

[L’exemple d’application](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/websockets/samples) qui accompagne cet article est une application d’écho. Elle comporte une page web qui établit des connexions WebSocket, et le serveur renvoie au client les messages qu’il reçoit. Exécutez l’application à partir d’une invite de commandes (elle n’est pas configurée pour s’exécuter à partir de Visual Studio avec IIS Express) et accédez à http://localhost:5000. La page web affiche l’état de la connexion en haut à gauche :

![État initial de la page web](websockets/_static/start.png)

Sélectionnez **Connect** (Connexion) pour envoyer une requête WebSocket à l’URL affichée. Entrez un message de test, puis sélectionnez **Send** (Envoyer). Quand vous avez terminé, sélectionnez **Close Socket** (Fermer le socket). La section **Communication Log** (Journal des communications) signale chaque action d’ouverture, d’envoi et de fermeture en temps réel.

![État initial de la page web](websockets/_static/end.png)
