---
title: ASP.NET Core SignalR client Java
author: mikaelm12
description: Découvrez comment utiliser le client ASP.NET Core SignalR Java.
monikerRange: '>= aspnetcore-2.2'
ms.author: mimengis
ms.custom: mvc
ms.date: 11/12/2019
no-loc:
- SignalR
uid: signalr/java-client
ms.openlocfilehash: d7143b2c22ecdc4e68f484aa4c244e1c520beae0
ms.sourcegitcommit: 3fc3020961e1289ee5bf5f3c365ce8304d8ebf19
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/12/2019
ms.locfileid: "73963780"
---
# <a name="aspnet-core-opno-locsignalr-java-client"></a><span data-ttu-id="fdae1-103">ASP.NET Core SignalR client Java</span><span class="sxs-lookup"><span data-stu-id="fdae1-103">ASP.NET Core SignalR Java client</span></span>

<span data-ttu-id="fdae1-104">Par [Mikael Mengistu](https://twitter.com/MikaelM_12)</span><span class="sxs-lookup"><span data-stu-id="fdae1-104">By [Mikael Mengistu](https://twitter.com/MikaelM_12)</span></span>

<span data-ttu-id="fdae1-105">Le client Java permet de se connecter à un serveur ASP.NET Core SignalR à partir de code Java, y compris les applications Android.</span><span class="sxs-lookup"><span data-stu-id="fdae1-105">The Java client enables connecting to an ASP.NET Core SignalR server from Java code, including Android apps.</span></span> <span data-ttu-id="fdae1-106">À l’instar du client [JavaScript](xref:signalr/javascript-client) et du [client .net](xref:signalr/dotnet-client), le client Java vous permet de recevoir et d’envoyer des messages à un hub en temps réel.</span><span class="sxs-lookup"><span data-stu-id="fdae1-106">Like the [JavaScript client](xref:signalr/javascript-client) and the [.NET client](xref:signalr/dotnet-client), the Java client enables you to receive and send messages to a hub in real time.</span></span> <span data-ttu-id="fdae1-107">Le client Java est disponible dans ASP.NET Core 2,2 et versions ultérieures.</span><span class="sxs-lookup"><span data-stu-id="fdae1-107">The Java client is available in ASP.NET Core 2.2 and later.</span></span>

<span data-ttu-id="fdae1-108">L’exemple d’application de console Java référencé dans cet article utilise le client SignalR Java.</span><span class="sxs-lookup"><span data-stu-id="fdae1-108">The sample Java console app referenced in this article uses the SignalR Java client.</span></span>

<span data-ttu-id="fdae1-109">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/signalr/java-client/sample) ([procédure de téléchargement](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="fdae1-109">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/signalr/java-client/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="install-the-opno-locsignalr-java-client-package"></a><span data-ttu-id="fdae1-110">Installer le package client Java SignalR</span><span class="sxs-lookup"><span data-stu-id="fdae1-110">Install the SignalR Java client package</span></span>

<span data-ttu-id="fdae1-111">Le fichier jar *signalr-1.0.0* permet aux clients de se connecter à SignalR hubs.</span><span class="sxs-lookup"><span data-stu-id="fdae1-111">The *signalr-1.0.0* JAR file allows clients to connect to SignalR hubs.</span></span> <span data-ttu-id="fdae1-112">Pour rechercher le dernier numéro de version du fichier JAR, consultez les résultats de la [recherche Maven](https://search.maven.org/search?q=g:com.microsoft.signalr%20AND%20a:signalr).</span><span class="sxs-lookup"><span data-stu-id="fdae1-112">To find the latest JAR file version number, see the [Maven search results](https://search.maven.org/search?q=g:com.microsoft.signalr%20AND%20a:signalr).</span></span>

<span data-ttu-id="fdae1-113">Si vous utilisez Gradle, ajoutez la ligne suivante à la section `dependencies` de votre fichier *Build. Gradle* :</span><span class="sxs-lookup"><span data-stu-id="fdae1-113">If using Gradle, add the following line to the `dependencies` section of your *build.gradle* file:</span></span>

```gradle
implementation 'com.microsoft.signalr:signalr:1.0.0'
```

<span data-ttu-id="fdae1-114">Si vous utilisez Maven, ajoutez les lignes suivantes dans l’élément `<dependencies>` de votre fichier *fichier POM. xml* :</span><span class="sxs-lookup"><span data-stu-id="fdae1-114">If using Maven, add the following lines inside the `<dependencies>` element of your *pom.xml* file:</span></span>

[!code-xml[pom.xml dependency element](java-client/sample/pom.xml?name=snippet_dependencyElement)]

## <a name="connect-to-a-hub"></a><span data-ttu-id="fdae1-115">Se connecter à un hub</span><span class="sxs-lookup"><span data-stu-id="fdae1-115">Connect to a hub</span></span>

<span data-ttu-id="fdae1-116">Pour établir un `HubConnection`, le `HubConnectionBuilder` doit être utilisé.</span><span class="sxs-lookup"><span data-stu-id="fdae1-116">To establish a `HubConnection`, the `HubConnectionBuilder` should be used.</span></span> <span data-ttu-id="fdae1-117">L’URL du Hub et le niveau de journalisation peuvent être configurés lors de la création d’une connexion.</span><span class="sxs-lookup"><span data-stu-id="fdae1-117">The hub URL and log level can be configured while building a connection.</span></span> <span data-ttu-id="fdae1-118">Configurez toutes les options requises en appelant l’une des méthodes `HubConnectionBuilder` avant `build`.</span><span class="sxs-lookup"><span data-stu-id="fdae1-118">Configure any required options by calling any of the `HubConnectionBuilder` methods before `build`.</span></span> <span data-ttu-id="fdae1-119">Démarrez la connexion avec `start`.</span><span class="sxs-lookup"><span data-stu-id="fdae1-119">Start the connection with `start`.</span></span>

[!code-java[Build hub connection](java-client/sample/src/main/java/Chat.java?range=16-17)]

## <a name="call-hub-methods-from-client"></a><span data-ttu-id="fdae1-120">Appeler des méthodes de hub à partir du client</span><span class="sxs-lookup"><span data-stu-id="fdae1-120">Call hub methods from client</span></span>

<span data-ttu-id="fdae1-121">Un appel à `send` appelle une méthode de concentrateur.</span><span class="sxs-lookup"><span data-stu-id="fdae1-121">A call to `send` invokes a hub method.</span></span> <span data-ttu-id="fdae1-122">Passez le nom de la méthode de hub et de tous les arguments définis dans la méthode de hub à `send`.</span><span class="sxs-lookup"><span data-stu-id="fdae1-122">Pass the hub method name and any arguments defined in the hub method to `send`.</span></span>

[!code-java[send method](java-client/sample/src/main/java/Chat.java?range=28)]

> [!NOTE]
> <span data-ttu-id="fdae1-123">Si vous utilisez le service Azure SignalR en *mode sans serveur*, vous ne pouvez pas appeler les méthodes de concentrateur à partir d’un client.</span><span class="sxs-lookup"><span data-stu-id="fdae1-123">If you're using Azure SignalR Service in *Serverless mode*, you cannot call hub methods from a client.</span></span> <span data-ttu-id="fdae1-124">Pour plus d’informations, consultez la [documentation du ServiceSignalR](/azure/azure-signalr/signalr-concept-serverless-development-config).</span><span class="sxs-lookup"><span data-stu-id="fdae1-124">For more information, see the [SignalR Service documentation](/azure/azure-signalr/signalr-concept-serverless-development-config).</span></span>

## <a name="call-client-methods-from-hub"></a><span data-ttu-id="fdae1-125">Appeler des méthodes clientes à partir du Hub</span><span class="sxs-lookup"><span data-stu-id="fdae1-125">Call client methods from hub</span></span>

<span data-ttu-id="fdae1-126">Utilisez `hubConnection.on` pour définir des méthodes sur le client que le hub peut appeler.</span><span class="sxs-lookup"><span data-stu-id="fdae1-126">Use `hubConnection.on` to define methods on the client that the hub can call.</span></span> <span data-ttu-id="fdae1-127">Définissez les méthodes après la génération mais avant de commencer la connexion.</span><span class="sxs-lookup"><span data-stu-id="fdae1-127">Define the methods after building but before starting the connection.</span></span>

[!code-java[Define client methods](java-client/sample/src/main/java/Chat.java?range=19-21)]

## <a name="add-logging"></a><span data-ttu-id="fdae1-128">Ajouter la journalisation</span><span class="sxs-lookup"><span data-stu-id="fdae1-128">Add logging</span></span>

<span data-ttu-id="fdae1-129">Le client Java SignalR utilise la bibliothèque [SLF4J](https://www.slf4j.org/) pour la journalisation.</span><span class="sxs-lookup"><span data-stu-id="fdae1-129">The SignalR Java client uses the [SLF4J](https://www.slf4j.org/) library for logging.</span></span> <span data-ttu-id="fdae1-130">Il s’agit d’une API de journalisation de haut niveau qui permet aux utilisateurs de la bibliothèque de choisir leur propre implémentation de journalisation spécifique en introduisant une dépendance de journalisation spécifique.</span><span class="sxs-lookup"><span data-stu-id="fdae1-130">It's a high-level logging API that allows users of the library to chose their own specific logging implementation by bringing in a specific logging dependency.</span></span> <span data-ttu-id="fdae1-131">L’extrait de code suivant montre comment utiliser `java.util.logging` avec le client Java SignalR.</span><span class="sxs-lookup"><span data-stu-id="fdae1-131">The following code snippet shows how to use `java.util.logging` with the SignalR Java client.</span></span>

```gradle
implementation 'org.slf4j:slf4j-jdk14:1.7.25'
```

<span data-ttu-id="fdae1-132">Si vous ne configurez pas la journalisation dans vos dépendances, SLF4J charge un journal de non-opération par défaut avec le message d’avertissement suivant :</span><span class="sxs-lookup"><span data-stu-id="fdae1-132">If you don't configure logging in your dependencies, SLF4J loads a default no-operation logger with the following warning message:</span></span>

```
SLF4J: Failed to load class "org.slf4j.impl.StaticLoggerBinder".
SLF4J: Defaulting to no-operation (NOP) logger implementation
SLF4J: See http://www.slf4j.org/codes.html#StaticLoggerBinder for further details.
```

<span data-ttu-id="fdae1-133">Cela peut être ignoré en toute sécurité.</span><span class="sxs-lookup"><span data-stu-id="fdae1-133">This can safely be ignored.</span></span>

## <a name="android-development-notes"></a><span data-ttu-id="fdae1-134">Remarques sur le développement Android</span><span class="sxs-lookup"><span data-stu-id="fdae1-134">Android development notes</span></span>

<span data-ttu-id="fdae1-135">En ce qui concerne la Android SDK compatibilité pour les fonctionnalités du client SignalR, tenez compte des éléments suivants lorsque vous spécifiez votre version de Android SDK cible :</span><span class="sxs-lookup"><span data-stu-id="fdae1-135">With regards to Android SDK compatibility for the SignalR client features, consider the following items when specifying your target Android SDK version:</span></span>

* <span data-ttu-id="fdae1-136">Le client Java SignalR s’exécute sur l’API Android de niveau 16 et versions ultérieures.</span><span class="sxs-lookup"><span data-stu-id="fdae1-136">The SignalR Java Client will run on Android API Level 16 and later.</span></span>
* <span data-ttu-id="fdae1-137">La connexion via le service Azure SignalR nécessite le niveau d’API Android 20 et versions ultérieures, car le [service azure SignalR](/azure/azure-signalr/signalr-overview) requiert TLS 1,2 et ne prend pas en charge les suites de chiffrement SHA-1.</span><span class="sxs-lookup"><span data-stu-id="fdae1-137">Connecting through the Azure SignalR Service will require Android API Level 20 and later because the [Azure SignalR Service](/azure/azure-signalr/signalr-overview) requires TLS 1.2 and doesn't support SHA-1-based cipher suites.</span></span> <span data-ttu-id="fdae1-138">Android a [ajouté la prise en charge des suites de chiffrement SHA-256 (et versions ultérieures)](https://developer.android.com/reference/javax/net/ssl/SSLSocket) au niveau de l’API 20.</span><span class="sxs-lookup"><span data-stu-id="fdae1-138">Android [added support for SHA-256 (and above) cipher suites](https://developer.android.com/reference/javax/net/ssl/SSLSocket) in API Level 20.</span></span>

## <a name="configure-bearer-token-authentication"></a><span data-ttu-id="fdae1-139">Configurer l’authentification du jeton du porteur</span><span class="sxs-lookup"><span data-stu-id="fdae1-139">Configure bearer token authentication</span></span>

<span data-ttu-id="fdae1-140">Dans le client Java SignalR, vous pouvez configurer un jeton de porteur à utiliser pour l’authentification en fournissant une « fabrique de jetons d’accès » au [HttpHubConnectionBuilder](/java/api/com.microsoft.signalr._http_hub_connection_builder?view=aspnet-signalr-java).</span><span class="sxs-lookup"><span data-stu-id="fdae1-140">In the SignalR Java client, you can configure a bearer token to use for authentication by providing an "access token factory" to the [HttpHubConnectionBuilder](/java/api/com.microsoft.signalr._http_hub_connection_builder?view=aspnet-signalr-java).</span></span> <span data-ttu-id="fdae1-141">Utilisez [withAccessTokenFactory](/java/api/com.microsoft.signalr._http_hub_connection_builder.withaccesstokenprovider?view=aspnet-signalr-java#com_microsoft_signalr__http_hub_connection_builder_withAccessTokenProvider_Single_String__) pour fournir une [chaîne](https://github.com/ReactiveX/RxJava) [unique\<RxJava>](https://reactivex.io/documentation/single.html).</span><span class="sxs-lookup"><span data-stu-id="fdae1-141">Use [withAccessTokenFactory](/java/api/com.microsoft.signalr._http_hub_connection_builder.withaccesstokenprovider?view=aspnet-signalr-java#com_microsoft_signalr__http_hub_connection_builder_withAccessTokenProvider_Single_String__) to provide an [RxJava](https://github.com/ReactiveX/RxJava) [Single\<String>](https://reactivex.io/documentation/single.html).</span></span> <span data-ttu-id="fdae1-142">Avec un appel à [Single. Defer](https://reactivex.io/RxJava/javadoc/io/reactivex/Single.html#defer-java.util.concurrent.Callable-), vous pouvez écrire une logique pour produire des jetons d’accès pour votre client.</span><span class="sxs-lookup"><span data-stu-id="fdae1-142">With a call to [Single.defer](https://reactivex.io/RxJava/javadoc/io/reactivex/Single.html#defer-java.util.concurrent.Callable-), you can write logic to produce access tokens for your client.</span></span>

```java
HubConnection hubConnection = HubConnectionBuilder.create("YOUR HUB URL HERE")
    .withAccessTokenProvider(Single.defer(() -> {
        // Your logic here.
        return Single.just("An Access Token");
    })).build();
```

## <a name="known-limitations"></a><span data-ttu-id="fdae1-143">Limitations connues</span><span class="sxs-lookup"><span data-stu-id="fdae1-143">Known limitations</span></span>

::: moniker range=">= aspnetcore-3.0"

* <span data-ttu-id="fdae1-144">Seul le protocole JSON est pris en charge.</span><span class="sxs-lookup"><span data-stu-id="fdae1-144">Only the JSON protocol is supported.</span></span>
* <span data-ttu-id="fdae1-145">Le transport de secours et le transport des événements envoyés par le serveur ne sont pas pris en charge.</span><span class="sxs-lookup"><span data-stu-id="fdae1-145">Transport fallback and the Server Sent Events transport aren't supported.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

* <span data-ttu-id="fdae1-146">Seul le protocole JSON est pris en charge.</span><span class="sxs-lookup"><span data-stu-id="fdae1-146">Only the JSON protocol is supported.</span></span>
* <span data-ttu-id="fdae1-147">Seul le transport WebSocket est pris en charge.</span><span class="sxs-lookup"><span data-stu-id="fdae1-147">Only the WebSockets transport is supported.</span></span>
* <span data-ttu-id="fdae1-148">La diffusion en continu n’est pas encore prise en charge.</span><span class="sxs-lookup"><span data-stu-id="fdae1-148">Streaming isn't supported yet.</span></span>

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="fdae1-149">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="fdae1-149">Additional resources</span></span>

* [<span data-ttu-id="fdae1-150">Référence API Java</span><span class="sxs-lookup"><span data-stu-id="fdae1-150">Java API reference</span></span>](/java/api/com.microsoft.signalr?view=aspnet-signalr-java)
* <xref:signalr/hubs>
* <xref:signalr/javascript-client>
* <xref:signalr/publish-to-azure-web-app>
* <span data-ttu-id="fdae1-151">[Documentation sans serveur d’Azure SignalR service](/azure/azure-signalr/signalr-concept-serverless-development-config)</span><span class="sxs-lookup"><span data-stu-id="fdae1-151">[Azure SignalR Service serverless documentation](/azure/azure-signalr/signalr-concept-serverless-development-config)</span></span>
