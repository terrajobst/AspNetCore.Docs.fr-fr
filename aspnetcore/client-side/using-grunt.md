---
title: Utiliser Grunt dans ASP.NET Core
author: rick-anderson
description: Utiliser Grunt dans ASP.NET Core
ms.author: riande
ms.date: 05/14/2019
uid: client-side/using-grunt
ms.openlocfilehash: 4d9b6cf6f9a0007e9722bc054f0d9a7608f1473b
ms.sourcegitcommit: 3ee6ee0051c3d2c8d47a58cb17eef1a84a4c46a0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/14/2019
ms.locfileid: "65621003"
---
# <a name="use-grunt-in-aspnet-core"></a><span data-ttu-id="c67a3-103">Utiliser Grunt dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c67a3-103">Use Grunt in ASP.NET Core</span></span>

<span data-ttu-id="c67a3-104">Par [Noel riz](https://blog.falafel.com/falafel-software-recognized-sitefinity-website-year/)</span><span class="sxs-lookup"><span data-stu-id="c67a3-104">By [Noel Rice](https://blog.falafel.com/falafel-software-recognized-sitefinity-website-year/)</span></span>

<span data-ttu-id="c67a3-105">Grunt est un exécuteur de tâches JavaScript qui automatise la minimisation de script, la compilation de TypeScript, qualité du code, outils de la moulinette « lint », les processeurs préliminaire CSS et pratiquement n’importe quel répétitive, qui doit effectuer pour prendre en charge le développement de clients.</span><span class="sxs-lookup"><span data-stu-id="c67a3-105">Grunt is a JavaScript task runner that automates script minification, TypeScript compilation, code quality "lint" tools, CSS pre-processors, and just about any repetitive chore that needs doing to support client development.</span></span> <span data-ttu-id="c67a3-106">Grunt est entièrement pris en charge dans Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="c67a3-106">Grunt is fully supported in Visual Studio.</span></span>

<span data-ttu-id="c67a3-107">Cet exemple utilise un projet ASP.NET Core vide comme point de départ, pour montrer comment automatiser le processus de génération de client à partir de zéro.</span><span class="sxs-lookup"><span data-stu-id="c67a3-107">This example uses an empty ASP.NET Core project as its starting point, to show how to automate the client build process from scratch.</span></span>

<span data-ttu-id="c67a3-108">L’exemple terminé nettoie le répertoire de déploiement cible, combine les fichiers JavaScript, vérifie la qualité du code, condense le contenu d’un fichier JavaScript et déploie à la racine de votre application web.</span><span class="sxs-lookup"><span data-stu-id="c67a3-108">The finished example cleans the target deployment directory, combines JavaScript files, checks code quality, condenses JavaScript file content and deploys to the root of your web application.</span></span> <span data-ttu-id="c67a3-109">Nous allons utiliser les packages suivants :</span><span class="sxs-lookup"><span data-stu-id="c67a3-109">We will use the following packages:</span></span>

* <span data-ttu-id="c67a3-110">**Grunt**: Le package de test runner de tâches Grunt.</span><span class="sxs-lookup"><span data-stu-id="c67a3-110">**grunt**: The Grunt task runner package.</span></span>

* <span data-ttu-id="c67a3-111">**Grunt-cotisation-clean**: Un plug-in qui supprime des fichiers ou répertoires.</span><span class="sxs-lookup"><span data-stu-id="c67a3-111">**grunt-contrib-clean**: A plugin that removes files or directories.</span></span>

* <span data-ttu-id="c67a3-112">**Grunt-cotisation-jshint**: Un plug-in qui passe en revue la qualité du code JavaScript.</span><span class="sxs-lookup"><span data-stu-id="c67a3-112">**grunt-contrib-jshint**: A plugin that reviews JavaScript code quality.</span></span>

* <span data-ttu-id="c67a3-113">**Grunt-cotisation-concat**: Un plug-in qui regroupe des fichiers dans un seul fichier.</span><span class="sxs-lookup"><span data-stu-id="c67a3-113">**grunt-contrib-concat**: A plugin that joins files into a single file.</span></span>

* <span data-ttu-id="c67a3-114">**Grunt-cotisation-uglify**: Un plug-in qui minimise le code JavaScript pour réduire la taille.</span><span class="sxs-lookup"><span data-stu-id="c67a3-114">**grunt-contrib-uglify**: A plugin that minifies JavaScript to reduce size.</span></span>

* <span data-ttu-id="c67a3-115">**Grunt-cotisation-watch**: Un plug-in qui surveille l’activité de fichier.</span><span class="sxs-lookup"><span data-stu-id="c67a3-115">**grunt-contrib-watch**: A plugin that watches file activity.</span></span>

## <a name="preparing-the-application"></a><span data-ttu-id="c67a3-116">Préparation de l’application</span><span class="sxs-lookup"><span data-stu-id="c67a3-116">Preparing the application</span></span>

<span data-ttu-id="c67a3-117">Pour commencer, configurez une nouvelle application web vide et ajouter des fichiers d’exemple TypeScript.</span><span class="sxs-lookup"><span data-stu-id="c67a3-117">To begin, set up a new empty web application and add TypeScript example files.</span></span> <span data-ttu-id="c67a3-118">Les fichiers TypeScript sont automatiquement compilés dans JavaScript à l’aide des paramètres de Visual Studio par défaut et seront notre matières premières à traiter à l’aide de Grunt.</span><span class="sxs-lookup"><span data-stu-id="c67a3-118">TypeScript files are automatically compiled into JavaScript using default Visual Studio settings and will be our raw material to process using Grunt.</span></span>

1. <span data-ttu-id="c67a3-119">Dans Visual Studio, créez un nouveau `ASP.NET Web Application`.</span><span class="sxs-lookup"><span data-stu-id="c67a3-119">In Visual Studio, create a new `ASP.NET Web Application`.</span></span>

2. <span data-ttu-id="c67a3-120">Dans le **nouveau projet ASP.NET** boîte de dialogue, sélectionnez ASP.NET Core **vide** modèle et cliquez sur le bouton OK.</span><span class="sxs-lookup"><span data-stu-id="c67a3-120">In the **New ASP.NET Project** dialog, select the ASP.NET Core **Empty** template and click the OK button.</span></span>

3. <span data-ttu-id="c67a3-121">Dans l’Explorateur de solutions, passez en revue la structure de projet.</span><span class="sxs-lookup"><span data-stu-id="c67a3-121">In the Solution Explorer, review the project structure.</span></span> <span data-ttu-id="c67a3-122">Le `\src` dossier inclut vide `wwwroot` et `Dependencies` nœuds.</span><span class="sxs-lookup"><span data-stu-id="c67a3-122">The `\src` folder includes empty `wwwroot` and `Dependencies` nodes.</span></span>

    ![solution de site web vide](using-grunt/_static/grunt-solution-explorer.png)

4. <span data-ttu-id="c67a3-124">Ajoutez un nouveau dossier nommé `TypeScript` à votre répertoire de projet.</span><span class="sxs-lookup"><span data-stu-id="c67a3-124">Add a new folder named `TypeScript` to your project directory.</span></span>

5. <span data-ttu-id="c67a3-125">Avant d’ajouter tous les fichiers, assurez-vous que Visual Studio a l’option « compiler lors de l’enregistrement » pour les fichiers TypeScript activées.</span><span class="sxs-lookup"><span data-stu-id="c67a3-125">Before adding any files, make sure that Visual Studio has the option 'compile on save' for TypeScript files checked.</span></span> <span data-ttu-id="c67a3-126">Accédez à **outils** > **Options** > **éditeur de texte** > **Typescript**  >  **Projet**:</span><span class="sxs-lookup"><span data-stu-id="c67a3-126">Navigate to **Tools** > **Options** > **Text Editor** > **Typescript** > **Project**:</span></span>

    ![Options de compilation automatique des fichiers de TypeScript](using-grunt/_static/typescript-options.png)

6. <span data-ttu-id="c67a3-128">Cliquez sur le `TypeScript` répertoire et sélectionnez **Ajouter > nouvel élément** dans le menu contextuel.</span><span class="sxs-lookup"><span data-stu-id="c67a3-128">Right-click the `TypeScript` directory and select **Add > New Item** from the context menu.</span></span> <span data-ttu-id="c67a3-129">Sélectionnez le **fichier JavaScript** d’élément et nommez le fichier *Tastes.ts* (Notez le \*extension .ts).</span><span class="sxs-lookup"><span data-stu-id="c67a3-129">Select the **JavaScript file** item and name the file *Tastes.ts* (note the \*.ts extension).</span></span> <span data-ttu-id="c67a3-130">Copiez la ligne de code TypeScript ci-dessous dans le fichier (lorsque vous enregistrez, un nouveau *Tastes.js* fichier s’affiche avec la source de JavaScript).</span><span class="sxs-lookup"><span data-stu-id="c67a3-130">Copy the line of TypeScript code below into the file (when you save, a new *Tastes.js* file will appear with the JavaScript source).</span></span>

    ```typescript
    enum Tastes { Sweet, Sour, Salty, Bitter }
    ```

7. <span data-ttu-id="c67a3-131">Ajoutez un deuxième fichier dans le **TypeScript** directory et nommez-le `Food.ts`.</span><span class="sxs-lookup"><span data-stu-id="c67a3-131">Add a second file to the **TypeScript** directory and name it `Food.ts`.</span></span> <span data-ttu-id="c67a3-132">Copiez le code ci-dessous dans le fichier.</span><span class="sxs-lookup"><span data-stu-id="c67a3-132">Copy the code below into the file.</span></span>

    ```typescript
    class Food {
      constructor(name: string, calories: number) {
        this._name = name;
        this._calories = calories;
      }

      private _name: string;
      get Name() {
        return this._name;
      }

      private _calories: number;
      get Calories() {
        return this._calories;
      }

      private _taste: Tastes;
      get Taste(): Tastes { return this._taste }
      set Taste(value: Tastes) {
        this._taste = value;
      }
    }
    ```

## <a name="configuring-npm"></a><span data-ttu-id="c67a3-133">Configuration de NPM</span><span class="sxs-lookup"><span data-stu-id="c67a3-133">Configuring NPM</span></span>

<span data-ttu-id="c67a3-134">Ensuite, configurez NPM pour télécharger grunt et tâches de grunt.</span><span class="sxs-lookup"><span data-stu-id="c67a3-134">Next, configure NPM to download grunt and grunt-tasks.</span></span>

1. <span data-ttu-id="c67a3-135">Dans l’Explorateur de solutions, cliquez sur le projet et sélectionnez **Ajouter > nouvel élément** dans le menu contextuel.</span><span class="sxs-lookup"><span data-stu-id="c67a3-135">In the Solution Explorer, right-click the project and select **Add > New Item** from the context menu.</span></span> <span data-ttu-id="c67a3-136">Sélectionnez le **fichier de configuration de NPM** d’élément, laissez le nom par défaut, *package.json*, puis cliquez sur le **ajouter** bouton.</span><span class="sxs-lookup"><span data-stu-id="c67a3-136">Select the **NPM configuration file** item, leave the default name, *package.json*, and click the **Add** button.</span></span>

2. <span data-ttu-id="c67a3-137">Dans le *package.json* de fichiers, à l’intérieur du `devDependencies` accolades d’objet, entrez « grunt ».</span><span class="sxs-lookup"><span data-stu-id="c67a3-137">In the *package.json* file, inside the `devDependencies` object braces, enter "grunt".</span></span> <span data-ttu-id="c67a3-138">Sélectionnez `grunt` dans Intellisense liste et appuyez sur la touche ENTRÉE.</span><span class="sxs-lookup"><span data-stu-id="c67a3-138">Select `grunt` from the Intellisense list and press the Enter key.</span></span> <span data-ttu-id="c67a3-139">Visual Studio citer le nom du package grunt et ajouter un signe deux-points.</span><span class="sxs-lookup"><span data-stu-id="c67a3-139">Visual Studio will quote the grunt package name, and add a colon.</span></span> <span data-ttu-id="c67a3-140">À droite du signe deux-points, sélectionnez la dernière version stable du package à partir du haut de la liste Intellisense (appuyez sur `Ctrl-Space` si Intellisense n’apparaît pas).</span><span class="sxs-lookup"><span data-stu-id="c67a3-140">To the right of the colon, select the latest stable version of the package from the top of the Intellisense list (press `Ctrl-Space` if Intellisense doesn't appear).</span></span>

    ![Grunt Intellisense](using-grunt/_static/devdependencies-grunt.png)

    > [!NOTE]
    > <span data-ttu-id="c67a3-142">NPM utilise [semver](http://semver.org/) pour organiser les dépendances.</span><span class="sxs-lookup"><span data-stu-id="c67a3-142">NPM uses [semantic versioning](http://semver.org/) to organize dependencies.</span></span> <span data-ttu-id="c67a3-143"> Le contrôle de version sémantique, également appelé SemVer, identifie les packages avec le modèle de numérotation \<majeure >.\< mineure >. \<correctif >.</span><span class="sxs-lookup"><span data-stu-id="c67a3-143">Semantic versioning, also known as SemVer, identifies packages with the numbering scheme \<major>.\<minor>.\<patch>.</span></span> <span data-ttu-id="c67a3-144">IntelliSense simplifie la gestion sémantique de version en affichant uniquement quelques choix répandus en matière.</span><span class="sxs-lookup"><span data-stu-id="c67a3-144">Intellisense simplifies semantic versioning by showing only a few common choices.</span></span> <span data-ttu-id="c67a3-145">Le premier élément dans la liste Intellisense (0.4.5 dans l’exemple ci-dessus) est considéré comme la dernière version stable du package.</span><span class="sxs-lookup"><span data-stu-id="c67a3-145">The top item in the Intellisense list (0.4.5 in the example above) is considered the latest stable version of the package.</span></span> <span data-ttu-id="c67a3-146">Le symbole de signe insertion (^) correspond à la version majeure la plus récente et le tilde (~) correspond à la version mineure la plus récente.</span><span class="sxs-lookup"><span data-stu-id="c67a3-146">The caret (^) symbol matches the most recent major version and the tilde (~) matches the most recent minor version.</span></span> <span data-ttu-id="c67a3-147">Consultez le [référence d’analyseur NPM semver version](https://www.npmjs.com/package/semver) comme guide pour l’expressivité complète qui fournit de SemVer.</span><span class="sxs-lookup"><span data-stu-id="c67a3-147">See the [NPM semver version parser reference](https://www.npmjs.com/package/semver) as a guide to the full expressivity that SemVer provides.</span></span>

3. <span data-ttu-id="c67a3-148">Ajouter plus de dépendances pour charger grunt-cotisation -\* packages pour *propre*, *jshint*, *concat*, *uglify*et *espion* comme indiqué dans l’exemple ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="c67a3-148">Add more dependencies to load grunt-contrib-\* packages for *clean*, *jshint*, *concat*, *uglify*, and *watch* as shown in the example below.</span></span> <span data-ttu-id="c67a3-149">Les versions n’avez pas besoin de correspondre à l’exemple.</span><span class="sxs-lookup"><span data-stu-id="c67a3-149">The versions don't need to match the example.</span></span>

    ```json
    "devDependencies": {
      "grunt": "0.4.5",
      "grunt-contrib-clean": "0.6.0",
      "grunt-contrib-jshint": "0.11.0",
      "grunt-contrib-concat": "0.5.1",
      "grunt-contrib-uglify": "0.8.0",
      "grunt-contrib-watch": "0.6.1"
    }
    ```

4. <span data-ttu-id="c67a3-150">Enregistrer le *package.json* fichier.</span><span class="sxs-lookup"><span data-stu-id="c67a3-150">Save the *package.json* file.</span></span>

<span data-ttu-id="c67a3-151">Les packages pour chaque `devDependencies` élément télécharge, ainsi que tous les fichiers qui nécessite de chaque package.</span><span class="sxs-lookup"><span data-stu-id="c67a3-151">The packages for each `devDependencies` item will download, along with any files that each package requires.</span></span> <span data-ttu-id="c67a3-152">Vous trouverez les fichiers du package dans le *node_modules* répertoire en activant la **afficher tous les fichiers** situé dans **l’Explorateur de solutions**.</span><span class="sxs-lookup"><span data-stu-id="c67a3-152">You can find the package files in the *node_modules* directory by enabling the **Show All Files** button in **Solution Explorer**.</span></span>

![node_modules de Grunt](using-grunt/_static/node-modules.png)

> [!NOTE]
> <span data-ttu-id="c67a3-154">Si vous le souhaitez, vous pouvez restaurer manuellement les dépendances dans **l’Explorateur de solutions** en cliquant sur `Dependencies\NPM` et en sélectionnant le **restaurer les Packages** option de menu.</span><span class="sxs-lookup"><span data-stu-id="c67a3-154">If you need to, you can manually restore dependencies in **Solution Explorer** by right-clicking on `Dependencies\NPM` and selecting the **Restore Packages** menu option.</span></span>

![restaurer des packages](using-grunt/_static/restore-packages.png)

## <a name="configuring-grunt"></a><span data-ttu-id="c67a3-156">Configuration Grunt</span><span class="sxs-lookup"><span data-stu-id="c67a3-156">Configuring Grunt</span></span>

<span data-ttu-id="c67a3-157">Grunt est configuré à l’aide d’un manifeste nommé *Gruntfile.js* qui définit, charge et enregistre les tâches qui peuvent être exécutés manuellement ou configurés pour exécuter automatiquement en fonction des événements dans Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="c67a3-157">Grunt is configured using a manifest named *Gruntfile.js* that defines, loads and registers tasks that can be run manually or configured to run automatically based on events in Visual Studio.</span></span>

1. <span data-ttu-id="c67a3-158">Cliquez sur le projet et sélectionnez **ajouter** > **un nouvel élément**.</span><span class="sxs-lookup"><span data-stu-id="c67a3-158">Right-click the project and select **Add** > **New Item**.</span></span> <span data-ttu-id="c67a3-159">Sélectionnez le **fichier JavaScript** modèle d’élément, remplacez le nom par *Gruntfile.js*, puis cliquez sur le **ajouter** bouton.</span><span class="sxs-lookup"><span data-stu-id="c67a3-159">Select the **JavaScript File** item template, change the name to *Gruntfile.js*, and click the **Add** button.</span></span>

1. <span data-ttu-id="c67a3-160">Ajoutez le code suivant à *Gruntfile.js*.</span><span class="sxs-lookup"><span data-stu-id="c67a3-160">Add the following code to *Gruntfile.js*.</span></span> <span data-ttu-id="c67a3-161">Le `initConfig` fonction définit des options pour chaque package et le reste du module charge et inscrire des tâches.</span><span class="sxs-lookup"><span data-stu-id="c67a3-161">The `initConfig` function sets options for each package, and the remainder of the module loads and register tasks.</span></span>

   ```javascript
   module.exports = function (grunt) {
     grunt.initConfig({
     });
   };
   ```

1. <span data-ttu-id="c67a3-162">À l’intérieur de la `initConfig` de fonction, ajouter des options pour le `clean` comme indiqué dans l’exemple de tâches *Gruntfile.js* ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="c67a3-162">Inside the `initConfig` function, add options for the `clean` task as shown in the example *Gruntfile.js* below.</span></span> <span data-ttu-id="c67a3-163">Le `clean` tâche accepte un tableau de chaînes de répertoire.</span><span class="sxs-lookup"><span data-stu-id="c67a3-163">The `clean` task accepts an array of directory strings.</span></span> <span data-ttu-id="c67a3-164">Cette tâche supprime les fichiers à partir de *wwwroot/lib* et supprime l’intégralité de */temp* directory.</span><span class="sxs-lookup"><span data-stu-id="c67a3-164">This task removes files from *wwwroot/lib* and removes the entire */temp* directory.</span></span>

    ```javascript
    module.exports = function (grunt) {
      grunt.initConfig({
        clean: ["wwwroot/lib/*", "temp/"],
      });
    };
    ```

1. <span data-ttu-id="c67a3-165">Ci-dessous le `initConfig` de fonction, ajoutez un appel à `grunt.loadNpmTasks`.</span><span class="sxs-lookup"><span data-stu-id="c67a3-165">Below the `initConfig` function, add a call to `grunt.loadNpmTasks`.</span></span> <span data-ttu-id="c67a3-166">Cela rendra la tâche exécutable à partir de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="c67a3-166">This will make the task runnable from Visual Studio.</span></span>

    ```javascript
    grunt.loadNpmTasks("grunt-contrib-clean");
    ```

1. <span data-ttu-id="c67a3-167">Enregistrer *Gruntfile.js*.</span><span class="sxs-lookup"><span data-stu-id="c67a3-167">Save *Gruntfile.js*.</span></span> <span data-ttu-id="c67a3-168">Le fichier doit ressembler à la capture d’écran ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="c67a3-168">The file should look something like the screenshot below.</span></span>

    ![gruntfile initiale](using-grunt/_static/gruntfile-js-initial.png)

1. <span data-ttu-id="c67a3-170">Avec le bouton droit *Gruntfile.js* et sélectionnez **Task Runner Explorer** dans le menu contextuel.</span><span class="sxs-lookup"><span data-stu-id="c67a3-170">Right-click *Gruntfile.js* and select **Task Runner Explorer** from the context menu.</span></span> <span data-ttu-id="c67a3-171">Le **Task Runner Explorer** fenêtre s’ouvre.</span><span class="sxs-lookup"><span data-stu-id="c67a3-171">The **Task Runner Explorer** window will open.</span></span>

    ![menu de l’Explorateur exécuteur de tâche](using-grunt/_static/task-runner-explorer-menu.png)

1. <span data-ttu-id="c67a3-173">Vérifiez que `clean` s’affiche sous **tâches** dans le **Task Runner Explorer**.</span><span class="sxs-lookup"><span data-stu-id="c67a3-173">Verify that `clean` shows under **Tasks** in the **Task Runner Explorer**.</span></span>

    ![liste des tâches tâches runner explorer](using-grunt/_static/task-runner-explorer-tasks.png)

1. <span data-ttu-id="c67a3-175">Avec le bouton droit de la tâche clean et sélectionnez **exécuter** dans le menu contextuel.</span><span class="sxs-lookup"><span data-stu-id="c67a3-175">Right-click the clean task and select **Run** from the context menu.</span></span> <span data-ttu-id="c67a3-176">Une fenêtre de commande affiche la progression de la tâche.</span><span class="sxs-lookup"><span data-stu-id="c67a3-176">A command window displays progress of the task.</span></span>

    ![tâche de nettoyage de tâche runner explorer exécuter](using-grunt/_static/task-runner-explorer-run-clean.png)

    > [!NOTE]
    > <span data-ttu-id="c67a3-178">Il n’existe aucun fichier ou répertoire pour nettoyer encore.</span><span class="sxs-lookup"><span data-stu-id="c67a3-178">There are no files or directories to clean yet.</span></span> <span data-ttu-id="c67a3-179">Si vous le souhaitez, vous pouvez les créer manuellement dans l’Explorateur de solutions, puis exécutez la tâche clean comme un test.</span><span class="sxs-lookup"><span data-stu-id="c67a3-179">If you like, you can manually create them in the Solution Explorer and then run the clean task as a test.</span></span>

1. <span data-ttu-id="c67a3-180">Dans le `initConfig` de fonction, ajoutez une entrée pour `concat` en utilisant le code ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="c67a3-180">In the `initConfig` function, add an entry for `concat` using the code below.</span></span>

    <span data-ttu-id="c67a3-181">Le `src` tableau de propriétés répertorie les fichiers de combiner, dans l’ordre qu’ils doivent être combinées.</span><span class="sxs-lookup"><span data-stu-id="c67a3-181">The `src` property array lists files to combine, in the order that they should be combined.</span></span> <span data-ttu-id="c67a3-182">Le `dest` propriété affecte le chemin d’accès au fichier combiné qui est généré.</span><span class="sxs-lookup"><span data-stu-id="c67a3-182">The `dest` property assigns the path to the combined file that's produced.</span></span>

    ```javascript
    concat: {
      all: {
        src: ['TypeScript/Tastes.js', 'TypeScript/Food.js'],
        dest: 'temp/combined.js'
      }
    },
    ```

    > [!NOTE]
    > <span data-ttu-id="c67a3-183">Le `all` propriété dans le code ci-dessus est le nom d’une cible.</span><span class="sxs-lookup"><span data-stu-id="c67a3-183">The `all` property in the code above is the name of a target.</span></span> <span data-ttu-id="c67a3-184">Cibles dans certaines tâches Grunt servent à autoriser plusieurs environnements de génération.</span><span class="sxs-lookup"><span data-stu-id="c67a3-184">Targets are used in some Grunt tasks to allow multiple build environments.</span></span> <span data-ttu-id="c67a3-185">Vous pouvez afficher les cibles intégrées à l’aide d’IntelliSense ou affecter les vôtres.</span><span class="sxs-lookup"><span data-stu-id="c67a3-185">You can view the built-in targets using IntelliSense or assign your own.</span></span>

1. <span data-ttu-id="c67a3-186">Ajouter le `jshint` de tâches en utilisant le code ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="c67a3-186">Add the `jshint` task using the code below.</span></span>

    <span data-ttu-id="c67a3-187">Le jshint `code-quality` utilitaire est exécuté sur chaque fichier JavaScript dans le *temp* directory.</span><span class="sxs-lookup"><span data-stu-id="c67a3-187">The jshint `code-quality` utility is run against every JavaScript file found in the *temp* directory.</span></span>

    ```javascript
    jshint: {
      files: ['temp/*.js'],
      options: {
        '-W069': false,
      }
    },
    ```

    > [!NOTE]
    > <span data-ttu-id="c67a3-188">L’option «-W069 » est une erreur produite par jshint lorsque JavaScript utilise mettre entre parenthèses la syntaxe pour assigner une propriété au lieu de la notation à points, par exemple, `Tastes["Sweet"]` au lieu de `Tastes.Sweet`.</span><span class="sxs-lookup"><span data-stu-id="c67a3-188">The option "-W069" is an error produced by jshint when JavaScript uses bracket syntax to assign a property instead of dot notation, i.e. `Tastes["Sweet"]` instead of `Tastes.Sweet`.</span></span> <span data-ttu-id="c67a3-189">L’option désactive l’avertissement pour autoriser le reste du processus pour continuer.</span><span class="sxs-lookup"><span data-stu-id="c67a3-189">The option turns off the warning to allow the rest of the process to continue.</span></span>

1. <span data-ttu-id="c67a3-190">Ajouter le `uglify` de tâches en utilisant le code ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="c67a3-190">Add the `uglify` task using the code below.</span></span>

    <span data-ttu-id="c67a3-191">La tâche minimise le *combined.js* fichier trouvé dans le répertoire temp et crée le fichier de résultats dans wwwroot/lib conformément à la convention d’affectation de noms standard  *\<nom de fichier\>. min.js*.</span><span class="sxs-lookup"><span data-stu-id="c67a3-191">The task minifies the *combined.js* file found in the temp directory and creates the result file in wwwroot/lib following the standard naming convention *\<file name\>.min.js*.</span></span>

    ```javascript
    uglify: {
     all: {
       src: ['temp/combined.js'],
       dest: 'wwwroot/lib/combined.min.js'
     }
    },
    ```

1. <span data-ttu-id="c67a3-192">Sous l’appel à `grunt.loadNpmTasks` qui charge `grunt-contrib-clean`, incluent le même appel pour jshint, concat et uglify en utilisant le code ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="c67a3-192">Under the call to `grunt.loadNpmTasks` that loads `grunt-contrib-clean`, include the same call for jshint, concat, and uglify using the code below.</span></span>

    ```javascript
    grunt.loadNpmTasks('grunt-contrib-jshint');
    grunt.loadNpmTasks('grunt-contrib-concat');
    grunt.loadNpmTasks('grunt-contrib-uglify');
    ```

1. <span data-ttu-id="c67a3-193">Enregistrer *Gruntfile.js*.</span><span class="sxs-lookup"><span data-stu-id="c67a3-193">Save *Gruntfile.js*.</span></span> <span data-ttu-id="c67a3-194">Le fichier doit ressembler à l’exemple ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="c67a3-194">The file should look something like the example below.</span></span>

    ![exemple de fichier complet grunt](using-grunt/_static/gruntfile-js-complete.png)

1. <span data-ttu-id="c67a3-196">Notez que le **Task Runner Explorer** liste de tâches inclut `clean`, `concat`, `jshint` et `uglify` tâches.</span><span class="sxs-lookup"><span data-stu-id="c67a3-196">Notice that the **Task Runner Explorer** Tasks list includes `clean`, `concat`, `jshint` and `uglify` tasks.</span></span> <span data-ttu-id="c67a3-197">Exécuter chaque tâche dans l’ordre et observez les résultats dans **l’Explorateur de solutions**.</span><span class="sxs-lookup"><span data-stu-id="c67a3-197">Run each task in order and observe the results in **Solution Explorer**.</span></span> <span data-ttu-id="c67a3-198">Chaque tâche doit s’exécuter sans erreur.</span><span class="sxs-lookup"><span data-stu-id="c67a3-198">Each task should run without errors.</span></span>

    ![Explorateur d’exécuteur de tâches exécuter chaque tâche](using-grunt/_static/task-runner-explorer-run-each-task.png)

    <span data-ttu-id="c67a3-200">La tâche concat crée un nouveau *combined.js* de fichier et le place dans le répertoire temporaire.</span><span class="sxs-lookup"><span data-stu-id="c67a3-200">The concat task creates a new *combined.js* file and places it into the temp directory.</span></span> <span data-ttu-id="c67a3-201">Le `jshint` tâche simplement s’exécute et ne produit pas sortie.</span><span class="sxs-lookup"><span data-stu-id="c67a3-201">The `jshint` task simply runs and doesn't produce output.</span></span> <span data-ttu-id="c67a3-202">Le `uglify` tâche crée un nouveau *combined.min.js* de fichier et le place dans *wwwroot/lib*.</span><span class="sxs-lookup"><span data-stu-id="c67a3-202">The `uglify` task creates a new *combined.min.js* file and places it into *wwwroot/lib*.</span></span> <span data-ttu-id="c67a3-203">L’opération, la solution doit ressembler à la capture d’écran ci-dessous :</span><span class="sxs-lookup"><span data-stu-id="c67a3-203">On completion, the solution should look something like the screenshot below:</span></span>

    ![une fois toutes les tâches de l’Explorateur de solutions](using-grunt/_static/solution-explorer-after-all-tasks.png)

    > [!NOTE]
    > <span data-ttu-id="c67a3-205">Pour plus d’informations sur les options pour chaque package, visitez [ https://www.npmjs.com/ ](https://www.npmjs.com/) et recherche le nom du package dans la zone de recherche dans la page principale.</span><span class="sxs-lookup"><span data-stu-id="c67a3-205">For more information on the options for each package, visit [https://www.npmjs.com/](https://www.npmjs.com/) and lookup the package name in the search box on the main page.</span></span> <span data-ttu-id="c67a3-206">Par exemple, vous pouvez rechercher le package de grunt-cotisation-clean pour obtenir un lien vers la documentation qui explique tous ses paramètres.</span><span class="sxs-lookup"><span data-stu-id="c67a3-206">For example, you can look up the grunt-contrib-clean package to get a documentation link that explains all of its parameters.</span></span>

### <a name="all-together-now"></a><span data-ttu-id="c67a3-207">Maintenant tous ensemble !</span><span class="sxs-lookup"><span data-stu-id="c67a3-207">All together now</span></span>

<span data-ttu-id="c67a3-208">Utiliser le Grunt `registerTask()` méthode à exécuter une série de tâches dans une séquence particulière.</span><span class="sxs-lookup"><span data-stu-id="c67a3-208">Use the Grunt `registerTask()` method to run a series of tasks in a particular sequence.</span></span> <span data-ttu-id="c67a3-209">Par exemple, pour exécuter l’exemple -> des étapes ci-dessus dans l’ordre propre concat -> jshint -> uglify, ajoutez le code ci-dessous au module.</span><span class="sxs-lookup"><span data-stu-id="c67a3-209">For example, to run the example steps above in the order clean -> concat -> jshint -> uglify, add the code below to the module.</span></span> <span data-ttu-id="c67a3-210">Le code doit être ajouté au même niveau que les appels loadNpmTasks(), à l’extérieur initConfig.</span><span class="sxs-lookup"><span data-stu-id="c67a3-210">The code should be added to the same level as the loadNpmTasks() calls, outside initConfig.</span></span>

```javascript
grunt.registerTask("all", ['clean', 'concat', 'jshint', 'uglify']);
```

<span data-ttu-id="c67a3-211">La nouvelle tâche s’affiche dans Task Runner Explorer sous tâches de l’Alias.</span><span class="sxs-lookup"><span data-stu-id="c67a3-211">The new task shows up in Task Runner Explorer under Alias Tasks.</span></span> <span data-ttu-id="c67a3-212">Vous pouvez avec le bouton droit et l’exécuter comme vous le feriez autres tâches.</span><span class="sxs-lookup"><span data-stu-id="c67a3-212">You can right-click and run it just as you would other tasks.</span></span> <span data-ttu-id="c67a3-213">Le `all` tâche s’exécutera `clean`, `concat`, `jshint` et `uglify`, dans l’ordre.</span><span class="sxs-lookup"><span data-stu-id="c67a3-213">The `all` task will run `clean`, `concat`, `jshint` and `uglify`, in order.</span></span>

![tâches d’alias grunt](using-grunt/_static/alias-tasks.png)

## <a name="watching-for-changes"></a><span data-ttu-id="c67a3-215">Détection des changements</span><span class="sxs-lookup"><span data-stu-id="c67a3-215">Watching for changes</span></span>

<span data-ttu-id="c67a3-216">Un `watch` tâche garde un œil sur les fichiers et répertoires.</span><span class="sxs-lookup"><span data-stu-id="c67a3-216">A `watch` task keeps an eye on files and directories.</span></span> <span data-ttu-id="c67a3-217">L’observation déclenche automatiquement des tâches s’il détecte des modifications.</span><span class="sxs-lookup"><span data-stu-id="c67a3-217">The watch triggers tasks automatically if it detects changes.</span></span> <span data-ttu-id="c67a3-218">Ajoutez le code ci-dessous à initConfig pour surveiller les modifications apportées aux \*fichiers .js dans le répertoire de TypeScript.</span><span class="sxs-lookup"><span data-stu-id="c67a3-218">Add the code below to initConfig to watch for changes to \*.js files in the TypeScript directory.</span></span> <span data-ttu-id="c67a3-219">Si un fichier JavaScript est modifié, `watch` exécutera le `all` tâche.</span><span class="sxs-lookup"><span data-stu-id="c67a3-219">If a JavaScript file is changed, `watch` will run the `all` task.</span></span>

```javascript
watch: {
  files: ["TypeScript/*.js"],
  tasks: ["all"]
}
```

<span data-ttu-id="c67a3-220">Ajoutez un appel à `loadNpmTasks()` pour afficher le `watch` tâche dans l’Explorateur d’exécuteur de tâche.</span><span class="sxs-lookup"><span data-stu-id="c67a3-220">Add a call to `loadNpmTasks()` to show the `watch` task in Task Runner Explorer.</span></span>

```javascript
grunt.loadNpmTasks('grunt-contrib-watch');
```

<span data-ttu-id="c67a3-221">Avec le bouton droit de la tâche espion dans Task Runner Explorer et sélectionnez Exécuter dans le menu contextuel.</span><span class="sxs-lookup"><span data-stu-id="c67a3-221">Right-click the watch task in Task Runner Explorer and select Run from the context menu.</span></span> <span data-ttu-id="c67a3-222">La fenêtre de commande qui affiche l’exécution des tâches Espion affiche un paramètre « en attente... » Message.</span><span class="sxs-lookup"><span data-stu-id="c67a3-222">The command window that shows the watch task running will display a "Waiting…" message.</span></span> <span data-ttu-id="c67a3-223">Ouvrez un des fichiers TypeScript, ajoutez un espace, puis enregistrez le fichier.</span><span class="sxs-lookup"><span data-stu-id="c67a3-223">Open one of the TypeScript files, add a space, and then save the file.</span></span> <span data-ttu-id="c67a3-224">Cela déclenche la tâche espion et déclencher les autres tâches à exécuter dans l’ordre.</span><span class="sxs-lookup"><span data-stu-id="c67a3-224">This will trigger the watch task and trigger the other tasks to run in order.</span></span> <span data-ttu-id="c67a3-225">La capture d’écran ci-dessous montre un exemple d’exécution.</span><span class="sxs-lookup"><span data-stu-id="c67a3-225">The screenshot below shows a sample run.</span></span>

![sortie des tâches en cours d’exécution](using-grunt/_static/watch-running.png)

## <a name="binding-to-visual-studio-events"></a><span data-ttu-id="c67a3-227">Liaison à des événements de Visual Studio</span><span class="sxs-lookup"><span data-stu-id="c67a3-227">Binding to Visual Studio events</span></span>

<span data-ttu-id="c67a3-228">Sauf si vous souhaitez démarrer manuellement vos tâches chaque fois que vous travaillez dans Visual Studio, vous pouvez lier des tâches à **avant de générer**, **après génération**, **Clean**, et  **Projet Open** événements.</span><span class="sxs-lookup"><span data-stu-id="c67a3-228">Unless you want to manually start your tasks every time you work in Visual Studio, you can bind tasks to **Before Build**, **After Build**, **Clean**, and **Project Open** events.</span></span>

<span data-ttu-id="c67a3-229">Nous allons lier `watch` afin qu’il exécute chaque fois que Visual Studio s’ouvre.</span><span class="sxs-lookup"><span data-stu-id="c67a3-229">Let’s bind `watch` so that it runs every time Visual Studio opens.</span></span> <span data-ttu-id="c67a3-230">Dans l’Explorateur d’exécuteur de tâche, avec le bouton droit de la tâche de surveillance et sélectionnez **liaisons > projet ouvert** dans le menu contextuel.</span><span class="sxs-lookup"><span data-stu-id="c67a3-230">In Task Runner Explorer, right-click the watch task and select **Bindings > Project Open** from the context menu.</span></span>

![lier une tâche à l’ouverture du projet](using-grunt/_static/bindings-project-open.png)

<span data-ttu-id="c67a3-232">Déchargez et rechargez le projet.</span><span class="sxs-lookup"><span data-stu-id="c67a3-232">Unload and reload the project.</span></span> <span data-ttu-id="c67a3-233">Lorsque le projet se charge là encore, la tâche espion commence à s’exécuter automatiquement.</span><span class="sxs-lookup"><span data-stu-id="c67a3-233">When the project loads again, the watch task will start running automatically.</span></span>

## <a name="summary"></a><span data-ttu-id="c67a3-234">Récapitulatif</span><span class="sxs-lookup"><span data-stu-id="c67a3-234">Summary</span></span>

<span data-ttu-id="c67a3-235">Grunt est un exécuteur de tâches puissantes qui peut être utilisé pour automatiser la plupart des tâches de génération de client.</span><span class="sxs-lookup"><span data-stu-id="c67a3-235">Grunt is a powerful task runner that can be used to automate most client-build tasks.</span></span> <span data-ttu-id="c67a3-236">Grunt tire parti de NPM pour proposer ses packages, fonctionnalités et outils d’intégration avec Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="c67a3-236">Grunt leverages NPM to deliver its packages, and features tooling integration with Visual Studio.</span></span> <span data-ttu-id="c67a3-237">Task Runner Explorer de Visual Studio détecte les modifications apportées aux fichiers de configuration et fournit une interface pratique pour exécuter des tâches, afficher les tâches en cours d’exécution et lier des tâches aux événements de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="c67a3-237">Visual Studio's Task Runner Explorer detects changes to configuration files and provides a convenient interface to run tasks, view running tasks, and bind tasks to Visual Studio events.</span></span>
