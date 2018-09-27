---
title: Utiliser l’interface de ligne de commande (CLI) LibMan avec ASP.NET Core
author: scottaddie
description: Découvrez comment utiliser l’interface de ligne de commande (CLI) LibMan dans un projet ASP.NET Core.
ms.author: scaddie
ms.custom: mvc
ms.date: 08/30/2018
uid: client-side/libman/libman-cli
ms.openlocfilehash: 5667f79648a60b8fd9496f8041ef08891ab766af
ms.sourcegitcommit: 9bdba90b2c97a4016188434657194b2d7027d6e3
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2018
ms.locfileid: "47402083"
---
# <a name="use-the-libman-command-line-interface-cli-with-aspnet-core"></a><span data-ttu-id="95890-103">Utiliser l’interface de ligne de commande (CLI) LibMan avec ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="95890-103">Use the LibMan command-line interface (CLI) with ASP.NET Core</span></span>

<span data-ttu-id="95890-104">Par [Scott Addie](https://twitter.com/Scott_Addie)</span><span class="sxs-lookup"><span data-stu-id="95890-104">By [Scott Addie](https://twitter.com/Scott_Addie)</span></span>

<span data-ttu-id="95890-105">Le [LibMan](xref:client-side/libman/index) CLI est un outil multiplateforme qui a pris en charge partout où .NET Core est pris en charge.</span><span class="sxs-lookup"><span data-stu-id="95890-105">The [LibMan](xref:client-side/libman/index) CLI is a cross-platform tool that's supported everywhere .NET Core is supported.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="95890-106">Prérequis</span><span class="sxs-lookup"><span data-stu-id="95890-106">Prerequisites</span></span>

* [!INCLUDE [2.1-SDK](../../includes/2.1-SDK.md)]

## <a name="installation"></a><span data-ttu-id="95890-107">Installation</span><span class="sxs-lookup"><span data-stu-id="95890-107">Installation</span></span>

<span data-ttu-id="95890-108">Pour installer la CLI LibMan :</span><span class="sxs-lookup"><span data-stu-id="95890-108">To install the LibMan CLI:</span></span>

```console
dotnet tool install -g Microsoft.Web.LibraryManager.Cli
```

<span data-ttu-id="95890-109">Un [outil Global de .NET Core](/dotnet/core/tools/global-tools#install-a-global-tool) est installé à partir de la [Microsoft.Web.LibraryManager.Cli](https://www.nuget.org/packages/Microsoft.Web.LibraryManager.Cli/) package NuGet.</span><span class="sxs-lookup"><span data-stu-id="95890-109">A [.NET Core Global Tool](/dotnet/core/tools/global-tools#install-a-global-tool) is installed from the [Microsoft.Web.LibraryManager.Cli](https://www.nuget.org/packages/Microsoft.Web.LibraryManager.Cli/) NuGet package.</span></span>

<span data-ttu-id="95890-110">Pour installer la CLI LibMan à partir d’une source de package NuGet spécifique :</span><span class="sxs-lookup"><span data-stu-id="95890-110">To install the LibMan CLI from a specific NuGet package source:</span></span>

```console
dotnet tool install -g Microsoft.Web.LibraryManager.Cli --version 1.0.94-g606058a278 --add-source C:\Temp\
```

<span data-ttu-id="95890-111">Dans l’exemple précédent, un outil Global de .NET Core est installé à partir de l’ordinateur local Windows *C:\Temp\Microsoft.Web.LibraryManager.Cli.1.0.94-g606058a278.nupkg* fichier.</span><span class="sxs-lookup"><span data-stu-id="95890-111">In the preceding example, a .NET Core Global Tool is installed from the local Windows machine's *C:\Temp\Microsoft.Web.LibraryManager.Cli.1.0.94-g606058a278.nupkg* file.</span></span>

## <a name="usage"></a><span data-ttu-id="95890-112">Utilisation</span><span class="sxs-lookup"><span data-stu-id="95890-112">Usage</span></span>

<span data-ttu-id="95890-113">Après l’installation de l’interface CLI, la commande suivante peut être utilisée :</span><span class="sxs-lookup"><span data-stu-id="95890-113">After successful installation of the CLI, the following command can be used:</span></span>

```console
libman
```

<span data-ttu-id="95890-114">Pour afficher la version CLI installée :</span><span class="sxs-lookup"><span data-stu-id="95890-114">To view the installed CLI version:</span></span>

```console
libman --version
```

<span data-ttu-id="95890-115">Pour afficher les commandes CLI disponibles :</span><span class="sxs-lookup"><span data-stu-id="95890-115">To view the available CLI commands:</span></span>

```console
libman --help
```

<span data-ttu-id="95890-116">La commande précédente affiche une sortie similaire à ce qui suit :</span><span class="sxs-lookup"><span data-stu-id="95890-116">The preceding command displays output similar to the following:</span></span>

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

<span data-ttu-id="95890-117">Les sections suivantes décrivent les commandes CLI disponibles.</span><span class="sxs-lookup"><span data-stu-id="95890-117">The following sections outline the available CLI commands.</span></span>

## <a name="initialize-libman-in-the-project"></a><span data-ttu-id="95890-118">Initialiser LibMan dans le projet</span><span class="sxs-lookup"><span data-stu-id="95890-118">Initialize LibMan in the project</span></span>

<span data-ttu-id="95890-119">Le `libman init` commande crée un *libman.json* fichier s’il n’existe.</span><span class="sxs-lookup"><span data-stu-id="95890-119">The `libman init` command creates a *libman.json* file if one doesn't exist.</span></span> <span data-ttu-id="95890-120">Le fichier est créé avec le contenu de modèle d’élément par défaut.</span><span class="sxs-lookup"><span data-stu-id="95890-120">The file is created with the default item template content.</span></span>

### <a name="synopsis"></a><span data-ttu-id="95890-121">Résumé</span><span class="sxs-lookup"><span data-stu-id="95890-121">Synopsis</span></span>

```console
libman init [-d|--default-destination] [-p|--default-provider] [--verbosity]
libman init [-h|--help]
```

### <a name="options"></a><span data-ttu-id="95890-122">Options</span><span class="sxs-lookup"><span data-stu-id="95890-122">Options</span></span>

<span data-ttu-id="95890-123">Les options suivantes sont disponibles pour le `libman init` commande :</span><span class="sxs-lookup"><span data-stu-id="95890-123">The following options are available for the `libman init` command:</span></span>

* `-d|--default-destination <PATH>`

  <span data-ttu-id="95890-124">Un chemin d’accès relatif au dossier actuel.</span><span class="sxs-lookup"><span data-stu-id="95890-124">A path relative to the current folder.</span></span> <span data-ttu-id="95890-125">Fichiers de bibliothèque sont installés à cet emplacement si aucun `destination` propriété est définie pour une bibliothèque dans *libman.json*.</span><span class="sxs-lookup"><span data-stu-id="95890-125">Library files are installed in this location if no `destination` property is defined for a library in *libman.json*.</span></span> <span data-ttu-id="95890-126">Le `<PATH>` valeur est écrite dans le `defaultDestination` propriété du *libman.json*.</span><span class="sxs-lookup"><span data-stu-id="95890-126">The `<PATH>` value is written to the `defaultDestination` property of *libman.json*.</span></span>

* `-p|--default-provider <PROVIDER>`

  <span data-ttu-id="95890-127">Le fournisseur à utiliser si aucun fournisseur n’est défini pour une bibliothèque donnée.</span><span class="sxs-lookup"><span data-stu-id="95890-127">The provider to use if no provider is defined for a given library.</span></span> <span data-ttu-id="95890-128">Le `<PROVIDER>` valeur est écrite dans le `defaultProvider` propriété du *libman.json*.</span><span class="sxs-lookup"><span data-stu-id="95890-128">The `<PROVIDER>` value is written to the `defaultProvider` property of *libman.json*.</span></span> <span data-ttu-id="95890-129">Remplacez `<PROVIDER>` avec l’une des valeurs suivantes :</span><span class="sxs-lookup"><span data-stu-id="95890-129">Replace `<PROVIDER>` with one of the following values:</span></span>

  [!INCLUDE [LibMan provider names](../../includes/libman-cli/provider-names.md)]

[!INCLUDE [standard-cli-options](../../includes/libman-cli/standard-cli-options.md)]

### <a name="examples"></a><span data-ttu-id="95890-130">Exemples</span><span class="sxs-lookup"><span data-stu-id="95890-130">Examples</span></span>

<span data-ttu-id="95890-131">Pour créer un *libman.json* fichier dans un projet ASP.NET Core :</span><span class="sxs-lookup"><span data-stu-id="95890-131">To create a *libman.json* file in an ASP.NET Core project:</span></span>

* <span data-ttu-id="95890-132">Accédez à la racine du projet.</span><span class="sxs-lookup"><span data-stu-id="95890-132">Navigate to the project root.</span></span>
* <span data-ttu-id="95890-133">Exécutez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="95890-133">Run the following command:</span></span>

  ```console
  libman init
  ```

* <span data-ttu-id="95890-134">Tapez le nom du fournisseur de valeur par défaut, ou appuyez sur `Enter` pour utiliser le fournisseur CDNJS par défaut.</span><span class="sxs-lookup"><span data-stu-id="95890-134">Type the name of the default provider, or press `Enter` to use the default CDNJS provider.</span></span> <span data-ttu-id="95890-135">Les valeurs valides sont les suivantes :</span><span class="sxs-lookup"><span data-stu-id="95890-135">Valid values include:</span></span>

  [!INCLUDE [LibMan provider names](../../includes/libman-cli/provider-names.md)]

  ![commande d’init libman - fournisseur par défaut](_static/libman-init-provider.png)

<span data-ttu-id="95890-137">Un *libman.json* fichier est ajouté à la racine du projet avec le contenu suivant :</span><span class="sxs-lookup"><span data-stu-id="95890-137">A *libman.json* file is added to the project root with the following content:</span></span>

```json
{
  "version": "1.0",
  "defaultProvider": "cdnjs",
  "libraries": []
}
```

## <a name="add-library-files"></a><span data-ttu-id="95890-138">Ajouter des fichiers de bibliothèque</span><span class="sxs-lookup"><span data-stu-id="95890-138">Add library files</span></span>

<span data-ttu-id="95890-139">Le `libman install` commande télécharge et installe les fichiers de bibliothèque dans le projet.</span><span class="sxs-lookup"><span data-stu-id="95890-139">The `libman install` command downloads and installs library files into the project.</span></span> <span data-ttu-id="95890-140">Un *libman.json* fichier est ajouté s’il n’existe.</span><span class="sxs-lookup"><span data-stu-id="95890-140">A *libman.json* file is added if one doesn't exist.</span></span> <span data-ttu-id="95890-141">Le *libman.json* fichier est modifié pour stocker les détails de configuration pour les fichiers de bibliothèque.</span><span class="sxs-lookup"><span data-stu-id="95890-141">The *libman.json* file is modified to store configuration details for the library files.</span></span>

### <a name="synopsis"></a><span data-ttu-id="95890-142">Résumé</span><span class="sxs-lookup"><span data-stu-id="95890-142">Synopsis</span></span>

```console
libman install <LIBRARY> [-d|--destination] [--files] [-p|--provider] [--verbosity]
libman install [-h|--help]
```

### <a name="arguments"></a><span data-ttu-id="95890-143">Arguments</span><span class="sxs-lookup"><span data-stu-id="95890-143">Arguments</span></span>

`LIBRARY`

<span data-ttu-id="95890-144">Le nom de la bibliothèque à installer.</span><span class="sxs-lookup"><span data-stu-id="95890-144">The name of the library to install.</span></span> <span data-ttu-id="95890-145">Ce nom peut inclure la notation de numéro de version (par exemple, `@1.2.0`).</span><span class="sxs-lookup"><span data-stu-id="95890-145">This name may include version number notation (for example, `@1.2.0`).</span></span>

### <a name="options"></a><span data-ttu-id="95890-146">Options</span><span class="sxs-lookup"><span data-stu-id="95890-146">Options</span></span>

<span data-ttu-id="95890-147">Les options suivantes sont disponibles pour le `libman install` commande :</span><span class="sxs-lookup"><span data-stu-id="95890-147">The following options are available for the `libman install` command:</span></span>

* `-d|--destination <PATH>`

  <span data-ttu-id="95890-148">L’emplacement d’installation de la bibliothèque.</span><span class="sxs-lookup"><span data-stu-id="95890-148">The location to install the library.</span></span> <span data-ttu-id="95890-149">Si non spécifié, l’emplacement par défaut est utilisé.</span><span class="sxs-lookup"><span data-stu-id="95890-149">If not specified, the default location is used.</span></span> <span data-ttu-id="95890-150">Si aucun `defaultDestination` propriété est spécifiée dans *libman.json*, cette option est requise.</span><span class="sxs-lookup"><span data-stu-id="95890-150">If no `defaultDestination` property is specified in *libman.json*, this option is required.</span></span>

* `--files <FILE>`

  <span data-ttu-id="95890-151">Spécifiez le nom du fichier à installer à partir de la bibliothèque.</span><span class="sxs-lookup"><span data-stu-id="95890-151">Specify the name of the file to install from the library.</span></span> <span data-ttu-id="95890-152">Si non spécifié, tous les fichiers à partir de la bibliothèque sont installés.</span><span class="sxs-lookup"><span data-stu-id="95890-152">If not specified, all files from the library are installed.</span></span> <span data-ttu-id="95890-153">Fournir un `--files` option par fichier doit être installé.</span><span class="sxs-lookup"><span data-stu-id="95890-153">Provide one `--files` option per file to be installed.</span></span> <span data-ttu-id="95890-154">Chemins d’accès relatifs sont trop pris en charge.</span><span class="sxs-lookup"><span data-stu-id="95890-154">Relative paths are supported too.</span></span> <span data-ttu-id="95890-155">Par exemple : `--files dist/browser/signalr.js`.</span><span class="sxs-lookup"><span data-stu-id="95890-155">For example: `--files dist/browser/signalr.js`.</span></span>

* `-p|--provider <PROVIDER>`

  <span data-ttu-id="95890-156">Le nom du fournisseur à utiliser pour l’acquisition de la bibliothèque.</span><span class="sxs-lookup"><span data-stu-id="95890-156">The name of the provider to use for the library acquisition.</span></span> <span data-ttu-id="95890-157">Remplacez `<PROVIDER>` avec l’une des valeurs suivantes :</span><span class="sxs-lookup"><span data-stu-id="95890-157">Replace `<PROVIDER>` with one of the following values:</span></span>
  
  [!INCLUDE [LibMan provider names](../../includes/libman-cli/provider-names.md)]

  <span data-ttu-id="95890-158">Si non spécifié, le `defaultProvider` propriété dans *libman.json* est utilisé.</span><span class="sxs-lookup"><span data-stu-id="95890-158">If not specified, the `defaultProvider` property in *libman.json* is used.</span></span> <span data-ttu-id="95890-159">Si aucun `defaultProvider` propriété est spécifiée dans *libman.json*, cette option est requise.</span><span class="sxs-lookup"><span data-stu-id="95890-159">If no `defaultProvider` property is specified in *libman.json*, this option is required.</span></span>

[!INCLUDE [standard-cli-options](../../includes/libman-cli/standard-cli-options.md)]

### <a name="examples"></a><span data-ttu-id="95890-160">Exemples</span><span class="sxs-lookup"><span data-stu-id="95890-160">Examples</span></span>

<span data-ttu-id="95890-161">Considérez les éléments suivants *libman.json* fichier :</span><span class="sxs-lookup"><span data-stu-id="95890-161">Consider the following *libman.json* file:</span></span>

```json
{
  "version": "1.0",
  "defaultProvider": "cdnjs",
  "libraries": []
}
```

<span data-ttu-id="95890-162">Pour installer la version de jQuery 3.2.1 *jquery.min.js* de fichiers à la *wwwroot/scripts/jquery* dossier à l’aide du fournisseur CDNJS :</span><span class="sxs-lookup"><span data-stu-id="95890-162">To install the jQuery version 3.2.1 *jquery.min.js* file to the *wwwroot/scripts/jquery* folder using the CDNJS provider:</span></span>

```console
libman install jquery@3.2.1 --provider cdnjs --destination wwwroot/scripts/jquery --files jquery.min.js
```

<span data-ttu-id="95890-163">Le *libman.json* fichier ressemble à ceci :</span><span class="sxs-lookup"><span data-stu-id="95890-163">The *libman.json* file resembles the following:</span></span>

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

<span data-ttu-id="95890-164">Pour installer le *calendar.js* et *calendar.css* fichiers à partir de *C:\\temp\\contosoCalendar\\*  à l’aide du système de fichiers fournisseur :</span><span class="sxs-lookup"><span data-stu-id="95890-164">To install the *calendar.js* and *calendar.css* files from *C:\\temp\\contosoCalendar\\* using the file system provider:</span></span>

  ```console
  libman install C:\temp\contosoCalendar\ --provider filesystem --files calendar.js --files calendar.css
  ```

<span data-ttu-id="95890-165">L’invite suivante s’affiche pour deux raisons :</span><span class="sxs-lookup"><span data-stu-id="95890-165">The following prompt appears for two reasons:</span></span>

* <span data-ttu-id="95890-166">Le *libman.json* fichier ne contient pas un `defaultDestination` propriété.</span><span class="sxs-lookup"><span data-stu-id="95890-166">The *libman.json* file doesn't contain a `defaultDestination` property.</span></span>
* <span data-ttu-id="95890-167">Le `libman install` commande ne contient pas le `-d|--destination` option.</span><span class="sxs-lookup"><span data-stu-id="95890-167">The `libman install` command doesn't contain the `-d|--destination` option.</span></span>

![libman installer command - destination](_static/libman-install-destination.png)

<span data-ttu-id="95890-169">Après avoir accepté la destination par défaut, le *libman.json* fichier ressemble à ceci :</span><span class="sxs-lookup"><span data-stu-id="95890-169">After accepting the default destination, the *libman.json* file resembles the following:</span></span>

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

## <a name="restore-library-files"></a><span data-ttu-id="95890-170">Restaurer des fichiers de bibliothèque</span><span class="sxs-lookup"><span data-stu-id="95890-170">Restore library files</span></span>

<span data-ttu-id="95890-171">Le `libman restore` commande installe les fichiers de bibliothèque définies dans *libman.json*.</span><span class="sxs-lookup"><span data-stu-id="95890-171">The `libman restore` command installs library files defined in *libman.json*.</span></span> <span data-ttu-id="95890-172">Les règles suivantes s'appliquent :</span><span class="sxs-lookup"><span data-stu-id="95890-172">The following rules apply:</span></span>

* <span data-ttu-id="95890-173">Si aucun *libman.json* fichier existe à la racine du projet, une erreur est retournée.</span><span class="sxs-lookup"><span data-stu-id="95890-173">If no *libman.json* file exists in the project root, an error is returned.</span></span>
* <span data-ttu-id="95890-174">Si une bibliothèque spécifie un fournisseur, le `defaultProvider` propriété dans *libman.json* est ignoré.</span><span class="sxs-lookup"><span data-stu-id="95890-174">If a library specifies a provider, the `defaultProvider` property in *libman.json* is ignored.</span></span>
* <span data-ttu-id="95890-175">Si une bibliothèque spécifie un type de destination, le `defaultDestination` propriété dans *libman.json* est ignoré.</span><span class="sxs-lookup"><span data-stu-id="95890-175">If a library specifies a destination, the `defaultDestination` property in *libman.json* is ignored.</span></span>

### <a name="synopsis"></a><span data-ttu-id="95890-176">Résumé</span><span class="sxs-lookup"><span data-stu-id="95890-176">Synopsis</span></span>

```console
libman restore [--verbosity]
libman restore [-h|--help]
```

### <a name="options"></a><span data-ttu-id="95890-177">Options</span><span class="sxs-lookup"><span data-stu-id="95890-177">Options</span></span>

<span data-ttu-id="95890-178">Les options suivantes sont disponibles pour le `libman restore` commande :</span><span class="sxs-lookup"><span data-stu-id="95890-178">The following options are available for the `libman restore` command:</span></span>

[!INCLUDE [standard-cli-options](../../includes/libman-cli/standard-cli-options.md)]

### <a name="examples"></a><span data-ttu-id="95890-179">Exemples</span><span class="sxs-lookup"><span data-stu-id="95890-179">Examples</span></span>

<span data-ttu-id="95890-180">Pour restaurer les fichiers de bibliothèque définies dans *libman.json*:</span><span class="sxs-lookup"><span data-stu-id="95890-180">To restore the library files defined in *libman.json*:</span></span>

```console
libman restore
```

## <a name="delete-library-files"></a><span data-ttu-id="95890-181">Supprimer les fichiers de bibliothèque</span><span class="sxs-lookup"><span data-stu-id="95890-181">Delete library files</span></span>

<span data-ttu-id="95890-182">Le `libman clean` commande supprime les fichiers de bibliothèque précédemment restaurés via LibMan.</span><span class="sxs-lookup"><span data-stu-id="95890-182">The `libman clean` command deletes library files previously restored via LibMan.</span></span> <span data-ttu-id="95890-183">Les dossiers qui devient vides après cette opération sont supprimés.</span><span class="sxs-lookup"><span data-stu-id="95890-183">Folders that become empty after this operation are deleted.</span></span> <span data-ttu-id="95890-184">Les fichiers de bibliothèque associés des configurations dans le `libraries` propriété du *libman.json* ne sont pas supprimées.</span><span class="sxs-lookup"><span data-stu-id="95890-184">The library files' associated configurations in the `libraries` property of *libman.json* aren't removed.</span></span>

### <a name="synopsis"></a><span data-ttu-id="95890-185">Résumé</span><span class="sxs-lookup"><span data-stu-id="95890-185">Synopsis</span></span>

```console
libman clean [--verbosity]
libman clean [-h|--help]
```

### <a name="options"></a><span data-ttu-id="95890-186">Options</span><span class="sxs-lookup"><span data-stu-id="95890-186">Options</span></span>

<span data-ttu-id="95890-187">Les options suivantes sont disponibles pour le `libman clean` commande :</span><span class="sxs-lookup"><span data-stu-id="95890-187">The following options are available for the `libman clean` command:</span></span>

[!INCLUDE [standard-cli-options](../../includes/libman-cli/standard-cli-options.md)]

### <a name="examples"></a><span data-ttu-id="95890-188">Exemples</span><span class="sxs-lookup"><span data-stu-id="95890-188">Examples</span></span>

<span data-ttu-id="95890-189">Pour supprimer les fichiers de bibliothèque installés via LibMan :</span><span class="sxs-lookup"><span data-stu-id="95890-189">To delete library files installed via LibMan:</span></span>

```console
libman clean
```

## <a name="uninstall-library-files"></a><span data-ttu-id="95890-190">Désinstaller les fichiers de bibliothèque</span><span class="sxs-lookup"><span data-stu-id="95890-190">Uninstall library files</span></span>

<span data-ttu-id="95890-191">Le `libman uninstall` commande :</span><span class="sxs-lookup"><span data-stu-id="95890-191">The `libman uninstall` command:</span></span>

* <span data-ttu-id="95890-192">Supprime tous les fichiers associés à la bibliothèque spécifiée à partir de la destination dans *libman.json*.</span><span class="sxs-lookup"><span data-stu-id="95890-192">Deletes all files associated with the specified library from the destination in *libman.json*.</span></span>
* <span data-ttu-id="95890-193">Supprime la configuration de la bibliothèque associée à partir de *libman.json*.</span><span class="sxs-lookup"><span data-stu-id="95890-193">Removes the associated library configuration from *libman.json*.</span></span>

<span data-ttu-id="95890-194">Une erreur se produit lorsque :</span><span class="sxs-lookup"><span data-stu-id="95890-194">An error occurs when:</span></span>

* <span data-ttu-id="95890-195">Ne *libman.json* fichier existe à la racine du projet.</span><span class="sxs-lookup"><span data-stu-id="95890-195">No *libman.json* file exists in the project root.</span></span>
* <span data-ttu-id="95890-196">La bibliothèque spécifiée n’existe pas.</span><span class="sxs-lookup"><span data-stu-id="95890-196">The specified library doesn't exist.</span></span>

<span data-ttu-id="95890-197">Si plus d’une bibliothèque portant le même nom est installée, vous êtes invité à choisir une.</span><span class="sxs-lookup"><span data-stu-id="95890-197">If more than one library with the same name is installed, you're prompted to choose one.</span></span>

### <a name="synopsis"></a><span data-ttu-id="95890-198">Résumé</span><span class="sxs-lookup"><span data-stu-id="95890-198">Synopsis</span></span>

```console
libman uninstall <LIBRARY> [--verbosity]
libman uninstall [-h|--help]
```

### <a name="arguments"></a><span data-ttu-id="95890-199">Arguments</span><span class="sxs-lookup"><span data-stu-id="95890-199">Arguments</span></span>

`LIBRARY`

<span data-ttu-id="95890-200">Le nom de la bibliothèque à désinstaller.</span><span class="sxs-lookup"><span data-stu-id="95890-200">The name of the library to uninstall.</span></span> <span data-ttu-id="95890-201">Ce nom peut inclure la notation de numéro de version (par exemple, `@1.2.0`).</span><span class="sxs-lookup"><span data-stu-id="95890-201">This name may include version number notation (for example, `@1.2.0`).</span></span>

### <a name="options"></a><span data-ttu-id="95890-202">Options</span><span class="sxs-lookup"><span data-stu-id="95890-202">Options</span></span>

<span data-ttu-id="95890-203">Les options suivantes sont disponibles pour le `libman uninstall` commande :</span><span class="sxs-lookup"><span data-stu-id="95890-203">The following options are available for the `libman uninstall` command:</span></span>

[!INCLUDE [standard-cli-options](../../includes/libman-cli/standard-cli-options.md)]

### <a name="examples"></a><span data-ttu-id="95890-204">Exemples</span><span class="sxs-lookup"><span data-stu-id="95890-204">Examples</span></span>

<span data-ttu-id="95890-205">Considérez les éléments suivants *libman.json* fichier :</span><span class="sxs-lookup"><span data-stu-id="95890-205">Consider the following *libman.json* file:</span></span>

[!code-json[](samples/LibManSample/libman.json)]

* <span data-ttu-id="95890-206">Pour désinstaller jQuery, des commandes suivantes réussissent :</span><span class="sxs-lookup"><span data-stu-id="95890-206">To uninstall jQuery, either of the following commands succeed:</span></span>

  ```console
  libman uninstall jquery
  ```

  ```console
  libman uninstall jquery@3.3.1
  ```

* <span data-ttu-id="95890-207">Pour désinstaller les fichiers Lodash installés via le `filesystem` fournisseur :</span><span class="sxs-lookup"><span data-stu-id="95890-207">To uninstall the Lodash files installed via the `filesystem` provider:</span></span>

  ```console
  libman uninstall C:\temp\lodash\
  ```

## <a name="update-library-version"></a><span data-ttu-id="95890-208">Mettre à jour la version de la bibliothèque</span><span class="sxs-lookup"><span data-stu-id="95890-208">Update library version</span></span>

<span data-ttu-id="95890-209">Le `libman update` commande met à jour une bibliothèque installée via LibMan à la version spécifiée.</span><span class="sxs-lookup"><span data-stu-id="95890-209">The `libman update` command updates a library installed via LibMan to the specified version.</span></span>

<span data-ttu-id="95890-210">Une erreur se produit lorsque :</span><span class="sxs-lookup"><span data-stu-id="95890-210">An error occurs when:</span></span>

* <span data-ttu-id="95890-211">Ne *libman.json* fichier existe à la racine du projet.</span><span class="sxs-lookup"><span data-stu-id="95890-211">No *libman.json* file exists in the project root.</span></span>
* <span data-ttu-id="95890-212">La bibliothèque spécifiée n’existe pas.</span><span class="sxs-lookup"><span data-stu-id="95890-212">The specified library doesn't exist.</span></span>

<span data-ttu-id="95890-213">Si plus d’une bibliothèque portant le même nom est installée, vous êtes invité à choisir une.</span><span class="sxs-lookup"><span data-stu-id="95890-213">If more than one library with the same name is installed, you're prompted to choose one.</span></span>

### <a name="synopsis"></a><span data-ttu-id="95890-214">Résumé</span><span class="sxs-lookup"><span data-stu-id="95890-214">Synopsis</span></span>

```console
libman update <LIBRARY> [-pre] [--to] [--verbosity]
libman update [-h|--help]
```

### <a name="arguments"></a><span data-ttu-id="95890-215">Arguments</span><span class="sxs-lookup"><span data-stu-id="95890-215">Arguments</span></span>

`LIBRARY`

<span data-ttu-id="95890-216">Le nom de la bibliothèque pour mettre à jour.</span><span class="sxs-lookup"><span data-stu-id="95890-216">The name of the library to update.</span></span>

### <a name="options"></a><span data-ttu-id="95890-217">Options</span><span class="sxs-lookup"><span data-stu-id="95890-217">Options</span></span>

<span data-ttu-id="95890-218">Les options suivantes sont disponibles pour le `libman update` commande :</span><span class="sxs-lookup"><span data-stu-id="95890-218">The following options are available for the `libman update` command:</span></span>

* `-pre`

  <span data-ttu-id="95890-219">Obtenir la dernière version préliminaire de la bibliothèque.</span><span class="sxs-lookup"><span data-stu-id="95890-219">Obtain the latest prerelease version of the library.</span></span>

* `--to <VERSION>`

  <span data-ttu-id="95890-220">Obtenir une version spécifique de la bibliothèque.</span><span class="sxs-lookup"><span data-stu-id="95890-220">Obtain a specific version of the library.</span></span>

[!INCLUDE [standard-cli-options](../../includes/libman-cli/standard-cli-options.md)]

### <a name="examples"></a><span data-ttu-id="95890-221">Exemples</span><span class="sxs-lookup"><span data-stu-id="95890-221">Examples</span></span>

* <span data-ttu-id="95890-222">Pour mettre à jour de jQuery vers la dernière version :</span><span class="sxs-lookup"><span data-stu-id="95890-222">To update jQuery to the latest version:</span></span>

  ```console
  libman update jquery
  ```

* <span data-ttu-id="95890-223">Pour mettre à jour de jQuery vers la version 3.3.1 :</span><span class="sxs-lookup"><span data-stu-id="95890-223">To update jQuery to version 3.3.1:</span></span>

  ```console
  libman update jquery --to 3.3.1
  ```

* <span data-ttu-id="95890-224">Pour mettre à jour de jQuery vers la dernière version préliminaire :</span><span class="sxs-lookup"><span data-stu-id="95890-224">To update jQuery to the latest prerelease version:</span></span>

  ```console
  libman update jquery -pre
  ```

## <a name="manage-library-cache"></a><span data-ttu-id="95890-225">Gérer le cache de la bibliothèque</span><span class="sxs-lookup"><span data-stu-id="95890-225">Manage library cache</span></span>

<span data-ttu-id="95890-226">Le `libman cache` commande gère le cache de la bibliothèque LibMan.</span><span class="sxs-lookup"><span data-stu-id="95890-226">The `libman cache` command manages the LibMan library cache.</span></span> <span data-ttu-id="95890-227">Le `filesystem` fournisseur n’utilise pas le cache de la bibliothèque.</span><span class="sxs-lookup"><span data-stu-id="95890-227">The `filesystem` provider doesn't use the library cache.</span></span>

### <a name="synopsis"></a><span data-ttu-id="95890-228">Résumé</span><span class="sxs-lookup"><span data-stu-id="95890-228">Synopsis</span></span>

```console
libman cache clean [<PROVIDER>] [--verbosity]
libman cache list [--files] [--libraries] [--verbosity]
libman cache [-h|--help]
```

### <a name="arguments"></a><span data-ttu-id="95890-229">Arguments</span><span class="sxs-lookup"><span data-stu-id="95890-229">Arguments</span></span>

`PROVIDER`

<span data-ttu-id="95890-230">Utilisé uniquement avec la `clean` commande.</span><span class="sxs-lookup"><span data-stu-id="95890-230">Only used with the `clean` command.</span></span> <span data-ttu-id="95890-231">Spécifie le cache du fournisseur à nettoyer.</span><span class="sxs-lookup"><span data-stu-id="95890-231">Specifies the provider cache to clean.</span></span> <span data-ttu-id="95890-232">Les valeurs valides sont les suivantes :</span><span class="sxs-lookup"><span data-stu-id="95890-232">Valid values include:</span></span>

[!INCLUDE [LibMan provider names](../../includes/libman-cli/provider-names.md)]

### <a name="options"></a><span data-ttu-id="95890-233">Options</span><span class="sxs-lookup"><span data-stu-id="95890-233">Options</span></span>

<span data-ttu-id="95890-234">Les options suivantes sont disponibles pour le `libman cache` commande :</span><span class="sxs-lookup"><span data-stu-id="95890-234">The following options are available for the `libman cache` command:</span></span>

* `--files`

  <span data-ttu-id="95890-235">Répertorier les noms des fichiers qui sont mis en cache.</span><span class="sxs-lookup"><span data-stu-id="95890-235">List the names of files that are cached.</span></span>

* `--libraries`

  <span data-ttu-id="95890-236">Répertorier les noms des bibliothèques qui sont mis en cache.</span><span class="sxs-lookup"><span data-stu-id="95890-236">List the names of libraries that are cached.</span></span>

[!INCLUDE [standard-cli-options](../../includes/libman-cli/standard-cli-options.md)]

### <a name="examples"></a><span data-ttu-id="95890-237">Exemples</span><span class="sxs-lookup"><span data-stu-id="95890-237">Examples</span></span>

* <span data-ttu-id="95890-238">Pour afficher les noms de bibliothèques mis en cache par le fournisseur, utilisez une des commandes suivantes :</span><span class="sxs-lookup"><span data-stu-id="95890-238">To view the names of cached libraries per provider, use one of the following commands:</span></span>

  ```console
  libman cache list
  ```

  ```console
  libman cache list --libraries
  ```

  <span data-ttu-id="95890-239">Une sortie similaire à la suivante s’affiche à l’écran :</span><span class="sxs-lookup"><span data-stu-id="95890-239">Output similar to the following is displayed:</span></span>

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

* <span data-ttu-id="95890-240">Pour afficher les noms de fichiers de bibliothèque mis en cache par le fournisseur :</span><span class="sxs-lookup"><span data-stu-id="95890-240">To view the names of cached library files per provider:</span></span>

  ```console
  libman cache list --files
  ```

  <span data-ttu-id="95890-241">Une sortie similaire à la suivante s’affiche à l’écran :</span><span class="sxs-lookup"><span data-stu-id="95890-241">Output similar to the following is displayed:</span></span>

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

  <span data-ttu-id="95890-242">Notez que la sortie précédente montre que jQuery versions 3.2.1 et 3.3.1 sont mis en cache sous le fournisseur CDNJS.</span><span class="sxs-lookup"><span data-stu-id="95890-242">Notice the preceding output shows that jQuery versions 3.2.1 and 3.3.1 are cached under the CDNJS provider.</span></span>

* <span data-ttu-id="95890-243">Pour vider le cache de la bibliothèque pour le fournisseur CDNJS :</span><span class="sxs-lookup"><span data-stu-id="95890-243">To empty the library cache for the CDNJS provider:</span></span>

  ```console
  libman cache clean cdnjs
  ```

  <span data-ttu-id="95890-244">Après avoir vidé le cache du fournisseur CDNJS, le `libman cache list` commande affiche les éléments suivants :</span><span class="sxs-lookup"><span data-stu-id="95890-244">After emptying the CDNJS provider cache, the `libman cache list` command displays the following:</span></span>

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

* <span data-ttu-id="95890-245">Pour vider le cache pour tous les fournisseurs pris en charge :</span><span class="sxs-lookup"><span data-stu-id="95890-245">To empty the cache for all supported providers:</span></span>

  ```console
  libman cache clean
  ```

  <span data-ttu-id="95890-246">Après avoir vidé tous les caches de fournisseur, le `libman cache list` commande affiche les éléments suivants :</span><span class="sxs-lookup"><span data-stu-id="95890-246">After emptying all provider caches, the `libman cache list` command displays the following:</span></span>

  ```console
  Cache contents:
  ---------------
  unpkg:
      (empty)
  cdnjs:
      (empty)
  ```

## <a name="additional-resources"></a><span data-ttu-id="95890-247">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="95890-247">Additional resources</span></span>

* [<span data-ttu-id="95890-248">Installer un outil Global</span><span class="sxs-lookup"><span data-stu-id="95890-248">Install a Global Tool</span></span>](/dotnet/core/tools/global-tools#install-a-global-tool)
* <xref:client-side/libman/libman-vs>
* [<span data-ttu-id="95890-249">Dépôt GitHub LibMan</span><span class="sxs-lookup"><span data-stu-id="95890-249">LibMan GitHub repository</span></span>](https://github.com/aspnet/LibraryManager)
