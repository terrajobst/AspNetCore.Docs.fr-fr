---
title: Regrouper et réduire les ressources statiques dans ASP.NET Core
author: scottaddie
description: Découvrez comment optimiser les ressources statiques dans une application Web ASP.NET Core en appliquant des techniques de regroupement et de minimisation.
ms.author: scaddie
ms.custom: mvc
ms.date: 06/17/2019
uid: client-side/bundling-and-minification
ms.openlocfilehash: 7499381a24a2513a4fbd1205f245e624c86647c3
ms.sourcegitcommit: 215954a638d24124f791024c66fd4fb9109fd380
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/18/2019
ms.locfileid: "71080560"
---
# <a name="bundle-and-minify-static-assets-in-aspnet-core"></a>Regrouper et réduire les ressources statiques dans ASP.NET Core

Par [Scott Addie](https://twitter.com/Scott_Addie) et [David pin](https://twitter.com/davidpine7)

Cet article explique les avantages de l’application du regroupement et de la minimisation, notamment la façon dont ces fonctionnalités peuvent être utilisées avec ASP.NET Core Web Apps.

## <a name="what-is-bundling-and-minification"></a>Qu’est-ce que le regroupement et la minimisation ?

Le regroupement et la minimisation sont deux optimisations de performances distinctes que vous pouvez appliquer dans une application Web. Utilisés ensemble, le regroupement et la minimisation améliorent les performances en réduisant le nombre de demandes de serveur et en réduisant la taille des ressources statiques demandées.

Le regroupement et la minimisation améliorent principalement le temps de chargement de la première page de la demande. Une fois qu’une page Web a été demandée, le navigateur met en cache les ressources statiques (JavaScript, CSS et images). Par conséquent, le regroupement et la minimisation n’améliorent pas les performances lorsque vous demandez la même page ou les mêmes pages sur le même site demandant les mêmes ressources. Si l’en-tête Expires n’est pas défini correctement sur les ressources et si le regroupement et la minimisation ne sont pas utilisés, les heuristiques d’actualisation du navigateur marquent les ressources obsolètes après quelques jours. En outre, le navigateur requiert une demande de validation pour chaque ressource. Dans ce cas, le regroupement et la minimisation améliorent les performances même après la première demande de page.

### <a name="bundling"></a>regroupement

Le regroupement combine plusieurs fichiers dans un seul fichier. Le regroupement réduit le nombre de demandes de serveur nécessaires pour afficher une ressource Web, telle qu’une page Web. Vous pouvez créer un nombre quelconque de regroupements individuels spécifiquement pour CSS, JavaScript, etc. Moins de fichiers signifie moins de demandes HTTP du navigateur vers le serveur ou à partir du service qui fournit votre application. Cela a pour effet d’améliorer les performances de chargement de la première page.

### <a name="minification"></a>Réduction

La minimisation supprime les caractères inutiles du code sans modifier la fonctionnalité. Le résultat est une réduction significative de la taille des ressources demandées (telles que CSS, les images et les fichiers JavaScript). Les effets secondaires communs de la minimisation incluent la réduction des noms de variables à un caractère et la suppression des commentaires et des espaces inutiles.

Considérons la fonction JavaScript suivante :

[!code-javascript[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/wwwroot/js/site.js)]

La minimisation réduit la fonction à ce qui suit :

[!code-javascript[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/wwwroot/js/site.min.js)]

En plus de supprimer les commentaires et les espaces superflus, les noms de paramètres et de variables suivants ont été renommés comme suit :

D'origine | Affectation d'un nouveau nom
--- | :---:
`imageTagAndImageID` | `t`
`imageContext` | `a`
`imageElement` | `r`

## <a name="impact-of-bundling-and-minification"></a>Impact du regroupement et de la minimisation

Le tableau suivant présente les différences entre le chargement individuel des ressources et l’utilisation du regroupement et de la minimisation :

Action | Avec B/M | Sans B/M | Modification
--- | :---: | :---: | :---:
Demandes de fichier  | 7   | 18     | 157%
Ko transférés | 156 | 264.68 | 70%
Temps de chargement (MS) | 885 | 2360   | 167%

Les navigateurs sont relativement détaillés en ce qui concerne les en-têtes de requête HTTP. La mesure Total octets envoyés a vu une réduction significative lors du regroupement. Le temps de chargement montre une amélioration significative, mais cet exemple s’est exécuté localement. Des gains de performances plus élevés sont réalisés lorsque vous utilisez le regroupement et la minimisation avec les ressources transférées sur un réseau.

## <a name="choose-a-bundling-and-minification-strategy"></a>Choisir une stratégie de regroupement et de minimisation

Les modèles de projet MVC et Razor Pages fournissent une solution prête à l’emploi pour le regroupement et la minimisation consistant en un fichier de configuration JSON. Des outils tiers, tels que le testeur de tâches [grunt](xref:client-side/using-grunt) , accomplissent les mêmes tâches avec un peu plus de complexité. Un outil tiers est une solution idéale lorsque votre flux de travail de développement nécessite un traitement au-&mdash;delà du regroupement et de la minimisation, tels que le tissu et l’optimisation d’image. En utilisant le regroupement et la minimisation au moment du design, les fichiers minimisés sont créés avant le déploiement de l’application. Le regroupement et le minimisation avant le déploiement offrent l’avantage de réduire la charge du serveur. Toutefois, il est important de reconnaître que le regroupement et la minimisation au moment du design augmentent la complexité de la génération et ne fonctionne qu’avec les fichiers statiques.

## <a name="configure-bundling-and-minification"></a>Configurer le regroupement et la minimisation

::: moniker range="<= aspnetcore-2.0"

Dans ASP.NET Core 2,0 ou version antérieure, les modèles de projet MVC et Razor Pages fournissent un fichier de configuration *bundleconfig. JSON* qui définit les options pour chaque Bundle :

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

Dans ASP.NET Core 2,1 ou version ultérieure, ajoutez un nouveau fichier JSON, nommé *bundleconfig. JSON*, à la racine du projet MVC ou Razor pages. Incluez le code JSON suivant dans ce fichier comme point de départ :

::: moniker-end

[!code-json[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/bundleconfig.json)]

Le fichier *bundleconfig. JSON* définit les options pour chaque bundle. Dans l’exemple précédent, une configuration de regroupement unique est définie pour les fichiers JavaScript personnalisés (*wwwroot/js/site. js*) et StyleSheet (*wwwroot/CSS/site. CSS*).

Les options de configuration sont les suivantes:

* `outputFileName`: Nom du fichier de Bundle à générer. Peut contenir un chemin d’accès relatif à partir du fichier *bundleconfig. JSON* . **Obligatoire**
* `inputFiles`: Tableau de fichiers à regrouper. Il s’agit de chemins d’accès relatifs au fichier de configuration. **facultatif**, * une valeur vide génère un fichier de sortie vide. les modèles [globbing](https://www.tldp.org/LDP/abs/html/globbingref.html) sont pris en charge.
* `minify`: Options de minimisation pour le type de sortie. **facultatif**, *par défaut `minify: { enabled: true }` :*
  * Les options de configuration sont disponibles pour chaque type de fichier de sortie.
    * [Minifier CSS](https://github.com/madskristensen/BundlerMinifier/wiki/cssminifier)
    * [Minifier JavaScript](https://github.com/madskristensen/BundlerMinifier/wiki/JavaScript-Minifier-settings)
    * [Minifier HTML](https://github.com/madskristensen/BundlerMinifier/wiki)
* `includeInProject`: Indicateur précisant s’il faut ajouter les fichiers générés au fichier projet. **facultatif**, *valeur par défaut-false*
* `sourceMap`: Indicateur précisant s’il faut générer un mappage source pour le fichier groupé. **facultatif**, *valeur par défaut-false*
* `sourceMapRootPath`: Chemin d’accès racine pour le stockage du fichier de mappage source généré.

## <a name="build-time-execution-of-bundling-and-minification"></a>Exécution du regroupement et de la minimisation au moment de la génération

Le package NuGet [BuildBundlerMinifier](https://www.nuget.org/packages/BuildBundlerMinifier/) permet l’exécution de regroupement et de minimisation au moment de la génération. Le package injecte des [cibles MSBuild](/visualstudio/msbuild/msbuild-targets) qui s’exécutent au moment de la génération et du nettoyage. Le fichier *bundleconfig. JSON* est analysé par le processus de génération pour produire les fichiers de sortie en fonction de la configuration définie.

> [!NOTE]
> BuildBundlerMinifier appartient à un projet basé sur la communauté sur GitHub pour lequel Microsoft n’offre aucune prise en charge. Les problèmes doivent être classés [ici](https://github.com/madskristensen/BundlerMinifier/issues).

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Ajoutez le package *BuildBundlerMinifier* à votre projet.

Générez le projet. Les éléments suivants s’affichent dans la fenêtre Sortie :

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

Nettoyez le projet. Les éléments suivants s’affichent dans la fenêtre Sortie :

```console
1>------ Clean started: Project: BuildBundlerMinifierApp, Configuration: Debug Any CPU ------
1>
1>Bundler: Cleaning output from bundleconfig.json
1>Bundler: Done cleaning output file from bundleconfig.json
========== Clean: 1 succeeded, 0 failed, 0 skipped ==========
```

# <a name="net-core-clitabnetcore-cli"></a>[CLI .NET Core](#tab/netcore-cli)

Ajoutez le package *BuildBundlerMinifier* à votre projet :

```dotnetcli
dotnet add package BuildBundlerMinifier
```

Si vous utilisez ASP.NET Core 1. x, restaurez le package que vous venez d’ajouter :

```dotnetcli
dotnet restore
```

Générez le projet :

```dotnetcli
dotnet build
```

Les éléments suivants s’affichent :

```console
Microsoft (R) Build Engine version 15.4.8.50001 for .NET Core
Copyright (C) Microsoft Corporation. All rights reserved.


    Bundler: Begin processing bundleconfig.json
    Bundler: Done processing bundleconfig.json
    BuildBundlerMinifierApp -> C:\BuildBundlerMinifierApp\bin\Debug\netcoreapp2.0\BuildBundlerMinifierApp.dll
```

Nettoyez le projet :

```dotnetcli
dotnet clean
```

La sortie suivante apparaît :

```console
Microsoft (R) Build Engine version 15.4.8.50001 for .NET Core
Copyright (C) Microsoft Corporation. All rights reserved.


  Bundler: Cleaning output from bundleconfig.json
  Bundler: Done cleaning output file from bundleconfig.json
```

---

## <a name="ad-hoc-execution-of-bundling-and-minification"></a>Exécution ad hoc du regroupement et de la minimisation

Il est possible d’exécuter les tâches de regroupement et de minimisation sur une base ad hoc, sans générer le projet. Ajoutez le package NuGet [BundlerMinifier. Core](https://www.nuget.org/packages/BundlerMinifier.Core/) à votre projet :

[!code-xml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/BuildBundlerMinifierApp.csproj?range=10)]

> [!NOTE]
> BundlerMinifier. Core appartient à un projet basé sur la communauté sur GitHub pour lequel Microsoft n’offre aucune prise en charge. Les problèmes doivent être classés [ici](https://github.com/madskristensen/BundlerMinifier/issues).

Ce package étend les CLI .NET Core pour inclure l’outil *dotnet-Bundle* . La commande suivante peut être exécutée dans la fenêtre de la console du gestionnaire de package (PMC) ou dans une interface de commande :

```dotnetcli
dotnet bundle
```

> [!IMPORTANT]
> Le gestionnaire de package NuGet ajoute des dépendances au fichier * `<PackageReference />` . csproj en tant que nœuds. La `dotnet bundle` commande est inscrite avec l’CLI .net Core uniquement lorsqu' `<DotNetCliToolReference />` un nœud est utilisé. Modifiez le fichier *. csproj en conséquence.

## <a name="add-files-to-workflow"></a>Ajouter des fichiers au flux de travail

Prenons l’exemple d’un fichier *. CSS personnalisé* supplémentaire qui ressemble à ce qui suit :

[!code-css[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/wwwroot/css/custom.css)]

Pour réduire *Custom. CSS* et le regrouper avec *site. CSS* dans un fichier *site. min. CSS* , ajoutez le chemin d’accès relatif à *bundleconfig. JSON*:

[!code-json[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/bundleconfig2.json?highlight=6)]

> [!NOTE]
> Vous pouvez également utiliser le modèle globbing suivant :
>
> ```json
> "inputFiles": ["wwwroot/**/*(*.css|!(*.min.css))"]
> ```
>
> Ce modèle globbing correspond à tous les fichiers CSS et exclut le modèle de fichier minimisés.

Générez l'application. Ouvrez *site. min. CSS* et notez que le contenu du fichier *Custom. CSS* est ajouté à la fin du fichier.

## <a name="environment-based-bundling-and-minification"></a>Regroupement et minimisation basés sur l’environnement

Il est recommandé d’utiliser les fichiers regroupés et minimisés de votre application dans un environnement de production. Pendant le développement, les fichiers d’origine facilitent le débogage de l’application.

Spécifiez les fichiers à inclure dans vos pages à l’aide du [tag Helper d’environnement](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper) dans vos vues. Le tag Helper d’environnement affiche uniquement son contenu lorsqu’il s’exécute dans des [environnements](xref:fundamentals/environments)spécifiques.

La balise suivante `environment` affiche les fichiers CSS non traités lors de l’exécution dans `Development` l’environnement :

::: moniker range=">= aspnetcore-2.0"

[!code-cshtml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/Pages/_Layout.cshtml?highlight=3&range=21-24)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-cshtml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/Pages/_Layout.cshtml?highlight=3&range=9-12)]

::: moniker-end

La balise suivante `environment` affiche les fichiers CSS regroupés et minimisés lorsqu’ils s’exécutent dans `Development`un environnement autre que. Par exemple, l’exécution `Production` de `Staging` dans ou déclenche le rendu de ces feuilles de style :

::: moniker range=">= aspnetcore-2.0"

[!code-cshtml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/Pages/_Layout.cshtml?highlight=5&range=25-30)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-cshtml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/Pages/_Layout.cshtml?highlight=3&range=13-18)]

::: moniker-end

## <a name="consume-bundleconfigjson-from-gulp"></a>Utilisation de bundleconfig. JSON à partir de Gulp

Dans certains cas, le flux de travail de regroupement et de minimisation d’une application nécessite un traitement supplémentaire. Les exemples incluent l’optimisation des images, la combustion du cache et le traitement des ressources CDN. Pour répondre à ces exigences, vous pouvez convertir le flux de travail de regroupement et de minimisation pour utiliser Gulp.

### <a name="use-the-bundler--minifier-extension"></a>Utiliser l’extension & Minifier du Bundleeur

L’extension [Minifier du bundleeur](https://marketplace.visualstudio.com/items?itemName=MadsKristensen.BundlerMinifier) Visual Studio & gère la conversion en Gulp.

> [!NOTE]
> Le Bundleeur & extension Minifier appartient à un projet basé sur la communauté sur GitHub pour lequel Microsoft n’offre aucune prise en charge. Les problèmes doivent être classés [ici](https://github.com/madskristensen/BundlerMinifier/issues).

Cliquez avec le bouton droit sur le fichier *bundleconfig. JSON* dans Explorateur de solutions et sélectionnez **bundleer & Minifier** > **convertir en Gulp...** :

![Convertir en élément de menu contextuel Gulp](../client-side/bundling-and-minification/_static/convert-to-gulp.png)

Les fichiers *gulpfile. js* et *Package. JSON* sont ajoutés au projet. Les packages de [NPM](https://www.npmjs.com/) de prise en charge répertoriés dans la `devDependencies` section du fichier *Package. JSON* sont installés.

Exécutez la commande suivante dans la fenêtre PMC pour installer Gulp CLI en tant que dépendance globale :

```console
npm i -g gulp-cli
```

Le fichier *gulpfile. js* lit le fichier *bundleconfig. JSON* pour les entrées, les sorties et les paramètres.

[!code-javascript[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/gulpfile.js?range=1-12&highlight=10)]

### <a name="convert-manually"></a>Convertir manuellement

Si Visual Studio et/ou le Bundleer & extension Minifier ne sont pas disponibles, convertissez-les manuellement.

Ajoutez un fichier *Package. JSON* , avec le code `devDependencies`suivant, à la racine du projet :

> [!WARNING]
> Le `gulp-uglify` module ne prend pas en charge ECMAScript (es) 2015/ES6 et versions ultérieures. Installez [Gulp-terser](https://www.npmjs.com/package/gulp-terser) au lieu `gulp-uglify` de pour utiliser ES2015/ES6 ou une version ultérieure.

[!code-json[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/package.json?range=5-13)]

Installez les dépendances en exécutant la commande suivante au même niveau que *Package. JSON*:

```console
npm i
```

Installez Gulp CLI en tant que dépendance globale :

```console
npm i -g gulp-cli
```

Copiez le fichier *gulpfile. js* ci-dessous sur la racine du projet :

[!code-javascript[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/gulpfile.js?range=1-11,14-)]

### <a name="run-gulp-tasks"></a>Exécuter des tâches Gulp

Pour déclencher la tâche de minimisation Gulp avant la génération du projet dans Visual Studio, ajoutez la [cible MSBuild](/visualstudio/msbuild/msbuild-targets) suivante au fichier *. csproj :

[!code-xml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/BuildBundlerMinifierApp.csproj?range=14-16)]

Dans cet exemple, toutes les tâches définies dans `MyPreCompileTarget` la cible s’exécutent avant la `Build` cible prédéfinie. Une sortie similaire à ce qui suit apparaît dans la fenêtre sortie de Visual Studio :

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


## <a name="additional-resources"></a>Ressources supplémentaires

* [Utiliser Grunt](xref:client-side/using-grunt)
* [Utiliser plusieurs environnements](xref:fundamentals/environments)
* [Les Tag Helpers](xref:mvc/views/tag-helpers/intro)
