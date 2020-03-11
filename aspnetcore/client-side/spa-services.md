---
title: Utilisez les services JavaScript pour créer des applications à page unique dans ASP.NET Core
author: scottaddie
description: Découvrez les avantages de l’utilisation des services JavaScript pour créer une application à page unique (SPA) sauvegardée par ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: scaddie
ms.custom: H1Hack27Feb2017
ms.date: 09/06/2019
uid: client-side/spa-services
ms.openlocfilehash: c0c73882afd579510ad9cdf5b485c1d6fbeadd1c
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78663778"
---
# <a name="use-javascript-services-to-create-single-page-applications-in-aspnet-core"></a>Utilisez les services JavaScript pour créer des applications à page unique dans ASP.NET Core

Par [Scott Addie](https://github.com/scottaddie) et [Fiyaz Hasan](https://fiyazhasan.me/)

Une Application à Page unique (SPA) est un type d’application web en raison de son expérience utilisateur riche inhérente. L’intégration d’infrastructures ou de bibliothèques SPA côté client, telles que l' [angle](https://angular.io/) ou la [réaction](https://facebook.github.io/react/), avec des frameworks côté serveur, tels que les ASP.net Core peut s’avérer difficile. Les services JavaScript ont été développés pour réduire les frottements dans le processus d’intégration. Il permet à une opération transparente entre les piles de technologie de serveur et de client.

::: moniker range=">= aspnetcore-3.0"

> [!WARNING]
> Les fonctionnalités décrites dans cet article sont obsolètes à partir de ASP.NET Core 3,0. Un mécanisme d’intégration de frameworks SPA plus simple est disponible dans le package NuGet [Microsoft. AspNetCore. SpaServices. extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.SpaServices.Extensions) . Pour plus d’informations, consultez [[Announcement] Obsoleting Microsoft. AspNetCore. SpaServices et Microsoft. AspNetCore. NodeServices](https://github.com/dotnet/AspNetCore/issues/12890).

::: moniker-end

## <a name="what-is-javascript-services"></a>Qu’est-ce que les services JavaScript ?

Les services JavaScript sont une collection de technologies côté client pour ASP.NET Core. Son objectif est de positionner ASP.NET Core en tant que côté serveur conseillée développeurs pour la création d’applications à page unique.

Les services JavaScript sont constitués de deux packages NuGet distincts :

* [Microsoft. AspNetCore. NodeServices](https://www.nuget.org/packages/Microsoft.AspNetCore.NodeServices/) (NodeServices)
* [Microsoft. AspNetCore. SpaServices](https://www.nuget.org/packages/Microsoft.AspNetCore.SpaServices/) (SpaServices)

Ces packages sont utiles dans les scénarios suivants :

* Exécuter JavaScript sur le serveur
* Utiliser un framework SPA ou une bibliothèque
* Générez les ressources côté client avec Webpack

Une grande partie de cet article met l’accent est placée sur l’utilisation du package SpaServices.

## <a name="what-is-spaservices"></a>Qu’est SpaServices

SpaServices a été créé pour positionner ASP.NET Core en tant que côté serveur conseillée développeurs pour la création d’applications à page unique. SpaServices n’est pas requis pour développer des SPAs avec ASP.NET Core, et il ne verrouille pas les développeurs dans une infrastructure cliente particulière.

SpaServices fournit infrastructure utile telles que :

* [Prérendu côté serveur](#server-side-prerendering)
* [Middleware de développement WebPack](#webpack-dev-middleware)
* [Remplacement du module réactif](#hot-module-replacement)
* [Assistance du routage](#routing-helpers)

Collectivement, ces composants d’infrastructure améliorent le flux de travail de développement et de l’expérience de l’exécution. Les composants peuvent être adoptés individuellement.

## <a name="prerequisites-for-using-spaservices"></a>Conditions préalables pour l’utilisation de SpaServices

Pour utiliser SpaServices, installez les éléments suivants :

* [Node. js](https://nodejs.org/) (version 6 ou ultérieure) avec NPM

  * Pour vérifier ces composants sont installés et sont accessibles, exécutez la commande suivante à partir de la ligne de commande :

    ```console
    node -v && npm -v
    ```

  * En cas de déploiement sur un site Web Azure, aucune action n’est requise&mdash;node. js est installé et disponible dans les environnements de serveur.

* [!INCLUDE [](~/includes/net-core-sdk-download-link.md)]

  * Sur Windows à l’aide de Visual Studio 2017, le kit de développement logiciel (SDK) est installé en sélectionnant la charge de travail **développement multiplateforme .net Core** .

* Package NuGet [Microsoft. AspNetCore. SpaServices](https://www.nuget.org/packages/Microsoft.AspNetCore.SpaServices/)

## <a name="server-side-prerendering"></a>Côté serveur pré-rendu

Une application universelle (également appelé isomorphes) est une application JavaScript capable d’exécuter à la fois sur le serveur et le client. Angular, React et autres infrastructures populaires fournissent une plateforme universelle pour ce style de développement d’application. L’idée est de rendre tout d’abord les composants d’infrastructure sur le serveur via Node.js, puis de déléguer davantage d’exécution au client.

ASP.NET Core [balises](xref:mvc/views/tag-helpers/intro) fournies par SpaServices simplifient l’implémentation du prérendu côté serveur en appelant les fonctions JavaScript sur le serveur.

### <a name="server-side-prerendering-prerequisites"></a>Conditions préalables pour le prérendu côté serveur

Installer le package NPM [ASPNET-PreRender](https://www.npmjs.com/package/aspnet-prerendering) :

```console
npm i -S aspnet-prerendering
```

### <a name="server-side-prerendering-configuration"></a>Configuration du prérendu côté serveur

Les tag helpers sont rendus détectables via l’inscription de l’espace de noms dans le fichier *_ViewImports. cshtml* du projet :

[!code-cshtml[](../client-side/spa-services/sample/SpaServicesSampleApp/Views/_ViewImports.cshtml?highlight=3)]

Ces Tag Helpers clarifient les subtilités de communiquer directement avec les API de bas niveau en tirant parti d’une syntaxe de type HTML à l’intérieur de la vue Razor :

[!code-cshtml[](../client-side/spa-services/sample/SpaServicesSampleApp/Views/Home/Index.cshtml?range=5)]

### <a name="asp-prerender-module-tag-helper"></a>ASP-prerende-tag Helper module

Le tag Helper `asp-prerender-module`, utilisé dans l’exemple de code précédent, exécute *ClientApp/dist/main-Server. js* sur le serveur via node. js. Par souci de clarté, le fichier *main-Server. js* est un artefact de la tâche Transpilation de machine à écrire en JavaScript dans le processus de génération [WebPack](https://webpack.github.io/) . WebPack définit un alias de point d’entrée de `main-server`; et, le parcours du graphique de dépendance pour cet alias commence au niveau du fichier *ClientApp/Boot-Server. TS* :

[!code-javascript[](../client-side/spa-services/sample/SpaServicesSampleApp/webpack.config.js?range=53)]

Dans l’exemple angulaire suivant, le fichier *ClientApp/Boot-Server. TS* utilise la fonction `createServerRenderer` et `RenderResult` type du package NPM `aspnet-prerendering` pour configurer le rendu du serveur via node. js. Le balisage HTML destiné au rendu côté serveur est passé à un appel de fonction Resolve, qui est encapsulé dans un objet `Promise` JavaScript fortement typé. L’importance de l’objet `Promise` est qu’il fournit de manière asynchrone le balisage HTML à la page pour l’injection dans l’élément d’espace réservé du DOM.

[!code-typescript[](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/boot-server.ts?range=6,10-34,79-)]

### <a name="asp-prerender-data-tag-helper"></a>ASP-prerende-tag Helper de données

Lorsqu’il est associé au tag Helper `asp-prerender-module`, le tag Helper `asp-prerender-data` peut être utilisé pour transmettre des informations contextuelles de la vue Razor au code JavaScript côté serveur. Par exemple, le balisage suivant passe les données utilisateur au module `main-server` :

[!code-cshtml[](../client-side/spa-services/sample/SpaServicesSampleApp/Views/Home/Index.cshtml?range=9-12)]

L’argument `UserName` reçu est sérialisé à l’aide du sérialiseur JSON intégré et est stocké dans l’objet `params.data`. Dans l’exemple angulaire suivant, les données sont utilisées pour construire un message de salutation personnalisé au sein d’un élément `h1` :

[!code-typescript[](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/boot-server.ts?range=6,10-21,38-52,79-)]

Les noms de propriétés passés dans tag helpers sont représentés par la notation **casse Pascal** . Comparez-le à JavaScript, où les mêmes noms de propriétés sont représentés avec **la casse mixte**. La configuration de sérialisation JSON par défaut est chargée de cette différence.

Pour développer l’exemple de code précédent, les données peuvent être transmises du serveur à la vue en mettant en attente la propriété `globals` fournie à la fonction `resolve` :

[!code-typescript[](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/boot-server.ts?range=6,10-21,57-77,79-)]

Le tableau `postList` défini dans l’objet `globals` est attaché à l’objet `window` global du navigateur. Cette variable levage à portée globale permet d’éliminer une duplication des efforts, en particulier en ce qui concerne le chargement des données mêmes qu’une seule fois sur le serveur et à nouveau sur le client.

![(variable globale) postList attaché à l’objet de fenêtre](spa-services/_static/global_variable.png)

## <a name="webpack-dev-middleware"></a>Intergiciel (middleware) de Webpack Dev

L’intergiciel (middleware) de développement [WebPack](https://webpack.js.org/guides/development/#using-webpack-dev-middleware) introduit un flux de travail de développement rationalisé dans lequel WebPack génère des ressources à la demande. L’intergiciel (middleware) compile automatiquement et fournit des ressources de côté client lorsqu’une page est rechargée dans le navigateur. L’autre approche consiste à appeler manuellement la Webpack via un script de génération du projet npm quand une dépendance tierce ou le code personnalisé change. Un script de génération NPM dans le fichier *Package. JSON* est illustré dans l’exemple suivant :

```json
"build": "npm run build:vendor && npm run build:custom",
```

### <a name="webpack-dev-middleware-prerequisites"></a>Conditions préalables pour le middleware de développement WebPack

Installez le package [ASPNET-WebPack](https://www.npmjs.com/package/aspnet-webpack) NPM :

```console
npm i -D aspnet-webpack
```

### <a name="webpack-dev-middleware-configuration"></a>Configuration de l’intergiciel (middleware) WebPack

L’intergiciel (middleware) de développement WebPack est inscrit dans le pipeline de requête HTTP via le code suivant dans la méthode `Configure` du fichier *Startup.cs* :

[!code-csharp[](../client-side/spa-services/sample/SpaServicesSampleApp/Startup.cs?name=snippet_WebpackMiddlewareRegistration&highlight=4)]

La méthode d’extension `UseWebpackDevMiddleware` doit être appelée avant l’inscription de l' [Hébergement de fichiers statiques](xref:fundamentals/static-files) via la méthode d’extension `UseStaticFiles`. Pour des raisons de sécurité, inscrivez le middleware uniquement lorsque l’application s’exécute en mode de développement.

La propriété `output.publicPath` du fichier *WebPack. config. js* indique à l’intergiciel (middleware) de surveiller les modifications apportées au dossier `dist` :

[!code-javascript[](../client-side/spa-services/sample/SpaServicesSampleApp/webpack.config.js?range=6,13-16)]

## <a name="hot-module-replacement"></a>Remplacement d’un Module à chaud

Considérez la fonctionnalité de [remplacement de module à chaud](https://webpack.js.org/concepts/hot-module-replacement/) (HMR) de WebPack comme une évolution de l’intergiciel (middleware) de [développement WebPack](#webpack-dev-middleware). HMR présente les mêmes avantages, mais elle simplifie davantage le flux de travail de développement en mettant à jour automatiquement de contenu de la page après la compilation les modifications. À ne pas confondre avec une actualisation du navigateur, ce qui entraînerait une interférence avec l’état en mémoire actuel et la session de débogage de l’application SPA. Il existe un lien direct entre le service de l’intergiciel (middleware) de Webpack développement et le navigateur, ce qui signifie que les modifications sont envoyées au navigateur.

### <a name="hot-module-replacement-prerequisites"></a>Conditions préalables pour le remplacement des modules à chaud

Installez le package NPM [WebPack-Hot-middleware](https://www.npmjs.com/package/webpack-hot-middleware) :

```console
npm i -D webpack-hot-middleware
```

### <a name="hot-module-replacement-configuration"></a>Configuration du remplacement des modules à chaud

Le composant HMR doit être inscrit dans le pipeline de requête HTTP de MVC dans la méthode `Configure` :

```csharp
app.UseWebpackDevMiddleware(new WebpackDevMiddlewareOptions {
    HotModuleReplacement = true
});
```

Comme c’était le cas avec l’intergiciel (middleware) de [développement WebPack](#webpack-dev-middleware), la méthode d’extension `UseWebpackDevMiddleware` doit être appelée avant la méthode d’extension `UseStaticFiles`. Pour des raisons de sécurité, inscrivez le middleware uniquement lorsque l’application s’exécute en mode de développement.

Le fichier *WebPack. config. js* doit définir un tableau `plugins`, même s’il est laissé vide :

[!code-javascript[](../client-side/spa-services/sample/SpaServicesSampleApp/webpack.config.js?range=6,25)]

Après le chargement de l’application dans le navigateur, onglet de la Console outils de développement fournit la confirmation de l’activation de HMR :

![Message de connexion de remplacement d’un Module à chaud](spa-services/_static/hmr_connected.png)

## <a name="routing-helpers"></a>Programmes d’assistance de routage

Dans la plupart des ASP.NET Core, le routage côté client est souvent souhaité en plus du routage côté serveur. Les systèmes de routage SPA et MVC peuvent travailler indépendamment sans interférence. Il existe, toutefois, un bord cas posant défis : identification des réponses HTTP 404.

Considérez le scénario dans lequel un itinéraire sans extension de `/some/page` est utilisé. Supposons que la demande n’à la correspondance un itinéraire côté serveur, mais son modèle ne correspond pas à un itinéraire côté client. Considérons à présent une demande entrante pour `/images/user-512.png`, qui s’attend généralement à trouver un fichier image sur le serveur. Si le chemin d’accès de la ressource demandé ne correspond à aucun itinéraire côté serveur ou à un fichier statique, il est peu probable que l’application côté client la gère&mdash;qui retourne généralement un code d’état HTTP 404 est souhaité.

### <a name="routing-helpers-prerequisites"></a>Conditions préalables pour le routage

Installez le package NPM du routage côté client. À l’aide d’Angular par exemple :

```console
npm i -S @angular/router
```

### <a name="routing-helpers-configuration"></a>Configuration des assistances pour le routage

Une méthode d’extension nommée `MapSpaFallbackRoute` est utilisée dans la méthode `Configure` :

[!code-csharp[](../client-side/spa-services/sample/SpaServicesSampleApp/Startup.cs?name=snippet_MvcRoutingTable&highlight=7-9)]

Les itinéraires sont évalués dans l’ordre dans lequel ils sont configurés. Par conséquent, l’itinéraire de `default` dans l’exemple de code précédent est utilisé en premier pour les critères spéciaux.

## <a name="create-a-new-project"></a>Création d'un projet

Les services JavaScript fournissent des modèles d’application préconfigurés. SpaServices est utilisé dans ces modèles conjointement avec différents frameworks et bibliothèques, tels que angulaire, REACT et Redux.

Ces modèles peuvent être installés par le biais de l’interface CLI .NET Core en exécutant la commande suivante :

```dotnetcli
dotnet new --install Microsoft.AspNetCore.SpaTemplates::*
```

Une liste des modèles disponibles s’affiche :

| Modèles                                 | Nom court | Langage | Balises        |
| ------------------------------------------| :--------: | :------: | :---------: |
| MVC ASP.NET Core avec Angular             | angular    | [C#]     | Web/MVC/SPA |
| MVC ASP.NET Core avec React.js            | react      | [C#]     | Web/MVC/SPA |
| MVC ASP.NET Core avec React.js et Redux  | reactredux | [C#]     | Web/MVC/SPA |

Pour créer un nouveau projet à l’aide de l’un des modèles SPA, incluez le **nom abrégé** du modèle dans la commande [dotnet New](/dotnet/core/tools/dotnet-new) . La commande suivante crée une application Angular avec ASP.NET Core MVC est configuré pour le côté serveur :

```dotnetcli
dotnet new angular
```

### <a name="set-the-runtime-configuration-mode"></a>Définir le mode de configuration du runtime

Il existe deux modes de configuration de runtime principal :

* **Développement** :
  * Il inclut des mappages de source pour faciliter le débogage.
  * N’Optimisez le code côté client pour les performances.
* **Production** :
  * Exclut les mappages de sources.
  * Optimise le code côté client via le regroupement et la minimisation.

ASP.NET Core utilise une variable d’environnement nommée `ASPNETCORE_ENVIRONMENT` pour stocker le mode de configuration. Pour plus d’informations, consultez [définir l’environnement](xref:fundamentals/environments#set-the-environment).

### <a name="run-with-net-core-cli"></a>Exécuter avec CLI .NET Core

Restaurer le NuGet requis et les packages npm en exécutant la commande suivante à la racine du projet :

```dotnetcli
dotnet restore && npm i
```

Générer et exécuter l’application :

```dotnetcli
dotnet run
```

L’application démarre sur localhost en fonction du [mode de configuration](#set-the-runtime-configuration-mode)de l’exécution. La navigation vers `http://localhost:5000` dans le navigateur affiche la page d’accueil.

### <a name="run-with-visual-studio-2017"></a>Exécuter avec Visual Studio 2017

Ouvrez le fichier *. csproj* généré par la commande [dotnet New](/dotnet/core/tools/dotnet-new) . Les packages NuGet et npm nécessaires sont restaurés automatiquement sur le projet ouvert. Ce processus de restauration peut prendre quelques minutes, et l’application est prête à exécuter lorsqu’elle est terminée. Cliquez sur le bouton vert exécuter ou appuyez sur `Ctrl + F5`, et le navigateur s’ouvre sur la page d’accueil de l’application. L’application s’exécute sur localhost en fonction du [mode de configuration du runtime](#set-the-runtime-configuration-mode).

## <a name="test-the-app"></a>Test de l'application

Les modèles SpaServices sont préconfigurés pour exécuter des tests côté client à l’aide de [Karma](https://karma-runner.github.io/1.0/index.html) et [Jasmine](https://jasmine.github.io/). Jasmine est une unité de populaires framework de tests pour JavaScript, tandis que Karma est un test runner pour ces tests. Karma est configuré pour fonctionner avec l' [intergiciel (middleware) WebPack dev](#webpack-dev-middleware) , de sorte que le développeur n’est pas obligé d’arrêter et d’exécuter le test à chaque fois que des modifications sont apportées. Que ce soit le code s’exécutant sur le cas de test ou le cas de test lui-même, le test s’exécute automatiquement.

À l’aide de l’application angulaire comme exemple, deux cas de test Jasmine sont déjà fournis pour le `CounterComponent` dans le fichier *Counter. composant. spec. TS* :

[!code-typescript[](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/app/components/counter/counter.component.spec.ts?range=15-28)]

Ouvrez l’invite de commandes dans le répertoire *ClientApp* . Exécutez la commande suivante :

```console
npm test
```

Le script lance Karma Test Runner, qui lit les paramètres définis dans le fichier *Karma. conf. js* . Parmi d’autres paramètres, *Karma. conf. js* identifie les fichiers de test à exécuter via son tableau `files` :

[!code-javascript[](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/test/karma.conf.js?range=4-5,8-11)]

## <a name="publish-the-app"></a>Publier l’application

Pour plus d’informations sur la publication sur Azure, consultez [ce numéro GitHub](https://github.com/dotnet/AspNetCore.Docs/issues/12474) .

Combinant les ressources côté client générés et les artefacts de ASP.NET Core publiées dans un package prêt à déployer peut s’avérer fastidieuse. Heureusement, SpaServices orchestre la totalité du processus de publication avec une cible MSBuild personnalisée nommée `RunWebpack`:

[!code-xml[](../client-side/spa-services/sample/SpaServicesSampleApp/SpaServicesSampleApp.csproj?range=31-45)]

La cible MSBuild responsabilités est les suivantes :

1. Restaurez les packages NPM.
1. Créer une build de niveau production des ressources tierces côté client.
1. Créez une build de niveau production des ressources client personnalisées.
1. Copiez les ressources générées par WebPack dans le dossier de publication.

La cible MSBuild est appelée lors de l’exécution :

```dotnetcli
dotnet publish -c Release
```

## <a name="additional-resources"></a>Ressources supplémentaires

* [Documents angulaires](https://angular.io/docs)
