---
title: ASP.NET Core SignalR .NET Client
author: rachelappel
description: Informations sur le Client de .NET SignalR ASP.NET Core
manager: wpickett
monikerRange: '>= aspnetcore-2.1'
ms.author: rachelap
ms.custom: mvc
ms.date: 05/18/2018
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: signalr/dotnet-client
ms.openlocfilehash: 412d2362575789f1fb4792940df6d3dd24dbdd5a
ms.sourcegitcommit: 300a1127957dcdbce1b6ad79a7b9dc676f571510
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/23/2018
---
# <a name="aspnet-core-signalr-net-client"></a>ASP.NET Core SignalR .NET Client

Par [Rachel Appel](http://twitter.com/rachelappel)

Le client d’ASP.NET Core SignalR .NET peut être utilisé par les applications Xamarin, WPF, Windows Forms, Console et .NET Core. Comme le [client JavaScript](xref:signalr/javascript-client), le client .NET permet de recevoir et envoyer et recevoir des messages à un concentrateur en temps réel.

[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/Docs/tree/live/aspnetcore/signalr/dotnet-client/sample) ([procédure de téléchargement](xref:tutorials/index#how-to-download-a-sample))

L’exemple de code dans cet article est une application WPF qui utilise le client ASP.NET Core SignalR .NET.

## <a name="setup-client"></a>Le programme d’installation de client

Le `Microsoft.AspNetCore.SignalR.Client` package est requis pour les clients .NET pour se connecter à des concentrateurs SignalR. Pour installer la bibliothèque cliente, exécutez la commande suivante dans le **Package Manager Console** fenêtre :

```powershell
Install-Package Microsoft.AspNetCore.SignalR.Client
```

## <a name="connect-to-a-hub"></a>Se connecter à un concentrateur

Pour établir une connexion, créer un `HubConnectionBuilder` et appelez `Build`. L’URL du concentrateur, protocole, type de transport, au niveau du journal, en-têtes et autres options peuvent être configurées lors de la création d’une connexion. Configurer les options requises en les insérant le `HubConnectionBuilder` méthodes dans `Build`. Démarrer la connexion avec `StartAsync`.

[!code-csharp[Build hub connection](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?highlight=15-17,33)]

## <a name="call-hub-methods-from-client"></a>Appeler des méthodes de concentrateur du client

`InvokeAsync` appelle des méthodes sur le concentrateur. Passez le nom de la méthode de concentrateur et de tous les arguments définis dans la méthode de concentrateur pour `InvokeAsync`. SignalR est asynchrone, par conséquent, utilisez `async` et `await` lors de l’appel.

[!code-csharp[InvokeAsync method](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?range=48-49)]

## <a name="call-client-methods-from-hub"></a>Appeler des méthodes de client à partir du hub

Définir le concentrateur appelle à l’aide des méthodes `connection.On` après sa création, mais avant de démarrer la connexion.

[!code-csharp[Define client methods](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?range=22-29)]

Le code précédent dans `connection.On` s’exécute lorsque le code côté serveur appelle à l’aide de la `SendAsync` (méthode).

[!code-csharp[Call client method](dotnet-client/sample/signalrchat/hubs/chathub.cs?range=8-11)]

## <a name="error-handling-and-logging"></a>Journalisation et gestion des erreurs

Gérer les erreurs avec une instruction try-catch. Inspecter le `Exception` objet afin de déterminer l’action appropriée à entreprendre après une erreur se produit.

[!code-csharp[Logging](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?range=46-54)]

## <a name="additional-resources"></a>Ressources supplémentaires

* [Hubs](xref:signalr/hubs)
* [Client JavaScript](xref:signalr/javascript-client)
* [Publier sur Azure](xref:signalr/publish-to-azure-web-app)