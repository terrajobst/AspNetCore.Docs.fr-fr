---
title: Client ASP.NET Core SignalR Java
author: mikaelm12
description: Découvrez comment utiliser le client d’ASP.NET Core SignalR Java.
monikerRange: '>= aspnetcore-2.2'
ms.author: mimengis
ms.custom: mvc
ms.date: 09/06/2018
uid: signalr/java-client
ms.openlocfilehash: 0eba59a05ea6fd3fed46fcab86ac20caf40ebb65
ms.sourcegitcommit: 8bf4dff3069e62972c1b0839a93fb444e502afe7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/20/2018
ms.locfileid: "46482916"
---
# <a name="aspnet-core-signalr-java-client"></a><span data-ttu-id="ce9cb-103">ASP.NET Core SignalR Java Client</span><span class="sxs-lookup"><span data-stu-id="ce9cb-103">ASP.NET Core SignalR Java Client</span></span>

<span data-ttu-id="ce9cb-104">Par [Mikael Mengistu](https://twitter.com/MikaelM_12)</span><span class="sxs-lookup"><span data-stu-id="ce9cb-104">By [Mikael Mengistu](https://twitter.com/MikaelM_12)</span></span>

<span data-ttu-id="ce9cb-105">Le client Java permet la connexion à un serveur d’ASP.NET Core SignalR à partir de code Java, y compris les applications Android.</span><span class="sxs-lookup"><span data-stu-id="ce9cb-105">The Java client enables connecting to an ASP.NET Core SignalR server from Java code, including Android apps.</span></span> <span data-ttu-id="ce9cb-106">Comme le [client JavaScript](xref:signalr/javascript-client) et [client .NET](xref:signalr/dotnet-client), le client Java vous permet de recevoir et envoyer des messages à un hub en temps réel.</span><span class="sxs-lookup"><span data-stu-id="ce9cb-106">Like the [JavaScript client](xref:signalr/javascript-client) and the [.NET client](xref:signalr/dotnet-client), the Java client enables you to receive and send messages to a hub in real time.</span></span> <span data-ttu-id="ce9cb-107">Le client Java est disponible dans ASP.NET Core 2.2 et versions ultérieures.</span><span class="sxs-lookup"><span data-stu-id="ce9cb-107">The Java client is available in ASP.NET Core 2.2 and later.</span></span>

<span data-ttu-id="ce9cb-108">L’exemple d’application de console Java référencé dans cet article utilise le client SignalR Java.</span><span class="sxs-lookup"><span data-stu-id="ce9cb-108">The sample Java console app referenced in this article uses the SignalR Java client.</span></span>

<span data-ttu-id="ce9cb-109">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/java-client/sample) ([procédure de téléchargement](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="ce9cb-109">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/java-client/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="install-the-signalr-java-client-package"></a><span data-ttu-id="ce9cb-110">Installer le package du client Java de SignalR</span><span class="sxs-lookup"><span data-stu-id="ce9cb-110">Install the SignalR Java client package</span></span>

<span data-ttu-id="ce9cb-111">Le *signalr-0.1.0-preview2-35174* fichier JAR permet aux clients pour se connecter à des concentrateurs SignalR.</span><span class="sxs-lookup"><span data-stu-id="ce9cb-111">The *signalr-0.1.0-preview2-35174* JAR file allows clients to connect to SignalR hubs.</span></span> <span data-ttu-id="ce9cb-112">Pour rechercher le dernier numéro de version de fichier JAR, consultez le [résultats de la recherche Maven](https://search.maven.org/search?q=g:com.microsoft.aspnet%20AND%20a:signalr&core=gav).</span><span class="sxs-lookup"><span data-stu-id="ce9cb-112">To find the latest JAR file version number, see the [Maven search results](https://search.maven.org/search?q=g:com.microsoft.aspnet%20AND%20a:signalr&core=gav).</span></span>

<span data-ttu-id="ce9cb-113">Si vous utilisez Gradle, ajoutez la ligne suivante à la `dependencies` section de votre *build.gradle* fichier :</span><span class="sxs-lookup"><span data-stu-id="ce9cb-113">If using Gradle, add the following line to the `dependencies` section of your *build.gradle* file:</span></span>

```gradle
implementation 'com.microsoft.aspnet:signalr:0.1.0-preview2-35174'
```

<span data-ttu-id="ce9cb-114">Si à l’aide de Maven, ajoutez les lignes suivantes à l’intérieur de la `<dependencies>` élément de votre *pom.xml* fichier :</span><span class="sxs-lookup"><span data-stu-id="ce9cb-114">If using Maven, add the following lines inside the `<dependencies>` element of your *pom.xml* file:</span></span>

[!code-xml[pom.xml dependency element](java-client/sample/pom.xml?name=snippet_dependencyElement)]

## <a name="connect-to-a-hub"></a><span data-ttu-id="ce9cb-115">Se connecter à un concentrateur</span><span class="sxs-lookup"><span data-stu-id="ce9cb-115">Connect to a hub</span></span>

<span data-ttu-id="ce9cb-116">Pour établir une `HubConnection`, la `HubConnectionBuilder` doit être utilisé.</span><span class="sxs-lookup"><span data-stu-id="ce9cb-116">To establish a `HubConnection`, the `HubConnectionBuilder` should be used.</span></span> <span data-ttu-id="ce9cb-117">Le niveau d’URL et le journal de hub peut être configuré lors de la création d’une connexion.</span><span class="sxs-lookup"><span data-stu-id="ce9cb-117">The hub URL and log level can be configured while building a connection.</span></span> <span data-ttu-id="ce9cb-118">Configurez les options requises en appelant une de le `HubConnectionBuilder` méthodes avant `build`.</span><span class="sxs-lookup"><span data-stu-id="ce9cb-118">Configure any required options by calling any of the `HubConnectionBuilder` methods before `build`.</span></span> <span data-ttu-id="ce9cb-119">Démarrez la connexion avec `start`.</span><span class="sxs-lookup"><span data-stu-id="ce9cb-119">Start the connection with `start`.</span></span>

[!code-java[Build hub connection](java-client/sample/src/main/java/Chat.java?range=17-20)]

## <a name="call-hub-methods-from-client"></a><span data-ttu-id="ce9cb-120">Appeler des méthodes de hub à partir du client</span><span class="sxs-lookup"><span data-stu-id="ce9cb-120">Call hub methods from client</span></span>

<span data-ttu-id="ce9cb-121">Un appel à `send` appelle une méthode de concentrateur.</span><span class="sxs-lookup"><span data-stu-id="ce9cb-121">A call to `send` invokes a hub method.</span></span> <span data-ttu-id="ce9cb-122">Passez le nom de la méthode de hub et de tous les arguments définis dans la méthode de hub à `send`.</span><span class="sxs-lookup"><span data-stu-id="ce9cb-122">Pass the hub method name and any arguments defined in the hub method to `send`.</span></span>

[!code-java[send method](java-client/sample/src/main/java/Chat.java?range=31)]

## <a name="call-client-methods-from-hub"></a><span data-ttu-id="ce9cb-123">Appeler des méthodes de client à partir de hub</span><span class="sxs-lookup"><span data-stu-id="ce9cb-123">Call client methods from hub</span></span>

<span data-ttu-id="ce9cb-124">Utilisez `hubConnection.on` pour définir des méthodes sur le client que le hub peut appeler.</span><span class="sxs-lookup"><span data-stu-id="ce9cb-124">Use `hubConnection.on` to define methods on the client that the hub can call.</span></span> <span data-ttu-id="ce9cb-125">Définissez les méthodes après la génération, mais avant de démarrer la connexion.</span><span class="sxs-lookup"><span data-stu-id="ce9cb-125">Define the methods after building but before starting the connection.</span></span>

[!code-java[Define client methods](java-client/sample/src/main/java/Chat.java?range=22-24)]

## <a name="known-limitations"></a><span data-ttu-id="ce9cb-126">Limitations connues</span><span class="sxs-lookup"><span data-stu-id="ce9cb-126">Known limitations</span></span>

<span data-ttu-id="ce9cb-127">Il s’agit d’une version préliminaire du client Java.</span><span class="sxs-lookup"><span data-stu-id="ce9cb-127">This is an early preview release of the Java client.</span></span> <span data-ttu-id="ce9cb-128">Il existe de nombreuses fonctionnalités qui ne sont pas encore pris en charge.</span><span class="sxs-lookup"><span data-stu-id="ce9cb-128">There are many features that aren't supported yet.</span></span> <span data-ttu-id="ce9cb-129">Les intervalles suivants sont en cours de traitement pour les versions futures :</span><span class="sxs-lookup"><span data-stu-id="ce9cb-129">The following gaps are being worked on for future releases:</span></span>

* <span data-ttu-id="ce9cb-130">Seuls les types primitifs peuvent être acceptés en tant que paramètres et types de retour.</span><span class="sxs-lookup"><span data-stu-id="ce9cb-130">Only primitive types can be accepted as parameters and return types.</span></span>
* <span data-ttu-id="ce9cb-131">Les API sont synchrones.</span><span class="sxs-lookup"><span data-stu-id="ce9cb-131">The APIs are synchronous.</span></span>
* <span data-ttu-id="ce9cb-132">Seul le type d’appel « Envoyer » est pris en charge pour l’instant.</span><span class="sxs-lookup"><span data-stu-id="ce9cb-132">Only the "Send" call type is supported at this time.</span></span> <span data-ttu-id="ce9cb-133">« Appeler » et la diffusion en continu des valeurs de retour ne sont pas pris en charge.</span><span class="sxs-lookup"><span data-stu-id="ce9cb-133">"Invoke" and streaming of return values aren't supported.</span></span>
* <span data-ttu-id="ce9cb-134">Seul le protocole JSON est pris en charge.</span><span class="sxs-lookup"><span data-stu-id="ce9cb-134">Only the JSON protocol is supported.</span></span>
* <span data-ttu-id="ce9cb-135">Uniquement le transport WebSocket est pris en charge.</span><span class="sxs-lookup"><span data-stu-id="ce9cb-135">Only the WebSockets transport is supported.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="ce9cb-136">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="ce9cb-136">Additional resources</span></span>

* [<span data-ttu-id="ce9cb-137">Référence de l’API Java</span><span class="sxs-lookup"><span data-stu-id="ce9cb-137">Java API reference</span></span>](/java/api/com.microsoft.aspnet.signalr?view=aspnet-signalr-java)
* <xref:signalr/hubs>
* <xref:signalr/javascript-client>
* <xref:signalr/publish-to-azure-web-app>
