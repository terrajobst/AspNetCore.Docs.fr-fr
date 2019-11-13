---
title: ASP.NET Core SignalR client JavaScript
author: bradygaster
description: Vue d’ensemble de ASP.NET Core SignalR client JavaScript.
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 11/12/2019
no-loc:
- SignalR
uid: signalr/javascript-client
ms.openlocfilehash: 926160a41c82853d83890f0d52b14d7d5561a990
ms.sourcegitcommit: 3fc3020961e1289ee5bf5f3c365ce8304d8ebf19
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/12/2019
ms.locfileid: "73963768"
---
# <a name="aspnet-core-opno-locsignalr-javascript-client"></a>ASP.NET Core SignalR client JavaScript

Par [Rachel Appel](https://twitter.com/rachelappel)

La bibliothèque cliente ASP.NET Core SignalR JavaScript permet aux développeurs d’appeler du code de concentrateur côté serveur.

[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/AspNetCore.Docs/tree/live/aspnetcore/signalr/javascript-client/sample) ([procédure de téléchargement](xref:index#how-to-download-a-sample))

## <a name="install-the-opno-locsignalr-client-package"></a>Installer le package client SignalR

La bibliothèque cliente JavaScript SignalR est fournie sous la forme d’un package [NPM](https://www.npmjs.com/) . Si vous utilisez Visual Studio, exécutez `npm install` à partir de la **console du gestionnaire de package** dans le dossier racine. Pour Visual Studio Code, exécutez la commande à partir du **Terminal intégré**.

::: moniker range=">= aspnetcore-3.0"

  ```console
  npm init -y
  npm install @microsoft/signalr
  ```

NPM installe le contenu du package dans le dossier *node_modules\\@microsoft\signalr\dist\browser* . Créez un nouveau dossier nommé *signalr* sous le dossier *wwwroot\\lib* . Copiez le fichier *signalr. js* dans le dossier *wwwroot\lib\signalr* .

::: moniker-end

::: moniker range="< aspnetcore-3.0"

  ```console
  npm init -y
  npm install @aspnet/signalr
  ```

NPM installe le contenu du package dans le dossier *node_modules\\@aspnet\signalr\dist\browser* . Créez un nouveau dossier nommé *signalr* sous le dossier *wwwroot\\lib* . Copiez le fichier *signalr. js* dans le dossier *wwwroot\lib\signalr* .

::: moniker-end

## <a name="use-the-opno-locsignalr-javascript-client"></a>Utiliser le client JavaScript SignalR

Référencez le client JavaScript SignalR dans l’élément `<script>`.

```html
<script src="~/lib/signalr/signalr.js"></script>
```

## <a name="connect-to-a-hub"></a>Se connecter à un concentrateur

Le code suivant crée et démarre une connexion. Le nom du concentrateur ne respecte pas la casse.

[!code-javascript[Call hub methods](javascript-client/sample/wwwroot/js/chat.js?range=9-13,43-45)]

### <a name="cross-origin-connections"></a>Connexions Cross-Origin

En règle générale, les navigateurs chargent les connexions à partir du même domaine que la page demandée. Toutefois, il peut arriver qu’une connexion à un autre domaine soit nécessaire.

Pour empêcher un site malveillant de lire des données sensibles à partir d’un autre site, les [connexions Cross-Origin](xref:security/cors) sont désactivées par défaut. Pour autoriser une demande Cross-Origin, activez-la dans la classe `Startup`.

[!code-csharp[Cross-origin connections](javascript-client/sample/Startup.cs?highlight=29-35,56)]

## <a name="call-hub-methods-from-client"></a>Appeler les méthodes de concentrateur à partir du client

Les clients JavaScript appellent des méthodes publiques sur des hubs à l’aide de la méthode [Invoke](/javascript/api/%40aspnet/signalr/hubconnection#invoke) de [HubConnection](/javascript/api/%40aspnet/signalr/hubconnection). La méthode `invoke` accepte deux arguments :

* Nom de la méthode de concentrateur. Dans l’exemple suivant, le nom de la méthode sur le Hub est `SendMessage`.
* Tous les arguments définis dans la méthode de concentrateur. Dans l’exemple suivant, le nom de l’argument est `message`. L’exemple de code utilise la syntaxe de fonction Arrow qui est prise en charge dans les versions actuelles de tous les principaux navigateurs, à l’exception d’Internet Explorer.

  [!code-javascript[Call hub methods](javascript-client/sample/wwwroot/js/chat.js?range=24)]

> [!NOTE]
> Si vous utilisez le service Azure SignalR en *mode sans serveur*, vous ne pouvez pas appeler les méthodes de concentrateur à partir d’un client. Pour plus d’informations, consultez la [documentation du ServiceSignalR](/azure/azure-signalr/signalr-concept-serverless-development-config).

La méthode `invoke` retourne une [promesse](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Promise)JavaScript. La `Promise` est résolue avec la valeur de retour (le cas échéant) lorsque la méthode sur le serveur retourne. Si la méthode sur le serveur génère une erreur, le `Promise` est rejeté avec le message d’erreur. Utilisez les méthodes `then` et `catch` sur le `Promise` lui-même pour gérer ces cas (ou `await` syntaxe).

La méthode `send` retourne un `Promise`JavaScript. La `Promise` est résolue lorsque le message a été envoyé au serveur. En cas d’erreur lors de l’envoi du message, le `Promise` est rejeté avec le message d’erreur. Utilisez les méthodes `then` et `catch` sur le `Promise` lui-même pour gérer ces cas (ou `await` syntaxe).

> [!NOTE]
> L’utilisation de `send` n’attend pas que le serveur ait reçu le message. Par conséquent, il n’est pas possible de retourner des données ou des erreurs à partir du serveur.

## <a name="call-client-methods-from-hub"></a>Appeler des méthodes clientes à partir du Hub

Pour recevoir des messages à partir du concentrateur, définissez une méthode à l’aide de la méthode [on](/javascript/api/%40aspnet/signalr/hubconnection#on) de l' `HubConnection`.

* Nom de la méthode du client JavaScript. Dans l’exemple suivant, le nom de la méthode est `ReceiveMessage`.
* Arguments que le Hub transmet à la méthode. Dans l’exemple suivant, la valeur de l’argument est `message`.

[!code-javascript[Receive calls from hub](javascript-client/sample/wwwroot/js/chat.js?range=14-19)]

Le code précédent dans `connection.on` s’exécute lorsque le code côté serveur l’appelle à l’aide de la méthode [SendAsync](/dotnet/api/microsoft.aspnetcore.signalr.clientproxyextensions.sendasync) .

[!code-csharp[Call client-side](javascript-client/sample/hubs/chathub.cs?range=8-11)]

SignalR détermine la méthode cliente à appeler en faisant correspondre le nom de la méthode et les arguments définis dans `SendAsync` et `connection.on`.

> [!NOTE]
> Il est recommandé d’appeler la méthode [Start](/javascript/api/%40aspnet/signalr/hubconnection#start) sur le `HubConnection` après `on`. Cela permet de s’assurer que vos gestionnaires sont inscrits avant la réception de tous les messages.

## <a name="error-handling-and-logging"></a>Gestion et journalisation des erreurs

Chaînez une méthode `catch` à la fin de la méthode `start` pour gérer les erreurs côté client. Utilisez `console.error` pour générer des erreurs dans la console du navigateur.

[!code-javascript[Error handling](javascript-client/sample/wwwroot/js/chat.js?range=49-51)]

Configurez le suivi des journaux côté client en passant un enregistreur d’événements et un type d’événement à consigner lorsque la connexion est établie. Les messages sont enregistrés avec le niveau de journalisation spécifié et supérieur. Les niveaux de journalisation disponibles sont les suivants :

* `signalR.LogLevel.Error` &ndash; des messages d’erreur. Journalise uniquement les messages `Error`.
* `signalR.LogLevel.Warning` &ndash; des messages d’avertissement sur les erreurs potentielles. Journalise les messages `Warning`et `Error`.
* `signalR.LogLevel.Information` &ndash; messages d’État sans erreurs. Journalise les messages `Information`, `Warning`et `Error`.
* `signalR.LogLevel.Trace` &ndash; des messages de suivi. Journalise tout, y compris les données transférées entre le Hub et le client.

Utilisez la méthode [configureLogging](/javascript/api/%40aspnet/signalr/hubconnectionbuilder#configurelogging) sur [HubConnectionBuilder](/javascript/api/%40aspnet/signalr/hubconnectionbuilder) pour configurer le niveau de journalisation. Les messages sont enregistrés dans la console du navigateur.

[!code-javascript[Logging levels](javascript-client/sample/wwwroot/js/chat.js?range=9-12)]

## <a name="reconnect-clients"></a>Reconnecter les clients

::: moniker range=">= aspnetcore-3.0"

### <a name="automatically-reconnect"></a>Se reconnecter automatiquement

Le client JavaScript pour SignalR peut être configuré pour se reconnecter automatiquement à l’aide de la méthode `withAutomaticReconnect` sur [HubConnectionBuilder](/javascript/api/%40aspnet/signalr/hubconnectionbuilder). Par défaut, il ne se reconnectera pas automatiquement.

```javascript
const connection = new signalR.HubConnectionBuilder()
    .withUrl("/chatHub")
    .withAutomaticReconnect()
    .build();
```

Sans aucun paramètre, `withAutomaticReconnect()` configure le client pour attendre 0, 2, 10 et 30 secondes, respectivement, avant d’essayer chaque tentative de reconnexion, en s’arrêtant après quatre tentatives ayant échoué.

Avant de commencer les tentatives de reconnexion, le `HubConnection` passe à l’État `HubConnectionState.Reconnecting` et déclenche ses rappels `onreconnecting` au lieu de passer à l’État `Disconnected` et de déclencher ses rappels de `onclose` comme un `HubConnection` sans que la reconnexion automatique soit configurée. Cela permet d’avertir les utilisateurs que la connexion a été perdue et de désactiver les éléments d’interface utilisateur.

```javascript
connection.onreconnecting((error) => {
    console.assert(connection.state === signalR.HubConnectionState.Reconnecting);

    document.getElementById("messageInput").disabled = true;

    const li = document.createElement("li");
    li.textContent = `Connection lost due to error "${error}". Reconnecting.`;
    document.getElementById("messagesList").appendChild(li);
});
```

Si le client se reconnecte avec succès dans les quatre premières tentatives, le `HubConnection` passe à l’État `Connected` et active ses rappels `onreconnected`. Cela permet d’informer les utilisateurs que la connexion a été rétablie.

Étant donné que la connexion semble entièrement nouvelle sur le serveur, une nouvelle `connectionId` sera fournie au rappel `onreconnected`.

> [!WARNING]
> Le paramètre de `connectionId` du rappel `onreconnected` n’est pas défini si le `HubConnection` a été configuré pour [ignorer la négociation](xref:signalr/configuration#configure-client-options).

```javascript
connection.onreconnected((connectionId) => {
    console.assert(connection.state === signalR.HubConnectionState.Connected);

    document.getElementById("messageInput").disabled = false;

    const li = document.createElement("li");
    li.textContent = `Connection reestablished. Connected with connectionId "${connectionId}".`;
    document.getElementById("messagesList").appendChild(li);
});
```

`withAutomaticReconnect()` ne configure pas le `HubConnection` pour réessayer les échecs de démarrage initial, les échecs de démarrage doivent donc être gérés manuellement :

```javascript
async function start() {
    try {
        await connection.start();
        console.assert(connection.state === signalR.HubConnectionState.Connected);
        console.log("connected");
    } catch (err) {
        console.assert(connection.state === signalR.HubConnectionState.Disconnected);
        console.log(err);
        setTimeout(() => start(), 5000);
    }
};
```

Si le client ne se reconnecte pas correctement dans les quatre premières tentatives, le `HubConnection` passera à l’État `Disconnected` et déclenchera ses rappels [OnClose](/javascript/api/%40aspnet/signalr/hubconnection#onclose) . Cela permet d’informer les utilisateurs que la connexion a été perdue définitivement et de recommander l’actualisation de la page :

```javascript
connection.onclose((error) => {
    console.assert(connection.state === signalR.HubConnectionState.Disconnected);

    document.getElementById("messageInput").disabled = true;

    const li = document.createElement("li");
    li.textContent = `Connection closed due to error "${error}". Try refreshing this page to restart the connection.`;
    document.getElementById("messagesList").appendChild(li);
});
```

Pour configurer un nombre personnalisé de tentatives de reconnexion avant de vous déconnecter ou de modifier le délai de reconnexion, `withAutomaticReconnect` accepte un tableau de nombres représentant le délai en millisecondes à attendre avant de démarrer chaque tentative de reconnexion.

```javascript
const connection = new signalR.HubConnectionBuilder()
    .withUrl("/chatHub")
    .withAutomaticReconnect([0, 0, 10000])
    .build();

    // .withAutomaticReconnect([0, 2000, 10000, 30000]) yields the default behavior
```

L’exemple précédent configure le `HubConnection` pour commencer à tenter de se reconnecter immédiatement après la perte de la connexion. Cela est également vrai pour la configuration par défaut.

Si la première tentative de reconnexion échoue, la deuxième tentative de reconnexion démarre également immédiatement au lieu d’attendre 2 secondes comme dans la configuration par défaut.

Si la deuxième tentative de reconnexion échoue, la troisième tentative de reconnexion démarrera dans 10 secondes, ce qui revient à la configuration par défaut.

Le comportement personnalisé divergent ensuite du comportement par défaut en s’arrêtant après le troisième échec de tentative de reconnexion au lieu d’essayer une nouvelle tentative de reconnexion dans 30 secondes, comme dans la configuration par défaut.

Si vous souhaitez encore plus de contrôle sur le minutage et le nombre de tentatives de reconnexion automatique, `withAutomaticReconnect` accepte un objet qui implémente l’interface `IRetryPolicy`, qui a une méthode unique nommée `nextRetryDelayInMilliseconds`.

`nextRetryDelayInMilliseconds` accepte un seul argument avec le type `RetryContext`. La `RetryContext` a trois propriétés : `previousRetryCount`, `elapsedMilliseconds` et `retryReason` qui sont une `number`, une `number` et une `Error` respectivement. Avant la première tentative de reconnexion, les `previousRetryCount` et `elapsedMilliseconds` sont nuls, et le `retryReason` est l’erreur qui a provoqué la perte de la connexion. Après chaque nouvelle tentative d’échec, `previousRetryCount` sera incrémenté d’une unité, `elapsedMilliseconds` sera mis à jour pour refléter le temps passé à se reconnecter jusqu’à présent en millisecondes, et la `retryReason` sera l’erreur qui a provoqué l’échec de la dernière tentative de reconnexion.

`nextRetryDelayInMilliseconds` doit retourner un nombre représentant le nombre de millisecondes à attendre avant la tentative de reconnexion suivante ou `null` si le `HubConnection` doit s’arrêter de se reconnecter.

```javascript
const connection = new signalR.HubConnectionBuilder()
    .withUrl("/chatHub")
    .withAutomaticReconnect({
        nextRetryDelayInMilliseconds: retryContext => {
          if (retryContext.elapsedMilliseconds < 60000) {
            // If we've been reconnecting for less than 60 seconds so far,
            // wait between 0 and 10 seconds before the next reconnect attempt.
            return Math.random() * 10000;
          } else {
            // If we've been reconnecting for more than 60 seconds so far, stop reconnecting.
            return null;
          }
        })
    .build();
```

Vous pouvez également écrire du code qui reconnectera votre client manuellement, comme illustré dans [reconnexion manuelle](#manually-reconnect).

::: moniker-end

### <a name="manually-reconnect"></a>Reconnexion manuelle

::: moniker range="< aspnetcore-3.0"

> [!WARNING]
> Avant le 3,0, le client JavaScript pour SignalR ne se reconnecte pas automatiquement. Vous devez écrire du code qui reconnectera votre client manuellement.

::: moniker-end

Le code suivant illustre une approche de reconnexion manuelle classique :

1. Une fonction (dans ce cas, la fonction `start`) est créée pour démarrer la connexion.
1. Appelez la fonction `start` dans le gestionnaire d’événements `onclose` de la connexion.

[!code-javascript[Reconnect the JavaScript client](javascript-client/sample/wwwroot/js/chat.js?range=28-40)]

Une implémentation réelle utilise une interruption exponentielle ou une nouvelle tentative un nombre spécifié de fois avant d’abandonner.

## <a name="additional-resources"></a>Ressources supplémentaires

* [Informations de référence sur l’API JavaScript](/javascript/api/?view=signalr-js-latest)
* [Didacticiel JavaScript](xref:tutorials/signalr)
* [Didacticiel WebPack et machine à écrire](xref:tutorials/signalr-typescript-webpack)
* [Hubs](xref:signalr/hubs)
* [Client .NET](xref:signalr/dotnet-client)
* [Publier sur Azure](xref:signalr/publish-to-azure-web-app)
* [Requêtes Cross-Origin (CORS)](xref:security/cors)
* [Documentation sans serveur d’Azure SignalR service](/azure/azure-signalr/signalr-concept-serverless-development-config)
