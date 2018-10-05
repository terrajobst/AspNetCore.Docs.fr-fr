---
title: Regrouper et minimiser les ressources statiques dans ASP.NET Core
author: scottaddie
description: Découvrez comment optimiser les ressources statiques dans une application web ASP.NET Core en appliquant les techniques de regroupement et minimisation.
ms.author: scaddie
ms.custom: mvc
ms.date: 10/04/2018
uid: client-side/bundling-and-minification
ms.openlocfilehash: 152f3c810b587d734c1b1076a09ea38d13872e2d
ms.sourcegitcommit: 7890dfb5a8f8c07d813f166d3ab0c263f893d0c6
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/04/2018
ms.locfileid: "48795403"
---
# <a name="bundle-and-minify-static-assets-in-aspnet-core"></a>Regrouper et minimiser les ressources statiques dans ASP.NET Core

Par [Scott Addie](https://twitter.com/Scott_Addie) et [David PIN](https://twitter.com/davidpine7)

Cet article explique les avantages de l’application de regroupement et minimisation, y compris comment ces fonctionnalités peuvent être utilisées avec les applications web ASP.NET Core.

## <a name="what-is-bundling-and-minification"></a>Qu’est le regroupement et minimisation

Regroupement et minimisation sont deux optimisations de performances distincts que vous pouvez appliquer dans une application web. Utilisés ensemble, regroupement et minimisation améliorent les performances en réduisant le nombre de demandes de serveur et de réduire la taille des actifs statiques demandés.

Regroupement et minimisation principalement améliorent la première fois de charge de demande de page. Une fois qu’une page web a été demandée, le navigateur met en cache les ressources statiques (JavaScript, CSS et images). Par conséquent, regroupement et minimisation ne pas améliorer les performances lors de la demande de même, les pages, sur le même site demande les mêmes ressources. Si l’expiration en-tête n’est pas défini correctement sur les ressources et si le regroupement et minimisation n’est pas utilisé, heuristique d’actualisation du navigateur marque les ressources obsolètes après quelques jours. En outre, le navigateur requiert une demande de validation pour chaque élément multimédia. Dans ce cas, regroupement et minimisation fournissent une amélioration des performances même après la première demande de page.

### <a name="bundling"></a>Le regroupement

Regroupement de combiner plusieurs fichiers dans un seul fichier. Regroupement réduit le nombre de demandes de serveur qui sont nécessaires à l’affichage d’une ressource web, telle qu’une page web. Vous pouvez créer n’importe quel nombre de regroupements individuels spécifiquement pour CSS, JavaScript, etc. Moins de fichiers signifie moins de demandes HTTP à partir du navigateur au serveur ou à partir du service fournissant à votre application. Il en résulte amélioration des performances de charge première page.

### <a name="minification"></a>Minimisation

Minimisation supprime les caractères inutiles à partir du code sans dégrader les fonctionnalités. Il en résulte une réduction de taille importante dans les ressources demandées (par exemple, images, fichiers CSS et JavaScript). Les effets secondaires communs de minimisation incluent raccourcir les noms de variables pour un caractère et de suppression de commentaires et espace blanc inutile.

Considérez la fonction JavaScript suivante :

[!code-javascript[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/wwwroot/js/site.js)]

Minimisation réduit la fonction à ce qui suit :

[!code-javascript[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/wwwroot/js/site.min.js)]

Outre la suppression de commentaires et l’espace blanc inutile, les noms de paramètre et sa variable suivants ont été renommés comme suit :

D'origine | Affectation d'un nouveau nom
--- | :---:
`imageTagAndImageID` | `t`
`imageContext` | `a`
`imageElement` | `r`

## <a name="impact-of-bundling-and-minification"></a>Impact de regroupement et minimisation

Le tableau suivant présente les différences entre le chargement de ressources et à l’aide de regroupement et minimisation individuellement :

Action | Avec B/M | Sans B/M | Modification
--- | :---: | :---: | :---:
Demandes de fichiers  | 7   | 18     | 157%
Ko transférée | 156 | 264.68 | 70%
Temps de chargement (ms) | 885 | 2360   | 167%

Les navigateurs sont assez détaillés en ce qui concerne les en-têtes de requête HTTP. Le nombre total d’octets envoyés métrique vu une réduction significative lors du regroupement. Le temps de chargement montre une amélioration significative, mais cet exemple a été exécuté localement. Gains de performances sont réalisés lorsque des composants à l’aide de regroupement et minimisation transférées sur un réseau.

## <a name="choose-a-bundling-and-minification-strategy"></a>Choisir une stratégie de regroupement et minimisation

Les modèles de projet MVC et les Pages Razor fournissent une solution out-of-the-box pour le regroupement et minimisation consistant en un fichier de configuration JSON. Outils tiers, tels que le [Gulp](xref:client-side/using-gulp) et [Grunt](xref:client-side/using-grunt) exécuteurs de tâches, d’accomplir les mêmes tâches avec un peu plus complexe. Un outil tiers est idéaux lorsque votre flux de travail de développement requiert un traitement au-delà de regroupement et minimisation&mdash;telles que l’optimisation lint et image. À l’aide de regroupement et minimisation au moment du design, les fichiers réduits sont créés avant le déploiement de l’application. Bundles et minimisation avant le déploiement offre l’avantage d’une charge serveur réduite. Toutefois, il est important de reconnaître ce regroupement au moment du design et de minimisation augmente la complexité de la build et fonctionne uniquement avec les fichiers statiques.

## <a name="configure-bundling-and-minification"></a>Configurer le regroupement et minimisation

Les modèles de projet MVC et les Pages Razor fournissent une *bundleconfig.json* fichier de configuration qui définit les options pour chaque regroupement. Par défaut, une configuration de lot unique est définie pour le code JavaScript personnalisé (*wwwroot/js/site.js*) et la feuille de style (*wwwroot/css/site.css*) fichiers :

[!code-json[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/bundleconfig.json)]

Options de configuration sont les suivantes :

* `outputFileName`: Nom de fichier d’offre groupée de sortie. Peut contenir un chemin d’accès relatif à partir de la *bundleconfig.json* fichier. **Obligatoire**
* `inputFiles`: Tableau de fichiers à regrouper. Il s’agit des chemins d’accès relatifs au fichier de configuration. **facultatif**, * une valeur vide résulte en un fichier de sortie vide. [utilisation des caractères génériques](http://www.tldp.org/LDP/abs/html/globbingref.html) modèles sont pris en charge.
* `minify`: Les options de la minimisation, pour le type de sortie. **facultatif**, *par défaut : `minify: { enabled: true }`*
  * Options de configuration sont disponibles par type de fichier de sortie.
    * [Minimisation CSS](https://github.com/madskristensen/BundlerMinifier/wiki/cssminifier)
    * [Minimisation de JavaScript](https://github.com/madskristensen/BundlerMinifier/wiki/JavaScript-Minifier-settings)
    * [Minimisation de HTML](https://github.com/madskristensen/BundlerMinifier/wiki)
* `includeInProject`: Indicateur précisant s’il faut ajouter les fichiers générés au fichier projet. **facultatif**, *par défaut : false*
* `sourceMap`: Indicateur spécifiant s’il faut générer une carte de code source pour le fichier regroupé. **facultatif**, *par défaut : false*
* `sourceMapRootPath`: Le chemin d’accès racine pour stocker le fichier de mappage de source généré.

## <a name="build-time-execution-of-bundling-and-minification"></a>Exécution du moment de la génération de regroupement et minimisation

Le [BuildBundlerMinifier](https://www.nuget.org/packages/BuildBundlerMinifier/) package NuGet permet l’exécution de regroupement et minimisation au moment de la génération. Le package injecte [cibles MSBuild](/visualstudio/msbuild/msbuild-targets) qui exécuté à la génération et l’heure propre. Le *bundleconfig.json* fichier est analysé par le processus de génération pour générer les fichiers de sortie selon la configuration définie.

> [!NOTE]
> BuildBundlerMinifier appartient à un projet communautaire sur GitHub pour laquelle Microsoft ne fournit aucun support. Soumettre des problèmes [ici](https://github.com/madskristensen/BundlerMinifier/issues).

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Ajouter le *BuildBundlerMinifier* package à votre projet.

Générez le projet. Le message suivant s’affiche dans la fenêtre Sortie :

```console
1>------ Build started: Project: BuildBundlerMinifierApp, Configuration: Debug Any CPU ------
1>
1>Bundler: Begin processing bundleconfig.json
1>  Minified wwwroot/css/site.min.css
1>  Minified wwwroot/js/site.min.js
1>Bundler: Done processing bundleconfig.json
1>BuildBundlerMinifierApp -> C:\BuildBundlerMinifierApp\bin\Debug\netcoreapp2.0\BuildBundlerMinifierApp.dll
========== Build: 1 succeeded, 0 failed, 0 up-to-date, 0 skipped ==========
```

Nettoyer le projet. Le message suivant s’affiche dans la fenêtre Sortie :

```console
1>------ Clean started: Project: BuildBundlerMinifierApp, Configuration: Debug Any CPU ------
1>
1>Bundler: Cleaning output from bundleconfig.json
1>Bundler: Done cleaning output file from bundleconfig.json
========== Clean: 1 succeeded, 0 failed, 0 skipped ==========
```

# <a name="net-core-clitabnetcore-cli"></a>[CLI .NET Core](#tab/netcore-cli)

Ajouter le *BuildBundlerMinifier* package à votre projet :

```console
dotnet add package BuildBundlerMinifier
```

Si vous utilisez ASP.NET Core 1.x, restaurer le package nouvellement ajouté :

```console
dotnet restore
```

Générez le projet :

```console
dotnet build
```

Ce qui suit s’affiche :

```console
Microsoft (R) Build Engine version 15.4.8.50001 for .NET Core
Copyright (C) Microsoft Corporation. All rights reserved.


    Bundler: Begin processing bundleconfig.json
    Bundler: Done processing bundleconfig.json
    BuildBundlerMinifierApp -> C:\BuildBundlerMinifierApp\bin\Debug\netcoreapp2.0\BuildBundlerMinifierApp.dll
```

Nettoyer le projet :

```console
dotnet clean
```

La sortie suivante s’affiche :

```console
Microsoft (R) Build Engine version 15.4.8.50001 for .NET Core
Copyright (C) Microsoft Corporation. All rights reserved.


  Bundler: Cleaning output from bundleconfig.json
  Bundler: Done cleaning output file from bundleconfig.json
```

---

## <a name="ad-hoc-execution-of-bundling-and-minification"></a>Exécution ad hoc de regroupement et minimisation

Il est possible d’exécuter les tâches de regroupement et minimisation sur une base ad hoc, sans générer le projet. Ajouter le [BundlerMinifier.Core](https://www.nuget.org/packages/BundlerMinifier.Core/) package NuGet à votre projet :

[!code-xml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/BuildBundlerMinifierApp.csproj?range=10)]

> [!NOTE]
> BundlerMinifier.Core appartient à un projet communautaire sur GitHub pour laquelle Microsoft ne fournit aucun support. Soumettre des problèmes [ici](https://github.com/madskristensen/BundlerMinifier/issues).

Ce package étend l’interface CLI .NET Core pour inclure le *dotnet-bundle* outil. La commande suivante peut être exécutée dans la fenêtre de Console de gestionnaire de Package (PMC) ou dans un interpréteur de commandes :

```console
dotnet bundle
```

> [!IMPORTANT]
> Gestionnaire de Package NuGet ajoute des dépendances au fichier *.csproj comme `<PackageReference />` nœuds. Le `dotnet bundle` commande est inscrit avec l’interface CLI .NET Core uniquement quand un `<DotNetCliToolReference />` nœud est utilisé. Modifiez le fichier *.csproj en conséquence.

## <a name="add-files-to-workflow"></a>Ajouter des fichiers à un flux de travail

Prenons un exemple dans lequel un autre *custom.css* fichier est ajouté, qui ressemble à ce qui suit :

[!code-css[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/wwwroot/css/custom.css)]

À minimiser *custom.css* et la regrouper avec *site.css* dans un *site.min.css* , ajoutez le chemin d’accès relatif à *bundleconfig.json*:

[!code-json[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/bundleconfig2.json?highlight=6)]

> [!NOTE]
> Vous pouvez également le modèle d’utilisation des caractères génériques suivants peut être utilisé :
>
> ```json
> "inputFiles": ["wwwroot/**/*(*.css|!(*.min.css))"]
> ```
>
> Ce modèle de globbing correspond à tous les fichiers CSS et exclut le modèle de fichier réduit.

Générez l'application. Ouvrez *site.min.css* et notez le contenu de *custom.css* est ajouté à la fin du fichier.

## <a name="environment-based-bundling-and-minification"></a>Regroupement et minimisation basé sur l’environnement

Comme meilleure pratique, les fichiers regroupés et minimisés de votre application doivent être utilisés dans un environnement de production. Pendant le développement, les fichiers d’origine apporter pour faciliter le débogage de l’application.

Spécifier les fichiers à inclure dans vos pages à l’aide de la [Tag Helper d’environnement](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper) dans vos vues. Le Tag Helper environnement restitue uniquement son contenu lors de l’exécution en particulier [environnements](xref:fundamentals/environments).

Ce qui suit `environment` balise restitue les fichiers CSS non traités lors de l’exécution le `Development` environnement :

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

[!code-cshtml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/Pages/_Layout.cshtml?highlight=3&range=21-24)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

[!code-cshtml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/Pages/_Layout.cshtml?highlight=3&range=9-12)]

---

Ce qui suit `environment` effectue le rendu lors de l’exécution dans un environnement autre que les fichiers CSS regroupés et minimisés `Development`. Par exemple, en cours d’exécution `Production` ou `Staging` déclenche le rendu de ces feuilles de style :

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

[!code-cshtml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/Pages/_Layout.cshtml?highlight=5&range=25-30)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

[!code-cshtml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/Pages/_Layout.cshtml?highlight=3&range=13-18)]

---

## <a name="consume-bundleconfigjson-from-gulp"></a>Consommer bundleconfig.json à partir de Gulp

Il existe des cas dans lequel les flux de travail regroupement et minimisation d’une application nécessite un traitement supplémentaire. Exemples : optimisation d’image, cache busting et traitement actif CDN. Pour satisfaire ces exigences, vous pouvez convertir le flux de travail de regroupement et minimisation pour utiliser Gulp.

### <a name="use-the-bundler--minifier-extension"></a>Utiliser l’extension Bundler & Minifier

Visual Studio [Bundler & Minifier](https://marketplace.visualstudio.com/items?itemName=MadsKristensen.BundlerMinifier) extension gère la conversion en Gulp.

> [!NOTE]
> L’extension Bundler & Minifier appartient à un projet communautaire sur GitHub pour laquelle Microsoft ne fournit aucun support. Soumettre des problèmes [ici](https://github.com/madskristensen/BundlerMinifier/issues).

Avec le bouton droit le *bundleconfig.json* fichier dans l’Explorateur de solutions et sélectionnez **Bundler & Minifier** > **convertir à Gulp...** :

![Élément de menu contextuel de convertir le Gulp](../client-side/bundling-and-minification/_static/convert-to-gulp.png)

Le *gulpfile.js* et *package.json* fichiers sont ajoutés au projet. La prise en charge [npm](https://www.npmjs.com/) packages répertoriés dans le *package.json* du fichier `devDependencies` section sont installés.

Exécutez la commande suivante dans la fenêtre PMC pour installer l’interface CLI de Gulp comme dépendance globale :

```console
npm i -g gulp-cli
```

Le *gulpfile.js* lectures de fichiers le *bundleconfig.json* fichier pour les entrées, sorties et des paramètres.

[!code-javascript[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/gulpfile.js?range=1-12&highlight=10)]

### <a name="convert-manually"></a>Convertir manuellement

Si Visual Studio et/ou l’extension Bundler & Minifier ne sont pas disponible, convertir manuellement.

Ajouter un *package.json* fichier, par le code suivant `devDependencies`, à la racine du projet :

[!code-json[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/package.json?range=5-13)]

Installer les dépendances en exécutant la commande suivante au même niveau que *package.json*:

```console
npm i
```

Installez l’interface CLI de Gulp comme une dépendance globale :

```console
npm i -g gulp-cli
```

Copie le *gulpfile.js* fichier ci-dessous à la racine du projet :

[!code-javascript[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/gulpfile.js?range=1-11,14-)]

### <a name="run-gulp-tasks"></a>Exécuter des tâches Gulp

Pour déclencher la tâche de minimisation Gulp avant la génération du projet dans Visual Studio, ajoutez le code suivant [cible MSBuild](/visualstudio/msbuild/msbuild-targets) au fichier *.csproj :

[!code-xml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/BuildBundlerMinifierApp.csproj?range=14-16)]

Dans cet exemple, toutes les tâches définies dans le `MyPreCompileTarget` cible à exécuter avant le texte prédéfini `Build` cible. Sortie similaire à ce qui suit s’affiche dans la fenêtre de sortie de Visual Studio :

```console
1>------ Build started: Project: BuildBundlerMinifierApp, Configuration: Debug Any CPU ------
1>BuildBundlerMinifierApp -> C:\BuildBundlerMinifierApp\bin\Debug\netcoreapp2.0\BuildBundlerMinifierApp.dll
1>[14:17:49] Using gulpfile C:\BuildBundlerMinifierApp\gulpfile.js
1>[14:17:49] Starting 'min:js'...
1>[14:17:49] Starting 'min:css'...
1>[14:17:49] Starting 'min:html'...
1>[14:17:49] Finished 'min:js' after 83 ms
1>[14:17:49] Finished 'min:css' after 88 ms
========== Build: 1 succeeded, 0 failed, 0 up-to-date, 0 skipped ==========
```

Task Runner Explorer de Visual Studio peut également servir pour lier des tâches Gulp à des événements spécifiques de Visual Studio. Consultez [en cours d’exécution des tâches par défaut](xref:client-side/using-gulp#running-default-tasks) pour obtenir des instructions sur cette méthode.

## <a name="additional-resources"></a>Ressources supplémentaires

* [Utiliser Gulp](xref:client-side/using-gulp)
* [Utiliser Grunt](xref:client-side/using-grunt)
* [Utiliser plusieurs environnements](xref:fundamentals/environments)
* [Les Tag Helpers](xref:mvc/views/tag-helpers/intro)
