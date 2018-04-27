---
title: ASP.NET Core SignalR JavaScript client
author: rachelappel
description: Vue d’ensemble de client ASP.NET Core SignalR JavaScript.
manager: wpickett
monikerRange: '>= aspnetcore-2.1'
ms.author: rachelap
ms.custom: mvc
ms.date: 04/06/2018
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: signalr/javascript-client
ms.openlocfilehash: cca1a523bd536f4365e54c196f6c9024779d5d76
ms.sourcegitcommit: c79fd3592f444d58e17518914f8873d0a11219c0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/18/2018
---
# <a name="aspnet-core-signalr-javascript-client"></a>ASP.NET Core SignalR JavaScript client

Par [Rachel Appel](http://twitter.com/rachelappel)

[!INCLUDE [2.1 preview notice](~/includes/2.1.md)]

La bibliothèque cliente ASP.NET Core SignalR JavaScript permet aux développeurs d’appeler du code de concentrateur côté serveur.

[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/Docs/tree/live/aspnetcore/signalr/javascript-client/sample) ([procédure de téléchargement](xref:tutorials/index#how-to-download-a-sample))

## <a name="install-the-signalr-client-package"></a>Installez le package de client SignalR

La bibliothèque cliente SignalR JavaScript est remise sous une [npm](https://www.npmjs.com/) package. Si vous utilisez Visual Studio, exécutez `npm install` à partir de la **Package Manager Console** tandis que dans le dossier racine. Pour le Code de Visual Studio, exécutez la commande depuis le **Terminal Server intégré**.

  ```console
   npm init -y
   npm install @aspnet/signalr
  ```

Npm installe le contenu du package dans le *node_modules\\ @aspnet\signalr\dist\browser*  dossier. Créez un dossier nommé *signalr* sous le *wwwroot\\lib* dossier. Copie le *signalr.js* de fichiers à la *wwwroot\lib\signalr* dossier.

## <a name="use-the-signalr-javascript-client"></a>Utiliser le client SignalR JavaScript

Référencer le client SignalR JavaScript dans le `<script>` élément.

```html
<script src="~/lib/signalr/signalr.js"></script>
```

## <a name="connect-to-a-hub"></a>Se connecter à un concentrateur

Le code suivant crée et lance une connexion. Nom de concentrateur respecte la casse.

[!code-javascript[Call hub methods](javascript-client/sample/wwwroot/js/chat.js?range=1-2,18)]

### <a name="cross-origin-connections"></a>Connexions de cross-origine

En règle générale, navigateurs chargent les connexions à partir du même domaine que la page demandée. Toutefois, il existe des occasions lorsqu’une connexion à un autre domaine est requise.

Pour empêcher la lecture des données sensibles à partir d’un autre site, un site malveillant [cross-origine connexions](xref:security/cors) sont désactivées par défaut. Pour autoriser une demande cross-origin, activez-le dans la `Startup` classe.

[!code-csharp[Cross-origin connections](javascript-client/sample/Startup.cs?highlight=29-34,55)]

## <a name="call-hub-methods-from-client"></a>Appeler des méthodes de concentrateur du client

Les clients JavaScript appellent les méthodes publiques sur les concentrateurs par à l’aide de `connection.invoke`. Le `invoke` méthode accepte deux arguments :

* Le nom de la méthode de concentrateur. Dans l’exemple suivant, le nom du concentrateur est `SendMessage`.
* Tous les arguments définis dans la méthode de concentrateur. Dans l’exemple suivant, le nom de l’argument est `message`.

[!code-javascript[Call hub methods](javascript-client/sample/wwwroot/js/chat.js?range=14)]

## <a name="call-client-methods-from-hub"></a>Appeler des méthodes de client à partir du hub

Pour recevoir des messages à partir du concentrateur, définissez une méthode à l’aide de la `connection.on` (méthode).

* Le nom de la méthode du client JavaScript. Dans l’exemple suivant, le nom de la méthode est `ReceiveMessage`.
* Arguments que concentrateur passe à la méthode. Dans l’exemple suivant, la valeur de l’argument est `message`.

[!code-javascript[Receive calls from hub](javascript-client/sample/wwwroot/js/chat.js?range=4-9)]

Le code précédent dans `connection.on` s’exécute lorsque le code côté serveur appelle à l’aide de la `SendAsync` (méthode).

[!code-javascript[Call client-side](javascript-client/sample/hubs/chathub.cs?range=8-11)]

SignalR détermine la méthode de client à appeler en faisant correspondre le nom de méthode et les arguments définis dans `SendAsync` et `connection.on`.

> [!NOTE]
> Comme meilleure pratique, appelez `connection.start` après `connection.on` afin des gestionnaires inscrits avant la réception des messages.

## <a name="error-handling-and-logging"></a>Journalisation et gestion des erreurs

Chaîne d’un `catch` méthode à la fin de la `connection.start` méthode pour gérer les erreurs du côté client. Utilisez `console.error` aux erreurs de sortie à la console du navigateur.

[!code-javascript[Error handling](javascript-client/sample/wwwroot/js/chat.js?range=18)]

Configurer le suivi de journal côté client en passant un enregistreur d’événements et le type d’événement pour se connecter lors de la connexion est établie. Les messages sont enregistrés et le niveau de journal spécifié. Niveaux de journalisation disponibles sont les suivantes :

* `signalR.LogLevel.Error` : Messages d’erreur. Journaux `Error` messages uniquement.
* `signalR.LogLevel.Warning` : Messages d’avertissement sur les erreurs potentielles. Journaux `Warning`, et `Error` messages.
* `signalR.LogLevel.Information` : Messages d’état sans erreurs. Journaux `Information`, `Warning`, et `Error` messages.
* `signalR.LogLevel.Trace` : Messages de trace. Enregistre tous les éléments, y compris les données transportées entre le client et du concentrateur.

Passez l’enregistreur d’événements à la connexion pour commencer à enregistrer. En général, les outils de développement de navigateur contiennent une console qui affiche les messages.

[!code-javascript[Logging levels](javascript-client/sample/wwwroot/js/chat.js?range=1-2)]

## <a name="related-resources"></a>Ressources connexes

* [Concentrateurs SignalR ASP.NET Core](xref:signalr/hubs)
* [Activer les demandes Cross-Origin (CORS) dans ASP.NET Core](xref:security/cors)