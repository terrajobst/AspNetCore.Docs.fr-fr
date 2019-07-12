---
title: Utiliser Grunt dans ASP.NET Core
author: rick-anderson
description: Utiliser Grunt dans ASP.NET Core
ms.author: riande
ms.date: 06/18/2019
uid: client-side/using-grunt
ms.openlocfilehash: f3832bd1fe5721fbda114103ac11a8d55312bcb2
ms.sourcegitcommit: 8516b586541e6ba402e57228e356639b85dfb2b9
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67813548"
---
# <a name="use-grunt-in-aspnet-core"></a>Utiliser Grunt dans ASP.NET Core

Grunt est un exécuteur de tâches JavaScript qui automatise la minimisation de script, la compilation de TypeScript, qualité du code, outils de la moulinette « lint », les processeurs préliminaire CSS et pratiquement n’importe quel répétitive, qui doit effectuer pour prendre en charge le développement de clients. Grunt est entièrement pris en charge dans Visual Studio.

Cet exemple utilise un projet ASP.NET Core vide comme point de départ, pour montrer comment automatiser le processus de génération de client à partir de zéro.

L’exemple terminé nettoie le répertoire de déploiement cible, combine les fichiers JavaScript, vérifie la qualité du code, condense le contenu d’un fichier JavaScript et déploie à la racine de votre application web. Nous allons utiliser les packages suivants :

* **Grunt**: Le package de test runner de tâches Grunt.

* **Grunt-cotisation-clean**: Un plug-in qui supprime des fichiers ou répertoires.

* **Grunt-cotisation-jshint**: Un plug-in qui passe en revue la qualité du code JavaScript.

* **Grunt-cotisation-concat**: Un plug-in qui regroupe des fichiers dans un seul fichier.

* **Grunt-cotisation-uglify**: Un plug-in qui minimise le code JavaScript pour réduire la taille.

* **Grunt-cotisation-watch**: Un plug-in qui surveille l’activité de fichier.

## <a name="preparing-the-application"></a>Préparation de l’application

Pour commencer, configurez une nouvelle application web vide et ajouter des fichiers d’exemple TypeScript. Les fichiers TypeScript sont automatiquement compilés dans JavaScript à l’aide des paramètres de Visual Studio par défaut et seront notre matières premières à traiter à l’aide de Grunt.

1. Dans Visual Studio, créez un nouveau `ASP.NET Web Application`.

2. Dans le **nouveau projet ASP.NET** boîte de dialogue, sélectionnez ASP.NET Core **vide** modèle et cliquez sur le bouton OK.

3. Dans l’Explorateur de solutions, passez en revue la structure de projet. Le `\src` dossier inclut vide `wwwroot` et `Dependencies` nœuds.

    ![solution de site web vide](using-grunt/_static/grunt-solution-explorer.png)

4. Ajoutez un nouveau dossier nommé `TypeScript` à votre répertoire de projet.

5. Avant d’ajouter tous les fichiers, assurez-vous que Visual Studio a l’option « compiler lors de l’enregistrement » pour les fichiers TypeScript activées. Accédez à **outils** > **Options** > **éditeur de texte** > **Typescript**  >  **Projet**:

    ![Options de compilation automatique des fichiers de TypeScript](using-grunt/_static/typescript-options.png)

6. Cliquez sur le `TypeScript` répertoire et sélectionnez **Ajouter > nouvel élément** dans le menu contextuel. Sélectionnez le **fichier JavaScript** d’élément et nommez le fichier *Tastes.ts* (Notez le \*extension .ts). Copiez la ligne de code TypeScript ci-dessous dans le fichier (lorsque vous enregistrez, un nouveau *Tastes.js* fichier s’affiche avec la source de JavaScript).

    ```typescript
    enum Tastes { Sweet, Sour, Salty, Bitter }
    ```

7. Ajoutez un deuxième fichier dans le **TypeScript** directory et nommez-le `Food.ts`. Copiez le code ci-dessous dans le fichier.

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

Ensuite, configurez NPM pour télécharger grunt et tâches de grunt.

1. Dans l’Explorateur de solutions, cliquez sur le projet et sélectionnez **Ajouter > nouvel élément** dans le menu contextuel. Sélectionnez le **fichier de configuration de NPM** d’élément, laissez le nom par défaut, *package.json*, puis cliquez sur le **ajouter** bouton.

