---
title: Utiliser ASP.NET Core SignalR avec TypeScript et Webpack
author: ssougnez
description: Dans ce tutoriel, vous configurez Webpack pour regrouper et générer une application web ASP.NET Core SignalR dont le client est écrit en TypeScript.
monikerRange: '>= aspnetcore-2.1'
ms.author: scaddie
ms.custom: mvc
ms.date: 06/29/2018
uid: tutorials/signalr-typescript-webpack
ms.openlocfilehash: 03cb0c8ca2f6e4b48ebcbb4af0ed9c42c55f419a
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37808319"
---
# <a name="use-aspnet-core-signalr-with-typescript-and-webpack"></a>Utiliser ASP.NET Core SignalR avec TypeScript et Webpack

Par [Sébastien Sougnez](https://twitter.com/ssougnez) et [Scott Addie](https://twitter.com/Scott_Addie)

[Webpack](https://webpack.js.org/) permet aux développeurs de regrouper et générer les ressources côté client d’une application web. Ce tutoriel montre comment utiliser Webpack dans une application web ASP.NET Core SignalR dont le client est écrit en [TypeScript](https://www.typescriptlang.org/).

Dans ce didacticiel, vous apprendrez à :

> [!div class="checklist"]
> * Générer automatiquement un modèle d’application ASP.NET Core SignalR de démarrage
> * Configurer le client TypeScript SignalR
> * Configurer un pipeline de build à l’aide de Webpack
> * Configurer le serveur SignalR
> * Activer la communication entre le client et le serveur

[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/signalr-typescript-webpack/sample) ([procédure de téléchargement](xref:tutorials/index#how-to-download-a-sample))

## <a name="prerequisites"></a>Prérequis

Installez les logiciels suivants :

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* [Kit SDK .NET Core 2.1 ou version ultérieure](https://www.microsoft.com/net/download/all)
* [Node.js](https://nodejs.org/) avec [npm](https://www.npmjs.com/)
* [Visual Studio 2017](https://www.visualstudio.com/downloads/) version 15.7.3 ou ultérieure avec la charge de travail **ASP.NET et développement web**

# <a name="net-core-clitabnetcore-cli"></a>[CLI .NET Core](#tab/netcore-cli)

* [Kit SDK .NET Core 2.1 ou version ultérieure](https://www.microsoft.com/net/download/all)
* [Node.js](https://nodejs.org/) avec [npm](https://www.npmjs.com/)

---

## <a name="create-the-aspnet-core-web-app"></a>Créer l’application web ASP.NET Core

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Configurez Visual Studio pour rechercher npm dans la variable d'environnement *PATH*. Par défaut, Visual Studio utilise la version de npm qu’il trouve dans son répertoire d’installation. Suivez ces instructions dans Visual Studio :

1. Accédez à **Outils** > **Options** > **Projets et solutions** > **Gestion des packages web** > **Outils web externes**.
1. Sélectionnez l’entrée *$(PATH)* dans la liste. Cliquez sur la flèche vers le haut pour déplacer l’entrée à la deuxième position dans la liste. La première entrée référence les packages locaux du projet.

    ![Configuration de Visual Studio](signalr-typescript-webpack/_static/signalr-configure-path-visual-studio.png)

La configuration de Visual Studio est terminée. Nous allons maintenant créer le projet.

1. Utilisez l’option de menu **Fichier** > **Nouveau** > **Projet** et choisissez le modèle **Application web ASP.NET Core**.
1. Nommez le projet *SignalRWebPack*, puis cliquez sur le bouton **OK**.
1. Sélectionnez *.NET Core* dans la liste déroulante du framework cible, puis *ASP.NET Core 2.1* dans la liste déroulante du sélecteur de framework. Sélectionnez le modèle **vide**, puis cliquez sur le bouton **OK**.

# <a name="net-core-clitabnetcore-cli"></a>[CLI .NET Core](#tab/netcore-cli)

Exécutez la commande suivante dans le **Terminal intégré** :

```console
dotnet new web -o SignalRWebPack
```

Une application web ASP.NET Core vide, ciblant .NET Core, est créée dans un répertoire *SignalRWebPack*.

---

## <a name="configure-webpack-and-typescript"></a>Configurer Webpack et TypeScript

Les étapes suivantes configurent la conversion de TypeScript en JavaScript et le regroupement des ressources côté client.

1. Exécutez la commande suivante à la racine du projet pour créer un fichier *package.json* :

    ```console
    npm init -y
    ```

1. Ajoutez la propriété en surbrillance au fichier *package.json* :

    [!code-json[package.json](signalr-typescript-webpack/sample/snippets/package1.json?highlight=4)]

    La définition de la propriété `private` sur `true` empêche les avertissements d’installation de package à l’étape suivante.

1. Installez les packages npm nécessaires. Exécutez la commande suivante à la racine du projet :

    ```console
    npm install -D -E clean-webpack-plugin@0.1.19 css-loader@0.28.11 html-webpack-plugin@3.2.0 mini-css-extract-plugin@0.4.0 ts-loader@4.4.1 typescript@2.9.2 webpack@4.12.0 webpack-cli@3.0.6
    ```

    Détails de commande à prendre en compte :

    * Un numéro de version suit le signe `@` pour chaque nom de package. npm installe ces versions de package spécifiques.
    * L’option `-E` désactive le comportement par défaut de npm consistant à écrire des opérateurs de plage de [gestion sémantique des versions](https://semver.org/) dans *package.json*. Par exemple, `"webpack": "4.12.0"` peut être utilisé à la place de `"webpack": "^4.12.0"`. Cette option empêche les mises à niveau involontaires vers des versions de package plus récentes.

    Consultez la documentation officielle de [npm-install](https://docs.npmjs.com/cli/install) pour plus de détails.

1. Remplacez la propriété `scripts` du fichier *package.json* par l’extrait de code suivant :

    ```json
    "scripts": {
      "build": "webpack --mode=development --watch",
      "release": "webpack --mode=production",
      "publish": "npm run release && dotnet publish -c Release"
    },
    ```

    Explication des scripts :

    * `build` : Regroupe vos ressources côté client en mode de développement et surveille les changements de fichier. L’observateur de fichiers force la regénération du regroupement chaque fois qu’un fichier projet change. L’option `mode` désactive les optimisations de production, comme la minimisation de l’arborescence (tree shaking). Utilisez uniquement `build` dans le développement.
    * `release` : Regroupe vos ressources côté client en mode de production.
    * `publish` : Exécute le script `release` pour regrouper les ressources côté client en mode de production. La commande appelle la commande [publish](/dotnet/core/tools/dotnet-publish) de CLI .NET Core pour publier l’application.

1. Créez un fichier nommé *webpack.config.js* à la racine du projet avec le contenu suivant :

    [!code-javascript[webpack.config.js](signalr-typescript-webpack/sample/webpack.config.js)]

    Le fichier précédent configure la compilation Webpack. Détails de configuration à prendre en compte :

    * La propriété `output` remplace la valeur par défaut de *dist*. Le regroupement est émis dans le répertoire *wwwroot* à la place.
    * Le tableau `resolve.extensions` inclut *.js* pour importer le client JavaScript SignalR.

1. Créez un répertoire *src* à la racine du projet. Son objectif est de stocker les ressources côté client du projet.

1. Créez *src/index.html* avec le contenu suivant.

    [!code-html[index.html](signalr-typescript-webpack/sample/src/index.html)]

    Le code HTML précédent définit le balisage réutilisable de la page d’accueil.

1. Créez un répertoire *src/css*. Son objectif est de stocker les fichiers *.css* du projet.

1. Créez *src/css/main.css* avec le contenu suivant :

    [!code-css[main.css](signalr-typescript-webpack/sample/src/css/main.css)]

    Le fichier *main.css* précédent définit le style de l’application.

1. Créez *src/tsconfig.json* avec le contenu suivant :

    [!code-json[tsconfig.json](signalr-typescript-webpack/sample/src/tsconfig.json)]

    Le code précédent configure le compilateur TypeScript pour produire du code JavaScript compatible [ECMAScript](https://wikipedia.org/wiki/ECMAScript) 5.

1. Créez *src/index.ts* avec le contenu suivant :

    [!code-typescript[index.ts](signalr-typescript-webpack/sample/snippets/index1.ts?name=snippet_IndexTsPhase1File)]

    Le code TypeScript précédent récupère les références aux éléments DOM et joint deux gestionnaires d’événements :

    * `keyup` : Cet événement se déclenche quand l’utilisateur tape des données dans la zone de texte identifiée comme `tbMessage`. La fonction `send` est appelée quand l’utilisateur appuie sur la touche **Entrée**.
    * `click` : Cet événement se déclenche quand l’utilisateur clique sur le bouton **Envoyer**. La fonction `send` est appelée.

## <a name="configure-the-aspnet-core-app"></a>Configurer l’application ASP.NET Core

1. Le code fourni dans la méthode `Startup.Configure` affiche *Hello World!*. Remplacez l’appel de méthode `app.Run` par des appels à [UseDefaultFiles](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) et [UseStaticFiles](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_).

    [!code-csharp[Startup](signalr-typescript-webpack/sample/Startup.cs?name=snippet_UseStaticDefaultFiles)]

    Le code précédent permet au serveur de localiser et traiter le fichier *index.html*, que l’utilisateur entre son URL complète ou l’URL racine de l’application web.

1. Appelez [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr#Microsoft_Extensions_DependencyInjection_SignalRDependencyInjectionExtensions_AddSignalR_Microsoft_Extensions_DependencyInjection_IServiceCollection_) dans la méthode `Startup.ConfigureServices`. Il ajoute les services SignalR à votre projet.

    [!code-csharp[Startup](signalr-typescript-webpack/sample/Startup.cs?name=snippet_AddSignalR)]

1. Mappez une route */hub* au hub `ChatHub`. Ajoutez les lignes suivantes à la fin de la méthode `Startup.Configure` :

    [!code-csharp[Startup](signalr-typescript-webpack/sample/Startup.cs?name=snippet_UseSignalR)]

1. Créez un répertoire *Hubs* à la racine du projet. Son objectif est de stocker le hub SignalR, qui est créé à l’étape suivante.

1. Créez un hub *Hubs/ChatHub.cs* avec le code suivant :

    [!code-csharp[ChatHub](signalr-typescript-webpack/sample/snippets/ChatHub.cs?name=snippet_ChatHubStubClass)]

1. Ajoutez le code suivant en haut du fichier *Startup.cs* pour résoudre la référence de `ChatHub` :

    [!code-csharp[Startup](signalr-typescript-webpack/sample/Startup.cs?name=snippet_HubsNamespace)]

## <a name="enable-client-and-server-communication"></a>Activer la communication entre le client et le serveur

Actuellement, l’application affiche un formulaire simple pour envoyer des messages. Rien ne se produit quand vous essayez de le faire. Le serveur écoute une route spécifique, mais ne fait rien avec les messages envoyés.

1. Exécutez la commande suivante à la racine du projet :

    ```console
    npm install @aspnet/signalr
    ```

    La commande précédente installe le [client SignalR TypeScript](https://www.npmjs.com/package/@aspnet/signalr), ce qui permet au client d’envoyer des messages au serveur.

1. Ajoutez le code en surbrillance au fichier *src/index.ts* :

    [!code-typescript[index.ts](signalr-typescript-webpack/sample/snippets/index2.ts?name=snippet_IndexTsPhase2File&highlight=2,9-23)]

    Le code précédent prend en charge la réception de messages du serveur. La classe `HubConnectionBuilder` crée un générateur pour configurer la connexion de serveur. La fonction `withUrl` configure l’URL du hub.

    SignalR permet l’échange de messages entre un client et un serveur. Chaque message a un nom spécifique. Par exemple, vous pouvez avoir des messages avec le nom `messageReceived` qui exécutent la logique chargée d’afficher le nouveau message dans la zone de messages. L’écoute d’un message spécifique peut être effectuée au moyen de la fonction `on`. Vous pouvez écouter n’importe quel nombre de noms de message. Vous pouvez aussi passer des paramètres au message, comme le nom de l’auteur et le contenu du message reçu. Dès que le client reçoit un message, un élément `div` est créé avec le nom de l’auteur et le contenu du message dans son attribut `innerHTML`. Il est ajouté à l’élément `div` principal qui affiche les messages.

1. Maintenant que le client peut recevoir un message, configurez-le pour en envoyer. Ajoutez le code en surbrillance au fichier *src/index.ts* :

    [!code-typescript[index.ts](signalr-typescript-webpack/sample/src/index.ts?highlight=34-35)]

    L’envoi d’un message au moyen de la connexion WebSocket nécessite l’appel de la méthode `send`. Le premier paramètre de la méthode est le nom du message. Les données du message se trouvent dans les autres paramètres. Dans cet exemple, un message identifié comme `newMessage` est envoyé au serveur. Le message se compose du nom d’utilisateur et de l’entrée de l’utilisateur dans une zone de texte. Si l’envoi fonctionne, la valeur de la zone de texte est effacée.

1. Ajoutez la méthode en surbrillance à la classe `ChatHub` :

    [!code-csharp[ChatHub](signalr-typescript-webpack/sample/Hubs/ChatHub.cs?highlight=8-11)]

    Le code précédent diffuse les messages reçus à tous les utilisateurs connectés, une fois que le serveur les reçoit. Vous n’avez pas besoin d’appeler une méthode générique `on` pour recevoir tous les messages. Il vous suffit d’avoir une méthode du même nom que le message.

    Dans cet exemple, le client TypeScript envoie un message identifié comme `newMessage`. La méthode C# `NewMessage` attend les données envoyées par le client. Un appel est effectué à la méthode [SendAsync](/dotnet/api/microsoft.aspnetcore.signalr.clientproxyextensions.sendasync) sur [Clients.All](/dotnet/api/microsoft.aspnetcore.signalr.ihubclients-1.all). Les messages reçus sont envoyés à tous les clients connectés au hub.

## <a name="test-the-app"></a>Tester l’application

Vérifiez que l’application fonctionne avec les étapes suivantes.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

1. Exécuter Webpack en mode de *mise en production*. Dans la fenêtre **Console du Gestionnaire de package**, exécutez la commande suivante à la racine du projet :

    [!INCLUDE [npm-run-release](../includes/signalr-typescript-webpack/npm-run-release.md)]

1. Sélectionnez **Déboguer** > **Démarrer sans débogage** pour lancer l’application dans un navigateur sans attacher le débogueur. Le fichier *wwwroot/index.html* est délivré sous `http://localhost:<port_number>`.

1. Ouvrez une autre instance de navigateur (n’importe quel navigateur). Copiez l’URL de la barre d’adresse.

1. Choisissez un navigateur, tapez quelque chose dans la zone de texte **Message**, puis cliquez sur le bouton **Envoyer**. Le nom unique de l’utilisateur et le message sont affichés instantanément dans les deux pages.

# <a name="net-core-clitabnetcore-cli"></a>[CLI .NET Core](#tab/netcore-cli)

1. Exécutez Webpack en mode de *mise en production* en exécutant la commande suivante à la racine du projet :

    [!INCLUDE [npm-run-release](../includes/signalr-typescript-webpack/npm-run-release.md)]

1. Générez et exécutez l’application en exécutant la commande suivante à la racine du projet :

    ```console
    dotnet run
    ```

    Le serveur web démarre l’application et la rend disponible sur localhost.

1. Ouvrez un navigateur avec l’adresse `http://localhost:<port_number>`. Le fichier *wwwroot/index.html* est présent. Copiez l’URL à partir de la barre d’adresse.

1. Ouvrez une autre instance de navigateur (n’importe quel navigateur). Copiez l’URL de la barre d’adresse.

1. Choisissez un navigateur, tapez quelque chose dans la zone de texte **Message**, puis cliquez sur le bouton **Envoyer**. Le nom unique de l’utilisateur et le message sont affichés instantanément dans les deux pages.

---

![message affiché dans les deux fenêtres de navigateur](signalr-typescript-webpack/_static/browsers-message-broadcast.png)

## <a name="additional-resources"></a>Ressources supplémentaires

* <xref:signalr/javascript-client>
* <xref:signalr/hubs>