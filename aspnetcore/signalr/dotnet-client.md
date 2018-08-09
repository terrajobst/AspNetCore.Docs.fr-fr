---
title: 'Client .NET SignalR ASP.NET Core '
author: tdykstra
description: Informations sur le Client .NET SignalR ASP.NET Core
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 08/07/2018
uid: signalr/dotnet-client
ms.openlocfilehash: 970888a410b2486a20f98ce77a8674f8ec357f50
ms.sourcegitcommit: 028ad28c546de706ace98066c76774de33e4ad20
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/08/2018
ms.locfileid: "39655250"
---
# <a name="aspnet-core-signalr-net-client"></a><span data-ttu-id="6788c-103">Client .NET SignalR ASP.NET Core </span><span class="sxs-lookup"><span data-stu-id="6788c-103">ASP.NET Core SignalR .NET Client</span></span>

<span data-ttu-id="6788c-104">Par [Rachel Appel](http://twitter.com/rachelappel)</span><span class="sxs-lookup"><span data-stu-id="6788c-104">By [Rachel Appel](http://twitter.com/rachelappel)</span></span>

<span data-ttu-id="6788c-105">Le client .NET SignalR d’ASP.NET Core peut être utilisé par les applications Xamarin, WPF, Windows Forms, Console et .NET Core.</span><span class="sxs-lookup"><span data-stu-id="6788c-105">The ASP.NET Core SignalR .NET client can be used by Xamarin, WPF, Windows Forms, Console, and .NET Core apps.</span></span> <span data-ttu-id="6788c-106">Comme le [client JavaScript](xref:signalr/javascript-client), le client .NET permet de recevoir et d’envoyer des messages à un hub en temps réel.</span><span class="sxs-lookup"><span data-stu-id="6788c-106">Like the [JavaScript client](xref:signalr/javascript-client), the .NET client enables you to receive and send and receive messages to a hub in real time.</span></span>

> [!NOTE]
> <span data-ttu-id="6788c-107">Xamarin a des exigences spéciales pour la version de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="6788c-107">Xamarin has special requirements for Visual Studio version.</span></span> <span data-ttu-id="6788c-108">Pour plus d’informations, consultez [SignalR Client 2.1.1 dans Xamarin](https://github.com/aspnet/Announcements/issues/305).</span><span class="sxs-lookup"><span data-stu-id="6788c-108">For more information, see [SignalR Client 2.1.1 in Xamarin](https://github.com/aspnet/Announcements/issues/305).</span></span>

<span data-ttu-id="6788c-109">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/Docs/tree/live/aspnetcore/signalr/dotnet-client/sample) ([procédure de téléchargement](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="6788c-109">[View or download sample code](https://github.com/aspnet/Docs/tree/live/aspnetcore/signalr/dotnet-client/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="6788c-110">L’exemple de code dans cet article est une application WPF qui utilise le client .NET SignalR ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="6788c-110">The code sample in this article is a WPF app that uses the ASP.NET Core SignalR .NET client.</span></span>

## <a name="install-the-signalr-net-client-package"></a><span data-ttu-id="6788c-111">Installer le package du client SignalR .NET</span><span class="sxs-lookup"><span data-stu-id="6788c-111">Install the SignalR .NET client package</span></span>

<span data-ttu-id="6788c-112">Le package `Microsoft.AspNetCore.SignalR.Client` est requis pour que les clients .NET se connectent à des hubs SignalR.</span><span class="sxs-lookup"><span data-stu-id="6788c-112">The `Microsoft.AspNetCore.SignalR.Client` package is needed for .NET clients to connect to SignalR hubs.</span></span> <span data-ttu-id="6788c-113">Pour installer la bibliothèque cliente, exécutez la commande suivante dans la fenêtre **Console du Gestionnaire de package** :</span><span class="sxs-lookup"><span data-stu-id="6788c-113">To install the client library, run the following command in the **Package Manager Console** window:</span></span>

```powershell
Install-Package Microsoft.AspNetCore.SignalR.Client
```

## <a name="connect-to-a-hub"></a><span data-ttu-id="6788c-114">Se connecter à un concentrateur</span><span class="sxs-lookup"><span data-stu-id="6788c-114">Connect to a hub</span></span>

<span data-ttu-id="6788c-115">Pour établir une connexion, créez un `HubConnectionBuilder` et appelez `Build`.</span><span class="sxs-lookup"><span data-stu-id="6788c-115">To establish a connection, create a `HubConnectionBuilder` and call `Build`.</span></span> <span data-ttu-id="6788c-116">L’URL du hub, le protocole, le type de transport, le niveau du journalisation, les en-têtes et les autres options peuvent être configurées lors de la création d’une connexion.</span><span class="sxs-lookup"><span data-stu-id="6788c-116">The hub URL, protocol, transport type, log level, headers, and other options can be configured while building a connection.</span></span> <span data-ttu-id="6788c-117">Configurez les options requises en insérant les méthodes `HubConnectionBuilder` dans `Build`.</span><span class="sxs-lookup"><span data-stu-id="6788c-117">Configure any required options by inserting any of the `HubConnectionBuilder` methods into `Build`.</span></span> <span data-ttu-id="6788c-118">Démarrez la connexion avec `StartAsync`.</span><span class="sxs-lookup"><span data-stu-id="6788c-118">Start the connection with `StartAsync`.</span></span>

[!code-csharp[Build hub connection](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?highlight=15-17,33)]

## <a name="call-hub-methods-from-client"></a><span data-ttu-id="6788c-119">Appeler des méthodes de hub à partir du client</span><span class="sxs-lookup"><span data-stu-id="6788c-119">Call hub methods from client</span></span>

<span data-ttu-id="6788c-120">`InvokeAsync` appelle des méthodes sur le hub.</span><span class="sxs-lookup"><span data-stu-id="6788c-120">`InvokeAsync` calls methods on the hub.</span></span> <span data-ttu-id="6788c-121">Passez le nom de la méthode de hub et de tous les arguments définis dans la méthode de hub à `InvokeAsync`.</span><span class="sxs-lookup"><span data-stu-id="6788c-121">Pass the hub method name and any arguments defined in the hub method to `InvokeAsync`.</span></span> <span data-ttu-id="6788c-122">SignalR est asynchrone, par conséquent, utilisez `async` et `await` lors de l’appe.</span><span class="sxs-lookup"><span data-stu-id="6788c-122">SignalR is asynchronous, so use `async` and `await` when making the calls.</span></span>

[!code-csharp[InvokeAsync method](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?range=48-49)]

## <a name="call-client-methods-from-hub"></a><span data-ttu-id="6788c-123">Appeler des méthodes de client à partir de hub</span><span class="sxs-lookup"><span data-stu-id="6788c-123">Call client methods from hub</span></span>

<span data-ttu-id="6788c-124">Définissez les méthodes appelées par le hub en utilisant `connection.On` après la génération, mais avant de démarrer la connexion.</span><span class="sxs-lookup"><span data-stu-id="6788c-124">Define methods the hub calls using `connection.On` after building, but before starting the connection.</span></span>

[!code-csharp[Define client methods](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?range=22-29)]

<span data-ttu-id="6788c-125">Le code précédent dans `connection.On` s’exécute lorsque le code côté serveur l’appelle en utilisant la méthode `SendAsync`.</span><span class="sxs-lookup"><span data-stu-id="6788c-125">The preceding code in `connection.On` runs when server-side code calls it using the `SendAsync` method.</span></span>

[!code-csharp[Call client method](dotnet-client/sample/signalrchat/hubs/chathub.cs?range=8-11)]

## <a name="error-handling-and-logging"></a><span data-ttu-id="6788c-126">Journalisation et gestion des erreurs</span><span class="sxs-lookup"><span data-stu-id="6788c-126">Error handling and logging</span></span>

<span data-ttu-id="6788c-127">Gérez les erreurs avec une instruction try-catch.</span><span class="sxs-lookup"><span data-stu-id="6788c-127">Handle errors with a try-catch statement.</span></span> <span data-ttu-id="6788c-128">Inspectez l'objet `Exception` afin de déterminer l’action appropriée à entreprendre après qu'une erreur se produit.</span><span class="sxs-lookup"><span data-stu-id="6788c-128">Inspect the `Exception` object to determine the proper action to take after an error occurs.</span></span>

[!code-csharp[Logging](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?range=46-54)]

## <a name="additional-resources"></a><span data-ttu-id="6788c-129">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="6788c-129">Additional resources</span></span>

* [<span data-ttu-id="6788c-130">Hubs</span><span class="sxs-lookup"><span data-stu-id="6788c-130">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="6788c-131">Client JavaScript</span><span class="sxs-lookup"><span data-stu-id="6788c-131">JavaScript client</span></span>](xref:signalr/javascript-client)
* [<span data-ttu-id="6788c-132">Publier sur Azure</span><span class="sxs-lookup"><span data-stu-id="6788c-132">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)
