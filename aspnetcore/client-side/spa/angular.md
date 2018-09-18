---
title: Utiliser le modèle de projet Angular avec ASP.NET Core
author: SteveSandersonMS
description: Découvrez comment démarrer avec le modèle de projet d’application à page unique ASP.NET Core pour Angular et la CLI Angular.
monikerRange: '>= aspnetcore-2.0'
ms.author: scaddie
ms.custom: mvc
ms.date: 02/21/2018
uid: spa/angular
ms.openlocfilehash: 763b4fff7c64432328af660c66e6ff3f8f697f71
ms.sourcegitcommit: b2723654af4969a24545f09ebe32004cb5e84a96
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/18/2018
ms.locfileid: "46011352"
---
# <a name="use-the-angular-project-template-with-aspnet-core"></a>Utiliser le modèle de projet Angular avec ASP.NET Core

::: moniker range="= aspnetcore-2.0"

> [!NOTE]
> Cette documentation ne concerne pas le modèle de projet Angular inclus dans ASP.NET Core 2.0. Elle traite du nouveau modèle Angular que vous pouvez mettre à jour manuellement. Par défaut, le modèle est inclus dans ASP.NET Core 2.1.

::: moniker-end

Le modèle de projet Angular mis à jour fournit un point de départ pratique pour les applications ASP.NET Core utilisant Angular et la CLI Angular pour implémenter une interface utilisateur (IU) côté client enrichie.

Le modèle est équivalent à la création d’un projet ASP.NET Core en tant que back-end d’API et d’un projet CLI Angular en tant qu’interface utilisateur. Le modèle offre l’avantage d’héberger les deux types de projets dans un projet d’application unique. Par conséquent, le projet d’application peut être généré et publié sous la forme d’une unité unique.

## <a name="create-a-new-app"></a>Créer une application

::: moniker range="= aspnetcore-2.0"

Si vous utilisez ASP.NET Core 2.0, vérifiez que vous avez [installé le modèle de projet React mis à jour](xref:spa/index#installation).

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

Si ASP.NET Core 2.1 est installé, il est inutile d’installer le modèle de projet Angular.

::: moniker-end

Créez un projet à partir d’une invite de commandes à l’aide de la commande `dotnet new angular` dans un répertoire vide. Par exemple, les commandes suivantes créent l’application dans un répertoire *my-new-app* et basculent vers ce répertoire :

```console
dotnet new angular -o my-new-app
cd my-new-app
```

Exécutez l’application à partir de Visual Studio ou de CLI .NET Core :

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio/)

Ouvrez le fichier *.csproj* généré, puis exécutez l’application normalement à partir de là.

Le processus de génération restaure les dépendances npm à la première exécution, ce qui peut prendre plusieurs minutes. Les générations suivantes sont beaucoup plus rapides.

# <a name="net-core-clitabnetcore-cli"></a>[CLI .NET Core](#tab/netcore-cli/)

Vérifiez que vous avez une variable d’environnement `ASPNETCORE_Environment` ayant pour valeur `Development`. Sur Windows (dans les invites non-PowerShell), exécutez `SET ASPNETCORE_Environment=Development`. Sur Linux ou macOS, exécutez `export ASPNETCORE_Environment=Development`.

Exécutez [dotnet build](/dotnet/core/tools/dotnet-build) pour vérifier que l’application se génère correctement. À la première exécution, le processus de génération restaure les dépendances npm, ce qui peut prendre plusieurs minutes. Les générations suivantes sont beaucoup plus rapides.

Exécutez [dotnet run](/dotnet/core/tools/dotnet-run) pour démarrer l’application. Un message semblable au message suivant est journalisé :

```console
Now listening on: http://localhost:<port>
```

Accédez à cette URL dans un navigateur.

L’application démarre une instance du serveur CLI Angular en arrière-plan. Un message semblable au message suivant est journalisé : *NG Live Development Server est à l’écoute sur localhost :&lt;otherport&gt;, ouvrez votre navigateur sur http://localhost:&lt;otherport&gt;/*. Ignorez ce message&mdash;ce n’est **pas** l’URL de l’application ASP.NET Core et CLI Angular combinée.

---

Le modèle de projet crée une application ASP.NET Core et une application Angular. L’application ASP.NET Core est destinée à être utilisée pour tous les aspects liés au serveur, tels que l’accès aux données et l’autorisation. L’application Angular, qui réside dans le sous-répertoire *ClientApp*, est destinée à être utilisée pour tout ce qui touche l’interface utilisateur.

