---
title: 'Client .NET SignalR ASP.NET Core '
author: tdykstra
description: Informations sur le Client .NET SignalR ASP.NET Core
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 09/10/2018
uid: signalr/dotnet-client
ms.openlocfilehash: 205ca8ca228dcc2cc77f7e9b6431943851a3b152
ms.sourcegitcommit: 1a2fc47fb5d3da0f2a3c3269613ab20eb3b0da2c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/11/2018
ms.locfileid: "44373317"
---
# <a name="aspnet-core-signalr-net-client"></a>Client .NET SignalR ASP.NET Core 

Par [Rachel Appel](http://twitter.com/rachelappel)

La bibliothèque cliente ASP.NET Core SignalR .NET vous permet de communiquer avec les concentrateurs SignalR à partir des applications .NET.

> [!NOTE]
> Xamarin a des exigences spéciales pour la version de Visual Studio. Pour plus d’informations, consultez [SignalR Client 2.1.1 dans Xamarin](https://github.com/aspnet/Announcements/issues/305).

[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/dotnet-client/sample) ([procédure de téléchargement](xref:tutorials/index#how-to-download-a-sample))

L’exemple de code dans cet article est une application WPF qui utilise le client .NET SignalR ASP.NET Core.

## <a name="install-the-signalr-net-client-package"></a>Installer le package du client SignalR .NET

Le package `Microsoft.AspNetCore.SignalR.Client` est requis pour que les clients .NET se connectent à des hubs SignalR. Pour installer la bibliothèque cliente, exécutez la commande suivante dans la fenêtre **Console du Gestionnaire de package** :

```powershell
Install-Package Microsoft.AspNetCore.SignalR.Client
```

## <a name="connect-to-a-hub"></a>Se connecter à un concentrateur

Pour établir une connexion, créez un `HubConnectionBuilder` et appelez `Build`. L’URL du hub, le protocole, le type de transport, le niveau du journalisation, les en-têtes et les autres options peuvent être configurées lors de la création d’une connexion. Configurez les options requises en insérant les méthodes `HubConnectionBuilder` dans `Build`. Démarrez la connexion avec `StartAsync`.

[!code-csharp[Build hub connection](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_MainWindowClass&highlight=14-16,32)]

## <a name="call-hub-methods-from-client"></a>Appeler des méthodes de hub à partir du client

`InvokeAsync` appelle des méthodes sur le hub. Passez le nom de la méthode de hub et de tous les arguments définis dans la méthode de hub à `InvokeAsync`. SignalR est asynchrone, par conséquent, utilisez `async` et `await` lors de l’appe.

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
