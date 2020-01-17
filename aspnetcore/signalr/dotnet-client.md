---
title: ASP.NET Core SignalR client .NET
author: bradygaster
description: Informations sur le client ASP.NET Core SignalR .NET
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 01/14/2020
no-loc:
- SignalR
uid: signalr/dotnet-client
ms.openlocfilehash: 39d9eccdb1e0457b177e75e6f94f3dd185b0093d
ms.sourcegitcommit: cbd30479f42cbb3385000ef834d9c7d021fd218d
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/16/2020
ms.locfileid: "76146314"
---
# <a name="aspnet-core-opno-locsignalr-net-client"></a>ASP.NET Core SignalR client .NET

La bibliothèque cliente ASP.NET Core SignalR .NET vous permet de communiquer avec SignalR hubs à partir d’applications .NET.

[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/signalr/dotnet-client/sample) ([procédure de téléchargement](xref:index#how-to-download-a-sample))

L’exemple de code dans cet article est une application WPF qui utilise le client ASP.NET Core SignalR .NET.

## <a name="install-the-opno-locsignalr-net-client-package"></a>Installer le package client .NET SignalR

[Microsoft.AspNetCore.SignalR.](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR.Client)Le package client est requis pour que les clients .net se connectent à SignalR hubs.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Pour installer la bibliothèque cliente, exécutez la commande suivante dans la fenêtre **Console du Gestionnaire de package** :

```powershell
Install-Package Microsoft.AspNetCore.SignalR.Client
```

# <a name="net-core-clitabnetcore-cli"></a>[CLI .NET Core](#tab/netcore-cli)

Pour installer la bibliothèque cliente, exécutez la commande suivante dans une interface de commande :

```dotnetcli
dotnet add package Microsoft.AspNetCore.SignalR.Client
```

---

## <a name="connect-to-a-hub"></a>Se connecter à un hub

Pour établir une connexion, créez un `HubConnectionBuilder` et appelez `Build`. L’URL du hub, le protocole, le type de transport, le niveau du journalisation, les en-têtes et les autres options peuvent être configurés lors de la création d’une connexion. Configurez les options requises en insérant les méthodes du `HubConnectionBuilder` dans `Build`. Démarrez la connexion avec `StartAsync`.

[!code-csharp[Build hub connection](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_MainWindowClass&highlight=15-17,39)]

## <a name="handle-lost-connection"></a>Gérer la connexion perdue

::: moniker range=">= aspnetcore-3.0"

### <a name="automatically-reconnect"></a>Se reconnecter automatiquement

La <xref:Microsoft.AspNetCore.SignalR.Client.HubConnection> peut être configurée pour se reconnecter automatiquement à l’aide de la méthode `WithAutomaticReconnect` sur le <xref:Microsoft.AspNetCore.SignalR.Client.HubConnectionBuilder>. Par défaut, il ne se reconnectera pas automatiquement.

```csharp
HubConnection connection= new HubConnectionBuilder()
    .WithUrl(new Uri("http://127.0.0.1:5000/chatHub"))
    .WithAutomaticReconnect()
    .Build();
```

Sans aucun paramètre, `WithAutomaticReconnect()` configure le client pour attendre 0, 2, 10 et 30 secondes, respectivement, avant d’essayer chaque tentative de reconnexion, en s’arrêtant après quatre tentatives ayant échoué.

Avant de commencer les tentatives de reconnexion, le `HubConnection` passe à l’État `HubConnectionState.Reconnecting` et déclenche l’événement `Reconnecting`.  Cela permet d’avertir les utilisateurs que la connexion a été perdue et de désactiver les éléments d’interface utilisateur. Les applications non interactives peuvent commencer à mettre en file d’attente ou à déposer des messages.

```csharp
connection.Reconnecting += error =>
{
    Debug.Assert(connection.State == HubConnectionState.Reconnecting);

    // Notify users the connection was lost and the client is reconnecting.
    // Start queuing or dropping messages.

    return Task.CompletedTask;
};
```

Si le client se reconnecte correctement dans les quatre premières tentatives, le `HubConnection` passe à l’État `Connected` et active l’événement `Reconnected`. Cela permet d’informer les utilisateurs que la connexion a été rétablie et de défiler tous les messages mis en file d’attente.

Étant donné que la connexion semble entièrement nouvelle sur le serveur, un nouveau `ConnectionId` sera fourni aux gestionnaires d’événements `Reconnected`.

> [!WARNING]
> Le paramètre de `connectionId` du gestionnaire d’événements `Reconnected` sera null si le `HubConnection` a été configuré pour [ignorer la négociation](xref:signalr/configuration#configure-client-options).

```csharp
connection.Reconnected += connectionId =>
{
    Debug.Assert(connection.State == HubConnectionState.Connected);

    // Notify users the connection was reestablished.
    // Start dequeuing messages queued while reconnecting if any.

    return Task.CompletedTask;
};
```

`WithAutomaticReconnect()` ne configure pas le `HubConnection` pour réessayer les échecs de démarrage initial, les échecs de démarrage doivent donc être gérés manuellement :

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

Si le client ne se reconnecte pas correctement dans les quatre premières tentatives, le `HubConnection` passera à l’État `Disconnected` et déclenchera l’événement <xref:Microsoft.AspNetCore.SignalR.Client.HubConnection.Closed>. Cela offre la possibilité de tenter de redémarrer manuellement la connexion ou d’informer les utilisateurs que la connexion a été définitivement perdue.

```csharp
connection.Closed += error =>
{
    Debug.Assert(connection.State == HubConnectionState.Disconnected);

    // Notify users the connection has been closed or manually try to restart the connection.

    return Task.CompletedTask;
};
```

Pour configurer un nombre personnalisé de tentatives de reconnexion avant de vous déconnecter ou de modifier le délai de reconnexion, `WithAutomaticReconnect` accepte un tableau de nombres représentant le délai en millisecondes à attendre avant de démarrer chaque tentative de reconnexion.

```csharp
HubConnection connection= new HubConnectionBuilder()
    .WithUrl(new Uri("http://127.0.0.1:5000/chatHub"))
    .WithAutomaticReconnect(new[] { TimeSpan.Zero, TimeSpan.Zero, TimeSpan.FromSeconds(10) })
    .Build();

    // .WithAutomaticReconnect(new[] { TimeSpan.Zero, TimeSpan.FromSeconds(2), TimeSpan.FromSeconds(10), TimeSpan.FromSeconds(30) }) yields the default behavior.
```

L’exemple précédent configure le `HubConnection` pour commencer à tenter de se reconnecter immédiatement après la perte de la connexion. Cela est également vrai pour la configuration par défaut.

Si la première tentative de reconnexion échoue, la deuxième tentative de reconnexion démarre également immédiatement au lieu d’attendre 2 secondes comme dans la configuration par défaut.

Si la deuxième tentative de reconnexion échoue, la troisième tentative de reconnexion démarrera dans 10 secondes, ce qui revient à la configuration par défaut.

Le comportement personnalisé s’est ensuite dévergé du comportement par défaut en s’arrêtant après le troisième échec de tentative de reconnexion. Dans la configuration par défaut, il y aura une autre tentative de reconnexion dans 30 secondes.

Si vous souhaitez encore plus de contrôle sur le minutage et le nombre de tentatives de reconnexion automatique, `WithAutomaticReconnect` accepte un objet qui implémente l’interface `IRetryPolicy`, qui a une méthode unique nommée `NextRetryDelay`.

`NextRetryDelay` accepte un seul argument avec le type `RetryContext`. La `RetryContext` a trois propriétés : `PreviousRetryCount`, `ElapsedTime` et `RetryReason`, qui sont une `long`, une `TimeSpan` et une `Exception` respectivement. Avant la première tentative de reconnexion, les `PreviousRetryCount` et `ElapsedTime` seront nuls, et le `RetryReason` sera l’exception qui a provoqué la perte de la connexion. Après chaque nouvelle tentative d’échec, `PreviousRetryCount` sera incrémenté d’une unité, `ElapsedTime` sera mis à jour pour refléter le temps passé à se reconnecter jusqu’à présent, et la `RetryReason` sera l’exception qui a provoqué l’échec de la dernière tentative de reconnexion.

`NextRetryDelay` doit retourner une valeur TimeSpan représentant le délai d’attente avant la tentative de reconnexion suivante ou `null` si le `HubConnection` doit s’arrêter de se reconnecter.

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
            return TimeSpan.FromSeconds(_random.NextDouble() * 10);
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

Vous pouvez également écrire du code qui reconnectera votre client manuellement, comme illustré dans [reconnexion manuelle](#manually-reconnect).

::: moniker-end

### <a name="manually-reconnect"></a>Reconnexion manuelle

::: moniker range="< aspnetcore-3.0"

> [!WARNING]
> Avant le 3,0, le client .NET pour SignalR ne se reconnecte pas automatiquement. Vous devez écrire du code qui reconnectera votre client manuellement.

::: moniker-end

Utilisez l'événement <xref:Microsoft.AspNetCore.SignalR.Client.HubConnection.Closed> pour répondre à une perte de connexion. Par exemple, vous souhaiterez peut-être automatiser la reconnexion.

L'événement `Closed` nécessite un délégué qui retourne une `Task`, ce qui permet l’exécution sans utiliser de code asynchrone `async void`. Pour répondre à la signature du délégué dans un gestionnaire d’événements `Closed` qui s’exécute de façon synchrone, retournez `Task.CompletedTask`:

```csharp
connection.Closed += (error) => {
    // Do your close logic.
    return Task.CompletedTask;
};
```

La principale raison de la prise en charge asynchrone est de pouvoir redémarrer la connexion. Le démarrage d’une connexion est une action asynchrone.

Dans un gestionnaire `Closed` qui redémarre la connexion, envisagez d’attendre un délai aléatoire afin d'éviter de surcharger le serveur, comme indiqué dans l’exemple suivant :

[!code-csharp[Use Closed event handler to automate reconnection](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_ClosedRestart)]

## <a name="call-hub-methods-from-client"></a>Appeler des méthodes de hub à partir du client

`InvokeAsync` appelle des méthodes sur le hub. Passez le nom de la méthode de hub et de tous les arguments définis dans la méthode de hub à `InvokeAsync`. SignalR est asynchrone, utilisez `async` et `await` lors des appels.

[!code-csharp[InvokeAsync method](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_InvokeAsync)]

La méthode `InvokeAsync` retourne une `Task` qui se termine lorsque la méthode serveur est retournée. La valeur de retour, le cas échéant, est fournie comme résultat de l' `Task`. Toutes les exceptions levées par la méthode sur le serveur génèrent une erreur de `Task`. Utilisez la syntaxe `await` pour attendre la fin de la méthode serveur et `try...catch` syntaxe pour gérer les erreurs.

La méthode `SendAsync` retourne une `Task` qui se termine lorsque le message a été envoyé au serveur. Aucune valeur de retour n’est fournie, car cette `Task` n’attend pas la fin de la méthode du serveur. Toutes les exceptions levées sur le client lors de l’envoi du message génèrent une erreur de `Task`. Utilisez la syntaxe `await` et `try...catch` pour gérer les erreurs d’envoi.

> [!NOTE]
> Si vous utilisez le service Azure SignalR en *mode sans serveur*, vous ne pouvez pas appeler les méthodes de concentrateur à partir d’un client. Pour plus d’informations, consultez la [documentation du ServiceSignalR](/azure/azure-signalr/signalr-concept-serverless-development-config).

## <a name="call-client-methods-from-hub"></a>Appeler des méthodes clientes à partir du Hub

Définissez les méthodes appelées par le hub en utilisant `connection.On` après la génération, mais avant de démarrer la connexion.

[!code-csharp[Define client methods](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_ConnectionOn)]

Le code précédent dans `connection.On` s’exécute lorsque le code côté serveur l’appelle en utilisant la méthode `SendAsync`.

[!code-csharp[Call client method](dotnet-client/sample/signalrchat/hubs/chathub.cs?name=snippet_SendMessage)]

## <a name="error-handling-and-logging"></a>Gestion et journalisation des erreurs

Gérez les erreurs avec une instruction try-catch. Inspectez l'objet `Exception` afin de déterminer l’action appropriée à entreprendre après qu'une erreur se produit.

[!code-csharp[Logging](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_ErrorHandling)]

## <a name="additional-resources"></a>Ressources supplémentaires

* [Hubs](xref:signalr/hubs)
* [Client JavaScript](xref:signalr/javascript-client)
* [Publier sur Azure](xref:signalr/publish-to-azure-web-app)
* [Documentation sans serveur d’Azure SignalR service](/azure/azure-signalr/signalr-concept-serverless-development-config)
