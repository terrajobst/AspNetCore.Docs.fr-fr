---
title: Utilisation de Gulp dans ASP.NET Core
author: rick-anderson
description: Découvrez comment utiliser Gulp dans ASP.NET Core.
manager: wpickett
ms.author: riande
ms.custom: H1Hack27Feb2017
ms.date: 02/28/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: client-side/using-gulp
ms.openlocfilehash: f776b2025b6ebfeff28d3903aaeac4d7d89665b3
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/06/2018
---
# <a name="use-gulp-in-aspnet-core"></a>Utilisation de Gulp dans ASP.NET Core

Par [Erik Reitan](https://github.com/Erikre), [Scott Addie](https://scottaddie.com), [Michel Roth](https://github.com/danroth27), et [Shayne Boyer](https://twitter.com/spboyer)
basées JavaScript
Dans une application web moderne classique, le processus de génération peut :

* Regrouper et minimiser les fichiers JavaScript et CSS.
* Exécuter les outils pour appeler les tâches de regroupement et de minimisation avant chaque build.
* Compiler lds fichiers LESS ou SASS en CSS.
* Compiler des fichiers CoffeeScript ou TypeScript en JavaScript.

Un *exécuteur de tâches* est un outil qui automatise les tâches de développement de routine et bien plus encore. Visual Studio fournit la prise en charge intégrée pour deux exécuteurs de tâches de base de JavaScript populaires: [Gulp](https://gulpjs.com/) et [Grunt](using-grunt.md).

## <a name="gulp"></a>gulp
Gulp est un toolkit de build en continu basé sur JavaScript pour le code côté client. Il est couramment utilisé pour diffuser des fichiers côté client via une série de processus de déclenchement d’un événement spécifique dans un environnement de génération. Par exemple, Gulp peut être utilisé pour automatiser [le regroupement et la minimisation](bundling-and-minification.md) ou le nettoyage d’un environnement de développement avant une nouvelle build.
Gulp est un toolkit de build en continu basé sur JavaScript pour le code côté client. Il est couramment utilisé pour diffuser des fichiers côté client via une série de processus de déclenchement d’un événement spécifique dans un environnement de génération. Par exemple, Gulp peut être utilisé pour automatiser [le regroupement et la minimisation](bundling-and-minification.md) ou le nettoyage d’un environnement de développement avant une nouvelle build.

Un ensemble de tâches de Gulp est défini dans *gulpfile.js*. Le code JavaScript suivant inclut les modules Gulp et spécifie les chemins d’accès des fichiers référencés dans les tâches à venir :

```javascript
/// <binding Clean='clean' />
"use strict";

var gulp = require("gulp"),
  rimraf = require("rimraf"),
  concat = require("gulp-concat"),
  cssmin = require("gulp-cssmin"),
  uglify = require("gulp-uglify");

var paths = {
  webroot: "./wwwroot/"
};

paths.js = paths.webroot + "js/**/*.js";
paths.minJs = paths.webroot + "js/**/*.min.js";
paths.css = paths.webroot + "css/**/*.css";
paths.minCss = paths.webroot + "css/**/*.min.css";
paths.concatJsDest = paths.webroot + "js/site.min.js";
paths.concatCssDest = paths.webroot + "css/site.min.css";
```

Le code ci-dessus spécifie quels modules de nœud sont requis. La fonction `require` importe chaque module afin que les tâches dépendantes puissent utiliser leurs fonctionnalités. Chaque module importé est assigné à une variable. Les modules peuvent être localisés par nom ou chemin d’accès. Dans cet exemple, les modules nommé `gulp`, `rimraf`, `gulp-concat`, `gulp-cssmin`, et `gulp-uglify` sont récupérés par nom. En outre, une série de chemins d’accès sont créés afin que les emplacements des fichiers CSS et JavaScript puissent être réutilisés et référencés dans les tâches. Le tableau suivant fournit des descriptions des modules inclus dans *gulpfile.js*.

| Nom du module | Description |
| ----------- | ----------- |
| gulp        | Le système de génération de diffusion en continu Gulp. Pour plus d’informations, consultez [gulp](https://www.npmjs.com/package/gulp). |
| rimraf      | Un module Node de suppression. Pour plus d’informations, consultez [rimraf](https://www.npmjs.com/package/rimraf). |
| gulp-concat | Un module qui concatène des fichiers en fonction de caractère de saut de ligne du système d’exploitation. Pour plus d’informations, consultez [gulp-concat](https://www.npmjs.com/package/gulp-concat). |
| gulp-cssmin | Un module qui minimise les fichiers CSS. Pour plus d’informations, consultez [gulp-cssmin](https://www.npmjs.com/package/gulp-cssmin). |
| gulp-uglify | Un module qui minimise les fichiers *.js*. Pour plus d’informations, consultez [gulp-uglify](https://www.npmjs.com/package/gulp-uglify). |

Une fois que les modules requis sont importés, les tâches peuvent être spécifiées. Ici, il existe six tâches enregistrées, représentées par le code suivant :

```javascript
gulp.task("clean:js", function (cb) {
  rimraf(paths.concatJsDest, cb);
});

gulp.task("clean:css", function (cb) {
  rimraf(paths.concatCssDest, cb);
});

gulp.task("clean", ["clean:js", "clean:css"]);

gulp.task("min:js", function () {
  return gulp.src([paths.js, "!" + paths.minJs], { base: "." })
    .pipe(concat(paths.concatJsDest))
    .pipe(uglify())
    .pipe(gulp.dest("."));
});

egulp.task("min:css", function () {
  return gulp.src([paths.css, "!" + paths.minCss])
    .pipe(concat(paths.concatCssDest))
    .pipe(cssmin())
    .pipe(gulp.dest("."));
});

gulp.task("min", ["min:js", "min:css"]);
```

Le tableau suivant fournit une explication sur les tâches spécifiées dans le code ci-dessus :

|Nom de la tâche|Description|
|--- |--- |
|nettoyer : js|Une tâche qui utilise le module Node de suppression rimraf pour supprimer la version réduite du fichier site.js.|
|nettoyer : css|Une tâche qui utilise le module Node de suppression de rimraf pour supprimer la version réduite du fichier site.css.|
|Nettoyer|Une tâche qui appelle la tâche `clean:js`, suivie par la tâche `clean:css`.|
|min:js|Une tâche qui minimise et concatène tous les fichiers .js dans le dossier js. Le. les fichiers min.js sont exclus.|
|min:CSS|Une tâche qui minimise et concatène tous les fichiers .css dans le dossier css. Le. les fichiers min.css sont exclus.|
|min|Une tâche qui appelle la tâche `min:js`suivi par la tâche `min:css`.|
## <a name="running-default-tasks"></a>Exécution des tâches par défaut

Si vous n’avez pas déjà créé une application Web, créez un nouveau projet d’Application Web ASP.NET dans Visual Studio.

1.  Ajouter un nouveau fichier JavaScript à votre projet et nommez-le *gulpfile.js*, puis copiez le code suivant.

    ```javascript
    /// <binding Clean='clean' />
    "use strict";
    
    var gulp = require("gulp"),
      rimraf = require("rimraf"),
      concat = require("gulp-concat"),
      cssmin = require("gulp-cssmin"),
      uglify = require("gulp-uglify");
    
    var paths = {
      webroot: "./wwwroot/"
    };
    
    paths.js = paths.webroot + "js/**/*.js";
    paths.minJs = paths.webroot + "js/**/*.min.js";
    paths.css = paths.webroot + "css/**/*.css";
    paths.minCss = paths.webroot + "css/**/*.min.css";
    paths.concatJsDest = paths.webroot + "js/site.min.js";
    paths.concatCssDest = paths.webroot + "css/site.min.css";
    
    gulp.task("clean:js", function (cb) {
       rimraf(paths.concatJsDest, cb);
    });
        gulp.task("clean:css", function (cb) {
      rimraf(paths.concatCssDest, cb);
    });
    
    gulp.task("clean", ["clean:js", "clean:css"]);
    
    gulp.task("min:js", function () {
      return gulp.src([paths.js, "!" + paths.minJs], { base: "." })
        .pipe(concat(paths.concatJsDest))
        .pipe(uglify())
        .pipe(gulp.dest("."));nu    });
    
    gulp.task("min:css", function () {
      return gulp.src([paths.css, "!" + paths.minCss])
        .pipe(concat(paths.concatCssDest))
        .pipe(cssmin())
        .pipe(gulp.dest("."));
    });
    
    gulp.task("min", ["min:js", "min:css"]);
    ```

2.  Ouvrez le fichier *package.json* (ajoutez-le s'il n'existe pas) et ajoutez le code suivant.

    ```json
    {
      "devDependencies": {
        "gulp": "3.9.1",
        "gulp-concat": "2.6.1",
        "gulp-cssmin": "0.1.7",
        "gulp-uglify": "2.0.1",
         "rimraf": "2.6.1"
      }
    }
    ```

3.  Dans **l’Explorateur de solutions**, cliquez avec le bouton droit sur *gulpfile.js*, puis sélectionnez **Task Runner Explorer**.
    
    ![Ouvrez Task Runner Explorer à partir de l’Explorateur de solutions](using-gulp/_static/02-SolutionExplorer-TaskRunnerExplorer.png)
    
    **Task Runner Explorer** affiche la liste des tâches de Gulp. (Vous devrez peut-être cliquez sur le bouton **Actualiser** qui apparaît à gauche du nom du projet.)
    ![Task Runner Explorer](using-gulp/_static/03-TaskRunnerExplorer.png)
    
    > [!IMPORTANT]
    > Le **Task Runner Explorer** élément de menu contextuel s’affiche uniquement si *gulpfile.js* est dans le répertoire racine du projet.
4.  Sous **Tâches** dans **Task Runner Explorer**, cliquez avec le bouton droit sur **clean**, puis sélectionnez **exécuter** dans le menu contextuel. 

    ![Tâche de nettoyage d’Task Runner Explorer](using-gulp/_static/04-TaskRunner-clean.png)

    **Task Runner Explorer** créera un nouvel onglet nommé **clean** et exécuter la tâche clean telle qu’elle est définie dans *gulpfile.js*.

5.  Cliquer avec le bouton droit sur la tâche **clean**, puis sélectionnez **Bindings**  >  **Before Build**.

    La liaison **avant la build** configure la tâche clean pour qu'elle s’exécute automatiquement avant chaque génération du projet.

Les Bindings définis avec **Task Runner Explorer** sont stockés sous la forme d’un commentaire en haut de votre *gulpfile.js* et sont effectifs seulement dans Visual Studio. Une alternative qui ne nécessite pas Visual Studio consiste à configurer l’exécution automatique des tâches de gulp dans votre fichier *.csproj*. Par exemple, ajoutez ceci dans votre fichier *.csproj* :

```xml<Target Name="MyPreCompileTarget" BeforeTargets="Build">
  <Exec Command="gulp clean" />
</Target>
```

Maintenant la tâche de nettoyage est exécutée lorsque vous exécutez le projet dans Visual Studio ou à partir d’une invite de commandes à l’aide de la commande [dotnet run](/dotnet/core/tools/dotnet-run) (exécuter `npm install` d'abord).

## <a name="defining-and-running-a-new-task"></a>Définition et exécution d'une nouvelle tâche

Pour définir une nouvelle tâche Gulp, modifier *gulpfile.js*.

1.  Ajoutez le code JavaScript suivant à la fin de *gulpfile.js*:

    ```javascript
    gulp.task("first", function () {
      console.log('first task! <-----');
    });
    ```

    Cette tâche est nommée `first`, et elle affiche simplement une chaîne.
2.  Enregistrez *gulpfile.js*.
3.  Dans **l’Explorateur de solutions**, cliquez avec le bouton droit *gulpfile.js*, puis sélectionnez *Task Runner Explorer*.

3.  Dans **l’Explorateur de solutions**, cliquez avec le bouton droit sur *gulpfile.js*, puis sélectionnez *Task Runner Explorer*.

4.  Dans **Task Runner Explorer**, cliquez avec le bouton droit sur **first**, puis sélectionnez **Run**.

    ![Exécutez la tâche first de Task Runner Explorer](using-gulp/_static/06-TaskRunner-First.png)

    Le texte de sortie s’affiche. Pour voir des exemples basés sur des scénarios courants, consultez [Gulp les recettes](#gulp-recipes).
## <a name="defining-and-running-tasks-in-a-series"></a>Définition et l’exécution de tâches dans une série

Lorsque vous exécutez plusieurs tâches, les tâches sont exécutées simultanément par défaut. Toutefois, si vous avez besoin d'exécuter des tâches dans un ordre spécifique, vous devez spécifier quand chaque tâche est terminée, ainsi que les tâches qui dépendent de l’achèvement d’une autre tâche.

1.  Pour définir une série de tâches à exécuter dans l’ordre, remplacez la tâche `first` que vous avez ajoutée ci-dessus dans *gulpfile.js* avec les éléments suivants :

    ```javascript
    gulp.task("series:first", function () {
      console.log('first task! <-----');
    });
 
    gulp.task("series:second", ["series:first"], function () {
      console.log('second task! <-----');
    });
 
    gulp.task("series", ["series:first", "series:second"], function () {});
    ```
 
    Vous disposez maintenant de trois tâches : `series:first`, `series:second`, et `series`. La tâche `series:second` inclut un deuxième paramètre qui spécifie un tableau de tâches à exécuter et terminées avant que la tâche `series:second` tâche exécutera. Comme indiqué dans le code ci-dessus, seule la tâche `series:first` doit être réalisée avant que la tâche `series:second`s'exécute.

2.  Enregistrez *gulpfile.js*.
 
3.  Dans **l’Explorateur de solutions**, cliquez avec le bouton droit sur *gulpfile.js* et sélectionnez **Task Runner Explorer** si elle n’est pas déjà ouverte.

4.  Dans **Task Runner Explorer**, cliquez avec le bouton droit sur **series** et sélectionnez **Run**.

    ![Task Runner Explorer exécuter la tâche de série](using-gulp/_static/07-TaskRunner-Series.png)

## <a name="intellisense"></a>IntelliSense

aIntelliSense fournit l’exécution de code, des descriptions de paramètre et d’autres fonctionnalités pour améliorer la productivité et réduire les erreurs. Les tâches de gulp sont écrites en JavaScript ; Par conséquent, IntelliSense peut fournir une assistance lors du développement. Lorsque vous travaillez avec JavaScript, IntelliSense répertorie les objets, les fonctions, les propriétés et paramètres qui sont disponibles en fonction de votre contexte actuel. Sélectionnez une option de programmation dans la liste contextuelle fournie par IntelliSense pour compléter le code.
![gulp IntelliSense](using-gulp/_static/08-IntelliSense.png)

Pour plus d’informations sur IntelliSense, consultez [JavaScript IntelliSense](https://docs.microsoft.com/visualstudio/ide/javascript-intellisense).

## <a name="development-staging-and-production-environments"></a>Environnements de développement, de staging et de production

  Lorsque Gulp est utilisé pour optimiser les fichiers côté client pour le développement et la production, les fichiers traités sont enregistrés dans des emplacements de développment et de production locaux. Le fichier *_Layout.cshtml* utilise la balise d'assitance **environnement** pour fournir deux versions différentes de fichiers CSS. Une version des fichiers CSS est pour le développement et l’autre version est optimisée pour les tests et la production. Dans Visual Studio 2017, lorsque vous modifiez la variable d'environnement **ASPNETCORE_ENVIRONMENT** `Production`, Visual Studio génère l’application Web et un lien vers les fichiers CSS sous forme réduites. La balise suivante montre les balises d'assistance **environnement** qui contiennent des balises de liens `Development` CSS réduites et fichiers `Staging, Production` pour les fichiers CSS.

```html
r<environment names="Development">
    <script src="~/lib/jquery/dist/jquery.js"></script>
    <script src="~/lib/bootstrap/dist/js/bootstrap.js"></script>
    <script src="~/js/site.js" asp-append-version="true"></script>
</environment>
<environment names="Staging,Production">
    <script src="https://ajax.aspnetcdn.com/ajax/jquery/jquery-2.2.0.min.js"
            asp-fallback-src="~/lib/jquery/dist/jquery.min.js"
            asp-fallback-test="window.jQuery"
           crossorigin="anonymous"
            integrity="sha384-K+ctZQ+LL8q6tP7I94W+qzQsfRV2a+AfHIi9k8z8l9ggpc8X+Ytst4yBo/hH+8Fk">
    </script>
    <script src="https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/bootstrap.min.js"
            asp-fallback-src="~/lib/bootstrap/dist/js/bootstrap.min.js"
            asp-fallback-test="window.jQuery && window.jQuery.fn && window.jQuery.fn.modal"
            crossorigin="anonymous"
            integrity="sha384-Tc5IQib027qvyjSMfHjOMaLkfuWVxZxUPnCJA7l2mCWNIpG9mGCD8wGNIcPD7Txa">
    </script>
    <script src="~/js/site.min.js" asp-append-version="true"></script>
</environment>
```

## <a name="switching-between-environments"></a>Basculer entre les environnements

Pour basculer entre la compilation pour des environnements différents, vous devez modifier la valeur de la variable d'environnement **ASPNETCORE_ENVIRONMENT**.

1.  Dans **Task Runner Explorer**, vérifiez que la tâche**min** ta été définie pour s'exécuter **avant de générer**.

2.  Dans **l’Explorateur de solutions**, cliquez sur le nom du projet et sélectionnez **Propriétés**.

    La feuille de propriétés de l’application Web s’affiche.

3.  Cliquez sur l’onglet **Déboguer**.

4.  Définir la valeur de la variable d’environnement **:Hosting:Environment** à `Production`.

5.  Appuyez sur **F5** pour exécuter l’application dans un navigateur.

6.  Dans la fenêtre du navigateur, cliquez avec le bouton droit sur la page, puis sélectionnez **afficher la Source** pour afficher le code HTML de la page.

    Notez que les liens de la feuille de style pointent vers les versions non minimisées des fichiers CSS.

7.  Fermez le navigateur pour arrêter l’application Web.

8.  Dans Visual Studio, revenir à la feuille de propriétés de l’application Web et modifier la variable d’environnement **Hosting:Environment**pour revenir à `Development`.

9.  Appuyez sur **F5** pour réexécuter l’application dans un navigateur.

10. Dans la fenêtre du navigateur, cliquez avec le bouton droit sur la page, puis sélectionnez **afficher la Source** pour afficher le code HTML de la page.

    Notez que les liens de la feuille de style pointent vers les versions non minifiés des fichiers CSS.

Pour plus d’informations sur les environnements dans ASP.NET Core, consultez [travailler avec plusieurs environnements](../fundamentals/environments.md).

## <a name="task-and-module-details"></a>Détails de tâche et de module

Une tâche Gulp est inscrite avec un nom de fonction. Vous pouvez spécifier des dépendances si d’autres tâches doivent être exécutées avant la tâche actuelle. Des fonctions supplémentaires permettent d'exécuter et de surveiller les tâches Gulp, ainsi que de définir la source (*src*) et la destination (*dest*) des fichiers en cours de modification. Les fonctions principales de l'API Gulp sont les suivantes :

|Gulp (fonction)|Syntaxe|Description|
|---   |--- |--- |
|task  |`gulp.task(name[, deps], fn) { }`|La fonction `task` crée une tâche. Le paramètre `name` définit le nom de la tâche. Le paramètre `deps` contient un tableau de tâches à effectuer avant d’exécuter cette tâche. Le paramètre `fn` représente une fonction de rappel qui effectue les opérations de la tâche.|
|watch |`gulp.watch(glob [, opts], tasks) { }`|La fonction `watch` surveille les fichiers et exécute des tâches lorsqu’une modification de fichier se produit. Le paramètre `glob` est un `string` ou `array` qui détermine les fichiers à surveiller. Le paramètre `opts` fournit la surveillance des options de fichiers supplémentaires.|
|src   |`gulp.src(globs[, options]) { }`|La fonction `src` fournit des fichiers qui correspondent aux valeurs glob. Le paramètre `glob` est une `string` ou un `array` qui détermine les fichiers à lire. Le paramètre `options` fournit des options de fichiers supplémentaires.|
|destination  |`gulp.dest(path[, options]) { }`|La fonction `dest` définit un emplacement dans lequel les fichiers peuvent être écrits. Le paramètre `path` est une chaîne ou une fonction qui détermine le dossier de destination. Le paramètre `options` est un objet qui spécifie les options de dossier de sortie.|

Pour plus d’informations de référence supplémentaires sur l'API de Gulp , consultez [documentation de l'API Gulp](https://github.com/gulpjs/gulp/blob/master/docs/API.md).

## <a name="gulp-recipes"></a>Recettes Gulp

La Communauté Gulp fournit Gulp [recettes](https://github.com/gulpjs/gulp/blob/master/docs/recipes/README.md). Ces recettes sont constituées des tâches Gulp pour résoudre des scénarios courants.

## <a name="additional-resources"></a>Ressources supplémentaires

* [Documentation de gulp](https://github.com/gulpjs/gulp/blob/master/docs/README.md)
* [Reroupement et minimisation dans ASP.NET Core](bundling-and-minification.md)
* [Utiliser Grunt dans ASP.NET Core](using-grunt.md)
