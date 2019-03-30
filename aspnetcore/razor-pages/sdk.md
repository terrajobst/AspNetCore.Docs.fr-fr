---
title: SDK Razor ASP.NET Core
author: Rick-Anderson
description: Découvrez comment les pages Razor dans ASP.NET Core permettent de développer des scénarios orientés page de façon plus simple et plus productive qu’avec MVC.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc, seodec18
ms.date: 10/25/2018
uid: razor-pages/sdk
ms.openlocfilehash: de51c9443e639cd64c234b6975cf7252bb7a2b9a
ms.sourcegitcommit: 3e9e1f6d572947e15347e818f769e27dea56b648
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/30/2019
ms.locfileid: "58751030"
---
# <a name="aspnet-core-razor-sdk"></a>SDK Razor ASP.NET Core

Par [Rick Anderson](https://twitter.com/RickAndMSFT)

Le [!INCLUDE[](~/includes/2.1-SDK.md)] inclut le `Microsoft.NET.Sdk.Razor` MSBuild SDK (Kit de développement logiciel Razor). Le SDK Razor :

* Normalise l’expérience liée à la génération, à l’empaquetage et à la publication de projets contenant des fichiers [Razor](xref:mvc/views/razor) pour les projets ASP.NET Core basés sur MVC.
* Comprend un ensemble de cibles, de propriétés et d’éléments prédéfinis qui permettent de personnaliser la compilation des fichiers Razor.

## <a name="prerequisites"></a>Prérequis

[!INCLUDE[](~/includes/2.1-SDK.md)]

## <a name="using-the-razor-sdk"></a>Utilisation du SDK Razor

La plupart des applications web ne sont pas requis pour référencer explicitement le SDK Razor.

Pour utiliser le SDK Razor pour générer des bibliothèques de classes contenant des vues ou des pages Razor :

* Utilisez `Microsoft.NET.Sdk.Razor` à la place de `Microsoft.NET.Sdk` :

  ```xml
  <Project SDK="Microsoft.NET.Sdk.Razor">
    ...
  </Project>
  ```

* En règle générale, une référence de package à `Microsoft.AspNetCore.Mvc` est nécessaire pour recevoir des dépendances supplémentaires sont nécessaires pour générer et compiler les Pages Razor et les vues Razor. Au minimum, votre projet doit ajouter des références de package pour :

  * `Microsoft.AspNetCore.Razor.Design` 
  * `Microsoft.AspNetCore.Mvc.Razor.Extensions`
  * `Microsoft.AspNetCore.Mvc.Razor`
    
  Le `Microsoft.AspNetCore.Razor.Design` package fournit les tâches de compilation Razor et les cibles pour le projet.

  Les packages précédents sont inclus dans `Microsoft.AspNetCore.Mvc`. Le balisage suivant montre un fichier projet qui utilise le SDK de Razor pour générer les fichiers pour une application ASP.NET Core Razor Pages Razor :
    
  [!code-xml[](sdk/sample/RazorSDK.csproj)]

::: moniker range="= aspnetcore-2.1"

> [!WARNING]
> Le `Microsoft.AspNetCore.Razor.Design` et `Microsoft.AspNetCore.Mvc.Razor.Extensions` packages sont inclus dans le [Microsoft.AspNetCore.App métapackage](xref:fundamentals/metapackage-app). Toutefois, la version sans `Microsoft.AspNetCore.App` référence de package fournit un métapackage à l’application qui n’inclut pas la dernière version de `Microsoft.AspNetCore.Razor.Design`. Projets doivent faire référence à une version cohérente du `Microsoft.AspNetCore.Razor.Design` (ou `Microsoft.AspNetCore.Mvc`) afin que les derniers correctifs du moment de la génération pour Razor sont inclus. Pour plus d’informations, consultez [ce problème GitHub](https://github.com/aspnet/Razor/issues/2553).

::: moniker-end

### <a name="properties"></a>Properties

Les propriétés suivantes contrôlent le comportement du SDK Razor dans le cadre d’une build de projet :

* `RazorCompileOnBuild` &ndash; Lorsque `true`, compile et émet l’assembly Razor dans le cadre de la génération du projet. La valeur par défaut est `true`.
* `RazorCompileOnPublish` &ndash; Lorsque `true`, compile et émet l’assembly Razor dans le cadre de la publication du projet. La valeur par défaut est `true`.

Les propriétés et les éléments dans le tableau suivant sont utilisés pour configurer les entrées et sortie pour le SDK Razor.

| Éléments | Description |
| ----- | ----------- |
| `RazorGenerate` | Composants d’élément (fichiers *.cshtml*) fournis en entrée aux cibles de génération de code. |
| `RazorCompile` | Des éléments Item (*.cs* fichiers) qui sont des entrées aux cibles de compilation Razor. Utilisez ItemGroup pour spécifier d’autres fichiers à compiler dans l’assembly Razor. |
| `RazorTargetAssemblyAttribute` | Composants d’élément utilisés pour générer le code d’attributs pour l’assembly Razor. Exemple :  <br>`RazorAssemblyAttribute`<br>`Include="System.Reflection.AssemblyMetadataAttribute"`<br>`_Parameter1="BuildSource" _Parameter2="https://docs.microsoft.com/">` |
| `RazorEmbeddedResource` | Éléments ajoutés en tant que ressources incorporées à l’assembly généré de Razor. |

| Propriété | Description |
| -------- | ----------- |
| `RazorTargetName` | Nom de fichier (sans extension) de l’assembly produit par Razor. | 
| `RazorOutputPath` | Répertoire de sortie Razor. |
| `RazorCompileToolset` | Permet de déterminer l’ensemble d’outils utilisé pour générer l’assembly Razor. Les valeurs valides sont `Implicit`, `RazorSDK` et `PrecompilationTool`. |
| [EnableDefaultContentItems](https://github.com/aspnet/websdk/blob/rel-2.0.0/src/ProjectSystem/Microsoft.NET.Sdk.Web.ProjectSystem.Targets/netstandard1.0/Microsoft.NET.Sdk.Web.ProjectSystem.targets#L21) | La valeur par défaut est `true`. Lorsque `true`, inclut *web.config*, *.json*, et *.cshtml* fichiers en tant que contenu dans le projet. Lorsque référencé par le biais de `Microsoft.NET.Sdk.Web`, les fichiers sous *wwwroot* et fichiers de configuration sont également incluses. |
| `EnableDefaultRazorGenerateItems` | Si la valeur est `true`, inclut les fichiers *.cshtml* des éléments `Content` dans les éléments `RazorGenerate`. |
| `GenerateRazorTargetAssemblyInfo` | Lorsque `true`, génère un *.cs* fichier contenant les attributs spécifiés par `RazorAssemblyAttribute` et inclut le fichier dans la sortie de la compilation. |
| `EnableDefaultRazorTargetAssemblyInfoAttributes` | Si la valeur est `true`, ajoute un ensemble par défaut d’attributs d’assembly à `RazorAssemblyAttribute`. |
| `CopyRazorGenerateFilesToPublishDirectory` | Lorsque `true`, copies `RazorGenerate` éléments (*.cshtml*) fichiers au répertoire de publication. En règle générale, les fichiers Razor ne sont pas nécessaires pour une application publiée si elles participent compilation au moment de la génération ou au moment de publier. La valeur par défaut est `false`. |
| `CopyRefAssembliesToPublishDirectory` | Si la valeur est `true`, copie les éléments d’assembly de référence dans le répertoire de publication. En règle générale, les assemblys de référence ne sont pas nécessaires pour une application publiée si compilation Razor se produit au moment de la génération ou au moment de publier. La valeur `true` si votre application publiée nécessite une compilation de runtime. Par exemple, la valeur est la valeur `true` si l’application modifie *.cshtml* lors de l’exécution de fichiers ou utilise des vues incorporés. La valeur par défaut est `false`. |
| `IncludeRazorContentInPack` | Lorsque `true`, tous les éléments de contenu Razor (*.cshtml* fichiers) sont marqués pour inclusion dans le package NuGet généré. La valeur par défaut est `false`. |
| `EmbedRazorGenerateSources` | Si la valeur est `true`, ajoute des éléments RazorGenerate (*.cshtml*) comme fichiers incorporés à l’assembly Razor généré. La valeur par défaut est `false`. |
| `UseRazorBuildServer` | Si la valeur est `true`, utilise un processus de serveur de build persistant pour décharger le travail de génération de code. Utilise par défaut la valeur de `UseSharedCompilation`. |

Pour plus d’informations sur les propriétés, voir [Propriétés MSBuild](/visualstudio/msbuild/msbuild-properties).

### <a name="targets"></a>Cibles

Le SDK Razor définit deux cibles principales :

* `RazorGenerate` &ndash; Génère du code *.cs* fichiers à partir de `RazorGenerate` des éléments item. Utilisez la propriété `RazorGenerateDependsOn` pour spécifier des cibles supplémentaires pouvant s’exécuter avant ou après cette cible.
* `RazorCompile` &ndash; Compile généré *.cs* dans des fichiers à un assembly de Razor. Utilisez `RazorCompileDependsOn` pour spécifier des cibles supplémentaires qui peuvent s’exécuter avant ou après cette cible.

### <a name="runtime-compilation-of-razor-views"></a>Compilation au moment du runtime des vues Razor

* Par défaut, le SDK Razor ne publie pas les assemblys de référence nécessaires à l’exécution de la compilation au moment du runtime. Cela entraîne des échecs de compilation quand le modèle d’application s’appuie sur la compilation au moment du runtime&mdash;par exemple, l’application utilise des vues incorporées ou change les vues une fois l’application publiée. Définissez `CopyRefAssembliesToPublishDirectory` sur `true` pour continuer à publier les assemblys de référence.

* Pour une application web, vérifiez que votre application cible le `Microsoft.NET.Sdk.Web` SDK.
