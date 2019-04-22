---
title: Client JavaScript ASP.NET Core SignalR
author: bradygaster
description: Vue d’ensemble du client JavaScript ASP.NET Core SignalR.
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 04/17/2019
uid: signalr/javascript-client
ms.openlocfilehash: f1f072e63928502fa1bad62e808ff035e57f2fd3
ms.sourcegitcommit: eb784a68219b4829d8e50c8a334c38d4b94e0cfa
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/22/2019
ms.locfileid: "59983011"
---
# <a name="aspnet-core-signalr-javascript-client"></a><span data-ttu-id="f6215-103">Client JavaScript ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="f6215-103">ASP.NET Core SignalR JavaScript client</span></span>

<span data-ttu-id="f6215-104">Par [Rachel Appel](http://twitter.com/rachelappel)</span><span class="sxs-lookup"><span data-stu-id="f6215-104">By [Rachel Appel](http://twitter.com/rachelappel)</span></span>

<span data-ttu-id="f6215-105">La bibliothèque cliente JavaScript ASP.NET Core SignalR permet aux développeurs d’appeler un hub à partir de code côté serveur.</span><span class="sxs-lookup"><span data-stu-id="f6215-105">The ASP.NET Core SignalR JavaScript client library enables developers to call server-side hub code.</span></span>

<span data-ttu-id="f6215-106">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/Docs/tree/live/aspnetcore/signalr/javascript-client/sample) ([procédure de téléchargement](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="f6215-106">[View or download sample code](https://github.com/aspnet/Docs/tree/live/aspnetcore/signalr/javascript-client/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="install-the-signalr-client-package"></a><span data-ttu-id="f6215-107">Installer le package du client SignalR</span><span class="sxs-lookup"><span data-stu-id="f6215-107">Install the SignalR client package</span></span>

<span data-ttu-id="f6215-108">La bibliothèque de client SignalR JavaScript est remise en tant qu’un [npm](https://www.npmjs.com/) package.</span><span class="sxs-lookup"><span data-stu-id="f6215-108">The SignalR JavaScript client library is delivered as an [npm](https://www.npmjs.com/) package.</span></span> <span data-ttu-id="f6215-109">Si vous utilisez Visual Studio, exécutez `npm install` à partir de la **Console du Gestionnaire de Package** lorsque vous êtes dans le dossier racine.</span><span class="sxs-lookup"><span data-stu-id="f6215-109">If you're using Visual Studio, run `npm install` from the **Package Manager Console** while in the root folder.</span></span> <span data-ttu-id="f6215-110">Pour Visual Studio Code, exécutez la commande depuis le **Terminal intégré**.</span><span class="sxs-lookup"><span data-stu-id="f6215-110">For Visual Studio Code, run the command from the **Integrated Terminal**.</span></span>

  ```console
  npm init -y
  npm install @aspnet/signalr
  ```

<span data-ttu-id="f6215-111">npm installe le contenu du package dans le dossier *node_modules\\@aspnet\signalr\dist\browser*.</span><span class="sxs-lookup"><span data-stu-id="f6215-111">npm installs the package contents in the *node_modules\\@aspnet\signalr\dist\browser* folder.</span></span> <span data-ttu-id="f6215-112">Créez un dossier nommé *signalr* sous le dossier *wwwroot\\lib*.</span><span class="sxs-lookup"><span data-stu-id="f6215-112">Create a new folder named *signalr* under the *wwwroot\\lib* folder.</span></span> <span data-ttu-id="f6215-113">Copiez le fichier *signalr.js* dans le dossier *wwwroot\lib\signalr*.</span><span class="sxs-lookup"><span data-stu-id="f6215-113">Copy the *signalr.js* file to the *wwwroot\lib\signalr* folder.</span></span>

## <a name="use-the-signalr-javascript-client"></a><span data-ttu-id="f6215-114">Utiliser le client JavaScript SignalR</span><span class="sxs-lookup"><span data-stu-id="f6215-114">Use the SignalR JavaScript client</span></span>

<span data-ttu-id="f6215-115">Référencez le client JavaScript SignalR dans l'élément `<script>`.</span><span class="sxs-lookup"><span data-stu-id="f6215-115">Reference the SignalR JavaScript client in the `<script>` element.</span></span>

```html
<script src="~/lib/signalr/signalr.js"></script>
```

## <a name="connect-to-a-hub"></a><span data-ttu-id="f6215-116">Se connecter à un hub</span><span class="sxs-lookup"><span data-stu-id="f6215-116">Connect to a hub</span></span>

<span data-ttu-id="f6215-117">Le code suivant crée et démarre une connexion.</span><span class="sxs-lookup"><span data-stu-id="f6215-117">The following code creates and starts a connection.</span></span> <span data-ttu-id="f6215-118">Nom de concentrateur respecte la casse.</span><span class="sxs-lookup"><span data-stu-id="f6215-118">The hub's name is case insensitive.</span></span>

[!code-javascript[Call hub methods](javascript-client/sample/wwwroot/js/chat.js?range=9-13,43-45)]

### <a name="cross-origin-connections"></a><span data-ttu-id="f6215-119">Connexions cross-origin</span><span class="sxs-lookup"><span data-stu-id="f6215-119">Cross-origin connections</span></span>

<span data-ttu-id="f6215-120">En règle générale, les navigateurs chargent des connexions à partir du même domaine que la page demandée.</span><span class="sxs-lookup"><span data-stu-id="f6215-120">Typically, browsers load connections from the same domain as the requested page.</span></span> <span data-ttu-id="f6215-121">Toutefois, il existe des cas où une connexion à un autre domaine est nécessaire.</span><span class="sxs-lookup"><span data-stu-id="f6215-121">However, there are occasions when a connection to another domain is required.</span></span>

<span data-ttu-id="f6215-122">Pour empêcher la lecture des données sensibles à partir d’un autre site, un site malveillant [cross-origin connexions](xref:security/cors) sont désactivés par défaut.</span><span class="sxs-lookup"><span data-stu-id="f6215-122">To prevent a malicious site from reading sensitive data from another site, [cross-origin connections](xref:security/cors) are disabled by default.</span></span> <span data-ttu-id="f6215-123">Pour autoriser une demande de cross-origin, activez-la dans le `Startup` classe.</span><span class="sxs-lookup"><span data-stu-id="f6215-123">To allow a cross-origin request, enable it in the `Startup` class.</span></span>

[!code-csharp[Cross-origin connections](javascript-client/sample/Startup.cs?highlight=29-35,56)]

## <a name="call-hub-methods-from-client"></a><span data-ttu-id="f6215-124">Appeler des méthodes de hub à partir du client</span><span class="sxs-lookup"><span data-stu-id="f6215-124">Call hub methods from client</span></span>

<span data-ttu-id="f6215-125">Les clients JavaScript appellent les méthodes publiques sur les hubs via la méthode [invoke](/javascript/api/%40aspnet/signalr/hubconnection#invoke) de la [HubConnection](/javascript/api/%40aspnet/signalr/hubconnection).</span><span class="sxs-lookup"><span data-stu-id="f6215-125">JavaScript clients call public methods on hubs via the [invoke](/javascript/api/%40aspnet/signalr/hubconnection#invoke) method of the [HubConnection](/javascript/api/%40aspnet/signalr/hubconnection).</span></span> <span data-ttu-id="f6215-126">La méthode `invoke` accepte deux arguments :</span><span class="sxs-lookup"><span data-stu-id="f6215-126">The `invoke` method accepts two arguments:</span></span>

* <span data-ttu-id="f6215-127">Le nom de la méthode de hub.</span><span class="sxs-lookup"><span data-stu-id="f6215-127">The name of the hub method.</span></span> <span data-ttu-id="f6215-128">Dans l’exemple suivant, le nom de méthode sur le hub est `SendMessage`.</span><span class="sxs-lookup"><span data-stu-id="f6215-128">In the following example, the method name on the hub is `SendMessage`.</span></span>
* <span data-ttu-id="f6215-129">Tous les arguments définis dans la méthode de hub.</span><span class="sxs-lookup"><span data-stu-id="f6215-129">Any arguments defined in the hub method.</span></span> <span data-ttu-id="f6215-130">Dans l’exemple suivant, le nom de l’argument est `message`.</span><span class="sxs-lookup"><span data-stu-id="f6215-130">In the following example, the argument name is `message`.</span></span> <span data-ttu-id="f6215-131">L’exemple de code utilise la syntaxe de fonction arrow est prise en charge dans les versions actuelles de tous les principaux navigateurs à l’exception d’Internet Explorer.</span><span class="sxs-lookup"><span data-stu-id="f6215-131">The example code uses arrow function syntax that is supported in current versions of all major browsers except Internet Explorer.</span></span>

  [!code-javascript[Call hub methods](javascript-client/sample/wwwroot/js/chat.js?range=24)]

> [!NOTE]
> <span data-ttu-id="f6215-132">Si vous utilisez le Service Azure SignalR dans *mode sans serveur*, vous ne pouvez pas appeler des méthodes de concentrateur à partir d’un client.</span><span class="sxs-lookup"><span data-stu-id="f6215-132">If you're using Azure SignalR Service in *Serverless mode*, you cannot call hub methods from a client.</span></span> <span data-ttu-id="f6215-133">Pour plus d’informations, consultez le [documentation de SignalR Service](/azure/azure-signalr/signalr-concept-serverless-development-config).</span><span class="sxs-lookup"><span data-stu-id="f6215-133">For more information, see the [SignalR Service documentation](/azure/azure-signalr/signalr-concept-serverless-development-config).</span></span>

<span data-ttu-id="f6215-134">Le `invoke` méthode retourne un code JavaScript [promesse](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Promise).</span><span class="sxs-lookup"><span data-stu-id="f6215-134">The `invoke` method returns a JavaScript [Promise](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Promise).</span></span> <span data-ttu-id="f6215-135">Le `Promise` est résolu avec la valeur de retour (le cas échéant) lorsque la méthode sur le serveur est retournée.</span><span class="sxs-lookup"><span data-stu-id="f6215-135">The `Promise` is resolved with the return value (if any) when the method on the server returns.</span></span> <span data-ttu-id="f6215-136">Si la méthode sur le serveur génère une erreur, le `Promise` est rejetée avec le message d’erreur.</span><span class="sxs-lookup"><span data-stu-id="f6215-136">If the method on the server throws an error, the `Promise` is rejected with the error message.</span></span> <span data-ttu-id="f6215-137">Utilisez le `then` et `catch` méthodes sur le `Promise` lui-même pour gérer ces cas (ou `await` syntaxe).</span><span class="sxs-lookup"><span data-stu-id="f6215-137">Use the `then` and `catch` methods on the `Promise` itself to handle these cases (or `await` syntax).</span></span>

<span data-ttu-id="f6215-138">Le `send` méthode retourne un code JavaScript `Promise`.</span><span class="sxs-lookup"><span data-stu-id="f6215-138">The `send` method returns a JavaScript `Promise`.</span></span> <span data-ttu-id="f6215-139">Le `Promise` est résolu lorsque le message a été envoyé au serveur.</span><span class="sxs-lookup"><span data-stu-id="f6215-139">The `Promise` is resolved when the message has been sent to the server.</span></span> <span data-ttu-id="f6215-140">S’il existe une erreur d’envoi du message, le `Promise` est rejetée avec le message d’erreur.</span><span class="sxs-lookup"><span data-stu-id="f6215-140">If there is an error sending the message, the `Promise` is rejected with the error message.</span></span> <span data-ttu-id="f6215-141">Utilisez le `then` et `catch` méthodes sur le `Promise` lui-même pour gérer ces cas (ou `await` syntaxe).</span><span class="sxs-lookup"><span data-stu-id="f6215-141">Use the `then` and `catch` methods on the `Promise` itself to handle these cases (or `await` syntax).</span></span>

> [!NOTE]
> <span data-ttu-id="f6215-142">À l’aide de `send` n’attend pas jusqu'à ce que le serveur a reçu le message.</span><span class="sxs-lookup"><span data-stu-id="f6215-142">Using `send` doesn't wait until the server has received the message.</span></span> <span data-ttu-id="f6215-143">Par conséquent, il n’est pas possible de retourner des erreurs ou des données à partir du serveur.</span><span class="sxs-lookup"><span data-stu-id="f6215-143">Consequently, it's not possible to return data or errors from the server.</span></span>

## <a name="call-client-methods-from-hub"></a><span data-ttu-id="f6215-144">Appeler des méthodes de client à partir de hub</span><span class="sxs-lookup"><span data-stu-id="f6215-144">Call client methods from hub</span></span>

<span data-ttu-id="f6215-145">Pour recevoir des messages à partir du hub, définissez une méthode à l’aide de la méthode [on](/javascript/api/%40aspnet/signalr/hubconnection#on) de la `HubConnection`.</span><span class="sxs-lookup"><span data-stu-id="f6215-145">To receive messages from the hub, define a method using the [on](/javascript/api/%40aspnet/signalr/hubconnection#on) method of the `HubConnection`.</span></span>

* <span data-ttu-id="f6215-146">Le nom de la méthode du client JavaScript.</span><span class="sxs-lookup"><span data-stu-id="f6215-146">The name of the JavaScript client method.</span></span> <span data-ttu-id="f6215-147">Dans l’exemple suivant, le nom de la méthode est `ReceiveMessage`.</span><span class="sxs-lookup"><span data-stu-id="f6215-147">In the following example, the method name is `ReceiveMessage`.</span></span>
* <span data-ttu-id="f6215-148">Les arguments que le hub passe à la méthode.</span><span class="sxs-lookup"><span data-stu-id="f6215-148">Arguments the hub passes to the method.</span></span> <span data-ttu-id="f6215-149">Dans l’exemple suivant, la valeur d’argument est `message`.</span><span class="sxs-lookup"><span data-stu-id="f6215-149">In the following example, the argument value is `message`.</span></span>

[!code-javascript[Receive calls from hub](javascript-client/sample/wwwroot/js/chat.js?range=14-19)]

<span data-ttu-id="f6215-150">Le code précédent dans `connection.on` s’exécute lorsque le code côté serveur appelle à l’aide de la [SendAsync](/dotnet/api/microsoft.aspnetcore.signalr.clientproxyextensions.sendasync) (méthode).</span><span class="sxs-lookup"><span data-stu-id="f6215-150">The preceding code in `connection.on` runs when server-side code calls it using the [SendAsync](/dotnet/api/microsoft.aspnetcore.signalr.clientproxyextensions.sendasync) method.</span></span>

[!code-csharp[Call client-side](javascript-client/sample/hubs/chathub.cs?range=8-11)]

<span data-ttu-id="f6215-151">SignalR détermine la méthode de client à appeler en faisant correspondre le nom de méthode et les arguments définis dans `SendAsync` et `connection.on`.</span><span class="sxs-lookup"><span data-stu-id="f6215-151">SignalR determines which client method to call by matching the method name and arguments defined in `SendAsync` and `connection.on`.</span></span>

> [!NOTE]
> <span data-ttu-id="f6215-152">Comme meilleure pratique, appelez la méthode [start](/javascript/api/%40aspnet/signalr/hubconnection#start) sur le `HubConnection` après `on`.</span><span class="sxs-lookup"><span data-stu-id="f6215-152">As a best practice, call the [start](/javascript/api/%40aspnet/signalr/hubconnection#start) method on the `HubConnection` after `on`.</span></span> <span data-ttu-id="f6215-153">En procédant comme ceci, vos gestionnaires sont enregistrés avant que tous les messages soient reçus.</span><span class="sxs-lookup"><span data-stu-id="f6215-153">Doing so ensures your handlers are registered before any messages are received.</span></span>

## <a name="error-handling-and-logging"></a><span data-ttu-id="f6215-154">Journalisation et gestion des erreurs</span><span class="sxs-lookup"><span data-stu-id="f6215-154">Error handling and logging</span></span>

<span data-ttu-id="f6215-155">Chaînez une méthode `catch` à la fin de la méthode `start` pour gérer les erreurs côté client.</span><span class="sxs-lookup"><span data-stu-id="f6215-155">Chain a `catch` method to the end of the `start` method to handle client-side errors.</span></span> <span data-ttu-id="f6215-156">Utilisez `console.error` pour envoyer les erreurs vers la console du navigateur.</span><span class="sxs-lookup"><span data-stu-id="f6215-156">Use `console.error` to output errors to the browser's console.</span></span>

[!code-javascript[Error handling](javascript-client/sample/wwwroot/js/chat.js?range=49-51)]

<span data-ttu-id="f6215-157">Configurer le suivi du journal côté client en passant un enregistreur d’événements et le type d’événement pour vous connecter lorsque la connexion est établie.</span><span class="sxs-lookup"><span data-stu-id="f6215-157">Setup client-side log tracing by passing a logger and type of event to log when the connection is made.</span></span> <span data-ttu-id="f6215-158">Les messages sont enregistrés avec le niveau de journalisation spécifié et versions ultérieures.</span><span class="sxs-lookup"><span data-stu-id="f6215-158">Messages are logged with the specified log level and higher.</span></span> <span data-ttu-id="f6215-159">Niveaux de consignation disponibles sont les suivantes :</span><span class="sxs-lookup"><span data-stu-id="f6215-159">Available log levels are as follows:</span></span>

* <span data-ttu-id="f6215-160">`signalR.LogLevel.Error` &ndash; Messages d’erreur.</span><span class="sxs-lookup"><span data-stu-id="f6215-160">`signalR.LogLevel.Error` &ndash; Error messages.</span></span> <span data-ttu-id="f6215-161">Journaux `Error` messages uniquement.</span><span class="sxs-lookup"><span data-stu-id="f6215-161">Logs `Error` messages only.</span></span>
* <span data-ttu-id="f6215-162">`signalR.LogLevel.Warning` &ndash; Messages d’avertissement concernant les erreurs potentielles.</span><span class="sxs-lookup"><span data-stu-id="f6215-162">`signalR.LogLevel.Warning` &ndash; Warning messages about potential errors.</span></span> <span data-ttu-id="f6215-163">Journaux `Warning`, et `Error` messages.</span><span class="sxs-lookup"><span data-stu-id="f6215-163">Logs `Warning`, and `Error` messages.</span></span>
* <span data-ttu-id="f6215-164">`signalR.LogLevel.Information` &ndash; Messages d’état sans erreurs.</span><span class="sxs-lookup"><span data-stu-id="f6215-164">`signalR.LogLevel.Information` &ndash; Status messages without errors.</span></span> <span data-ttu-id="f6215-165">Journaux `Information`, `Warning`, et `Error` messages.</span><span class="sxs-lookup"><span data-stu-id="f6215-165">Logs `Information`, `Warning`, and `Error` messages.</span></span>
* <span data-ttu-id="f6215-166">`signalR.LogLevel.Trace` &ndash; Messages de trace.</span><span class="sxs-lookup"><span data-stu-id="f6215-166">`signalR.LogLevel.Trace` &ndash; Trace messages.</span></span> <span data-ttu-id="f6215-167">Consigne tout, y compris les données transportées entre le hub et le client.</span><span class="sxs-lookup"><span data-stu-id="f6215-167">Logs everything, including data transported between hub and client.</span></span>

<span data-ttu-id="f6215-168">Utilisez le [configureLogging](/javascript/api/%40aspnet/signalr/hubconnectionbuilder#configurelogging) méthode sur [HubConnectionBuilder](/javascript/api/%40aspnet/signalr/hubconnectionbuilder) pour configurer le niveau de journalisation.</span><span class="sxs-lookup"><span data-stu-id="f6215-168">Use the [configureLogging](/javascript/api/%40aspnet/signalr/hubconnectionbuilder#configurelogging) method on [HubConnectionBuilder](/javascript/api/%40aspnet/signalr/hubconnectionbuilder) to configure the log level.</span></span> <span data-ttu-id="f6215-169">Les messages sont enregistrés dans la console du navigateur.</span><span class="sxs-lookup"><span data-stu-id="f6215-169">Messages are logged to the browser console.</span></span>

[!code-javascript[Logging levels](javascript-client/sample/wwwroot/js/chat.js?range=9-12)]

## <a name="reconnect-clients"></a><span data-ttu-id="f6215-170">Reconnecter les clients</span><span class="sxs-lookup"><span data-stu-id="f6215-170">Reconnect clients</span></span>

::: moniker range=">= aspnetcore-3.0"

### <a name="automatically-reconnect"></a><span data-ttu-id="f6215-171">Se reconnecter automatiquement</span><span class="sxs-lookup"><span data-stu-id="f6215-171">Automatically reconnect</span></span>

<span data-ttu-id="f6215-172">Le client JavaScript pour SignalR peut être configuré pour se reconnecter automatiquement à l’aide de la `withAutomaticReconnect` méthode sur [HubConnectionBuilder](/javascript/api/%40aspnet/signalr/hubconnectionbuilder).</span><span class="sxs-lookup"><span data-stu-id="f6215-172">The JavaScript client for SignalR can be configured to automatically reconnect using the `withAutomaticReconnect` method on [HubConnectionBuilder](/javascript/api/%40aspnet/signalr/hubconnectionbuilder).</span></span> <span data-ttu-id="f6215-173">Il ne se reconnecter automatiquement par défaut.</span><span class="sxs-lookup"><span data-stu-id="f6215-173">It won't automatically reconnect by default.</span></span>

```javascript
const connection = new signalR.HubConnectionBuilder()
    .withUrl("/chatHub")
    .withAutomaticReconnect()
    .build();
```

<span data-ttu-id="f6215-174">Sans paramètres, `withAutomaticReconnect()` configure le client en attente 0, 2, 10 et 30 secondes respectivement avant d’essayer de chaque tentative de reconnexion s’arrête après avoir quatre tentatives ayant échoué.</span><span class="sxs-lookup"><span data-stu-id="f6215-174">Without any parameters, `withAutomaticReconnect()` configures the client to wait 0, 2, 10, and 30 seconds respectively before trying each reconnect attempt, stopping after four failed attempts.</span></span>

<span data-ttu-id="f6215-175">Avant de commencer toute tentative de reconnexion, le `HubConnection` sera la transition vers le `HubConnectionState.Reconnecting` d’état et déclencher son `onreconnecting` rappels au lieu de la transition vers le `Disconnected` état et le déclenchement son `onclose` rappels comme un `HubConnection`sans reconnexion automatique configuré.</span><span class="sxs-lookup"><span data-stu-id="f6215-175">Before starting any reconnect attempts, the `HubConnection` will transition to the `HubConnectionState.Reconnecting` state and fire its `onreconnecting` callbacks instead of transitioning to the `Disconnected` state and triggering its `onclose` callbacks like a `HubConnection` without automatic reconnect configured.</span></span> <span data-ttu-id="f6215-176">Cela fournit une opportunité pour avertir les utilisateurs que la connexion a été perdue et désactiver les éléments d’interface utilisateur.</span><span class="sxs-lookup"><span data-stu-id="f6215-176">This provides an opportunity to warn users that the connection has been lost and to disable UI elements.</span></span>

```javascript
connection.onreconnecting((error) => {
  console.assert(connection.state === signalR.HubConnectionState.Reconnecting);

  document.getElementById("messageInput").disabled = true;

  const li = document.createElement("li");
  li.textContent = `Connection lost due to error "${error}". Reconnecting.`;
  document.getElementById("messagesList").appendChild(li);
});
```

<span data-ttu-id="f6215-177">Si le client se reconnecte avec succès au sein de ses quatre premières tentatives, le `HubConnection` passera à le `Connected` d’état et déclencher son `onreconnected` rappels.</span><span class="sxs-lookup"><span data-stu-id="f6215-177">If the client successfully reconnects within its first four attempts, the `HubConnection` will transition back to the `Connected` state and fire its `onreconnected` callbacks.</span></span> <span data-ttu-id="f6215-178">Cela permet d’informer les utilisateurs de que la connexion a été rétablie.</span><span class="sxs-lookup"><span data-stu-id="f6215-178">This provides an opportunity to inform users the connection has been reestablished.</span></span>

<span data-ttu-id="f6215-179">Étant donné que la connexion est entièrement nouveau sur le serveur, un nouveau `connectionId` seront fournies à la `onreconnected` rappel.</span><span class="sxs-lookup"><span data-stu-id="f6215-179">Since the connection looks entirely new to the server, a new `connectionId` will be provided to the `onreconnected` callback.</span></span>

> [!WARNING]
> <span data-ttu-id="f6215-180">Le `onreconnected` du rappel `connectionId` paramètre n’est pas défini si le `HubConnection` a été configuré pour [ignorer négociation](xref:signalr/configuration#configure-client-options).</span><span class="sxs-lookup"><span data-stu-id="f6215-180">The `onreconnected` callback's `connectionId` parameter will be undefined if the `HubConnection` was configured to [skip negotiation](xref:signalr/configuration#configure-client-options).</span></span>

```javascript
connection.onreconnected((connectionId) => {
  console.assert(connection.state === signalR.HubConnectionState.Connected);

  document.getElementById("messageInput").disabled = false;

  const li = document.createElement("li");
  li.textContent = `Connection reestablished. Connected with connectionId "${connectionId}".`;
  document.getElementById("messagesList").appendChild(li);
});
```

<span data-ttu-id="f6215-181">`withAutomaticReconnect()` ne configurez pas le `HubConnection` pour réessayer d’échecs de démarrage initial, afin de l’échec de démarrage doivent être traitées manuellement :</span><span class="sxs-lookup"><span data-stu-id="f6215-181">`withAutomaticReconnect()` won't configure the `HubConnection` to retry initial start failures, so start failures need to be handled manually:</span></span>

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

<span data-ttu-id="f6215-182">Si le client ne se reconnecter avec succès au sein de ses quatre premières tentatives, le `HubConnection` sera la transition vers le `Disconnected` d’état et déclencher son [onclose](/javascript/api/%40aspnet/signalr/hubconnection#onclose) rappels.</span><span class="sxs-lookup"><span data-stu-id="f6215-182">If the client doesn't successfully reconnect within its first four attempts, the `HubConnection` will transition to the `Disconnected` state and fire its [onclose](/javascript/api/%40aspnet/signalr/hubconnection#onclose) callbacks.</span></span> <span data-ttu-id="f6215-183">Cela permet d’informer les utilisateurs de la connexion a été définitivement perdu et vous recommandons de l’actualisation de la page :</span><span class="sxs-lookup"><span data-stu-id="f6215-183">This provides an opportunity to inform users the connection has been permanently lost and recommend refreshing the page:</span></span>

```javascript
connection.onclose((error) => {
  console.assert(connection.state === signalR.HubConnectionState.Disconnected);

  document.getElementById("messageInput").disabled = true;

  const li = document.createElement("li");
  li.textContent = `Connection closed due to error "${error}". Try refreshing this page to restart the connection.`;
  document.getElementById("messagesList").appendChild(li);
})
```

<span data-ttu-id="f6215-184">Pour configurer le nombre de tentatives de reconnexion avant de déconnecter ou de modifier le minutage de la reconnexion, `withAutomaticReconnect` accepte un tableau de nombres représentant le délai en millisecondes à attendre avant de commencer chaque tentative de reconnexion.</span><span class="sxs-lookup"><span data-stu-id="f6215-184">In order to configure a custom number of reconnect attempts before disconnecting or change the reconnect timing, `withAutomaticReconnect` accepts an array of numbers representing the delay in milliseconds to wait before starting each reconnect attempt.</span></span>

```javascript
const connection = new signalR.HubConnectionBuilder()
    .withUrl("/chatHub")
    .withAutomaticReconnect([0, 0, 10000])
    .build();

    // .withAutomaticReconnect([0, 2000, 10000, 30000]) yields the default behavior
```

<span data-ttu-id="f6215-185">L’exemple précédent configure le `HubConnection` pour démarrer une tentative de reconnexions immédiatement après la connexion est perdue.</span><span class="sxs-lookup"><span data-stu-id="f6215-185">The preceding example configures the `HubConnection` to start attempting reconnects immediately after the connection is lost.</span></span> <span data-ttu-id="f6215-186">Cela vaut également pour la configuration par défaut.</span><span class="sxs-lookup"><span data-stu-id="f6215-186">This is also true for the default configuration.</span></span>

<span data-ttu-id="f6215-187">Si la première tentative de reconnexion échoue, la deuxième tentative de reconnexion également démarre immédiatement au lieu d’attendre 2 secondes comme il le ferait dans la configuration par défaut.</span><span class="sxs-lookup"><span data-stu-id="f6215-187">If the first reconnect attempt fails, the second reconnect attempt will also start immediately instead of waiting 2 seconds like it would in the default configuration.</span></span>

<span data-ttu-id="f6215-188">Si la deuxième tentative de reconnexion échoue, la troisième tentative de reconnexion démarrera dans 10 secondes, ce qui revient à nouveau à la configuration par défaut.</span><span class="sxs-lookup"><span data-stu-id="f6215-188">If the second reconnect attempt fails, the third reconnect attempt will start in 10 seconds which is again like the default configuration.</span></span>

<span data-ttu-id="f6215-189">Le comportement personnalisé puis diffère à nouveau le comportement par défaut en arrêtant après la reconnexion troisième tentative d’échec au lieu de tenter une reconnexion plus tentative dans un autre 30 secondes comme il le ferait dans la configuration par défaut.</span><span class="sxs-lookup"><span data-stu-id="f6215-189">The custom behavior then diverges again from the default behavior by stopping after the third reconnect attempt failure instead of trying one more reconnect attempt in another 30 seconds like it would in the default configuration.</span></span>

<span data-ttu-id="f6215-190">Si vous souhaitez mieux contrôler la fréquence et le nombre d’automatique nouvelle tentative, `withAutomaticReconnect` accepte un objet qui implémente le `IReconnectPolicy` interface, ce qui a une méthode unique nommée `nextRetryDelayInMilliseconds`.</span><span class="sxs-lookup"><span data-stu-id="f6215-190">If you want even more control over the timing and number of automatic reconnect attempts, `withAutomaticReconnect` accepts an object implementing the `IReconnectPolicy` interface, which has a single method named `nextRetryDelayInMilliseconds`.</span></span>

<span data-ttu-id="f6215-191">`nextRetryDelayInMilliseconds` accepte deux arguments, `previousRetryCount` et `elapsedMilliseconds`, qui sont les deux nombres.</span><span class="sxs-lookup"><span data-stu-id="f6215-191">`nextRetryDelayInMilliseconds` takes two arguments, `previousRetryCount` and `elapsedMilliseconds`, which are both numbers.</span></span> <span data-ttu-id="f6215-192">Avant la première tentative de reconnexion, les deux `previousRetryCount` et `elapsedMilliseconds` sera égal à zéro.</span><span class="sxs-lookup"><span data-stu-id="f6215-192">Before the first reconnect attempt, both `previousRetryCount` and `elapsedMilliseconds` will be zero.</span></span> <span data-ttu-id="f6215-193">Après chaque tentative ayant échoué, `previousRetryCount` sera incrémenté et `elapsedMilliseconds` sera mis à jour pour refléter la quantité de temps passé à la reconnexion jusqu’en millisecondes.</span><span class="sxs-lookup"><span data-stu-id="f6215-193">After each failed retry attempt, `previousRetryCount` will be incremented by one and `elapsedMilliseconds` will be updated to reflect the amount of time spent reconnecting so far in milliseconds.</span></span>

<span data-ttu-id="f6215-194">`nextRetryDelayInMilliseconds` doit retourner un nombre représentant le nombre de millisecondes à attendre avant la prochaine tentative de reconnexion ou `null` si le `HubConnection` doit s’arrêter la reconnexion.</span><span class="sxs-lookup"><span data-stu-id="f6215-194">`nextRetryDelayInMilliseconds` must return either a number representing the number of milliseconds to wait before the next reconnect attempt or `null` if the `HubConnection` should stop reconnecting.</span></span>

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

<span data-ttu-id="f6215-195">Vous pouvez également écrire le code qui se reconnectera votre client manuellement, comme illustré dans [reconnecter manuellement](#manually-reconnect).</span><span class="sxs-lookup"><span data-stu-id="f6215-195">Alternatively, you can write code that will reconnect your client manually as demonstrated in [Manually reconnect](#manually-reconnect).</span></span>

::: moniker-end

### <a name="manually-reconnect"></a><span data-ttu-id="f6215-196">Reconnecter manuellement</span><span class="sxs-lookup"><span data-stu-id="f6215-196">Manually reconnect</span></span>

::: moniker range="< aspnetcore-3.0"

> [!WARNING]
> <span data-ttu-id="f6215-197">Avant la version 3.0, le client JavaScript pour SignalR ne se reconnecter automatiquement.</span><span class="sxs-lookup"><span data-stu-id="f6215-197">Prior to 3.0, the JavaScript client for SignalR doesn't automatically reconnect.</span></span> <span data-ttu-id="f6215-198">Vous devez écrire du code qui se reconnectera votre client manuellement.</span><span class="sxs-lookup"><span data-stu-id="f6215-198">You must write code that will reconnect your client manually.</span></span>

::: moniker-end

<span data-ttu-id="f6215-199">Le code suivant illustre une approche classique de reconnexion manuelle :</span><span class="sxs-lookup"><span data-stu-id="f6215-199">The following code demonstrates a typical manual reconnection approach:</span></span>

1. <span data-ttu-id="f6215-200">Une fonction (dans ce cas, le `start` (fonction)) est créé pour démarrer la connexion.</span><span class="sxs-lookup"><span data-stu-id="f6215-200">A function (in this case, the `start` function) is created to start the connection.</span></span>
1. <span data-ttu-id="f6215-201">Appelez le `start` (fonction) dans la connexion `onclose` Gestionnaire d’événements.</span><span class="sxs-lookup"><span data-stu-id="f6215-201">Call the `start` function in the connection's `onclose` event handler.</span></span>

[!code-javascript[Reconnect the JavaScript client](javascript-client/sample/wwwroot/js/chat.js?range=28-40)]

<span data-ttu-id="f6215-202">Une implémentation réelle serait utiliser une temporisation exponentielle ou réessayer un nombre spécifié de fois avant d’abandonner.</span><span class="sxs-lookup"><span data-stu-id="f6215-202">A real-world implementation would use an exponential back-off or retry a specified number of times before giving up.</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="f6215-203">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="f6215-203">Additional resources</span></span>

* [<span data-ttu-id="f6215-204">Référence API JavaScript</span><span class="sxs-lookup"><span data-stu-id="f6215-204">JavaScript API reference</span></span>](/javascript/api/?view=signalr-js-latest)
* [<span data-ttu-id="f6215-205">Didacticiel de JavaScript</span><span class="sxs-lookup"><span data-stu-id="f6215-205">JavaScript tutorial</span></span>](xref:tutorials/signalr)
* [<span data-ttu-id="f6215-206">Didacticiel WebPack et TypeScript</span><span class="sxs-lookup"><span data-stu-id="f6215-206">WebPack and TypeScript tutorial</span></span>](xref:tutorials/signalr-typescript-webpack)
* [<span data-ttu-id="f6215-207">Hubs</span><span class="sxs-lookup"><span data-stu-id="f6215-207">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="f6215-208">Client .NET</span><span class="sxs-lookup"><span data-stu-id="f6215-208">.NET client</span></span>](xref:signalr/dotnet-client)
* [<span data-ttu-id="f6215-209">Publier sur Azure</span><span class="sxs-lookup"><span data-stu-id="f6215-209">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)
* [<span data-ttu-id="f6215-210">Demandes de cross-Origin (CORS)</span><span class="sxs-lookup"><span data-stu-id="f6215-210">Cross-Origin Requests (CORS)</span></span>](xref:security/cors)
* [<span data-ttu-id="f6215-211">Documentation de serverless Azure SignalR Service</span><span class="sxs-lookup"><span data-stu-id="f6215-211">Azure SignalR Service serverless documentation</span></span>](/azure/azure-signalr/signalr-concept-serverless-development-config)