## <a name="add-pages-images-styles-modules-etc"></a>Ajouter des pages, des images, des styles, des modules, etc.

Le répertoire *ClientApp* contient une application CLI Angular standard. Pour plus d’informations, consultez la [documentation Angular](https://github.com/angular/angular-cli/wiki) officielle.

Il existe de légères différences entre l’application Angular créée par ce modèle et celle créée par la CLI Angular (via `ng new`) ; toutefois, les fonctionnalités de l’application sont identiques. L’application créée par le modèle contient une mise en page basée sur [Bootstrap](https://getbootstrap.com/) et un exemple de routage de base.

## <a name="run-ng-commands"></a>Exécuter des commandes ng

Dans une invite de commandes, basculez vers le sous-répertoire *ClientApp* :

```console
cd ClientApp
```

Si l’outil `ng` est installé de manière globale, vous pouvez exécuter n’importe laquelle de ses commandes. Par exemple, vous pouvez exécuter `ng lint`, `ng test` ou toute autre [commande CLI Angular](https://github.com/angular/angular-cli/wiki#additional-commands). Mais il est inutile d’exécuter `ng serve`, car votre application ASP.NET Core se charge de traiter les parties côté serveur et côté client de votre application. En interne, elle utilise `ng serve` dans le développement.

Si l’outil `ng` n’est pas installé, exécutez `npm run ng` à la place. Par exemple, vous pouvez exécuter `npm run ng lint` ou `npm run ng test`.

## <a name="install-npm-packages"></a>Installer des packages npm

Pour installer des packages npm tiers, utilisez une invite de commandes dans le sous-répertoire *ClientApp*. Exemple :

```console
cd ClientApp
npm install --save <package_name>
```

## <a name="publish-and-deploy"></a>Publier et déployer

Pendant le développement, l’application s’exécute en mode optimisé pour des raisons pratiques. Par exemple, les bundles JavaScript incluent des mappages de sources (ce qui vous permet de voir votre code TypeScript d’origine pendant le débogage). L’application se recompile et se recharge automatiquement en cas de modification des fichiers TypeScript, HTML et CSS sur le disque.

Dans un environnement de production, fournissez une version de votre application qui est optimisée pour les performances. Ce comportement est configuré pour se produire automatiquement. Quand vous publiez, la configuration de build émet une build compilée AoT (Ahead-of-Time) réduite de votre code côté client. Contrairement à la build de développement, la build de production ne requiert pas que Node.js soit installé sur le serveur (sauf si vous avez activé le [préaffichage côté serveur](#server-side-rendering)).

Vous pouvez utiliser des [méthodes d’hébergement et de déploiement ASP.NET Core](xref:host-and-deploy/index) standard.

## <a name="run-ng-serve-independently"></a>Exécuter « ng serve » indépendamment

Le projet est configuré pour démarrer sa propre instance du serveur CLI Angular en arrière-plan quand l’application ASP.NET Core démarre en mode de développement. Ainsi, vous n’êtes pas obligé d’exécuter un serveur distinct manuellement.

Cette configuration par défaut présente un inconvénient. Chaque fois que vous modifiez votre code C# et que votre application ASP.NET Core doit redémarrer, le serveur CLI Angular redémarre. Environ 10 secondes sont requises pour démarrer la sauvegarde. Si vous apportez des modifications fréquentes au code C# et que vous ne souhaitez pas attendre que la CLI Angular redémarre, exécutez le serveur CLI Angular en externe, indépendamment du processus ASP.NET Core. Pour ce faire :

1. Dans une invite de commandes, basculez vers le sous-répertoire *ClientApp* et lancez le serveur de développement CLI Angular :

    ```console
    cd ClientApp
    npm start
    ```

    > [!IMPORTANT]
    > Utilisez `npm start` pour lancer le serveur de développement CLI Angular, et non `ng serve`, de sorte que la configuration dans *package.json* soit respectée. Pour transmettre des paramètres supplémentaires au serveur CLI Angular, ajoutez-les à la ligne `scripts` pertinente dans votre fichier *package.json*.

2. Modifiez votre application ASP.NET Core afin qu’elle utilise l’instance CLI Angular externe au lieu de lancer une instance propre. Dans votre classe *Startup*, remplacez l’appel `spa.UseAngularCliServer` par ce qui suit :

    ```csharp
    spa.UseProxyToSpaDevelopmentServer("http://localhost:4200");
    ```

Quand vous démarrez votre application ASP.NET Core, elle ne lance pas un serveur CLI Angular. L’instance que vous avez démarrée manuellement est utilisée à la place. Cela lui permet de démarrer et de redémarrer plus rapidement. Elle n’attend plus que votre CLI Angular regénère votre application cliente à chaque fois.

## <a name="server-side-rendering"></a>Rendu côté serveur

En tant que fonctionnalité de performances, vous pouvez choisir de préafficher votre application Angular sur le serveur et de l’exécuter sur le client. Cela signifie que les navigateurs reçoivent le balisage HTML qui représente l’interface utilisateur initiale de votre application, qu’ils affichent avant même de télécharger et d’exécuter vos bundles JavaScript. La plupart de cette implémentation provient d’une fonctionnalité Angular appelée [Angular Universal](https://universal.angular.io/).

> [!TIP]
> L’activation du rendu côté serveur (SSR) présente un nombre de complications supplémentaires à la fois au cours du développement et du déploiement. Lisez les [inconvénients du SSR](#drawbacks-of-ssr) pour déterminer si le SSR est adapté à vos besoins.

Pour activer le SSR, vous devez effectuer un certain nombre d’ajouts à votre projet.

Dans la classe *Démarrer*, *après* la ligne qui configure `spa.Options.SourcePath` et *avant* l’appel à `UseAngularCliServer` ou `UseProxyToSpaDevelopmentServer`, ajoutez ce qui suit :

[!code-csharp[](sample/AngularServerSideRendering/Startup.cs?name=snippet_Call_UseSpa&highlight=5-12)]

En mode de développement, ce code tente de générer le bundle SSR en exécutant le script `build:ssr`, qui est défini dans *ClientApp\package.json*. Cette opération génère une application Angular nommée `ssr`, qui n’est pas encore définie.

À la fin du tableau `apps` dans *ClientApp/.angular-cli.json*, définissez une application supplémentaire nommée `ssr`. Utilisez les options suivantes :

[!code-json[](sample/AngularServerSideRendering/ClientApp/.angular-cli.json?range=24-41)]

Cette nouvelle configuration d’application SSR requiert deux fichiers supplémentaires : *tsconfig.server.json* et *main.server.ts*. Le fichier *tsconfig.server.json* spécifie les options de compilation TypeScript. Le fichier *main.server.ts* sert de point d’entrée du code pendant le rendu côté serveur.

Ajoutez un nouveau fichier appelé *tsconfig.server.json* à l’intérieur de *ClientApp/src* (avec le fichier *tsconfig.app.json* existant), qui contient les éléments suivants :

[!code-json[](sample/AngularServerSideRendering/ClientApp/src/tsconfig.server.json)]

Ce fichier configure le compilateur AoT d’Angular pour rechercher un module appelé `app.server.module`. Ajoutez ceci en créant un nouveau fichier dans *ClientApp/src/app/app.server.module.ts* (avec l’élément *app.module.ts* existant) qui contient les éléments suivants :

[!code-typescript[](sample/AngularServerSideRendering/ClientApp/src/app/app.server.module.ts)]

Ce module hérite de votre `app.module` côté client et définit quels modules Angular supplémentaires sont disponibles au cours du SSR.

N’oubliez pas que la nouvelle entrée `ssr` dans *.angular-cli.json* faisait référence à un fichier de point d’entrée appelé *main.server.ts*. Vous n’avez pas encore ajouté ce fichier et il est maintenant temps de le faire. Créez un nouveau fichier dans *ClientApp/src/main.server.ts* (avec l’élément *main.ts* existant), qui contient les éléments suivants :

[!code-typescript[](sample/AngularServerSideRendering/ClientApp/src/main.server.ts)]

ASP.NET Core exécute le code de ce fichier pour chaque requête lorsqu’il exécute l’intergiciel `UseSpaPrerendering` (middleware) que vous avez ajouté à la classe *Démarrer*. Il gère la réception de `params` à partir du code .NET (par exemple, l’URL demandée) et les appels aux API SSR Angular pour obtenir le code HTML résultant.

À proprement parler, cela suffit pour activer SSR en mode de développement. Il est primordial d’effectuer une modification finale afin que votre application fonctionne correctement lors de sa publication. Dans le fichier *.csproj* principal de votre application, définissez la propriété  `BuildServerSideRenderer` sur `true` :

[!code-xml[](sample/AngularServerSideRendering/AngularServerSideRendering.csproj?name=snippet_EnableBuildServerSideRenderer)]

Cela configure le processus de génération pour exécuter `build:ssr` lors de la publication et déployer les fichiers SSR sur le serveur. Si vous n’activez pas cette option, SSR échoue en production.

Lorsque votre application s’exécute en mode de développement ou de production, le code Angular est préaffiché au format HTML sur le serveur. Le code côté client s’exécute normalement.

### <a name="pass-data-from-net-code-into-typescript-code"></a>Transmettre des données à partir du code .NET dans le code TypeScript

Au cours du rendu côté serveur (SSR), vous pouvez souhaiter transmettre des données par requête à partir de votre application ASP.NET Core dans votre application Angular. Par exemple, vous pouvez transmettre des informations sur le cookie ou un élément lu dans une base de données. Pour ce faire, modifiez votre classe *Démarrer*. Dans le rappel pour `UseSpaPrerendering`, définissez une valeur pour `options.SupplyData` telle que la suivante :

```csharp
options.SupplyData = (context, data) =>
{
    // Creates a new value called isHttpsRequest that's passed to TypeScript code
    data["isHttpsRequest"] = context.Request.IsHttps;
};
```

Le rappel `SupplyData` vous permet de transmettre des données sérialisables JSON, arbitraires, par requête (par exemple, chaînes, valeurs booléennes ou nombres). Votre code *main.server.ts* les reçoit en tant que `params.data`. Ainsi, l’exemple de code précédent transmet une valeur booléenne en tant que `params.data.isHttpsRequest` dans le rappel `createServerRenderer`. Vous pouvez transmettre ceci à d’autres parties de votre application prises en charge par Angular. Par exemple, consultez la façon dont *main.server.ts* transmet la valeur `BASE_URL` à un composant dont le constructeur est déclaré pour le recevoir.

### <a name="drawbacks-of-ssr"></a>Inconvénients du SSR

Toutes les applications ne bénéficient pas du rendu côté serveur (SSR). Le principal avantage concerne les performances. Les visiteurs consultant votre application via une connexion réseau lente ou sur des appareils mobiles lents voient l’interface utilisateur initiale rapidement, même si l’extraction ou l’analyse des bundles JavaScript prend du temps. Toutefois, de nombreuses applications monopage sont principalement utilisées via des réseaux d’entreprise internes rapides sur des ordinateurs rapides où l’application s’affiche presque instantanément.

En même temps, il existe des inconvénients importants concernant l’activation du SSR. Il ajoute une complexité au processus de développement. Votre code doit s’exécuter dans deux environnements différents : côté client et côté serveur (dans un environnement Node.js appelé à partir d’ASP.NET Core). Voici quelques éléments à prendre en compte :

* Le SSR requiert une installation Node.js sur vos serveurs de production. C’est automatiquement le cas pour certains scénarios de déploiement, comme Azure App Services, mais pas pour d’autres, comme Azure Service Fabric.
* L’activation de l’indicateur de génération `BuildServerSideRenderer` entraîne votre répertoire *node_modules* à publier. Ce dossier contient plus de 20 000 fichiers, ce qui allonge le temps de déploiement.
* Pour exécuter votre code dans un environnement Node.js, il ne peut pas s’appuyer sur l’existence d’API JavaScript spécifiques à un navigateur comme `window` ou `localStorage`. Si votre code (ou une bibliothèque tierce que vous référencez) tente d’utiliser ces API, vous obtiendrez une erreur au cours du SSR. Par exemple, n’utilisez pas jQuery, car il fait référence à des API spécifiques au navigateur dans de nombreux endroits. Pour éviter les erreurs, vous devez éviter le SSR ou éviter les bibliothèques ou les API spécifiques au navigateur. Vous pouvez encapsuler tous les appels à ces API dans des vérifications pour vous assurer qu’ils ne sont pas appelés au cours du SSR. Par exemple, utilisez une vérification telle que la suivante dans le code JavaScript ou TypeScript :

    ```javascript
    if (typeof window !== 'undefined') {
        // Call browser-specific APIs here
    }
    ```
