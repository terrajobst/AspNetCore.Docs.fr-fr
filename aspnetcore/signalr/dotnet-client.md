---
title: Client .NET SignalR ASP.NET Core
author: bradygaster
description: Informations sur le Client .NET SignalR ASP.NET Core
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 04/17/2019
uid: signalr/dotnet-client
ms.openlocfilehash: 97c553874cb1e4b678fa0e5cd65074f135193861
ms.sourcegitcommit: 4ef0362ef8b6e5426fc5af18f22734158fe587e1
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/17/2019
ms.locfileid: "67153115"
---
# <a name="aspnet-core-signalr-net-client"></a><span data-ttu-id="79641-103">Client .NET SignalR ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="79641-103">ASP.NET Core SignalR .NET Client</span></span>

<span data-ttu-id="79641-104">La bibliothèque cliente ASP.NET Core SignalR .NET vous permet de communiquer avec les hubs SignalR à partir des applications .NET.</span><span class="sxs-lookup"><span data-stu-id="79641-104">The ASP.NET Core SignalR .NET client library lets you communicate with SignalR hubs from .NET apps.</span></span>

<span data-ttu-id="79641-105">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/signalr/dotnet-client/sample) ([procédure de téléchargement](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="79641-105">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/signalr/dotnet-client/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="79641-106">L’exemple de code dans cet article est une application WPF qui utilise le client .NET SignalR ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="79641-106">The code sample in this article is a WPF app that uses the ASP.NET Core SignalR .NET client.</span></span>

## <a name="install-the-signalr-net-client-package"></a><span data-ttu-id="79641-107">Installer le package du client .NET SignalR</span><span class="sxs-lookup"><span data-stu-id="79641-107">Install the SignalR .NET client package</span></span>

<span data-ttu-id="79641-108">Le package `Microsoft.AspNetCore.SignalR.Client` est requis pour que les clients .NET se connectent à des hubs SignalR.</span><span class="sxs-lookup"><span data-stu-id="79641-108">The `Microsoft.AspNetCore.SignalR.Client` package is needed for .NET clients to connect to SignalR hubs.</span></span> <span data-ttu-id="79641-109">Pour installer la bibliothèque cliente, exécutez la commande suivante dans la fenêtre **Console du Gestionnaire de package** :</span><span class="sxs-lookup"><span data-stu-id="79641-109">To install the client library, run the following command in the **Package Manager Console** window:</span></span>

```powershell
Install-Package Microsoft.AspNetCore.SignalR.Client
```

## <a name="connect-to-a-hub"></a><span data-ttu-id="79641-110">Se connecter à un hub</span><span class="sxs-lookup"><span data-stu-id="79641-110">Connect to a hub</span></span>

<span data-ttu-id="79641-111">Pour établir une connexion, créez un `HubConnectionBuilder` et appelez-le `Build`.</span><span class="sxs-lookup"><span data-stu-id="79641-111">To establish a connection, create a `HubConnectionBuilder` and call `Build`.</span></span> <span data-ttu-id="79641-112">L’URL du hub, le protocole, le type de transport, le niveau du journalisation, les en-têtes et les autres options peuvent être configurés lors de la création d’une connexion.</span><span class="sxs-lookup"><span data-stu-id="79641-112">The hub URL, protocol, transport type, log level, headers, and other options can be configured while building a connection.</span></span> <span data-ttu-id="79641-113">Configurez les options requises en insérant les méthodes du `HubConnectionBuilder` dans `Build`.</span><span class="sxs-lookup"><span data-stu-id="79641-113">Configure any required options by inserting any of the `HubConnectionBuilder` methods into `Build`.</span></span> <span data-ttu-id="79641-114">Démarrez la connexion avec `StartAsync`.</span><span class="sxs-lookup"><span data-stu-id="79641-114">Start the connection with `StartAsync`.</span></span>

[!code-csharp[Build hub connection](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_MainWindowClass&highlight=15-17,39)]

## <a name="handle-lost-connection"></a><span data-ttu-id="79641-115">Gérer la perte de connexion</span><span class="sxs-lookup"><span data-stu-id="79641-115">Handle lost connection</span></span>

::: moniker range=">= aspnetcore-3.0"

### <a name="automatically-reconnect"></a><span data-ttu-id="79641-116">Se reconnecter automatiquement</span><span class="sxs-lookup"><span data-stu-id="79641-116">Automatically reconnect</span></span>

<span data-ttu-id="79641-117">Le <xref:Microsoft.AspNetCore.SignalR.Client.HubConnection> peuvent être configurés pour se reconnecter automatiquement à l’aide de la `WithAutomaticReconnect` méthode sur le <xref:Microsoft.AspNetCore.SignalR.Client.HubConnectionBuilder>.</span><span class="sxs-lookup"><span data-stu-id="79641-117">The <xref:Microsoft.AspNetCore.SignalR.Client.HubConnection> can be configured to automatically reconnect using the `WithAutomaticReconnect` method on the <xref:Microsoft.AspNetCore.SignalR.Client.HubConnectionBuilder>.</span></span> <span data-ttu-id="79641-118">Il ne se reconnecter automatiquement par défaut.</span><span class="sxs-lookup"><span data-stu-id="79641-118">It won't automatically reconnect by default.</span></span>

```csharp
HubConnection connection= new HubConnectionBuilder()
    .WithUrl(new Uri("http://127.0.0.1:5000/chatHub"))
    .WithAutomaticReconnect()
    .Build();
```

<span data-ttu-id="79641-119">Sans paramètres, `WithAutomaticReconnect()` configure le client en attente 0, 2, 10 et 30 secondes respectivement avant d’essayer de chaque tentative de reconnexion s’arrête après avoir quatre tentatives ayant échoué.</span><span class="sxs-lookup"><span data-stu-id="79641-119">Without any parameters, `WithAutomaticReconnect()` configures the client to wait 0, 2, 10, and 30 seconds respectively before trying each reconnect attempt, stopping after four failed attempts.</span></span>

<span data-ttu-id="79641-120">Avant de commencer toute tentative de reconnexion, le `HubConnection` sera la transition vers le `HubConnectionState.Reconnecting` d’état et de déclencher le `Reconnecting` événement.</span><span class="sxs-lookup"><span data-stu-id="79641-120">Before starting any reconnect attempts, the `HubConnection` will transition to the `HubConnectionState.Reconnecting` state and fire the `Reconnecting` event.</span></span>  <span data-ttu-id="79641-121">Cela fournit une opportunité pour avertir les utilisateurs que la connexion a été perdue et désactiver les éléments d’interface utilisateur.</span><span class="sxs-lookup"><span data-stu-id="79641-121">This provides an opportunity to warn users that the connection has been lost and to disable UI elements.</span></span> <span data-ttu-id="79641-122">Les applications non interactives peuvent démarrer queuing ou la suppression des messages.</span><span class="sxs-lookup"><span data-stu-id="79641-122">Non-interactive apps can start queuing or dropping messages.</span></span>

```csharp
connection.Reconnecting += error =>
{
    Debug.Assert(connection.State == HubConnectionState.Reconnecting);

    // Notify users the connection was lost and the client is reconnecting.
    // Start queuing or dropping messages.

    return Task.CompletedTask;
};
```

<span data-ttu-id="79641-123">Si le client se reconnecte avec succès au sein de ses quatre premières tentatives, le `HubConnection` passera à le `Connected` d’état et de déclencher le `Reconnected` événement.</span><span class="sxs-lookup"><span data-stu-id="79641-123">If the client successfully reconnects within its first four attempts, the `HubConnection` will transition back to the `Connected` state and fire the `Reconnected` event.</span></span> <span data-ttu-id="79641-124">Cela permet d’informer les utilisateurs de la connexion a été rétablie et tous les messages en file d’attente de la file d’attente.</span><span class="sxs-lookup"><span data-stu-id="79641-124">This provides an opportunity to inform users the connection has been reestablished and dequeue any queued messages.</span></span>

<span data-ttu-id="79641-125">Étant donné que la connexion est entièrement nouveau sur le serveur, un nouveau `ConnectionId` seront fournies à la `Reconnected` gestionnaires d’événements.</span><span class="sxs-lookup"><span data-stu-id="79641-125">Since the connection looks entirely new to the server, a new `ConnectionId` will be provided to the `Reconnected` event handlers.</span></span>

> [!WARNING]
> <span data-ttu-id="79641-126">Le `Reconnected` du Gestionnaire d’événements `connectionId` paramètre sera null si le `HubConnection` a été configuré pour [ignorer négociation](xref:signalr/configuration#configure-client-options).</span><span class="sxs-lookup"><span data-stu-id="79641-126">The `Reconnected` event handler's `connectionId` parameter will be null if the `HubConnection` was configured to [skip negotiation](xref:signalr/configuration#configure-client-options).</span></span>

```csharp
connection.Reconnected += connectionId =>
{
    Debug.Assert(connection.State == HubConnectionState.Connected);

    // Notify users the connection was reestablished.
    // Start dequeuing messages queued while reconnecting if any.

    return Task.CompletedTask;
};
```

<span data-ttu-id="79641-127">`WithAutomaticReconnect()` ne configurez pas le `HubConnection` pour réessayer d’échecs de démarrage initial, afin de l’échec de démarrage doivent être traitées manuellement :</span><span class="sxs-lookup"><span data-stu-id="79641-127">`WithAutomaticReconnect()` won't configure the `HubConnection` to retry initial start failures, so start failures need to be handled manually:</span></span>

```csharp
public static async Task<bool> ConnectWithRetryAsync(HubConnection connection, CancellationToken token)
{
    // Keep trying to until we can start or the token is canceled.
    while (true)
    {
        try
        {
            await connection.StartAsync(token);
            Debug.Assert(connection.State == HubConnectionState.Connected);
            return true;
        }
        catch when (token.IsCancellationRequested)
        {
            return false;
        }
        catch
        {
            // Failed to connect, trying again in 5000 ms.
            Debug.Assert(connection.State == HubConnectionState.Disconnected);
            await Task.Delay(5000);
        }
    }
}
```

<span data-ttu-id="79641-128">Si le client ne se reconnecter avec succès au sein de ses quatre premières tentatives, le `HubConnection` sera la transition vers le `Disconnected` d’état et de déclencher le <xref:Microsoft.AspNetCore.SignalR.Client.HubConnection.Closed> événement.</span><span class="sxs-lookup"><span data-stu-id="79641-128">If the client doesn't successfully reconnect within its first four attempts, the `HubConnection` will transition to the `Disconnected` state and fire the <xref:Microsoft.AspNetCore.SignalR.Client.HubConnection.Closed> event.</span></span> <span data-ttu-id="79641-129">Cela fournit une opportunité pour tenter de redémarrer la connexion manuellement ou informer les utilisateurs de que la connexion a été définitivement perdue.</span><span class="sxs-lookup"><span data-stu-id="79641-129">This provides an opportunity to attempt to restart the connection manually or inform users the connection has been permanently lost.</span></span>

```csharp
connection.Closed += error =>
{
    Debug.Assert(connection.State == HubConnectionState.Disconnected);

    // Notify users the connection has been closed or manually try to restart the connection.

    return Task.CompletedTask;
};
```

<span data-ttu-id="79641-130">Pour configurer le nombre de tentatives de reconnexion avant de déconnecter ou de modifier le minutage de la reconnexion, `WithAutomaticReconnect` accepte un tableau de nombres représentant le délai en millisecondes à attendre avant de commencer chaque tentative de reconnexion.</span><span class="sxs-lookup"><span data-stu-id="79641-130">In order to configure a custom number of reconnect attempts before disconnecting or change the reconnect timing, `WithAutomaticReconnect` accepts an array of numbers representing the delay in milliseconds to wait before starting each reconnect attempt.</span></span>

```csharp
HubConnection connection= new HubConnectionBuilder()
    .WithUrl(new Uri("http://127.0.0.1:5000/chatHub"))
    .WithAutomaticReconnect(new[] { TimeSpan.Zero, TimeSpan.Zero, TimeSpan.FromSeconds(10) })
    .Build();

    // .WithAutomaticReconnect(new[] { TimeSpan.Zero, TimeSpan.FromSeconds(2), TimeSpan.FromSeconds(10), TimeSpan.FromSeconds(30) }) yields the default behavior.
```

<span data-ttu-id="79641-131">L’exemple précédent configure le `HubConnection` pour démarrer une tentative de reconnexions immédiatement après la connexion est perdue.</span><span class="sxs-lookup"><span data-stu-id="79641-131">The preceding example configures the `HubConnection` to start attempting reconnects immediately after the connection is lost.</span></span> <span data-ttu-id="79641-132">Cela vaut également pour la configuration par défaut.</span><span class="sxs-lookup"><span data-stu-id="79641-132">This is also true for the default configuration.</span></span>

<span data-ttu-id="79641-133">Si la première tentative de reconnexion échoue, la deuxième tentative de reconnexion également démarre immédiatement au lieu d’attendre 2 secondes comme il le ferait dans la configuration par défaut.</span><span class="sxs-lookup"><span data-stu-id="79641-133">If the first reconnect attempt fails, the second reconnect attempt will also start immediately instead of waiting 2 seconds like it would in the default configuration.</span></span>

<span data-ttu-id="79641-134">Si la deuxième tentative de reconnexion échoue, la troisième tentative de reconnexion démarrera dans 10 secondes, ce qui revient à nouveau à la configuration par défaut.</span><span class="sxs-lookup"><span data-stu-id="79641-134">If the second reconnect attempt fails, the third reconnect attempt will start in 10 seconds which is again like the default configuration.</span></span>

<span data-ttu-id="79641-135">Le comportement personnalisé puis diffère à nouveau le comportement par défaut en arrêtant après que la reconnexion troisième tentative d’échec.</span><span class="sxs-lookup"><span data-stu-id="79641-135">The custom behavior then diverges again from the default behavior by stopping after the third reconnect attempt failure.</span></span> <span data-ttu-id="79641-136">Dans la configuration par défaut il être un plus reconnectait tentative dans les 30 secondes.</span><span class="sxs-lookup"><span data-stu-id="79641-136">In the default configuration there would be one more reconnect attempt in another 30 seconds.</span></span>

<span data-ttu-id="79641-137">Si vous souhaitez mieux contrôler la fréquence et le nombre d’automatique nouvelle tentative, `WithAutomaticReconnect` accepte un objet qui implémente le `IRetryPolicy` interface, ce qui a une méthode unique nommée `NextRetryDelay`.</span><span class="sxs-lookup"><span data-stu-id="79641-137">If you want even more control over the timing and number of automatic reconnect attempts, `WithAutomaticReconnect` accepts an object implementing the `IRetryPolicy` interface, which has a single method named `NextRetryDelay`.</span></span>

<span data-ttu-id="79641-138">`NextRetryDelay` accepte un seul argument avec le type `RetryContext`.</span><span class="sxs-lookup"><span data-stu-id="79641-138">`NextRetryDelay` takes a single argument with the type `RetryContext`.</span></span> <span data-ttu-id="79641-139">Le `RetryContext` possède trois propriétés : `PreviousRetryCount`, `ElapsedTime` et `RetryReason` qui sont un `long`, un `TimeSpan` et un `Exception` respectivement.</span><span class="sxs-lookup"><span data-stu-id="79641-139">The `RetryContext` has three properties: `PreviousRetryCount`, `ElapsedTime` and `RetryReason` which are a `long`, a `TimeSpan` and an `Exception` respectively.</span></span> <span data-ttu-id="79641-140">Avant la première tentative de reconnexion, les deux `PreviousRetryCount` et `ElapsedTime` sera égal à zéro et le `RetryReason` sera l’Exception qui a provoqué la connexion est perdue.</span><span class="sxs-lookup"><span data-stu-id="79641-140">Before the first reconnect attempt, both `PreviousRetryCount` and `ElapsedTime` will be zero, and the `RetryReason` will be the Exception that caused the connection to be lost.</span></span> <span data-ttu-id="79641-141">Après chaque tentative ayant échoué, `PreviousRetryCount` sera incrémenté d’un, `ElapsedTime` sera mis à jour pour refléter la quantité de temps passé à la reconnexion jusqu’ici et le `RetryReason` sera l’Exception qui a provoqué la dernière tentative de reconnexion d’échouer.</span><span class="sxs-lookup"><span data-stu-id="79641-141">After each failed retry attempt, `PreviousRetryCount` will be incremented by one, `ElapsedTime` will be updated to reflect the amount of time spent reconnecting so far, and the `RetryReason` will be the Exception that caused the last reconnect attempt to fail.</span></span>

<span data-ttu-id="79641-142">`NextRetryDelay` doit retourner soit un laps de temps représentant le temps à attendre avant la prochaine tentative de reconnexion ou `null` si le `HubConnection` doit s’arrêter la reconnexion.</span><span class="sxs-lookup"><span data-stu-id="79641-142">`NextRetryDelay` must return either a TimeSpan representing the time to wait before the next reconnect attempt or `null` if the `HubConnection` should stop reconnecting.</span></span>

```csharp
public class RandomRetryPolicy : IRetryPolicy
{
    private readonly Random _random = new Random();

    public TimeSpan? NextRetryDelay(RetryContext retryContext)
    {
        // If we've been reconnecting for less than 60 seconds so far,
        // wait between 0 and 10 seconds before the next reconnect attempt.
        if (retryContext.ElapsedTime < TimeSpan.FromSeconds(60))
        {
            return TimeSpan.FromSeconds(_random.Next() * 10);
        }
        else
        {
            // If we've been reconnecting for more than 60 seconds so far, stop reconnecting.
            return null;
        }
    }
}
```

```csharp
HubConnection connection = new HubConnectionBuilder()
    .WithUrl(new Uri("http://127.0.0.1:5000/chatHub"))
    .WithAutomaticReconnect(new RandomRetryPolicy())
    .Build();
```

<span data-ttu-id="79641-143">Vous pouvez également écrire le code qui se reconnectera votre client manuellement, comme illustré dans [reconnecter manuellement](#manually-reconnect).</span><span class="sxs-lookup"><span data-stu-id="79641-143">Alternatively, you can write code that will reconnect your client manually as demonstrated in [Manually reconnect](#manually-reconnect).</span></span>

::: moniker-end

### <a name="manually-reconnect"></a><span data-ttu-id="79641-144">Reconnecter manuellement</span><span class="sxs-lookup"><span data-stu-id="79641-144">Manually reconnect</span></span>

::: moniker range="< aspnetcore-3.0"

> [!WARNING]
> <span data-ttu-id="79641-145">Avant la version 3.0, le client .NET pour SignalR ne se reconnecter automatiquement.</span><span class="sxs-lookup"><span data-stu-id="79641-145">Prior to 3.0, the .NET client for SignalR doesn't automatically reconnect.</span></span> <span data-ttu-id="79641-146">Vous devez écrire du code qui se reconnectera votre client manuellement.</span><span class="sxs-lookup"><span data-stu-id="79641-146">You must write code that will reconnect your client manually.</span></span>

::: moniker-end

<span data-ttu-id="79641-147">Utilisez l'événement <xref:Microsoft.AspNetCore.SignalR.Client.HubConnection.Closed> pour répondre à une perte de connexion.</span><span class="sxs-lookup"><span data-stu-id="79641-147">Use the <xref:Microsoft.AspNetCore.SignalR.Client.HubConnection.Closed> event to respond to a lost connection.</span></span> <span data-ttu-id="79641-148">Par exemple, vous pouvez souhaiter automatiser une reconnexion.</span><span class="sxs-lookup"><span data-stu-id="79641-148">For example, you might want to automate reconnection.</span></span>

<span data-ttu-id="79641-149">L'événement `Closed` nécessite un délégué qui retourne une `Task`, ce qui permet l’exécution sans utiliser de code asynchrone `async void`.</span><span class="sxs-lookup"><span data-stu-id="79641-149">The `Closed` event requires a delegate that returns a `Task`, which allows async code to run without using `async void`.</span></span> <span data-ttu-id="79641-150">Pour répondre à la signature du délégué dans un gestionnaire d’événements `Closed` qui s’exécute de façon synchrone, retournez `Task.CompletedTask`:</span><span class="sxs-lookup"><span data-stu-id="79641-150">To satisfy the delegate signature in a `Closed` event handler that runs synchronously, return `Task.CompletedTask`:</span></span>

```csharp
connection.Closed += (error) => {
    // Do your close logic.
    return Task.CompletedTask;
};
```

<span data-ttu-id="79641-151">La principale raison de la prise en charge asynchrone est de pouvoir redémarrer la connexion.</span><span class="sxs-lookup"><span data-stu-id="79641-151">The main reason for the async support is so you can restart the connection.</span></span> <span data-ttu-id="79641-152">Le démarrage d’une connexion est une action asynchrone.</span><span class="sxs-lookup"><span data-stu-id="79641-152">Starting a connection is an async action.</span></span>

<span data-ttu-id="79641-153">Dans un gestionnaire `Closed` qui redémarre la connexion, envisagez d’attendre un délai aléatoire afin d'éviter de surcharger le serveur, comme indiqué dans l’exemple suivant :</span><span class="sxs-lookup"><span data-stu-id="79641-153">In a `Closed` handler that restarts the connection, consider waiting for some random delay to prevent overloading the server, as shown in the following example:</span></span>

[!code-csharp[Use Closed event handler to automate reconnection](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_ClosedRestart)]

## <a name="call-hub-methods-from-client"></a><span data-ttu-id="79641-154">Appeler des méthodes de hub à partir du client</span><span class="sxs-lookup"><span data-stu-id="79641-154">Call hub methods from client</span></span>

<span data-ttu-id="79641-155">`InvokeAsync` appelle des méthodes sur le hub.</span><span class="sxs-lookup"><span data-stu-id="79641-155">`InvokeAsync` calls methods on the hub.</span></span> <span data-ttu-id="79641-156">Passez le nom de la méthode de hub et de tous les arguments définis dans la méthode de hub à `InvokeAsync`.</span><span class="sxs-lookup"><span data-stu-id="79641-156">Pass the hub method name and any arguments defined in the hub method to `InvokeAsync`.</span></span> <span data-ttu-id="79641-157">SignalR est asynchrone, par conséquent, utilisez `async` et `await` lors de l’appel.</span><span class="sxs-lookup"><span data-stu-id="79641-157">SignalR is asynchronous, so use `async` and `await` when making the calls.</span></span>

[!code-csharp[InvokeAsync method](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_InvokeAsync)]

<span data-ttu-id="79641-158">Le `InvokeAsync` méthode retourne un `Task` qui se termine lorsque la méthode de serveur est retournée.</span><span class="sxs-lookup"><span data-stu-id="79641-158">The `InvokeAsync` method returns a `Task` which completes when the server method returns.</span></span> <span data-ttu-id="79641-159">La valeur de retour, le cas échéant, est fournie en tant que le résultat de la `Task`.</span><span class="sxs-lookup"><span data-stu-id="79641-159">The return value, if any, is provided as the result of the `Task`.</span></span> <span data-ttu-id="79641-160">Toutes les exceptions levées par la méthode sur le serveur génère une erreur `Task`.</span><span class="sxs-lookup"><span data-stu-id="79641-160">Any exceptions thrown by the method on the server produce a faulted `Task`.</span></span> <span data-ttu-id="79641-161">Utilisez `await` syntaxe d’attente de la méthode de serveur pour terminer et `try...catch` syntaxe permettant de gérer les erreurs.</span><span class="sxs-lookup"><span data-stu-id="79641-161">Use `await` syntax to wait for the server method to complete and `try...catch` syntax to handle errors.</span></span>

<span data-ttu-id="79641-162">Le `SendAsync` méthode retourne un `Task` qui se termine lorsque le message a été envoyé au serveur.</span><span class="sxs-lookup"><span data-stu-id="79641-162">The `SendAsync` method returns a `Task` which completes when the message has been sent to the server.</span></span> <span data-ttu-id="79641-163">Aucune valeur de retour n’est fourni, car ce `Task` n’attendez la fin de la méthode de serveur.</span><span class="sxs-lookup"><span data-stu-id="79641-163">No return value is provided since this `Task` doesn't wait until the server method completes.</span></span> <span data-ttu-id="79641-164">Toutes les exceptions levées sur le client lors de l’envoi du message génère une erreur `Task`.</span><span class="sxs-lookup"><span data-stu-id="79641-164">Any exceptions thrown on the client while sending the message produce a faulted `Task`.</span></span> <span data-ttu-id="79641-165">Utilisez `await` et `try...catch` syntaxe pour gérer les erreurs d’envoi.</span><span class="sxs-lookup"><span data-stu-id="79641-165">Use `await` and `try...catch` syntax to handle send errors.</span></span>

> [!NOTE]
> <span data-ttu-id="79641-166">Si vous utilisez le Service Azure SignalR dans *mode sans serveur*, vous ne pouvez pas appeler des méthodes de concentrateur à partir d’un client.</span><span class="sxs-lookup"><span data-stu-id="79641-166">If you're using Azure SignalR Service in *Serverless mode*, you cannot call hub methods from a client.</span></span> <span data-ttu-id="79641-167">Pour plus d’informations, consultez le [documentation de SignalR Service](/azure/azure-signalr/signalr-concept-serverless-development-config).</span><span class="sxs-lookup"><span data-stu-id="79641-167">For more information, see the [SignalR Service documentation](/azure/azure-signalr/signalr-concept-serverless-development-config).</span></span>

## <a name="call-client-methods-from-hub"></a><span data-ttu-id="79641-168">Appeler des méthodes de client à partir de hub</span><span class="sxs-lookup"><span data-stu-id="79641-168">Call client methods from hub</span></span>

<span data-ttu-id="79641-169">Définissez les méthodes appelées par le hub en utilisant `connection.On` après la génération, mais avant de démarrer la connexion.</span><span class="sxs-lookup"><span data-stu-id="79641-169">Define methods the hub calls using `connection.On` after building, but before starting the connection.</span></span>

[!code-csharp[Define client methods](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_ConnectionOn)]

<span data-ttu-id="79641-170">Le code précédent dans `connection.On` s’exécute lorsque le code côté serveur l’appelle en utilisant la méthode `SendAsync`.</span><span class="sxs-lookup"><span data-stu-id="79641-170">The preceding code in `connection.On` runs when server-side code calls it using the `SendAsync` method.</span></span>

[!code-csharp[Call client method](dotnet-client/sample/signalrchat/hubs/chathub.cs?name=snippet_SendMessage)]

## <a name="error-handling-and-logging"></a><span data-ttu-id="79641-171">Journalisation et gestion des erreurs</span><span class="sxs-lookup"><span data-stu-id="79641-171">Error handling and logging</span></span>

<span data-ttu-id="79641-172">Gérez les erreurs avec une instruction try-catch.</span><span class="sxs-lookup"><span data-stu-id="79641-172">Handle errors with a try-catch statement.</span></span> <span data-ttu-id="79641-173">Inspectez l'objet `Exception` afin de déterminer l’action appropriée à entreprendre après qu'une erreur se produit.</span><span class="sxs-lookup"><span data-stu-id="79641-173">Inspect the `Exception` object to determine the proper action to take after an error occurs.</span></span>

[!code-csharp[Logging](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_ErrorHandling)]

## <a name="additional-resources"></a><span data-ttu-id="79641-174">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="79641-174">Additional resources</span></span>

* [<span data-ttu-id="79641-175">Hubs</span><span class="sxs-lookup"><span data-stu-id="79641-175">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="79641-176">Client JavaScript</span><span class="sxs-lookup"><span data-stu-id="79641-176">JavaScript client</span></span>](xref:signalr/javascript-client)
* [<span data-ttu-id="79641-177">Publier sur Azure</span><span class="sxs-lookup"><span data-stu-id="79641-177">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)
* [<span data-ttu-id="79641-178">Documentation de serverless Azure SignalR Service</span><span class="sxs-lookup"><span data-stu-id="79641-178">Azure SignalR Service serverless documentation</span></span>](/azure/azure-signalr/signalr-concept-serverless-development-config)
