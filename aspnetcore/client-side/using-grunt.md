---
title: Utiliser grunt dans ASP.NET Core
author: rick-anderson
description: Utiliser grunt dans ASP.NET Core
ms.author: riande
ms.date: 12/05/2019
uid: client-side/using-grunt
ms.openlocfilehash: e516b85da7e94d0c93be642086fede0a11fea3c2
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78657590"
---
# <a name="use-grunt-in-aspnet-core"></a>Utiliser grunt dans ASP.NET Core

Grunt est un testeur de tâches JavaScript qui automatise la minimisation de script, la compilation d’écriture automatique, les outils de qualité de code « Lint », les préprocesseurs CSS et tout ce qui est nécessaire pour prendre en charge le développement de clients. Grunt est entièrement pris en charge dans Visual Studio.

Cet exemple utilise un projet de ASP.NET Core vide comme point de départ pour montrer comment automatiser le processus de génération du client à partir de zéro.

L’exemple terminé nettoie le répertoire de déploiement cible, combine des fichiers JavaScript, vérifie la qualité du code, condense le contenu du fichier JavaScript et le déploie à la racine de votre application Web. Nous utiliserons les packages suivants :

* **grunt**: le package grunt Task Runner.

* **grunt-contrib-Clean**: plug-in qui supprime des fichiers ou des répertoires.

* **grunt-contrib-jshint**: plug-in qui vérifie la qualité du code JavaScript.

* **grunt-concontribution-Concat**: plug-in qui joint des fichiers dans un fichier unique.

* **grunt-contrib-uglify**: plug-in qui minimise JavaScript pour réduire la taille.

* **grunt-contrib-Watch**: un plug-in qui surveille l’activité des fichiers.

## <a name="preparing-the-application"></a>Préparation de l’application

Pour commencer, configurez une nouvelle application Web vide et ajoutez des exemples de fichiers de machine à écrire. Les fichiers de machine à écrire sont automatiquement compilés en JavaScript à l’aide des paramètres par défaut de Visual Studio et sont nos documents bruts à traiter à l’aide de Grunt.

1. Dans Visual Studio, créez un `ASP.NET Web Application`.

2. Dans la boîte de dialogue **nouveau projet ASP.net** , sélectionnez le ASP.net Core modèle **vide** , puis cliquez sur le bouton OK.

3. Dans le Explorateur de solutions, passez en revue la structure du projet. Le dossier `\src` comprend des nœuds de `wwwroot` et de `Dependencies` vides.

    ![solution Web vide](using-grunt/_static/grunt-solution-explorer.png)

4. Ajoutez un nouveau dossier nommé `TypeScript` à votre répertoire de projet.

5. Avant d’ajouter des fichiers, assurez-vous que Visual Studio a l’option « compiler à l’enregistrement » pour les fichiers de machine à écrire activée. Accédez à **outils** > **options** > **éditeur de texte** > **écrire** > **projet**:

    ![options configuration de la compilation automatique des fichiers de définition d’une machine](using-grunt/_static/typescript-options.png)

6. Cliquez avec le bouton droit sur le répertoire `TypeScript`, puis sélectionnez **ajouter > nouvel élément** dans le menu contextuel. Sélectionnez l’élément de **fichier JavaScript** et nommez le fichier *goûts. TS* (Notez l’extension \*. TS). Copiez la ligne de code machine à écrire ci-dessous dans le fichier (lorsque vous enregistrez, un nouveau fichier *goûts. js* s’affiche avec la source JavaScript).

    ```typescript
    enum Tastes { Sweet, Sour, Salty, Bitter }
    ```

7. Ajoutez un deuxième fichier au répertoire de la **machine à écrire** et nommez-le `Food.ts`. Copiez le code ci-dessous dans le fichier.

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

## <a name="configuring-npm"></a>Configuration de NPM

Ensuite, configurez NPM pour télécharger grunt et grunt-Tasks.

1. Dans le Explorateur de solutions, cliquez avec le bouton droit sur le projet et sélectionnez **ajouter > nouvel élément** dans le menu contextuel. Sélectionnez l’élément de **fichier de configuration NPM** , laissez le nom par défaut *Package. JSON*, puis cliquez sur le bouton **Ajouter** .

