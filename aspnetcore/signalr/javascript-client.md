---
title: Client JavaScript ASP.NET Core SignalR
author: tdykstra
description: Vue d’ensemble du client JavaScript ASP.NET Core SignalR.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 11/14/2018
uid: signalr/javascript-client
ms.openlocfilehash: acdb4d1a59d980010fe89fe381190425cbb12901
ms.sourcegitcommit: 42a8164b8aba21f322ffefacb92301bdfb4d3c2d
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/16/2019
ms.locfileid: "54341450"
---
# <a name="aspnet-core-signalr-javascript-client"></a><span data-ttu-id="99e45-103">Client JavaScript ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="99e45-103">ASP.NET Core SignalR JavaScript client</span></span>

<span data-ttu-id="99e45-104">Par [Rachel Appel](http://twitter.com/rachelappel)</span><span class="sxs-lookup"><span data-stu-id="99e45-104">By [Rachel Appel](http://twitter.com/rachelappel)</span></span>

<span data-ttu-id="99e45-105">La bibliothèque cliente JavaScript ASP.NET Core SignalR permet aux développeurs d’appeler un hub à partir de code côté serveur.</span><span class="sxs-lookup"><span data-stu-id="99e45-105">The ASP.NET Core SignalR JavaScript client library enables developers to call server-side hub code.</span></span>

<span data-ttu-id="99e45-106">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/Docs/tree/live/aspnetcore/signalr/javascript-client/sample) ([procédure de téléchargement](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="99e45-106">[View or download sample code](https://github.com/aspnet/Docs/tree/live/aspnetcore/signalr/javascript-client/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="install-the-signalr-client-package"></a><span data-ttu-id="99e45-107">Installer le package du client SignalR</span><span class="sxs-lookup"><span data-stu-id="99e45-107">Install the SignalR client package</span></span>

<span data-ttu-id="99e45-108">La bibliothèque de client SignalR JavaScript est remise en tant qu’un [npm](https://www.npmjs.com/) package.</span><span class="sxs-lookup"><span data-stu-id="99e45-108">The SignalR JavaScript client library is delivered as an [npm](https://www.npmjs.com/) package.</span></span> <span data-ttu-id="99e45-109">Si vous utilisez Visual Studio, exécutez `npm install` à partir de la **Console du Gestionnaire de Package** lorsque vous êtes dans le dossier racine.</span><span class="sxs-lookup"><span data-stu-id="99e45-109">If you're using Visual Studio, run `npm install` from the **Package Manager Console** while in the root folder.</span></span> <span data-ttu-id="99e45-110">Pour Visual Studio Code, exécutez la commande depuis le **Terminal intégré**.</span><span class="sxs-lookup"><span data-stu-id="99e45-110">For Visual Studio Code, run the command from the **Integrated Terminal**.</span></span>

  ```console
  npm init -y
  npm install @aspnet/signalr
  ```

<span data-ttu-id="99e45-111">npm installe le contenu du package dans le dossier *node_modules\\@aspnet\signalr\dist\browser*.</span><span class="sxs-lookup"><span data-stu-id="99e45-111">npm installs the package contents in the *node_modules\\@aspnet\signalr\dist\browser* folder.</span></span> <span data-ttu-id="99e45-112">Créez un dossier nommé *signalr* sous le dossier *wwwroot\\lib*.</span><span class="sxs-lookup"><span data-stu-id="99e45-112">Create a new folder named *signalr* under the *wwwroot\\lib* folder.</span></span> <span data-ttu-id="99e45-113">Copiez le fichier *signalr.js* dans le dossier *wwwroot\lib\signalr*.</span><span class="sxs-lookup"><span data-stu-id="99e45-113">Copy the *signalr.js* file to the *wwwroot\lib\signalr* folder.</span></span>

## <a name="use-the-signalr-javascript-client"></a><span data-ttu-id="99e45-114">Utiliser le client JavaScript SignalR</span><span class="sxs-lookup"><span data-stu-id="99e45-114">Use the SignalR JavaScript client</span></span>

<span data-ttu-id="99e45-115">Référencez le client JavaScript SignalR dans l'élément `<script>`.</span><span class="sxs-lookup"><span data-stu-id="99e45-115">Reference the SignalR JavaScript client in the `<script>` element.</span></span>

```html
<script src="~/lib/signalr/signalr.js"></script>
```

## <a name="connect-to-a-hub"></a><span data-ttu-id="99e45-116">Se connecter à un hub</span><span class="sxs-lookup"><span data-stu-id="99e45-116">Connect to a hub</span></span>

<span data-ttu-id="99e45-117">Le code suivant crée et démarre une connexion.</span><span class="sxs-lookup"><span data-stu-id="99e45-117">The following code creates and starts a connection.</span></span> <span data-ttu-id="99e45-118">Nom de concentrateur respecte la casse.</span><span class="sxs-lookup"><span data-stu-id="99e45-118">The hub's name is case insensitive.</span></span>

[!code-javascript[Call hub methods](javascript-client/sample/wwwroot/js/chat.js?range=9-12)]

### <a name="cross-origin-connections"></a><span data-ttu-id="99e45-119">Connexions cross-origin</span><span class="sxs-lookup"><span data-stu-id="99e45-119">Cross-origin connections</span></span>

<span data-ttu-id="99e45-120">En règle générale, les navigateurs chargent des connexions à partir du même domaine que la page demandée.</span><span class="sxs-lookup"><span data-stu-id="99e45-120">Typically, browsers load connections from the same domain as the requested page.</span></span> <span data-ttu-id="99e45-121">Toutefois, il existe des cas où une connexion à un autre domaine est nécessaire.</span><span class="sxs-lookup"><span data-stu-id="99e45-121">However, there are occasions when a connection to another domain is required.</span></span>

<span data-ttu-id="99e45-122">Pour empêcher la lecture des données sensibles à partir d’un autre site, un site malveillant [cross-origin connexions](xref:security/cors) sont désactivés par défaut.</span><span class="sxs-lookup"><span data-stu-id="99e45-122">To prevent a malicious site from reading sensitive data from another site, [cross-origin connections](xref:security/cors) are disabled by default.</span></span> <span data-ttu-id="99e45-123">Pour autoriser une demande de cross-origin, activez-la dans le `Startup` classe.</span><span class="sxs-lookup"><span data-stu-id="99e45-123">To allow a cross-origin request, enable it in the `Startup` class.</span></span>

[!code-csharp[Cross-origin connections](javascript-client/sample/Startup.cs?highlight=29-35,56)]

## <a name="call-hub-methods-from-client"></a><span data-ttu-id="99e45-124">Appeler des méthodes de hub à partir du client</span><span class="sxs-lookup"><span data-stu-id="99e45-124">Call hub methods from client</span></span>

<span data-ttu-id="99e45-125">Les clients JavaScript appellent les méthodes publiques sur les hubs via la méthode [invoke](/javascript/api/%40aspnet/signalr/hubconnection#invoke) de la [HubConnection](/javascript/api/%40aspnet/signalr/hubconnection).</span><span class="sxs-lookup"><span data-stu-id="99e45-125">JavaScript clients call public methods on hubs via the [invoke](/javascript/api/%40aspnet/signalr/hubconnection#invoke) method of the [HubConnection](/javascript/api/%40aspnet/signalr/hubconnection).</span></span> <span data-ttu-id="99e45-126">La méthode `invoke` accepte deux arguments :</span><span class="sxs-lookup"><span data-stu-id="99e45-126">The `invoke` method accepts two arguments:</span></span>

* <span data-ttu-id="99e45-127">Le nom de la méthode de hub.</span><span class="sxs-lookup"><span data-stu-id="99e45-127">The name of the hub method.</span></span> <span data-ttu-id="99e45-128">Dans l’exemple suivant, le nom de méthode sur le hub est `SendMessage`.</span><span class="sxs-lookup"><span data-stu-id="99e45-128">In the following example, the method name on the hub is `SendMessage`.</span></span>
* <span data-ttu-id="99e45-129">Tous les arguments définis dans la méthode de hub.</span><span class="sxs-lookup"><span data-stu-id="99e45-129">Any arguments defined in the hub method.</span></span> <span data-ttu-id="99e45-130">Dans l’exemple suivant, le nom de l’argument est `message`.</span><span class="sxs-lookup"><span data-stu-id="99e45-130">In the following example, the argument name is `message`.</span></span> <span data-ttu-id="99e45-131">L’exemple de code utilise la syntaxe de fonction arrow est prise en charge dans les versions actuelles de tous les principaux navigateurs à l’exception d’Internet Explorer.</span><span class="sxs-lookup"><span data-stu-id="99e45-131">The example code uses arrow function syntax that is supported in current versions of all major browsers except Internet Explorer.</span></span>

  [!code-javascript[Call hub methods](javascript-client/sample/wwwroot/js/chat.js?range=24)]

## <a name="call-client-methods-from-hub"></a><span data-ttu-id="99e45-132">Appeler des méthodes de client à partir de hub</span><span class="sxs-lookup"><span data-stu-id="99e45-132">Call client methods from hub</span></span>

<span data-ttu-id="99e45-133">Pour recevoir des messages à partir du hub, définissez une méthode à l’aide de la méthode [on](/javascript/api/%40aspnet/signalr/hubconnection#on) de la `HubConnection`.</span><span class="sxs-lookup"><span data-stu-id="99e45-133">To receive messages from the hub, define a method using the [on](/javascript/api/%40aspnet/signalr/hubconnection#on) method of the `HubConnection`.</span></span>

* <span data-ttu-id="99e45-134">Le nom de la méthode du client JavaScript.</span><span class="sxs-lookup"><span data-stu-id="99e45-134">The name of the JavaScript client method.</span></span> <span data-ttu-id="99e45-135">Dans l’exemple suivant, le nom de la méthode est `ReceiveMessage`.</span><span class="sxs-lookup"><span data-stu-id="99e45-135">In the following example, the method name is `ReceiveMessage`.</span></span>
* <span data-ttu-id="99e45-136">Les arguments que le hub passe à la méthode.</span><span class="sxs-lookup"><span data-stu-id="99e45-136">Arguments the hub passes to the method.</span></span> <span data-ttu-id="99e45-137">Dans l’exemple suivant, la valeur d’argument est `message`.</span><span class="sxs-lookup"><span data-stu-id="99e45-137">In the following example, the argument value is `message`.</span></span>

[!code-javascript[Receive calls from hub](javascript-client/sample/wwwroot/js/chat.js?range=14-19)]

<span data-ttu-id="99e45-138">Le code précédent dans `connection.on` s’exécute lorsque le code côté serveur appelle à l’aide de la [SendAsync](/dotnet/api/microsoft.aspnetcore.signalr.clientproxyextensions.sendasync) (méthode).</span><span class="sxs-lookup"><span data-stu-id="99e45-138">The preceding code in `connection.on` runs when server-side code calls it using the [SendAsync](/dotnet/api/microsoft.aspnetcore.signalr.clientproxyextensions.sendasync) method.</span></span>

[!code-csharp[Call client-side](javascript-client/sample/hubs/chathub.cs?range=8-11)]

<span data-ttu-id="99e45-139">SignalR détermine la méthode de client à appeler en faisant correspondre le nom de méthode et les arguments définis dans `SendAsync` et `connection.on`.</span><span class="sxs-lookup"><span data-stu-id="99e45-139">SignalR determines which client method to call by matching the method name and arguments defined in `SendAsync` and `connection.on`.</span></span>

> [!NOTE]
> <span data-ttu-id="99e45-140">Comme meilleure pratique, appelez la méthode [start](/javascript/api/%40aspnet/signalr/hubconnection#start) sur le `HubConnection` après `on`.</span><span class="sxs-lookup"><span data-stu-id="99e45-140">As a best practice, call the [start](/javascript/api/%40aspnet/signalr/hubconnection#start) method on the `HubConnection` after `on`.</span></span> <span data-ttu-id="99e45-141">En procédant comme ceci, vos gestionnaires sont enregistrés avant que tous les messages soient reçus.</span><span class="sxs-lookup"><span data-stu-id="99e45-141">Doing so ensures your handlers are registered before any messages are received.</span></span>

## <a name="error-handling-and-logging"></a><span data-ttu-id="99e45-142">Journalisation et gestion des erreurs</span><span class="sxs-lookup"><span data-stu-id="99e45-142">Error handling and logging</span></span>

<span data-ttu-id="99e45-143">Chaînez une méthode `catch` à la fin de la méthode `start` pour gérer les erreurs côté client.</span><span class="sxs-lookup"><span data-stu-id="99e45-143">Chain a `catch` method to the end of the `start` method to handle client-side errors.</span></span> <span data-ttu-id="99e45-144">Utilisez `console.error` pour envoyer les erreurs vers la console du navigateur.</span><span class="sxs-lookup"><span data-stu-id="99e45-144">Use `console.error` to output errors to the browser's console.</span></span>

[!code-javascript[Error handling](javascript-client/sample/wwwroot/js/chat.js?range=43-45)]

<span data-ttu-id="99e45-145">Configurer le suivi du journal côté client en passant un enregistreur d’événements et le type d’événement pour vous connecter lorsque la connexion est établie.</span><span class="sxs-lookup"><span data-stu-id="99e45-145">Setup client-side log tracing by passing a logger and type of event to log when the connection is made.</span></span> <span data-ttu-id="99e45-146">Les messages sont enregistrés avec le niveau de journalisation spécifié et versions ultérieures.</span><span class="sxs-lookup"><span data-stu-id="99e45-146">Messages are logged with the specified log level and higher.</span></span> <span data-ttu-id="99e45-147">Niveaux de consignation disponibles sont les suivantes :</span><span class="sxs-lookup"><span data-stu-id="99e45-147">Available log levels are as follows:</span></span>

* <span data-ttu-id="99e45-148">`signalR.LogLevel.Error` &ndash; Messages d’erreur.</span><span class="sxs-lookup"><span data-stu-id="99e45-148">`signalR.LogLevel.Error` &ndash; Error messages.</span></span> <span data-ttu-id="99e45-149">Journaux `Error` messages uniquement.</span><span class="sxs-lookup"><span data-stu-id="99e45-149">Logs `Error` messages only.</span></span>
* <span data-ttu-id="99e45-150">`signalR.LogLevel.Warning` &ndash; Messages d’avertissement concernant les erreurs potentielles.</span><span class="sxs-lookup"><span data-stu-id="99e45-150">`signalR.LogLevel.Warning` &ndash; Warning messages about potential errors.</span></span> <span data-ttu-id="99e45-151">Journaux `Warning`, et `Error` messages.</span><span class="sxs-lookup"><span data-stu-id="99e45-151">Logs `Warning`, and `Error` messages.</span></span>
* <span data-ttu-id="99e45-152">`signalR.LogLevel.Information` &ndash; Messages d’état sans erreurs.</span><span class="sxs-lookup"><span data-stu-id="99e45-152">`signalR.LogLevel.Information` &ndash; Status messages without errors.</span></span> <span data-ttu-id="99e45-153">Journaux `Information`, `Warning`, et `Error` messages.</span><span class="sxs-lookup"><span data-stu-id="99e45-153">Logs `Information`, `Warning`, and `Error` messages.</span></span>
* <span data-ttu-id="99e45-154">`signalR.LogLevel.Trace` &ndash; Messages de trace.</span><span class="sxs-lookup"><span data-stu-id="99e45-154">`signalR.LogLevel.Trace` &ndash; Trace messages.</span></span> <span data-ttu-id="99e45-155">Consigne tout, y compris les données transportées entre le hub et le client.</span><span class="sxs-lookup"><span data-stu-id="99e45-155">Logs everything, including data transported between hub and client.</span></span>

<span data-ttu-id="99e45-156">Utilisez le [configureLogging](/javascript/api/%40aspnet/signalr/hubconnectionbuilder#configurelogging) méthode sur [HubConnectionBuilder](/javascript/api/%40aspnet/signalr/hubconnectionbuilder) pour configurer le niveau de journalisation.</span><span class="sxs-lookup"><span data-stu-id="99e45-156">Use the [configureLogging](/javascript/api/%40aspnet/signalr/hubconnectionbuilder#configurelogging) method on [HubConnectionBuilder](/javascript/api/%40aspnet/signalr/hubconnectionbuilder) to configure the log level.</span></span> <span data-ttu-id="99e45-157">Les messages sont enregistrés dans la console du navigateur.</span><span class="sxs-lookup"><span data-stu-id="99e45-157">Messages are logged to the browser console.</span></span>

[!code-javascript[Logging levels](javascript-client/sample/wwwroot/js/chat.js?range=9-12)]

## <a name="reconnect-clients"></a><span data-ttu-id="99e45-158">Reconnecter les clients</span><span class="sxs-lookup"><span data-stu-id="99e45-158">Reconnect clients</span></span>

<span data-ttu-id="99e45-159">Le client JavaScript pour SignalR ne se reconnecter automatiquement.</span><span class="sxs-lookup"><span data-stu-id="99e45-159">The JavaScript client for SignalR doesn't automatically reconnect.</span></span> <span data-ttu-id="99e45-160">Vous devez écrire du code qui se reconnectera votre client manuellement.</span><span class="sxs-lookup"><span data-stu-id="99e45-160">You must write code that will reconnect your client manually.</span></span> <span data-ttu-id="99e45-161">Le code suivant illustre une approche classique de reconnexion :</span><span class="sxs-lookup"><span data-stu-id="99e45-161">The following code demonstrates a typical reconnection approach:</span></span>

1. <span data-ttu-id="99e45-162">Une fonction (dans ce cas, le `start` (fonction)) est créé pour démarrer la connexion.</span><span class="sxs-lookup"><span data-stu-id="99e45-162">A function (in this case, the `start` function) is created to start the connection.</span></span>
1. <span data-ttu-id="99e45-163">Appelez le `start` (fonction) dans la connexion `onclose` Gestionnaire d’événements.</span><span class="sxs-lookup"><span data-stu-id="99e45-163">Call the `start` function in the connection's `onclose` event handler.</span></span>

[!code-javascript[Reconnect the JavaScript client](javascript-client/sample/wwwroot/js/chat.js?range=28-40)]

<span data-ttu-id="99e45-164">Une implémentation réelle serait utiliser une temporisation exponentielle ou réessayer un nombre spécifié de fois avant d’abandonner.</span><span class="sxs-lookup"><span data-stu-id="99e45-164">A real-world implementation would use an exponential back-off or retry a specified number of times before giving up.</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="99e45-165">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="99e45-165">Additional resources</span></span>

* [<span data-ttu-id="99e45-166">Référence API JavaScript</span><span class="sxs-lookup"><span data-stu-id="99e45-166">JavaScript API reference</span></span>](/javascript/api/?view=signalr-js-latest)
* [<span data-ttu-id="99e45-167">Didacticiel de JavaScript</span><span class="sxs-lookup"><span data-stu-id="99e45-167">JavaScript tutorial</span></span>](xref:tutorials/signalr)
* [<span data-ttu-id="99e45-168">Didacticiel WebPack et TypeScript</span><span class="sxs-lookup"><span data-stu-id="99e45-168">WebPack and TypeScript tutorial</span></span>](xref:tutorials/signalr-typescript-webpack)
* [<span data-ttu-id="99e45-169">Hubs</span><span class="sxs-lookup"><span data-stu-id="99e45-169">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="99e45-170">Client .NET</span><span class="sxs-lookup"><span data-stu-id="99e45-170">.NET client</span></span>](xref:signalr/dotnet-client)
* [<span data-ttu-id="99e45-171">Publier sur Azure</span><span class="sxs-lookup"><span data-stu-id="99e45-171">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)
* [<span data-ttu-id="99e45-172">Demandes de cross-Origin (CORS)</span><span class="sxs-lookup"><span data-stu-id="99e45-172">Cross-Origin Requests (CORS)</span></span>](xref:security/cors)
