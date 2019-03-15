---
title: Client ASP.NET Core SignalR Java
author: mikaelm12
description: Découvrez comment utiliser le client d’ASP.NET Core SignalR Java.
monikerRange: '>= aspnetcore-2.2'
ms.author: mimengis
ms.custom: mvc
ms.date: 03/14/2019
uid: signalr/java-client
ms.openlocfilehash: 09e5ce23ddcc250d212a8cdf1176f39531a9c0ba
ms.sourcegitcommit: d913bca90373c07f89b1d1df01af5fc01fc908ef
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/14/2019
ms.locfileid: "57978488"
---
# <a name="aspnet-core-signalr-java-client"></a>Client ASP.NET Core SignalR Java

Par [Mikael Mengistu](https://twitter.com/MikaelM_12)

Le client Java permet la connexion à un serveur d’ASP.NET Core SignalR à partir de code Java, y compris les applications Android. Comme le [client JavaScript](xref:signalr/javascript-client) et [client .NET](xref:signalr/dotnet-client), le client Java vous permet de recevoir et envoyer des messages à un hub en temps réel. Le client Java est disponible dans ASP.NET Core 2.2 et versions ultérieures.

L’exemple d’application de console Java référencé dans cet article utilise le client SignalR Java.

[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/java-client/sample) ([procédure de téléchargement](xref:index#how-to-download-a-sample))

## <a name="install-the-signalr-java-client-package"></a>Installer le package du client Java de SignalR

Le *signalr-1.0.0* fichier JAR permet aux clients pour se connecter à des concentrateurs SignalR. Pour rechercher le dernier numéro de version de fichier JAR, consultez le [résultats de la recherche Maven](https://search.maven.org/search?q=g:com.microsoft.signalr%20AND%20a:signalr).

Si vous utilisez Gradle, ajoutez la ligne suivante à la `dependencies` section de votre *build.gradle* fichier :

```gradle
implementation 'com.microsoft.signalr:signalr:1.0.0'
```

Si à l’aide de Maven, ajoutez les lignes suivantes à l’intérieur de la `<dependencies>` élément de votre *pom.xml* fichier :

[!code-xml[pom.xml dependency element](java-client/sample/pom.xml?name=snippet_dependencyElement)]

## <a name="connect-to-a-hub"></a>Se connecter à un hub

Pour établir une `HubConnection`, la `HubConnectionBuilder` doit être utilisé. Le niveau d’URL et le journal de hub peut être configuré lors de la création d’une connexion. Configurez les options requises en appelant une de le `HubConnectionBuilder` méthodes avant `build`. Démarrez la connexion avec `start`.

[!code-java[Build hub connection](java-client/sample/src/main/java/Chat.java?range=16-17)]

## <a name="call-hub-methods-from-client"></a>Appeler des méthodes de hub à partir du client

Un appel à `send` appelle une méthode de concentrateur. Passez le nom de la méthode de hub et de tous les arguments définis dans la méthode de hub à `send`.

[!code-java[send method](java-client/sample/src/main/java/Chat.java?range=28)]

> [!NOTE]
> Si vous utilisez le Service Azure SignalR dans *mode sans serveur*, vous ne pouvez pas appeler des méthodes de concentrateur à partir d’un client. Pour plus d’informations, consultez le [documentation de SignalR Service](/azure/azure-signalr/signalr-concept-serverless-development-config).

## <a name="call-client-methods-from-hub"></a>Appeler des méthodes de client à partir de hub

Utilisez `hubConnection.on` pour définir des méthodes sur le client que le hub peut appeler. Définissez les méthodes après la génération, mais avant de démarrer la connexion.

[!code-java[Define client methods](java-client/sample/src/main/java/Chat.java?range=19-21)]

## <a name="add-logging"></a>Ajouter la journalisation

Le client SignalR Java utilise le [SLF4J](https://www.slf4j.org/) bibliothèque pour la journalisation. C’est une API de haut niveau de journalisation qui permet aux utilisateurs de la bibliothèque de choisir leur propre implémentation de journalisation spécifiques en apportant une dépendance de journalisation spécifique. L’extrait de code suivant montre comment utiliser `java.util.logging` avec le client SignalR Java.

```gradle
implementation 'org.slf4j:slf4j-jdk14:1.7.25'
```

Si vous ne configurez pas la journalisation dans vos dépendances, SLF4J charge un enregistreur d’événements non opération par défaut avec le message d’avertissement suivant :

```
SLF4J: Failed to load class "org.slf4j.impl.StaticLoggerBinder".
SLF4J: Defaulting to no-operation (NOP) logger implementation
SLF4J: See http://www.slf4j.org/codes.html#StaticLoggerBinder for further details.
```

Cela peut être ignoré sans risque.

## <a name="android-development-notes"></a>Notes de développement Android

En ce qui concerne la compatibilité du Kit Android SDK pour les fonctionnalités de client SignalR, tenez compte des éléments suivants lorsque vous spécifiez votre version de SDK Android cible :

* Le Client Java SignalR exécutera sur Android API niveau 16 et versions ultérieures.
* Connexion via le Service Azure SignalR nécessitera le niveau d’API Android 20 et versions ultérieur, car le [Service Azure SignalR](/azure/azure-signalr/signalr-overview) nécessite TLS 1.2 et ne prend pas en charge les suites de chiffrement basé sur SHA-1. Android [ajouté des suites de chiffrement prise en charge pour l’algorithme SHA-256 (et versions ultérieures)](https://developer.android.com/reference/javax/net/ssl/SSLSocket) dans le niveau d’API 20.

## <a name="configure-bearer-token-authentication"></a>Configurer l’authentification du jeton du porteur

Dans le client SignalR Java, vous pouvez configurer un jeton du porteur à utiliser pour l’authentification en fournissant une « fabrique jeton accès » à la [HttpHubConnectionBuilder](/java/api/com.microsoft.signalr._http_hub_connection_builder?view=aspnet-signalr-java). Utilisez [withAccessTokenFactory](/java/api/com.microsoft.signalr._http_hub_connection_builder.withaccesstokenprovider?view=aspnet-signalr-java#com_microsoft_signalr__http_hub_connection_builder_withAccessTokenProvider_Single_String__) pour fournir un [RxJava](https://github.com/ReactiveX/RxJava) [unique<String>](http://reactivex.io/documentation/single.html). Avec un appel à [Single.defer](http://reactivex.io/RxJava/javadoc/io/reactivex/Single.html#defer-java.util.concurrent.Callable-), vous pouvez écrire la logique pour générer des jetons d’accès pour votre client.

```java
HubConnection hubConnection = HubConnectionBuilder.create("YOUR HUB URL HERE")
    .withAccessTokenProvider(Single.defer(() -> {
        // Your logic here.
        return Single.just("An Access Token");
    })).build();
```

## <a name="known-limitations"></a>Limitations connues

* Seul le protocole JSON est pris en charge.
* Uniquement le transport WebSocket est pris en charge.
* Diffusion en continu n’est pas encore pris en charge.

## <a name="additional-resources"></a>Ressources supplémentaires

* [Référence API Java](/java/api/com.microsoft.signalr?view=aspnet-signalr-java)
* <xref:signalr/hubs>
* <xref:signalr/javascript-client>
* <xref:signalr/publish-to-azure-web-app>
* [Documentation de serverless Azure SignalR Service](/azure/azure-signalr/signalr-concept-serverless-development-config)
