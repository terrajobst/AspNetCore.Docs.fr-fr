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
# <a name="aspnet-core-signalr-net-client"></a>Client .NET SignalR ASP.NET Core

La bibliothèque cliente ASP.NET Core SignalR .NET vous permet de communiquer avec les hubs SignalR à partir des applications .NET.

[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/signalr/dotnet-client/sample) ([procédure de téléchargement](xref:index#how-to-download-a-sample))

L’exemple de code dans cet article est une application WPF qui utilise le client .NET SignalR ASP.NET Core.

## <a name="install-the-signalr-net-client-package"></a>Installer le package du client .NET SignalR

Le package `Microsoft.AspNetCore.SignalR.Client` est requis pour que les clients .NET se connectent à des hubs SignalR. Pour installer la bibliothèque cliente, exécutez la commande suivante dans la fenêtre **Console du Gestionnaire de package** :

```powershell
Install-Package Microsoft.AspNetCore.SignalR.Client
```

## <a name="connect-to-a-hub"></a>Se connecter à un hub

Pour établir une connexion, créez un `HubConnectionBuilder` et appelez-le `Build`. L’URL du hub, le protocole, le type de transport, le niveau du journalisation, les en-têtes et les autres options peuvent être configurés lors de la création d’une connexion. Configurez les options requises en insérant les méthodes du `HubConnectionBuilder` dans `Build`. Démarrez la connexion avec `StartAsync`.

[!code-csharp[Build hub connection](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_MainWindowClass&highlight=15-17,39)]

## <a name="handle-lost-connection"></a>Gérer la perte de connexion

::: moniker range=">= aspnetcore-3.0"

### <a name="automatically-reconnect"></a>Se reconnecter automatiquement

Le <xref:Microsoft.AspNetCore.SignalR.Client.HubConnection> peuvent être configurés pour se reconnecter automatiquement à l’aide de la `WithAutomaticReconnect` méthode sur le <xref:Microsoft.AspNetCore.SignalR.Client.HubConnectionBuilder>. Il ne se reconnecter automatiquement par défaut.

```csharp
HubConnection connection= new HubConnectionBuilder()
    .WithUrl(new Uri("http://127.0.0.1:5000/chatHub"))
    .WithAutomaticReconnect()
    .Build();
```

Sans paramètres, `WithAutomaticReconnect()` configure le client en attente 0, 2, 10 et 30 secondes respectivement avant d’essayer de chaque tentative de reconnexion s’arrête après avoir quatre tentatives ayant échoué.

Avant de commencer toute tentative de reconnexion, le `HubConnection` sera la transition vers le `HubConnectionState.Reconnecting` d’état et de déclencher le `Reconnecting` événement.  Cela fournit une opportunité pour avertir les utilisateurs que la connexion a été perdue et désactiver les éléments d’interface utilisateur. Les applications non interactives peuvent démarrer queuing ou la suppression des messages.

```csharp
connection.Reconnecting += error =>
{
    Debug.Assert(connection.State == HubConnectionState.Reconnecting);

    // Notify users the connection was lost and the client is reconnecting.
    // Start queuing or dropping messages.

    return Task.CompletedTask;
};
```

Si le client se reconnecte avec succès au sein de ses quatre premières tentatives, le `HubConnection` passera à le `Connected` d’état et de déclencher le `Reconnected` événement. Cela permet d’informer les utilisateurs de la connexion a été rétablie et tous les messages en file d’attente de la file d’attente.

Étant donné que la connexion est entièrement nouveau sur le serveur, un nouveau `ConnectionId` seront fournies à la `Reconnected` gestionnaires d’événements.

> [!WARNING]
> Le `Reconnected` du Gestionnaire d’événements `connectionId` paramètre sera null si le `HubConnection` a été configuré pour [ignorer négociation](xref:signalr/configuration#configure-client-options).

```csharp
connection.Reconnected += connectionId =>
{
    Debug.Assert(connection.State == HubConnectionState.Connected);

    // Notify users the connection was reestablished.
    // Start dequeuing messages queued while reconnecting if any.

    return Task.CompletedTask;
};
```

`WithAutomaticReconnect()` ne configurez pas le `HubConnection` pour réessayer d’échecs de démarrage initial, afin de l’échec de démarrage doivent être traitées manuellement :

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

Si le client ne se reconnecter avec succès au sein de ses quatre premières tentatives, le `HubConnection` sera la transition vers le `Disconnected` d’état et de déclencher le <xref:Microsoft.AspNetCore.SignalR.Client.HubConnection.Closed> événement. Cela fournit une opportunité pour tenter de redémarrer la connexion manuellement ou informer les utilisateurs de que la connexion a été définitivement perdue.

```csharp
connection.Closed += error =>
{
    Debug.Assert(connection.State == HubConnectionState.Disconnected);

    // Notify users the connection has been closed or manually try to restart the connection.

    return Task.CompletedTask;
};
```

Pour configurer le nombre de tentatives de reconnexion avant de déconnecter ou de modifier le minutage de la reconnexion, `WithAutomaticReconnect` accepte un tableau de nombres représentant le délai en millisecondes à attendre avant de commencer chaque tentative de reconnexion.

```csharp
HubConnection connection= new HubConnectionBuilder()
    .WithUrl(new Uri("http://127.0.0.1:5000/chatHub"))
    .WithAutomaticReconnect(new[] { TimeSpan.Zero, TimeSpan.Zero, TimeSpan.FromSeconds(10) })
    .Build();

    // .WithAutomaticReconnect(new[] { TimeSpan.Zero, TimeSpan.FromSeconds(2), TimeSpan.FromSeconds(10), TimeSpan.FromSeconds(30) }) yields the default behavior.
```

L’exemple précédent configure le `HubConnection` pour démarrer une tentative de reconnexions immédiatement après la connexion est perdue. Cela vaut également pour la configuration par défaut.

Si la première tentative de reconnexion échoue, la deuxième tentative de reconnexion également démarre immédiatement au lieu d’attendre 2 secondes comme il le ferait dans la configuration par défaut.

Si la deuxième tentative de reconnexion échoue, la troisième tentative de reconnexion démarrera dans 10 secondes, ce qui revient à nouveau à la configuration par défaut.

Le comportement personnalisé puis diffère à nouveau le comportement par défaut en arrêtant après que la reconnexion troisième tentative d’échec. Dans la configuration par défaut il être un plus reconnectait tentative dans les 30 secondes.

Si vous souhaitez mieux contrôler la fréquence et le nombre d’automatique nouvelle tentative, `WithAutomaticReconnect` accepte un objet qui implémente le `IRetryPolicy` interface, ce qui a une méthode unique nommée `NextRetryDelay`.

`NextRetryDelay` accepte un seul argument avec le type `RetryContext`. Le `RetryContext` possède trois propriétés : `PreviousRetryCount`, `ElapsedTime` et `RetryReason` qui sont un `long`, un `TimeSpan` et un `Exception` respectivement. Avant la première tentative de reconnexion, les deux `PreviousRetryCount` et `ElapsedTime` sera égal à zéro et le `RetryReason` sera l’Exception qui a provoqué la connexion est perdue. Après chaque tentative ayant échoué, `PreviousRetryCount` sera incrémenté d’un, `ElapsedTime` sera mis à jour pour refléter la quantité de temps passé à la reconnexion jusqu’ici et le `RetryReason` sera l’Exception qui a provoqué la dernière tentative de reconnexion d’échouer.

`NextRetryDelay` doit retourner soit un laps de temps représentant le temps à attendre avant la prochaine tentative de reconnexion ou `null` si le `HubConnection` doit s’arrêter la reconnexion.

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

Vous pouvez également écrire le code qui se reconnectera votre client manuellement, comme illustré dans [reconnecter manuellement](#manually-reconnect).

::: moniker-end

### <a name="manually-reconnect"></a>Reconnecter manuellement

::: moniker range="< aspnetcore-3.0"

> [!WARNING]
> Avant la version 3.0, le client .NET pour SignalR ne se reconnecter automatiquement. Vous devez écrire du code qui se reconnectera votre client manuellement.

::: moniker-end

Utilisez l'événement <xref:Microsoft.AspNetCore.SignalR.Client.HubConnection.Closed> pour répondre à une perte de connexion. Par exemple, vous pouvez souhaiter automatiser une reconnexion.

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

`InvokeAsync` appelle des méthodes sur le hub. Passez le nom de la méthode de hub et de tous les arguments définis dans la méthode de hub à `InvokeAsync`. SignalR est asynchrone, par conséquent, utilisez `async` et `await` lors de l’appel.

[!code-csharp[InvokeAsync method](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_InvokeAsync)]

Le `InvokeAsync` méthode retourne un `Task` qui se termine lorsque la méthode de serveur est retournée. La valeur de retour, le cas échéant, est fournie en tant que le résultat de la `Task`. Toutes les exceptions levées par la méthode sur le serveur génère une erreur `Task`. Utilisez `await` syntaxe d’attente de la méthode de serveur pour terminer et `try...catch` syntaxe permettant de gérer les erreurs.

Le `SendAsync` méthode retourne un `Task` qui se termine lorsque le message a été envoyé au serveur. Aucune valeur de retour n’est fourni, car ce `Task` n’attendez la fin de la méthode de serveur. Toutes les exceptions levées sur le client lors de l’envoi du message génère une erreur `Task`. Utilisez `await` et `try...catch` syntaxe pour gérer les erreurs d’envoi.

> [!NOTE]
> Si vous utilisez le Service Azure SignalR dans *mode sans serveur*, vous ne pouvez pas appeler des méthodes de concentrateur à partir d’un client. Pour plus d’informations, consultez le [documentation de SignalR Service](/azure/azure-signalr/signalr-concept-serverless-development-config).

## <a name="call-client-methods-from-hub"></a>Appeler des méthodes de client à partir de hub

Définissez les méthodes appelées par le hub en utilisant `connection.On` après la génération, mais avant de démarrer la connexion.

[!code-csharp[Define client methods](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_ConnectionOn)]

Le code précédent dans `connection.On` s’exécute lorsque le code côté serveur l’appelle en utilisant la méthode `SendAsync`.

[!code-csharp[Call client method](dotnet-client/sample/signalrchat/hubs/chathub.cs?name=snippet_SendMessage)]

## <a name="error-handling-and-logging"></a>Journalisation et gestion des erreurs

Gérez les erreurs avec une instruction try-catch. Inspectez l'objet `Exception` afin de déterminer l’action appropriée à entreprendre après qu'une erreur se produit.

[!code-csharp[Logging](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_ErrorHandling)]

## <a name="additional-resources"></a>Ressources supplémentaires

* [Hubs](xref:signalr/hubs)
* [Client JavaScript](xref:signalr/javascript-client)
* [Publier sur Azure](xref:signalr/publish-to-azure-web-app)
* [Documentation de serverless Azure SignalR Service](/azure/azure-signalr/signalr-concept-serverless-development-config)