2. Dans le *package.json* de fichiers, à l’intérieur du `devDependencies` accolades d’objet, entrez « grunt ». Sélectionnez `grunt` dans Intellisense liste et appuyez sur la touche ENTRÉE. Visual Studio citer le nom du package grunt et ajouter un signe deux-points. À droite du signe deux-points, sélectionnez la dernière version stable du package à partir du haut de la liste Intellisense (appuyez sur `Ctrl-Space` si Intellisense n’apparaît pas).

    ![Grunt Intellisense](using-grunt/_static/devdependencies-grunt.png)

    > [!NOTE]
    > NPM utilise [semver](https://semver.org/) pour organiser les dépendances. Le contrôle de version sémantique, également appelé SemVer, identifie les packages avec le modèle de numérotation \<majeure >.\< mineure >. \<correctif >. IntelliSense simplifie la gestion sémantique de version en affichant uniquement quelques choix répandus en matière. Le premier élément dans la liste Intellisense (0.4.5 dans l’exemple ci-dessus) est considéré comme la dernière version stable du package. Le symbole de signe insertion (^) correspond à la version majeure la plus récente et le tilde (~) correspond à la version mineure la plus récente. Consultez le [référence d’analyseur NPM semver version](https://www.npmjs.com/package/semver) comme guide pour l’expressivité complète qui fournit de SemVer.

3. Ajouter plus de dépendances pour charger grunt-cotisation -\* packages pour *propre*, *jshint*, *concat*, *uglify*et *espion* comme indiqué dans l’exemple ci-dessous. Les versions n’avez pas besoin de correspondre à l’exemple.

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

4. Enregistrer le *package.json* fichier.

Les packages pour chaque `devDependencies` élément télécharge, ainsi que tous les fichiers qui nécessite de chaque package. Vous trouverez les fichiers du package dans le *node_modules* répertoire en activant la **afficher tous les fichiers** situé dans **l’Explorateur de solutions**.

![node_modules de Grunt](using-grunt/_static/node-modules.png)

> [!NOTE]
> Si vous le souhaitez, vous pouvez restaurer manuellement les dépendances dans **l’Explorateur de solutions** en cliquant sur `Dependencies\NPM` et en sélectionnant le **restaurer les Packages** option de menu.

![restaurer des packages](using-grunt/_static/restore-packages.png)

## <a name="configuring-grunt"></a>Configuration Grunt

Grunt est configuré à l’aide d’un manifeste nommé *Gruntfile.js* qui définit, charge et enregistre les tâches qui peuvent être exécutés manuellement ou configurés pour exécuter automatiquement en fonction des événements dans Visual Studio.

1. Cliquez sur le projet et sélectionnez **ajouter** > **un nouvel élément**. Sélectionnez le **fichier JavaScript** modèle d’élément, remplacez le nom par *Gruntfile.js*, puis cliquez sur le **ajouter** bouton.

1. Ajoutez le code suivant à *Gruntfile.js*. Le `initConfig` fonction définit des options pour chaque package et le reste du module charge et inscrire des tâches.

   ```javascript
   module.exports = function (grunt) {
     grunt.initConfig({
     });
   };
   ```

1. À l’intérieur de la `initConfig` de fonction, ajouter des options pour le `clean` comme indiqué dans l’exemple de tâches *Gruntfile.js* ci-dessous. Le `clean` tâche accepte un tableau de chaînes de répertoire. Cette tâche supprime les fichiers à partir de *wwwroot/lib* et supprime l’intégralité de */temp* directory.

    ```javascript
    module.exports = function (grunt) {
      grunt.initConfig({
        clean: ["wwwroot/lib/*", "temp/"],
      });
    };
    ```

1. Ci-dessous le `initConfig` de fonction, ajoutez un appel à `grunt.loadNpmTasks`. Cela rendra la tâche exécutable à partir de Visual Studio.

    ```javascript
    grunt.loadNpmTasks("grunt-contrib-clean");
    ```

1. Enregistrer *Gruntfile.js*. Le fichier doit ressembler à la capture d’écran ci-dessous.

    ![gruntfile initiale](using-grunt/_static/gruntfile-js-initial.png)

1. Avec le bouton droit *Gruntfile.js* et sélectionnez **Task Runner Explorer** dans le menu contextuel. Le **Task Runner Explorer** fenêtre s’ouvre.

    ![menu de l’Explorateur exécuteur de tâche](using-grunt/_static/task-runner-explorer-menu.png)

1. Vérifiez que `clean` s’affiche sous **tâches** dans le **Task Runner Explorer**.

    ![liste des tâches tâches runner explorer](using-grunt/_static/task-runner-explorer-tasks.png)

1. Avec le bouton droit de la tâche clean et sélectionnez **exécuter** dans le menu contextuel. Une fenêtre de commande affiche la progression de la tâche.

    ![tâche de nettoyage de tâche runner explorer exécuter](using-grunt/_static/task-runner-explorer-run-clean.png)

    > [!NOTE]
    > Il n’existe aucun fichier ou répertoire pour nettoyer encore. Si vous le souhaitez, vous pouvez les créer manuellement dans l’Explorateur de solutions, puis exécutez la tâche clean comme un test.

1. Dans le `initConfig` de fonction, ajoutez une entrée pour `concat` en utilisant le code ci-dessous.

    Le `src` tableau de propriétés répertorie les fichiers de combiner, dans l’ordre qu’ils doivent être combinées. Le `dest` propriété affecte le chemin d’accès au fichier combiné qui est généré.

    ```javascript
    concat: {
      all: {
        src: ['TypeScript/Tastes.js', 'TypeScript/Food.js'],
        dest: 'temp/combined.js'
      }
    },
    ```

    > [!NOTE]
    > Le `all` propriété dans le code ci-dessus est le nom d’une cible. Cibles dans certaines tâches Grunt servent à autoriser plusieurs environnements de génération. Vous pouvez afficher les cibles intégrées à l’aide d’IntelliSense ou affecter les vôtres.

1. Ajouter le `jshint` de tâches en utilisant le code ci-dessous.

    Le jshint `code-quality` utilitaire est exécuté sur chaque fichier JavaScript dans le *temp* directory.

    ```javascript
    jshint: {
      files: ['temp/*.js'],
      options: {
        '-W069': false,
      }
    },
    ```

    > [!NOTE]
    > L’option «-W069 » est une erreur produite par jshint lorsque JavaScript utilise mettre entre parenthèses la syntaxe pour assigner une propriété au lieu de la notation à points, par exemple, `Tastes["Sweet"]` au lieu de `Tastes.Sweet`. L’option désactive l’avertissement pour autoriser le reste du processus pour continuer.

1. Ajouter le `uglify` de tâches en utilisant le code ci-dessous.

    La tâche minimise le *combined.js* fichier trouvé dans le répertoire temp et crée le fichier de résultats dans wwwroot/lib conformément à la convention d’affectation de noms standard  *\<nom de fichier\>. min.js*.

    ```javascript
    uglify: {
     all: {
       src: ['temp/combined.js'],
       dest: 'wwwroot/lib/combined.min.js'
     }
    },
    ```

1. Sous l’appel à `grunt.loadNpmTasks` qui charge `grunt-contrib-clean`, incluent le même appel pour jshint, concat et uglify en utilisant le code ci-dessous.

    ```javascript
    grunt.loadNpmTasks('grunt-contrib-jshint');
    grunt.loadNpmTasks('grunt-contrib-concat');
    grunt.loadNpmTasks('grunt-contrib-uglify');
    ```

1. Enregistrer *Gruntfile.js*. Le fichier doit ressembler à l’exemple ci-dessous.

    ![exemple de fichier complet grunt](using-grunt/_static/gruntfile-js-complete.png)

1. Notez que le **Task Runner Explorer** liste de tâches inclut `clean`, `concat`, `jshint` et `uglify` tâches. Exécuter chaque tâche dans l’ordre et observez les résultats dans **l’Explorateur de solutions**. Chaque tâche doit s’exécuter sans erreur.

    ![Explorateur d’exécuteur de tâches exécuter chaque tâche](using-grunt/_static/task-runner-explorer-run-each-task.png)

    La tâche concat crée un nouveau *combined.js* de fichier et le place dans le répertoire temporaire. Le `jshint` tâche simplement s’exécute et ne produit pas sortie. Le `uglify` tâche crée un nouveau *combined.min.js* de fichier et le place dans *wwwroot/lib*. L’opération, la solution doit ressembler à la capture d’écran ci-dessous :

    ![une fois toutes les tâches de l’Explorateur de solutions](using-grunt/_static/solution-explorer-after-all-tasks.png)

    > [!NOTE]
    > Pour plus d’informations sur les options pour chaque package, visitez [ https://www.npmjs.com/ ](https://www.npmjs.com/) et recherche le nom du package dans la zone de recherche dans la page principale. Par exemple, vous pouvez rechercher le package de grunt-cotisation-clean pour obtenir un lien vers la documentation qui explique tous ses paramètres.

### <a name="all-together-now"></a>Maintenant tous ensemble !

Utiliser le Grunt `registerTask()` méthode à exécuter une série de tâches dans une séquence particulière. Par exemple, pour exécuter l’exemple -> des étapes ci-dessus dans l’ordre propre concat -> jshint -> uglify, ajoutez le code ci-dessous au module. Le code doit être ajouté au même niveau que les appels loadNpmTasks(), à l’extérieur initConfig.

```javascript
grunt.registerTask("all", ['clean', 'concat', 'jshint', 'uglify']);
```

La nouvelle tâche s’affiche dans Task Runner Explorer sous tâches de l’Alias. Vous pouvez avec le bouton droit et l’exécuter comme vous le feriez autres tâches. Le `all` tâche s’exécutera `clean`, `concat`, `jshint` et `uglify`, dans l’ordre.

![tâches d’alias grunt](using-grunt/_static/alias-tasks.png)

## <a name="watching-for-changes"></a>Détection des changements

Un `watch` tâche garde un œil sur les fichiers et répertoires. L’observation déclenche automatiquement des tâches s’il détecte des modifications. Ajoutez le code ci-dessous à initConfig pour surveiller les modifications apportées aux \*fichiers .js dans le répertoire de TypeScript. Si un fichier JavaScript est modifié, `watch` exécutera le `all` tâche.

```javascript
watch: {
  files: ["TypeScript/*.js"],
  tasks: ["all"]
}
```

Ajoutez un appel à `loadNpmTasks()` pour afficher le `watch` tâche dans l’Explorateur d’exécuteur de tâche.

```javascript
grunt.loadNpmTasks('grunt-contrib-watch');
```

Avec le bouton droit de la tâche espion dans Task Runner Explorer et sélectionnez Exécuter dans le menu contextuel. La fenêtre de commande qui affiche l’exécution des tâches Espion affiche un paramètre « en attente... » Message. Ouvrez un des fichiers TypeScript, ajoutez un espace, puis enregistrez le fichier. Cela déclenche la tâche espion et déclencher les autres tâches à exécuter dans l’ordre. La capture d’écran ci-dessous montre un exemple d’exécution.

![sortie des tâches en cours d’exécution](using-grunt/_static/watch-running.png)

## <a name="binding-to-visual-studio-events"></a>Liaison à des événements de Visual Studio

Sauf si vous souhaitez démarrer manuellement vos tâches chaque fois que vous travaillez dans Visual Studio, vous pouvez lier des tâches à **avant de générer**, **après génération**, **Clean**, et  **Projet Open** événements.

Nous allons lier `watch` afin qu’il exécute chaque fois que Visual Studio s’ouvre. Dans l’Explorateur d’exécuteur de tâche, avec le bouton droit de la tâche de surveillance et sélectionnez **liaisons > projet ouvert** dans le menu contextuel.

![lier une tâche à l’ouverture du projet](using-grunt/_static/bindings-project-open.png)

Déchargez et rechargez le projet. Lorsque le projet se charge là encore, la tâche espion commence à s’exécuter automatiquement.

## <a name="summary"></a>Récapitulatif

Grunt est un exécuteur de tâches puissantes qui peut être utilisé pour automatiser la plupart des tâches de génération de client. Grunt tire parti de NPM pour proposer ses packages, fonctionnalités et outils d’intégration avec Visual Studio. Task Runner Explorer de Visual Studio détecte les modifications apportées aux fichiers de configuration et fournit une interface pratique pour exécuter des tâches, afficher les tâches en cours d’exécution et lier des tâches aux événements de Visual Studio.
