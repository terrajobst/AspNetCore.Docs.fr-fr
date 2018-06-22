---
title: ASP.NET Core SignalR JavaScript client
author: rachelappel
description: Vue d’ensemble de client ASP.NET Core SignalR JavaScript.
monikerRange: '>= aspnetcore-2.1'
ms.author: rachelap
ms.custom: mvc
ms.date: 05/29/2018
uid: signalr/javascript-client
ms.openlocfilehash: 9ea12dc21abfef6d2e6f3fcc455d8aa58ecaa011
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/20/2018
ms.locfileid: "36271942"
---
# <a name="aspnet-core-signalr-javascript-client"></a><span data-ttu-id="39195-103">ASP.NET Core SignalR JavaScript client</span><span class="sxs-lookup"><span data-stu-id="39195-103">ASP.NET Core SignalR JavaScript client</span></span>

<span data-ttu-id="39195-104">Par [Rachel Appel](http://twitter.com/rachelappel)</span><span class="sxs-lookup"><span data-stu-id="39195-104">By [Rachel Appel](http://twitter.com/rachelappel)</span></span>

<span data-ttu-id="39195-105">La bibliothèque cliente ASP.NET Core SignalR JavaScript permet aux développeurs d’appeler du code de concentrateur côté serveur.</span><span class="sxs-lookup"><span data-stu-id="39195-105">The ASP.NET Core SignalR JavaScript client library enables developers to call server-side hub code.</span></span>

<span data-ttu-id="39195-106">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/Docs/tree/live/aspnetcore/signalr/javascript-client/sample) ([procédure de téléchargement](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="39195-106">[View or download sample code](https://github.com/aspnet/Docs/tree/live/aspnetcore/signalr/javascript-client/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="install-the-signalr-client-package"></a><span data-ttu-id="39195-107">Installez le package de client SignalR</span><span class="sxs-lookup"><span data-stu-id="39195-107">Install the SignalR client package</span></span>

<span data-ttu-id="39195-108">La bibliothèque cliente SignalR JavaScript est remise sous une [npm](https://www.npmjs.com/) package.</span><span class="sxs-lookup"><span data-stu-id="39195-108">The SignalR JavaScript client library is delivered as an [npm](https://www.npmjs.com/) package.</span></span> <span data-ttu-id="39195-109">Si vous utilisez Visual Studio, exécutez `npm install` à partir de la **Package Manager Console** tandis que dans le dossier racine.</span><span class="sxs-lookup"><span data-stu-id="39195-109">If you're using Visual Studio, run `npm install` from the **Package Manager Console** while in the root folder.</span></span> <span data-ttu-id="39195-110">Pour le Code de Visual Studio, exécutez la commande depuis le **Terminal Server intégré**.</span><span class="sxs-lookup"><span data-stu-id="39195-110">For Visual Studio Code, run the command from the **Integrated Terminal**.</span></span>

  ```console
   npm init -y
   npm install @aspnet/signalr
  ```

<span data-ttu-id="39195-111">Npm installe le contenu du package dans le *node_modules\\ @aspnet\signalr\dist\browser*  dossier.</span><span class="sxs-lookup"><span data-stu-id="39195-111">Npm installs the package contents in the *node_modules\\@aspnet\signalr\dist\browser* folder.</span></span> <span data-ttu-id="39195-112">Créez un dossier nommé *signalr* sous le *wwwroot\\lib* dossier.</span><span class="sxs-lookup"><span data-stu-id="39195-112">Create a new folder named *signalr* under the *wwwroot\\lib* folder.</span></span> <span data-ttu-id="39195-113">Copie le *signalr.js* de fichiers à la *wwwroot\lib\signalr* dossier.</span><span class="sxs-lookup"><span data-stu-id="39195-113">Copy the *signalr.js* file to the *wwwroot\lib\signalr* folder.</span></span>

## <a name="use-the-signalr-javascript-client"></a><span data-ttu-id="39195-114">Utiliser le client SignalR JavaScript</span><span class="sxs-lookup"><span data-stu-id="39195-114">Use the SignalR JavaScript client</span></span>

<span data-ttu-id="39195-115">Référencer le client SignalR JavaScript dans le `<script>` élément.</span><span class="sxs-lookup"><span data-stu-id="39195-115">Reference the SignalR JavaScript client in the `<script>` element.</span></span>

```html
<script src="~/lib/signalr/signalr.js"></script>
```

## <a name="connect-to-a-hub"></a><span data-ttu-id="39195-116">Se connecter à un concentrateur</span><span class="sxs-lookup"><span data-stu-id="39195-116">Connect to a hub</span></span>

<span data-ttu-id="39195-117">Le code suivant crée et lance une connexion.</span><span class="sxs-lookup"><span data-stu-id="39195-117">The following code creates and starts a connection.</span></span> <span data-ttu-id="39195-118">Nom de concentrateur respecte la casse.</span><span class="sxs-lookup"><span data-stu-id="39195-118">The hub's name is case insensitive.</span></span>

[!code-javascript[Call hub methods](javascript-client/sample/wwwroot/js/chat.js?range=9-12,28)]

### <a name="cross-origin-connections"></a><span data-ttu-id="39195-119">Connexions de cross-origine</span><span class="sxs-lookup"><span data-stu-id="39195-119">Cross-origin connections</span></span>

<span data-ttu-id="39195-120">En règle générale, navigateurs chargent les connexions à partir du même domaine que la page demandée.</span><span class="sxs-lookup"><span data-stu-id="39195-120">Typically, browsers load connections from the same domain as the requested page.</span></span> <span data-ttu-id="39195-121">Toutefois, il existe des occasions lorsqu’une connexion à un autre domaine est requise.</span><span class="sxs-lookup"><span data-stu-id="39195-121">However, there are occasions when a connection to another domain is required.</span></span>

<span data-ttu-id="39195-122">Pour empêcher la lecture des données sensibles à partir d’un autre site, un site malveillant [cross-origine connexions](xref:security/cors) sont désactivées par défaut.</span><span class="sxs-lookup"><span data-stu-id="39195-122">To prevent a malicious site from reading sensitive data from another site, [cross-origin connections](xref:security/cors) are disabled by default.</span></span> <span data-ttu-id="39195-123">Pour autoriser une demande cross-origin, activez-le dans la `Startup` classe.</span><span class="sxs-lookup"><span data-stu-id="39195-123">To allow a cross-origin request, enable it in the `Startup` class.</span></span>

[!code-csharp[Cross-origin connections](javascript-client/sample/Startup.cs?highlight=29-35,56)]

## <a name="call-hub-methods-from-client"></a><span data-ttu-id="39195-124">Appeler des méthodes de hub à partir du client</span><span class="sxs-lookup"><span data-stu-id="39195-124">Call hub methods from client</span></span>

<span data-ttu-id="39195-125">Les clients JavaScript appellent les méthodes publiques sur les concentrateurs par à l’aide de `connection.invoke`.</span><span class="sxs-lookup"><span data-stu-id="39195-125">JavaScript clients call public methods on hubs by using `connection.invoke`.</span></span> <span data-ttu-id="39195-126">Le `invoke` méthode accepte deux arguments :</span><span class="sxs-lookup"><span data-stu-id="39195-126">The `invoke` method accepts two arguments:</span></span>

* <span data-ttu-id="39195-127">Le nom de la méthode de concentrateur.</span><span class="sxs-lookup"><span data-stu-id="39195-127">The name of the hub method.</span></span> <span data-ttu-id="39195-128">Dans l’exemple suivant, le nom du concentrateur est `SendMessage`.</span><span class="sxs-lookup"><span data-stu-id="39195-128">In the following example, the hub name is `SendMessage`.</span></span>
* <span data-ttu-id="39195-129">Tous les arguments définis dans la méthode de concentrateur.</span><span class="sxs-lookup"><span data-stu-id="39195-129">Any arguments defined in the hub method.</span></span> <span data-ttu-id="39195-130">Dans l’exemple suivant, le nom de l’argument est `message`.</span><span class="sxs-lookup"><span data-stu-id="39195-130">In the following example, the argument name is `message`.</span></span>

[!code-javascript[Call hub methods](javascript-client/sample/wwwroot/js/chat.js?range=24)]

## <a name="call-client-methods-from-hub"></a><span data-ttu-id="39195-131">Appeler des méthodes de client à partir du hub</span><span class="sxs-lookup"><span data-stu-id="39195-131">Call client methods from hub</span></span>

<span data-ttu-id="39195-132">Pour recevoir des messages à partir du concentrateur, définissez une méthode à l’aide de la `connection.on` (méthode).</span><span class="sxs-lookup"><span data-stu-id="39195-132">To receive messages from the hub, define a method using the `connection.on` method.</span></span>

* <span data-ttu-id="39195-133">Le nom de la méthode du client JavaScript.</span><span class="sxs-lookup"><span data-stu-id="39195-133">The name of the JavaScript client method.</span></span> <span data-ttu-id="39195-134">Dans l’exemple suivant, le nom de la méthode est `ReceiveMessage`.</span><span class="sxs-lookup"><span data-stu-id="39195-134">In the following example, the method name is `ReceiveMessage`.</span></span>
* <span data-ttu-id="39195-135">Arguments que concentrateur passe à la méthode.</span><span class="sxs-lookup"><span data-stu-id="39195-135">Arguments the hub passes to the method.</span></span> <span data-ttu-id="39195-136">Dans l’exemple suivant, la valeur de l’argument est `message`.</span><span class="sxs-lookup"><span data-stu-id="39195-136">In the following example, the argument value is `message`.</span></span>

[!code-javascript[Receive calls from hub](javascript-client/sample/wwwroot/js/chat.js?range=14-19)]

<span data-ttu-id="39195-137">Le code précédent dans `connection.on` s’exécute lorsque le code côté serveur l’appelle en utilisant la méthode `SendAsync`.</span><span class="sxs-lookup"><span data-stu-id="39195-137">The preceding code in `connection.on` runs when server-side code calls it using the `SendAsync` method.</span></span>

[!code-csharp[Call client-side](javascript-client/sample/hubs/chathub.cs?range=8-11)]

<span data-ttu-id="39195-138">SignalR détermine la méthode de client à appeler en faisant correspondre le nom de méthode et les arguments définis dans `SendAsync` et `connection.on`.</span><span class="sxs-lookup"><span data-stu-id="39195-138">SignalR determines which client method to call by matching the method name and arguments defined in `SendAsync` and `connection.on`.</span></span>

> [!NOTE]
> <span data-ttu-id="39195-139">Comme meilleure pratique, appelez `connection.start` après `connection.on` afin des gestionnaires inscrits avant la réception des messages.</span><span class="sxs-lookup"><span data-stu-id="39195-139">As a best practice, call `connection.start` after `connection.on` so your handlers are registered before any messages are received.</span></span>

## <a name="error-handling-and-logging"></a><span data-ttu-id="39195-140">Journalisation et gestion des erreurs</span><span class="sxs-lookup"><span data-stu-id="39195-140">Error handling and logging</span></span>

<span data-ttu-id="39195-141">Chaîne d’un `catch` méthode à la fin de la `connection.start` méthode pour gérer les erreurs du côté client.</span><span class="sxs-lookup"><span data-stu-id="39195-141">Chain a `catch` method to the end of the `connection.start` method to handle client-side errors.</span></span> <span data-ttu-id="39195-142">Utilisez `console.error` aux erreurs de sortie à la console du navigateur.</span><span class="sxs-lookup"><span data-stu-id="39195-142">Use `console.error` to output errors to the browser's console.</span></span>

[!code-javascript[Error handling](javascript-client/sample/wwwroot/js/chat.js?range=28)]

<span data-ttu-id="39195-143">Configurer le suivi de journal côté client en passant un enregistreur d’événements et le type d’événement pour se connecter lors de la connexion est établie.</span><span class="sxs-lookup"><span data-stu-id="39195-143">Setup client-side log tracing by passing a logger and type of event to log when the connection is made.</span></span> <span data-ttu-id="39195-144">Les messages sont enregistrés et le niveau de journal spécifié.</span><span class="sxs-lookup"><span data-stu-id="39195-144">Messages are logged with the specified log level and higher.</span></span> <span data-ttu-id="39195-145">Niveaux de journalisation disponibles sont les suivantes :</span><span class="sxs-lookup"><span data-stu-id="39195-145">Available log levels are as follows:</span></span>

* <span data-ttu-id="39195-146">`signalR.LogLevel.Error` : Messages d’erreur.</span><span class="sxs-lookup"><span data-stu-id="39195-146">`signalR.LogLevel.Error` : Error messages.</span></span> <span data-ttu-id="39195-147">Journaux `Error` messages uniquement.</span><span class="sxs-lookup"><span data-stu-id="39195-147">Logs `Error` messages only.</span></span>
* <span data-ttu-id="39195-148">`signalR.LogLevel.Warning` : Messages d’avertissement sur les erreurs potentielles.</span><span class="sxs-lookup"><span data-stu-id="39195-148">`signalR.LogLevel.Warning` : Warning messages about potential errors.</span></span> <span data-ttu-id="39195-149">Journaux `Warning`, et `Error` messages.</span><span class="sxs-lookup"><span data-stu-id="39195-149">Logs `Warning`, and `Error` messages.</span></span>
* <span data-ttu-id="39195-150">`signalR.LogLevel.Information` : Messages d’état sans erreurs.</span><span class="sxs-lookup"><span data-stu-id="39195-150">`signalR.LogLevel.Information` : Status messages without errors.</span></span> <span data-ttu-id="39195-151">Journaux `Information`, `Warning`, et `Error` messages.</span><span class="sxs-lookup"><span data-stu-id="39195-151">Logs `Information`, `Warning`, and `Error` messages.</span></span>
* <span data-ttu-id="39195-152">`signalR.LogLevel.Trace` : Messages de trace.</span><span class="sxs-lookup"><span data-stu-id="39195-152">`signalR.LogLevel.Trace` : Trace messages.</span></span> <span data-ttu-id="39195-153">Enregistre tous les éléments, y compris les données transportées entre le client et du concentrateur.</span><span class="sxs-lookup"><span data-stu-id="39195-153">Logs everything, including data transported between hub and client.</span></span>

<span data-ttu-id="39195-154">Utilisez le `configureLogging` méthode sur `HubConnectionBuilder` pour configurer le niveau de journal.</span><span class="sxs-lookup"><span data-stu-id="39195-154">Use the `configureLogging` method on `HubConnectionBuilder` to configure the log level.</span></span> <span data-ttu-id="39195-155">Les messages sont enregistrés dans la console du navigateur.</span><span class="sxs-lookup"><span data-stu-id="39195-155">Messages are logged to the browser console.</span></span>

[!code-javascript[Logging levels](javascript-client/sample/wwwroot/js/chat.js?range=9-12)]

## <a name="related-resources"></a><span data-ttu-id="39195-156">Ressources connexes</span><span class="sxs-lookup"><span data-stu-id="39195-156">Related resources</span></span>

* [<span data-ttu-id="39195-157">Hubs</span><span class="sxs-lookup"><span data-stu-id="39195-157">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="39195-158">Client .NET</span><span class="sxs-lookup"><span data-stu-id="39195-158">.NET client</span></span>](xref:signalr/dotnet-client)
* [<span data-ttu-id="39195-159">Publier sur Azure</span><span class="sxs-lookup"><span data-stu-id="39195-159">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)
* [<span data-ttu-id="39195-160">Activer les demandes Cross-Origin (CORS) dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="39195-160">Enable Cross-Origin Requests (CORS) in ASP.NET Core</span></span>](xref:security/cors)