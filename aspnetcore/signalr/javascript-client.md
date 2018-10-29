---
title: Client JavaScript ASP.NET Core SignalR
author: tdykstra
description: Vue d’ensemble du client JavaScript ASP.NET Core SignalR.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 08/14/2018
uid: signalr/javascript-client
ms.openlocfilehash: 02844c35d1933d36576c25ff335a572fb65eff5c
ms.sourcegitcommit: 375e9a67f5e1f7b0faaa056b4b46294cc70f55b7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/29/2018
ms.locfileid: "50208016"
---
# <a name="aspnet-core-signalr-javascript-client"></a><span data-ttu-id="3af02-103">Client JavaScript ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="3af02-103">ASP.NET Core SignalR JavaScript client</span></span>

<span data-ttu-id="3af02-104">Par [Rachel Appel](http://twitter.com/rachelappel)</span><span class="sxs-lookup"><span data-stu-id="3af02-104">By [Rachel Appel](http://twitter.com/rachelappel)</span></span>

<span data-ttu-id="3af02-105">La bibliothèque cliente JavaScript ASP.NET Core SignalR permet aux développeurs d’appeler un hub à partir de code côté serveur.</span><span class="sxs-lookup"><span data-stu-id="3af02-105">The ASP.NET Core SignalR JavaScript client library enables developers to call server-side hub code.</span></span>

<span data-ttu-id="3af02-106">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/Docs/tree/live/aspnetcore/signalr/javascript-client/sample) ([procédure de téléchargement](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="3af02-106">[View or download sample code](https://github.com/aspnet/Docs/tree/live/aspnetcore/signalr/javascript-client/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="install-the-signalr-client-package"></a><span data-ttu-id="3af02-107">Installer le package du client SignalR</span><span class="sxs-lookup"><span data-stu-id="3af02-107">Install the SignalR client package</span></span>

<span data-ttu-id="3af02-108">La bibliothèque de client SignalR JavaScript est remise en tant qu’un [npm](https://www.npmjs.com/) package.</span><span class="sxs-lookup"><span data-stu-id="3af02-108">The SignalR JavaScript client library is delivered as an [npm](https://www.npmjs.com/) package.</span></span> <span data-ttu-id="3af02-109">Si vous utilisez Visual Studio, exécutez `npm install` à partir de la **Console du Gestionnaire de Package** lorsque vous êtes dans le dossier racine.</span><span class="sxs-lookup"><span data-stu-id="3af02-109">If you're using Visual Studio, run `npm install` from the **Package Manager Console** while in the root folder.</span></span> <span data-ttu-id="3af02-110">Pour Visual Studio Code, exécutez la commande depuis le **Terminal intégré**.</span><span class="sxs-lookup"><span data-stu-id="3af02-110">For Visual Studio Code, run the command from the **Integrated Terminal**.</span></span>

  ```console
  npm init -y
  npm install @aspnet/signalr
  ```

<span data-ttu-id="3af02-111">npm installe le contenu du package dans le dossier *node_modules\\@aspnet\signalr\dist\browser*.</span><span class="sxs-lookup"><span data-stu-id="3af02-111">npm installs the package contents in the *node_modules\\@aspnet\signalr\dist\browser* folder.</span></span> <span data-ttu-id="3af02-112">Créez un dossier nommé *signalr* sous le dossier *wwwroot\\lib*.</span><span class="sxs-lookup"><span data-stu-id="3af02-112">Create a new folder named *signalr* under the *wwwroot\\lib* folder.</span></span> <span data-ttu-id="3af02-113">Copiez le fichier *signalr.js* dans le dossier *wwwroot\lib\signalr*.</span><span class="sxs-lookup"><span data-stu-id="3af02-113">Copy the *signalr.js* file to the *wwwroot\lib\signalr* folder.</span></span>

## <a name="use-the-signalr-javascript-client"></a><span data-ttu-id="3af02-114">Utiliser le client JavaScript SignalR</span><span class="sxs-lookup"><span data-stu-id="3af02-114">Use the SignalR JavaScript client</span></span>

<span data-ttu-id="3af02-115">Référencez le client JavaScript SignalR dans l'élément `<script>`.</span><span class="sxs-lookup"><span data-stu-id="3af02-115">Reference the SignalR JavaScript client in the `<script>` element.</span></span>

```html
<script src="~/lib/signalr/signalr.js"></script>
```

## <a name="connect-to-a-hub"></a><span data-ttu-id="3af02-116">Se connecter à un hub</span><span class="sxs-lookup"><span data-stu-id="3af02-116">Connect to a hub</span></span>

<span data-ttu-id="3af02-117">Le code suivant crée et démarre une connexion.</span><span class="sxs-lookup"><span data-stu-id="3af02-117">The following code creates and starts a connection.</span></span> <span data-ttu-id="3af02-118">Nom de concentrateur respecte la casse.</span><span class="sxs-lookup"><span data-stu-id="3af02-118">The hub's name is case insensitive.</span></span>

[!code-javascript[Call hub methods](javascript-client/sample/wwwroot/js/chat.js?range=9-12,28)]

### <a name="cross-origin-connections"></a><span data-ttu-id="3af02-119">Connexions cross-origin</span><span class="sxs-lookup"><span data-stu-id="3af02-119">Cross-origin connections</span></span>

<span data-ttu-id="3af02-120">En règle générale, les navigateurs chargent des connexions à partir du même domaine que la page demandée.</span><span class="sxs-lookup"><span data-stu-id="3af02-120">Typically, browsers load connections from the same domain as the requested page.</span></span> <span data-ttu-id="3af02-121">Toutefois, il existe des cas où une connexion à un autre domaine est nécessaire.</span><span class="sxs-lookup"><span data-stu-id="3af02-121">However, there are occasions when a connection to another domain is required.</span></span>

<span data-ttu-id="3af02-122">Pour empêcher la lecture des données sensibles à partir d’un autre site, un site malveillant [cross-origin connexions](xref:security/cors) sont désactivés par défaut.</span><span class="sxs-lookup"><span data-stu-id="3af02-122">To prevent a malicious site from reading sensitive data from another site, [cross-origin connections](xref:security/cors) are disabled by default.</span></span> <span data-ttu-id="3af02-123">Pour autoriser une demande de cross-origin, activez-la dans le `Startup` classe.</span><span class="sxs-lookup"><span data-stu-id="3af02-123">To allow a cross-origin request, enable it in the `Startup` class.</span></span>

[!code-csharp[Cross-origin connections](javascript-client/sample/Startup.cs?highlight=29-35,56)]

## <a name="call-hub-methods-from-client"></a><span data-ttu-id="3af02-124">Appeler des méthodes de hub à partir du client</span><span class="sxs-lookup"><span data-stu-id="3af02-124">Call hub methods from client</span></span>

<span data-ttu-id="3af02-125">Les clients JavaScript appellent les méthodes publiques sur les hubs via la méthode [invoke](/javascript/api/%40aspnet/signalr/hubconnection#invoke) de la [HubConnection](/javascript/api/%40aspnet/signalr/hubconnection).</span><span class="sxs-lookup"><span data-stu-id="3af02-125">JavaScript clients call public methods on hubs via the [invoke](/javascript/api/%40aspnet/signalr/hubconnection#invoke) method of the [HubConnection](/javascript/api/%40aspnet/signalr/hubconnection).</span></span> <span data-ttu-id="3af02-126">La méthode `invoke` accepte deux arguments :</span><span class="sxs-lookup"><span data-stu-id="3af02-126">The `invoke` method accepts two arguments:</span></span>

* <span data-ttu-id="3af02-127">Le nom de la méthode de hub.</span><span class="sxs-lookup"><span data-stu-id="3af02-127">The name of the hub method.</span></span> <span data-ttu-id="3af02-128">Dans l’exemple suivant, le nom de méthode sur le hub est `SendMessage`.</span><span class="sxs-lookup"><span data-stu-id="3af02-128">In the following example, the method name on the hub is `SendMessage`.</span></span>
* <span data-ttu-id="3af02-129">Tous les arguments définis dans la méthode de hub.</span><span class="sxs-lookup"><span data-stu-id="3af02-129">Any arguments defined in the hub method.</span></span> <span data-ttu-id="3af02-130">Dans l’exemple suivant, le nom de l’argument est `message`.</span><span class="sxs-lookup"><span data-stu-id="3af02-130">In the following example, the argument name is `message`.</span></span>

  [!code-javascript[Call hub methods](javascript-client/sample/wwwroot/js/chat.js?range=24)]

## <a name="call-client-methods-from-hub"></a><span data-ttu-id="3af02-131">Appeler des méthodes de client à partir de hub</span><span class="sxs-lookup"><span data-stu-id="3af02-131">Call client methods from hub</span></span>

<span data-ttu-id="3af02-132">Pour recevoir des messages à partir du hub, définissez une méthode à l’aide de la méthode [on](/javascript/api/%40aspnet/signalr/hubconnection#on) de la `HubConnection`.</span><span class="sxs-lookup"><span data-stu-id="3af02-132">To receive messages from the hub, define a method using the [on](/javascript/api/%40aspnet/signalr/hubconnection#on) method of the `HubConnection`.</span></span>

* <span data-ttu-id="3af02-133">Le nom de la méthode du client JavaScript.</span><span class="sxs-lookup"><span data-stu-id="3af02-133">The name of the JavaScript client method.</span></span> <span data-ttu-id="3af02-134">Dans l’exemple suivant, le nom de la méthode est `ReceiveMessage`.</span><span class="sxs-lookup"><span data-stu-id="3af02-134">In the following example, the method name is `ReceiveMessage`.</span></span>
* <span data-ttu-id="3af02-135">Les arguments que le hub passe à la méthode.</span><span class="sxs-lookup"><span data-stu-id="3af02-135">Arguments the hub passes to the method.</span></span> <span data-ttu-id="3af02-136">Dans l’exemple suivant, la valeur d’argument est `message`.</span><span class="sxs-lookup"><span data-stu-id="3af02-136">In the following example, the argument value is `message`.</span></span>

[!code-javascript[Receive calls from hub](javascript-client/sample/wwwroot/js/chat.js?range=14-19)]

<span data-ttu-id="3af02-137">Le code précédent dans `connection.on` s’exécute lorsque le code côté serveur appelle à l’aide de la [SendAsync](/dotnet/api/microsoft.aspnetcore.signalr.clientproxyextensions.sendasync) (méthode).</span><span class="sxs-lookup"><span data-stu-id="3af02-137">The preceding code in `connection.on` runs when server-side code calls it using the [SendAsync](/dotnet/api/microsoft.aspnetcore.signalr.clientproxyextensions.sendasync) method.</span></span>

[!code-csharp[Call client-side](javascript-client/sample/hubs/chathub.cs?range=8-11)]

<span data-ttu-id="3af02-138">SignalR détermine la méthode de client à appeler en faisant correspondre le nom de méthode et les arguments définis dans `SendAsync` et `connection.on`.</span><span class="sxs-lookup"><span data-stu-id="3af02-138">SignalR determines which client method to call by matching the method name and arguments defined in `SendAsync` and `connection.on`.</span></span>

> [!NOTE]
> <span data-ttu-id="3af02-139">Comme meilleure pratique, appelez la méthode [start](/javascript/api/%40aspnet/signalr/hubconnection#start) sur le `HubConnection` après `on`.</span><span class="sxs-lookup"><span data-stu-id="3af02-139">As a best practice, call the [start](/javascript/api/%40aspnet/signalr/hubconnection#start) method on the `HubConnection` after `on`.</span></span> <span data-ttu-id="3af02-140">En procédant comme ceci, vos gestionnaires sont enregistrés avant que tous les messages soient reçus.</span><span class="sxs-lookup"><span data-stu-id="3af02-140">Doing so ensures your handlers are registered before any messages are received.</span></span>

## <a name="error-handling-and-logging"></a><span data-ttu-id="3af02-141">Journalisation et gestion des erreurs</span><span class="sxs-lookup"><span data-stu-id="3af02-141">Error handling and logging</span></span>

<span data-ttu-id="3af02-142">Chaînez une méthode `catch` à la fin de la méthode `start` pour gérer les erreurs côté client.</span><span class="sxs-lookup"><span data-stu-id="3af02-142">Chain a `catch` method to the end of the `start` method to handle client-side errors.</span></span> <span data-ttu-id="3af02-143">Utilisez `console.error` pour envoyer les erreurs vers la console du navigateur.</span><span class="sxs-lookup"><span data-stu-id="3af02-143">Use `console.error` to output errors to the browser's console.</span></span>

[!code-javascript[Error handling](javascript-client/sample/wwwroot/js/chat.js?range=28)]

<span data-ttu-id="3af02-144">Configurer le suivi du journal côté client en passant un enregistreur d’événements et le type d’événement pour vous connecter lorsque la connexion est établie.</span><span class="sxs-lookup"><span data-stu-id="3af02-144">Setup client-side log tracing by passing a logger and type of event to log when the connection is made.</span></span> <span data-ttu-id="3af02-145">Les messages sont enregistrés avec le niveau de journalisation spécifié et versions ultérieures.</span><span class="sxs-lookup"><span data-stu-id="3af02-145">Messages are logged with the specified log level and higher.</span></span> <span data-ttu-id="3af02-146">Niveaux de consignation disponibles sont les suivantes :</span><span class="sxs-lookup"><span data-stu-id="3af02-146">Available log levels are as follows:</span></span>

* <span data-ttu-id="3af02-147">`signalR.LogLevel.Error` &ndash; Messages d’erreur.</span><span class="sxs-lookup"><span data-stu-id="3af02-147">`signalR.LogLevel.Error` &ndash; Error messages.</span></span> <span data-ttu-id="3af02-148">Journaux `Error` messages uniquement.</span><span class="sxs-lookup"><span data-stu-id="3af02-148">Logs `Error` messages only.</span></span>
* <span data-ttu-id="3af02-149">`signalR.LogLevel.Warning` &ndash; Messages d’avertissement concernant les erreurs potentielles.</span><span class="sxs-lookup"><span data-stu-id="3af02-149">`signalR.LogLevel.Warning` &ndash; Warning messages about potential errors.</span></span> <span data-ttu-id="3af02-150">Journaux `Warning`, et `Error` messages.</span><span class="sxs-lookup"><span data-stu-id="3af02-150">Logs `Warning`, and `Error` messages.</span></span>
* <span data-ttu-id="3af02-151">`signalR.LogLevel.Information` &ndash; Messages d’état sans erreurs.</span><span class="sxs-lookup"><span data-stu-id="3af02-151">`signalR.LogLevel.Information` &ndash; Status messages without errors.</span></span> <span data-ttu-id="3af02-152">Journaux `Information`, `Warning`, et `Error` messages.</span><span class="sxs-lookup"><span data-stu-id="3af02-152">Logs `Information`, `Warning`, and `Error` messages.</span></span>
* <span data-ttu-id="3af02-153">`signalR.LogLevel.Trace` &ndash; Messages de trace.</span><span class="sxs-lookup"><span data-stu-id="3af02-153">`signalR.LogLevel.Trace` &ndash; Trace messages.</span></span> <span data-ttu-id="3af02-154">Consigne tout, y compris les données transportées entre le hub et le client.</span><span class="sxs-lookup"><span data-stu-id="3af02-154">Logs everything, including data transported between hub and client.</span></span>

<span data-ttu-id="3af02-155">Utilisez le [configureLogging](/javascript/api/%40aspnet/signalr/hubconnectionbuilder#configurelogging) méthode sur [HubConnectionBuilder](/javascript/api/%40aspnet/signalr/hubconnectionbuilder) pour configurer le niveau de journalisation.</span><span class="sxs-lookup"><span data-stu-id="3af02-155">Use the [configureLogging](/javascript/api/%40aspnet/signalr/hubconnectionbuilder#configurelogging) method on [HubConnectionBuilder](/javascript/api/%40aspnet/signalr/hubconnectionbuilder) to configure the log level.</span></span> <span data-ttu-id="3af02-156">Les messages sont enregistrés dans la console du navigateur.</span><span class="sxs-lookup"><span data-stu-id="3af02-156">Messages are logged to the browser console.</span></span>

[!code-javascript[Logging levels](javascript-client/sample/wwwroot/js/chat.js?range=9-12)]

## <a name="additional-resources"></a><span data-ttu-id="3af02-157">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="3af02-157">Additional resources</span></span>

* [<span data-ttu-id="3af02-158">Référence API JavaScript</span><span class="sxs-lookup"><span data-stu-id="3af02-158">JavaScript API reference</span></span>](/javascript/api/?view=signalr-js-latest)
* [<span data-ttu-id="3af02-159">Hubs</span><span class="sxs-lookup"><span data-stu-id="3af02-159">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="3af02-160">Client .NET</span><span class="sxs-lookup"><span data-stu-id="3af02-160">.NET client</span></span>](xref:signalr/dotnet-client)
* [<span data-ttu-id="3af02-161">Publier sur Azure</span><span class="sxs-lookup"><span data-stu-id="3af02-161">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)
* [<span data-ttu-id="3af02-162">Autoriser les demandes de Cross-Origin (CORS) dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="3af02-162">Enable Cross-Origin Requests (CORS) in ASP.NET Core</span></span>](xref:security/cors)
