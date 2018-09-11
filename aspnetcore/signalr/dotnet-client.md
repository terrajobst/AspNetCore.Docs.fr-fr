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
# <a name="aspnet-core-signalr-net-client"></a><span data-ttu-id="b07a0-103">Client .NET SignalR ASP.NET Core </span><span class="sxs-lookup"><span data-stu-id="b07a0-103">ASP.NET Core SignalR .NET Client</span></span>

<span data-ttu-id="b07a0-104">Par [Rachel Appel](http://twitter.com/rachelappel)</span><span class="sxs-lookup"><span data-stu-id="b07a0-104">By [Rachel Appel](http://twitter.com/rachelappel)</span></span>

<span data-ttu-id="b07a0-105">La bibliothèque cliente ASP.NET Core SignalR .NET vous permet de communiquer avec les concentrateurs SignalR à partir des applications .NET.</span><span class="sxs-lookup"><span data-stu-id="b07a0-105">The ASP.NET Core SignalR .NET client library lets you communicate with SignalR hubs from .NET apps.</span></span>

> [!NOTE]
> <span data-ttu-id="b07a0-106">Xamarin a des exigences spéciales pour la version de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="b07a0-106">Xamarin has special requirements for Visual Studio version.</span></span> <span data-ttu-id="b07a0-107">Pour plus d’informations, consultez [SignalR Client 2.1.1 dans Xamarin](https://github.com/aspnet/Announcements/issues/305).</span><span class="sxs-lookup"><span data-stu-id="b07a0-107">For more information, see [SignalR Client 2.1.1 in Xamarin](https://github.com/aspnet/Announcements/issues/305).</span></span>

<span data-ttu-id="b07a0-108">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/dotnet-client/sample) ([procédure de téléchargement](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="b07a0-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/dotnet-client/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="b07a0-109">L’exemple de code dans cet article est une application WPF qui utilise le client .NET SignalR ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="b07a0-109">The code sample in this article is a WPF app that uses the ASP.NET Core SignalR .NET client.</span></span>

## <a name="install-the-signalr-net-client-package"></a><span data-ttu-id="b07a0-110">Installer le package du client SignalR .NET</span><span class="sxs-lookup"><span data-stu-id="b07a0-110">Install the SignalR .NET client package</span></span>

<span data-ttu-id="b07a0-111">Le package `Microsoft.AspNetCore.SignalR.Client` est requis pour que les clients .NET se connectent à des hubs SignalR.</span><span class="sxs-lookup"><span data-stu-id="b07a0-111">The `Microsoft.AspNetCore.SignalR.Client` package is needed for .NET clients to connect to SignalR hubs.</span></span> <span data-ttu-id="b07a0-112">Pour installer la bibliothèque cliente, exécutez la commande suivante dans la fenêtre **Console du Gestionnaire de package** :</span><span class="sxs-lookup"><span data-stu-id="b07a0-112">To install the client library, run the following command in the **Package Manager Console** window:</span></span>

```powershell
Install-Package Microsoft.AspNetCore.SignalR.Client
```

## <a name="connect-to-a-hub"></a><span data-ttu-id="b07a0-113">Se connecter à un concentrateur</span><span class="sxs-lookup"><span data-stu-id="b07a0-113">Connect to a hub</span></span>

<span data-ttu-id="b07a0-114">Pour établir une connexion, créez un `HubConnectionBuilder` et appelez `Build`.</span><span class="sxs-lookup"><span data-stu-id="b07a0-114">To establish a connection, create a `HubConnectionBuilder` and call `Build`.</span></span> <span data-ttu-id="b07a0-115">L’URL du hub, le protocole, le type de transport, le niveau du journalisation, les en-têtes et les autres options peuvent être configurées lors de la création d’une connexion.</span><span class="sxs-lookup"><span data-stu-id="b07a0-115">The hub URL, protocol, transport type, log level, headers, and other options can be configured while building a connection.</span></span> <span data-ttu-id="b07a0-116">Configurez les options requises en insérant les méthodes `HubConnectionBuilder` dans `Build`.</span><span class="sxs-lookup"><span data-stu-id="b07a0-116">Configure any required options by inserting any of the `HubConnectionBuilder` methods into `Build`.</span></span> <span data-ttu-id="b07a0-117">Démarrez la connexion avec `StartAsync`.</span><span class="sxs-lookup"><span data-stu-id="b07a0-117">Start the connection with `StartAsync`.</span></span>

[!code-csharp[Build hub connection](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_MainWindowClass&highlight=14-16,32)]

## <a name="call-hub-methods-from-client"></a><span data-ttu-id="b07a0-118">Appeler des méthodes de hub à partir du client</span><span class="sxs-lookup"><span data-stu-id="b07a0-118">Call hub methods from client</span></span>

<span data-ttu-id="b07a0-119">`InvokeAsync` appelle des méthodes sur le hub.</span><span class="sxs-lookup"><span data-stu-id="b07a0-119">`InvokeAsync` calls methods on the hub.</span></span> <span data-ttu-id="b07a0-120">Passez le nom de la méthode de hub et de tous les arguments définis dans la méthode de hub à `InvokeAsync`.</span><span class="sxs-lookup"><span data-stu-id="b07a0-120">Pass the hub method name and any arguments defined in the hub method to `InvokeAsync`.</span></span> <span data-ttu-id="b07a0-121">SignalR est asynchrone, par conséquent, utilisez `async` et `await` lors de l’appe.</span><span class="sxs-lookup"><span data-stu-id="b07a0-121">SignalR is asynchronous, so use `async` and `await` when making the calls.</span></span>

[!code-csharp[InvokeAsync method](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_InvokeAsync)]

## <a name="call-client-methods-from-hub"></a><span data-ttu-id="b07a0-122">Appeler des méthodes de client à partir de hub</span><span class="sxs-lookup"><span data-stu-id="b07a0-122">Call client methods from hub</span></span>

<span data-ttu-id="b07a0-123">Définissez les méthodes appelées par le hub en utilisant `connection.On` après la génération, mais avant de démarrer la connexion.</span><span class="sxs-lookup"><span data-stu-id="b07a0-123">Define methods the hub calls using `connection.On` after building, but before starting the connection.</span></span>

[!code-csharp[Define client methods](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_ConnectionOn)]

<span data-ttu-id="b07a0-124">Le code précédent dans `connection.On` s’exécute lorsque le code côté serveur l’appelle en utilisant la méthode `SendAsync`.</span><span class="sxs-lookup"><span data-stu-id="b07a0-124">The preceding code in `connection.On` runs when server-side code calls it using the `SendAsync` method.</span></span>

[!code-csharp[Call client method](dotnet-client/sample/signalrchat/hubs/chathub.cs?name=snippet_SendMessage)]

## <a name="error-handling-and-logging"></a><span data-ttu-id="b07a0-125">Journalisation et gestion des erreurs</span><span class="sxs-lookup"><span data-stu-id="b07a0-125">Error handling and logging</span></span>

<span data-ttu-id="b07a0-126">Gérez les erreurs avec une instruction try-catch.</span><span class="sxs-lookup"><span data-stu-id="b07a0-126">Handle errors with a try-catch statement.</span></span> <span data-ttu-id="b07a0-127">Inspectez l'objet `Exception` afin de déterminer l’action appropriée à entreprendre après qu'une erreur se produit.</span><span class="sxs-lookup"><span data-stu-id="b07a0-127">Inspect the `Exception` object to determine the proper action to take after an error occurs.</span></span>

[!code-csharp[Logging](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_ErrorHandling)]

## <a name="additional-resources"></a><span data-ttu-id="b07a0-128">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="b07a0-128">Additional resources</span></span>

* [<span data-ttu-id="b07a0-129">Hubs</span><span class="sxs-lookup"><span data-stu-id="b07a0-129">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="b07a0-130">Client JavaScript</span><span class="sxs-lookup"><span data-stu-id="b07a0-130">JavaScript client</span></span>](xref:signalr/javascript-client)
* [<span data-ttu-id="b07a0-131">Publier sur Azure</span><span class="sxs-lookup"><span data-stu-id="b07a0-131">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)
