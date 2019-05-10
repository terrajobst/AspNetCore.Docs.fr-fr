---
title: Utiliser l’interface de ligne de commande (CLI) LibMan avec ASP.NET Core
author: scottaddie
description: Découvrez comment utiliser l’interface de ligne de commande (CLI) LibMan dans un projet ASP.NET Core.
ms.author: scaddie
ms.custom: mvc
ms.date: 08/30/2018
uid: client-side/libman/libman-cli
ms.openlocfilehash: 5667f79648a60b8fd9496f8041ef08891ab766af
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/27/2019
ms.locfileid: "64894756"
---
# <a name="use-the-libman-command-line-interface-cli-with-aspnet-core"></a>Utiliser l’interface de ligne de commande (CLI) LibMan avec ASP.NET Core

Par [Scott Addie](https://twitter.com/Scott_Addie)

Le [LibMan](xref:client-side/libman/index) CLI est un outil multiplateforme qui a pris en charge partout où .NET Core est pris en charge.

## <a name="prerequisites"></a>Prérequis

* [!INCLUDE [2.1-SDK](../../includes/2.1-SDK.md)]

## <a name="installation"></a>Installation

Pour installer la CLI LibMan :

```console
dotnet tool install -g Microsoft.Web.LibraryManager.Cli
```

Un [outil Global de .NET Core](/dotnet/core/tools/global-tools#install-a-global-tool) est installé à partir de la [Microsoft.Web.LibraryManager.Cli](https://www.nuget.org/packages/Microsoft.Web.LibraryManager.Cli/) package NuGet.

Pour installer la CLI LibMan à partir d’une source de package NuGet spécifique :

```console
dotnet tool install -g Microsoft.Web.LibraryManager.Cli --version 1.0.94-g606058a278 --add-source C:\Temp\
```

Dans l’exemple précédent, un outil Global de .NET Core est installé à partir de l’ordinateur local Windows *C:\Temp\Microsoft.Web.LibraryManager.Cli.1.0.94-g606058a278.nupkg* fichier.

## <a name="usage"></a>Utilisation

Après l’installation de l’interface CLI, la commande suivante peut être utilisée :

```console
libman
```

Pour afficher la version CLI installée :

```console
libman --version
```

Pour afficher les commandes CLI disponibles :

```console
libman --help
```

La commande précédente affiche une sortie similaire à ce qui suit :

```console
 1.0.163+g45474d37ed

Usage: libman [options] [command]

Options:
  --help|-h  Show help information
  --version  Show version information

Commands:
  cache      List or clean libman cache contents
  clean      Deletes all library files defined in libman.json from the project
  init       Create a new libman.json
  install    Add a library definition to the libman.json file, and download the 
             library to the specified location
  restore    Downloads all files from provider and saves them to specified 
             destination
  uninstall  Deletes all files for the specified library from their specified 
             destination, then removes the specified library definition from 
             libman.json
  update     Updates the specified library

Use "libman [command] --help" for more information about a command.
```

Les sections suivantes décrivent les commandes CLI disponibles.

## <a name="initialize-libman-in-the-project"></a>Initialiser LibMan dans le projet

Le `libman init` commande crée un *libman.json* fichier s’il n’existe. Le fichier est créé avec le contenu de modèle d’élément par défaut.

### <a name="synopsis"></a>Résumé

```console
libman init [-d|--default-destination] [-p|--default-provider] [--verbosity]
libman init [-h|--help]
```

### <a name="options"></a>Options

Les options suivantes sont disponibles pour le `libman init` commande :

* `-d|--default-destination <PATH>`

  Un chemin d’accès relatif au dossier actuel. Fichiers de bibliothèque sont installés à cet emplacement si aucun `destination` propriété est définie pour une bibliothèque dans *libman.json*. Le `<PATH>` valeur est écrite dans le `defaultDestination` propriété du *libman.json*.

* `-p|--default-provider <PROVIDER>`

  Le fournisseur à utiliser si aucun fournisseur n’est défini pour une bibliothèque donnée. Le `<PROVIDER>` valeur est écrite dans le `defaultProvider` propriété du *libman.json*. Remplacez `<PROVIDER>` avec l’une des valeurs suivantes :

  [!INCLUDE [LibMan provider names](../../includes/libman-cli/provider-names.md)]

[!INCLUDE [standard-cli-options](../../includes/libman-cli/standard-cli-options.md)]

### <a name="examples"></a>Exemples

Pour créer un *libman.json* fichier dans un projet ASP.NET Core :

* Accédez à la racine du projet.
* Exécutez la commande suivante :

  ```console
  libman init
  ```

* Tapez le nom du fournisseur de valeur par défaut, ou appuyez sur `Enter` pour utiliser le fournisseur CDNJS par défaut. Les valeurs valides sont les suivantes :

  [!INCLUDE [LibMan provider names](../../includes/libman-cli/provider-names.md)]

  ![commande d’init libman - fournisseur par défaut](_static/libman-init-provider.png)

Un *libman.json* fichier est ajouté à la racine du projet avec le contenu suivant :

```json
{
  "version": "1.0",
  "defaultProvider": "cdnjs",
  "libraries": []
}
```

## <a name="add-library-files"></a>Ajouter des fichiers de bibliothèque

Le `libman install` commande télécharge et installe les fichiers de bibliothèque dans le projet. Un *libman.json* fichier est ajouté s’il n’existe. Le *libman.json* fichier est modifié pour stocker les détails de configuration pour les fichiers de bibliothèque.

### <a name="synopsis"></a>Résumé

```console
libman install <LIBRARY> [-d|--destination] [--files] [-p|--provider] [--verbosity]
libman install [-h|--help]
```

### <a name="arguments"></a>Arguments

`LIBRARY`

Le nom de la bibliothèque à installer. Ce nom peut inclure la notation de numéro de version (par exemple, `@1.2.0`).

### <a name="options"></a>Options

Les options suivantes sont disponibles pour le `libman install` commande :

* `-d|--destination <PATH>`

  L’emplacement d’installation de la bibliothèque. Si non spécifié, l’emplacement par défaut est utilisé. Si aucun `defaultDestination` propriété est spécifiée dans *libman.json*, cette option est requise.

* `--files <FILE>`

  Spécifiez le nom du fichier à installer à partir de la bibliothèque. Si non spécifié, tous les fichiers à partir de la bibliothèque sont installés. Fournir un `--files` option par fichier doit être installé. Chemins d’accès relatifs sont trop pris en charge. Par exemple : `--files dist/browser/signalr.js`.

* `-p|--provider <PROVIDER>`

  Le nom du fournisseur à utiliser pour l’acquisition de la bibliothèque. Remplacez `<PROVIDER>` avec l’une des valeurs suivantes :
  
  [!INCLUDE [LibMan provider names](../../includes/libman-cli/provider-names.md)]

  Si non spécifié, le `defaultProvider` propriété dans *libman.json* est utilisé. Si aucun `defaultProvider` propriété est spécifiée dans *libman.json*, cette option est requise.

[!INCLUDE [standard-cli-options](../../includes/libman-cli/standard-cli-options.md)]

### <a name="examples"></a>Exemples

Considérez les éléments suivants *libman.json* fichier :

```json
{
  "version": "1.0",
  "defaultProvider": "cdnjs",
  "libraries": []
}
```

Pour installer la version de jQuery 3.2.1 *jquery.min.js* de fichiers à la *wwwroot/scripts/jquery* dossier à l’aide du fournisseur CDNJS :

```console
libman install jquery@3.2.1 --provider cdnjs --destination wwwroot/scripts/jquery --files jquery.min.js
```

Le *libman.json* fichier ressemble à ceci :

```json
{
  "version": "1.0",
  "defaultProvider": "cdnjs",
  "libraries": [
    {
      "library": "jquery@3.2.1",
      "destination": "wwwroot/scripts/jquery",
      "files": [
        "jquery.min.js"
      ]
    }
  ]
}
```

Pour installer le *calendar.js* et *calendar.css* fichiers à partir de *C:\\temp\\contosoCalendar\\*  à l’aide du système de fichiers fournisseur :

  ```console
  libman install C:\temp\contosoCalendar\ --provider filesystem --files calendar.js --files calendar.css
  ```

L’invite suivante s’affiche pour deux raisons :

* Le *libman.json* fichier ne contient pas un `defaultDestination` propriété.
* Le `libman install` commande ne contient pas le `-d|--destination` option.

![libman installer command - destination](_static/libman-install-destination.png)

Après avoir accepté la destination par défaut, le *libman.json* fichier ressemble à ceci :

```json
{
  "version": "1.0",
  "defaultProvider": "cdnjs",
  "libraries": [
    {
      "library": "jquery@3.2.1",
      "destination": "wwwroot/scripts/jquery",
      "files": [
        "jquery.min.js"
      ]
    },
    {
      "library": "C:\\temp\\contosoCalendar\\",
      "provider": "filesystem",
      "destination": "wwwroot/lib/contosoCalendar",
      "files": [
        "calendar.js",
        "calendar.css"
      ]
    }
  ]
}
```

## <a name="restore-library-files"></a>Restaurer des fichiers de bibliothèque

Le `libman restore` commande installe les fichiers de bibliothèque définies dans *libman.json*. Les règles suivantes s'appliquent :

* Si aucun *libman.json* fichier existe à la racine du projet, une erreur est retournée.
* Si une bibliothèque spécifie un fournisseur, le `defaultProvider` propriété dans *libman.json* est ignoré.
* Si une bibliothèque spécifie un type de destination, le `defaultDestination` propriété dans *libman.json* est ignoré.

### <a name="synopsis"></a>Résumé

```console
libman restore [--verbosity]
libman restore [-h|--help]
```

### <a name="options"></a>Options

Les options suivantes sont disponibles pour le `libman restore` commande :

[!INCLUDE [standard-cli-options](../../includes/libman-cli/standard-cli-options.md)]

### <a name="examples"></a>Exemples

Pour restaurer les fichiers de bibliothèque définies dans *libman.json*:

```console
libman restore
```

## <a name="delete-library-files"></a>Supprimer les fichiers de bibliothèque

Le `libman clean` commande supprime les fichiers de bibliothèque précédemment restaurés via LibMan. Les dossiers qui devient vides après cette opération sont supprimés. Les fichiers de bibliothèque associés des configurations dans le `libraries` propriété du *libman.json* ne sont pas supprimées.

### <a name="synopsis"></a>Résumé

```console
libman clean [--verbosity]
libman clean [-h|--help]
```

### <a name="options"></a>Options

Les options suivantes sont disponibles pour le `libman clean` commande :

[!INCLUDE [standard-cli-options](../../includes/libman-cli/standard-cli-options.md)]

### <a name="examples"></a>Exemples

Pour supprimer les fichiers de bibliothèque installés via LibMan :

```console
libman clean
```

## <a name="uninstall-library-files"></a>Désinstaller les fichiers de bibliothèque

Le `libman uninstall` commande :

* Supprime tous les fichiers associés à la bibliothèque spécifiée à partir de la destination dans *libman.json*.
* Supprime la configuration de la bibliothèque associée à partir de *libman.json*.

Une erreur se produit lorsque :

* Ne *libman.json* fichier existe à la racine du projet.
* La bibliothèque spécifiée n’existe pas.

Si plus d’une bibliothèque portant le même nom est installée, vous êtes invité à choisir une.

### <a name="synopsis"></a>Résumé

```console
libman uninstall <LIBRARY> [--verbosity]
libman uninstall [-h|--help]
```

### <a name="arguments"></a>Arguments

`LIBRARY`

Le nom de la bibliothèque à désinstaller. Ce nom peut inclure la notation de numéro de version (par exemple, `@1.2.0`).

### <a name="options"></a>Options

Les options suivantes sont disponibles pour le `libman uninstall` commande :

[!INCLUDE [standard-cli-options](../../includes/libman-cli/standard-cli-options.md)]

### <a name="examples"></a>Exemples

Considérez les éléments suivants *libman.json* fichier :

[!code-json[](samples/LibManSample/libman.json)]

* Pour désinstaller jQuery, des commandes suivantes réussissent :

  ```console
  libman uninstall jquery
  ```

  ```console
  libman uninstall jquery@3.3.1
  ```

* Pour désinstaller les fichiers Lodash installés via le `filesystem` fournisseur :

  ```console
  libman uninstall C:\temp\lodash\
  ```

## <a name="update-library-version"></a>Mettre à jour la version de la bibliothèque

Le `libman update` commande met à jour une bibliothèque installée via LibMan à la version spécifiée.

Une erreur se produit lorsque :

* Ne *libman.json* fichier existe à la racine du projet.
* La bibliothèque spécifiée n’existe pas.

Si plus d’une bibliothèque portant le même nom est installée, vous êtes invité à choisir une.

### <a name="synopsis"></a>Résumé

```console
libman update <LIBRARY> [-pre] [--to] [--verbosity]
libman update [-h|--help]
```

### <a name="arguments"></a>Arguments

`LIBRARY`

Le nom de la bibliothèque pour mettre à jour.

### <a name="options"></a>Options

Les options suivantes sont disponibles pour le `libman update` commande :

* `-pre`

  Obtenir la dernière version préliminaire de la bibliothèque.

* `--to <VERSION>`

  Obtenir une version spécifique de la bibliothèque.

[!INCLUDE [standard-cli-options](../../includes/libman-cli/standard-cli-options.md)]

### <a name="examples"></a>Exemples

* Pour mettre à jour de jQuery vers la dernière version :

  ```console
  libman update jquery
  ```

* Pour mettre à jour de jQuery vers la version 3.3.1 :

  ```console
  libman update jquery --to 3.3.1
  ```

* Pour mettre à jour de jQuery vers la dernière version préliminaire :

  ```console
  libman update jquery -pre
  ```

## <a name="manage-library-cache"></a>Gérer le cache de la bibliothèque

Le `libman cache` commande gère le cache de la bibliothèque LibMan. Le `filesystem` fournisseur n’utilise pas le cache de la bibliothèque.

### <a name="synopsis"></a>Résumé

```console
libman cache clean [<PROVIDER>] [--verbosity]
libman cache list [--files] [--libraries] [--verbosity]
libman cache [-h|--help]
```

### <a name="arguments"></a>Arguments

`PROVIDER`

Utilisé uniquement avec la `clean` commande. Spécifie le cache du fournisseur à nettoyer. Les valeurs valides sont les suivantes :

[!INCLUDE [LibMan provider names](../../includes/libman-cli/provider-names.md)]

### <a name="options"></a>Options

Les options suivantes sont disponibles pour le `libman cache` commande :

* `--files`

  Répertorier les noms des fichiers qui sont mis en cache.

* `--libraries`

  Répertorier les noms des bibliothèques qui sont mis en cache.

[!INCLUDE [standard-cli-options](../../includes/libman-cli/standard-cli-options.md)]

### <a name="examples"></a>Exemples

* Pour afficher les noms de bibliothèques mis en cache par le fournisseur, utilisez une des commandes suivantes :

  ```console
  libman cache list
  ```

  ```console
  libman cache list --libraries
  ```

  Une sortie similaire à la suivante s’affiche à l’écran :

  ```console
  Cache contents:
  ---------------
  unpkg:
      knockout
      react
      vue
  cdnjs:
      font-awesome
      jquery
      knockout
      lodash.js
      react
  ```

* Pour afficher les noms de fichiers de bibliothèque mis en cache par le fournisseur :

  ```console
  libman cache list --files
  ```

  Une sortie similaire à la suivante s’affiche à l’écran :

  ```console
  Cache contents:
  ---------------
  unpkg:
      knockout:
          <list omitted for brevity>
      react:
          <list omitted for brevity>
      vue:
          <list omitted for brevity>
  cdnjs:
      font-awesome
          metadata.json
      jquery
          metadata.json
          3.2.1\core.js
          3.2.1\jquery.js
          3.2.1\jquery.min.js
          3.2.1\jquery.min.map
          3.2.1\jquery.slim.js
          3.2.1\jquery.slim.min.js
          3.2.1\jquery.slim.min.map
          3.3.1\core.js
          3.3.1\jquery.js
          3.3.1\jquery.min.js
          3.3.1\jquery.min.map
          3.3.1\jquery.slim.js
          3.3.1\jquery.slim.min.js
          3.3.1\jquery.slim.min.map
      knockout
          metadata.json
          3.4.2\knockout-debug.js
          3.4.2\knockout-min.js
      lodash.js
          metadata.json
          4.17.10\lodash.js
          4.17.10\lodash.min.js
      react
          metadata.json
  ```

  Notez que la sortie précédente montre que jQuery versions 3.2.1 et 3.3.1 sont mis en cache sous le fournisseur CDNJS.

* Pour vider le cache de la bibliothèque pour le fournisseur CDNJS :

  ```console
  libman cache clean cdnjs
  ```

  Après avoir vidé le cache du fournisseur CDNJS, le `libman cache list` commande affiche les éléments suivants :

  ```console
  Cache contents:
  ---------------
  unpkg:
      knockout
      react
      vue
  cdnjs:
      (empty)
  ```

* Pour vider le cache pour tous les fournisseurs pris en charge :

  ```console
  libman cache clean
  ```

  Après avoir vidé tous les caches de fournisseur, le `libman cache list` commande affiche les éléments suivants :

  ```console
  Cache contents:
  ---------------
  unpkg:
      (empty)
  cdnjs:
      (empty)
  ```

## <a name="additional-resources"></a>Ressources supplémentaires

* [Installer un outil Global](/dotnet/core/tools/global-tools#install-a-global-tool)
* <xref:client-side/libman/libman-vs>
* [Dépôt GitHub LibMan](https://github.com/aspnet/LibraryManager)
