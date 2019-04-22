---
title: Client JavaScript ASP.NET Core SignalR
author: bradygaster
description: Vue d’ensemble du client JavaScript ASP.NET Core SignalR.
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 04/17/2019
uid: signalr/javascript-client
ms.openlocfilehash: e58015221497a9f962edf9f9fdba7ea3025d7694
ms.sourcegitcommit: 78339e9891c8676db01a6e81e9cb0cdaa280162f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59705602"
---
# <a name="aspnet-core-signalr-javascript-client"></a>Client JavaScript ASP.NET Core SignalR

Par [Rachel Appel](http://twitter.com/rachelappel)

La bibliothèque cliente JavaScript ASP.NET Core SignalR permet aux développeurs d’appeler un hub à partir de code côté serveur.

[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/Docs/tree/live/aspnetcore/signalr/javascript-client/sample) ([procédure de téléchargement](xref:index#how-to-download-a-sample))

## <a name="install-the-signalr-client-package"></a>Installer le package du client SignalR

La bibliothèque de client SignalR JavaScript est remise en tant qu’un [npm](https://www.npmjs.com/) package. Si vous utilisez Visual Studio, exécutez `npm install` à partir de la **Console du Gestionnaire de Package** lorsque vous êtes dans le dossier racine. Pour Visual Studio Code, exécutez la commande depuis le **Terminal intégré**.

  ```console
  npm init -y
  npm install @aspnet/signalr
  ```

npm installe le contenu du package dans le dossier *node_modules\\@aspnet\signalr\dist\browser*. Créez un dossier nommé *signalr* sous le dossier *wwwroot\\lib*. Copiez le fichier *signalr.js* dans le dossier *wwwroot\lib\signalr*.

## <a name="use-the-signalr-javascript-client"></a>Utiliser le client JavaScript SignalR

Référencez le client JavaScript SignalR dans l'élément `<script>`.

```html
<script src="~/lib/signalr/signalr.js"></script>
```

## <a name="connect-to-a-hub"></a>Se connecter à un hub

Le code suivant crée et démarre une connexion. Nom de concentrateur respecte la casse.

[!code-javascript[Call hub methods](javascript-client/sample/wwwroot/js/chat.js?range=9-13,43-45)]

### <a name="cross-origin-connections"></a>Connexions cross-origin

En règle générale, les navigateurs chargent des connexions à partir du même domaine que la page demandée. Toutefois, il existe des cas où une connexion à un autre domaine est nécessaire.

Pour empêcher la lecture des données sensibles à partir d’un autre site, un site malveillant [cross-origin connexions](xref:security/cors) sont désactivés par défaut. Pour autoriser une demande de cross-origin, activez-la dans le `Startup` classe.

[!code-csharp[Cross-origin connections](javascript-client/sample/Startup.cs?highlight=29-35,56)]

## <a name="call-hub-methods-from-client"></a>Appeler des méthodes de hub à partir du client

Les clients JavaScript appellent les méthodes publiques sur les hubs via la méthode [invoke](/javascript/api/%40aspnet/signalr/hubconnection#invoke) de la [HubConnection](/javascript/api/%40aspnet/signalr/hubconnection). La méthode `invoke` accepte deux arguments :

* Le nom de la méthode de hub. Dans l’exemple suivant, le nom de méthode sur le hub est `SendMessage`.
* Tous les arguments définis dans la méthode de hub. Dans l’exemple suivant, le nom de l’argument est `message`. L’exemple de code utilise la syntaxe de fonction arrow est prise en charge dans les versions actuelles de tous les principaux navigateurs à l’exception d’Internet Explorer.

  [!code-javascript[Call hub methods](javascript-client/sample/wwwroot/js/chat.js?range=24)]

> [!NOTE]
> Si vous utilisez le Service Azure SignalR dans *mode sans serveur*, vous ne pouvez pas appeler des méthodes de concentrateur à partir d’un client. Pour plus d’informations, consultez le [documentation de SignalR Service](/azure/azure-signalr/signalr-concept-serverless-development-config).

## <a name="call-client-methods-from-hub"></a>Appeler des méthodes de client à partir de hub

Pour recevoir des messages à partir du hub, définissez une méthode à l’aide de la méthode [on](/javascript/api/%40aspnet/signalr/hubconnection#on) de la `HubConnection`.

* Le nom de la méthode du client JavaScript. Dans l’exemple suivant, le nom de la méthode est `ReceiveMessage`.
* Les arguments que le hub passe à la méthode. Dans l’exemple suivant, la valeur d’argument est `message`.

[!code-javascript[Receive calls from hub](javascript-client/sample/wwwroot/js/chat.js?range=14-19)]

Le code précédent dans `connection.on` s’exécute lorsque le code côté serveur appelle à l’aide de la [SendAsync](/dotnet/api/microsoft.aspnetcore.signalr.clientproxyextensions.sendasync) (méthode).

[!code-csharp[Call client-side](javascript-client/sample/hubs/chathub.cs?range=8-11)]

SignalR détermine la méthode de client à appeler en faisant correspondre le nom de méthode et les arguments définis dans `SendAsync` et `connection.on`.

> [!NOTE]
> Comme meilleure pratique, appelez la méthode [start](/javascript/api/%40aspnet/signalr/hubconnection#start) sur le `HubConnection` après `on`. En procédant comme ceci, vos gestionnaires sont enregistrés avant que tous les messages soient reçus.

## <a name="error-handling-and-logging"></a>Journalisation et gestion des erreurs

Chaînez une méthode `catch` à la fin de la méthode `start` pour gérer les erreurs côté client. Utilisez `console.error` pour envoyer les erreurs vers la console du navigateur.

[!code-javascript[Error handling](javascript-client/sample/wwwroot/js/chat.js?range=49-51)]

Configurer le suivi du journal côté client en passant un enregistreur d’événements et le type d’événement pour vous connecter lorsque la connexion est établie. Les messages sont enregistrés avec le niveau de journalisation spécifié et versions ultérieures. Niveaux de consignation disponibles sont les suivantes :

* `signalR.LogLevel.Error` &ndash; Messages d’erreur. Journaux `Error` messages uniquement.
* `signalR.LogLevel.Warning` &ndash; Messages d’avertissement concernant les erreurs potentielles. Journaux `Warning`, et `Error` messages.
* `signalR.LogLevel.Information` &ndash; Messages d’état sans erreurs. Journaux `Information`, `Warning`, et `Error` messages.
* `signalR.LogLevel.Trace` &ndash; Messages de trace. Consigne tout, y compris les données transportées entre le hub et le client.

Utilisez le [configureLogging](/javascript/api/%40aspnet/signalr/hubconnectionbuilder#configurelogging) méthode sur [HubConnectionBuilder](/javascript/api/%40aspnet/signalr/hubconnectionbuilder) pour configurer le niveau de journalisation. Les messages sont enregistrés dans la console du navigateur.

[!code-javascript[Logging levels](javascript-client/sample/wwwroot/js/chat.js?range=9-12)]

## <a name="reconnect-clients"></a>Reconnecter les clients

::: moniker range=">= aspnetcore-3.0"

### <a name="automatically-reconnect"></a>Se reconnecter automatiquement

Le client JavaScript pour SignalR peut être configuré pour se reconnecter automatiquement à l’aide de la `withAutomaticReconnect` méthode sur [HubConnectionBuilder](/javascript/api/%40aspnet/signalr/hubconnectionbuilder). Il ne se reconnecter automatiquement par défaut.

```javascript
const connection = new signalR.HubConnectionBuilder()
    .withUrl("/chatHub")
    .withAutomaticReconnect()
    .build();
```

Sans paramètres, `withAutomaticReconnect()` configure le client en attente 0, 2, 10 et 30 secondes respectivement avant d’essayer de chaque tentative de reconnexion s’arrête après avoir quatre tentatives ayant échoué.

Avant de commencer toute tentative de reconnexion, le `HubConnection` sera la transition vers le `HubConnectionState.Reconnecting` d’état et déclencher son `onreconnecting` rappels au lieu de la transition vers le `Disconnected` état et le déclenchement son `onclose` rappels comme un `HubConnection`sans reconnexion automatique configuré. Cela fournit une opportunité pour avertir les utilisateurs que la connexion a été perdue et désactiver les éléments d’interface utilisateur.

```javascript
connection.onreconnecting((error) => {
  console.assert(connection.state === signalR.HubConnectionState.Reconnecting);

  document.getElementById("messageInput").disabled = true;

  const li = document.createElement("li");
  li.textContent = `Connection lost due to error "${error}". Reconnecting.`;
  document.getElementById("messagesList").appendChild(li);
});
```

Si le client se reconnecte avec succès au sein de ses quatre premières tentatives, le `HubConnection` passera à le `Connected` d’état et déclencher son `onreconnected` rappels. Cela permet d’informer les utilisateurs de que la connexion a été rétablie.

Étant donné que la connexion est entièrement nouveau sur le serveur, un nouveau `connectionId` seront fournies à la `onreconnected` rappel.

> [!WARNING]
> Le `onreconnected` du rappel `connectionId` paramètre n’est pas défini si le `HubConnection` a été configuré pour [ignorer négociation](xref:signalr/configuration#configure-client-options).

```javascript
connection.onreconnected((connectionId) => {
  console.assert(connection.state === signalR.HubConnectionState.Connected);

  document.getElementById("messageInput").disabled = false;

  const li = document.createElement("li");
  li.textContent = `Connection reestablished. Connected with connectionId "${connectionId}".`;
  document.getElementById("messagesList").appendChild(li);
});
```

`withAutomaticReconnect()` ne configurez pas le `HubConnection` pour réessayer d’échecs de démarrage initial, afin de l’échec de démarrage doivent être traitées manuellement :

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

Si le client ne se reconnecter avec succès au sein de ses quatre premières tentatives, le `HubConnection` sera la transition vers le `Disconnected` d’état et déclencher son [onclose](/javascript/api/%40aspnet/signalr/hubconnection#onclose) rappels. Cela permet d’informer les utilisateurs de la connexion a été définitivement perdu et vous recommandons de l’actualisation de la page :

```javascript
connection.onclose((error) => {
  console.assert(connection.state === signalR.HubConnectionState.Disconnected);

  document.getElementById("messageInput").disabled = true;

  const li = document.createElement("li");
  li.textContent = `Connection closed due to error "${error}". Try refreshing this page to restart the connection.`;
  document.getElementById("messagesList").appendChild(li);
})
```

Pour configurer le nombre de tentatives de reconnexion avant de déconnecter ou de modifier le minutage de la reconnexion, `withAutomaticReconnect` accepte un tableau de nombres représentant le délai en millisecondes à attendre avant de commencer chaque tentative de reconnexion.

```javascript
const connection = new signalR.HubConnectionBuilder()
    .withUrl("/chatHub")
    .withAutomaticReconnect([0, 0, 10000])
    .build();

    // .withAutomaticReconnect([0, 2000, 10000, 30000]) yields the default behavior
```

L’exemple précédent configure le `HubConnection` pour démarrer une tentative de reconnexions immédiatement après la connexion est perdue. Cela vaut également pour la configuration par défaut.

Si la première tentative de reconnexion échoue, la deuxième tentative de reconnexion également démarre immédiatement au lieu d’attendre 2 secondes comme il le ferait dans la configuration par défaut.

Si la deuxième tentative de reconnexion échoue, la troisième tentative de reconnexion démarrera dans 10 secondes, ce qui revient à nouveau à la configuration par défaut.

Le comportement personnalisé puis diffère à nouveau le comportement par défaut en arrêtant après la reconnexion troisième tentative d’échec au lieu de tenter une reconnexion plus tentative dans un autre 30 secondes comme il le ferait dans la configuration par défaut.

Si vous souhaitez mieux contrôler la fréquence et le nombre d’automatique nouvelle tentative, `withAutomaticReconnect` accepte un objet qui implémente le `IReconnectPolicy` interface, ce qui a une méthode unique nommée `nextRetryDelayInMilliseconds`.

`nextRetryDelayInMilliseconds` accepte deux arguments, `previousRetryCount` et `elapsedMilliseconds`, qui sont les deux nombres. Avant la première tentative de reconnexion, les deux `previousRetryCount` et `elapsedMilliseconds` sera égal à zéro. Après chaque tentative ayant échoué, `previousRetryCount` sera incrémenté et `elapsedMilliseconds` sera mis à jour pour refléter la quantité de temps passé à la reconnexion jusqu’en millisecondes.

`nextRetryDelayInMilliseconds` doit retourner un nombre représentant le nombre de millisecondes à attendre avant la prochaine tentative de reconnexion ou `null` si le `HubConnection` doit s’arrêter la reconnexion.

```javascript
const connection = new signalR.HubConnectionBuilder()
    .withUrl("/chatHub")
    .withAutomaticReconnect({
        nextRetryDelayInMilliseconds: (previousRetryCount, elapsedMilliseconds) => {
          if (elapsedMilliseconds < 60000) {
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

Vous pouvez également écrire le code qui se reconnectera votre client manuellement, comme illustré dans [reconnecter manuellement](#manually-reconnect).

::: moniker-end

### <a name="manually-reconnect"></a>Reconnecter manuellement

::: moniker range="< aspnetcore-3.0"

> [!WARNING]
> Avant la version 3.0, le client JavaScript pour SignalR ne se reconnecter automatiquement. Vous devez écrire du code qui se reconnectera votre client manuellement.

::: moniker-end

Le code suivant illustre une approche classique de reconnexion manuelle :

1. Une fonction (dans ce cas, le `start` (fonction)) est créé pour démarrer la connexion.
1. Appelez le `start` (fonction) dans la connexion `onclose` Gestionnaire d’événements.

[!code-javascript[Reconnect the JavaScript client](javascript-client/sample/wwwroot/js/chat.js?range=28-40)]

Une implémentation réelle serait utiliser une temporisation exponentielle ou réessayer un nombre spécifié de fois avant d’abandonner. 

## <a name="additional-resources"></a>Ressources supplémentaires

* [Référence API JavaScript](/javascript/api/?view=signalr-js-latest)
* [Didacticiel de JavaScript](xref:tutorials/signalr)
* [Didacticiel WebPack et TypeScript](xref:tutorials/signalr-typescript-webpack)
* [Hubs](xref:signalr/hubs)
* [Client .NET](xref:signalr/dotnet-client)
* [Publier sur Azure](xref:signalr/publish-to-azure-web-app)
* [Demandes de cross-Origin (CORS)](xref:security/cors)
* [Documentation de serverless Azure SignalR Service](/azure/azure-signalr/signalr-concept-serverless-development-config)
