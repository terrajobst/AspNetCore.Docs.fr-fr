---
title: Utiliser Gulp dans ASP.NET Core
author: rick-anderson
description: Découvrez comment utiliser Gulp dans ASP.NET Core.
ms.author: riande
ms.custom: H1Hack27Feb2017
ms.date: 10/04/2018
uid: client-side/using-gulp
ms.openlocfilehash: 4f383be0498b5b861bd43cc0f0685b1e62c7571b
ms.sourcegitcommit: 7890dfb5a8f8c07d813f166d3ab0c263f893d0c6
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/04/2018
ms.locfileid: "48795516"
---
# <a name="use-gulp-in-aspnet-core"></a><span data-ttu-id="03653-103">Utiliser Gulp dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="03653-103">Use Gulp in ASP.NET Core</span></span>

<span data-ttu-id="03653-104">Par [Scott Addie](https://scottaddie.com), [Shayne Boyer](https://twitter.com/spboyer), et [David PIN](https://twitter.com/davidpine7)</span><span class="sxs-lookup"><span data-stu-id="03653-104">By [Scott Addie](https://scottaddie.com), [Shayne Boyer](https://twitter.com/spboyer), and [David Pine](https://twitter.com/davidpine7)</span></span>

<span data-ttu-id="03653-105">Dans une application web moderne typique, le processus de génération peut :</span><span class="sxs-lookup"><span data-stu-id="03653-105">In a typical modern web app, the build process might:</span></span>

* <span data-ttu-id="03653-106">Regrouper et minimiser les fichiers JavaScript et CSS.</span><span class="sxs-lookup"><span data-stu-id="03653-106">Bundle and minify JavaScript and CSS files.</span></span>
* <span data-ttu-id="03653-107">Exécuter les outils pour appeler les tâches de regroupement et de minimisation avant chaque build.</span><span class="sxs-lookup"><span data-stu-id="03653-107">Run tools to call the bundling and minification tasks before each build.</span></span>
* <span data-ttu-id="03653-108">Compilez les fichiers LESS ou SASS en fichiers CSS.</span><span class="sxs-lookup"><span data-stu-id="03653-108">Compile LESS or SASS files to CSS.</span></span>
* <span data-ttu-id="03653-109">Compiler des fichiers CoffeeScript ou TypeScript en JavaScript.</span><span class="sxs-lookup"><span data-stu-id="03653-109">Compile CoffeeScript or TypeScript files to JavaScript.</span></span>

<span data-ttu-id="03653-110">Un *exécuteur de tâches* est un outil qui automatise les tâches de développement de routine et bien plus encore.</span><span class="sxs-lookup"><span data-stu-id="03653-110">A *task runner* is a tool which automates these routine development tasks and more.</span></span> <span data-ttu-id="03653-111">Visual Studio fournit la prise en charge intégrée pour deux exécuteurs de tâches de base de JavaScript populaires: [Gulp](https://gulpjs.com/) et [Grunt](using-grunt.md).</span><span class="sxs-lookup"><span data-stu-id="03653-111">Visual Studio provides built-in support for two popular JavaScript-based task runners: [Gulp](https://gulpjs.com/) and [Grunt](using-grunt.md).</span></span>

## <a name="gulp"></a><span data-ttu-id="03653-112">Gulp</span><span class="sxs-lookup"><span data-stu-id="03653-112">Gulp</span></span>

<span data-ttu-id="03653-113">Gulp est un toolkit de build en continu basé sur JavaScript pour le code côté client.</span><span class="sxs-lookup"><span data-stu-id="03653-113">Gulp is a JavaScript-based streaming build toolkit for client-side code.</span></span> <span data-ttu-id="03653-114">Il est couramment utilisé pour diffuser des fichiers côté client via une série de processus de déclenchement d’un événement spécifique dans un environnement de génération.</span><span class="sxs-lookup"><span data-stu-id="03653-114">It's commonly used to stream client-side files through a series of processes when a specific event is triggered in a build environment.</span></span> <span data-ttu-id="03653-115">Par exemple, Gulp peut être utilisé pour automatiser [le regroupement et la minimisation](bundling-and-minification.md) ou le nettoyage d’un environnement de développement avant une nouvelle build.</span><span class="sxs-lookup"><span data-stu-id="03653-115">For instance, Gulp can be used to automate [bundling and minification](bundling-and-minification.md) or the cleansing of a development environment before a new build.</span></span>

<span data-ttu-id="03653-116">Un ensemble de tâches Gulp est défini dans *gulpfile.js*.</span><span class="sxs-lookup"><span data-stu-id="03653-116">A set of Gulp tasks is defined in *gulpfile.js*.</span></span> <span data-ttu-id="03653-117">Le code JavaScript suivant inclut les modules Gulp et spécifie les chemins d’accès de fichier pour être référencé dans les tâches à venir :</span><span class="sxs-lookup"><span data-stu-id="03653-117">The following JavaScript includes Gulp modules and specifies file paths to be referenced within the forthcoming tasks:</span></span>

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

<span data-ttu-id="03653-118">Le code ci-dessus spécifie quels modules de nœud sont requis.</span><span class="sxs-lookup"><span data-stu-id="03653-118">The above code specifies which Node modules are required.</span></span> <span data-ttu-id="03653-119">La fonction `require` importe chaque module afin que les tâches dépendantes puissent utiliser leurs fonctionnalités.</span><span class="sxs-lookup"><span data-stu-id="03653-119">The `require` function imports each module so that the dependent tasks can utilize their features.</span></span> <span data-ttu-id="03653-120">Chaque module importé est assigné à une variable.</span><span class="sxs-lookup"><span data-stu-id="03653-120">Each of the imported modules is assigned to a variable.</span></span> <span data-ttu-id="03653-121">Les modules peuvent être localisés par nom ou chemin d’accès.</span><span class="sxs-lookup"><span data-stu-id="03653-121">The modules can be located either by name or path.</span></span> <span data-ttu-id="03653-122">Dans cet exemple, les modules nommé `gulp`, `rimraf`, `gulp-concat`, `gulp-cssmin`, et `gulp-uglify` sont récupérés par nom.</span><span class="sxs-lookup"><span data-stu-id="03653-122">In this example, the modules named `gulp`, `rimraf`, `gulp-concat`, `gulp-cssmin`, and `gulp-uglify` are retrieved by name.</span></span> <span data-ttu-id="03653-123">En outre, une série de chemins d’accès sont créés afin que les emplacements des fichiers CSS et JavaScript puissent être réutilisés et référencés dans les tâches.</span><span class="sxs-lookup"><span data-stu-id="03653-123">Additionally, a series of paths are created so that the locations of CSS and JavaScript files can be reused and referenced within the tasks.</span></span> <span data-ttu-id="03653-124">Le tableau suivant fournit des descriptions des modules inclus dans *gulpfile.js*.</span><span class="sxs-lookup"><span data-stu-id="03653-124">The following table provides descriptions of the modules included in *gulpfile.js*.</span></span>

| <span data-ttu-id="03653-125">Nom du module</span><span class="sxs-lookup"><span data-stu-id="03653-125">Module Name</span></span> | <span data-ttu-id="03653-126">Description</span><span class="sxs-lookup"><span data-stu-id="03653-126">Description</span></span> |
| ----------- | ----------- |
| <span data-ttu-id="03653-127">gulp</span><span class="sxs-lookup"><span data-stu-id="03653-127">gulp</span></span>        | <span data-ttu-id="03653-128">Système de génération en continu Gulp.</span><span class="sxs-lookup"><span data-stu-id="03653-128">The Gulp streaming build system.</span></span> <span data-ttu-id="03653-129">Pour plus d’informations, consultez [gulp](https://www.npmjs.com/package/gulp).</span><span class="sxs-lookup"><span data-stu-id="03653-129">For more information, see [gulp](https://www.npmjs.com/package/gulp).</span></span> |
| <span data-ttu-id="03653-130">rimraf</span><span class="sxs-lookup"><span data-stu-id="03653-130">rimraf</span></span>      | <span data-ttu-id="03653-131">Un module Node de suppression.</span><span class="sxs-lookup"><span data-stu-id="03653-131">A Node deletion module.</span></span> <span data-ttu-id="03653-132">Pour plus d’informations, consultez [rimraf](https://www.npmjs.com/package/rimraf).</span><span class="sxs-lookup"><span data-stu-id="03653-132">For more information, see [rimraf](https://www.npmjs.com/package/rimraf).</span></span> |
| <span data-ttu-id="03653-133">concat de gulp</span><span class="sxs-lookup"><span data-stu-id="03653-133">gulp-concat</span></span> | <span data-ttu-id="03653-134">Un module qui concatène les fichiers en fonction de caractère de saut de ligne du système d’exploitation.</span><span class="sxs-lookup"><span data-stu-id="03653-134">A module that concatenates files based on the operating system's newline character.</span></span> <span data-ttu-id="03653-135">Pour plus d’informations, consultez [gulp-concat](https://www.npmjs.com/package/gulp-concat).</span><span class="sxs-lookup"><span data-stu-id="03653-135">For more information, see [gulp-concat](https://www.npmjs.com/package/gulp-concat).</span></span> |
| <span data-ttu-id="03653-136">cssmin de gulp</span><span class="sxs-lookup"><span data-stu-id="03653-136">gulp-cssmin</span></span> | <span data-ttu-id="03653-137">Un module qui minimise les fichiers CSS.</span><span class="sxs-lookup"><span data-stu-id="03653-137">A module that minifies CSS files.</span></span> <span data-ttu-id="03653-138">Pour plus d’informations, consultez [gulp-cssmin](https://www.npmjs.com/package/gulp-cssmin).</span><span class="sxs-lookup"><span data-stu-id="03653-138">For more information, see [gulp-cssmin](https://www.npmjs.com/package/gulp-cssmin).</span></span> |
| <span data-ttu-id="03653-139">uglify de gulp</span><span class="sxs-lookup"><span data-stu-id="03653-139">gulp-uglify</span></span> | <span data-ttu-id="03653-140">Un module qui minimise les fichiers *.js*.</span><span class="sxs-lookup"><span data-stu-id="03653-140">A module that minifies *.js* files.</span></span> <span data-ttu-id="03653-141">Pour plus d’informations, consultez [uglify de gulp](https://www.npmjs.com/package/gulp-uglify).</span><span class="sxs-lookup"><span data-stu-id="03653-141">For more information, see [gulp-uglify](https://www.npmjs.com/package/gulp-uglify).</span></span> |

<span data-ttu-id="03653-142">Une fois que les modules requis sont importés, les tâches peuvent être spécifiées.</span><span class="sxs-lookup"><span data-stu-id="03653-142">Once the requisite modules are imported, the tasks can be specified.</span></span> <span data-ttu-id="03653-143">Ici, il existe six tâches enregistrées, représentées par le code suivant :</span><span class="sxs-lookup"><span data-stu-id="03653-143">Here there are six tasks registered, represented by the following code:</span></span>

```javascript
gulp.task("clean:js", done => rimraf(paths.concatJsDest, done));
gulp.task("clean:css", done => rimraf(paths.concatCssDest, done));
gulp.task("clean", gulp.series(["clean:js", "clean:css"]));

gulp.task("min:js", () => {
  return gulp.src([paths.js, "!" + paths.minJs], { base: "." })
    .pipe(concat(paths.concatJsDest))
    .pipe(uglify())
    .pipe(gulp.dest("."));
});

gulp.task("min:css", () => {
  return gulp.src([paths.css, "!" + paths.minCss])
    .pipe(concat(paths.concatCssDest))
    .pipe(cssmin())
    .pipe(gulp.dest("."));
});

gulp.task("min", gulp.series(["min:js", "min:css"]));
    
// A 'default' task is required by Gulp v4
gulp.task("default", gulp.series(["min"]));
```

---

<span data-ttu-id="03653-144">Le tableau suivant fournit une explication des tâches spécifiées dans le code ci-dessus :</span><span class="sxs-lookup"><span data-stu-id="03653-144">The following table provides an explanation of the tasks specified in the code above:</span></span>

|<span data-ttu-id="03653-145">Nom de la tâche</span><span class="sxs-lookup"><span data-stu-id="03653-145">Task Name</span></span>|<span data-ttu-id="03653-146">Description</span><span class="sxs-lookup"><span data-stu-id="03653-146">Description</span></span>|
|--- |--- |
|<span data-ttu-id="03653-147">nettoyer : js</span><span class="sxs-lookup"><span data-stu-id="03653-147">clean:js</span></span>|<span data-ttu-id="03653-148">Une tâche qui utilise le module Node de suppression rimraf pour supprimer la version réduite du fichier site.js.</span><span class="sxs-lookup"><span data-stu-id="03653-148">A task that uses the rimraf Node deletion module to remove the minified version of the site.js file.</span></span>|
|<span data-ttu-id="03653-149">nettoyer : css</span><span class="sxs-lookup"><span data-stu-id="03653-149">clean:css</span></span>|<span data-ttu-id="03653-150">Une tâche qui utilise le module Node de suppression de rimraf pour supprimer la version réduite du fichier site.css.</span><span class="sxs-lookup"><span data-stu-id="03653-150">A task that uses the rimraf Node deletion module to remove the minified version of the site.css file.</span></span>|
|<span data-ttu-id="03653-151">Nettoyer</span><span class="sxs-lookup"><span data-stu-id="03653-151">clean</span></span>|<span data-ttu-id="03653-152">Une tâche qui appelle la tâche `clean:js`, suivie par la tâche `clean:css`.</span><span class="sxs-lookup"><span data-stu-id="03653-152">A task that calls the `clean:js` task, followed by the `clean:css` task.</span></span>|
|<span data-ttu-id="03653-153">min:js</span><span class="sxs-lookup"><span data-stu-id="03653-153">min:js</span></span>|<span data-ttu-id="03653-154">Une tâche qui minimise et concatène tous les fichiers .js dans le dossier js.</span><span class="sxs-lookup"><span data-stu-id="03653-154">A task that minifies and concatenates all .js files within the js folder.</span></span> <span data-ttu-id="03653-155">Le. min.js fichiers sont exclus.</span><span class="sxs-lookup"><span data-stu-id="03653-155">The .min.js files are excluded.</span></span>|
|<span data-ttu-id="03653-156">min:CSS</span><span class="sxs-lookup"><span data-stu-id="03653-156">min:css</span></span>|<span data-ttu-id="03653-157">Une tâche qui minimise et concatène tous les fichiers .css dans le dossier css.</span><span class="sxs-lookup"><span data-stu-id="03653-157">A task that minifies and concatenates all .css files within the css folder.</span></span> <span data-ttu-id="03653-158">Le. les fichiers min.css sont exclus.</span><span class="sxs-lookup"><span data-stu-id="03653-158">The .min.css files are excluded.</span></span>|
|<span data-ttu-id="03653-159">min</span><span class="sxs-lookup"><span data-stu-id="03653-159">min</span></span>|<span data-ttu-id="03653-160">Une tâche qui appelle la tâche `min:js`suivi par la tâche `min:css`.</span><span class="sxs-lookup"><span data-stu-id="03653-160">A task that calls the `min:js` task, followed by the `min:css` task.</span></span>|

## <a name="running-default-tasks"></a><span data-ttu-id="03653-161">Tâches en cours d’exécution par défaut</span><span class="sxs-lookup"><span data-stu-id="03653-161">Running default tasks</span></span>

<span data-ttu-id="03653-162">Si vous n’avez pas déjà créé une nouvelle application Web, créez un nouveau projet d’Application Web ASP.NET dans Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="03653-162">If you haven't already created a new Web app, create a new ASP.NET Web Application project in Visual Studio.</span></span>

1.  <span data-ttu-id="03653-163">Ouvrez le fichier *package.json* (ajoutez-le s'il n'existe pas) et ajoutez le code suivant.</span><span class="sxs-lookup"><span data-stu-id="03653-163">Open the *package.json* file (add if not there) and add the following.</span></span>

    ```json
    {
      "devDependencies": {
        "gulp": "^4.0.0",
        "gulp-concat": "2.6.1",
        "gulp-cssmin": "0.2.0",
        "gulp-uglify": "3.0.0",
        "rimraf": "2.6.1"
      }
    }
    ```

2.  <span data-ttu-id="03653-164">Ajoutez un nouveau fichier JavaScript à votre projet et nommez-le *gulpfile.js*, puis copiez le code suivant.</span><span class="sxs-lookup"><span data-stu-id="03653-164">Add a new JavaScript file to your project and name it *gulpfile.js*, then copy the following code.</span></span>

    ```javascript
    /// <binding Clean='clean' />
    "use strict";
    
    const gulp = require("gulp"),
          rimraf = require("rimraf"),
          concat = require("gulp-concat"),
          cssmin = require("gulp-cssmin"),
          uglify = require("gulp-uglify");
    
    const paths = {
      webroot: "./wwwroot/"
    };
    
    paths.js = paths.webroot + "js/**/*.js";
    paths.minJs = paths.webroot + "js/**/*.min.js";
    paths.css = paths.webroot + "css/**/*.css";
    paths.minCss = paths.webroot + "css/**/*.min.css";
    paths.concatJsDest = paths.webroot + "js/site.min.js";
    paths.concatCssDest = paths.webroot + "css/site.min.css";
    
    gulp.task("clean:js", done => rimraf(paths.concatJsDest, done));
    gulp.task("clean:css", done => rimraf(paths.concatCssDest, done));
    gulp.task("clean", gulp.series(["clean:js", "clean:css"]));

    gulp.task("min:js", () => {
      return gulp.src([paths.js, "!" + paths.minJs], { base: "." })
        .pipe(concat(paths.concatJsDest))
        .pipe(uglify())
        .pipe(gulp.dest("."));
    });

    gulp.task("min:css", () => {
      return gulp.src([paths.css, "!" + paths.minCss])
        .pipe(concat(paths.concatCssDest))
        .pipe(cssmin())
        .pipe(gulp.dest("."));
    });

    gulp.task("min", gulp.series(["min:js", "min:css"]));
    
    // A 'default' task is required by Gulp v4
    gulp.task("default", gulp.series(["min"]));
    ```

3.  <span data-ttu-id="03653-165">Dans **l’Explorateur de solutions**, cliquez avec le bouton droit sur *gulpfile.js*, puis sélectionnez **Task Runner Explorer**.</span><span class="sxs-lookup"><span data-stu-id="03653-165">In **Solution Explorer**, right-click *gulpfile.js*, and select **Task Runner Explorer**.</span></span>
    
    ![Ouvrez Task Runner Explorer à partir de l’Explorateur de solutions](using-gulp/_static/02-SolutionExplorer-TaskRunnerExplorer.png)
    
    <span data-ttu-id="03653-167">**Task Runner Explorer** affiche la liste des tâches de Gulp.</span><span class="sxs-lookup"><span data-stu-id="03653-167">**Task Runner Explorer** shows the list of Gulp tasks.</span></span> <span data-ttu-id="03653-168">(Vous devrez peut-être cliquez sur le bouton **Actualiser** qui apparaît à gauche du nom du projet.)</span><span class="sxs-lookup"><span data-stu-id="03653-168">(You might have to click the **Refresh** button that appears to the left of the project name.)</span></span>
    
    ![Task Runner Explorer](using-gulp/_static/03-TaskRunnerExplorer.png)
    
    > [!IMPORTANT]
    > <span data-ttu-id="03653-170">Le **Task Runner Explorer** élément de menu contextuel s’affiche uniquement si *gulpfile.js* est dans le répertoire racine du projet.</span><span class="sxs-lookup"><span data-stu-id="03653-170">The **Task Runner Explorer** context menu item appears only if *gulpfile.js* is in the root project directory.</span></span>

4.  <span data-ttu-id="03653-171">Sous **Tâches** dans **Task Runner Explorer**, cliquez avec le bouton droit sur **clean**, puis sélectionnez **exécuter** dans le menu contextuel. </span><span class="sxs-lookup"><span data-stu-id="03653-171">Underneath **Tasks** in **Task Runner Explorer**, right-click **clean**, and select **Run** from the pop-up menu.</span></span>

    ![Tâche de nettoyage d’Task Runner Explorer](using-gulp/_static/04-TaskRunner-clean.png)

    <span data-ttu-id="03653-173">**Task Runner Explorer** créera un nouvel onglet nommé **clean** et exécuter la tâche clean telle qu’elle est définie dans *gulpfile.js*.</span><span class="sxs-lookup"><span data-stu-id="03653-173">**Task Runner Explorer** will create a new tab named **clean** and execute the clean task as it's defined in *gulpfile.js*.</span></span>

5.  <span data-ttu-id="03653-174">Cliquer avec le bouton droit sur la tâche **clean**, puis sélectionnez **Bindings**  >  **Before Build**.</span><span class="sxs-lookup"><span data-stu-id="03653-174">Right-click the **clean** task, then select **Bindings** > **Before Build**.</span></span>

    ![Task Runner Explorer BeforeBuild de liaison](using-gulp/_static/05-TaskRunner-BeforeBuild.png)

    <span data-ttu-id="03653-176">La liaison **avant la build** configure la tâche clean pour qu'elle s’exécute automatiquement avant chaque génération du projet.</span><span class="sxs-lookup"><span data-stu-id="03653-176">The **Before Build** binding configures the clean task to run automatically before each build of the project.</span></span>

<span data-ttu-id="03653-177">Les Bindings définis avec **Task Runner Explorer** sont stockés sous la forme d’un commentaire en haut de votre *gulpfile.js* et sont effectifs seulement dans Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="03653-177">The bindings you set up with **Task Runner Explorer** are stored in the form of a comment at the top of your *gulpfile.js* and are effective only in Visual Studio.</span></span> <span data-ttu-id="03653-178">Une alternative qui ne nécessite pas Visual Studio consiste à configurer l’exécution automatique des tâches de gulp dans votre fichier *.csproj*.</span><span class="sxs-lookup"><span data-stu-id="03653-178">An alternative that doesn't require Visual Studio is to configure automatic execution of gulp tasks in your *.csproj* file.</span></span> <span data-ttu-id="03653-179">Par exemple, ajoutez ceci dans votre fichier *.csproj* :</span><span class="sxs-lookup"><span data-stu-id="03653-179">For example, put this in your *.csproj* file:</span></span>

```xml
<Target Name="MyPreCompileTarget" BeforeTargets="Build">
  <Exec Command="gulp clean" />
</Target>
```

<span data-ttu-id="03653-180">Maintenant la tâche de nettoyage est exécutée lorsque vous exécutez le projet dans Visual Studio ou à partir d’une invite de commandes à l’aide de la commande [dotnet run](/dotnet/core/tools/dotnet-run) (exécuter `npm install` d'abord).</span><span class="sxs-lookup"><span data-stu-id="03653-180">Now the clean task is executed when you run the project in Visual Studio or from a command prompt using the [dotnet run](/dotnet/core/tools/dotnet-run) command (run `npm install` first).</span></span>

## <a name="defining-and-running-a-new-task"></a><span data-ttu-id="03653-181">Définition et exécution d'une nouvelle tâche</span><span class="sxs-lookup"><span data-stu-id="03653-181">Defining and running a new task</span></span>

<span data-ttu-id="03653-182">Pour définir une nouvelle tâche Gulp, modifier *gulpfile.js*.</span><span class="sxs-lookup"><span data-stu-id="03653-182">To define a new Gulp task, modify *gulpfile.js*.</span></span>

1.  <span data-ttu-id="03653-183">Ajoutez le code JavaScript suivant à la fin de *gulpfile.js*:</span><span class="sxs-lookup"><span data-stu-id="03653-183">Add the following JavaScript to the end of *gulpfile.js*:</span></span>

    ```javascript
    gulp.task('first', done => {
      console.log('first task! <-----');
      done(); // signal completion
    });
    ```

    <span data-ttu-id="03653-184">Cette tâche est nommée `first`, et elle affiche simplement une chaîne.</span><span class="sxs-lookup"><span data-stu-id="03653-184">This task is named `first`, and it simply displays a string.</span></span>

2.  <span data-ttu-id="03653-185">Enregistrez *gulpfile.js*.</span><span class="sxs-lookup"><span data-stu-id="03653-185">Save *gulpfile.js*.</span></span>

3.  <span data-ttu-id="03653-186">Dans **l’Explorateur de solutions**, cliquez avec le bouton droit sur *gulpfile.js*, puis sélectionnez *Task Runner Explorer*.</span><span class="sxs-lookup"><span data-stu-id="03653-186">In **Solution Explorer**, right-click *gulpfile.js*, and select *Task Runner Explorer*.</span></span>

4.  <span data-ttu-id="03653-187">Dans **Task Runner Explorer**, cliquez avec le bouton droit sur **first**, puis sélectionnez **Run**.</span><span class="sxs-lookup"><span data-stu-id="03653-187">In **Task Runner Explorer**, right-click **first**, and select **Run**.</span></span>

    ![Exécutez la tâche first de Task Runner Explorer](using-gulp/_static/06-TaskRunner-First.png)

    <span data-ttu-id="03653-189">Le texte de sortie s’affiche.</span><span class="sxs-lookup"><span data-stu-id="03653-189">The output text is displayed.</span></span> <span data-ttu-id="03653-190">Pour voir des exemples basés sur des scénarios courants, consultez [recettes Gulp](#gulp-recipes).</span><span class="sxs-lookup"><span data-stu-id="03653-190">To see examples based on common scenarios, see [Gulp Recipes](#gulp-recipes).</span></span>

## <a name="defining-and-running-tasks-in-a-series"></a><span data-ttu-id="03653-191">Définition et l’exécution des tâches dans une série</span><span class="sxs-lookup"><span data-stu-id="03653-191">Defining and running tasks in a series</span></span>

<span data-ttu-id="03653-192">Lorsque vous exécutez plusieurs tâches, les tâches sont exécutées simultanément par défaut.</span><span class="sxs-lookup"><span data-stu-id="03653-192">When you run multiple tasks, the tasks run concurrently by default.</span></span> <span data-ttu-id="03653-193">Toutefois, si vous avez besoin d'exécuter des tâches dans un ordre spécifique, vous devez spécifier quand chaque tâche est terminée, ainsi que les tâches qui dépendent de l’achèvement d’une autre tâche.</span><span class="sxs-lookup"><span data-stu-id="03653-193">However, if you need to run tasks in a specific order, you must specify when each task is complete, as well as which tasks depend on the completion of another task.</span></span>

1.  <span data-ttu-id="03653-194">Pour définir une série de tâches à exécuter dans l’ordre, remplacez la tâche `first` que vous avez ajoutée ci-dessus dans *gulpfile.js* avec les éléments suivants :</span><span class="sxs-lookup"><span data-stu-id="03653-194">To define a series of tasks to run in order, replace the `first` task that you added above in *gulpfile.js* with the following:</span></span>

    ```javascript
    gulp.task('series:first', done => {
      console.log('first task! <-----');
      done(); // signal completion
    });
    gulp.task('series:second', done => {
      console.log('second task! <-----');
      done(); // signal completion
    });

    gulp.task('series', gulp.series(['series:first', 'series:second']), () => { });

    // A 'default' task is required by Gulp v4
    gulp.task('default', gulp.series('series'));
    ```
 
    <span data-ttu-id="03653-195">Vous disposez maintenant de trois tâches : `series:first`, `series:second`, et `series`.</span><span class="sxs-lookup"><span data-stu-id="03653-195">You now have three tasks: `series:first`, `series:second`, and `series`.</span></span> <span data-ttu-id="03653-196">La tâche `series:second` inclut un deuxième paramètre qui spécifie un tableau de tâches à exécuter et terminées avant que la tâche `series:second` tâche exécutera.</span><span class="sxs-lookup"><span data-stu-id="03653-196">The `series:second` task includes a second parameter which specifies an array of tasks to be run and completed before the `series:second` task will run.</span></span> <span data-ttu-id="03653-197">Comme indiqué dans le code ci-dessus, seule la tâche `series:first` doit être réalisée avant que la tâche `series:second`s'exécute.</span><span class="sxs-lookup"><span data-stu-id="03653-197">As specified in the code above, only the `series:first` task must be completed before the `series:second` task will run.</span></span>

2.  <span data-ttu-id="03653-198">Enregistrez *gulpfile.js*.</span><span class="sxs-lookup"><span data-stu-id="03653-198">Save *gulpfile.js*.</span></span>

3.  <span data-ttu-id="03653-199">Dans **l’Explorateur de solutions**, cliquez avec le bouton droit sur *gulpfile.js* et sélectionnez **Task Runner Explorer** si elle n’est pas déjà ouverte.</span><span class="sxs-lookup"><span data-stu-id="03653-199">In **Solution Explorer**, right-click *gulpfile.js* and select **Task Runner Explorer** if it isn't already open.</span></span>

4.  <span data-ttu-id="03653-200">Dans **Task Runner Explorer**, cliquez avec le bouton droit sur **series** et sélectionnez **Run**.</span><span class="sxs-lookup"><span data-stu-id="03653-200">In **Task Runner Explorer**, right-click **series** and select **Run**.</span></span>

    ![Task Runner Explorer exécuter la tâche de série](using-gulp/_static/07-TaskRunner-Series.png)

## <a name="intellisense"></a><span data-ttu-id="03653-202">IntelliSense</span><span class="sxs-lookup"><span data-stu-id="03653-202">IntelliSense</span></span>

<span data-ttu-id="03653-203">IntelliSense propose la saisie semi-automatique de code, des descriptions de paramètres et d’autres fonctionnalités pour augmenter la productivité et réduire les erreurs.</span><span class="sxs-lookup"><span data-stu-id="03653-203">IntelliSense provides code completion, parameter descriptions, and other features to boost productivity and to decrease errors.</span></span> <span data-ttu-id="03653-204">Tâches gulp sont écrits en JavaScript ; Par conséquent, IntelliSense peut fournir une assistance lors du développement.</span><span class="sxs-lookup"><span data-stu-id="03653-204">Gulp tasks are written in JavaScript; therefore, IntelliSense can provide assistance while developing.</span></span> <span data-ttu-id="03653-205">Lorsque vous travaillez avec JavaScript, IntelliSense répertorie les objets, les fonctions, les propriétés et paramètres qui sont disponibles en fonction de votre contexte actuel.</span><span class="sxs-lookup"><span data-stu-id="03653-205">As you work with JavaScript, IntelliSense lists the objects, functions, properties, and parameters that are available based on your current context.</span></span> <span data-ttu-id="03653-206">Sélectionnez une option de codage dans la liste contextuelle fournie par IntelliSense pour compléter le code.</span><span class="sxs-lookup"><span data-stu-id="03653-206">Select a coding option from the pop-up list provided by IntelliSense to complete the code.</span></span>

![gulp IntelliSense](using-gulp/_static/08-IntelliSense.png)

<span data-ttu-id="03653-208">Pour plus d’informations sur IntelliSense, consultez [JavaScript IntelliSense](/visualstudio/ide/javascript-intellisense).</span><span class="sxs-lookup"><span data-stu-id="03653-208">For more information about IntelliSense, see [JavaScript IntelliSense](/visualstudio/ide/javascript-intellisense).</span></span>

## <a name="development-staging-and-production-environments"></a><span data-ttu-id="03653-209">Environnements de développement, de staging et de production</span><span class="sxs-lookup"><span data-stu-id="03653-209">Development, staging, and production environments</span></span>

<span data-ttu-id="03653-210">Lorsque Gulp est utilisé pour optimiser les fichiers côté client pour la préproduction et production, les fichiers traités sont enregistrés dans un emplacement intermédiaire et de production local.</span><span class="sxs-lookup"><span data-stu-id="03653-210">When Gulp is used to optimize client-side files for staging and production, the processed files are saved to a local staging and production location.</span></span> <span data-ttu-id="03653-211">Le *_Layout.cshtml* fichier utilise le **environnement** balise d’assistance pour fournir deux versions différentes de fichiers CSS.</span><span class="sxs-lookup"><span data-stu-id="03653-211">The *_Layout.cshtml* file uses the **environment** tag helper to provide two different versions of CSS files.</span></span> <span data-ttu-id="03653-212">Une version des fichiers CSS est pour le développement et l’autre version est optimisée pour l’environnement intermédiaire et de production.</span><span class="sxs-lookup"><span data-stu-id="03653-212">One version of CSS files is for development and the other version is optimized for both staging and production.</span></span> <span data-ttu-id="03653-213">Dans Visual Studio 2017, lorsque vous modifiez le **ASPNETCORE_ENVIRONMENT** variable d’environnement `Production`, Visual Studio génère l’application Web et un lien vers les fichiers CSS réduites.</span><span class="sxs-lookup"><span data-stu-id="03653-213">In Visual Studio 2017, when you change the **ASPNETCORE_ENVIRONMENT** environment variable to `Production`, Visual Studio will build the Web app and link to the minimized CSS files.</span></span> <span data-ttu-id="03653-214">L’exemple de balisage suivant le **environnement** tag helpers contenant des balises de liens à la `Development` CSS fichiers et le minimisés `Staging, Production` fichiers CSS.</span><span class="sxs-lookup"><span data-stu-id="03653-214">The following markup shows the **environment** tag helpers containing link tags to the `Development` CSS files and the minified `Staging, Production` CSS files.</span></span>

```html
<environment names="Development">
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

## <a name="switching-between-environments"></a><span data-ttu-id="03653-215">Changer d’environnement</span><span class="sxs-lookup"><span data-stu-id="03653-215">Switching between environments</span></span>

<span data-ttu-id="03653-216">Pour basculer entre la compilation pour des environnements différents, vous devez modifier la valeur de la variable d'environnement **ASPNETCORE_ENVIRONMENT**.</span><span class="sxs-lookup"><span data-stu-id="03653-216">To switch between compiling for different environments, modify the **ASPNETCORE_ENVIRONMENT** environment variable's value.</span></span>

1.  <span data-ttu-id="03653-217">Dans **Task Runner Explorer**, vérifiez que la tâche**min** ta été définie pour s'exécuter **avant de générer**.</span><span class="sxs-lookup"><span data-stu-id="03653-217">In **Task Runner Explorer**, verify that the **min** task has been set to run **Before Build**.</span></span>

2.  <span data-ttu-id="03653-218">Dans **l’Explorateur de solutions**, cliquez sur le nom du projet et sélectionnez **Propriétés**.</span><span class="sxs-lookup"><span data-stu-id="03653-218">In **Solution Explorer**, right-click the project name and select **Properties**.</span></span>

    <span data-ttu-id="03653-219">La feuille de propriétés de l’application Web s’affiche.</span><span class="sxs-lookup"><span data-stu-id="03653-219">The property sheet for the Web app is displayed.</span></span>

3.  <span data-ttu-id="03653-220">Cliquez sur l’onglet **Déboguer**.</span><span class="sxs-lookup"><span data-stu-id="03653-220">Click the **Debug** tab.</span></span>

4.  <span data-ttu-id="03653-221">Définir la valeur de la variable d’environnement **:Hosting:Environment** à `Production`.</span><span class="sxs-lookup"><span data-stu-id="03653-221">Set the value of the **Hosting:Environment** environment variable to `Production`.</span></span>

5.  <span data-ttu-id="03653-222">Appuyez sur **F5** pour exécuter l’application dans un navigateur.</span><span class="sxs-lookup"><span data-stu-id="03653-222">Press **F5** to run the application in a browser.</span></span>

6.  <span data-ttu-id="03653-223">Dans la fenêtre du navigateur, cliquez avec le bouton droit sur la page, puis sélectionnez **afficher la Source** pour afficher le code HTML de la page.</span><span class="sxs-lookup"><span data-stu-id="03653-223">In the browser window, right-click the page and select **View Source** to view the HTML for the page.</span></span>

    <span data-ttu-id="03653-224">Notez que les liens de la feuille de style pointent vers les versions non minimisées des fichiers CSS.</span><span class="sxs-lookup"><span data-stu-id="03653-224">Notice that the stylesheet links point to the minified CSS files.</span></span>

7.  <span data-ttu-id="03653-225">Fermez le navigateur pour arrêter l’application Web.</span><span class="sxs-lookup"><span data-stu-id="03653-225">Close the browser to stop the Web app.</span></span>

8.  <span data-ttu-id="03653-226">Dans Visual Studio, revenir à la feuille de propriétés de l’application Web et modifier la variable d’environnement **Hosting:Environment**pour revenir à `Development`.</span><span class="sxs-lookup"><span data-stu-id="03653-226">In Visual Studio, return to the property sheet for the Web app and change the **Hosting:Environment** environment variable back to `Development`.</span></span>

9.  <span data-ttu-id="03653-227">Appuyez sur **F5** pour réexécuter l’application dans un navigateur.</span><span class="sxs-lookup"><span data-stu-id="03653-227">Press **F5** to run the application in a browser again.</span></span>

10. <span data-ttu-id="03653-228">Dans la fenêtre du navigateur, cliquez avec le bouton droit sur la page, puis sélectionnez **afficher la Source** pour afficher le code HTML de la page.</span><span class="sxs-lookup"><span data-stu-id="03653-228">In the browser window, right-click the page and select **View Source** to see the HTML for the page.</span></span>

    <span data-ttu-id="03653-229">Notez que les liens de la feuille de style pointent vers les versions non minifiés des fichiers CSS.</span><span class="sxs-lookup"><span data-stu-id="03653-229">Notice that the stylesheet links point to the unminified versions of the CSS files.</span></span>

<span data-ttu-id="03653-230">Pour plus d’informations sur les environnements dans ASP.NET Core, consultez [utiliser plusieurs environnements](../fundamentals/environments.md).</span><span class="sxs-lookup"><span data-stu-id="03653-230">For more information related to environments in ASP.NET Core, see [Use multiple environments](../fundamentals/environments.md).</span></span>

## <a name="task-and-module-details"></a><span data-ttu-id="03653-231">Détails de tâche et de module</span><span class="sxs-lookup"><span data-stu-id="03653-231">Task and module details</span></span>

<span data-ttu-id="03653-232">Une tâche Gulp est inscrite avec un nom de fonction.</span><span class="sxs-lookup"><span data-stu-id="03653-232">A Gulp task is registered with a function name.</span></span> <span data-ttu-id="03653-233">Vous pouvez spécifier des dépendances si d’autres tâches doivent être exécutées avant la tâche actuelle.</span><span class="sxs-lookup"><span data-stu-id="03653-233">You can specify dependencies if other tasks must run before the current task.</span></span> <span data-ttu-id="03653-234">Des fonctions supplémentaires permettent d'exécuter et de surveiller les tâches Gulp, ainsi que de définir la source (*src*) et la destination (*dest*) des fichiers en cours de modification.</span><span class="sxs-lookup"><span data-stu-id="03653-234">Additional functions allow you to run and watch the Gulp tasks, as well as set the source (*src*) and destination (*dest*) of the files being modified.</span></span> <span data-ttu-id="03653-235">Les fonctions principales de l'API Gulp sont les suivantes :</span><span class="sxs-lookup"><span data-stu-id="03653-235">The following are the primary Gulp API functions:</span></span>

|<span data-ttu-id="03653-236">Gulp (fonction)</span><span class="sxs-lookup"><span data-stu-id="03653-236">Gulp Function</span></span>|<span data-ttu-id="03653-237">Syntaxe</span><span class="sxs-lookup"><span data-stu-id="03653-237">Syntax</span></span>|<span data-ttu-id="03653-238">Description</span><span class="sxs-lookup"><span data-stu-id="03653-238">Description</span></span>|
|---   |--- |--- |
|<span data-ttu-id="03653-239">task</span><span class="sxs-lookup"><span data-stu-id="03653-239">task</span></span>  |`gulp.task(name[, deps], fn) { }`|<span data-ttu-id="03653-240">La fonction `task` crée une tâche.</span><span class="sxs-lookup"><span data-stu-id="03653-240">The `task` function creates a task.</span></span> <span data-ttu-id="03653-241">Le paramètre `name` définit le nom de la tâche.</span><span class="sxs-lookup"><span data-stu-id="03653-241">The `name` parameter defines the name of the task.</span></span> <span data-ttu-id="03653-242">Le paramètre `deps` contient un tableau de tâches à effectuer avant d’exécuter cette tâche.</span><span class="sxs-lookup"><span data-stu-id="03653-242">The `deps` parameter contains an array of tasks to be completed before this task runs.</span></span> <span data-ttu-id="03653-243">Le paramètre `fn` représente une fonction de rappel qui effectue les opérations de la tâche.</span><span class="sxs-lookup"><span data-stu-id="03653-243">The `fn` parameter represents a callback function which performs the operations of the task.</span></span>|
|<span data-ttu-id="03653-244">watch</span><span class="sxs-lookup"><span data-stu-id="03653-244">watch</span></span> |`gulp.watch(glob [, opts], tasks) { }`|<span data-ttu-id="03653-245">La fonction `watch` surveille les fichiers et exécute des tâches lorsqu’une modification de fichier se produit.</span><span class="sxs-lookup"><span data-stu-id="03653-245">The `watch` function monitors files and runs tasks when a file change occurs.</span></span> <span data-ttu-id="03653-246">Le paramètre `glob` est un `string` ou `array` qui détermine les fichiers à surveiller.</span><span class="sxs-lookup"><span data-stu-id="03653-246">The `glob` parameter is a `string` or `array` that determines which files to watch.</span></span> <span data-ttu-id="03653-247">Le paramètre `opts` fournit la surveillance des options de fichiers supplémentaires.</span><span class="sxs-lookup"><span data-stu-id="03653-247">The `opts` parameter provides additional file watching options.</span></span>|
|<span data-ttu-id="03653-248">src</span><span class="sxs-lookup"><span data-stu-id="03653-248">src</span></span>   |`gulp.src(globs[, options]) { }`|<span data-ttu-id="03653-249">La fonction `src` fournit des fichiers qui correspondent aux valeurs glob.</span><span class="sxs-lookup"><span data-stu-id="03653-249">The `src` function provides files that match the glob value(s).</span></span> <span data-ttu-id="03653-250">Le paramètre `glob` est une `string` ou un `array` qui détermine les fichiers à lire.</span><span class="sxs-lookup"><span data-stu-id="03653-250">The `glob` parameter is a `string` or `array` that determines which files to read.</span></span> <span data-ttu-id="03653-251">Le paramètre `options` fournit des options de fichiers supplémentaires.</span><span class="sxs-lookup"><span data-stu-id="03653-251">The `options` parameter provides additional file options.</span></span>|
|<span data-ttu-id="03653-252">destination</span><span class="sxs-lookup"><span data-stu-id="03653-252">dest</span></span>  |`gulp.dest(path[, options]) { }`|<span data-ttu-id="03653-253">La fonction `dest` définit un emplacement dans lequel les fichiers peuvent être écrits.</span><span class="sxs-lookup"><span data-stu-id="03653-253">The `dest` function defines a location to which files can be written.</span></span> <span data-ttu-id="03653-254">Le paramètre `path` est une chaîne ou une fonction qui détermine le dossier de destination.</span><span class="sxs-lookup"><span data-stu-id="03653-254">The `path` parameter is a string or function that determines the destination folder.</span></span> <span data-ttu-id="03653-255">Le paramètre `options` est un objet qui spécifie les options de dossier de sortie.</span><span class="sxs-lookup"><span data-stu-id="03653-255">The `options` parameter is an object that specifies output folder options.</span></span>|

<span data-ttu-id="03653-256">Pour plus d’informations de référence supplémentaires sur l'API de Gulp , consultez [documentation de l'API Gulp](https://github.com/gulpjs/gulp/blob/master/docs/API.md).</span><span class="sxs-lookup"><span data-stu-id="03653-256">For additional Gulp API reference information, see [Gulp Docs API](https://github.com/gulpjs/gulp/blob/master/docs/API.md).</span></span>

## <a name="gulp-recipes"></a><span data-ttu-id="03653-257">Recettes Gulp</span><span class="sxs-lookup"><span data-stu-id="03653-257">Gulp recipes</span></span>

<span data-ttu-id="03653-258">La Communauté Gulp fournit Gulp [recettes](https://github.com/gulpjs/gulp/blob/master/docs/recipes/README.md).</span><span class="sxs-lookup"><span data-stu-id="03653-258">The Gulp community provides Gulp [Recipes](https://github.com/gulpjs/gulp/blob/master/docs/recipes/README.md).</span></span> <span data-ttu-id="03653-259">Ces recettes sont constituées des tâches Gulp pour résoudre des scénarios courants.</span><span class="sxs-lookup"><span data-stu-id="03653-259">These recipes consist of Gulp tasks to address common scenarios.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="03653-260">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="03653-260">Additional resources</span></span>

* [<span data-ttu-id="03653-261">Documentation de gulp</span><span class="sxs-lookup"><span data-stu-id="03653-261">Gulp documentation</span></span>](https://github.com/gulpjs/gulp/blob/master/docs/README.md)
* [<span data-ttu-id="03653-262">Reroupement et minimisation dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="03653-262">Bundling and minification in ASP.NET Core</span></span>](bundling-and-minification.md)
* [<span data-ttu-id="03653-263">Utiliser Grunt dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="03653-263">Use Grunt in ASP.NET Core</span></span>](using-grunt.md)
