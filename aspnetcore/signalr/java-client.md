---
title: Client ASP.NET Core SignalR Java
author: mikaelm12
description: Découvrez comment utiliser le client d’ASP.NET Core SignalR Java.
monikerRange: '>= aspnetcore-2.2'
ms.author: mimengis
ms.custom: mvc
ms.date: 11/07/2018
uid: signalr/java-client
ms.openlocfilehash: d0eff38c1f622b896ed1dc3002238aec7b6bfd38
ms.sourcegitcommit: 8a65f6c2cbe290fb2418eed58f60fb74c95392c8
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/05/2018
ms.locfileid: "52892092"
---
# <a name="aspnet-core-signalr-java-client"></a><span data-ttu-id="f8121-103">Client ASP.NET Core SignalR Java</span><span class="sxs-lookup"><span data-stu-id="f8121-103">ASP.NET Core SignalR Java client</span></span>

<span data-ttu-id="f8121-104">Par [Mikael Mengistu](https://twitter.com/MikaelM_12)</span><span class="sxs-lookup"><span data-stu-id="f8121-104">By [Mikael Mengistu](https://twitter.com/MikaelM_12)</span></span>

<span data-ttu-id="f8121-105">Le client Java permet la connexion à un serveur d’ASP.NET Core SignalR à partir de code Java, y compris les applications Android.</span><span class="sxs-lookup"><span data-stu-id="f8121-105">The Java client enables connecting to an ASP.NET Core SignalR server from Java code, including Android apps.</span></span> <span data-ttu-id="f8121-106">Comme le [client JavaScript](xref:signalr/javascript-client) et [client .NET](xref:signalr/dotnet-client), le client Java vous permet de recevoir et envoyer des messages à un hub en temps réel.</span><span class="sxs-lookup"><span data-stu-id="f8121-106">Like the [JavaScript client](xref:signalr/javascript-client) and the [.NET client](xref:signalr/dotnet-client), the Java client enables you to receive and send messages to a hub in real time.</span></span> <span data-ttu-id="f8121-107">Le client Java est disponible dans ASP.NET Core 2.2 et versions ultérieures.</span><span class="sxs-lookup"><span data-stu-id="f8121-107">The Java client is available in ASP.NET Core 2.2 and later.</span></span>

<span data-ttu-id="f8121-108">L’exemple d’application de console Java référencé dans cet article utilise le client SignalR Java.</span><span class="sxs-lookup"><span data-stu-id="f8121-108">The sample Java console app referenced in this article uses the SignalR Java client.</span></span>

<span data-ttu-id="f8121-109">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/java-client/sample) ([procédure de téléchargement](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="f8121-109">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/java-client/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="install-the-signalr-java-client-package"></a><span data-ttu-id="f8121-110">Installer le package du client Java de SignalR</span><span class="sxs-lookup"><span data-stu-id="f8121-110">Install the SignalR Java client package</span></span>

<span data-ttu-id="f8121-111">Le *signalr-1.0.0* fichier JAR permet aux clients pour se connecter à des concentrateurs SignalR.</span><span class="sxs-lookup"><span data-stu-id="f8121-111">The *signalr-1.0.0* JAR file allows clients to connect to SignalR hubs.</span></span> <span data-ttu-id="f8121-112">Pour rechercher le dernier numéro de version de fichier JAR, consultez le [résultats de la recherche Maven](https://search.maven.org/search?q=g:com.microsoft.signalr%20AND%20a:signalr).</span><span class="sxs-lookup"><span data-stu-id="f8121-112">To find the latest JAR file version number, see the [Maven search results](https://search.maven.org/search?q=g:com.microsoft.signalr%20AND%20a:signalr).</span></span>

<span data-ttu-id="f8121-113">Si vous utilisez Gradle, ajoutez la ligne suivante à la `dependencies` section de votre *build.gradle* fichier :</span><span class="sxs-lookup"><span data-stu-id="f8121-113">If using Gradle, add the following line to the `dependencies` section of your *build.gradle* file:</span></span>

```gradle
implementation 'com.microsoft.signalr:signalr:1.0.0'
```

<span data-ttu-id="f8121-114">Si à l’aide de Maven, ajoutez les lignes suivantes à l’intérieur de la `<dependencies>` élément de votre *pom.xml* fichier :</span><span class="sxs-lookup"><span data-stu-id="f8121-114">If using Maven, add the following lines inside the `<dependencies>` element of your *pom.xml* file:</span></span>

[!code-xml[pom.xml dependency element](java-client/sample/pom.xml?name=snippet_dependencyElement)]

## <a name="connect-to-a-hub"></a><span data-ttu-id="f8121-115">Se connecter à un hub</span><span class="sxs-lookup"><span data-stu-id="f8121-115">Connect to a hub</span></span>

<span data-ttu-id="f8121-116">Pour établir une `HubConnection`, la `HubConnectionBuilder` doit être utilisé.</span><span class="sxs-lookup"><span data-stu-id="f8121-116">To establish a `HubConnection`, the `HubConnectionBuilder` should be used.</span></span> <span data-ttu-id="f8121-117">Le niveau d’URL et le journal de hub peut être configuré lors de la création d’une connexion.</span><span class="sxs-lookup"><span data-stu-id="f8121-117">The hub URL and log level can be configured while building a connection.</span></span> <span data-ttu-id="f8121-118">Configurez les options requises en appelant une de le `HubConnectionBuilder` méthodes avant `build`.</span><span class="sxs-lookup"><span data-stu-id="f8121-118">Configure any required options by calling any of the `HubConnectionBuilder` methods before `build`.</span></span> <span data-ttu-id="f8121-119">Démarrez la connexion avec `start`.</span><span class="sxs-lookup"><span data-stu-id="f8121-119">Start the connection with `start`.</span></span>

[!code-java[Build hub connection](java-client/sample/src/main/java/Chat.java?range=16-17)]

## <a name="call-hub-methods-from-client"></a><span data-ttu-id="f8121-120">Appeler des méthodes de hub à partir du client</span><span class="sxs-lookup"><span data-stu-id="f8121-120">Call hub methods from client</span></span>

<span data-ttu-id="f8121-121">Un appel à `send` appelle une méthode de concentrateur.</span><span class="sxs-lookup"><span data-stu-id="f8121-121">A call to `send` invokes a hub method.</span></span> <span data-ttu-id="f8121-122">Passez le nom de la méthode de hub et de tous les arguments définis dans la méthode de hub à `send`.</span><span class="sxs-lookup"><span data-stu-id="f8121-122">Pass the hub method name and any arguments defined in the hub method to `send`.</span></span>

[!code-java[send method](java-client/sample/src/main/java/Chat.java?range=28)]

## <a name="call-client-methods-from-hub"></a><span data-ttu-id="f8121-123">Appeler des méthodes de client à partir de hub</span><span class="sxs-lookup"><span data-stu-id="f8121-123">Call client methods from hub</span></span>

<span data-ttu-id="f8121-124">Utilisez `hubConnection.on` pour définir des méthodes sur le client que le hub peut appeler.</span><span class="sxs-lookup"><span data-stu-id="f8121-124">Use `hubConnection.on` to define methods on the client that the hub can call.</span></span> <span data-ttu-id="f8121-125">Définissez les méthodes après la génération, mais avant de démarrer la connexion.</span><span class="sxs-lookup"><span data-stu-id="f8121-125">Define the methods after building but before starting the connection.</span></span>

[!code-java[Define client methods](java-client/sample/src/main/java/Chat.java?range=19-21)]

## <a name="add-logging"></a><span data-ttu-id="f8121-126">Ajouter la journalisation</span><span class="sxs-lookup"><span data-stu-id="f8121-126">Add logging</span></span>

<span data-ttu-id="f8121-127">Le client SignalR Java utilise le [SLF4J](https://www.slf4j.org/) bibliothèque pour la journalisation.</span><span class="sxs-lookup"><span data-stu-id="f8121-127">The SignalR Java client uses the [SLF4J](https://www.slf4j.org/) library for logging.</span></span> <span data-ttu-id="f8121-128">C’est une API de haut niveau de journalisation qui permet aux utilisateurs de la bibliothèque de choisir leur propre implémentation de journalisation spécifiques en apportant une dépendance de journalisation spécifique.</span><span class="sxs-lookup"><span data-stu-id="f8121-128">It's a high-level logging API that allows users of the library to chose their own specific logging implementation by bringing in a specific logging dependency.</span></span> <span data-ttu-id="f8121-129">L’extrait de code suivant montre comment utiliser `java.util.logging` avec le client SignalR Java.</span><span class="sxs-lookup"><span data-stu-id="f8121-129">The following code snippet shows how to use `java.util.logging` with the SignalR Java client.</span></span>

```gradle
implementation 'org.slf4j:slf4j-jdk14:1.7.25'
```

<span data-ttu-id="f8121-130">Si vous ne configurez pas la journalisation dans vos dépendances, SLF4J charge un enregistreur d’événements non opération par défaut avec le message d’avertissement suivant :</span><span class="sxs-lookup"><span data-stu-id="f8121-130">If you don't configure logging in your dependencies, SLF4J loads a default no-operation logger with the following warning message:</span></span>

```
SLF4J: Failed to load class "org.slf4j.impl.StaticLoggerBinder".
SLF4J: Defaulting to no-operation (NOP) logger implementation
SLF4J: See http://www.slf4j.org/codes.html#StaticLoggerBinder for further details.
```

<span data-ttu-id="f8121-131">Cela peut être ignoré sans risque.</span><span class="sxs-lookup"><span data-stu-id="f8121-131">This can safely be ignored.</span></span>

## <a name="android-development-notes"></a><span data-ttu-id="f8121-132">Notes de développement Android</span><span class="sxs-lookup"><span data-stu-id="f8121-132">Android development notes</span></span>

<span data-ttu-id="f8121-133">En ce qui concerne la compatibilité du Kit Android SDK pour les fonctionnalités de client SignalR, tenez compte des éléments suivants lorsque vous spécifiez votre version de SDK Android cible :</span><span class="sxs-lookup"><span data-stu-id="f8121-133">With regards to Android SDK compatibility for the SignalR client features, consider the following items when specifying your target Android SDK version:</span></span>

* <span data-ttu-id="f8121-134">Le Client Java SignalR exécutera sur Android API niveau 16 et versions ultérieures.</span><span class="sxs-lookup"><span data-stu-id="f8121-134">The SignalR Java Client will run on Android API Level 16 and later.</span></span>
* <span data-ttu-id="f8121-135">Connexion via le Service Azure SignalR nécessitera le niveau d’API Android 20 et versions ultérieur, car le [Service Azure SignalR](/azure/azure-signalr/signalr-overview) nécessite TLS 1.2 et ne prend pas en charge les suites de chiffrement basé sur SHA-1.</span><span class="sxs-lookup"><span data-stu-id="f8121-135">Connecting through the Azure SignalR Service will require Android API Level 20 and later because the [Azure SignalR Service](/azure/azure-signalr/signalr-overview) requires TLS 1.2 and doesn't support SHA-1-based cipher suites.</span></span> <span data-ttu-id="f8121-136">Android [ajouté des suites de chiffrement prise en charge pour l’algorithme SHA-256 (et versions ultérieures)](https://developer.android.com/reference/javax/net/ssl/SSLSocket) dans le niveau d’API 20.</span><span class="sxs-lookup"><span data-stu-id="f8121-136">Android [added support for SHA-256 (and above) cipher suites](https://developer.android.com/reference/javax/net/ssl/SSLSocket) in API Level 20.</span></span>

## <a name="configure-bearer-token-authentication"></a><span data-ttu-id="f8121-137">Configurer l’authentification du jeton du porteur</span><span class="sxs-lookup"><span data-stu-id="f8121-137">Configure bearer token authentication</span></span>

<span data-ttu-id="f8121-138">Dans le client SignalR Java, vous pouvez configurer un jeton du porteur à utiliser pour l’authentification en fournissant une « fabrique jeton accès » à la [HttpHubConnectionBuilder](/java/api/com.microsoft.signalr._http_hub_connection_builder?view=aspnet-signalr-java).</span><span class="sxs-lookup"><span data-stu-id="f8121-138">In the SignalR Java client, you can configure a bearer token to use for authentication by providing an "access token factory" to the [HttpHubConnectionBuilder](/java/api/com.microsoft.signalr._http_hub_connection_builder?view=aspnet-signalr-java).</span></span> <span data-ttu-id="f8121-139">Utilisez [withAccessTokenFactory](/java/api/com.microsoft.signalr._http_hub_connection_builder.withaccesstokenprovider?view=aspnet-signalr-java#com_microsoft_signalr__http_hub_connection_builder_withAccessTokenProvider_Single_String__) pour fournir un [RxJava](https://github.com/ReactiveX/RxJava) [unique<String>](http://reactivex.io/documentation/single.html).</span><span class="sxs-lookup"><span data-stu-id="f8121-139">Use [withAccessTokenFactory](/java/api/com.microsoft.signalr._http_hub_connection_builder.withaccesstokenprovider?view=aspnet-signalr-java#com_microsoft_signalr__http_hub_connection_builder_withAccessTokenProvider_Single_String__) to provide an [RxJava](https://github.com/ReactiveX/RxJava) [Single<String>](http://reactivex.io/documentation/single.html).</span></span> <span data-ttu-id="f8121-140">Avec un appel à [Single.defer](http://reactivex.io/RxJava/javadoc/io/reactivex/Single.html#defer-java.util.concurrent.Callable-), vous pouvez écrire la logique pour générer des jetons d’accès pour votre client.</span><span class="sxs-lookup"><span data-stu-id="f8121-140">With a call to [Single.defer](http://reactivex.io/RxJava/javadoc/io/reactivex/Single.html#defer-java.util.concurrent.Callable-), you can write logic to produce access tokens for your client.</span></span>

```java
HubConnection hubConnection = HubConnectionBuilder.create("YOUR HUB URL HERE")
    .withAccessTokenProvider(Single.defer(() -> {
        // Your logic here.
        return Single.just("An Access Token");
    })).build();
```

## <a name="known-limitations"></a><span data-ttu-id="f8121-141">Limitations connues</span><span class="sxs-lookup"><span data-stu-id="f8121-141">Known limitations</span></span>

* <span data-ttu-id="f8121-142">Seul le protocole JSON est pris en charge.</span><span class="sxs-lookup"><span data-stu-id="f8121-142">Only the JSON protocol is supported.</span></span>
* <span data-ttu-id="f8121-143">Uniquement le transport WebSocket est pris en charge.</span><span class="sxs-lookup"><span data-stu-id="f8121-143">Only the WebSockets transport is supported.</span></span>
* <span data-ttu-id="f8121-144">Diffusion en continu n’est pas encore pris en charge.</span><span class="sxs-lookup"><span data-stu-id="f8121-144">Streaming isn't supported yet.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="f8121-145">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="f8121-145">Additional resources</span></span>

* [<span data-ttu-id="f8121-146">Référence API Java</span><span class="sxs-lookup"><span data-stu-id="f8121-146">Java API reference</span></span>](/java/api/com.microsoft.signalr?view=aspnet-signalr-java)
* <xref:signalr/hubs>
* <xref:signalr/javascript-client>
* <xref:signalr/publish-to-azure-web-app>
