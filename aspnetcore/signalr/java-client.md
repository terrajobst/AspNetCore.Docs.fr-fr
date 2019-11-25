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
# <a name="aspnet-core-opno-locsignalr-java-client"></a>ASP.NET Core SignalR client Java

Par [Mikael Mengistu](https://twitter.com/MikaelM_12)

Le client Java permet de se connecter à un serveur ASP.NET Core SignalR à partir de code Java, y compris les applications Android. À l’instar du client [JavaScript](xref:signalr/javascript-client) et du [client .net](xref:signalr/dotnet-client), le client Java vous permet de recevoir et d’envoyer des messages à un hub en temps réel. Le client Java est disponible dans ASP.NET Core 2,2 et versions ultérieures.

L’exemple d’application de console Java référencé dans cet article utilise le client SignalR Java.

[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/signalr/java-client/sample) ([procédure de téléchargement](xref:index#how-to-download-a-sample))

## <a name="install-the-opno-locsignalr-java-client-package"></a>Installer le package client Java SignalR

Le fichier jar *signalr-1.0.0* permet aux clients de se connecter à SignalR hubs. Pour rechercher le dernier numéro de version du fichier JAR, consultez les résultats de la [recherche Maven](https://search.maven.org/search?q=g:com.microsoft.signalr%20AND%20a:signalr).

Si vous utilisez Gradle, ajoutez la ligne suivante à la section `dependencies` de votre fichier *Build. Gradle* :

```gradle
implementation 'com.microsoft.signalr:signalr:1.0.0'
```

Si vous utilisez Maven, ajoutez les lignes suivantes dans l’élément `<dependencies>` de votre fichier *fichier POM. xml* :

[!code-xml[pom.xml dependency element](java-client/sample/pom.xml?name=snippet_dependencyElement)]

## <a name="connect-to-a-hub"></a>Se connecter à un hub

Pour établir un `HubConnection`, le `HubConnectionBuilder` doit être utilisé. L’URL du Hub et le niveau de journalisation peuvent être configurés lors de la création d’une connexion. Configurez toutes les options requises en appelant l’une des méthodes `HubConnectionBuilder` avant `build`. Démarrez la connexion avec `start`.

[!code-java[Build hub connection](java-client/sample/src/main/java/Chat.java?range=16-17)]

## <a name="call-hub-methods-from-client"></a>Appeler des méthodes de hub à partir du client

Un appel à `send` appelle une méthode de concentrateur. Passez le nom de la méthode de hub et de tous les arguments définis dans la méthode de hub à `send`.

[!code-java[send method](java-client/sample/src/main/java/Chat.java?range=28)]

> [!NOTE]
> Si vous utilisez le service Azure SignalR en *mode sans serveur*, vous ne pouvez pas appeler les méthodes de concentrateur à partir d’un client. Pour plus d’informations, consultez la [documentation du ServiceSignalR](/azure/azure-signalr/signalr-concept-serverless-development-config).

## <a name="call-client-methods-from-hub"></a>Appeler des méthodes clientes à partir du Hub

Utilisez `hubConnection.on` pour définir des méthodes sur le client que le hub peut appeler. Définissez les méthodes après la génération mais avant de commencer la connexion.

[!code-java[Define client methods](java-client/sample/src/main/java/Chat.java?range=19-21)]

## <a name="add-logging"></a>Ajouter la journalisation

Le client Java SignalR utilise la bibliothèque [SLF4J](https://www.slf4j.org/) pour la journalisation. Il s’agit d’une API de journalisation de haut niveau qui permet aux utilisateurs de la bibliothèque de choisir leur propre implémentation de journalisation spécifique en introduisant une dépendance de journalisation spécifique. L’extrait de code suivant montre comment utiliser `java.util.logging` avec le client Java SignalR.

```gradle
implementation 'org.slf4j:slf4j-jdk14:1.7.25'
```

Si vous ne configurez pas la journalisation dans vos dépendances, SLF4J charge un journal de non-opération par défaut avec le message d’avertissement suivant :

```
SLF4J: Failed to load class "org.slf4j.impl.StaticLoggerBinder".
SLF4J: Defaulting to no-operation (NOP) logger implementation
SLF4J: See http://www.slf4j.org/codes.html#StaticLoggerBinder for further details.
```

Cela peut être ignoré en toute sécurité.

## <a name="android-development-notes"></a>Remarques sur le développement Android

En ce qui concerne la Android SDK compatibilité pour les fonctionnalités du client SignalR, tenez compte des éléments suivants lorsque vous spécifiez votre version de Android SDK cible :

* Le client Java SignalR s’exécute sur l’API Android de niveau 16 et versions ultérieures.
* La connexion via le service Azure SignalR nécessite le niveau d’API Android 20 et versions ultérieures, car le [service azure SignalR](/azure/azure-signalr/signalr-overview) requiert TLS 1,2 et ne prend pas en charge les suites de chiffrement SHA-1. Android a [ajouté la prise en charge des suites de chiffrement SHA-256 (et versions ultérieures)](https://developer.android.com/reference/javax/net/ssl/SSLSocket) au niveau de l’API 20.

## <a name="configure-bearer-token-authentication"></a>Configurer l’authentification du jeton du porteur

Dans le client Java SignalR, vous pouvez configurer un jeton de porteur à utiliser pour l’authentification en fournissant une « fabrique de jetons d’accès » au [HttpHubConnectionBuilder](/java/api/com.microsoft.signalr._http_hub_connection_builder?view=aspnet-signalr-java). Utilisez [withAccessTokenFactory](/java/api/com.microsoft.signalr._http_hub_connection_builder.withaccesstokenprovider?view=aspnet-signalr-java#com_microsoft_signalr__http_hub_connection_builder_withAccessTokenProvider_Single_String__) pour fournir une [chaîne](https://github.com/ReactiveX/RxJava) [unique\<RxJava>](https://reactivex.io/documentation/single.html). Avec un appel à [Single. Defer](https://reactivex.io/RxJava/javadoc/io/reactivex/Single.html#defer-java.util.concurrent.Callable-), vous pouvez écrire une logique pour produire des jetons d’accès pour votre client.

```java
HubConnection hubConnection = HubConnectionBuilder.create("YOUR HUB URL HERE")
    .withAccessTokenProvider(Single.defer(() -> {
        // Your logic here.
        return Single.just("An Access Token");
    })).build();
```

## <a name="known-limitations"></a>Limitations connues

::: moniker range=">= aspnetcore-3.0"

* Seul le protocole JSON est pris en charge.
* Le transport de secours et le transport des événements envoyés par le serveur ne sont pas pris en charge.

::: moniker-end

::: moniker range="< aspnetcore-3.0"

* Seul le protocole JSON est pris en charge.
* Seul le transport WebSocket est pris en charge.
* La diffusion en continu n’est pas encore prise en charge.

::: moniker-end

## <a name="additional-resources"></a>Ressources supplémentaires

* [Référence API Java](/java/api/com.microsoft.signalr?view=aspnet-signalr-java)
* <xref:signalr/hubs>
* <xref:signalr/javascript-client>
* <xref:signalr/publish-to-azure-web-app>
* [Documentation sans serveur d’Azure SignalR service](/azure/azure-signalr/signalr-concept-serverless-development-config)
