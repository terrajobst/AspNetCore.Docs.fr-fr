---
title: Utiliser JavaScriptServices pour créer des Applications à Page unique dans ASP.NET Core
author: scottaddie
description: En savoir plus sur les avantages de l’utilisation de JavaScriptServices pour créer une Application à Page unique (SPA) soutenu par ASP.NET Core.
ms.author: scaddie
ms.custom: H1Hack27Feb2017
ms.date: 08/02/2017
uid: client-side/spa-services
ms.openlocfilehash: b0fc6be29e3ecedd9706238f439f229377bb5a63
ms.sourcegitcommit: ad28d1bc6657a743d5c2fa8902f82740689733bb
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/20/2018
ms.locfileid: "52256548"
---
# <a name="use-javascriptservices-to-create-single-page-applications-in-aspnet-core"></a>Utiliser JavaScriptServices pour créer des Applications à Page unique dans ASP.NET Core

Par [Scott Addie](https://github.com/scottaddie) et [Fiyaz Hasan](http://fiyazhasan.me/)

Une Application à Page unique (SPA) est un type d’application web en raison de son expérience utilisateur riche inhérente. L’intégration de frameworks SPA côté client ou les bibliothèques, telles que [Angular](https://angular.io/) ou [réagir](https://facebook.github.io/react/), avec les infrastructures côté serveur comme ASP.NET Core peut s’avérer difficile. [JavaScriptServices](https://github.com/aspnet/JavaScriptServices) a été développé pour réduire la friction lors du processus d’intégration. Il permet à une opération transparente entre les piles de technologie de serveur et de client.

<a name="what-is-js-services"></a>

## <a name="what-is-javascriptservices"></a>Qu’est JavaScriptServices

JavaScriptServices est une collection de technologies côté client pour ASP.NET Core. Son objectif est de positionner ASP.NET Core en tant que côté serveur conseillée développeurs pour la création d’applications à page unique.

JavaScriptServices se compose de trois packages NuGet distinctes :

* [Microsoft.AspNetCore.NodeServices](https://www.nuget.org/packages/Microsoft.AspNetCore.NodeServices/) (NodeServices)
* [Microsoft.AspNetCore.SpaServices](https://www.nuget.org/packages/Microsoft.AspNetCore.SpaServices/) (SpaServices)
* [Microsoft.AspNetCore.SpaTemplates](https://www.nuget.org/packages/Microsoft.AspNetCore.SpaTemplates/) (SpaTemplates)

Ces packages sont utiles si vous :

* Exécuter JavaScript sur le serveur
* Utiliser un framework SPA ou une bibliothèque
* Générez les ressources côté client avec Webpack

Une grande partie de cet article met l’accent est placée sur l’utilisation du package SpaServices.

<a name="what-is-spa-services"></a>

## <a name="what-is-spaservices"></a>Qu’est SpaServices

SpaServices a été créé pour positionner ASP.NET Core en tant que côté serveur conseillée développeurs pour la création d’applications à page unique. SpaServices n’est pas requis pour développer des applications à page unique avec ASP.NET Core, et il ne vous dans une infrastructure client particulier.

SpaServices fournit infrastructure utile telles que :

* [Côté serveur pré-rendu](#server-prerendering)
* [Intergiciel (middleware) de Webpack Dev](#webpack-dev-middleware)
* [Remplacement d’un Module à chaud](#hot-module-replacement)
* [Programmes d’assistance de routage](#routing-helpers)

Collectivement, ces composants d’infrastructure améliorent le flux de travail de développement et de l’expérience de l’exécution. Les composants peuvent être adoptés individuellement.

<a name="spa-services-prereqs"></a>

## <a name="prerequisites-for-using-spaservices"></a>Conditions préalables pour l’utilisation de SpaServices

Pour utiliser SpaServices, installez les éléments suivants :

* [Node.js](https://nodejs.org/) (version 6 ou version ultérieure) avec npm
  * Pour vérifier ces composants sont installés et sont accessibles, exécutez la commande suivante à partir de la ligne de commande :

    ```console
    node -v && npm -v
    ```

Remarque : Si vous effectuez un déploiement vers un site web Azure, vous n’avez à faire quoi que ce soit ici &mdash; Node.js est installé et disponible dans les environnements de serveur.

* [!INCLUDE [](~/includes/net-core-sdk-download-link.md)]

  * Si vous êtes sur Windows à l’aide de Visual Studio 2017, le SDK est installé en sélectionnant le **.NET Core le développement multiplateforme** charge de travail.

* [Microsoft.AspNetCore.SpaServices](https://www.nuget.org/packages/Microsoft.AspNetCore.SpaServices/) package NuGet

<a name="server-prerendering"></a>

## <a name="server-side-prerendering"></a>Côté serveur pré-rendu

Une application universelle (également appelé isomorphes) est une application JavaScript capable d’exécuter à la fois sur le serveur et le client. Angular, React et autres infrastructures populaires fournissent une plateforme universelle pour ce style de développement d’application. L’idée est de rendre tout d’abord les composants d’infrastructure sur le serveur via Node.js, puis de déléguer davantage d’exécution au client.

ASP.NET Core [Tag Helpers](xref:mvc/views/tag-helpers/intro) fournie par SpaServices simplifier l’implémentation de pré-rendu de côté serveur en appelant les fonctions JavaScript sur le serveur.

### <a name="prerequisites"></a>Prérequis

Installez les éléments suivants :

* [ASPNET-pré-rendu](https://www.npmjs.com/package/aspnet-prerendering) package npm :

    ```console
    npm i -S aspnet-prerendering
    ```

### <a name="configuration"></a>Configuration

Les Tag Helpers rendus détectables par le biais de l’inscription d’espace de noms dans le projet *_ViewImports.cshtml* fichier :

[!code-cshtml[](../client-side/spa-services/sample/SpaServicesSampleApp/Views/_ViewImports.cshtml?highlight=3)]

Ces Tag Helpers clarifient les subtilités de communiquer directement avec les API de bas niveau en tirant parti d’une syntaxe de type HTML à l’intérieur de la vue Razor :

[!code-cshtml[](../client-side/spa-services/sample/SpaServicesSampleApp/Views/Home/Index.cshtml?range=5)]

### <a name="the-asp-prerender-module-tag-helper"></a>Le `asp-prerender-module` Tag Helper

Le `asp-prerender-module` Tag Helper, utilisée dans l’exemple de code précédent, exécute *ClientApp/dist/main-server.js* sur le serveur via Node.js. Par souci de clarté, *main-server.js* fichier est un artefact de la tâche de transpilation de TypeScript et JavaScript dans le [Webpack](http://webpack.github.io/) du processus de génération. Webpack définit un alias de point d’entrée de `main-server`; et commence le parcours du graphique de dépendance pour cet alias le *ClientApp/démarrage-server.ts* fichier :

[!code-javascript[](../client-side/spa-services/sample/SpaServicesSampleApp/webpack.config.js?range=53)]

Dans l’exemple suivant Angular, le *ClientApp/démarrage-server.ts* fichier utilise le `createServerRenderer` (fonction) et `RenderResult` le type de la `aspnet-prerendering` package npm pour configurer le rendu du serveur via Node.js. Le balisage HTML destiné à rendu côté serveur est passé à un appel de fonction de résolution, lequel est encapsulé dans un script JavaScript fortement typée `Promise` objet. Le `Promise` importance de l’objet est qu’il fournit en mode asynchrone le balisage HTML pour la page pour l’injection dans l’élément d’espace réservé du DOM.

[!code-typescript[](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/boot-server.ts?range=6,10-34,79-)]

### <a name="the-asp-prerender-data-tag-helper"></a>Le `asp-prerender-data` Tag Helper

Associé à la `asp-prerender-module` Tag Helper, le `asp-prerender-data` Tag Helper peut être utilisé pour transmettre des informations contextuelles à partir de la vue Razor pour le code JavaScript côté serveur. Par exemple, le balisage suivant transmet les données utilisateur à la `main-server` module :

[!code-cshtml[](../client-side/spa-services/sample/SpaServicesSampleApp/Views/Home/Index.cshtml?range=9-12)]

La réponse reçue `UserName` argument est sérialisé à l’aide du sérialiseur JSON intégré et est stocké dans le `params.data` objet. Dans l’exemple suivant Angular, les données sont utilisées pour construire un message d’accueil personnalisé dans un `h1` élément :

[!code-typescript[](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/boot-server.ts?range=6,10-21,38-52,79-)]

Remarque : Les noms de propriété passés dans les Tag Helpers sont représentés par **la casse Pascal** notation. Comparez cela à JavaScript, où les mêmes noms de propriété sont représentés par **une casse mixte**. La configuration de sérialisation JSON par défaut est chargée de cette différence.

Pour étendre l’exemple de code précédent, données peuvent être transmises à partir du serveur à la vue par HYDRATATION le `globals` propriété fournie à la `resolve` (fonction) :

[!code-typescript[](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/boot-server.ts?range=6,10-21,57-77,79-)]

Le `postList` tableau défini à l’intérieur de la `globals` objet est attaché à du navigateur global `window` objet. Cette variable levage à portée globale permet d’éliminer une duplication des efforts, en particulier en ce qui concerne le chargement des données mêmes qu’une seule fois sur le serveur et à nouveau sur le client.

![(variable globale) postList attaché à l’objet de fenêtre](spa-services/_static/global_variable.png)

<a name="webpack-dev-middleware"></a>

## <a name="webpack-dev-middleware"></a>Intergiciel (middleware) de Webpack Dev

[Intergiciel (middleware) de Webpack Dev](https://webpack.github.io/docs/webpack-dev-middleware.html) introduit un flux de travail de développement simplifié par laquelle Webpack génère des ressources à la demande. L’intergiciel (middleware) compile automatiquement et fournit des ressources de côté client lorsqu’une page est rechargée dans le navigateur. L’autre approche consiste à appeler manuellement la Webpack via un script de génération du projet npm quand une dépendance tierce ou le code personnalisé change. Script de compilation d’un npm la *package.json* fichier est illustré dans l’exemple suivant :

```json
"build": "npm run build:vendor && npm run build:custom",
```

### <a name="prerequisites"></a>Prérequis

Installez les éléments suivants :

* [ASPNET-webpack](https://www.npmjs.com/package/aspnet-webpack) package npm :

    ```console
    npm i -D aspnet-webpack
    ```

### <a name="configuration"></a>Configuration

Intergiciel (middleware) de Webpack Dev est inscrit dans le pipeline de requêtes HTTP via le code suivant dans le *Startup.cs* du fichier `Configure` méthode :

[!code-csharp[](../client-side/spa-services/sample/SpaServicesSampleApp/Startup.cs?name=webpack-middleware-registration&highlight=4)]

Le `UseWebpackDevMiddleware` méthode d’extension doit être appelée avant [l’inscription de l’hébergement de fichier statique](xref:fundamentals/static-files) via la `UseStaticFiles` méthode d’extension. Pour des raisons de sécurité, inscrivez le middleware uniquement lorsque l’application s’exécute en mode de développement.

Le *webpack.config.js* du fichier `output.publicPath` propriété indique à l’intergiciel (middleware) pour surveiller le `dist` dossier pour les modifications :

[!code-javascript[](../client-side/spa-services/sample/SpaServicesSampleApp/webpack.config.js?range=6,13-16)]

<a name="hot-module-replacement"></a>

## <a name="hot-module-replacement"></a>Remplacement d’un Module à chaud

Pensez à Webpack [remplacement à chaud de Module](https://webpack.js.org/concepts/hot-module-replacement/) fonctionnalité (HMR) comme une évolution de [intergiciel (middleware) de Webpack Dev](#webpack-dev-middleware). HMR présente les mêmes avantages, mais elle simplifie davantage le flux de travail de développement en mettant à jour automatiquement de contenu de la page après la compilation les modifications. À ne pas confondre avec une actualisation du navigateur, ce qui entraînerait une interférence avec l’état en mémoire actuel et la session de débogage de l’application SPA. Il existe un lien direct entre le service de l’intergiciel (middleware) de Webpack développement et le navigateur, ce qui signifie que les modifications sont envoyées au navigateur.

### <a name="prerequisites"></a>Prérequis

Installez les éléments suivants :

* [intergiciel chaud Webpack](https://www.npmjs.com/package/webpack-hot-middleware) package npm :

    ```console
    npm i -D webpack-hot-middleware
    ```

### <a name="configuration"></a>Configuration

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

<a name="routing-helpers"></a>

## <a name="routing-helpers"></a>Programmes d’assistance de routage

Dans la plupart des SPA basé sur ASP.NET Core, vous souhaiterez routage côté client en plus du routage côté serveur. Les systèmes de routage SPA et MVC peuvent travailler indépendamment sans interférence. Il existe, toutefois, un bord cas posant défis : identification des réponses HTTP 404.

Considérez le scénario dans lequel un itinéraire sans extension de `/some/page` est utilisé. Supposons que la demande n’à la correspondance un itinéraire côté serveur, mais son modèle ne correspond pas à un itinéraire côté client. Examinons à présent une demande entrante pour `/images/user-512.png`, lequel attend généralement rechercher un fichier image sur le serveur. Si ce chemin d’accès de la ressource demandée ne correspond pas à toute approche côté serveur ou un fichier statique, il est peu probable que l’application côté client il traiterait — il est généralement conseillé de retourner un code d’état HTTP 404.

### <a name="prerequisites"></a>Prérequis

Installez les éléments suivants :

* Le package npm de routage côté client. À l’aide d’Angular par exemple :

    ```console
    npm i -S @angular/router
    ```

### <a name="configuration"></a>Configuration

Une méthode d’extension nommée `MapSpaFallbackRoute` est utilisé dans le `Configure` méthode :

[!code-csharp[](../client-side/spa-services/sample/SpaServicesSampleApp/Startup.cs?name=mvc-routing-table&highlight=7-9)]

Conseil : Les itinéraires sont évaluées dans l’ordre dans lequel ils sont configurés. Par conséquent, le `default` itinéraire dans l’exemple de code précédent est utilisé pour les critères spéciaux.

<a name="new-project-creation"></a>

## <a name="creating-a-new-project"></a>Création d’un projet

JavaScriptServices fournit des modèles d’application préconfigurés. SpaServices est utilisé dans ces modèles, conjointement avec différentes infrastructures et bibliothèques, telles que Angular et React avec Redux.

Ces modèles peuvent être installés par le biais de l’interface CLI .NET Core en exécutant la commande suivante :

```console
dotnet new --install Microsoft.AspNetCore.SpaTemplates::*
```

Une liste des modèles disponibles s’affiche :

| Modèles                                 | Nom court | Langue | Balises        |
|:------------------------------------------|:-----------|:---------|:------------|
| MVC ASP.NET Core avec Angular             | angular    | [C#]     | MVC/Web/SPA |
| MVC ASP.NET Core avec React.js            | react      | [C#]     | MVC/Web/SPA |
| MVC ASP.NET Core avec React.js et Redux  | reactredux | [C#]     | MVC/Web/SPA |

Pour créer un nouveau projet à l’aide d’un des modèles SPA, incluez le **nom court** du modèle dans le [dotnet nouvelle](/dotnet/core/tools/dotnet-new) commande. La commande suivante crée une application Angular avec ASP.NET Core MVC est configuré pour le côté serveur :

```console
dotnet new angular
```

<a name="runtime-config-mode"></a>

### <a name="set-the-runtime-configuration-mode"></a>Définir le mode de configuration du runtime

Il existe deux modes de configuration de runtime principal :

* **Développement**:
  * Il inclut des mappages de source pour faciliter le débogage.
  * N’Optimisez le code côté client pour les performances.
* **Production**:
  * Exclut les mappages de sources.
  * Optimise le code côté client via le regroupement et minimisation.

ASP.NET Core utilise une variable d’environnement nommée `ASPNETCORE_ENVIRONMENT` pour stocker le mode de configuration. Consultez **[définir l’environnement](xref:fundamentals/environments#set-the-environment)** pour plus d’informations.

### <a name="running-with-net-core-cli"></a>En cours d’exécution avec l’interface CLI .NET Core

Restaurer le NuGet requis et les packages npm en exécutant la commande suivante à la racine du projet :

```console
dotnet restore && npm i
```

Générer et exécuter l’application :

```console
dotnet run
```

Démarrage de l’application sur l’hôte local en fonction de la [mode de configuration de runtime](#runtime-config-mode). Navigation vers `http://localhost:5000` dans le navigateur affiche la page d’accueil.

### <a name="running-with-visual-studio-2017"></a>En cours d’exécution avec Visual Studio 2017

Ouvrez le *.csproj* fichier généré par le [dotnet nouvelle](/dotnet/core/tools/dotnet-new) commande. Les packages NuGet et npm nécessaires sont restaurés automatiquement sur le projet ouvert. Ce processus de restauration peut prendre quelques minutes, et l’application est prête à exécuter lorsqu’elle est terminée. Cliquez sur le bouton d’exécution vert ou appuyez sur `Ctrl + F5`, et le navigateur s’ouvre à la page d’accueil de l’application. L’application s’exécute sur l’hôte local en fonction de la [mode de configuration de runtime](#runtime-config-mode).

<a name="app-testing"></a>

## <a name="testing-the-app"></a>Test de l’application

Les modèles SpaServices sont préconfigurées pour exécuter des tests de côté client à l’aide de [Karma](https://karma-runner.github.io/1.0/index.html) et [Jasmine](https://jasmine.github.io/). Jasmine est une unité de populaires framework de tests pour JavaScript, tandis que Karma est un test runner pour ces tests. KARMA est configuré pour fonctionner avec le [intergiciel (middleware) de Webpack Dev](#webpack-dev-middleware) telles que le développeur n’est pas nécessaire d’arrêter et exécuter le test chaque fois que des modifications. Que ce soit le code s’exécutant sur le cas de test ou le cas de test lui-même, le test s’exécute automatiquement.

À l’aide de l’application Angular par exemple, deux cas de test Jasmine sont déjà fournies pour le `CounterComponent` dans le *counter.component.spec.ts* fichier :

[!code-typescript[](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/app/components/counter/counter.component.spec.ts?range=15-28)]

Ouvrez l’invite de commandes dans le *ClientApp* directory. Exécutez la commande suivante :

```console
npm test
```

Le script lance le testeur Karma, qui lit les paramètres définis dans le *karma.conf.js* fichier. Parmi d’autres paramètres, le *karma.conf.js* identifie les fichiers de test doit être exécuté par le biais de son `files` tableau :

[!code-javascript[](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/test/karma.conf.js?range=4-5,8-11)]

<a name="app-publishing"></a>

## <a name="publishing-the-application"></a>Publication de l’application

Combinant les ressources côté client générés et les artefacts de ASP.NET Core publiées dans un package prêt à déployer peut s’avérer fastidieuse. Heureusement, SpaServices orchestre ce processus d’ensemble de la publication avec une cible MSBuild personnalisée nommée `RunWebpack`:

[!code-xml[](../client-side/spa-services/sample/SpaServicesSampleApp/SpaServicesSampleApp.csproj?range=31-45)]

La cible MSBuild responsabilités est les suivantes :

1. Restaurer les packages npm
1. Créer une build de production à l’échelle des ressources par des tiers, côté client
1. Créer une build de production à l’échelle des ressources côté client personnalisés
1. Copier les actifs Webpack généré dans le dossier de publication

La cible MSBuild est appelée lors de l’exécution :

```console
dotnet publish -c Release
```

## <a name="additional-resources"></a>Ressources supplémentaires

* [Docs angulaires](https://angular.io/docs)
