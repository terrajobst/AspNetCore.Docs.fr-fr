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
ms.openlocfilehash: 3086b4aa532dfe992e19c193ef76f216f7835164
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78657856"
---
# <a name="aspnet-core-opno-locsignalr-javascript-client"></a><span data-ttu-id="760cb-103">ASP.NET Core SignalR client JavaScript</span><span class="sxs-lookup"><span data-stu-id="760cb-103">ASP.NET Core SignalR JavaScript client</span></span>

<span data-ttu-id="760cb-104">Par [Rachel Appel](https://twitter.com/rachelappel)</span><span class="sxs-lookup"><span data-stu-id="760cb-104">By [Rachel Appel](https://twitter.com/rachelappel)</span></span>

<span data-ttu-id="760cb-105">La bibliothèque cliente ASP.NET Core SignalR JavaScript permet aux développeurs d’appeler du code de concentrateur côté serveur.</span><span class="sxs-lookup"><span data-stu-id="760cb-105">The ASP.NET Core SignalR JavaScript client library enables developers to call server-side hub code.</span></span>

<span data-ttu-id="760cb-106">[Affichez ou téléchargez l’exemple de code](https://github.com/dotnet/AspNetCore.Docs/tree/live/aspnetcore/signalr/javascript-client/sample) ([procédure de téléchargement](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="760cb-106">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/live/aspnetcore/signalr/javascript-client/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="install-the-opno-locsignalr-client-package"></a><span data-ttu-id="760cb-107">Installer le package client SignalR</span><span class="sxs-lookup"><span data-stu-id="760cb-107">Install the SignalR client package</span></span>

<span data-ttu-id="760cb-108">La bibliothèque cliente JavaScript SignalR est fournie sous la forme d’un package [NPM](https://www.npmjs.com/) .</span><span class="sxs-lookup"><span data-stu-id="760cb-108">The SignalR JavaScript client library is delivered as an [npm](https://www.npmjs.com/) package.</span></span> <span data-ttu-id="760cb-109">Si vous utilisez Visual Studio, exécutez `npm install` à partir de la **console du gestionnaire de package** dans le dossier racine.</span><span class="sxs-lookup"><span data-stu-id="760cb-109">If you're using Visual Studio, run `npm install` from the **Package Manager Console** while in the root folder.</span></span> <span data-ttu-id="760cb-110">Pour Visual Studio Code, exécutez la commande à partir du **Terminal intégré**.</span><span class="sxs-lookup"><span data-stu-id="760cb-110">For Visual Studio Code, run the command from the **Integrated Terminal**.</span></span>

::: moniker range=">= aspnetcore-3.0"

  ```console
  npm init -y
  npm install @microsoft/signalr
  ```

<span data-ttu-id="760cb-111">NPM installe le contenu du package dans le dossier *node_modules\\@microsoft\signalr\dist\browser* .</span><span class="sxs-lookup"><span data-stu-id="760cb-111">npm installs the package contents in the *node_modules\\@microsoft\signalr\dist\browser* folder.</span></span> <span data-ttu-id="760cb-112">Créez un nouveau dossier nommé *signalr* sous le dossier *wwwroot\\lib* .</span><span class="sxs-lookup"><span data-stu-id="760cb-112">Create a new folder named *signalr* under the *wwwroot\\lib* folder.</span></span> <span data-ttu-id="760cb-113">Copiez le fichier *signalr. js* dans le dossier *wwwroot\lib\signalr* .</span><span class="sxs-lookup"><span data-stu-id="760cb-113">Copy the *signalr.js* file to the *wwwroot\lib\signalr* folder.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

  ```console
  npm init -y
  npm install @aspnet/signalr
  ```

<span data-ttu-id="760cb-114">NPM installe le contenu du package dans le dossier *node_modules\\@aspnet\signalr\dist\browser* .</span><span class="sxs-lookup"><span data-stu-id="760cb-114">npm installs the package contents in the *node_modules\\@aspnet\signalr\dist\browser* folder.</span></span> <span data-ttu-id="760cb-115">Créez un nouveau dossier nommé *signalr* sous le dossier *wwwroot\\lib* .</span><span class="sxs-lookup"><span data-stu-id="760cb-115">Create a new folder named *signalr* under the *wwwroot\\lib* folder.</span></span> <span data-ttu-id="760cb-116">Copiez le fichier *signalr. js* dans le dossier *wwwroot\lib\signalr* .</span><span class="sxs-lookup"><span data-stu-id="760cb-116">Copy the *signalr.js* file to the *wwwroot\lib\signalr* folder.</span></span>

::: moniker-end

## <a name="use-the-opno-locsignalr-javascript-client"></a><span data-ttu-id="760cb-117">Utiliser le client JavaScript SignalR</span><span class="sxs-lookup"><span data-stu-id="760cb-117">Use the SignalR JavaScript client</span></span>

<span data-ttu-id="760cb-118">Référencez le client JavaScript SignalR dans l’élément `<script>`.</span><span class="sxs-lookup"><span data-stu-id="760cb-118">Reference the SignalR JavaScript client in the `<script>` element.</span></span>

```html
<script src="~/lib/signalr/signalr.js"></script>
```

## <a name="connect-to-a-hub"></a><span data-ttu-id="760cb-119">Se connecter à un hub</span><span class="sxs-lookup"><span data-stu-id="760cb-119">Connect to a hub</span></span>

<span data-ttu-id="760cb-120">Le code suivant crée et démarre une connexion.</span><span class="sxs-lookup"><span data-stu-id="760cb-120">The following code creates and starts a connection.</span></span> <span data-ttu-id="760cb-121">Le nom du concentrateur ne respecte pas la casse.</span><span class="sxs-lookup"><span data-stu-id="760cb-121">The hub's name is case insensitive.</span></span>

[!code-javascript[Call hub methods](javascript-client/sample/wwwroot/js/chat.js?range=9-13,43-45)]

### <a name="cross-origin-connections"></a><span data-ttu-id="760cb-122">Connexions cross-origin</span><span class="sxs-lookup"><span data-stu-id="760cb-122">Cross-origin connections</span></span>

<span data-ttu-id="760cb-123">En règle générale, les navigateurs chargent les connexions à partir du même domaine que la page demandée.</span><span class="sxs-lookup"><span data-stu-id="760cb-123">Typically, browsers load connections from the same domain as the requested page.</span></span> <span data-ttu-id="760cb-124">Toutefois, il existe des cas où une connexion à un autre domaine est nécessaire.</span><span class="sxs-lookup"><span data-stu-id="760cb-124">However, there are occasions when a connection to another domain is required.</span></span>

<span data-ttu-id="760cb-125">Pour empêcher un site malveillant de lire des données sensibles à partir d’un autre site, les [connexions Cross-Origin](xref:security/cors) sont désactivées par défaut.</span><span class="sxs-lookup"><span data-stu-id="760cb-125">To prevent a malicious site from reading sensitive data from another site, [cross-origin connections](xref:security/cors) are disabled by default.</span></span> <span data-ttu-id="760cb-126">Pour autoriser une demande Cross-Origin, activez-la dans la classe `Startup`.</span><span class="sxs-lookup"><span data-stu-id="760cb-126">To allow a cross-origin request, enable it in the `Startup` class.</span></span>

[!code-csharp[Cross-origin connections](javascript-client/sample/Startup.cs?highlight=29-35,56)]

## <a name="call-hub-methods-from-client"></a><span data-ttu-id="760cb-127">Appeler des méthodes de hub à partir du client</span><span class="sxs-lookup"><span data-stu-id="760cb-127">Call hub methods from client</span></span>

<span data-ttu-id="760cb-128">Les clients JavaScript appellent des méthodes publiques sur des hubs à l’aide de la méthode [Invoke](/javascript/api/%40aspnet/signalr/hubconnection#invoke) de [HubConnection](/javascript/api/%40aspnet/signalr/hubconnection).</span><span class="sxs-lookup"><span data-stu-id="760cb-128">JavaScript clients call public methods on hubs via the [invoke](/javascript/api/%40aspnet/signalr/hubconnection#invoke) method of the [HubConnection](/javascript/api/%40aspnet/signalr/hubconnection).</span></span> <span data-ttu-id="760cb-129">La méthode `invoke` accepte deux arguments :</span><span class="sxs-lookup"><span data-stu-id="760cb-129">The `invoke` method accepts two arguments:</span></span>

* <span data-ttu-id="760cb-130">Le nom de la méthode de hub.</span><span class="sxs-lookup"><span data-stu-id="760cb-130">The name of the hub method.</span></span> <span data-ttu-id="760cb-131">Dans l’exemple suivant, le nom de la méthode sur le Hub est `SendMessage`.</span><span class="sxs-lookup"><span data-stu-id="760cb-131">In the following example, the method name on the hub is `SendMessage`.</span></span>
* <span data-ttu-id="760cb-132">Tous les arguments définis dans la méthode de hub.</span><span class="sxs-lookup"><span data-stu-id="760cb-132">Any arguments defined in the hub method.</span></span> <span data-ttu-id="760cb-133">Dans l’exemple suivant, le nom de l’argument est `message`.</span><span class="sxs-lookup"><span data-stu-id="760cb-133">In the following example, the argument name is `message`.</span></span> <span data-ttu-id="760cb-134">L’exemple de code utilise la syntaxe de fonction Arrow qui est prise en charge dans les versions actuelles de tous les principaux navigateurs, à l’exception d’Internet Explorer.</span><span class="sxs-lookup"><span data-stu-id="760cb-134">The example code uses arrow function syntax that is supported in current versions of all major browsers except Internet Explorer.</span></span>

  [!code-javascript[Call hub methods](javascript-client/sample/wwwroot/js/chat.js?range=24)]

> [!NOTE]
> <span data-ttu-id="760cb-135">Si vous utilisez le service Azure SignalR en *mode sans serveur*, vous ne pouvez pas appeler les méthodes de concentrateur à partir d’un client.</span><span class="sxs-lookup"><span data-stu-id="760cb-135">If you're using Azure SignalR Service in *Serverless mode*, you cannot call hub methods from a client.</span></span> <span data-ttu-id="760cb-136">Pour plus d’informations, consultez la [documentation du ServiceSignalR](/azure/azure-signalr/signalr-concept-serverless-development-config).</span><span class="sxs-lookup"><span data-stu-id="760cb-136">For more information, see the [SignalR Service documentation](/azure/azure-signalr/signalr-concept-serverless-development-config).</span></span>

<span data-ttu-id="760cb-137">La méthode `invoke` retourne une [promesse](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Promise)JavaScript.</span><span class="sxs-lookup"><span data-stu-id="760cb-137">The `invoke` method returns a JavaScript [Promise](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Promise).</span></span> <span data-ttu-id="760cb-138">La `Promise` est résolue avec la valeur de retour (le cas échéant) lorsque la méthode sur le serveur retourne.</span><span class="sxs-lookup"><span data-stu-id="760cb-138">The `Promise` is resolved with the return value (if any) when the method on the server returns.</span></span> <span data-ttu-id="760cb-139">Si la méthode sur le serveur génère une erreur, le `Promise` est rejeté avec le message d’erreur.</span><span class="sxs-lookup"><span data-stu-id="760cb-139">If the method on the server throws an error, the `Promise` is rejected with the error message.</span></span> <span data-ttu-id="760cb-140">Utilisez les méthodes `then` et `catch` sur le `Promise` lui-même pour gérer ces cas (ou `await` syntaxe).</span><span class="sxs-lookup"><span data-stu-id="760cb-140">Use the `then` and `catch` methods on the `Promise` itself to handle these cases (or `await` syntax).</span></span>

<span data-ttu-id="760cb-141">La méthode `send` retourne un `Promise`JavaScript.</span><span class="sxs-lookup"><span data-stu-id="760cb-141">The `send` method returns a JavaScript `Promise`.</span></span> <span data-ttu-id="760cb-142">La `Promise` est résolue lorsque le message a été envoyé au serveur.</span><span class="sxs-lookup"><span data-stu-id="760cb-142">The `Promise` is resolved when the message has been sent to the server.</span></span> <span data-ttu-id="760cb-143">En cas d’erreur lors de l’envoi du message, le `Promise` est rejeté avec le message d’erreur.</span><span class="sxs-lookup"><span data-stu-id="760cb-143">If there is an error sending the message, the `Promise` is rejected with the error message.</span></span> <span data-ttu-id="760cb-144">Utilisez les méthodes `then` et `catch` sur le `Promise` lui-même pour gérer ces cas (ou `await` syntaxe).</span><span class="sxs-lookup"><span data-stu-id="760cb-144">Use the `then` and `catch` methods on the `Promise` itself to handle these cases (or `await` syntax).</span></span>

> [!NOTE]
> <span data-ttu-id="760cb-145">L’utilisation de `send` n’attend pas que le serveur ait reçu le message.</span><span class="sxs-lookup"><span data-stu-id="760cb-145">Using `send` doesn't wait until the server has received the message.</span></span> <span data-ttu-id="760cb-146">Par conséquent, il n’est pas possible de retourner des données ou des erreurs à partir du serveur.</span><span class="sxs-lookup"><span data-stu-id="760cb-146">Consequently, it's not possible to return data or errors from the server.</span></span>

## <a name="call-client-methods-from-hub"></a><span data-ttu-id="760cb-147">Appeler des méthodes clientes à partir du Hub</span><span class="sxs-lookup"><span data-stu-id="760cb-147">Call client methods from hub</span></span>

<span data-ttu-id="760cb-148">Pour recevoir des messages à partir du concentrateur, définissez une méthode à l’aide de la méthode [on](/javascript/api/%40aspnet/signalr/hubconnection#on) de l' `HubConnection`.</span><span class="sxs-lookup"><span data-stu-id="760cb-148">To receive messages from the hub, define a method using the [on](/javascript/api/%40aspnet/signalr/hubconnection#on) method of the `HubConnection`.</span></span>

* <span data-ttu-id="760cb-149">Nom de la méthode du client JavaScript.</span><span class="sxs-lookup"><span data-stu-id="760cb-149">The name of the JavaScript client method.</span></span> <span data-ttu-id="760cb-150">Dans l’exemple suivant, le nom de la méthode est `ReceiveMessage`.</span><span class="sxs-lookup"><span data-stu-id="760cb-150">In the following example, the method name is `ReceiveMessage`.</span></span>
* <span data-ttu-id="760cb-151">Les arguments que le hub passe à la méthode.</span><span class="sxs-lookup"><span data-stu-id="760cb-151">Arguments the hub passes to the method.</span></span> <span data-ttu-id="760cb-152">Dans l’exemple suivant, la valeur de l’argument est `message`.</span><span class="sxs-lookup"><span data-stu-id="760cb-152">In the following example, the argument value is `message`.</span></span>

[!code-javascript[Receive calls from hub](javascript-client/sample/wwwroot/js/chat.js?range=14-19)]

<span data-ttu-id="760cb-153">Le code précédent dans `connection.on` s’exécute lorsque le code côté serveur l’appelle à l’aide de la méthode [SendAsync](/dotnet/api/microsoft.aspnetcore.signalr.clientproxyextensions.sendasync) .</span><span class="sxs-lookup"><span data-stu-id="760cb-153">The preceding code in `connection.on` runs when server-side code calls it using the [SendAsync](/dotnet/api/microsoft.aspnetcore.signalr.clientproxyextensions.sendasync) method.</span></span>

[!code-csharp[Call client-side](javascript-client/sample/hubs/chathub.cs?range=8-11)]

SignalR<span data-ttu-id="760cb-154"> détermine la méthode cliente à appeler en faisant correspondre le nom de la méthode et les arguments définis dans `SendAsync` et `connection.on`.</span><span class="sxs-lookup"><span data-stu-id="760cb-154"> determines which client method to call by matching the method name and arguments defined in `SendAsync` and `connection.on`.</span></span>

> [!NOTE]
> <span data-ttu-id="760cb-155">Il est recommandé d’appeler la méthode [Start](/javascript/api/%40aspnet/signalr/hubconnection#start) sur le `HubConnection` après `on`.</span><span class="sxs-lookup"><span data-stu-id="760cb-155">As a best practice, call the [start](/javascript/api/%40aspnet/signalr/hubconnection#start) method on the `HubConnection` after `on`.</span></span> <span data-ttu-id="760cb-156">En procédant comme ceci, vos gestionnaires sont enregistrés avant que tous les messages soient reçus.</span><span class="sxs-lookup"><span data-stu-id="760cb-156">Doing so ensures your handlers are registered before any messages are received.</span></span>

## <a name="error-handling-and-logging"></a><span data-ttu-id="760cb-157">Gestion et journalisation des erreurs</span><span class="sxs-lookup"><span data-stu-id="760cb-157">Error handling and logging</span></span>

<span data-ttu-id="760cb-158">Chaînez une méthode `catch` à la fin de la méthode `start` pour gérer les erreurs côté client.</span><span class="sxs-lookup"><span data-stu-id="760cb-158">Chain a `catch` method to the end of the `start` method to handle client-side errors.</span></span> <span data-ttu-id="760cb-159">Utilisez `console.error` pour générer des erreurs dans la console du navigateur.</span><span class="sxs-lookup"><span data-stu-id="760cb-159">Use `console.error` to output errors to the browser's console.</span></span>

[!code-javascript[Error handling](javascript-client/sample/wwwroot/js/chat.js?range=49-51)]

<span data-ttu-id="760cb-160">Configurez le suivi des journaux côté client en passant un enregistreur d’événements et un type d’événement à consigner lorsque la connexion est établie.</span><span class="sxs-lookup"><span data-stu-id="760cb-160">Setup client-side log tracing by passing a logger and type of event to log when the connection is made.</span></span> <span data-ttu-id="760cb-161">Les messages sont enregistrés avec le niveau de journalisation spécifié et supérieur.</span><span class="sxs-lookup"><span data-stu-id="760cb-161">Messages are logged with the specified log level and higher.</span></span> <span data-ttu-id="760cb-162">Les niveaux de journalisation disponibles sont les suivants :</span><span class="sxs-lookup"><span data-stu-id="760cb-162">Available log levels are as follows:</span></span>

* <span data-ttu-id="760cb-163">`signalR.LogLevel.Error` &ndash; des messages d’erreur.</span><span class="sxs-lookup"><span data-stu-id="760cb-163">`signalR.LogLevel.Error` &ndash; Error messages.</span></span> <span data-ttu-id="760cb-164">Journalise uniquement les messages `Error`.</span><span class="sxs-lookup"><span data-stu-id="760cb-164">Logs `Error` messages only.</span></span>
* <span data-ttu-id="760cb-165">`signalR.LogLevel.Warning` &ndash; des messages d’avertissement sur les erreurs potentielles.</span><span class="sxs-lookup"><span data-stu-id="760cb-165">`signalR.LogLevel.Warning` &ndash; Warning messages about potential errors.</span></span> <span data-ttu-id="760cb-166">Journalise les messages `Warning`et `Error`.</span><span class="sxs-lookup"><span data-stu-id="760cb-166">Logs `Warning`, and `Error` messages.</span></span>
* <span data-ttu-id="760cb-167">`signalR.LogLevel.Information` &ndash; messages d’État sans erreurs.</span><span class="sxs-lookup"><span data-stu-id="760cb-167">`signalR.LogLevel.Information` &ndash; Status messages without errors.</span></span> <span data-ttu-id="760cb-168">Journalise les messages `Information`, `Warning`et `Error`.</span><span class="sxs-lookup"><span data-stu-id="760cb-168">Logs `Information`, `Warning`, and `Error` messages.</span></span>
* <span data-ttu-id="760cb-169">`signalR.LogLevel.Trace` &ndash; des messages de suivi.</span><span class="sxs-lookup"><span data-stu-id="760cb-169">`signalR.LogLevel.Trace` &ndash; Trace messages.</span></span> <span data-ttu-id="760cb-170">Journalise tout, y compris les données transférées entre le Hub et le client.</span><span class="sxs-lookup"><span data-stu-id="760cb-170">Logs everything, including data transported between hub and client.</span></span>

<span data-ttu-id="760cb-171">Utilisez la méthode [configureLogging](/javascript/api/%40aspnet/signalr/hubconnectionbuilder#configurelogging) sur [HubConnectionBuilder](/javascript/api/%40aspnet/signalr/hubconnectionbuilder) pour configurer le niveau de journalisation.</span><span class="sxs-lookup"><span data-stu-id="760cb-171">Use the [configureLogging](/javascript/api/%40aspnet/signalr/hubconnectionbuilder#configurelogging) method on [HubConnectionBuilder](/javascript/api/%40aspnet/signalr/hubconnectionbuilder) to configure the log level.</span></span> <span data-ttu-id="760cb-172">Les messages sont enregistrés dans la console du navigateur.</span><span class="sxs-lookup"><span data-stu-id="760cb-172">Messages are logged to the browser console.</span></span>

[!code-javascript[Logging levels](javascript-client/sample/wwwroot/js/chat.js?range=9-12)]

## <a name="reconnect-clients"></a><span data-ttu-id="760cb-173">Reconnecter les clients</span><span class="sxs-lookup"><span data-stu-id="760cb-173">Reconnect clients</span></span>

::: moniker range=">= aspnetcore-3.0"

### <a name="automatically-reconnect"></a><span data-ttu-id="760cb-174">Se reconnecter automatiquement</span><span class="sxs-lookup"><span data-stu-id="760cb-174">Automatically reconnect</span></span>

<span data-ttu-id="760cb-175">Le client JavaScript pour SignalR peut être configuré pour se reconnecter automatiquement à l’aide de la méthode `withAutomaticReconnect` sur [HubConnectionBuilder](/javascript/api/%40aspnet/signalr/hubconnectionbuilder).</span><span class="sxs-lookup"><span data-stu-id="760cb-175">The JavaScript client for SignalR can be configured to automatically reconnect using the `withAutomaticReconnect` method on [HubConnectionBuilder](/javascript/api/%40aspnet/signalr/hubconnectionbuilder).</span></span> <span data-ttu-id="760cb-176">Par défaut, il ne se reconnectera pas automatiquement.</span><span class="sxs-lookup"><span data-stu-id="760cb-176">It won't automatically reconnect by default.</span></span>

```javascript
const connection = new signalR.HubConnectionBuilder()
    .withUrl("/chatHub")
    .withAutomaticReconnect()
    .build();
```

<span data-ttu-id="760cb-177">Sans aucun paramètre, `withAutomaticReconnect()` configure le client pour attendre 0, 2, 10 et 30 secondes, respectivement, avant d’essayer chaque tentative de reconnexion, en s’arrêtant après quatre tentatives ayant échoué.</span><span class="sxs-lookup"><span data-stu-id="760cb-177">Without any parameters, `withAutomaticReconnect()` configures the client to wait 0, 2, 10, and 30 seconds respectively before trying each reconnect attempt, stopping after four failed attempts.</span></span>

<span data-ttu-id="760cb-178">Avant de commencer les tentatives de reconnexion, le `HubConnection` passe à l’État `HubConnectionState.Reconnecting` et déclenche ses rappels `onreconnecting` au lieu de passer à l’État `Disconnected` et de déclencher ses rappels de `onclose` comme un `HubConnection` sans que la reconnexion automatique soit configurée.</span><span class="sxs-lookup"><span data-stu-id="760cb-178">Before starting any reconnect attempts, the `HubConnection` will transition to the `HubConnectionState.Reconnecting` state and fire its `onreconnecting` callbacks instead of transitioning to the `Disconnected` state and triggering its `onclose` callbacks like a `HubConnection` without automatic reconnect configured.</span></span> <span data-ttu-id="760cb-179">Cela permet d’avertir les utilisateurs que la connexion a été perdue et de désactiver les éléments d’interface utilisateur.</span><span class="sxs-lookup"><span data-stu-id="760cb-179">This provides an opportunity to warn users that the connection has been lost and to disable UI elements.</span></span>

```javascript
connection.onreconnecting((error) => {
    console.assert(connection.state === signalR.HubConnectionState.Reconnecting);

    document.getElementById("messageInput").disabled = true;

    const li = document.createElement("li");
    li.textContent = `Connection lost due to error "${error}". Reconnecting.`;
    document.getElementById("messagesList").appendChild(li);
});
```

<span data-ttu-id="760cb-180">Si le client se reconnecte avec succès dans les quatre premières tentatives, le `HubConnection` passe à l’État `Connected` et active ses rappels `onreconnected`.</span><span class="sxs-lookup"><span data-stu-id="760cb-180">If the client successfully reconnects within its first four attempts, the `HubConnection` will transition back to the `Connected` state and fire its `onreconnected` callbacks.</span></span> <span data-ttu-id="760cb-181">Cela permet d’informer les utilisateurs que la connexion a été rétablie.</span><span class="sxs-lookup"><span data-stu-id="760cb-181">This provides an opportunity to inform users the connection has been reestablished.</span></span>

<span data-ttu-id="760cb-182">Étant donné que la connexion semble entièrement nouvelle sur le serveur, une nouvelle `connectionId` sera fournie au rappel `onreconnected`.</span><span class="sxs-lookup"><span data-stu-id="760cb-182">Since the connection looks entirely new to the server, a new `connectionId` will be provided to the `onreconnected` callback.</span></span>

> [!WARNING]
> <span data-ttu-id="760cb-183">Le paramètre de `connectionId` du rappel `onreconnected` n’est pas défini si le `HubConnection` a été configuré pour [ignorer la négociation](xref:signalr/configuration#configure-client-options).</span><span class="sxs-lookup"><span data-stu-id="760cb-183">The `onreconnected` callback's `connectionId` parameter will be undefined if the `HubConnection` was configured to [skip negotiation](xref:signalr/configuration#configure-client-options).</span></span>

```javascript
connection.onreconnected((connectionId) => {
    console.assert(connection.state === signalR.HubConnectionState.Connected);

    document.getElementById("messageInput").disabled = false;

    const li = document.createElement("li");
    li.textContent = `Connection reestablished. Connected with connectionId "${connectionId}".`;
    document.getElementById("messagesList").appendChild(li);
});
```

<span data-ttu-id="760cb-184">`withAutomaticReconnect()` ne configure pas le `HubConnection` pour réessayer les échecs de démarrage initial, les échecs de démarrage doivent donc être gérés manuellement :</span><span class="sxs-lookup"><span data-stu-id="760cb-184">`withAutomaticReconnect()` won't configure the `HubConnection` to retry initial start failures, so start failures need to be handled manually:</span></span>

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

<span data-ttu-id="760cb-185">Si le client ne se reconnecte pas correctement dans les quatre premières tentatives, le `HubConnection` passera à l’État `Disconnected` et déclenchera ses rappels [OnClose](/javascript/api/%40aspnet/signalr/hubconnection#onclose) .</span><span class="sxs-lookup"><span data-stu-id="760cb-185">If the client doesn't successfully reconnect within its first four attempts, the `HubConnection` will transition to the `Disconnected` state and fire its [onclose](/javascript/api/%40aspnet/signalr/hubconnection#onclose) callbacks.</span></span> <span data-ttu-id="760cb-186">Cela permet d’informer les utilisateurs que la connexion a été perdue définitivement et de recommander l’actualisation de la page :</span><span class="sxs-lookup"><span data-stu-id="760cb-186">This provides an opportunity to inform users the connection has been permanently lost and recommend refreshing the page:</span></span>

```javascript
connection.onclose((error) => {
    console.assert(connection.state === signalR.HubConnectionState.Disconnected);

    document.getElementById("messageInput").disabled = true;

    const li = document.createElement("li");
    li.textContent = `Connection closed due to error "${error}". Try refreshing this page to restart the connection.`;
    document.getElementById("messagesList").appendChild(li);
});
```

<span data-ttu-id="760cb-187">Pour configurer un nombre personnalisé de tentatives de reconnexion avant de vous déconnecter ou de modifier le délai de reconnexion, `withAutomaticReconnect` accepte un tableau de nombres représentant le délai en millisecondes à attendre avant de démarrer chaque tentative de reconnexion.</span><span class="sxs-lookup"><span data-stu-id="760cb-187">In order to configure a custom number of reconnect attempts before disconnecting or change the reconnect timing, `withAutomaticReconnect` accepts an array of numbers representing the delay in milliseconds to wait before starting each reconnect attempt.</span></span>

```javascript
const connection = new signalR.HubConnectionBuilder()
    .withUrl("/chatHub")
    .withAutomaticReconnect([0, 0, 10000])
    .build();

    // .withAutomaticReconnect([0, 2000, 10000, 30000]) yields the default behavior
```

<span data-ttu-id="760cb-188">L’exemple précédent configure le `HubConnection` pour commencer à tenter de se reconnecter immédiatement après la perte de la connexion.</span><span class="sxs-lookup"><span data-stu-id="760cb-188">The preceding example configures the `HubConnection` to start attempting reconnects immediately after the connection is lost.</span></span> <span data-ttu-id="760cb-189">Cela est également vrai pour la configuration par défaut.</span><span class="sxs-lookup"><span data-stu-id="760cb-189">This is also true for the default configuration.</span></span>

<span data-ttu-id="760cb-190">Si la première tentative de reconnexion échoue, la deuxième tentative de reconnexion démarre également immédiatement au lieu d’attendre 2 secondes comme dans la configuration par défaut.</span><span class="sxs-lookup"><span data-stu-id="760cb-190">If the first reconnect attempt fails, the second reconnect attempt will also start immediately instead of waiting 2 seconds like it would in the default configuration.</span></span>

<span data-ttu-id="760cb-191">Si la deuxième tentative de reconnexion échoue, la troisième tentative de reconnexion démarrera dans 10 secondes, ce qui revient à la configuration par défaut.</span><span class="sxs-lookup"><span data-stu-id="760cb-191">If the second reconnect attempt fails, the third reconnect attempt will start in 10 seconds which is again like the default configuration.</span></span>

<span data-ttu-id="760cb-192">Le comportement personnalisé divergent ensuite du comportement par défaut en s’arrêtant après le troisième échec de tentative de reconnexion au lieu d’essayer une nouvelle tentative de reconnexion dans 30 secondes, comme dans la configuration par défaut.</span><span class="sxs-lookup"><span data-stu-id="760cb-192">The custom behavior then diverges again from the default behavior by stopping after the third reconnect attempt failure instead of trying one more reconnect attempt in another 30 seconds like it would in the default configuration.</span></span>

<span data-ttu-id="760cb-193">Si vous souhaitez encore plus de contrôle sur le minutage et le nombre de tentatives de reconnexion automatique, `withAutomaticReconnect` accepte un objet qui implémente l’interface `IRetryPolicy`, qui a une méthode unique nommée `nextRetryDelayInMilliseconds`.</span><span class="sxs-lookup"><span data-stu-id="760cb-193">If you want even more control over the timing and number of automatic reconnect attempts, `withAutomaticReconnect` accepts an object implementing the `IRetryPolicy` interface, which has a single method named `nextRetryDelayInMilliseconds`.</span></span>

<span data-ttu-id="760cb-194">`nextRetryDelayInMilliseconds` accepte un seul argument avec le type `RetryContext`.</span><span class="sxs-lookup"><span data-stu-id="760cb-194">`nextRetryDelayInMilliseconds` takes a single argument with the type `RetryContext`.</span></span> <span data-ttu-id="760cb-195">La `RetryContext` a trois propriétés : `previousRetryCount`, `elapsedMilliseconds` et `retryReason` qui sont une `number`, une `number` et une `Error` respectivement.</span><span class="sxs-lookup"><span data-stu-id="760cb-195">The `RetryContext` has three properties: `previousRetryCount`, `elapsedMilliseconds` and `retryReason` which are a `number`, a `number` and an `Error` respectively.</span></span> <span data-ttu-id="760cb-196">Avant la première tentative de reconnexion, les `previousRetryCount` et `elapsedMilliseconds` sont nuls, et le `retryReason` est l’erreur qui a provoqué la perte de la connexion.</span><span class="sxs-lookup"><span data-stu-id="760cb-196">Before the first reconnect attempt, both `previousRetryCount` and `elapsedMilliseconds` will be zero, and the `retryReason` will be the Error that caused the connection to be lost.</span></span> <span data-ttu-id="760cb-197">Après chaque nouvelle tentative d’échec, `previousRetryCount` sera incrémenté d’une unité, `elapsedMilliseconds` sera mis à jour pour refléter le temps passé à se reconnecter jusqu’à présent en millisecondes, et la `retryReason` sera l’erreur qui a provoqué l’échec de la dernière tentative de reconnexion.</span><span class="sxs-lookup"><span data-stu-id="760cb-197">After each failed retry attempt, `previousRetryCount` will be incremented by one, `elapsedMilliseconds` will be updated to reflect the amount of time spent reconnecting so far in milliseconds, and the `retryReason` will be the Error that caused the last reconnect attempt to fail.</span></span>

<span data-ttu-id="760cb-198">`nextRetryDelayInMilliseconds` doit retourner un nombre représentant le nombre de millisecondes à attendre avant la tentative de reconnexion suivante ou `null` si le `HubConnection` doit s’arrêter de se reconnecter.</span><span class="sxs-lookup"><span data-stu-id="760cb-198">`nextRetryDelayInMilliseconds` must return either a number representing the number of milliseconds to wait before the next reconnect attempt or `null` if the `HubConnection` should stop reconnecting.</span></span>

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
        }
    })
    .build();
```

<span data-ttu-id="760cb-199">Vous pouvez également écrire du code qui reconnectera votre client manuellement, comme illustré dans [reconnexion manuelle](#manually-reconnect).</span><span class="sxs-lookup"><span data-stu-id="760cb-199">Alternatively, you can write code that will reconnect your client manually as demonstrated in [Manually reconnect](#manually-reconnect).</span></span>

::: moniker-end

### <a name="manually-reconnect"></a><span data-ttu-id="760cb-200">Reconnexion manuelle</span><span class="sxs-lookup"><span data-stu-id="760cb-200">Manually reconnect</span></span>

::: moniker range="< aspnetcore-3.0"

> [!WARNING]
> <span data-ttu-id="760cb-201">Avant le 3,0, le client JavaScript pour SignalR ne se reconnecte pas automatiquement.</span><span class="sxs-lookup"><span data-stu-id="760cb-201">Prior to 3.0, the JavaScript client for SignalR doesn't automatically reconnect.</span></span> <span data-ttu-id="760cb-202">Vous devez écrire du code qui reconnectera votre client manuellement.</span><span class="sxs-lookup"><span data-stu-id="760cb-202">You must write code that will reconnect your client manually.</span></span>

::: moniker-end

<span data-ttu-id="760cb-203">Le code suivant illustre une approche de reconnexion manuelle classique :</span><span class="sxs-lookup"><span data-stu-id="760cb-203">The following code demonstrates a typical manual reconnection approach:</span></span>

1. <span data-ttu-id="760cb-204">Une fonction (dans ce cas, la fonction `start`) est créée pour démarrer la connexion.</span><span class="sxs-lookup"><span data-stu-id="760cb-204">A function (in this case, the `start` function) is created to start the connection.</span></span>
1. <span data-ttu-id="760cb-205">Appelez la fonction `start` dans le gestionnaire d’événements `onclose` de la connexion.</span><span class="sxs-lookup"><span data-stu-id="760cb-205">Call the `start` function in the connection's `onclose` event handler.</span></span>

[!code-javascript[Reconnect the JavaScript client](javascript-client/sample/wwwroot/js/chat.js?range=28-40)]

<span data-ttu-id="760cb-206">Une implémentation réelle utilise une interruption exponentielle ou une nouvelle tentative un nombre spécifié de fois avant d’abandonner.</span><span class="sxs-lookup"><span data-stu-id="760cb-206">A real-world implementation would use an exponential back-off or retry a specified number of times before giving up.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="760cb-207">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="760cb-207">Additional resources</span></span>

* [<span data-ttu-id="760cb-208">Référence de l’API JavaScript</span><span class="sxs-lookup"><span data-stu-id="760cb-208">JavaScript API reference</span></span>](/javascript/api/?view=signalr-js-latest)
* [<span data-ttu-id="760cb-209">Didacticiel JavaScript</span><span class="sxs-lookup"><span data-stu-id="760cb-209">JavaScript tutorial</span></span>](xref:tutorials/signalr)
* [<span data-ttu-id="760cb-210">Didacticiel WebPack et machine à écrire</span><span class="sxs-lookup"><span data-stu-id="760cb-210">WebPack and TypeScript tutorial</span></span>](xref:tutorials/signalr-typescript-webpack)
* [<span data-ttu-id="760cb-211">Hubs</span><span class="sxs-lookup"><span data-stu-id="760cb-211">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="760cb-212">Client .NET</span><span class="sxs-lookup"><span data-stu-id="760cb-212">.NET client</span></span>](xref:signalr/dotnet-client)
* [<span data-ttu-id="760cb-213">Publier sur Azure</span><span class="sxs-lookup"><span data-stu-id="760cb-213">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)
* [<span data-ttu-id="760cb-214">Requêtes Cross-Origin (CORS)</span><span class="sxs-lookup"><span data-stu-id="760cb-214">Cross-Origin Requests (CORS)</span></span>](xref:security/cors)
* <span data-ttu-id="760cb-215">[Documentation sans serveur d’Azure SignalR service](/azure/azure-signalr/signalr-concept-serverless-development-config)</span><span class="sxs-lookup"><span data-stu-id="760cb-215">[Azure SignalR Service serverless documentation](/azure/azure-signalr/signalr-concept-serverless-development-config)</span></span>