2. Dans le fichier *Package. JSON* , à l’intérieur des accolades de l’objet `devDependencies`, entrez « grunt ». Sélectionnez `grunt` dans la liste IntelliSense, puis appuyez sur la touche entrée. Visual Studio indiquera le nom du package grunt et ajoutera un signe deux-points. À droite du signe deux-points, sélectionnez la dernière version stable du package en haut de la liste IntelliSense (appuyez sur `Ctrl-Space` si IntelliSense n’apparaît pas).

    ![grunt IntelliSense](using-grunt/_static/devdependencies-grunt.png)

    > [!NOTE]
    > NPM utilise le contrôle de [version sémantique](https://semver.org/) pour organiser les dépendances. Le contrôle de version sémantique, également connu sous le nom de SemVer, identifie les packages avec le schéma de numérotation \<> principales.\<> mineure.\<> de correctif. IntelliSense simplifie le contrôle de version sémantique en n’apparaissant que quelques choix courants. L’élément supérieur dans la liste IntelliSense (0.4.5 dans l’exemple ci-dessus) est considéré comme la dernière version stable du package. Le symbole du signe insertion (^) correspond à la version majeure la plus récente et le tilde (~) correspond à la version mineure la plus récente. Consultez la [référence de l’analyseur de version NPM semver](https://www.npmjs.com/package/semver) en tant que guide de l’expression complète fournie par semver.

3. Ajoutez d’autres dépendances pour charger les packages grunt-contrib-\* pour *Clean*, *jshint*, *concat*, *uglify*et *Watch* comme indiqué dans l’exemple ci-dessous. Les versions n’ont pas besoin de correspondre à l’exemple.

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

4. Enregistrez le fichier *Package. JSON* .

Les packages de chaque élément de `devDependencies` sont téléchargés, ainsi que tous les fichiers requis par chaque package. Vous pouvez trouver les fichiers du package dans le répertoire *node_modules* en activant le bouton **Afficher tous les fichiers** dans **Explorateur de solutions**.

![grunt node_modules](using-grunt/_static/node-modules.png)

> [!NOTE]
> Si vous le souhaitez, vous pouvez restaurer manuellement les dépendances dans **Explorateur de solutions** en cliquant avec le bouton droit sur `Dependencies\NPM` et en sélectionnant l’option de menu **restaurer les packages** .

![restaurer les packages](using-grunt/_static/restore-packages.png)

## <a name="configuring-grunt"></a>Configuration de Grunt

Grunt est configuré à l’aide d’un manifeste nommé *Gruntfile. js* qui définit, charge et inscrit des tâches qui peuvent être exécutées manuellement ou configurées pour s’exécuter automatiquement en fonction des événements dans Visual Studio.

1. Cliquez avec le bouton droit sur le projet, puis sélectionnez **ajouter** > **nouvel élément**. Sélectionnez le modèle d’élément de **fichier JavaScript** , remplacez le nom par *Gruntfile. js*, puis cliquez sur le bouton **Ajouter** .

1. Ajoutez le code suivant à *Gruntfile. js*. La fonction `initConfig` définit des options pour chaque package, et le reste du module charge et inscrit les tâches.

   ```javascript
   module.exports = function (grunt) {
     grunt.initConfig({
     });
   };
   ```

1. À l’intérieur de la fonction `initConfig`, ajoutez des options pour la tâche `clean` comme indiqué dans l’exemple *Gruntfile. js* ci-dessous. La tâche `clean` accepte un tableau de chaînes de répertoire. Cette tâche supprime les fichiers de *wwwroot/lib* et supprime l’intégralité du répertoire */temp* .

    ```javascript
    module.exports = function (grunt) {
      grunt.initConfig({
        clean: ["wwwroot/lib/*", "temp/"],
      });
    };
    ```

1. Sous la fonction `initConfig`, ajoutez un appel à `grunt.loadNpmTasks`. Cela rendra la tâche exécutable à partir de Visual Studio.

    ```javascript
    grunt.loadNpmTasks("grunt-contrib-clean");
    ```

1. Enregistrez *Gruntfile. js*. Le fichier doit ressembler à la capture d’écran ci-dessous.

    ![gruntfile initial](using-grunt/_static/gruntfile-js-initial.png)

1. Cliquez avec le bouton droit sur *Gruntfile. js* et sélectionnez Explorateur de l’exécution de **tâches** dans le menu contextuel. La fenêtre de l' **Explorateur du Runner de tâche** s’ouvre.

    ![menu de l’Explorateur de l’exécuteur de tâches](using-grunt/_static/task-runner-explorer-menu.png)

1. Vérifiez que `clean` s’affiche sous **tâches** dans l’Explorateur de l’exécuteur de **tâches**.

    ![Liste des tâches de l’Explorateur de l’exécuteur de tâches](using-grunt/_static/task-runner-explorer-tasks.png)

1. Cliquez avec le bouton droit sur la tâche nettoyer et sélectionnez **exécuter** dans le menu contextuel. Une fenêtre de commande affiche la progression de la tâche.

    ![tâche de nettoyage de l’Explorateur de l’exécuteur de tâches](using-grunt/_static/task-runner-explorer-run-clean.png)

    > [!NOTE]
    > Il n’y a pas encore de fichiers ou de répertoires à nettoyer. Si vous le souhaitez, vous pouvez les créer manuellement dans le Explorateur de solutions puis exécuter la tâche nettoyer en tant que test.

1. Dans la fonction `initConfig`, ajoutez une entrée pour `concat` à l’aide du code ci-dessous.

    Le tableau de propriétés `src` répertorie les fichiers à combiner, dans l’ordre dans lequel ils doivent être combinés. La propriété `dest` assigne le chemin d’accès au fichier combiné qui est produit.

    ```javascript
    concat: {
      all: {
        src: ['TypeScript/Tastes.js', 'TypeScript/Food.js'],
        dest: 'temp/combined.js'
      }
    },
    ```

    > [!NOTE]
    > La propriété `all` dans le code ci-dessus est le nom d’une cible. Les cibles sont utilisées dans certaines tâches grunt pour autoriser plusieurs environnements de génération. Vous pouvez afficher les cibles intégrées à l’aide d’IntelliSense ou attribuer les vôtres.

1. Ajoutez la tâche `jshint` à l’aide du code ci-dessous.

    L’utilitaire de `code-quality` jshint est exécuté sur chaque fichier JavaScript trouvé dans le répertoire *temp* .

    ```javascript
    jshint: {
      files: ['temp/*.js'],
      options: {
        '-W069': false,
      }
    },
    ```

    > [!NOTE]
    > L’option « -W069 » est une erreur produite par jshint lorsque JavaScript utilise la syntaxe des crochets pour assigner une propriété à la place de la notation par points, c.-à-d. `Tastes["Sweet"]` au lieu de `Tastes.Sweet`. L’option désactive l’avertissement pour permettre au reste du processus de continuer.

1. Ajoutez la tâche `uglify` à l’aide du code ci-dessous.

    La tâche minimise le fichier *. js combiné* situé dans le répertoire temporaire et crée le fichier de résultats dans wwwroot/lib conformément à la Convention d’affectation de noms standard *\<nom de fichier\>. min. js*.

    ```javascript
    uglify: {
     all: {
       src: ['temp/combined.js'],
       dest: 'wwwroot/lib/combined.min.js'
     }
    },
    ```

1. Sous l’appel à `grunt.loadNpmTasks` qui charge `grunt-contrib-clean`, incluez le même appel pour jshint, Concat et uglify à l’aide du code ci-dessous.

    ```javascript
    grunt.loadNpmTasks('grunt-contrib-jshint');
    grunt.loadNpmTasks('grunt-contrib-concat');
    grunt.loadNpmTasks('grunt-contrib-uglify');
    ```

1. Enregistrez *Gruntfile. js*. Le fichier doit ressembler à l’exemple ci-dessous.

    ![exemple de fichier grunt complet](using-grunt/_static/gruntfile-js-complete.png)

1. Notez que la liste tâches de l' **Explorateur de tâche Runner** comprend des tâches `clean`, `concat`, `jshint` et `uglify`. Exécutez chaque tâche dans l’ordre et observez les résultats dans **Explorateur de solutions**. Chaque tâche doit s’exécuter sans erreur.

    ![l’Explorateur de tâche Runner exécute chaque tâche](using-grunt/_static/task-runner-explorer-run-each-task.png)

    La tâche Concat crée un fichier *. js combiné* et le place dans le répertoire Temp. La tâche `jshint` s’exécute simplement et ne produit pas de sortie. La tâche `uglify` crée un nouveau fichier *. min. js combiné* et le place dans *wwwroot/lib*. Une fois l’opération terminée, la solution devrait ressembler à la capture d’écran ci-dessous :

    ![Explorateur de solutions après toutes les tâches](using-grunt/_static/solution-explorer-after-all-tasks.png)

    > [!NOTE]
    > Pour plus d’informations sur les options de chaque package, consultez [https://www.npmjs.com/](https://www.npmjs.com/) et recherchez le nom du package dans la zone de recherche de la page principale. Par exemple, vous pouvez consulter le grunt-pretrib-Clean package pour obtenir un lien vers la documentation qui décrit tous ses paramètres.

### <a name="all-together-now"></a>Ensemble maintenant

Utilisez la méthode grunt `registerTask()` pour exécuter une série de tâches dans une séquence particulière. Par exemple, pour exécuter les étapes de l’exemple ci-dessus dans l’ordre de nettoyage-> Concat-> jshint-> uglify, ajoutez le code ci-dessous au module. Le code doit être ajouté au même niveau que les appels loadNpmTasks (), en dehors de initConfig.

```javascript
grunt.registerTask("all", ['clean', 'concat', 'jshint', 'uglify']);
```

La nouvelle tâche apparaît dans l’Explorateur de l’exécuteur de tâches, sous tâches d’alias. Vous pouvez cliquer avec le bouton droit et l’exécuter exactement comme vous le feriez pour d’autres tâches. La tâche `all` s’exécute `clean`, `concat`, `jshint` et `uglify`, dans l’ordre.

![tâches grunt alias](using-grunt/_static/alias-tasks.png)

## <a name="watching-for-changes"></a>Détection des changements

Une tâche de `watch` garde un œil sur les fichiers et les répertoires. La montre déclenche automatiquement des tâches si elle détecte des modifications. Ajoutez le code ci-dessous à initConfig pour surveiller les modifications apportées aux fichiers \*. js dans le répertoire de machine à écrire. Si vous modifiez un fichier JavaScript, `watch` exécutera la tâche `all`.

```javascript
watch: {
  files: ["TypeScript/*.js"],
  tasks: ["all"]
}
```

Ajoutez un appel à `loadNpmTasks()` pour afficher la tâche `watch` dans l’Explorateur de l’exécuteur de tâches.

```javascript
grunt.loadNpmTasks('grunt-contrib-watch');
```

Cliquez avec le bouton droit sur la tâche espion dans l’Explorateur de tâches et sélectionnez Exécuter dans le menu contextuel. La fenêtre de commande qui affiche la tâche d’observation en cours d’exécution affiche une « attente... » Message. Ouvrez l’un des fichiers de machine à écrire, ajoutez un espace, puis enregistrez le fichier. Cela déclenche la tâche Watch et déclenche l’exécution des autres tâches dans l’ordre. La capture d’écran ci-dessous montre un exemple d’exécution.

![sortie des tâches en cours d’exécution](using-grunt/_static/watch-running.png)

## <a name="binding-to-visual-studio-events"></a>Lier à des événements Visual Studio

À moins que vous ne souhaitiez démarrer vos tâches manuellement chaque fois que vous travaillez dans Visual Studio, liez des tâches à **avant la génération**, après les événements de **génération**, de **nettoyage**et d' **ouverture de projet** .

Liez `watch` pour qu’il s’exécute chaque fois que Visual Studio s’ouvre. Dans l’Explorateur de l’exécuteur de tâches, cliquez avec le bouton droit sur la tâche espion et sélectionnez **liaisons** > **projet ouvrir** dans le menu contextuel.

![lier une tâche à l’ouverture du projet](using-grunt/_static/bindings-project-open.png)

Déchargez et rechargez le projet. Lorsque le projet se recharge, la tâche espion commence automatiquement.

## <a name="summary"></a>Résumé

Grunt est un testeur de tâches puissant qui peut être utilisé pour automatiser la plupart des tâches de génération de clients. Grunt tire parti de NPM pour fournir ses packages et propose des fonctionnalités d’intégration avec Visual Studio. L’Explorateur de tâche Runner de Visual Studio détecte les modifications apportées aux fichiers de configuration et fournit une interface pratique pour exécuter des tâches, afficher des tâches en cours d’exécution et lier des tâches à des événements Visual Studio.
