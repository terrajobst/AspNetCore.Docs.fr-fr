---
title: 'Client .NET SignalR ASP.NET Core '
author: tdykstra
description: Informations sur le Client .NET SignalR ASP.NET Core
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 09/10/2018
uid: signalr/dotnet-client
ms.openlocfilehash: ef84ede2ed45ddc3b64d4ce8f5bd0018a681faf6
ms.sourcegitcommit: 4db337bd47d70c06fff91000c58bc048a491ccec
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/12/2018
ms.locfileid: "44749319"
---
# <a name="aspnet-core-signalr-net-client"></a>Client .NET SignalR ASP.NET Core 

La bibliothèque cliente ASP.NET Core SignalR .NET vous permet de communiquer avec les hubs SignalR à partir des applications .NET.

> [!NOTE]
> Xamarin a des prérequis spéciaux pour la version de Visual Studio. Pour plus d’informations, consultez [SignalR Client 2.1.1 dans Xamarin](https://github.com/aspnet/Announcements/issues/305).

[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/dotnet-client/sample) ([procédure de téléchargement](xref:tutorials/index#how-to-download-a-sample))

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

Utilisez l'événement <xref:Microsoft.AspNetCore.SignalR.Client.HubConnection.Closed> pour répondre à une perte de connexion. Par exemple, vous pouvez souhaiter automatiser une reconnexion.

L'événement `Closed` nécessite un délégué qui retourne une `Task`, ce qui permet l’exécution sans utiliser de code asynchrone `async void`. Pour répondre à la signature du délégué dans un gestionnaire d’événements `Closed` qui s’exécute de façon synchrone, retournez `Task.CompletedTask`:

```csharp
connection.Closed += (error) => {
    // Do your close logic.
    return Task.CompletedTask;
};
```

La principale raison de la prise en charge asynchrone est de pouvoir redémarrer la connexion. Le démarrage d’une connexion est une action asynchrone.

Dans un `Closed` gestionnaire qui redémarre la connexion, envisagez d’attendre un délai aléatoire éviter de surcharger le serveur, comme indiqué dans l’exemple suivant :

[!code-csharp[Use Closed event handler to automate reconnection](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_ClosedRestart)]

## <a name="call-hub-methods-from-client"></a>Appeler des méthodes de hub à partir du client

`InvokeAsync` appelle des méthodes sur le hub. Passez le nom de la méthode de hub et de tous les arguments définis dans la méthode de hub à `InvokeAsync`. SignalR est asynchrone, par conséquent, utilisez `async` et `await` lors de l’appel.

[!code-csharp[InvokeAsync method](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_InvokeAsync)]

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
