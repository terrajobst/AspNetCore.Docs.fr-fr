---
title: ASP.NET Core SignalR JavaScript client
author: tdykstra
description: Vue d’ensemble du client d’ASP.NET Core SignalR JavaScript.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 05/29/2018
uid: signalr/javascript-client
ms.openlocfilehash: c13c41b0344b0c880e842f2799d6ee97bd7fff7e
ms.sourcegitcommit: 3ca527f27c88cfc9d04688db5499e372fbc2c775
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/17/2018
ms.locfileid: "39095422"
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

Npm installe le contenu du package dans le *node_modules\\ @aspnet\signalr\dist\browser*  dossier. Créez un dossier nommé *signalr* sous le *wwwroot\\lib* dossier. Copie le *signalr.js* de fichiers à la *wwwroot\lib\signalr* dossier.

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

Les clients JavaScript appellent les méthodes publiques sur Event hubs grâce à l’aide de `connection.invoke`. Le `invoke` méthode accepte deux arguments :

* Le nom de la méthode de concentrateur. Dans l’exemple suivant, le nom de hub est `SendMessage`.
* Tous les arguments définis dans la méthode de concentrateur. Dans l’exemple suivant, le nom de l’argument est `message`.

[!code-javascript[Call hub methods](javascript-client/sample/wwwroot/js/chat.js?range=24)]

## <a name="call-client-methods-from-hub"></a>Appeler des méthodes de client à partir de hub

Pour recevoir des messages à partir du concentrateur, définir une méthode à l’aide de la `connection.on` (méthode).

* Le nom de la méthode du client JavaScript. Dans l’exemple suivant, le nom de la méthode est `ReceiveMessage`.
* Arguments du hub passe à la méthode. Dans l’exemple suivant, la valeur d’argument est `message`.

[!code-javascript[Receive calls from hub](javascript-client/sample/wwwroot/js/chat.js?range=14-19)]

Le code précédent dans `connection.on` s’exécute lorsque le code côté serveur l’appelle en utilisant la méthode `SendAsync`.

[!code-csharp[Call client-side](javascript-client/sample/hubs/chathub.cs?range=8-11)]

SignalR détermine la méthode de client à appeler en faisant correspondre le nom de méthode et arguments définis dans `SendAsync` et `connection.on`.

> [!NOTE]
> Il est recommandé, appelez `connection.start` après `connection.on` donc vos gestionnaires sont enregistrés avant tous les messages sont reçus.

## <a name="error-handling-and-logging"></a>Journalisation et gestion des erreurs

Chaîne un `catch` méthode à la fin de la `connection.start` méthode pour gérer les erreurs côté client. Utilisez `console.error` pour les erreurs de sortie à la console du navigateur.

[!code-javascript[Error handling](javascript-client/sample/wwwroot/js/chat.js?range=28)]

Configurer le suivi du journal côté client en passant un enregistreur d’événements et le type d’événement pour vous connecter lorsque la connexion est établie. Les messages sont enregistrés avec le niveau de journalisation spécifié et versions ultérieures. Niveaux de consignation disponibles sont les suivantes :

* `signalR.LogLevel.Error` : Messages d’erreur. Journaux `Error` messages uniquement.
* `signalR.LogLevel.Warning` : Messages d’avertissement concernant les erreurs potentielles. Journaux `Warning`, et `Error` messages.
* `signalR.LogLevel.Information` : Messages d’état sans erreurs. Journaux `Information`, `Warning`, et `Error` messages.
* `signalR.LogLevel.Trace` : Messages de trace. Consigne tout, y compris les données transportées entre le hub et le client.

Utilisez le `configureLogging` méthode sur `HubConnectionBuilder` pour configurer le niveau de journalisation. Les messages sont enregistrés dans la console du navigateur.

[!code-javascript[Logging levels](javascript-client/sample/wwwroot/js/chat.js?range=9-12)]

## <a name="related-resources"></a>Ressources connexes

* [Hubs](xref:signalr/hubs)
* [Client .NET](xref:signalr/dotnet-client)
* [Publier sur Azure](xref:signalr/publish-to-azure-web-app)
* [Autoriser les demandes de Cross-Origin (CORS) dans ASP.NET Core](xref:security/cors)
