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
# <a name="aspnet-core-signalr-java-client"></a>ASP.NET Core SignalR Java Client

Par [Mikael Mengistu](https://twitter.com/MikaelM_12)

Le client Java permet la connexion à un serveur d’ASP.NET Core SignalR à partir de code Java, y compris les applications Android. Comme le [client JavaScript](xref:signalr/javascript-client) et [client .NET](xref:signalr/dotnet-client), le client Java vous permet de recevoir et envoyer des messages à un hub en temps réel. Le client Java est disponible dans ASP.NET Core 2.2 et versions ultérieures.

L’exemple d’application de console Java référencé dans cet article utilise le client SignalR Java.

[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/java-client/sample) ([procédure de téléchargement](xref:tutorials/index#how-to-download-a-sample))

## <a name="install-the-signalr-java-client-package"></a>Installer le package du client Java de SignalR

Le *signalr-0.1.0-preview2-35174* fichier JAR permet aux clients pour se connecter à des concentrateurs SignalR. Pour rechercher le dernier numéro de version de fichier JAR, consultez le [résultats de la recherche Maven](https://search.maven.org/search?q=g:com.microsoft.aspnet%20AND%20a:signalr&core=gav).

Si vous utilisez Gradle, ajoutez la ligne suivante à la `dependencies` section de votre *build.gradle* fichier :

```gradle
implementation 'com.microsoft.aspnet:signalr:0.1.0-preview2-35174'
```

Si à l’aide de Maven, ajoutez les lignes suivantes à l’intérieur de la `<dependencies>` élément de votre *pom.xml* fichier :

[!code-xml[pom.xml dependency element](java-client/sample/pom.xml?name=snippet_dependencyElement)]

## <a name="connect-to-a-hub"></a>Se connecter à un concentrateur

Pour établir une `HubConnection`, la `HubConnectionBuilder` doit être utilisé. Le niveau d’URL et le journal de hub peut être configuré lors de la création d’une connexion. Configurez les options requises en appelant une de le `HubConnectionBuilder` méthodes avant `build`. Démarrez la connexion avec `start`.

[!code-java[Build hub connection](java-client/sample/src/main/java/Chat.java?range=17-20)]

## <a name="call-hub-methods-from-client"></a>Appeler des méthodes de hub à partir du client

Un appel à `send` appelle une méthode de concentrateur. Passez le nom de la méthode de hub et de tous les arguments définis dans la méthode de hub à `send`.

[!code-java[send method](java-client/sample/src/main/java/Chat.java?range=31)]

## <a name="call-client-methods-from-hub"></a>Appeler des méthodes de client à partir de hub

Utilisez `hubConnection.on` pour définir des méthodes sur le client que le hub peut appeler. Définissez les méthodes après la génération, mais avant de démarrer la connexion.

[!code-java[Define client methods](java-client/sample/src/main/java/Chat.java?range=22-24)]

## <a name="known-limitations"></a>Limitations connues

Il s’agit d’une version préliminaire du client Java. Il existe de nombreuses fonctionnalités qui ne sont pas encore pris en charge. Les intervalles suivants sont en cours de traitement pour les versions futures :

* Seuls les types primitifs peuvent être acceptés en tant que paramètres et types de retour.
* Les API sont synchrones.
* Seul le type d’appel « Envoyer » est pris en charge pour l’instant. « Appeler » et la diffusion en continu des valeurs de retour ne sont pas pris en charge.
* Seul le protocole JSON est pris en charge.
* Uniquement le transport WebSocket est pris en charge.

## <a name="additional-resources"></a>Ressources supplémentaires

* [Référence de l’API Java](/java/api/com.microsoft.aspnet.signalr?view=aspnet-signalr-java)
* <xref:signalr/hubs>
* <xref:signalr/javascript-client>
* <xref:signalr/publish-to-azure-web-app>
