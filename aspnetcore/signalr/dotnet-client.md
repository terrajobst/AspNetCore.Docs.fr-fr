---
title: 'Client .NET SignalR ASP.NET Core '
author: bradygaster
description: Informations sur le Client .NET SignalR ASP.NET Core
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 03/14/2019
uid: signalr/dotnet-client
ms.openlocfilehash: a03abef53aa44f0a1016b8f72d8e3a7af2f9bed1
ms.sourcegitcommit: d913bca90373c07f89b1d1df01af5fc01fc908ef
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/14/2019
ms.locfileid: "57978302"
---
# <a name="aspnet-core-signalr-net-client"></a><span data-ttu-id="cb483-103">Client .NET SignalR ASP.NET Core </span><span class="sxs-lookup"><span data-stu-id="cb483-103">ASP.NET Core SignalR .NET Client</span></span>

<span data-ttu-id="cb483-104">La bibliothèque cliente ASP.NET Core SignalR .NET vous permet de communiquer avec les hubs SignalR à partir des applications .NET.</span><span class="sxs-lookup"><span data-stu-id="cb483-104">The ASP.NET Core SignalR .NET client library lets you communicate with SignalR hubs from .NET apps.</span></span>

> [!NOTE]
> <span data-ttu-id="cb483-105">Xamarin a des prérequis spéciaux pour la version de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="cb483-105">Xamarin has special requirements for Visual Studio version.</span></span> <span data-ttu-id="cb483-106">Pour plus d’informations, consultez [SignalR Client 2.1.1 dans Xamarin](https://github.com/aspnet/Announcements/issues/305).</span><span class="sxs-lookup"><span data-stu-id="cb483-106">For more information, see [SignalR Client 2.1.1 in Xamarin](https://github.com/aspnet/Announcements/issues/305).</span></span>

<span data-ttu-id="cb483-107">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/dotnet-client/sample) ([procédure de téléchargement](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="cb483-107">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/dotnet-client/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="cb483-108">L’exemple de code dans cet article est une application WPF qui utilise le client .NET SignalR ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="cb483-108">The code sample in this article is a WPF app that uses the ASP.NET Core SignalR .NET client.</span></span>

## <a name="install-the-signalr-net-client-package"></a><span data-ttu-id="cb483-109">Installer le package du client .NET SignalR</span><span class="sxs-lookup"><span data-stu-id="cb483-109">Install the SignalR .NET client package</span></span>

<span data-ttu-id="cb483-110">Le package `Microsoft.AspNetCore.SignalR.Client` est requis pour que les clients .NET se connectent à des hubs SignalR.</span><span class="sxs-lookup"><span data-stu-id="cb483-110">The `Microsoft.AspNetCore.SignalR.Client` package is needed for .NET clients to connect to SignalR hubs.</span></span> <span data-ttu-id="cb483-111">Pour installer la bibliothèque cliente, exécutez la commande suivante dans la fenêtre **Console du Gestionnaire de package** :</span><span class="sxs-lookup"><span data-stu-id="cb483-111">To install the client library, run the following command in the **Package Manager Console** window:</span></span>

```powershell
Install-Package Microsoft.AspNetCore.SignalR.Client
```

## <a name="connect-to-a-hub"></a><span data-ttu-id="cb483-112">Se connecter à un hub</span><span class="sxs-lookup"><span data-stu-id="cb483-112">Connect to a hub</span></span>

<span data-ttu-id="cb483-113">Pour établir une connexion, créez un `HubConnectionBuilder` et appelez-le `Build`.</span><span class="sxs-lookup"><span data-stu-id="cb483-113">To establish a connection, create a `HubConnectionBuilder` and call `Build`.</span></span> <span data-ttu-id="cb483-114">L’URL du hub, le protocole, le type de transport, le niveau du journalisation, les en-têtes et les autres options peuvent être configurés lors de la création d’une connexion.</span><span class="sxs-lookup"><span data-stu-id="cb483-114">The hub URL, protocol, transport type, log level, headers, and other options can be configured while building a connection.</span></span> <span data-ttu-id="cb483-115">Configurez les options requises en insérant les méthodes du `HubConnectionBuilder` dans `Build`.</span><span class="sxs-lookup"><span data-stu-id="cb483-115">Configure any required options by inserting any of the `HubConnectionBuilder` methods into `Build`.</span></span> <span data-ttu-id="cb483-116">Démarrez la connexion avec `StartAsync`.</span><span class="sxs-lookup"><span data-stu-id="cb483-116">Start the connection with `StartAsync`.</span></span>

[!code-csharp[Build hub connection](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_MainWindowClass&highlight=15-17,39)]

## <a name="handle-lost-connection"></a><span data-ttu-id="cb483-117">Gérer la perte de connexion</span><span class="sxs-lookup"><span data-stu-id="cb483-117">Handle lost connection</span></span>

<span data-ttu-id="cb483-118">Utilisez l'événement <xref:Microsoft.AspNetCore.SignalR.Client.HubConnection.Closed> pour répondre à une perte de connexion.</span><span class="sxs-lookup"><span data-stu-id="cb483-118">Use the <xref:Microsoft.AspNetCore.SignalR.Client.HubConnection.Closed> event to respond to a lost connection.</span></span> <span data-ttu-id="cb483-119">Par exemple, vous pouvez souhaiter automatiser une reconnexion.</span><span class="sxs-lookup"><span data-stu-id="cb483-119">For example, you might want to automate reconnection.</span></span>

<span data-ttu-id="cb483-120">L'événement `Closed` nécessite un délégué qui retourne une `Task`, ce qui permet l’exécution sans utiliser de code asynchrone `async void`.</span><span class="sxs-lookup"><span data-stu-id="cb483-120">The `Closed` event requires a delegate that returns a `Task`, which allows async code to run without using `async void`.</span></span> <span data-ttu-id="cb483-121">Pour répondre à la signature du délégué dans un gestionnaire d’événements `Closed` qui s’exécute de façon synchrone, retournez `Task.CompletedTask`:</span><span class="sxs-lookup"><span data-stu-id="cb483-121">To satisfy the delegate signature in a `Closed` event handler that runs synchronously, return `Task.CompletedTask`:</span></span>

```csharp
connection.Closed += (error) => {
    // Do your close logic.
    return Task.CompletedTask;
};
```

<span data-ttu-id="cb483-122">La principale raison de la prise en charge asynchrone est de pouvoir redémarrer la connexion.</span><span class="sxs-lookup"><span data-stu-id="cb483-122">The main reason for the async support is so you can restart the connection.</span></span> <span data-ttu-id="cb483-123">Le démarrage d’une connexion est une action asynchrone.</span><span class="sxs-lookup"><span data-stu-id="cb483-123">Starting a connection is an async action.</span></span>

<span data-ttu-id="cb483-124">Dans un gestionnaire `Closed` qui redémarre la connexion, envisagez d’attendre un délai aléatoire afin d'éviter de surcharger le serveur, comme indiqué dans l’exemple suivant :</span><span class="sxs-lookup"><span data-stu-id="cb483-124">In a `Closed` handler that restarts the connection, consider waiting for some random delay to prevent overloading the server, as shown in the following example:</span></span>

[!code-csharp[Use Closed event handler to automate reconnection](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_ClosedRestart)]

## <a name="call-hub-methods-from-client"></a><span data-ttu-id="cb483-125">Appeler des méthodes de hub à partir du client</span><span class="sxs-lookup"><span data-stu-id="cb483-125">Call hub methods from client</span></span>

<span data-ttu-id="cb483-126">`InvokeAsync` appelle des méthodes sur le hub.</span><span class="sxs-lookup"><span data-stu-id="cb483-126">`InvokeAsync` calls methods on the hub.</span></span> <span data-ttu-id="cb483-127">Passez le nom de la méthode de hub et de tous les arguments définis dans la méthode de hub à `InvokeAsync`.</span><span class="sxs-lookup"><span data-stu-id="cb483-127">Pass the hub method name and any arguments defined in the hub method to `InvokeAsync`.</span></span> <span data-ttu-id="cb483-128">SignalR est asynchrone, par conséquent, utilisez `async` et `await` lors de l’appel.</span><span class="sxs-lookup"><span data-stu-id="cb483-128">SignalR is asynchronous, so use `async` and `await` when making the calls.</span></span>

[!code-csharp[InvokeAsync method](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_InvokeAsync)]

> [!NOTE]
> <span data-ttu-id="cb483-129">Si vous utilisez le Service Azure SignalR dans *mode sans serveur*, vous ne pouvez pas appeler des méthodes de concentrateur à partir d’un client.</span><span class="sxs-lookup"><span data-stu-id="cb483-129">If you're using Azure SignalR Service in *Serverless mode*, you cannot call hub methods from a client.</span></span> <span data-ttu-id="cb483-130">Pour plus d’informations, consultez le [documentation de SignalR Service](/azure/azure-signalr/signalr-concept-serverless-development-config).</span><span class="sxs-lookup"><span data-stu-id="cb483-130">For more information, see the [SignalR Service documentation](/azure/azure-signalr/signalr-concept-serverless-development-config).</span></span>

## <a name="call-client-methods-from-hub"></a><span data-ttu-id="cb483-131">Appeler des méthodes de client à partir de hub</span><span class="sxs-lookup"><span data-stu-id="cb483-131">Call client methods from hub</span></span>

<span data-ttu-id="cb483-132">Définissez les méthodes appelées par le hub en utilisant `connection.On` après la génération, mais avant de démarrer la connexion.</span><span class="sxs-lookup"><span data-stu-id="cb483-132">Define methods the hub calls using `connection.On` after building, but before starting the connection.</span></span>

[!code-csharp[Define client methods](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_ConnectionOn)]

<span data-ttu-id="cb483-133">Le code précédent dans `connection.On` s’exécute lorsque le code côté serveur l’appelle en utilisant la méthode `SendAsync`.</span><span class="sxs-lookup"><span data-stu-id="cb483-133">The preceding code in `connection.On` runs when server-side code calls it using the `SendAsync` method.</span></span>

[!code-csharp[Call client method](dotnet-client/sample/signalrchat/hubs/chathub.cs?name=snippet_SendMessage)]

## <a name="error-handling-and-logging"></a><span data-ttu-id="cb483-134">Journalisation et gestion des erreurs</span><span class="sxs-lookup"><span data-stu-id="cb483-134">Error handling and logging</span></span>

<span data-ttu-id="cb483-135">Gérez les erreurs avec une instruction try-catch.</span><span class="sxs-lookup"><span data-stu-id="cb483-135">Handle errors with a try-catch statement.</span></span> <span data-ttu-id="cb483-136">Inspectez l'objet `Exception` afin de déterminer l’action appropriée à entreprendre après qu'une erreur se produit.</span><span class="sxs-lookup"><span data-stu-id="cb483-136">Inspect the `Exception` object to determine the proper action to take after an error occurs.</span></span>

[!code-csharp[Logging](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_ErrorHandling)]

## <a name="additional-resources"></a><span data-ttu-id="cb483-137">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="cb483-137">Additional resources</span></span>

* [<span data-ttu-id="cb483-138">Hubs</span><span class="sxs-lookup"><span data-stu-id="cb483-138">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="cb483-139">Client JavaScript</span><span class="sxs-lookup"><span data-stu-id="cb483-139">JavaScript client</span></span>](xref:signalr/javascript-client)
* [<span data-ttu-id="cb483-140">Publier sur Azure</span><span class="sxs-lookup"><span data-stu-id="cb483-140">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)
* [<span data-ttu-id="cb483-141">Documentation de serverless Azure SignalR Service</span><span class="sxs-lookup"><span data-stu-id="cb483-141">Azure SignalR Service serverless documentation</span></span>](/azure/azure-signalr/signalr-concept-serverless-development-config)
