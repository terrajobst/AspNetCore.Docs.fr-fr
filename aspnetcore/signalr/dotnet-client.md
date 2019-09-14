---
title: Client .NET SignalR ASP.NET Core
author: bradygaster
description: Informations sur le Client .NET SignalR ASP.NET Core
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 09/13/2019
uid: signalr/dotnet-client
ms.openlocfilehash: d2755f652e734bad6447ddeb9a82345dcde25b28
ms.sourcegitcommit: 805f625d16d74e77f02f5f37326e5aceafcb78e3
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/13/2019
ms.locfileid: "70985494"
---
# <a name="aspnet-core-signalr-net-client"></a><span data-ttu-id="276a4-103">Client .NET SignalR ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="276a4-103">ASP.NET Core SignalR .NET Client</span></span>

<span data-ttu-id="276a4-104">La bibliothèque cliente ASP.NET Core SignalR .NET vous permet de communiquer avec les hubs SignalR à partir des applications .NET.</span><span class="sxs-lookup"><span data-stu-id="276a4-104">The ASP.NET Core SignalR .NET client library lets you communicate with SignalR hubs from .NET apps.</span></span>

<span data-ttu-id="276a4-105">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/signalr/dotnet-client/sample) ([procédure de téléchargement](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="276a4-105">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/signalr/dotnet-client/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="276a4-106">L’exemple de code dans cet article est une application WPF qui utilise le client .NET SignalR ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="276a4-106">The code sample in this article is a WPF app that uses the ASP.NET Core SignalR .NET client.</span></span>

## <a name="install-the-signalr-net-client-package"></a><span data-ttu-id="276a4-107">Installer le package du client .NET SignalR</span><span class="sxs-lookup"><span data-stu-id="276a4-107">Install the SignalR .NET client package</span></span>

<span data-ttu-id="276a4-108">Le package [Microsoft. AspNetCore. signalr. client](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR.Client) est requis pour que les clients .net se connectent aux hubs signalr.</span><span class="sxs-lookup"><span data-stu-id="276a4-108">The [Microsoft.AspNetCore.SignalR.Client](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR.Client) package is required for .NET clients to connect to SignalR hubs.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="276a4-109">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="276a4-109">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="276a4-110">Pour installer la bibliothèque cliente, exécutez la commande suivante dans la fenêtre **Console du Gestionnaire de package** :</span><span class="sxs-lookup"><span data-stu-id="276a4-110">To install the client library, run the following command in the **Package Manager Console** window:</span></span>

```powershell
Install-Package Microsoft.AspNetCore.SignalR.Client
```

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="276a4-111">CLI .NET Core</span><span class="sxs-lookup"><span data-stu-id="276a4-111">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="276a4-112">Pour installer la bibliothèque cliente, exécutez la commande suivante dans une interface de commande :</span><span class="sxs-lookup"><span data-stu-id="276a4-112">To install the client library, run the following command in a command shell:</span></span>

```console
dotnet add package Microsoft.AspNetCore.SignalR.Client
```

---

## <a name="connect-to-a-hub"></a><span data-ttu-id="276a4-113">Se connecter à un hub</span><span class="sxs-lookup"><span data-stu-id="276a4-113">Connect to a hub</span></span>

<span data-ttu-id="276a4-114">Pour établir une connexion, créez un `HubConnectionBuilder` et appelez-le `Build`.</span><span class="sxs-lookup"><span data-stu-id="276a4-114">To establish a connection, create a `HubConnectionBuilder` and call `Build`.</span></span> <span data-ttu-id="276a4-115">L’URL du hub, le protocole, le type de transport, le niveau du journalisation, les en-têtes et les autres options peuvent être configurés lors de la création d’une connexion.</span><span class="sxs-lookup"><span data-stu-id="276a4-115">The hub URL, protocol, transport type, log level, headers, and other options can be configured while building a connection.</span></span> <span data-ttu-id="276a4-116">Configurez les options requises en insérant les méthodes du `HubConnectionBuilder` dans `Build`.</span><span class="sxs-lookup"><span data-stu-id="276a4-116">Configure any required options by inserting any of the `HubConnectionBuilder` methods into `Build`.</span></span> <span data-ttu-id="276a4-117">Démarrez la connexion avec `StartAsync`.</span><span class="sxs-lookup"><span data-stu-id="276a4-117">Start the connection with `StartAsync`.</span></span>

[!code-csharp[Build hub connection](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_MainWindowClass&highlight=15-17,39)]

## <a name="handle-lost-connection"></a><span data-ttu-id="276a4-118">Gérer la connexion perdue</span><span class="sxs-lookup"><span data-stu-id="276a4-118">Handle lost connection</span></span>

::: moniker range=">= aspnetcore-3.0"

### <a name="automatically-reconnect"></a><span data-ttu-id="276a4-119">Se reconnecter automatiquement</span><span class="sxs-lookup"><span data-stu-id="276a4-119">Automatically reconnect</span></span>

<span data-ttu-id="276a4-120">Peut être configuré pour se reconnecter automatiquement à l' `WithAutomaticReconnect` aide de la méthode sur le. <xref:Microsoft.AspNetCore.SignalR.Client.HubConnectionBuilder> <xref:Microsoft.AspNetCore.SignalR.Client.HubConnection></span><span class="sxs-lookup"><span data-stu-id="276a4-120">The <xref:Microsoft.AspNetCore.SignalR.Client.HubConnection> can be configured to automatically reconnect using the `WithAutomaticReconnect` method on the <xref:Microsoft.AspNetCore.SignalR.Client.HubConnectionBuilder>.</span></span> <span data-ttu-id="276a4-121">Par défaut, il ne se reconnectera pas automatiquement.</span><span class="sxs-lookup"><span data-stu-id="276a4-121">It won't automatically reconnect by default.</span></span>

```csharp
HubConnection connection= new HubConnectionBuilder()
    .WithUrl(new Uri("http://127.0.0.1:5000/chatHub"))
    .WithAutomaticReconnect()
    .Build();
```

<span data-ttu-id="276a4-122">Sans aucun paramètre, `WithAutomaticReconnect()` configure le client pour attendre 0, 2, 10 et 30 secondes, respectivement, avant d’essayer chaque tentative de reconnexion, en s’arrêtant après quatre tentatives ayant échoué.</span><span class="sxs-lookup"><span data-stu-id="276a4-122">Without any parameters, `WithAutomaticReconnect()` configures the client to wait 0, 2, 10, and 30 seconds respectively before trying each reconnect attempt, stopping after four failed attempts.</span></span>

<span data-ttu-id="276a4-123">Avant de commencer les tentatives de reconnexion `HubConnection` , le passe à `HubConnectionState.Reconnecting` l’État et déclenche `Reconnecting` l’événement.</span><span class="sxs-lookup"><span data-stu-id="276a4-123">Before starting any reconnect attempts, the `HubConnection` will transition to the `HubConnectionState.Reconnecting` state and fire the `Reconnecting` event.</span></span>  <span data-ttu-id="276a4-124">Cela permet d’avertir les utilisateurs que la connexion a été perdue et de désactiver les éléments d’interface utilisateur.</span><span class="sxs-lookup"><span data-stu-id="276a4-124">This provides an opportunity to warn users that the connection has been lost and to disable UI elements.</span></span> <span data-ttu-id="276a4-125">Les applications non interactives peuvent commencer à mettre en file d’attente ou à déposer des messages.</span><span class="sxs-lookup"><span data-stu-id="276a4-125">Non-interactive apps can start queuing or dropping messages.</span></span>

```csharp
connection.Reconnecting += error =>
{
    Debug.Assert(connection.State == HubConnectionState.Reconnecting);

    // Notify users the connection was lost and the client is reconnecting.
    // Start queuing or dropping messages.

    return Task.CompletedTask;
};
```

<span data-ttu-id="276a4-126">Si le client se reconnecte correctement dans les quatre premières tentatives, `HubConnection` le passe à l' `Connected` État et déclenche l' `Reconnected` événement.</span><span class="sxs-lookup"><span data-stu-id="276a4-126">If the client successfully reconnects within its first four attempts, the `HubConnection` will transition back to the `Connected` state and fire the `Reconnected` event.</span></span> <span data-ttu-id="276a4-127">Cela permet d’informer les utilisateurs que la connexion a été rétablie et de défiler tous les messages mis en file d’attente.</span><span class="sxs-lookup"><span data-stu-id="276a4-127">This provides an opportunity to inform users the connection has been reestablished and dequeue any queued messages.</span></span>

<span data-ttu-id="276a4-128">Étant donné que la connexion semble entièrement nouvelle sur le serveur, `ConnectionId` une nouvelle est fournie `Reconnected` aux gestionnaires d’événements.</span><span class="sxs-lookup"><span data-stu-id="276a4-128">Since the connection looks entirely new to the server, a new `ConnectionId` will be provided to the `Reconnected` event handlers.</span></span>

> [!WARNING]
> <span data-ttu-id="276a4-129">Le `Reconnected` paramètre du gestionnaire `connectionId` d’événements sera null si a `HubConnection` été configuré pour [ignorer la négociation](xref:signalr/configuration#configure-client-options).</span><span class="sxs-lookup"><span data-stu-id="276a4-129">The `Reconnected` event handler's `connectionId` parameter will be null if the `HubConnection` was configured to [skip negotiation](xref:signalr/configuration#configure-client-options).</span></span>

```csharp
connection.Reconnected += connectionId =>
{
    Debug.Assert(connection.State == HubConnectionState.Connected);

    // Notify users the connection was reestablished.
    // Start dequeuing messages queued while reconnecting if any.

    return Task.CompletedTask;
};
```

<span data-ttu-id="276a4-130">`WithAutomaticReconnect()`ne configure `HubConnection` pas le pour réessayer les échecs de démarrage initial. les échecs de démarrage doivent donc être gérés manuellement:</span><span class="sxs-lookup"><span data-stu-id="276a4-130">`WithAutomaticReconnect()` won't configure the `HubConnection` to retry initial start failures, so start failures need to be handled manually:</span></span>

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

<span data-ttu-id="276a4-131">Si le client ne se reconnecte pas correctement dans les quatre premières `HubConnection` tentatives, le passe `Disconnected` à l’État et <xref:Microsoft.AspNetCore.SignalR.Client.HubConnection.Closed> déclenche l’événement.</span><span class="sxs-lookup"><span data-stu-id="276a4-131">If the client doesn't successfully reconnect within its first four attempts, the `HubConnection` will transition to the `Disconnected` state and fire the <xref:Microsoft.AspNetCore.SignalR.Client.HubConnection.Closed> event.</span></span> <span data-ttu-id="276a4-132">Cela offre la possibilité de tenter de redémarrer manuellement la connexion ou d’informer les utilisateurs que la connexion a été définitivement perdue.</span><span class="sxs-lookup"><span data-stu-id="276a4-132">This provides an opportunity to attempt to restart the connection manually or inform users the connection has been permanently lost.</span></span>

```csharp
connection.Closed += error =>
{
    Debug.Assert(connection.State == HubConnectionState.Disconnected);

    // Notify users the connection has been closed or manually try to restart the connection.

    return Task.CompletedTask;
};
```

<span data-ttu-id="276a4-133">Pour configurer un nombre personnalisé de tentatives de reconnexion avant de vous déconnecter ou de modifier le délai de reconnexion, `WithAutomaticReconnect` accepte un tableau de nombres représentant le délai en millisecondes à attendre avant de démarrer chaque tentative de reconnexion.</span><span class="sxs-lookup"><span data-stu-id="276a4-133">In order to configure a custom number of reconnect attempts before disconnecting or change the reconnect timing, `WithAutomaticReconnect` accepts an array of numbers representing the delay in milliseconds to wait before starting each reconnect attempt.</span></span>

```csharp
HubConnection connection= new HubConnectionBuilder()
    .WithUrl(new Uri("http://127.0.0.1:5000/chatHub"))
    .WithAutomaticReconnect(new[] { TimeSpan.Zero, TimeSpan.Zero, TimeSpan.FromSeconds(10) })
    .Build();

    // .WithAutomaticReconnect(new[] { TimeSpan.Zero, TimeSpan.FromSeconds(2), TimeSpan.FromSeconds(10), TimeSpan.FromSeconds(30) }) yields the default behavior.
```

<span data-ttu-id="276a4-134">L’exemple précédent configure le pour `HubConnection` qu’il commence à tenter de se reconnecter immédiatement après la perte de la connexion.</span><span class="sxs-lookup"><span data-stu-id="276a4-134">The preceding example configures the `HubConnection` to start attempting reconnects immediately after the connection is lost.</span></span> <span data-ttu-id="276a4-135">Cela est également vrai pour la configuration par défaut.</span><span class="sxs-lookup"><span data-stu-id="276a4-135">This is also true for the default configuration.</span></span>

<span data-ttu-id="276a4-136">Si la première tentative de reconnexion échoue, la deuxième tentative de reconnexion démarre également immédiatement au lieu d’attendre 2 secondes comme dans la configuration par défaut.</span><span class="sxs-lookup"><span data-stu-id="276a4-136">If the first reconnect attempt fails, the second reconnect attempt will also start immediately instead of waiting 2 seconds like it would in the default configuration.</span></span>

<span data-ttu-id="276a4-137">Si la deuxième tentative de reconnexion échoue, la troisième tentative de reconnexion démarrera dans 10 secondes, ce qui revient à la configuration par défaut.</span><span class="sxs-lookup"><span data-stu-id="276a4-137">If the second reconnect attempt fails, the third reconnect attempt will start in 10 seconds which is again like the default configuration.</span></span>

<span data-ttu-id="276a4-138">Le comportement personnalisé s’est ensuite dévergé du comportement par défaut en s’arrêtant après le troisième échec de tentative de reconnexion.</span><span class="sxs-lookup"><span data-stu-id="276a4-138">The custom behavior then diverges again from the default behavior by stopping after the third reconnect attempt failure.</span></span> <span data-ttu-id="276a4-139">Dans la configuration par défaut, il y aura une autre tentative de reconnexion dans 30 secondes.</span><span class="sxs-lookup"><span data-stu-id="276a4-139">In the default configuration there would be one more reconnect attempt in another 30 seconds.</span></span>

<span data-ttu-id="276a4-140">Si vous souhaitez encore plus de contrôle sur le minutage et le nombre de tentatives de reconnexion automatique, `WithAutomaticReconnect` accepte un objet qui implémente l' `IRetryPolicy` interface, qui `NextRetryDelay`a une méthode unique nommée.</span><span class="sxs-lookup"><span data-stu-id="276a4-140">If you want even more control over the timing and number of automatic reconnect attempts, `WithAutomaticReconnect` accepts an object implementing the `IRetryPolicy` interface, which has a single method named `NextRetryDelay`.</span></span>

<span data-ttu-id="276a4-141">`NextRetryDelay`accepte un seul argument avec le type `RetryContext`.</span><span class="sxs-lookup"><span data-stu-id="276a4-141">`NextRetryDelay` takes a single argument with the type `RetryContext`.</span></span> <span data-ttu-id="276a4-142">`PreviousRetryCount` `long` Atrois`TimeSpan` propriétés: `ElapsedTime` , et`RetryReason`qui sont respectivementun,unetun.`Exception` `RetryContext`</span><span class="sxs-lookup"><span data-stu-id="276a4-142">The `RetryContext` has three properties: `PreviousRetryCount`, `ElapsedTime` and `RetryReason` which are a `long`, a `TimeSpan` and an `Exception` respectively.</span></span> <span data-ttu-id="276a4-143">Avant la première tentative de reconnexion, `PreviousRetryCount` et `ElapsedTime` sera égal à zéro, et `RetryReason` sera l’exception qui a provoqué la perte de la connexion.</span><span class="sxs-lookup"><span data-stu-id="276a4-143">Before the first reconnect attempt, both `PreviousRetryCount` and `ElapsedTime` will be zero, and the `RetryReason` will be the Exception that caused the connection to be lost.</span></span> <span data-ttu-id="276a4-144">Après chaque nouvelle tentative d’échec `PreviousRetryCount` , sera incrémenté d’une unité `ElapsedTime` , sera mis à jour pour refléter le temps passé à se reconnecter jusqu’à présent, `RetryReason` et sera l’exception qui a provoqué l’échec de la dernière tentative de reconnexion.</span><span class="sxs-lookup"><span data-stu-id="276a4-144">After each failed retry attempt, `PreviousRetryCount` will be incremented by one, `ElapsedTime` will be updated to reflect the amount of time spent reconnecting so far, and the `RetryReason` will be the Exception that caused the last reconnect attempt to fail.</span></span>

<span data-ttu-id="276a4-145">`NextRetryDelay`doit retourner une valeur TimeSpan représentant le délai d’attente avant la tentative de reconnexion suivante `null` ou si `HubConnection` doit arrêter la reconnexion.</span><span class="sxs-lookup"><span data-stu-id="276a4-145">`NextRetryDelay` must return either a TimeSpan representing the time to wait before the next reconnect attempt or `null` if the `HubConnection` should stop reconnecting.</span></span>

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

<span data-ttu-id="276a4-146">Vous pouvez également écrire du code qui reconnectera votre client manuellement, comme illustré dans [reconnexion manuelle](#manually-reconnect).</span><span class="sxs-lookup"><span data-stu-id="276a4-146">Alternatively, you can write code that will reconnect your client manually as demonstrated in [Manually reconnect](#manually-reconnect).</span></span>

::: moniker-end

### <a name="manually-reconnect"></a><span data-ttu-id="276a4-147">Reconnexion manuelle</span><span class="sxs-lookup"><span data-stu-id="276a4-147">Manually reconnect</span></span>

::: moniker range="< aspnetcore-3.0"

> [!WARNING]
> <span data-ttu-id="276a4-148">Avant le 3,0, le client .NET pour Signalr ne se reconnecte pas automatiquement.</span><span class="sxs-lookup"><span data-stu-id="276a4-148">Prior to 3.0, the .NET client for SignalR doesn't automatically reconnect.</span></span> <span data-ttu-id="276a4-149">Vous devez écrire du code qui reconnectera votre client manuellement.</span><span class="sxs-lookup"><span data-stu-id="276a4-149">You must write code that will reconnect your client manually.</span></span>

::: moniker-end

<span data-ttu-id="276a4-150">Utilisez l'événement <xref:Microsoft.AspNetCore.SignalR.Client.HubConnection.Closed> pour répondre à une perte de connexion.</span><span class="sxs-lookup"><span data-stu-id="276a4-150">Use the <xref:Microsoft.AspNetCore.SignalR.Client.HubConnection.Closed> event to respond to a lost connection.</span></span> <span data-ttu-id="276a4-151">Par exemple, vous souhaiterez peut-être automatiser la reconnexion.</span><span class="sxs-lookup"><span data-stu-id="276a4-151">For example, you might want to automate reconnection.</span></span>

<span data-ttu-id="276a4-152">L'événement `Closed` nécessite un délégué qui retourne une `Task`, ce qui permet l’exécution sans utiliser de code asynchrone `async void`.</span><span class="sxs-lookup"><span data-stu-id="276a4-152">The `Closed` event requires a delegate that returns a `Task`, which allows async code to run without using `async void`.</span></span> <span data-ttu-id="276a4-153">Pour répondre à la signature du délégué dans un gestionnaire d’événements `Closed` qui s’exécute de façon synchrone, retournez `Task.CompletedTask`:</span><span class="sxs-lookup"><span data-stu-id="276a4-153">To satisfy the delegate signature in a `Closed` event handler that runs synchronously, return `Task.CompletedTask`:</span></span>

```csharp
connection.Closed += (error) => {
    // Do your close logic.
    return Task.CompletedTask;
};
```

<span data-ttu-id="276a4-154">La principale raison de la prise en charge asynchrone est de pouvoir redémarrer la connexion.</span><span class="sxs-lookup"><span data-stu-id="276a4-154">The main reason for the async support is so you can restart the connection.</span></span> <span data-ttu-id="276a4-155">Le démarrage d’une connexion est une action asynchrone.</span><span class="sxs-lookup"><span data-stu-id="276a4-155">Starting a connection is an async action.</span></span>

<span data-ttu-id="276a4-156">Dans un gestionnaire `Closed` qui redémarre la connexion, envisagez d’attendre un délai aléatoire afin d'éviter de surcharger le serveur, comme indiqué dans l’exemple suivant :</span><span class="sxs-lookup"><span data-stu-id="276a4-156">In a `Closed` handler that restarts the connection, consider waiting for some random delay to prevent overloading the server, as shown in the following example:</span></span>

[!code-csharp[Use Closed event handler to automate reconnection](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_ClosedRestart)]

## <a name="call-hub-methods-from-client"></a><span data-ttu-id="276a4-157">Appeler des méthodes de hub à partir du client</span><span class="sxs-lookup"><span data-stu-id="276a4-157">Call hub methods from client</span></span>

<span data-ttu-id="276a4-158">`InvokeAsync` appelle des méthodes sur le hub.</span><span class="sxs-lookup"><span data-stu-id="276a4-158">`InvokeAsync` calls methods on the hub.</span></span> <span data-ttu-id="276a4-159">Passez le nom de la méthode de hub et de tous les arguments définis dans la méthode de hub à `InvokeAsync`.</span><span class="sxs-lookup"><span data-stu-id="276a4-159">Pass the hub method name and any arguments defined in the hub method to `InvokeAsync`.</span></span> <span data-ttu-id="276a4-160">SignalR est asynchrone, par conséquent, utilisez `async` et `await` lors de l’appel.</span><span class="sxs-lookup"><span data-stu-id="276a4-160">SignalR is asynchronous, so use `async` and `await` when making the calls.</span></span>

[!code-csharp[InvokeAsync method](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_InvokeAsync)]

<span data-ttu-id="276a4-161">La `InvokeAsync` méthode retourne un `Task` qui se termine lorsque la méthode serveur retourne.</span><span class="sxs-lookup"><span data-stu-id="276a4-161">The `InvokeAsync` method returns a `Task` which completes when the server method returns.</span></span> <span data-ttu-id="276a4-162">La valeur de retour, le cas échéant, est fournie comme résultat de `Task`.</span><span class="sxs-lookup"><span data-stu-id="276a4-162">The return value, if any, is provided as the result of the `Task`.</span></span> <span data-ttu-id="276a4-163">Toute exception levée par la méthode sur le serveur génère une erreur `Task`.</span><span class="sxs-lookup"><span data-stu-id="276a4-163">Any exceptions thrown by the method on the server produce a faulted `Task`.</span></span> <span data-ttu-id="276a4-164">Utilisez `await` la syntaxe pour attendre la fin de la méthode serveur `try...catch` et la syntaxe pour gérer les erreurs.</span><span class="sxs-lookup"><span data-stu-id="276a4-164">Use `await` syntax to wait for the server method to complete and `try...catch` syntax to handle errors.</span></span>

<span data-ttu-id="276a4-165">La `SendAsync` méthode retourne un `Task` qui se termine lorsque le message a été envoyé au serveur.</span><span class="sxs-lookup"><span data-stu-id="276a4-165">The `SendAsync` method returns a `Task` which completes when the message has been sent to the server.</span></span> <span data-ttu-id="276a4-166">Aucune valeur de retour n’est fournie `Task` , car cela n’attend pas la fin de la méthode serveur.</span><span class="sxs-lookup"><span data-stu-id="276a4-166">No return value is provided since this `Task` doesn't wait until the server method completes.</span></span> <span data-ttu-id="276a4-167">Toutes les exceptions levées sur le client lors de l’envoi du message `Task`génèrent une erreur.</span><span class="sxs-lookup"><span data-stu-id="276a4-167">Any exceptions thrown on the client while sending the message produce a faulted `Task`.</span></span> <span data-ttu-id="276a4-168">Utilisez `await` la `try...catch` syntaxe et pour gérer les erreurs d’envoi.</span><span class="sxs-lookup"><span data-stu-id="276a4-168">Use `await` and `try...catch` syntax to handle send errors.</span></span>

> [!NOTE]
> <span data-ttu-id="276a4-169">Si vous utilisez le service Signalr Azure en *mode sans serveur*, vous ne pouvez pas appeler les méthodes de concentrateur à partir d’un client.</span><span class="sxs-lookup"><span data-stu-id="276a4-169">If you're using Azure SignalR Service in *Serverless mode*, you cannot call hub methods from a client.</span></span> <span data-ttu-id="276a4-170">Pour plus d’informations, consultez la [documentation du service signalr](/azure/azure-signalr/signalr-concept-serverless-development-config).</span><span class="sxs-lookup"><span data-stu-id="276a4-170">For more information, see the [SignalR Service documentation](/azure/azure-signalr/signalr-concept-serverless-development-config).</span></span>

## <a name="call-client-methods-from-hub"></a><span data-ttu-id="276a4-171">Appeler des méthodes clientes à partir du Hub</span><span class="sxs-lookup"><span data-stu-id="276a4-171">Call client methods from hub</span></span>

<span data-ttu-id="276a4-172">Définissez les méthodes appelées par le hub en utilisant `connection.On` après la génération, mais avant de démarrer la connexion.</span><span class="sxs-lookup"><span data-stu-id="276a4-172">Define methods the hub calls using `connection.On` after building, but before starting the connection.</span></span>

[!code-csharp[Define client methods](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_ConnectionOn)]

<span data-ttu-id="276a4-173">Le code précédent dans `connection.On` s’exécute lorsque le code côté serveur l’appelle en utilisant la méthode `SendAsync`.</span><span class="sxs-lookup"><span data-stu-id="276a4-173">The preceding code in `connection.On` runs when server-side code calls it using the `SendAsync` method.</span></span>

[!code-csharp[Call client method](dotnet-client/sample/signalrchat/hubs/chathub.cs?name=snippet_SendMessage)]

## <a name="error-handling-and-logging"></a><span data-ttu-id="276a4-174">Gestion et journalisation des erreurs</span><span class="sxs-lookup"><span data-stu-id="276a4-174">Error handling and logging</span></span>

<span data-ttu-id="276a4-175">Gérez les erreurs avec une instruction try-catch.</span><span class="sxs-lookup"><span data-stu-id="276a4-175">Handle errors with a try-catch statement.</span></span> <span data-ttu-id="276a4-176">Inspectez l'objet `Exception` afin de déterminer l’action appropriée à entreprendre après qu'une erreur se produit.</span><span class="sxs-lookup"><span data-stu-id="276a4-176">Inspect the `Exception` object to determine the proper action to take after an error occurs.</span></span>

[!code-csharp[Logging](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_ErrorHandling)]

## <a name="additional-resources"></a><span data-ttu-id="276a4-177">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="276a4-177">Additional resources</span></span>

* [<span data-ttu-id="276a4-178">Hubs</span><span class="sxs-lookup"><span data-stu-id="276a4-178">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="276a4-179">Client JavaScript</span><span class="sxs-lookup"><span data-stu-id="276a4-179">JavaScript client</span></span>](xref:signalr/javascript-client)
* [<span data-ttu-id="276a4-180">Publier sur Azure</span><span class="sxs-lookup"><span data-stu-id="276a4-180">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)
* [<span data-ttu-id="276a4-181">Documentation sans serveur du service Azure Signalr</span><span class="sxs-lookup"><span data-stu-id="276a4-181">Azure SignalR Service serverless documentation</span></span>](/azure/azure-signalr/signalr-concept-serverless-development-config)
