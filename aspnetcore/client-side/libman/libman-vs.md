---
title: Utiliser LibMan avec ASP.NET Core dans Visual Studio
author: scottaddie
description: Découvrez comment utiliser LibMan dans un projet ASP.NET Core avec Visual Studio.
ms.author: scaddie
ms.custom: mvc
ms.date: 08/20/2018
uid: client-side/libman/libman-vs
ms.openlocfilehash: 727bd80b7f37f6ebd9d37b7aab1aa6c33b85678c
ms.sourcegitcommit: 375e9a67f5e1f7b0faaa056b4b46294cc70f55b7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/29/2018
ms.locfileid: "50206725"
---
# <a name="use-libman-with-aspnet-core-in-visual-studio"></a><span data-ttu-id="db3a1-103">Utiliser LibMan avec ASP.NET Core dans Visual Studio</span><span class="sxs-lookup"><span data-stu-id="db3a1-103">Use LibMan with ASP.NET Core in Visual Studio</span></span>

<span data-ttu-id="db3a1-104">Par [Scott Addie](https://twitter.com/Scott_Addie)</span><span class="sxs-lookup"><span data-stu-id="db3a1-104">By [Scott Addie](https://twitter.com/Scott_Addie)</span></span>

<span data-ttu-id="db3a1-105">Visual Studio propose une prise en charge intégrée pour [LibMan](xref:client-side/libman/index) dans les projets ASP.NET Core, y compris :</span><span class="sxs-lookup"><span data-stu-id="db3a1-105">Visual Studio has built-in support for [LibMan](xref:client-side/libman/index) in ASP.NET Core projects, including:</span></span>

* <span data-ttu-id="db3a1-106">Prise en charge pour configurer et exécuter des opérations de restauration LibMan sur la build.</span><span class="sxs-lookup"><span data-stu-id="db3a1-106">Support for configuring and running LibMan restore operations on build.</span></span>
* <span data-ttu-id="db3a1-107">Éléments de menu pour déclencher des opérations de nettoyage et de restauration de LibMan.</span><span class="sxs-lookup"><span data-stu-id="db3a1-107">Menu items for triggering LibMan restore and clean operations.</span></span>
* <span data-ttu-id="db3a1-108">Boîte de dialogue de recherche pour la recherche des bibliothèques et d’ajouter les fichiers à un projet.</span><span class="sxs-lookup"><span data-stu-id="db3a1-108">Search dialog for finding libraries and adding the files to a project.</span></span>
* <span data-ttu-id="db3a1-109">Modification de la prise en charge pour *libman.json*&mdash;le fichier de manifeste LibMan.</span><span class="sxs-lookup"><span data-stu-id="db3a1-109">Editing support for *libman.json*&mdash;the LibMan manifest file.</span></span>

<span data-ttu-id="db3a1-110">[Afficher ou télécharger l’exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/client-side/libman/samples/) [(comment télécharger)](xref:index#how-to-download-a-sample)</span><span class="sxs-lookup"><span data-stu-id="db3a1-110">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/client-side/libman/samples/) [(how to download)](xref:index#how-to-download-a-sample)</span></span>

## <a name="prerequisites"></a><span data-ttu-id="db3a1-111">Prérequis</span><span class="sxs-lookup"><span data-stu-id="db3a1-111">Prerequisites</span></span>

* <span data-ttu-id="db3a1-112">Visual Studio 2017 version 15,8 ou version ultérieure avec le **ASP.NET et développement web** charge de travail</span><span class="sxs-lookup"><span data-stu-id="db3a1-112">Visual Studio 2017 version 15.8 or later with the **ASP.NET and web development** workload</span></span>

## <a name="add-library-files"></a><span data-ttu-id="db3a1-113">Ajouter des fichiers de bibliothèque</span><span class="sxs-lookup"><span data-stu-id="db3a1-113">Add library files</span></span>

<span data-ttu-id="db3a1-114">Fichiers de bibliothèque peuvent être ajoutées à un projet ASP.NET Core de deux manières différentes :</span><span class="sxs-lookup"><span data-stu-id="db3a1-114">Library files can be added to an ASP.NET Core project in two different ways:</span></span>

1. [<span data-ttu-id="db3a1-115">Utilisez la boîte de dialogue Ajouter une bibliothèque côté Client</span><span class="sxs-lookup"><span data-stu-id="db3a1-115">Use the Add Client-Side Library dialog</span></span>](#use-the-add-client-side-library-dialog)
1. [<span data-ttu-id="db3a1-116">Configurer manuellement les entrées du fichier manifeste LibMan</span><span class="sxs-lookup"><span data-stu-id="db3a1-116">Manually configure LibMan manifest file entries</span></span>](#manually-configure-libman-manifest-file-entries)

### <a name="use-the-add-client-side-library-dialog"></a><span data-ttu-id="db3a1-117">Utilisez la boîte de dialogue Ajouter une bibliothèque côté Client</span><span class="sxs-lookup"><span data-stu-id="db3a1-117">Use the Add Client-Side Library dialog</span></span>

<span data-ttu-id="db3a1-118">Suivez ces étapes pour installer une bibliothèque côté client :</span><span class="sxs-lookup"><span data-stu-id="db3a1-118">Follow these steps to install a client-side library:</span></span>

* <span data-ttu-id="db3a1-119">Dans **l’Explorateur de solutions**, cliquez sur le dossier du projet dans lequel les fichiers doivent être ajoutés.</span><span class="sxs-lookup"><span data-stu-id="db3a1-119">In **Solution Explorer**, right-click the project folder in which the files should be added.</span></span> <span data-ttu-id="db3a1-120">Choisissez **ajouter** > **bibliothèque côté Client**.</span><span class="sxs-lookup"><span data-stu-id="db3a1-120">Choose **Add** > **Client-Side Library**.</span></span> <span data-ttu-id="db3a1-121">Le **ajouter une bibliothèque côté Client** boîte de dialogue s’affiche :</span><span class="sxs-lookup"><span data-stu-id="db3a1-121">The **Add Client-Side Library** dialog appears:</span></span>

  ![Ajouter la boîte de dialogue Bibliothèque côté Client](_static/add-library-dialog.png)

* <span data-ttu-id="db3a1-123">Sélectionnez le fournisseur de bibliothèque à partir de la **fournisseur** liste déroulante.</span><span class="sxs-lookup"><span data-stu-id="db3a1-123">Select the library provider from the **Provider** drop down.</span></span> <span data-ttu-id="db3a1-124">CDNJS est le fournisseur par défaut.</span><span class="sxs-lookup"><span data-stu-id="db3a1-124">CDNJS is the default provider.</span></span>
* <span data-ttu-id="db3a1-125">Tapez le nom de la bibliothèque à récupérer dans la **bibliothèque** zone de texte.</span><span class="sxs-lookup"><span data-stu-id="db3a1-125">Type the library name to fetch in the **Library** text box.</span></span> <span data-ttu-id="db3a1-126">IntelliSense fournit une liste de bibliothèques à partir du texte fourni.</span><span class="sxs-lookup"><span data-stu-id="db3a1-126">IntelliSense provides a list of libraries beginning with the provided text.</span></span>
* <span data-ttu-id="db3a1-127">Sélectionnez la bibliothèque dans la liste IntelliSense.</span><span class="sxs-lookup"><span data-stu-id="db3a1-127">Select the library from the IntelliSense list.</span></span> <span data-ttu-id="db3a1-128">Notez que le nom de la bibliothèque est suivi du suffixe le `@` symbole et la dernière version stable connu pour le fournisseur sélectionné.</span><span class="sxs-lookup"><span data-stu-id="db3a1-128">Notice the library name is suffixed with the `@` symbol and the latest stable version known to the selected provider.</span></span>
* <span data-ttu-id="db3a1-129">Déterminer les fichiers à inclure :</span><span class="sxs-lookup"><span data-stu-id="db3a1-129">Decide which files to include:</span></span>
  * <span data-ttu-id="db3a1-130">Sélectionnez le **inclure tous les fichiers de bibliothèque** case d’option pour inclure tous les fichiers de la bibliothèque.</span><span class="sxs-lookup"><span data-stu-id="db3a1-130">Select the **Include all library files** radio button to include all of the library's files.</span></span>
  * <span data-ttu-id="db3a1-131">Sélectionnez le **choisir des fichiers spécifiques** case d’option pour inclure un sous-ensemble de fichiers de la bibliothèque.</span><span class="sxs-lookup"><span data-stu-id="db3a1-131">Select the **Choose specific files** radio button to include a subset of the library's files.</span></span> <span data-ttu-id="db3a1-132">Lorsque la case d’option est sélectionnée, l’arborescence de sélecteur de fichier est activée.</span><span class="sxs-lookup"><span data-stu-id="db3a1-132">When the radio button is selected, the file selector tree is enabled.</span></span> <span data-ttu-id="db3a1-133">Cochez les cases à gauche des noms de fichier à télécharger.</span><span class="sxs-lookup"><span data-stu-id="db3a1-133">Check the boxes to the left of the file names to download.</span></span>
* <span data-ttu-id="db3a1-134">Spécifiez le dossier de projet pour stocker les fichiers dans le **emplacement cible** zone de texte.</span><span class="sxs-lookup"><span data-stu-id="db3a1-134">Specify the project folder for storing the files in the **Target Location** text box.</span></span> <span data-ttu-id="db3a1-135">En tant que recommandation, stocker chaque bibliothèque dans un dossier distinct.</span><span class="sxs-lookup"><span data-stu-id="db3a1-135">As a recommendation, store each library in a separate folder.</span></span>

  <span data-ttu-id="db3a1-136">Le texte suggéré **emplacement cible** dossier repose sur l’emplacement à partir duquel la boîte de dialogue est lancé :</span><span class="sxs-lookup"><span data-stu-id="db3a1-136">The suggested **Target Location** folder is based on the location from which the dialog launched:</span></span>

  * <span data-ttu-id="db3a1-137">Si lancée à partir de la racine du projet :</span><span class="sxs-lookup"><span data-stu-id="db3a1-137">If launched from the project root:</span></span>
    * <span data-ttu-id="db3a1-138">*wwwroot/lib* est utilisé si *wwwroot* existe.</span><span class="sxs-lookup"><span data-stu-id="db3a1-138">*wwwroot/lib* is used if *wwwroot* exists.</span></span>
    * <span data-ttu-id="db3a1-139">*LIB* est utilisé si *wwwroot* n’existe pas.</span><span class="sxs-lookup"><span data-stu-id="db3a1-139">*lib* is used if *wwwroot* doesn't exist.</span></span>
  * <span data-ttu-id="db3a1-140">Si lancée à partir d’un dossier de projet, le nom du dossier correspondant est utilisé.</span><span class="sxs-lookup"><span data-stu-id="db3a1-140">If launched from a project folder, the corresponding folder name is used.</span></span>

  <span data-ttu-id="db3a1-141">La suggestion de dossier est suffixée avec le nom de la bibliothèque.</span><span class="sxs-lookup"><span data-stu-id="db3a1-141">The folder suggestion is suffixed with the library name.</span></span> <span data-ttu-id="db3a1-142">Le tableau suivant illustre les suggestions de dossier lors de l’installation de jQuery dans un projet de Pages Razor.</span><span class="sxs-lookup"><span data-stu-id="db3a1-142">The following table illustrates folder suggestions when installing jQuery in a Razor Pages project.</span></span>
  
  |<span data-ttu-id="db3a1-143">Emplacement de lancement</span><span class="sxs-lookup"><span data-stu-id="db3a1-143">Launch location</span></span>                           |<span data-ttu-id="db3a1-144">Dossier suggérée</span><span class="sxs-lookup"><span data-stu-id="db3a1-144">Suggested folder</span></span>      |
  |------------------------------------------|----------------------|
  |<span data-ttu-id="db3a1-145">racine du projet (si *wwwroot* existe)</span><span class="sxs-lookup"><span data-stu-id="db3a1-145">project root (if *wwwroot* exists)</span></span>        |<span data-ttu-id="db3a1-146">*wwwroot/lib/jquery /*</span><span class="sxs-lookup"><span data-stu-id="db3a1-146">*wwwroot/lib/jquery/*</span></span> |
  |<span data-ttu-id="db3a1-147">racine du projet (si *wwwroot* n’existe pas)</span><span class="sxs-lookup"><span data-stu-id="db3a1-147">project root (if *wwwroot* doesn't exist)</span></span> |<span data-ttu-id="db3a1-148">*lib/jquery /*</span><span class="sxs-lookup"><span data-stu-id="db3a1-148">*lib/jquery/*</span></span>         |
  |<span data-ttu-id="db3a1-149">*Pages* dossier de projet</span><span class="sxs-lookup"><span data-stu-id="db3a1-149">*Pages* folder in project</span></span>                 |<span data-ttu-id="db3a1-150">*Pages/jquery /*</span><span class="sxs-lookup"><span data-stu-id="db3a1-150">*Pages/jquery/*</span></span>       |

* <span data-ttu-id="db3a1-151">Cliquez sur le **installer** bouton pour télécharger les fichiers, par la configuration dans *libman.json*.</span><span class="sxs-lookup"><span data-stu-id="db3a1-151">Click the **Install** button to download the files, per the configuration in *libman.json*.</span></span>
* <span data-ttu-id="db3a1-152">Examinez le **Gestionnaire de bibliothèque** du flux de la **sortie** fenêtre pour les détails de l’installation.</span><span class="sxs-lookup"><span data-stu-id="db3a1-152">Review the **Library Manager** feed of the **Output** window for installation details.</span></span> <span data-ttu-id="db3a1-153">Exemple :</span><span class="sxs-lookup"><span data-stu-id="db3a1-153">For example:</span></span>

  ```console
  Restore operation started...
  Restoring libraries for project LibManSample
  Restoring library jquery@3.3.1... (LibManSample)
  wwwroot/lib/jquery/jquery.min.js written to destination (LibManSample)
  wwwroot/lib/jquery/jquery.js written to destination (LibManSample)
  wwwroot/lib/jquery/jquery.min.map written to destination (LibManSample)
  Restore operation completed
  1 libraries restored in 2.32 seconds
  ```

### <a name="manually-configure-libman-manifest-file-entries"></a><span data-ttu-id="db3a1-154">Configurer manuellement les entrées du fichier manifeste LibMan</span><span class="sxs-lookup"><span data-stu-id="db3a1-154">Manually configure LibMan manifest file entries</span></span>

<span data-ttu-id="db3a1-155">Toutes les opérations de LibMan dans Visual Studio sont basées sur le contenu du manifeste de la racine projet LibMan (*libman.json*).</span><span class="sxs-lookup"><span data-stu-id="db3a1-155">All LibMan operations in Visual Studio are based on the content of the project root's LibMan manifest (*libman.json*).</span></span> <span data-ttu-id="db3a1-156">Vous pouvez modifier manuellement *libman.json* pour configurer les fichiers de bibliothèque pour le projet.</span><span class="sxs-lookup"><span data-stu-id="db3a1-156">You can manually edit *libman.json* to configure library files for the project.</span></span> <span data-ttu-id="db3a1-157">Visual Studio restaure tous les fichiers de bibliothèque une fois *libman.json* est enregistré.</span><span class="sxs-lookup"><span data-stu-id="db3a1-157">Visual Studio restores all library files once *libman.json* is saved.</span></span>

<span data-ttu-id="db3a1-158">Pour ouvrir *libman.json* pour la modification, les options suivantes existent :</span><span class="sxs-lookup"><span data-stu-id="db3a1-158">To open *libman.json* for editing, the following options exist:</span></span>

* <span data-ttu-id="db3a1-159">Double-cliquez sur le *libman.json* fichier **l’Explorateur de solutions**.</span><span class="sxs-lookup"><span data-stu-id="db3a1-159">Double-click the *libman.json* file in **Solution Explorer**.</span></span>
* <span data-ttu-id="db3a1-160">Cliquez sur le projet dans **l’Explorateur de solutions** et sélectionnez **gérer les bibliothèques côté Client**.</span><span class="sxs-lookup"><span data-stu-id="db3a1-160">Right-click the project in **Solution Explorer** and select **Manage Client-Side Libraries**.</span></span> <span data-ttu-id="db3a1-161">**&#8224;**</span><span class="sxs-lookup"><span data-stu-id="db3a1-161">**&#8224;**</span></span>
* <span data-ttu-id="db3a1-162">Sélectionnez **gérer les bibliothèques côté Client** à partir de Visual Studio **projet** menu.</span><span class="sxs-lookup"><span data-stu-id="db3a1-162">Select **Manage Client-Side Libraries** from the Visual Studio **Project** menu.</span></span> <span data-ttu-id="db3a1-163">**&#8224;**</span><span class="sxs-lookup"><span data-stu-id="db3a1-163">**&#8224;**</span></span>

<span data-ttu-id="db3a1-164">**&#8224;** Si le *libman.json* fichier n’existe pas déjà dans la racine du projet, il sera créé avec le contenu de modèle d’élément par défaut.</span><span class="sxs-lookup"><span data-stu-id="db3a1-164">**&#8224;** If the *libman.json* file doesn't already exist in the project root, it will be created with the default item template content.</span></span>

<span data-ttu-id="db3a1-165">Visual Studio propose un riche JSON édition prise en charge telles que la colorisation, de mise en forme, IntelliSense et la validation de schéma.</span><span class="sxs-lookup"><span data-stu-id="db3a1-165">Visual Studio offers rich JSON editing support such as colorization, formatting, IntelliSense, and schema validation.</span></span> <span data-ttu-id="db3a1-166">Schéma JSON du manifeste de LibMan se trouve à [ http://json.schemastore.org/libman ](http://json.schemastore.org/libman).</span><span class="sxs-lookup"><span data-stu-id="db3a1-166">The LibMan manifest's JSON schema is found at [http://json.schemastore.org/libman](http://json.schemastore.org/libman).</span></span>

<span data-ttu-id="db3a1-167">Le fichier de manifeste suivantes, LibMan récupère les fichiers par la configuration définie dans le `libraries` propriété.</span><span class="sxs-lookup"><span data-stu-id="db3a1-167">With the following manifest file, LibMan retrieves files per the configuration defined in the `libraries` property.</span></span> <span data-ttu-id="db3a1-168">Une explication des littéraux d’objet défini dans `libraries` suit :</span><span class="sxs-lookup"><span data-stu-id="db3a1-168">An explanation of the object literals defined within `libraries` follows:</span></span>

* <span data-ttu-id="db3a1-169">Un sous-ensemble de [jQuery](https://jquery.com/) version 3.3.1 est récupérée à partir du fournisseur CDNJS.</span><span class="sxs-lookup"><span data-stu-id="db3a1-169">A subset of [jQuery](https://jquery.com/) version 3.3.1 is retrieved from the CDNJS provider.</span></span> <span data-ttu-id="db3a1-170">Le sous-ensemble est défini dans le `files` propriété&mdash;*jquery.min.js*, *jquery.js*, et *jquery.min.map*.</span><span class="sxs-lookup"><span data-stu-id="db3a1-170">The subset is defined in the `files` property&mdash;*jquery.min.js*, *jquery.js*, and *jquery.min.map*.</span></span> <span data-ttu-id="db3a1-171">Les fichiers sont placés dans le projet *wwwroot/lib/jquery* dossier.</span><span class="sxs-lookup"><span data-stu-id="db3a1-171">The files are placed in the project's *wwwroot/lib/jquery* folder.</span></span>
* <span data-ttu-id="db3a1-172">L’intégralité de [Bootstrap](https://getbootstrap.com/) version 4.1.3 est récupérée et placée dans un *wwwroot/lib/bootstrap* dossier.</span><span class="sxs-lookup"><span data-stu-id="db3a1-172">The entirety of [Bootstrap](https://getbootstrap.com/) version 4.1.3 is retrieved and placed in a *wwwroot/lib/bootstrap* folder.</span></span> <span data-ttu-id="db3a1-173">Le littéral d’objet `provider` substitutions de propriété le `defaultProvider` valeur de propriété.</span><span class="sxs-lookup"><span data-stu-id="db3a1-173">The object literal's `provider` property overrides the `defaultProvider` property value.</span></span> <span data-ttu-id="db3a1-174">LibMan récupère les fichiers d’amorçage à partir du fournisseur unpkg.</span><span class="sxs-lookup"><span data-stu-id="db3a1-174">LibMan retrieves the Bootstrap files from the unpkg provider.</span></span>
* <span data-ttu-id="db3a1-175">Un sous-ensemble de [Lodash](https://lodash.com/) a été approuvée par un corps de la gouvernant au sein de l’organisation.</span><span class="sxs-lookup"><span data-stu-id="db3a1-175">A subset of [Lodash](https://lodash.com/) was approved by a governing body within the organization.</span></span> <span data-ttu-id="db3a1-176">Le *lodash.js* et *lodash.min.js* fichiers sont récupérés sur le système de fichiers local à *C:\\temp\\lodash\\*.</span><span class="sxs-lookup"><span data-stu-id="db3a1-176">The *lodash.js* and *lodash.min.js* files are retrieved from the local file system at *C:\\temp\\lodash\\*.</span></span> <span data-ttu-id="db3a1-177">Les fichiers sont copiés dans le projet *wwwroot/lib/lodash* dossier.</span><span class="sxs-lookup"><span data-stu-id="db3a1-177">The files are copied to the project's *wwwroot/lib/lodash* folder.</span></span>

[!code-json[](samples/LibManSample/libman.json)]

> [!NOTE]
> <span data-ttu-id="db3a1-178">LibMan prend uniquement en charge une version de chaque bibliothèque à partir de chaque fournisseur.</span><span class="sxs-lookup"><span data-stu-id="db3a1-178">LibMan only supports one version of each library from each provider.</span></span> <span data-ttu-id="db3a1-179">Le *libman.json* fichier échoue la validation du schéma s’il contient deux bibliothèques portant le même nom de bibliothèque pour un fournisseur donné.</span><span class="sxs-lookup"><span data-stu-id="db3a1-179">The *libman.json* file fails schema validation if it contains two libraries with the same library name for a given provider.</span></span>

## <a name="restore-library-files"></a><span data-ttu-id="db3a1-180">Restaurer des fichiers de bibliothèque</span><span class="sxs-lookup"><span data-stu-id="db3a1-180">Restore library files</span></span>

<span data-ttu-id="db3a1-181">Pour restaurer les fichiers de bibliothèque à partir de Visual Studio, il doit être un *libman.json* fichier dans la racine du projet.</span><span class="sxs-lookup"><span data-stu-id="db3a1-181">To restore library files from within Visual Studio, there must be a valid *libman.json* file in the project root.</span></span> <span data-ttu-id="db3a1-182">Fichiers restaurés sont placés dans le projet à l’emplacement spécifié pour chaque bibliothèque.</span><span class="sxs-lookup"><span data-stu-id="db3a1-182">Restored files are placed in the project at the location specified for each library.</span></span>

<span data-ttu-id="db3a1-183">Fichiers de bibliothèque peuvent être restaurés dans un projet ASP.NET Core de deux manières :</span><span class="sxs-lookup"><span data-stu-id="db3a1-183">Library files can be restored in an ASP.NET Core project in two ways:</span></span>

1. [<span data-ttu-id="db3a1-184">Restaurer des fichiers pendant la génération</span><span class="sxs-lookup"><span data-stu-id="db3a1-184">Restore files during build</span></span>](#restore-files-during-build)
1. [<span data-ttu-id="db3a1-185">Restaurer des fichiers manuellement</span><span class="sxs-lookup"><span data-stu-id="db3a1-185">Restore files manually</span></span>](#restore-files-manually)

### <a name="restore-files-during-build"></a><span data-ttu-id="db3a1-186">Restaurer des fichiers pendant la génération</span><span class="sxs-lookup"><span data-stu-id="db3a1-186">Restore files during build</span></span>

<span data-ttu-id="db3a1-187">LibMan peut restaurer les fichiers de bibliothèque définies dans le cadre du processus de génération.</span><span class="sxs-lookup"><span data-stu-id="db3a1-187">LibMan can restore the defined library files as part of the build process.</span></span> <span data-ttu-id="db3a1-188">Par défaut, le *restauration sur build* comportement est désactivé.</span><span class="sxs-lookup"><span data-stu-id="db3a1-188">By default, the *restore-on-build* behavior is disabled.</span></span>

<span data-ttu-id="db3a1-189">Pour activer et tester le comportement de restauration sur build :</span><span class="sxs-lookup"><span data-stu-id="db3a1-189">To enable and test the restore-on-build behavior:</span></span>

* <span data-ttu-id="db3a1-190">Avec le bouton droit *libman.json* dans **l’Explorateur de solutions** et sélectionnez **activer des bibliothèques côté Client restaurer sur Build** dans le menu contextuel.</span><span class="sxs-lookup"><span data-stu-id="db3a1-190">Right-click *libman.json* in **Solution Explorer** and select **Enable Restore Client-Side Libraries on Build** from the context menu.</span></span>
* <span data-ttu-id="db3a1-191">Cliquez sur le **Oui** bouton lorsque vous êtes invité à installer un package NuGet.</span><span class="sxs-lookup"><span data-stu-id="db3a1-191">Click the **Yes** button when prompted to install a NuGet package.</span></span> <span data-ttu-id="db3a1-192">Le [Microsoft.Web.LibraryManager.Build](https://www.nuget.org/packages/Microsoft.Web.LibraryManager.Build/) package NuGet est ajouté au projet :</span><span class="sxs-lookup"><span data-stu-id="db3a1-192">The [Microsoft.Web.LibraryManager.Build](https://www.nuget.org/packages/Microsoft.Web.LibraryManager.Build/) NuGet package is added to the project:</span></span>

  [!code-xml[](samples/LibManSample/LibManSample.csproj?name=snippet_RestoreOnBuildPackage)]

* <span data-ttu-id="db3a1-193">Générez le projet pour confirmer la restauration des fichiers LibMan se produit.</span><span class="sxs-lookup"><span data-stu-id="db3a1-193">Build the project to confirm LibMan file restoration occurs.</span></span> <span data-ttu-id="db3a1-194">Le `Microsoft.Web.LibraryManager.Build` package injecte une cible MSBuild qui exécute LibMan pendant l’opération de génération du projet.</span><span class="sxs-lookup"><span data-stu-id="db3a1-194">The `Microsoft.Web.LibraryManager.Build` package injects an MSBuild target that runs LibMan during the project's build operation.</span></span>
* <span data-ttu-id="db3a1-195">Examinez le **Build** du flux de la **sortie** fenêtre pour un journal d’activité LibMan :</span><span class="sxs-lookup"><span data-stu-id="db3a1-195">Review the **Build** feed of the **Output** window for a LibMan activity log:</span></span>

  ```console
  1>------ Build started: Project: LibManSample, Configuration: Debug Any CPU ------
  1>
  1>Restore operation started...
  1>Restoring library jquery@3.3.1...
  1>Restoring library bootstrap@4.1.3...
  1>
  1>2 libraries restored in 10.66 seconds
  1>LibManSample -> C:\LibManSample\bin\Debug\netcoreapp2.1\LibManSample.dll
  ========== Build: 1 succeeded, 0 failed, 0 up-to-date, 0 skipped ==========
  ```

<span data-ttu-id="db3a1-196">Lorsque le comportement de restauration de build est activé, le *libman.json* menu contextuel affiche un **désactiver des bibliothèques côté Client restaurer sur Build** option.</span><span class="sxs-lookup"><span data-stu-id="db3a1-196">When the restore-on-build behavior is enabled, the *libman.json* context menu displays a **Disable Restore Client-Side Libraries on Build** option.</span></span> <span data-ttu-id="db3a1-197">Cette option supprime le `Microsoft.Web.LibraryManager.Build` référence à partir du fichier de projet de package.</span><span class="sxs-lookup"><span data-stu-id="db3a1-197">Selecting this option removes the `Microsoft.Web.LibraryManager.Build` package reference from the project file.</span></span> <span data-ttu-id="db3a1-198">Par conséquent, les bibliothèques côté client ne sont plus restaurés sur chaque build.</span><span class="sxs-lookup"><span data-stu-id="db3a1-198">Consequently, the client-side libraries are no longer restored on each build.</span></span>

<span data-ttu-id="db3a1-199">Quel que soit le paramètre de restauration sur build, vous pouvez restaurer manuellement à tout moment à partir de la *libman.json* menu contextuel.</span><span class="sxs-lookup"><span data-stu-id="db3a1-199">Regardless of the restore-on-build setting, you can manually restore at any time from the *libman.json* context menu.</span></span> <span data-ttu-id="db3a1-200">Pour plus d’informations, consultez [restaurer manuellement les fichiers](#restore-files-manually).</span><span class="sxs-lookup"><span data-stu-id="db3a1-200">For more information, see [Restore files manually](#restore-files-manually).</span></span>

### <a name="restore-files-manually"></a><span data-ttu-id="db3a1-201">Restaurer des fichiers manuellement</span><span class="sxs-lookup"><span data-stu-id="db3a1-201">Restore files manually</span></span>

<span data-ttu-id="db3a1-202">Pour restaurer manuellement les fichiers de bibliothèque :</span><span class="sxs-lookup"><span data-stu-id="db3a1-202">To manually restore library files:</span></span>

* <span data-ttu-id="db3a1-203">Pour tous les projets dans la solution :</span><span class="sxs-lookup"><span data-stu-id="db3a1-203">For all projects in the solution:</span></span>
  * <span data-ttu-id="db3a1-204">Cliquez sur le nom de la solution dans **l’Explorateur de solutions**.</span><span class="sxs-lookup"><span data-stu-id="db3a1-204">Right-click the solution name in **Solution Explorer**.</span></span>
  * <span data-ttu-id="db3a1-205">Sélectionnez le **restaurer les bibliothèques côté Client** option.</span><span class="sxs-lookup"><span data-stu-id="db3a1-205">Select the **Restore Client-Side Libraries** option.</span></span>
* <span data-ttu-id="db3a1-206">Pour un projet spécifique :</span><span class="sxs-lookup"><span data-stu-id="db3a1-206">For a specific project:</span></span>
  * <span data-ttu-id="db3a1-207">Cliquez sur le *libman.json* de fichiers dans **l’Explorateur de solutions**.</span><span class="sxs-lookup"><span data-stu-id="db3a1-207">Right-click the *libman.json* file in **Solution Explorer**.</span></span>
  * <span data-ttu-id="db3a1-208">Sélectionnez le **restaurer les bibliothèques côté Client** option.</span><span class="sxs-lookup"><span data-stu-id="db3a1-208">Select the **Restore Client-Side Libraries** option.</span></span>

<span data-ttu-id="db3a1-209">Pendant l’exécution de l’opération de restauration :</span><span class="sxs-lookup"><span data-stu-id="db3a1-209">While the restore operation is running:</span></span>

* <span data-ttu-id="db3a1-210">L’icône d’état de tâche Center (TSC) sur la barre d’état Visual Studio sera animée et lira *démarrée l’opération de restauration*.</span><span class="sxs-lookup"><span data-stu-id="db3a1-210">The Task Status Center (TSC) icon on the Visual Studio status bar will be animated and will read *Restore operation started*.</span></span> <span data-ttu-id="db3a1-211">En cliquant sur l’icône, une info-bulle indiquant les tâches en arrière-plan connus s’ouvre.</span><span class="sxs-lookup"><span data-stu-id="db3a1-211">Clicking the icon opens a tooltip listing the known background tasks.</span></span>
* <span data-ttu-id="db3a1-212">Messages seront envoyés à la barre d’état et la **Gestionnaire de bibliothèque** du flux de la **sortie** fenêtre.</span><span class="sxs-lookup"><span data-stu-id="db3a1-212">Messages will be sent to the status bar and the **Library Manager** feed of the **Output** window.</span></span> <span data-ttu-id="db3a1-213">Exemple :</span><span class="sxs-lookup"><span data-stu-id="db3a1-213">For example:</span></span>

  ```console
  Restore operation started...
  Restoring libraries for project LibManSample
  Restoring library jquery@3.3.1... (LibManSample)
  wwwroot/lib/jquery/jquery.min.js written to destination (LibManSample)
  wwwroot/lib/jquery/jquery.js written to destination (LibManSample)
  wwwroot/lib/jquery/jquery.min.map written to destination (LibManSample)
  Restore operation completed
  1 libraries restored in 2.32 seconds
  ```

## <a name="delete-library-files"></a><span data-ttu-id="db3a1-214">Supprimer les fichiers de bibliothèque</span><span class="sxs-lookup"><span data-stu-id="db3a1-214">Delete library files</span></span>

<span data-ttu-id="db3a1-215">Pour effectuer le *propre* opération, ce qui supprime les fichiers de bibliothèque précédemment restaurés dans Visual Studio :</span><span class="sxs-lookup"><span data-stu-id="db3a1-215">To perform the *clean* operation, which deletes library files previously restored in Visual Studio:</span></span>

* <span data-ttu-id="db3a1-216">Cliquez sur le *libman.json* de fichiers dans **l’Explorateur de solutions**.</span><span class="sxs-lookup"><span data-stu-id="db3a1-216">Right-click the *libman.json* file in **Solution Explorer**.</span></span>
* <span data-ttu-id="db3a1-217">Sélectionnez le **propre bibliothèques côté Client** option.</span><span class="sxs-lookup"><span data-stu-id="db3a1-217">Select the **Clean Client-Side Libraries** option.</span></span>

<span data-ttu-id="db3a1-218">Pour empêcher la suppression involontaire de fichiers non bibliothèque, l’opération de nettoyage ne supprime pas les répertoires entières.</span><span class="sxs-lookup"><span data-stu-id="db3a1-218">To prevent unintentional removal of non-library files, the clean operation doesn't delete whole directories.</span></span> <span data-ttu-id="db3a1-219">Elle supprime uniquement les fichiers qui ont été inclus dans la restauration précédente.</span><span class="sxs-lookup"><span data-stu-id="db3a1-219">It only removes files that were included in the previous restore.</span></span>

<span data-ttu-id="db3a1-220">Pendant l’exécution de l’opération de nettoyage :</span><span class="sxs-lookup"><span data-stu-id="db3a1-220">While the clean operation is running:</span></span>

* <span data-ttu-id="db3a1-221">Sur la barre d’état Visual Studio, l’icône TSC sera animée et lira *Démarrer de l’opération de bibliothèques de Client*.</span><span class="sxs-lookup"><span data-stu-id="db3a1-221">The TSC icon on the Visual Studio status bar will be animated and will read *Client libraries operation started*.</span></span> <span data-ttu-id="db3a1-222">En cliquant sur l’icône, une info-bulle indiquant les tâches en arrière-plan connus s’ouvre.</span><span class="sxs-lookup"><span data-stu-id="db3a1-222">Clicking the icon opens a tooltip listing the known background tasks.</span></span>
* <span data-ttu-id="db3a1-223">La barre d’état sont envoyés et le **Gestionnaire de bibliothèque** du flux de la **sortie** fenêtre.</span><span class="sxs-lookup"><span data-stu-id="db3a1-223">Messages are sent to the status bar and the **Library Manager** feed of the **Output** window.</span></span> <span data-ttu-id="db3a1-224">Exemple :</span><span class="sxs-lookup"><span data-stu-id="db3a1-224">For example:</span></span>

```console
Clean libraries operation started...
Clean libraries operation completed
2 libraries were successfully deleted in 1.91 secs
```

<span data-ttu-id="db3a1-225">L’opération de nettoyage supprime uniquement les fichiers du projet.</span><span class="sxs-lookup"><span data-stu-id="db3a1-225">The clean operation only deletes files from the project.</span></span> <span data-ttu-id="db3a1-226">Fichiers de bibliothèque restent dans le cache pour une récupération plus rapide sur les opérations de restauration ultérieure.</span><span class="sxs-lookup"><span data-stu-id="db3a1-226">Library files stay in the cache for faster retrieval on future restore operations.</span></span> <span data-ttu-id="db3a1-227">Pour gérer les fichiers de bibliothèque stockés dans le cache de l’ordinateur local, utilisez le [LibMan CLI](xref:client-side/libman/libman-cli).</span><span class="sxs-lookup"><span data-stu-id="db3a1-227">To manage library files stored in the local machine's cache, use the [LibMan CLI](xref:client-side/libman/libman-cli).</span></span>

## <a name="uninstall-library-files"></a><span data-ttu-id="db3a1-228">Désinstaller les fichiers de bibliothèque</span><span class="sxs-lookup"><span data-stu-id="db3a1-228">Uninstall library files</span></span>

<span data-ttu-id="db3a1-229">Pour désinstaller les fichiers de bibliothèque :</span><span class="sxs-lookup"><span data-stu-id="db3a1-229">To uninstall library files:</span></span>

* <span data-ttu-id="db3a1-230">Ouvrez *libman.json*.</span><span class="sxs-lookup"><span data-stu-id="db3a1-230">Open *libman.json*.</span></span>
* <span data-ttu-id="db3a1-231">Positionner le signe insertion à l’intérieur de le correspondantes `libraries` littéral d’objet.</span><span class="sxs-lookup"><span data-stu-id="db3a1-231">Position the caret inside the corresponding `libraries` object literal.</span></span>
* <span data-ttu-id="db3a1-232">Cliquez sur l’icône d’ampoule qui apparaît dans la marge de gauche, puis sélectionnez **désinstallation \<library_name > @\<library_version >**:</span><span class="sxs-lookup"><span data-stu-id="db3a1-232">Click the light bulb icon that appears in the left margin, and select **Uninstall \<library_name>@\<library_version>**:</span></span>

  ![Désinstaller l’option de menu contextuel de bibliothèque](_static/uninstall-menu-option.png)

<span data-ttu-id="db3a1-234">Ou bien, vous pouvez modifier manuellement et enregistrer le manifeste LibMan (*libman.json*).</span><span class="sxs-lookup"><span data-stu-id="db3a1-234">Alternatively, you can manually edit and save the LibMan manifest (*libman.json*).</span></span> <span data-ttu-id="db3a1-235">Le [l’opération de restauration](#restore-library-files) s’exécute lorsque le fichier est enregistré.</span><span class="sxs-lookup"><span data-stu-id="db3a1-235">The [restore operation](#restore-library-files) runs when the file is saved.</span></span> <span data-ttu-id="db3a1-236">Les fichiers de bibliothèque qui ne sont plus définies dans *libman.json* sont supprimés du projet.</span><span class="sxs-lookup"><span data-stu-id="db3a1-236">Library files that are no longer defined in *libman.json* are removed from the project.</span></span>

## <a name="update-library-version"></a><span data-ttu-id="db3a1-237">Mettre à jour la version de la bibliothèque</span><span class="sxs-lookup"><span data-stu-id="db3a1-237">Update library version</span></span>

<span data-ttu-id="db3a1-238">Pour vérifier une version de la bibliothèque mise à jour :</span><span class="sxs-lookup"><span data-stu-id="db3a1-238">To check for an updated library version:</span></span>

* <span data-ttu-id="db3a1-239">Ouvrez *libman.json*.</span><span class="sxs-lookup"><span data-stu-id="db3a1-239">Open *libman.json*.</span></span>
* <span data-ttu-id="db3a1-240">Positionner le signe insertion à l’intérieur de le correspondantes `libraries` littéral d’objet.</span><span class="sxs-lookup"><span data-stu-id="db3a1-240">Position the caret inside the corresponding `libraries` object literal.</span></span>
* <span data-ttu-id="db3a1-241">Cliquez sur l’icône d’ampoule qui apparaît dans la marge de gauche.</span><span class="sxs-lookup"><span data-stu-id="db3a1-241">Click the light bulb icon that appears in the left margin.</span></span> <span data-ttu-id="db3a1-242">Placez le curseur sur **vérifier les mises à jour**.</span><span class="sxs-lookup"><span data-stu-id="db3a1-242">Hover over **Check for updates**.</span></span>

<span data-ttu-id="db3a1-243">LibMan vérifie pour une version plus récente que la version installée de la bibliothèque.</span><span class="sxs-lookup"><span data-stu-id="db3a1-243">LibMan checks for a library version newer than the version installed.</span></span> <span data-ttu-id="db3a1-244">Les résultats suivants peuvent se produire :</span><span class="sxs-lookup"><span data-stu-id="db3a1-244">The following outcomes can occur:</span></span>

* <span data-ttu-id="db3a1-245">Un **aucune mise à jour trouvée** message s’affiche si la dernière version est déjà installée.</span><span class="sxs-lookup"><span data-stu-id="db3a1-245">A **No updates found** message is displayed if the latest version is already installed.</span></span>
* <span data-ttu-id="db3a1-246">La dernière version stable est affichée si ce n’est pas déjà installée.</span><span class="sxs-lookup"><span data-stu-id="db3a1-246">The latest stable version is displayed if not already installed.</span></span>

  ![Vérification de l’option de menu contextuel de mises à jour](_static/update-menu-option.png)

* <span data-ttu-id="db3a1-248">Si une version préliminaire plus récente que la version installée est disponible, la version préliminaire s’affiche.</span><span class="sxs-lookup"><span data-stu-id="db3a1-248">If a pre-release newer than the installed version is available, the pre-release is displayed.</span></span>

<span data-ttu-id="db3a1-249">Pour mettre à niveau vers une version antérieure de la bibliothèque, modifier manuellement la *libman.json* fichier.</span><span class="sxs-lookup"><span data-stu-id="db3a1-249">To downgrade to an older library version, manually edit the *libman.json* file.</span></span> <span data-ttu-id="db3a1-250">Lorsque le fichier est enregistré, le LibMan [l’opération de restauration](#restore-library-files):</span><span class="sxs-lookup"><span data-stu-id="db3a1-250">When the file is saved, the LibMan [restore operation](#restore-library-files):</span></span>

* <span data-ttu-id="db3a1-251">Supprime les fichiers redondants à partir de la version précédente.</span><span class="sxs-lookup"><span data-stu-id="db3a1-251">Removes redundant files from the previous version.</span></span>
* <span data-ttu-id="db3a1-252">Ajoute les fichiers nouveaux et mis à jour à partir de la nouvelle version.</span><span class="sxs-lookup"><span data-stu-id="db3a1-252">Adds new and updated files from the new version.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="db3a1-253">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="db3a1-253">Additional resources</span></span>

* <xref:client-side/libman/libman-cli>
* [<span data-ttu-id="db3a1-254">Dépôt GitHub LibMan</span><span class="sxs-lookup"><span data-stu-id="db3a1-254">LibMan GitHub repository</span></span>](https://github.com/aspnet/LibraryManager)
