---
title: Utiliser LibMan avec ASP.NET Core dans Visual Studio
author: scottaddie
description: Découvrez comment utiliser LibMan dans un projet ASP.NET Core avec Visual Studio.
ms.author: scaddie
ms.custom: mvc
ms.date: 08/20/2018
uid: client-side/libman/libman-vs
ms.openlocfilehash: d0494b96447ad2b256acbde7748209ff430ae7c1
ms.sourcegitcommit: 6afe57fb8d9055f88fedb92b16470398c4b9b24a
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/14/2019
ms.locfileid: "65610186"
---
# <a name="use-libman-with-aspnet-core-in-visual-studio"></a>Utiliser LibMan avec ASP.NET Core dans Visual Studio

Par [Scott Addie](https://twitter.com/Scott_Addie)

Visual Studio propose une prise en charge intégrée pour [LibMan](xref:client-side/libman/index) dans les projets ASP.NET Core, y compris :

* Prise en charge pour configurer et exécuter des opérations de restauration LibMan sur la build.
* Éléments de menu pour déclencher des opérations de nettoyage et de restauration de LibMan.
* Boîte de dialogue de recherche pour la recherche des bibliothèques et d’ajouter les fichiers à un projet.
* Modification de la prise en charge pour *libman.json*&mdash;le fichier de manifeste LibMan.

[Afficher ou télécharger l’exemple de code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/client-side/libman/samples/) [(comment télécharger)](xref:index#how-to-download-a-sample)

## <a name="prerequisites"></a>Prérequis

* [Visual Studio 2019](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019) avec la **ASP.NET et développement web** charge de travail

## <a name="add-library-files"></a>Ajouter des fichiers de bibliothèque

Fichiers de bibliothèque peuvent être ajoutées à un projet ASP.NET Core de deux manières différentes :

1. [Utilisez la boîte de dialogue Ajouter une bibliothèque côté Client](#use-the-add-client-side-library-dialog)
1. [Configurer manuellement les entrées du fichier manifeste LibMan](#manually-configure-libman-manifest-file-entries)

### <a name="use-the-add-client-side-library-dialog"></a>Utilisez la boîte de dialogue Ajouter une bibliothèque côté Client

Suivez ces étapes pour installer une bibliothèque côté client :

* Dans **l’Explorateur de solutions**, cliquez sur le dossier du projet dans lequel les fichiers doivent être ajoutés. Choisissez **ajouter** > **bibliothèque côté Client**. Le **ajouter une bibliothèque côté Client** boîte de dialogue s’affiche :

  ![Ajouter la boîte de dialogue Bibliothèque côté Client](_static/add-library-dialog.png)

* Sélectionnez le fournisseur de bibliothèque à partir de la **fournisseur** liste déroulante. CDNJS est le fournisseur par défaut.
* Tapez le nom de la bibliothèque à récupérer dans la **bibliothèque** zone de texte. IntelliSense fournit une liste de bibliothèques à partir du texte fourni.
* Sélectionnez la bibliothèque dans la liste IntelliSense. Notez que le nom de la bibliothèque est suivi du suffixe le `@` symbole et la dernière version stable connu pour le fournisseur sélectionné.
* Déterminer les fichiers à inclure :
  * Sélectionnez le **inclure tous les fichiers de bibliothèque** case d’option pour inclure tous les fichiers de la bibliothèque.
  * Sélectionnez le **choisir des fichiers spécifiques** case d’option pour inclure un sous-ensemble de fichiers de la bibliothèque. Lorsque la case d’option est sélectionnée, l’arborescence de sélecteur de fichier est activée. Cochez les cases à gauche des noms de fichier à télécharger.
* Spécifiez le dossier de projet pour stocker les fichiers dans le **emplacement cible** zone de texte. En tant que recommandation, stocker chaque bibliothèque dans un dossier distinct.

  Le texte suggéré **emplacement cible** dossier repose sur l’emplacement à partir duquel la boîte de dialogue est lancé :

  * Si lancée à partir de la racine du projet :
    * *wwwroot/lib* est utilisé si *wwwroot* existe.
    * *LIB* est utilisé si *wwwroot* n’existe pas.
  * Si lancée à partir d’un dossier de projet, le nom du dossier correspondant est utilisé.

  La suggestion de dossier est suffixée avec le nom de la bibliothèque. Le tableau suivant illustre les suggestions de dossier lors de l’installation de jQuery dans un projet de Pages Razor.
  
  |Emplacement de lancement                           |Dossier suggérée      |
  |------------------------------------------|----------------------|
  |racine du projet (si *wwwroot* existe)        |*wwwroot/lib/jquery/* |
  |racine du projet (si *wwwroot* n’existe pas) |*lib/jquery/*         |
  |*Pages* dossier de projet                 |*Pages/jquery /*       |

* Cliquez sur le **installer** bouton pour télécharger les fichiers, par la configuration dans *libman.json*.
* Examinez le **Gestionnaire de bibliothèque** du flux de la **sortie** fenêtre pour les détails de l’installation. Exemple :

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

### <a name="manually-configure-libman-manifest-file-entries"></a>Configurer manuellement les entrées du fichier manifeste LibMan

Toutes les opérations de LibMan dans Visual Studio sont basées sur le contenu du manifeste de la racine projet LibMan (*libman.json*). Vous pouvez modifier manuellement *libman.json* pour configurer les fichiers de bibliothèque pour le projet. Visual Studio restaure tous les fichiers de bibliothèque une fois *libman.json* est enregistré.

Pour ouvrir *libman.json* pour la modification, les options suivantes existent :

* Double-cliquez sur le *libman.json* fichier **l’Explorateur de solutions**.
* Cliquez sur le projet dans **l’Explorateur de solutions** et sélectionnez **gérer les bibliothèques côté Client**. **&#8224;**
* Sélectionnez **gérer les bibliothèques côté Client** à partir de Visual Studio **projet** menu. **&#8224;**

**&#8224;** Si le *libman.json* fichier n’existe pas déjà dans la racine du projet, il sera créé avec le contenu de modèle d’élément par défaut.

Visual Studio propose un riche JSON édition prise en charge telles que la colorisation, de mise en forme, IntelliSense et la validation de schéma. Schéma JSON du manifeste de LibMan se trouve à [ http://json.schemastore.org/libman ](http://json.schemastore.org/libman).

Le fichier de manifeste suivantes, LibMan récupère les fichiers par la configuration définie dans le `libraries` propriété. Une explication des littéraux d’objet défini dans `libraries` suit :

* Un sous-ensemble de [jQuery](https://jquery.com/) version 3.3.1 est récupérée à partir du fournisseur CDNJS. Le sous-ensemble est défini dans le `files` propriété&mdash;*jquery.min.js*, *jquery.js*, et *jquery.min.map*. Les fichiers sont placés dans le projet *wwwroot/lib/jquery* dossier.
* L’intégralité de [Bootstrap](https://getbootstrap.com/) version 4.1.3 est récupérée et placée dans un *wwwroot/lib/bootstrap* dossier. Le littéral d’objet `provider` substitutions de propriété le `defaultProvider` valeur de propriété. LibMan récupère les fichiers d’amorçage à partir du fournisseur unpkg.
* Un sous-ensemble de [Lodash](https://lodash.com/) a été approuvée par un corps de la gouvernant au sein de l’organisation. Le *lodash.js* et *lodash.min.js* fichiers sont récupérés sur le système de fichiers local à *C:\\temp\\lodash\\*. Les fichiers sont copiés dans le projet *wwwroot/lib/lodash* dossier.

[!code-json[](samples/LibManSample/libman.json)]

> [!NOTE]
> LibMan prend uniquement en charge une version de chaque bibliothèque à partir de chaque fournisseur. Le *libman.json* fichier échoue la validation du schéma s’il contient deux bibliothèques portant le même nom de bibliothèque pour un fournisseur donné.

## <a name="restore-library-files"></a>Restaurer des fichiers de bibliothèque

Pour restaurer les fichiers de bibliothèque à partir de Visual Studio, il doit être un *libman.json* fichier dans la racine du projet. Fichiers restaurés sont placés dans le projet à l’emplacement spécifié pour chaque bibliothèque.

Fichiers de bibliothèque peuvent être restaurés dans un projet ASP.NET Core de deux manières :

1. [Restaurer des fichiers pendant la génération](#restore-files-during-build)
1. [Restaurer des fichiers manuellement](#restore-files-manually)

### <a name="restore-files-during-build"></a>Restaurer des fichiers pendant la génération

LibMan peut restaurer les fichiers de bibliothèque définies dans le cadre du processus de génération. Par défaut, le *restauration sur build* comportement est désactivé.

Pour activer et tester le comportement de restauration sur build :

* Avec le bouton droit *libman.json* dans **l’Explorateur de solutions** et sélectionnez **activer des bibliothèques côté Client restaurer sur Build** dans le menu contextuel.
* Cliquez sur le **Oui** bouton lorsque vous êtes invité à installer un package NuGet. Le [Microsoft.Web.LibraryManager.Build](https://www.nuget.org/packages/Microsoft.Web.LibraryManager.Build/) package NuGet est ajouté au projet :

  [!code-xml[](samples/LibManSample/LibManSample.csproj?name=snippet_RestoreOnBuildPackage)]

* Générez le projet pour confirmer la restauration des fichiers LibMan se produit. Le `Microsoft.Web.LibraryManager.Build` package injecte une cible MSBuild qui exécute LibMan pendant l’opération de génération du projet.
* Examinez le **Build** du flux de la **sortie** fenêtre pour un journal d’activité LibMan :

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

Lorsque le comportement de restauration de build est activé, le *libman.json* menu contextuel affiche un **désactiver des bibliothèques côté Client restaurer sur Build** option. Cette option supprime le `Microsoft.Web.LibraryManager.Build` référence à partir du fichier de projet de package. Par conséquent, les bibliothèques côté client ne sont plus restaurés sur chaque build.

Quel que soit le paramètre de restauration sur build, vous pouvez restaurer manuellement à tout moment à partir de la *libman.json* menu contextuel. Pour plus d’informations, consultez [restaurer manuellement les fichiers](#restore-files-manually).

### <a name="restore-files-manually"></a>Restaurer des fichiers manuellement

Pour restaurer manuellement les fichiers de bibliothèque :

* Pour tous les projets dans la solution :
  * Cliquez sur le nom de la solution dans **l’Explorateur de solutions**.
  * Sélectionnez le **restaurer les bibliothèques côté Client** option.
* Pour un projet spécifique :
  * Cliquez sur le *libman.json* de fichiers dans **l’Explorateur de solutions**.
  * Sélectionnez le **restaurer les bibliothèques côté Client** option.

Pendant l’exécution de l’opération de restauration :

* L’icône d’état de tâche Center (TSC) sur la barre d’état Visual Studio sera animée et lira *démarrée l’opération de restauration*. En cliquant sur l’icône, une info-bulle indiquant les tâches en arrière-plan connus s’ouvre.
* Messages seront envoyés à la barre d’état et la **Gestionnaire de bibliothèque** du flux de la **sortie** fenêtre. Exemple :

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

## <a name="delete-library-files"></a>Supprimer les fichiers de bibliothèque

Pour effectuer le *propre* opération, ce qui supprime les fichiers de bibliothèque précédemment restaurés dans Visual Studio :

* Cliquez sur le *libman.json* de fichiers dans **l’Explorateur de solutions**.
* Sélectionnez le **propre bibliothèques côté Client** option.

Pour empêcher la suppression involontaire de fichiers non bibliothèque, l’opération de nettoyage ne supprime pas les répertoires entières. Elle supprime uniquement les fichiers qui ont été inclus dans la restauration précédente.

Pendant l’exécution de l’opération de nettoyage :

* Sur la barre d’état Visual Studio, l’icône TSC sera animée et lira *Démarrer de l’opération de bibliothèques de Client*. En cliquant sur l’icône, une info-bulle indiquant les tâches en arrière-plan connus s’ouvre.
* La barre d’état sont envoyés et le **Gestionnaire de bibliothèque** du flux de la **sortie** fenêtre. Exemple :

```console
Clean libraries operation started...
Clean libraries operation completed
2 libraries were successfully deleted in 1.91 secs
```

L’opération de nettoyage supprime uniquement les fichiers du projet. Fichiers de bibliothèque restent dans le cache pour une récupération plus rapide sur les opérations de restauration ultérieure. Pour gérer les fichiers de bibliothèque stockés dans le cache de l’ordinateur local, utilisez le [LibMan CLI](xref:client-side/libman/libman-cli).

## <a name="uninstall-library-files"></a>Désinstaller les fichiers de bibliothèque

Pour désinstaller les fichiers de bibliothèque :

* Ouvrez *libman.json*.
* Positionner le signe insertion à l’intérieur de le correspondantes `libraries` littéral d’objet.
* Cliquez sur l’icône d’ampoule qui apparaît dans la marge de gauche, puis sélectionnez **désinstallation \<library_name > @\<library_version >**:

  ![Désinstaller l’option de menu contextuel de bibliothèque](_static/uninstall-menu-option.png)

Ou bien, vous pouvez modifier manuellement et enregistrer le manifeste LibMan (*libman.json*). Le [l’opération de restauration](#restore-library-files) s’exécute lorsque le fichier est enregistré. Les fichiers de bibliothèque qui ne sont plus définies dans *libman.json* sont supprimés du projet.

## <a name="update-library-version"></a>Mettre à jour la version de la bibliothèque

Pour vérifier une version de la bibliothèque mise à jour :

* Ouvrez *libman.json*.
* Positionner le signe insertion à l’intérieur de le correspondantes `libraries` littéral d’objet.
* Cliquez sur l’icône d’ampoule qui apparaît dans la marge de gauche. Placez le curseur sur **vérifier les mises à jour**.

LibMan vérifie pour une version plus récente que la version installée de la bibliothèque. Les résultats suivants peuvent se produire :

* Un **aucune mise à jour trouvée** message s’affiche si la dernière version est déjà installée.
* La dernière version stable est affichée si ce n’est pas déjà installée.

  ![Vérification de l’option de menu contextuel de mises à jour](_static/update-menu-option.png)

* Si une version préliminaire plus récente que la version installée est disponible, la version préliminaire s’affiche.

Pour mettre à niveau vers une version antérieure de la bibliothèque, modifier manuellement la *libman.json* fichier. Lorsque le fichier est enregistré, le LibMan [l’opération de restauration](#restore-library-files):

* Supprime les fichiers redondants à partir de la version précédente.
* Ajoute les fichiers nouveaux et mis à jour à partir de la nouvelle version.

## <a name="additional-resources"></a>Ressources supplémentaires

* <xref:client-side/libman/libman-cli>
* [Dépôt GitHub LibMan](https://github.com/aspnet/LibraryManager)
