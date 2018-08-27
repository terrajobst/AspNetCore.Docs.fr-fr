---
title: ASP.NET Core SignalR JavaScript client
author: tdykstra
description: Vue d’ensemble du client d’ASP.NET Core SignalR JavaScript.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 08/14/2018
uid: signalr/javascript-client
ms.openlocfilehash: 639c30f1d145a3da5e4f5857f32c1b573c1bfce2
ms.sourcegitcommit: 2c158fcfd325cad97ead608a816e525fe3dcf757
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/14/2018
ms.locfileid: "41825706"
---
# <a name="aspnet-core-signalr-javascript-client"></a>ASP.NET Core SignalR JavaScript client

Par [Rachel Appel](http://twitter.com/rachelappel)

La bibliothèque cliente ASP.NET Core SignalR JavaScript permet aux développeurs d’appeler du code de concentrateur côté serveur.

[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/Docs/tree/live/aspnetcore/signalr/javascript-client/sample) ([procédure de téléchargement](xref:tutorials/index#how-to-download-a-sample))

## <a name="install-the-signalr-client-package"></a>Installer le package du client SignalR

La bibliothèque de client SignalR JavaScript est remise en tant qu’un [npm](https://www.npmjs.com/) package. Si vous utilisez Visual Studio, exécutez `npm install` à partir de la **Console du Gestionnaire de Package** lorsque vous êtes dans le dossier racine. Pour Visual Studio Code, exécutez la commande depuis le **Terminal intégré**.

  ```console
  npm init -y
  npm install @aspnet/signalr
  ```

npm installe le contenu du package dans le *node_modules\\@aspnet\signalr\dist\browser* dossier. Créez un dossier nommé *signalr* sous le *wwwroot\\lib* dossier. Copie le *signalr.js* de fichiers à la *wwwroot\lib\signalr* dossier.

## <a name="use-the-signalr-javascript-client"></a>Utilisez le client SignalR JavaScript

Référencer le client SignalR JavaScript dans les `<script>` élément.

```html
<script src="~/lib/signalr/signalr.js"></script>
```

## <a name="connect-to-a-hub"></a>Se connecter à un concentrateur

Le code suivant crée et démarre une connexion. Nom de concentrateur respecte la casse.

[!code-javascript[Call hub methods](javascript-client/sample/wwwroot/js/chat.js?range=9-12,28)]

### <a name="cross-origin-connections"></a>Connexions de cross-origin

En règle générale, les navigateurs chargent des connexions à partir du même domaine que la page demandée. Toutefois, il existe des occasions lorsqu’une connexion à un autre domaine est requise.

Pour empêcher la lecture des données sensibles à partir d’un autre site, un site malveillant [cross-origin connexions](xref:security/cors) sont désactivés par défaut. Pour autoriser une demande de cross-origin, activez-la dans le `Startup` classe.

[!code-csharp[Cross-origin connections](javascript-client/sample/Startup.cs?highlight=29-35,56)]

## <a name="call-hub-methods-from-client"></a>Appeler des méthodes de hub à partir du client

Les clients JavaScript appellent les méthodes publiques sur les hubs via le [appeler](/javascript/api/%40aspnet/signalr/hubconnection#invoke) méthode de la [HubConnection](/javascript/api/%40aspnet/signalr/hubconnection). Le `invoke` méthode accepte deux arguments :

* Le nom de la méthode de concentrateur. Dans l’exemple suivant, le nom de méthode sur le concentrateur est `SendMessage`.
* Tous les arguments définis dans la méthode de concentrateur. Dans l’exemple suivant, le nom de l’argument est `message`.

  [!code-javascript[Call hub methods](javascript-client/sample/wwwroot/js/chat.js?range=24)]

## <a name="call-client-methods-from-hub"></a>Appeler des méthodes de client à partir de hub

Pour recevoir des messages à partir du concentrateur, définir une méthode à l’aide de la [sur](/javascript/api/%40aspnet/signalr/hubconnection#on) méthode de la `HubConnection`.

* Le nom de la méthode du client JavaScript. Dans l’exemple suivant, le nom de la méthode est `ReceiveMessage`.
* Arguments du hub passe à la méthode. Dans l’exemple suivant, la valeur d’argument est `message`.

[!code-javascript[Receive calls from hub](javascript-client/sample/wwwroot/js/chat.js?range=14-19)]

Le code précédent dans `connection.on` s’exécute lorsque le code côté serveur appelle à l’aide de la [SendAsync](/dotnet/api/microsoft.aspnetcore.signalr.clientproxyextensions.sendasync) (méthode).

[!code-csharp[Call client-side](javascript-client/sample/hubs/chathub.cs?range=8-11)]

SignalR détermine la méthode de client à appeler en faisant correspondre le nom de méthode et arguments définis dans `SendAsync` et `connection.on`.

> [!NOTE]
> Comme meilleure pratique, appelez le [Démarrer](/javascript/api/%40aspnet/signalr/hubconnection#start) méthode sur le `HubConnection` après `on`. Ainsi, que vos gestionnaires sont enregistrés avant tous les messages sont reçus.

## <a name="error-handling-and-logging"></a>Journalisation et gestion des erreurs

Chaîne un `catch` méthode à la fin de la `start` méthode pour gérer les erreurs côté client. Utilisez `console.error` pour les erreurs de sortie à la console du navigateur.

[!code-javascript[Error handling](javascript-client/sample/wwwroot/js/chat.js?range=28)]

Configurer le suivi du journal côté client en passant un enregistreur d’événements et le type d’événement pour vous connecter lorsque la connexion est établie. Les messages sont enregistrés avec le niveau de journalisation spécifié et versions ultérieures. Niveaux de consignation disponibles sont les suivantes :

* `signalR.LogLevel.Error` &ndash; Messages d’erreur. Journaux `Error` messages uniquement.
* `signalR.LogLevel.Warning` &ndash; Messages d’avertissement concernant les erreurs potentielles. Journaux `Warning`, et `Error` messages.
* `signalR.LogLevel.Information` &ndash; Messages d’état sans erreurs. Journaux `Information`, `Warning`, et `Error` messages.
* `signalR.LogLevel.Trace` &ndash; Messages de trace. Consigne tout, y compris les données transportées entre le hub et le client.

Utilisez le [configureLogging](/javascript/api/%40aspnet/signalr/hubconnectionbuilder#configurelogging) méthode sur [HubConnectionBuilder](/javascript/api/%40aspnet/signalr/hubconnectionbuilder) pour configurer le niveau de journalisation. Les messages sont enregistrés dans la console du navigateur.

[!code-javascript[Logging levels](javascript-client/sample/wwwroot/js/chat.js?range=9-12)]

## <a name="related-resources"></a>Ressources connexes

* [Référence de l’API JavaScript](/javascript/api/)
* [Hubs](xref:signalr/hubs)
* [Client .NET](xref:signalr/dotnet-client)
* [Publier sur Azure](xref:signalr/publish-to-azure-web-app)
* [Autoriser les demandes de Cross-Origin (CORS) dans ASP.NET Core](xref:security/cors)
