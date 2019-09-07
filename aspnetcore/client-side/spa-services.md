---
title: Utilisez les services JavaScript pour créer des applications à page unique dans ASP.NET Core
author: scottaddie
description: Découvrez les avantages de l’utilisation des services JavaScript pour créer une application à page unique (SPA) sauvegardée par ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: scaddie
ms.custom: H1Hack27Feb2017
ms.date: 09/06/2019
uid: client-side/spa-services
ms.openlocfilehash: 16c9eb1d79bca792062d292795763c54dd02bd37
ms.sourcegitcommit: f65d8765e4b7c894481db9b37aa6969abc625a48
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/06/2019
ms.locfileid: "70773416"
---
# <a name="use-javascript-services-to-create-single-page-applications-in-aspnet-core"></a>Utilisez les services JavaScript pour créer des applications à page unique dans ASP.NET Core

Par [Scott Addie](https://github.com/scottaddie) et [Fiyaz Hasan](https://fiyazhasan.me/)

Une Application à Page unique (SPA) est un type d’application web en raison de son expérience utilisateur riche inhérente. L’intégration d’infrastructures ou de bibliothèques SPA côté client, telles que l' [angle](https://angular.io/) ou la [réaction](https://facebook.github.io/react/), avec des frameworks côté serveur, tels que les ASP.net Core peut s’avérer difficile. Les services JavaScript ont été développés pour réduire les frottements dans le processus d’intégration. Il permet à une opération transparente entre les piles de technologie de serveur et de client.

::: moniker range=">= aspnetcore-3.0"

> [!WARNING]
> Les fonctionnalités décrites dans cet article sont obsolètes à partir de ASP.NET Core 3,0. Un mécanisme d’intégration de frameworks SPA plus simple est disponible dans le package NuGet [Microsoft. AspNetCore. SpaServices. extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.SpaServices.Extensions) . Pour plus d’informations, consultez [[Announcement] Obsoleting Microsoft. AspNetCore. SpaServices et Microsoft. AspNetCore. NodeServices](https://github.com/aspnet/AspNetCore/issues/12890).

::: moniker-end

## <a name="what-is-javascript-services"></a>Qu’est-ce que les services JavaScript ?

Les services JavaScript sont une collection de technologies côté client pour ASP.NET Core. Son objectif est de positionner ASP.NET Core en tant que côté serveur conseillée développeurs pour la création d’applications à page unique.

Les services JavaScript sont constitués de deux packages NuGet distincts :

* [Microsoft.AspNetCore.NodeServices](https://www.nuget.org/packages/Microsoft.AspNetCore.NodeServices/) (NodeServices)
* [Microsoft.AspNetCore.SpaServices](https://www.nuget.org/packages/Microsoft.AspNetCore.SpaServices/) (SpaServices)

Ces packages sont utiles dans les scénarios suivants :

* Exécuter JavaScript sur le serveur
* Utiliser un framework SPA ou une bibliothèque
* Générez les ressources côté client avec Webpack

Une grande partie de cet article met l’accent est placée sur l’utilisation du package SpaServices.

## <a name="what-is-spaservices"></a>Qu’est SpaServices

SpaServices a été créé pour positionner ASP.NET Core en tant que côté serveur conseillée développeurs pour la création d’applications à page unique. SpaServices n’est pas requis pour développer des SPAs avec ASP.NET Core, et il ne verrouille pas les développeurs dans une infrastructure cliente particulière.

SpaServices fournit infrastructure utile telles que :

* [Côté serveur pré-rendu](#server-side-prerendering)
* [Intergiciel (middleware) de Webpack Dev](#webpack-dev-middleware)
* [Remplacement d’un Module à chaud](#hot-module-replacement)
* [Programmes d’assistance de routage](#routing-helpers)

Collectivement, ces composants d’infrastructure améliorent le flux de travail de développement et de l’expérience de l’exécution. Les composants peuvent être adoptés individuellement.

## <a name="prerequisites-for-using-spaservices"></a>Conditions préalables pour l’utilisation de SpaServices

Pour utiliser SpaServices, installez les éléments suivants :

* [Node.js](https://nodejs.org/) (version 6 ou version ultérieure) avec npm

  * Pour vérifier ces composants sont installés et sont accessibles, exécutez la commande suivante à partir de la ligne de commande :

    ```console
    node -v && npm -v
    ```

  * En cas de déploiement sur un site Web Azure, aucune action n'&mdash;est requise, node. js est installé et disponible dans les environnements de serveur.

* [!INCLUDE [](~/includes/net-core-sdk-download-link.md)]

  * Sur Windows à l’aide de Visual Studio 2017, le kit de développement logiciel (SDK) est installé en sélectionnant la charge de travail **développement multiplateforme .net Core** .

* [Microsoft.AspNetCore.SpaServices](https://www.nuget.org/packages/Microsoft.AspNetCore.SpaServices/) package NuGet

## <a name="server-side-prerendering"></a>Côté serveur pré-rendu

Une application universelle (également appelé isomorphes) est une application JavaScript capable d’exécuter à la fois sur le serveur et le client. Angular, React et autres infrastructures populaires fournissent une plateforme universelle pour ce style de développement d’application. L’idée est de rendre tout d’abord les composants d’infrastructure sur le serveur via Node.js, puis de déléguer davantage d’exécution au client.

ASP.NET Core [Tag Helpers](xref:mvc/views/tag-helpers/intro) fournie par SpaServices simplifier l’implémentation de pré-rendu de côté serveur en appelant les fonctions JavaScript sur le serveur.

### <a name="server-side-prerendering-prerequisites"></a>Conditions préalables pour le prérendu côté serveur

Installer le package NPM [ASPNET-PreRender](https://www.npmjs.com/package/aspnet-prerendering) :

```console
npm i -S aspnet-prerendering
```

### <a name="server-side-prerendering-configuration"></a>Configuration du prérendu côté serveur

Les Tag Helpers rendus détectables par le biais de l’inscription d’espace de noms dans le projet *_ViewImports.cshtml* fichier :

[!code-cshtml[](../client-side/spa-services/sample/SpaServicesSampleApp/Views/_ViewImports.cshtml?highlight=3)]

Ces Tag Helpers clarifient les subtilités de communiquer directement avec les API de bas niveau en tirant parti d’une syntaxe de type HTML à l’intérieur de la vue Razor :

[!code-cshtml[](../client-side/spa-services/sample/SpaServicesSampleApp/Views/Home/Index.cshtml?range=5)]

### <a name="asp-prerender-module-tag-helper"></a>ASP-prerende-tag Helper module

Le `asp-prerender-module` Tag Helper, utilisée dans l’exemple de code précédent, exécute *ClientApp/dist/main-server.js* sur le serveur via Node.js. Par souci de clarté, *main-server.js* fichier est un artefact de la tâche de transpilation de TypeScript et JavaScript dans le [Webpack](https://webpack.github.io/) du processus de génération. Webpack définit un alias de point d’entrée de `main-server`; et commence le parcours du graphique de dépendance pour cet alias le *ClientApp/démarrage-server.ts* fichier :

[!code-javascript[](../client-side/spa-services/sample/SpaServicesSampleApp/webpack.config.js?range=53)]

Dans l’exemple suivant Angular, le *ClientApp/démarrage-server.ts* fichier utilise le `createServerRenderer` (fonction) et `RenderResult` le type de la `aspnet-prerendering` package npm pour configurer le rendu du serveur via Node.js. Le balisage HTML destiné à rendu côté serveur est passé à un appel de fonction de résolution, lequel est encapsulé dans un script JavaScript fortement typée `Promise` objet. Le `Promise` importance de l’objet est qu’il fournit en mode asynchrone le balisage HTML pour la page pour l’injection dans l’élément d’espace réservé du DOM.

[!code-typescript[](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/boot-server.ts?range=6,10-34,79-)]

### <a name="asp-prerender-data-tag-helper"></a>ASP-prerende-tag Helper de données

Associé à la `asp-prerender-module` Tag Helper, le `asp-prerender-data` Tag Helper peut être utilisé pour transmettre des informations contextuelles à partir de la vue Razor pour le code JavaScript côté serveur. Par exemple, le balisage suivant transmet les données utilisateur à la `main-server` module :

[!code-cshtml[](../client-side/spa-services/sample/SpaServicesSampleApp/Views/Home/Index.cshtml?range=9-12)]

La réponse reçue `UserName` argument est sérialisé à l’aide du sérialiseur JSON intégré et est stocké dans le `params.data` objet. Dans l’exemple suivant Angular, les données sont utilisées pour construire un message d’accueil personnalisé dans un `h1` élément :

[!code-typescript[](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/boot-server.ts?range=6,10-21,38-52,79-)]

Les noms de propriétés passés dans tag helpers sont représentés par la notation **casse Pascal** . Comparez cela à JavaScript, où les mêmes noms de propriété sont représentés par **une casse mixte**. La configuration de sérialisation JSON par défaut est chargée de cette différence.

Pour étendre l’exemple de code précédent, données peuvent être transmises à partir du serveur à la vue par HYDRATATION le `globals` propriété fournie à la `resolve` (fonction) :

[!code-typescript[](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/boot-server.ts?range=6,10-21,57-77,79-)]

Le `postList` tableau défini à l’intérieur de la `globals` objet est attaché à du navigateur global `window` objet. Cette variable levage à portée globale permet d’éliminer une duplication des efforts, en particulier en ce qui concerne le chargement des données mêmes qu’une seule fois sur le serveur et à nouveau sur le client.

![(variable globale) postList attaché à l’objet de fenêtre](spa-services/_static/global_variable.png)

## <a name="webpack-dev-middleware"></a>Intergiciel (middleware) de Webpack Dev

[Intergiciel (middleware) de Webpack Dev](https://webpack.js.org/guides/development/#using-webpack-dev-middleware) introduit un flux de travail de développement simplifié par laquelle Webpack génère des ressources à la demande. L’intergiciel (middleware) compile automatiquement et fournit des ressources de côté client lorsqu’une page est rechargée dans le navigateur. L’autre approche consiste à appeler manuellement la Webpack via un script de génération du projet npm quand une dépendance tierce ou le code personnalisé change. Script de compilation d’un npm la *package.json* fichier est illustré dans l’exemple suivant :

```json
"build": "npm run build:vendor && npm run build:custom",
```

### <a name="webpack-dev-middleware-prerequisites"></a>Conditions préalables pour le middleware de développement WebPack

Installez le package [ASPNET-WebPack](https://www.npmjs.com/package/aspnet-webpack) NPM :

```console
npm i -D aspnet-webpack
```

### <a name="webpack-dev-middleware-configuration"></a>Configuration de l’intergiciel (middleware) WebPack

Intergiciel (middleware) de Webpack Dev est inscrit dans le pipeline de requêtes HTTP via le code suivant dans le *Startup.cs* du fichier `Configure` méthode :

[!code-csharp[](../client-side/spa-services/sample/SpaServicesSampleApp/Startup.cs?name=snippet_WebpackMiddlewareRegistration&highlight=4)]

Le `UseWebpackDevMiddleware` méthode d’extension doit être appelée avant [l’inscription de l’hébergement de fichier statique](xref:fundamentals/static-files) via la `UseStaticFiles` méthode d’extension. Pour des raisons de sécurité, inscrivez le middleware uniquement lorsque l’application s’exécute en mode de développement.

Le *webpack.config.js* du fichier `output.publicPath` propriété indique à l’intergiciel (middleware) pour surveiller le `dist` dossier pour les modifications :

[!code-javascript[](../client-side/spa-services/sample/SpaServicesSampleApp/webpack.config.js?range=6,13-16)]

## <a name="hot-module-replacement"></a>Remplacement d’un Module à chaud

Pensez à Webpack [remplacement à chaud de Module](https://webpack.js.org/concepts/hot-module-replacement/) fonctionnalité (HMR) comme une évolution de [intergiciel (middleware) de Webpack Dev](#webpack-dev-middleware). HMR présente les mêmes avantages, mais elle simplifie davantage le flux de travail de développement en mettant à jour automatiquement de contenu de la page après la compilation les modifications. À ne pas confondre avec une actualisation du navigateur, ce qui entraînerait une interférence avec l’état en mémoire actuel et la session de débogage de l’application SPA. Il existe un lien direct entre le service de l’intergiciel (middleware) de Webpack développement et le navigateur, ce qui signifie que les modifications sont envoyées au navigateur.

### <a name="hot-module-replacement-prerequisites"></a>Conditions préalables pour le remplacement des modules à chaud

Installez le package NPM [WebPack-Hot-middleware](https://www.npmjs.com/package/webpack-hot-middleware) :

```console
npm i -D webpack-hot-middleware
```

### <a name="hot-module-replacement-configuration"></a>Configuration du remplacement des modules à chaud

Le composant HMR doit être enregistré dans le pipeline de demande HTTP de MVC dans le `Configure` méthode :

```csharp
app.UseWebpackDevMiddleware(new WebpackDevMiddlewareOptions {
    HotModuleReplacement = true
});
```

En tant qu’était le cas avec [intergiciel (middleware) de Webpack Dev](#webpack-dev-middleware), le `UseWebpackDevMiddleware` méthode d’extension doit être appelée avant la `UseStaticFiles` méthode d’extension. Pour des raisons de sécurité, inscrivez le middleware uniquement lorsque l’application s’exécute en mode de développement.

Le *webpack.config.js* fichier doit définir un `plugins` de tableau, même si celui-ci est laissé vide :

[!code-javascript[](../client-side/spa-services/sample/SpaServicesSampleApp/webpack.config.js?range=6,25)]

Après le chargement de l’application dans le navigateur, onglet de la Console outils de développement fournit la confirmation de l’activation de HMR :

![Message de connexion de remplacement d’un Module à chaud](spa-services/_static/hmr_connected.png)

## <a name="routing-helpers"></a>Programmes d’assistance de routage

Dans la plupart des ASP.NET Core, le routage côté client est souvent souhaité en plus du routage côté serveur. Les systèmes de routage SPA et MVC peuvent travailler indépendamment sans interférence. Il existe, toutefois, un bord cas posant défis : identification des réponses HTTP 404.

Considérez le scénario dans lequel un itinéraire sans extension de `/some/page` est utilisé. Supposons que la demande n’à la correspondance un itinéraire côté serveur, mais son modèle ne correspond pas à un itinéraire côté client. Examinons à présent une demande entrante pour `/images/user-512.png`, lequel attend généralement rechercher un fichier image sur le serveur. Si le chemin d’accès de la ressource demandé ne correspond à aucun itinéraire côté serveur ou fichier statique, il est peu probable que l’application côté&mdash;client ne puisse le gérer. en général, le code d’état HTTP 404 est attendu.

### <a name="routing-helpers-prerequisites"></a>Conditions préalables pour le routage

Installez le package NPM du routage côté client. À l’aide d’Angular par exemple :

```console
npm i -S @angular/router
```

### <a name="routing-helpers-configuration"></a>Configuration des assistances pour le routage

Une méthode d’extension nommée `MapSpaFallbackRoute` est utilisé dans le `Configure` méthode :

[!code-csharp[](../client-side/spa-services/sample/SpaServicesSampleApp/Startup.cs?name=snippet_MvcRoutingTable&highlight=7-9)]

Les itinéraires sont évalués dans l’ordre dans lequel ils sont configurés. Par conséquent, le `default` itinéraire dans l’exemple de code précédent est utilisé pour les critères spéciaux.

## <a name="create-a-new-project"></a>Créer un nouveau projet

Les services JavaScript fournissent des modèles d’application préconfigurés. SpaServices est utilisé dans ces modèles conjointement avec différents frameworks et bibliothèques, tels que angulaire, REACT et Redux.

Ces modèles peuvent être installés par le biais de l’interface CLI .NET Core en exécutant la commande suivante :

```console
dotnet new --install Microsoft.AspNetCore.SpaTemplates::*
```

Une liste des modèles disponibles s’affiche :

| Modèles                                 | Nom court | Langue | Balises        |
| ------------------------------------------| :--------: | :------: | :---------: |
| MVC ASP.NET Core avec Angular             | angular    | [C#]     | MVC/Web/SPA |
| MVC ASP.NET Core avec React.js            | react      | [C#]     | MVC/Web/SPA |
| MVC ASP.NET Core avec React.js et Redux  | reactredux | [C#]     | MVC/Web/SPA |

Pour créer un nouveau projet à l’aide d’un des modèles SPA, incluez le **nom court** du modèle dans le [dotnet nouvelle](/dotnet/core/tools/dotnet-new) commande. La commande suivante crée une application Angular avec ASP.NET Core MVC est configuré pour le côté serveur :

```console
dotnet new angular
```

### <a name="set-the-runtime-configuration-mode"></a>Définir le mode de configuration du runtime

Il existe deux modes de configuration de runtime principal :

* **Développement**:
  * Il inclut des mappages de source pour faciliter le débogage.
  * N’Optimisez le code côté client pour les performances.
* **Production**:
  * Exclut les mappages de sources.
  * Optimise le code côté client via le regroupement et la minimisation.

ASP.NET Core utilise une variable d’environnement nommée `ASPNETCORE_ENVIRONMENT` pour stocker le mode de configuration. Pour plus d’informations, consultez [définir l’environnement](xref:fundamentals/environments#set-the-environment).

### <a name="run-with-net-core-cli"></a>Exécuter avec CLI .NET Core

Restaurer le NuGet requis et les packages npm en exécutant la commande suivante à la racine du projet :

```console
dotnet restore && npm i
```

Générer et exécuter l’application :

```console
dotnet run
```

Démarrage de l’application sur l’hôte local en fonction de la [mode de configuration de runtime](#set-the-runtime-configuration-mode). Navigation vers `http://localhost:5000` dans le navigateur affiche la page d’accueil.

### <a name="run-with-visual-studio-2017"></a>Exécuter avec Visual Studio 2017

Ouvrez le *.csproj* fichier généré par le [dotnet nouvelle](/dotnet/core/tools/dotnet-new) commande. Les packages NuGet et npm nécessaires sont restaurés automatiquement sur le projet ouvert. Ce processus de restauration peut prendre quelques minutes, et l’application est prête à exécuter lorsqu’elle est terminée. Cliquez sur le bouton d’exécution vert ou appuyez sur `Ctrl + F5`, et le navigateur s’ouvre à la page d’accueil de l’application. L’application s’exécute sur l’hôte local en fonction de la [mode de configuration de runtime](#set-the-runtime-configuration-mode).

## <a name="test-the-app"></a>Tester l’application

Les modèles SpaServices sont préconfigurées pour exécuter des tests de côté client à l’aide de [Karma](https://karma-runner.github.io/1.0/index.html) et [Jasmine](https://jasmine.github.io/). Jasmine est une unité de populaires framework de tests pour JavaScript, tandis que Karma est un test runner pour ces tests. KARMA est configuré pour fonctionner avec le [intergiciel (middleware) de Webpack Dev](#webpack-dev-middleware) telles que le développeur n’est pas nécessaire d’arrêter et exécuter le test chaque fois que des modifications. Que ce soit le code s’exécutant sur le cas de test ou le cas de test lui-même, le test s’exécute automatiquement.

À l’aide de l’application Angular par exemple, deux cas de test Jasmine sont déjà fournies pour le `CounterComponent` dans le *counter.component.spec.ts* fichier :

[!code-typescript[](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/app/components/counter/counter.component.spec.ts?range=15-28)]

Ouvrez l’invite de commandes dans le *ClientApp* directory. Exécutez la commande suivante :

```console
npm test
```

Le script lance le testeur Karma, qui lit les paramètres définis dans le *karma.conf.js* fichier. Parmi d’autres paramètres, le *karma.conf.js* identifie les fichiers de test doit être exécuté par le biais de son `files` tableau :

[!code-javascript[](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/test/karma.conf.js?range=4-5,8-11)]

## <a name="publish-the-app"></a>Publier l'application

Combinant les ressources côté client générés et les artefacts de ASP.NET Core publiées dans un package prêt à déployer peut s’avérer fastidieuse. Heureusement, SpaServices orchestre ce processus d’ensemble de la publication avec une cible MSBuild personnalisée nommée `RunWebpack`:

[!code-xml[](../client-side/spa-services/sample/SpaServicesSampleApp/SpaServicesSampleApp.csproj?range=31-45)]

La cible MSBuild responsabilités est les suivantes :

1. Restaurez les packages NPM.
1. Créer une build de niveau production des ressources tierces côté client.
1. Créez une build de niveau production des ressources client personnalisées.
1. Copiez les ressources générées par WebPack dans le dossier de publication.

La cible MSBuild est appelée lors de l’exécution :

```console
dotnet publish -c Release
```

## <a name="additional-resources"></a>Ressources supplémentaires

* [Docs angulaires](https://angular.io/docs)
