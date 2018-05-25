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
# <a name="aspnet-core-signalr-net-client"></a><span data-ttu-id="e42af-103">ASP.NET Core SignalR .NET Client</span><span class="sxs-lookup"><span data-stu-id="e42af-103">ASP.NET Core SignalR .NET Client</span></span>

<span data-ttu-id="e42af-104">Par [Rachel Appel](http://twitter.com/rachelappel)</span><span class="sxs-lookup"><span data-stu-id="e42af-104">By [Rachel Appel](http://twitter.com/rachelappel)</span></span>

<span data-ttu-id="e42af-105">Le client d’ASP.NET Core SignalR .NET peut être utilisé par les applications Xamarin, WPF, Windows Forms, Console et .NET Core.</span><span class="sxs-lookup"><span data-stu-id="e42af-105">The ASP.NET Core SignalR .NET client can be used by Xamarin, WPF, Windows Forms, Console, and .NET Core apps.</span></span> <span data-ttu-id="e42af-106">Comme le [client JavaScript](xref:signalr/javascript-client), le client .NET permet de recevoir et envoyer et recevoir des messages à un concentrateur en temps réel.</span><span class="sxs-lookup"><span data-stu-id="e42af-106">Like the [JavaScript client](xref:signalr/javascript-client), the .NET client enables you to receive and send and receive messages to a hub in real time.</span></span>

<span data-ttu-id="e42af-107">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/Docs/tree/live/aspnetcore/signalr/dotnet-client/sample) ([procédure de téléchargement](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="e42af-107">[View or download sample code](https://github.com/aspnet/Docs/tree/live/aspnetcore/signalr/dotnet-client/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="e42af-108">L’exemple de code dans cet article est une application WPF qui utilise le client ASP.NET Core SignalR .NET.</span><span class="sxs-lookup"><span data-stu-id="e42af-108">The code sample in this article is a WPF app that uses the ASP.NET Core SignalR .NET client.</span></span>

## <a name="setup-client"></a><span data-ttu-id="e42af-109">Le programme d’installation de client</span><span class="sxs-lookup"><span data-stu-id="e42af-109">Setup client</span></span>

<span data-ttu-id="e42af-110">Le `Microsoft.AspNetCore.SignalR.Client` package est requis pour les clients .NET pour se connecter à des concentrateurs SignalR.</span><span class="sxs-lookup"><span data-stu-id="e42af-110">The `Microsoft.AspNetCore.SignalR.Client` package is needed for .NET clients to connect to SignalR hubs.</span></span> <span data-ttu-id="e42af-111">Pour installer la bibliothèque cliente, exécutez la commande suivante dans le **Package Manager Console** fenêtre :</span><span class="sxs-lookup"><span data-stu-id="e42af-111">To install the client library, run the following command in the **Package Manager Console** window:</span></span>

```powershell
Install-Package Microsoft.AspNetCore.SignalR.Client
```

## <a name="connect-to-a-hub"></a><span data-ttu-id="e42af-112">Se connecter à un concentrateur</span><span class="sxs-lookup"><span data-stu-id="e42af-112">Connect to a hub</span></span>

<span data-ttu-id="e42af-113">Pour établir une connexion, créer un `HubConnectionBuilder` et appelez `Build`.</span><span class="sxs-lookup"><span data-stu-id="e42af-113">To establish a connection, create a `HubConnectionBuilder` and call `Build`.</span></span> <span data-ttu-id="e42af-114">L’URL du concentrateur, protocole, type de transport, au niveau du journal, en-têtes et autres options peuvent être configurées lors de la création d’une connexion.</span><span class="sxs-lookup"><span data-stu-id="e42af-114">The hub URL, protocol, transport type, log level, headers, and other options can be configured while building a connection.</span></span> <span data-ttu-id="e42af-115">Configurer les options requises en les insérant le `HubConnectionBuilder` méthodes dans `Build`.</span><span class="sxs-lookup"><span data-stu-id="e42af-115">Configure any required options by inserting any of the `HubConnectionBuilder` methods into `Build`.</span></span> <span data-ttu-id="e42af-116">Démarrer la connexion avec `StartAsync`.</span><span class="sxs-lookup"><span data-stu-id="e42af-116">Start the connection with `StartAsync`.</span></span>

[!code-csharp[Build hub connection](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?highlight=15-17,33)]

## <a name="call-hub-methods-from-client"></a><span data-ttu-id="e42af-117">Appeler des méthodes de concentrateur du client</span><span class="sxs-lookup"><span data-stu-id="e42af-117">Call hub methods from client</span></span>

<span data-ttu-id="e42af-118">`InvokeAsync` appelle des méthodes sur le concentrateur.</span><span class="sxs-lookup"><span data-stu-id="e42af-118">`InvokeAsync` calls methods on the hub.</span></span> <span data-ttu-id="e42af-119">Passez le nom de la méthode de concentrateur et de tous les arguments définis dans la méthode de concentrateur pour `InvokeAsync`.</span><span class="sxs-lookup"><span data-stu-id="e42af-119">Pass the hub method name and any arguments defined in the hub method to `InvokeAsync`.</span></span> <span data-ttu-id="e42af-120">SignalR est asynchrone, par conséquent, utilisez `async` et `await` lors de l’appel.</span><span class="sxs-lookup"><span data-stu-id="e42af-120">SignalR is asynchronous, so use `async` and `await` when making the calls.</span></span>

[!code-csharp[InvokeAsync method](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?range=48-49)]

## <a name="call-client-methods-from-hub"></a><span data-ttu-id="e42af-121">Appeler des méthodes de client à partir du hub</span><span class="sxs-lookup"><span data-stu-id="e42af-121">Call client methods from hub</span></span>

<span data-ttu-id="e42af-122">Définir le concentrateur appelle à l’aide des méthodes `connection.On` après sa création, mais avant de démarrer la connexion.</span><span class="sxs-lookup"><span data-stu-id="e42af-122">Define methods the hub calls using `connection.On` after building, but before starting the connection.</span></span>

[!code-csharp[Define client methods](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?range=22-29)]

<span data-ttu-id="e42af-123">Le code précédent dans `connection.On` s’exécute lorsque le code côté serveur appelle à l’aide de la `SendAsync` (méthode).</span><span class="sxs-lookup"><span data-stu-id="e42af-123">The preceding code in `connection.On` runs when server-side code calls it using the `SendAsync` method.</span></span>

[!code-csharp[Call client method](dotnet-client/sample/signalrchat/hubs/chathub.cs?range=8-11)]

## <a name="error-handling-and-logging"></a><span data-ttu-id="e42af-124">Journalisation et gestion des erreurs</span><span class="sxs-lookup"><span data-stu-id="e42af-124">Error handling and logging</span></span>

<span data-ttu-id="e42af-125">Gérer les erreurs avec une instruction try-catch.</span><span class="sxs-lookup"><span data-stu-id="e42af-125">Handle errors with a try-catch statement.</span></span> <span data-ttu-id="e42af-126">Inspecter le `Exception` objet afin de déterminer l’action appropriée à entreprendre après une erreur se produit.</span><span class="sxs-lookup"><span data-stu-id="e42af-126">Inspect the `Exception` object to determine the proper action to take after an error occurs.</span></span>

[!code-csharp[Logging](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?range=46-54)]

## <a name="additional-resources"></a><span data-ttu-id="e42af-127">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="e42af-127">Additional resources</span></span>

* [<span data-ttu-id="e42af-128">Hubs</span><span class="sxs-lookup"><span data-stu-id="e42af-128">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="e42af-129">Client JavaScript</span><span class="sxs-lookup"><span data-stu-id="e42af-129">JavaScript client</span></span>](xref:signalr/javascript-client)
* [<span data-ttu-id="e42af-130">Publier sur Azure</span><span class="sxs-lookup"><span data-stu-id="e42af-130">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)