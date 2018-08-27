---
title: SDK Razor ASP.NET Core
author: Rick-Anderson
description: Découvrez comment les pages Razor dans ASP.NET Core permettent de développer des scénarios orientés page de façon plus simple et plus productive qu’avec MVC.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 04/12/2018
uid: razor-pages/sdk
ms.openlocfilehash: 4dd48b13272ed847ff83e8826e10678d732b53f9
ms.sourcegitcommit: b8a2f14bf8dd346d7592977642b610bbcb0b0757
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/11/2018
ms.locfileid: "42909792"
---
# <a name="aspnet-core-razor-sdk"></a>SDK Razor ASP.NET Core

Par [Rick Anderson](https://twitter.com/RickAndMSFT)

[!INCLUDE [](~/includes/2.1-SDK.md)] inclut le SDK MSBuild `Microsoft.NET.Sdk.Razor` (SDK Razor). Le SDK Razor :

* Normalise l’expérience liée à la génération, à l’empaquetage et à la publication de projets contenant des fichiers [Razor](xref:mvc/views/razor) pour les projets ASP.NET Core basés sur MVC.
* Comprend un ensemble de cibles, de propriétés et d’éléments prédéfinis qui permettent de personnaliser la compilation des fichiers Razor.

## <a name="prerequisites"></a>Prérequis

[!INCLUDE[](~/includes/2.1-SDK.md)]

## <a name="using-the-razor-sdk"></a>Utilisation du SDK Razor

La plupart des applications web n’ont pas besoin de référencer expressément le SDK Razor. 

Pour utiliser le SDK Razor pour générer des bibliothèques de classes contenant des vues ou des pages Razor :

* Utilisez `Microsoft.NET.Sdk.Razor` à la place de `Microsoft.NET.Sdk` :
```xml
<Project SDK="Microsoft.NET.Sdk.Razor">
  ...
</Project>
```

* En général, une référence de package à `Microsoft.AspNetCore.Mvc` est nécessaire pour introduire les dépendances supplémentaires permettant de générer et de compiler les pages et les vues Razor. Au minimum, votre projet doit ajouter des références de package à :

    * `Microsoft.AspNetCore.Razor.Design` 
    * `Microsoft.AspNetCore.Mvc.Razor.Extensions`
    
 Les packages précédents sont inclus dans `Microsoft.AspNetCore.Mvc`. Le balisage suivant présente un fichier *.csproj* de base qui utilise le SDK Razor pour générer des fichiers Razor pour une application Razor Pages ASP.NET Core :
    
 [!code-xml[Main](sdk/sample/RazorSDK.csproj)]

### <a name="properties"></a>Properties

Les propriétés suivantes contrôlent le comportement du SDK Razor dans le cadre d’une build de projet :

* `RazorCompileOnBuild` : si la valeur est `true`, compile et émet l’assembly Razor dans le cadre de la génération du projet. La valeur par défaut est `true`.
* `RazorCompileOnPublish` : si la valeur est `true`, compile et émet l’assembly Razor dans le cadre de la publication du projet. La valeur par défaut est `true`.

Les propriétés et éléments suivants sont utilisés pour configurer les entrées et la sortie du SDK Razor :

| Éléments                                         | Description                                                                   |
| ------------                                  | -------------                                                                 |
| RazorGenerate                                 | Composants d’élément (fichiers *.cshtml*) fournis en entrée aux cibles de génération de code. |
| RazorCompile                                  | Composants d’élément (fichiers .cs) fournis en entrée aux cibles de compilation Razor. Utilisez ItemGroup pour spécifier d’autres fichiers à compiler dans l’assembly Razor. |
| RazorTargetAssemblyAttribute                  | Composants d’élément utilisés pour générer le code d’attributs pour l’assembly Razor. Exemple :  <br />`<RazorAssemblyAttribute ` <br />  `Include="System.Reflection.AssemblyMetadataAttribute"`<br />`  _Parameter1="BuildSource" _Parameter2="https://docs.asp.net/">` |
| RazorEmbeddedResource                         | Composants d’élément ajoutés comme ressources incorporées à l’assembly Razor généré. |

| Propriété                                      | Description                                                                   |
| ------------                                  | -------------                                                                 |
| RazorTargetName                               | Nom de fichier (sans extension) de l’assembly produit par Razor. | 
| RazorOutputPath                               | Répertoire de sortie Razor.                                      |
| RazorCompileToolset                           | Permet de déterminer l’ensemble d’outils utilisé pour générer l’assembly Razor. Les valeurs valides sont `Implicit` et `PrecompilationTool`. |
| EnableDefaultContentItems                     | Si la valeur est `true`, inclut certains types de fichiers, notamment des fichiers *.cshtml*, comme contenu dans le projet. En cas de référencement par le biais de Microsoft.NET.Sdk.Web, inclut également tous les fichiers sous *wwwroot* et les fichiers de configuration.         |
| EnableDefaultRazorGenerateItems               | Si la valeur est `true`, inclut les fichiers *.cshtml* des éléments `Content` dans les éléments `RazorGenerate`. |
| GenerateRazorTargetAssemblyInfo               | Si la valeur est `true`, génère un fichier *.cs* contenant les attributs spécifiés par `RazorAssemblyAttribute` et l’inclut dans la sortie de la compilation. |
| EnableDefaultRazorTargetAssemblyInfoAttributes | Si la valeur est `true`, ajoute un ensemble par défaut d’attributs d’assembly à `RazorAssemblyAttribute`. |
| CopyRazorGenerateFilesToPublishDirectory       | Si la valeur est `true`, copie les éléments RazorGenerate (fichiers *.cshtml*) dans le répertoire de publication. En général, une application publiée ne nécessite pas de fichiers Razor s’ils participent à la compilation au moment de la génération ou de la publication. La valeur par défaut est `false`. |
| CopyRefAssembliesToPublishDirectory            | Si la valeur est `true`, copie les éléments d’assembly de référence dans le répertoire de publication. En général, une application publiée ne nécessite pas d’assemblys de référence si la compilation Razor se produit au moment de la génération ou de la publication. Affectez `true` si votre application publiée nécessite une compilation au moment du runtime, par exemple si elle modifie des fichiers cshtml au moment du runtime, ou utilise des vues incorporées. La valeur par défaut est `false`. |
| IncludeRazorContentInPack                      | Si la valeur est `true`, tous les éléments de contenu Razor (fichiers *.cshtml*) sont marqués pour inclusion dans le package NuGet généré. La valeur par défaut est `false`. |
| EmbedRazorGenerateSources | Si la valeur est `true`, ajoute des éléments RazorGenerate (*.cshtml*) comme fichiers incorporés à l’assembly Razor généré. La valeur par défaut est `false`. |
| UseRazorBuildServer                           | Si la valeur est `true`, utilise un processus de serveur de build persistant pour décharger le travail de génération de code. Utilise par défaut la valeur de `UseSharedCompilation`. |

### <a name="targets"></a>Cibles
Le SDK Razor définit deux cibles principales :

* `RazorGenerate` : le code génère des fichiers *.cs* à partir des composants de l’élément RazorGenerate. Utilisez la propriété `RazorGenerateDependsOn` pour spécifier des cibles supplémentaires pouvant s’exécuter avant ou après cette cible.
* `RazorCompile` : compile les fichiers *.cs* générés dans un assembly Razor. Utilisez `RazorCompileDependsOn` pour spécifier des cibles supplémentaires qui peuvent s’exécuter avant ou après cette cible.

### <a name="runtime-compilation-of-razor-views"></a>Compilation au moment du runtime des vues Razor

* Par défaut, le SDK Razor ne publie pas les assemblys de référence nécessaires à l’exécution de la compilation au moment du runtime. Cela entraîne des échecs de compilation quand le modèle d’application s’appuie sur la compilation au moment du runtime&mdash;par exemple, l’application utilise des vues incorporées ou change les vues une fois l’application publiée. Définissez `CopyRefAssembliesToPublishDirectory` sur `true` pour continuer à publier les assemblys de référence.

* Pour les applications web, vérifiez que votre application cible le SDK `Microsoft.NET.Sdk.Web`.
