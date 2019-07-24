---
title: Client JavaScript ASP.NET Core SignalR
author: bradygaster
description: Vue d’ensemble du client JavaScript ASP.NET Core SignalR.
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 06/28/2019
uid: signalr/javascript-client
ms.openlocfilehash: f314e1fe0ef0ea73a28b332404a09f2956524132
ms.sourcegitcommit: f30b18442ed12831c7e86b0db249183ccd749f59
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/23/2019
ms.locfileid: "68412383"
---
# <a name="aspnet-core-signalr-javascript-client"></a>Client JavaScript ASP.NET Core SignalR

Par [Rachel Appel](https://twitter.com/rachelappel)

La bibliothèque cliente JavaScript ASP.NET Core SignalR permet aux développeurs d’appeler un hub à partir de code côté serveur.

[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/AspNetCore.Docs/tree/live/aspnetcore/signalr/javascript-client/sample) ([procédure de téléchargement](xref:index#how-to-download-a-sample))

## <a name="install-the-signalr-client-package"></a>Installer le package du client Signalr

La bibliothèque cliente JavaScript Signalr est fournie sous la forme d’un package [NPM](https://www.npmjs.com/) . Si vous utilisez Visual Studio, exécutez `npm install` à partir de la console du gestionnaire de **package** dans le dossier racine. Pour Visual Studio Code, exécutez la commande à partir du **Terminal intégré**.

::: moniker range=">= aspnetcore-3.0"

  ```console
  npm init -y
  npm install @microsoft/signalr
  ```

npm installe le contenu du package dans le dossier *node_modules\\@microsoft\signalr\dist\browser* . Créez un dossier nommé *signalr* sous le dossier *wwwroot\\lib*. Copiez le fichier *signalr.js* dans le dossier *wwwroot\lib\signalr*.

::: moniker-end

::: moniker range="< aspnetcore-3.0"

  ```console
  npm init -y
  npm install @aspnet/signalr
  ```

npm installe le contenu du package dans le dossier *node_modules\\@aspnet\signalr\dist\browser* . Créez un dossier nommé *signalr* sous le dossier *wwwroot\\lib*. Copiez le fichier *signalr.js* dans le dossier *wwwroot\lib\signalr*.

::: moniker-end

## <a name="use-the-signalr-javascript-client"></a>Utiliser le client JavaScript SignalR

Référencez le client JavaScript SignalR dans l'élément `<script>`.

```html
<script src="~/lib/signalr/signalr.js"></script>
```

## <a name="connect-to-a-hub"></a>Se connecter à un hub

Le code suivant crée et démarre une connexion. Le nom du concentrateur ne respecte pas la casse.

[!code-javascript[Call hub methods](javascript-client/sample/wwwroot/js/chat.js?range=9-13,43-45)]

### <a name="cross-origin-connections"></a>Connexions cross-origin

En règle générale, les navigateurs chargent les connexions à partir du même domaine que la page demandée. Toutefois, il existe des cas où une connexion à un autre domaine est nécessaire.

Pour empêcher un site malveillant de lire des données sensibles à partir d’un autre site, les [connexions Cross-Origin](xref:security/cors) sont désactivées par défaut. Pour autoriser une demande Cross-Origin, activez-la dans `Startup` la classe.

[!code-csharp[Cross-origin connections](javascript-client/sample/Startup.cs?highlight=29-35,56)]

## <a name="call-hub-methods-from-client"></a>Appeler des méthodes de hub à partir du client

Les clients JavaScript appellent les méthodes publiques sur les hubs via la méthode [invoke](/javascript/api/%40aspnet/signalr/hubconnection#invoke) de la [HubConnection](/javascript/api/%40aspnet/signalr/hubconnection). La méthode `invoke` accepte deux arguments :

* Le nom de la méthode de hub. Dans l’exemple suivant, le nom de méthode sur le hub est `SendMessage`.
* Tous les arguments définis dans la méthode de hub. Dans l’exemple suivant, le nom de l' `message`argument est. L’exemple de code utilise la syntaxe de fonction Arrow qui est prise en charge dans les versions actuelles de tous les principaux navigateurs, à l’exception d’Internet Explorer.

  [!code-javascript[Call hub methods](javascript-client/sample/wwwroot/js/chat.js?range=24)]

> [!NOTE]
> Si vous utilisez le service Signalr Azure en *mode sans serveur*, vous ne pouvez pas appeler les méthodes de concentrateur à partir d’un client. Pour plus d’informations, consultez la [documentation du service signalr](/azure/azure-signalr/signalr-concept-serverless-development-config).

La `invoke` méthode retourne une [promesse](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Promise)JavaScript. Le `Promise` est résolu avec la valeur de retour (le cas échéant) lorsque la méthode sur le serveur retourne. Si la méthode sur le serveur génère une erreur, `Promise` est rejetée avec le message d’erreur. Utilisez les `then` méthodes `catch` et sur le `Promise` lui-même pour gérer ces cas `await` (syntaxe).

La `send` méthode retourne un JavaScript `Promise`. Le `Promise` est résolu lorsque le message a été envoyé au serveur. En cas d’erreur lors de l’envoi du message `Promise` , est rejeté avec le message d’erreur. Utilisez les `then` méthodes `catch` et sur le `Promise` lui-même pour gérer ces cas `await` (syntaxe).

> [!NOTE]
> L' `send` utilisation de n’attend pas que le serveur ait reçu le message. Par conséquent, il n’est pas possible de retourner des données ou des erreurs à partir du serveur.

## <a name="call-client-methods-from-hub"></a>Appeler des méthodes clientes à partir du Hub

Pour recevoir des messages à partir du hub, définissez une méthode à l’aide de la méthode [on](/javascript/api/%40aspnet/signalr/hubconnection#on) de la `HubConnection`.

* Nom de la méthode du client JavaScript. Dans l’exemple suivant, le nom de la `ReceiveMessage`méthode est.
* Les arguments que le hub passe à la méthode. Dans l’exemple suivant, la valeur de l' `message`argument est.

[!code-javascript[Receive calls from hub](javascript-client/sample/wwwroot/js/chat.js?range=14-19)]

Le code précédent dans `connection.on` s’exécute lorsque le code côté serveur l’appelle à l’aide de la méthode [SendAsync](/dotnet/api/microsoft.aspnetcore.signalr.clientproxyextensions.sendasync) .

[!code-csharp[Call client-side](javascript-client/sample/hubs/chathub.cs?range=8-11)]

SignalR détermine la méthode de client à appeler en faisant correspondre le nom de méthode et les arguments définis dans `SendAsync` et `connection.on`.

> [!NOTE]
> Comme meilleure pratique, appelez la méthode [start](/javascript/api/%40aspnet/signalr/hubconnection#start) sur le `HubConnection` après `on`. En procédant comme ceci, vos gestionnaires sont enregistrés avant que tous les messages soient reçus.

## <a name="error-handling-and-logging"></a>Gestion et journalisation des erreurs

Chaînez une méthode `catch` à la fin de la méthode `start` pour gérer les erreurs côté client. Utilisez `console.error` pour envoyer les erreurs vers la console du navigateur.

[!code-javascript[Error handling](javascript-client/sample/wwwroot/js/chat.js?range=49-51)]

Configurez le suivi des journaux côté client en passant un enregistreur d’événements et un type d’événement à consigner lorsque la connexion est établie. Les messages sont enregistrés avec le niveau de journalisation spécifié et supérieur. Les niveaux de journalisation disponibles sont les suivants:

* `signalR.LogLevel.Error`&ndash; Messages d’erreur. Journalise `Error` uniquement les messages.
* `signalR.LogLevel.Warning`&ndash; Messages d’avertissement relatifs aux erreurs potentielles. Journaux `Warning` et`Error` messages.
* `signalR.LogLevel.Information`&ndash; Messages d’État sans erreurs. `Information`Journalise `Warning`les messages `Error` , et.
* `signalR.LogLevel.Trace`&ndash; Messages de trace. Journalise tout, y compris les données transférées entre le Hub et le client.

Utilisez la méthode [configureLogging](/javascript/api/%40aspnet/signalr/hubconnectionbuilder#configurelogging) sur [HubConnectionBuilder](/javascript/api/%40aspnet/signalr/hubconnectionbuilder) pour configurer le niveau de journalisation. Les messages sont enregistrés dans la console du navigateur.

[!code-javascript[Logging levels](javascript-client/sample/wwwroot/js/chat.js?range=9-12)]

## <a name="reconnect-clients"></a>Reconnecter les clients

::: moniker range=">= aspnetcore-3.0"

### <a name="automatically-reconnect"></a>Se reconnecter automatiquement

Le client JavaScript pour signalr peut être configuré pour se reconnecter automatiquement à `withAutomaticReconnect` l’aide de la méthode sur [HubConnectionBuilder](/javascript/api/%40aspnet/signalr/hubconnectionbuilder). Par défaut, il ne se reconnectera pas automatiquement.

```javascript
const connection = new signalR.HubConnectionBuilder()
    .withUrl("/chatHub")
    .withAutomaticReconnect()
    .build();
```

Sans aucun paramètre, `withAutomaticReconnect()` configure le client pour attendre 0, 2, 10 et 30 secondes, respectivement, avant d’essayer chaque tentative de reconnexion, en s’arrêtant après quatre tentatives ayant échoué.

Avant de commencer les tentatives de reconnexion `HubConnection` , le passe à `HubConnectionState.Reconnecting` l’État et déclenche `onreconnecting` ses rappels au lieu de passer à `Disconnected` l’État et de déclencher `onclose` ses rappels comme un `HubConnection`sans la reconnexion automatique configurée. Cela permet d’avertir les utilisateurs que la connexion a été perdue et de désactiver les éléments d’interface utilisateur.

```javascript
connection.onreconnecting((error) => {
    console.assert(connection.state === signalR.HubConnectionState.Reconnecting);

    document.getElementById("messageInput").disabled = true;

    const li = document.createElement("li");
    li.textContent = `Connection lost due to error "${error}". Reconnecting.`;
    document.getElementById("messagesList").appendChild(li);
});
```

Si le client se reconnecte correctement dans les quatre premières tentatives, `HubConnection` le passe à l' `Connected` État et déclenche ses `onreconnected` rappels. Cela permet d’informer les utilisateurs que la connexion a été rétablie.

Étant donné que la connexion semble entièrement nouvelle sur le serveur, `connectionId` un nouveau sera fourni `onreconnected` au rappel.

> [!WARNING]
> Le `onreconnected` paramètre du `connectionId` rappel n’est pas défini si le a `HubConnection` été configuré pour [ignorer la négociation](xref:signalr/configuration#configure-client-options).

```javascript
connection.onreconnected((connectionId) => {
    console.assert(connection.state === signalR.HubConnectionState.Connected);

    document.getElementById("messageInput").disabled = false;

    const li = document.createElement("li");
    li.textContent = `Connection reestablished. Connected with connectionId "${connectionId}".`;
    document.getElementById("messagesList").appendChild(li);
});
```

`withAutomaticReconnect()`ne configure `HubConnection` pas le pour réessayer les échecs de démarrage initial. les échecs de démarrage doivent donc être gérés manuellement:

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

Si le client ne se reconnecte pas correctement dans les quatre premières `HubConnection` tentatives, le passe `Disconnected` à l’État et déclenche ses rappels [OnClose](/javascript/api/%40aspnet/signalr/hubconnection#onclose) . Cela permet d’informer les utilisateurs que la connexion a été perdue définitivement et de recommander l’actualisation de la page:

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

L’exemple précédent configure le pour `HubConnection` qu’il commence à tenter de se reconnecter immédiatement après la perte de la connexion. Cela est également vrai pour la configuration par défaut.

Si la première tentative de reconnexion échoue, la deuxième tentative de reconnexion démarre également immédiatement au lieu d’attendre 2 secondes comme dans la configuration par défaut.

Si la deuxième tentative de reconnexion échoue, la troisième tentative de reconnexion démarrera dans 10 secondes, ce qui revient à la configuration par défaut.

Le comportement personnalisé divergent ensuite du comportement par défaut en s’arrêtant après le troisième échec de tentative de reconnexion au lieu d’essayer une nouvelle tentative de reconnexion dans 30 secondes, comme dans la configuration par défaut.

Si vous souhaitez encore plus de contrôle sur le minutage et le nombre de tentatives de reconnexion automatique, `withAutomaticReconnect` accepte un objet qui implémente l' `IRetryPolicy` interface, qui `nextRetryDelayInMilliseconds`a une méthode unique nommée.

`nextRetryDelayInMilliseconds`accepte un seul argument avec le type `RetryContext`. `previousRetryCount` `number` Atrois`number` propriétés: `elapsedMilliseconds` , et`retryReason`qui sont respectivementun,unetun.`Error` `RetryContext` Avant la première tentative de reconnexion, `previousRetryCount` et `elapsedMilliseconds` est égal à zéro `retryReason` et est l’erreur qui a provoqué la perte de la connexion. Après chaque nouvelle tentative d’échec `previousRetryCount` , est incrémenté d’une unité `elapsedMilliseconds` , est mis à jour pour refléter le temps passé à se reconnecter jusqu’à présent en `retryReason` millisecondes, et est l’erreur qui a provoqué la dernière tentative de reconnexion à incident.

`nextRetryDelayInMilliseconds`doit retourner un nombre représentant le nombre de millisecondes à attendre avant la tentative de reconnexion suivante ou `null` si doit arrêter la `HubConnection` reconnexion.

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
> Avant le 3,0, le client JavaScript pour Signalr ne se reconnecte pas automatiquement. Vous devez écrire du code qui reconnectera votre client manuellement.

::: moniker-end

Le code suivant illustre une approche de reconnexion manuelle classique:

1. Une fonction (dans ce cas, la `start` fonction) est créée pour démarrer la connexion.
1. Appelez la `start` fonction dans le gestionnaire d' `onclose` événements de la connexion.

[!code-javascript[Reconnect the JavaScript client](javascript-client/sample/wwwroot/js/chat.js?range=28-40)]

Une implémentation réelle utilise une interruption exponentielle ou une nouvelle tentative un nombre spécifié de fois avant d’abandonner.

## <a name="additional-resources"></a>Ressources supplémentaires

* [Référence API JavaScript](/javascript/api/?view=signalr-js-latest)
* [Didacticiel JavaScript](xref:tutorials/signalr)
* [Didacticiel WebPack et machine à écrire](xref:tutorials/signalr-typescript-webpack)
* [Hubs](xref:signalr/hubs)
* [Client .NET](xref:signalr/dotnet-client)
* [Publier sur Azure](xref:signalr/publish-to-azure-web-app)
* [Requêtes Cross-Origin (CORS)](xref:security/cors)
* [Documentation sans serveur du service Azure Signalr](/azure/azure-signalr/signalr-concept-serverless-development-config)
