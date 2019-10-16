---
title: SDK Razor ASP.NET Core
author: Rick-Anderson
description: Découvrez comment les pages Razor dans ASP.NET Core permettent de développer des scénarios orientés page de façon plus simple et plus productive qu’avec MVC.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc, seodec18
ms.date: 08/23/2019
uid: razor-pages/sdk
ms.openlocfilehash: 606d2bdca3fa4fb1c81df73ac697d2175c3ab633
ms.sourcegitcommit: 07d98ada57f2a5f6d809d44bdad7a15013109549
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/15/2019
ms.locfileid: "72334035"
---
# <a name="aspnet-core-razor-sdk"></a>SDK Razor ASP.NET Core

De [Rick Anderson](https://twitter.com/RickAndMSFT)

## <a name="overview"></a>Vue d'ensemble

Le [!INCLUDE[](~/includes/2.1-SDK.md)] comprend le kit de développement logiciel (SDK) MSBuild `Microsoft.NET.Sdk.Razor` (kit de développement logiciel (SDK). Le SDK Razor :

::: moniker range=">= aspnetcore-3.0"

* Est requis pour générer, empaqueter et publier des projets contenant des fichiers [Razor](xref:mvc/views/razor) pour ASP.net Core projets basés sur MVC ou [éblouissants](xref:blazor/index) .
* Comprend un ensemble de cibles, de propriétés et d’éléments prédéfinis qui permettent de personnaliser la compilation des fichiers Razor ( *. cshtml* ou *. Razor*).

Le kit de développement logiciel (SDK) Razor comprend des éléments `Content` avec des attributs `Include` définis sur les modèles de globbing `**\*.cshtml` et `**\*.razor`. Les fichiers correspondants sont publiés.

::: moniker-end

::: moniker range="< aspnetcore-3.0"

* Normalise l’expérience liée à la génération, à l’empaquetage et à la publication de projets contenant des fichiers [Razor](xref:mvc/views/razor) pour les projets ASP.NET Core basés sur MVC.
* Comprend un ensemble de cibles, de propriétés et d’éléments prédéfinis qui permettent de personnaliser la compilation des fichiers Razor.

Le kit de développement logiciel (SDK) Razor comprend un élément `Content` avec un attribut `Include` défini sur le modèle de globbing `**\*.cshtml`. Les fichiers correspondants sont publiés.

::: moniker-end

## <a name="prerequisites"></a>Configuration requise

[!INCLUDE[](~/includes/2.1-SDK.md)]

## <a name="use-the-razor-sdk"></a>Utiliser le kit de développement logiciel (SDK) Razor

La plupart des applications Web ne sont pas requises pour référencer explicitement le kit de développement logiciel (SDK) Razor.

::: moniker range=">= aspnetcore-3.0"

Pour utiliser le kit de développement logiciel (SDK) Razor afin de générer des bibliothèques de classes contenant des vues Razor ou des Razor Pages, nous vous recommandons de commencer par le modèle de projet Bibliothèque de classes Razor (RCL). Un RCL utilisé pour générer au minimum des fichiers éblouissants ( *. Razor*) requiert une référence au package [Microsoft. AspNetCore. Components](https://www.nuget.org/packages/Microsoft.AspNetCore.Components) . Un RCL qui est utilisé pour créer des vues ou des pages Razor (fichiers *. cshtml* ) nécessite au minimum le ciblage de `netcoreapp3.0` ou version ultérieure et a un `FrameworkReference` vers le [Microsoft. AspNetCore. app](xref:fundamentals/metapackage-app) dans son fichier projet.

::: moniker-end

::: moniker range="< aspnetcore-3.0"

Pour utiliser le SDK Razor pour générer des bibliothèques de classes contenant des vues ou des pages Razor :

* Utilisez `Microsoft.NET.Sdk.Razor` à la place de `Microsoft.NET.Sdk` :

  ```xml
  <Project SDK="Microsoft.NET.Sdk.Razor">
    <!-- omitted for brevity -->
  </Project>
  ```

* En règle générale, une référence de package à `Microsoft.AspNetCore.Mvc` est requise pour recevoir des dépendances supplémentaires requises pour générer et compiler des vues Razor Pages et Razor. Au minimum, votre projet doit ajouter des références de package à :

  * `Microsoft.AspNetCore.Razor.Design` 
  * `Microsoft.AspNetCore.Mvc.Razor.Extensions`
  * `Microsoft.AspNetCore.Mvc.Razor`
    
  Le package `Microsoft.AspNetCore.Razor.Design` fournit les tâches et les cibles de compilation Razor pour le projet.

  Les packages précédents sont inclus dans `Microsoft.AspNetCore.Mvc`. Le balisage suivant montre un fichier projet qui utilise le kit de développement logiciel (SDK) Razor pour générer des fichiers Razor pour une application ASP.NET Core Razor Pages :
    
  [!code-xml[](sdk/sample/RazorSDK.csproj)]
  
::: moniker-end

::: moniker range="= aspnetcore-2.1"

> [!WARNING]
> Les packages `Microsoft.AspNetCore.Razor.Design` et `Microsoft.AspNetCore.Mvc.Razor.Extensions` sont inclus dans le sous- [package Microsoft. AspNetCore. app](xref:fundamentals/metapackage-app). Toutefois, la référence de package sans version `Microsoft.AspNetCore.App` fournit un package à l’application qui n’inclut pas la dernière version de `Microsoft.AspNetCore.Razor.Design`. Les projets doivent référencer une version cohérente de `Microsoft.AspNetCore.Razor.Design` (ou `Microsoft.AspNetCore.Mvc`) afin que les derniers correctifs au moment de la génération pour Razor soient inclus. Pour plus d’informations, consultez [ce problème GitHub](https://github.com/aspnet/Razor/issues/2553).

::: moniker-end

### <a name="properties"></a>Propriétés

Les propriétés suivantes contrôlent le comportement du SDK Razor dans le cadre d’une build de projet :

* `RazorCompileOnBuild` &ndash; lorsque `true`, compile et émet l’assembly Razor dans le cadre de la génération du projet. La valeur par défaut est `true`.
* `RazorCompileOnPublish` &ndash; lorsque `true`, compile et émet l’assembly Razor dans le cadre de la publication du projet. La valeur par défaut est `true`.

Les propriétés et les éléments du tableau suivant sont utilisés pour configurer les entrées et la sortie vers le kit de développement logiciel (SDK) Razor.

::: moniker range=">= aspnetcore-3.0"

> [!WARNING]
> À compter de ASP.NET Core 3,0, les affichages MVC ou Razor Pages ne sont pas pris en charge par défaut si les propriétés MSBuild `RazorCompileOnBuild` ou `RazorCompileOnPublish` dans le fichier projet sont désactivées. Les applications doivent ajouter une référence explicite au package [Microsoft. AspNetCore. Mvc. Razor. RuntimeCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.RuntimeCompilation) si l’application s’appuie sur la compilation au moment de l’exécution pour traiter les fichiers *. cshtml* .

::: moniker-end

| Éléments | Description |
| ----- | ----------- |
| `RazorGenerate` | Éléments Item (fichiers *. cshtml* ) qui sont des entrées de la génération de code. |
| `RazorComponent` | Éléments Item (fichiers *. Razor* ) qui sont des entrées de la génération de code du composant Razor. |
| `RazorCompile` | Éléments Item (fichiers *. cs* ) qui sont des entrées dans les cibles de compilation Razor. Utilisez cette `ItemGroup` pour spécifier des fichiers supplémentaires à compiler dans l’assembly Razor. |
| `RazorTargetAssemblyAttribute` | Composants d’élément utilisés pour générer le code d’attributs pour l’assembly Razor. Exemple :  <br>`RazorAssemblyAttribute`<br>`Include="System.Reflection.AssemblyMetadataAttribute"`<br>`_Parameter1="BuildSource" _Parameter2="https://docs.microsoft.com/">` |
| `RazorEmbeddedResource` | Éléments Item ajoutés en tant que ressources incorporées à l’assembly Razor généré. |

| Property | Description |
| -------- | ----------- |
| `RazorTargetName` | Nom de fichier (sans extension) de l’assembly produit par Razor. | 
| `RazorOutputPath` | Répertoire de sortie Razor. |
| `RazorCompileToolset` | Permet de déterminer l’ensemble d’outils utilisé pour générer l’assembly Razor. Les valeurs valides sont `Implicit`, `RazorSDK` et `PrecompilationTool`. |
| [EnableDefaultContentItems](https://github.com/aspnet/websdk/blob/rel-2.0.0/src/ProjectSystem/Microsoft.NET.Sdk.Web.ProjectSystem.Targets/netstandard1.0/Microsoft.NET.Sdk.Web.ProjectSystem.targets#L21) | La valeur par défaut est `true`. Lorsque `true`, comprend les fichiers *Web. config*, *. JSON*et *. cshtml* en tant que contenu dans le projet. Lorsqu’ils sont référencés via `Microsoft.NET.Sdk.Web`, les fichiers des fichiers *wwwroot* et config sont également inclus. |
| `EnableDefaultRazorGenerateItems` | Si la valeur est `true`, inclut les fichiers *.cshtml* des éléments `Content` dans les éléments `RazorGenerate`. |
| `GenerateRazorTargetAssemblyInfo` | Lorsque `true`, génère un fichier *. cs* contenant les attributs spécifiés par `RazorAssemblyAttribute` et comprend le fichier dans la sortie de compilation. |
| `EnableDefaultRazorTargetAssemblyInfoAttributes` | Si la valeur est `true`, ajoute un ensemble par défaut d’attributs d’assembly à `RazorAssemblyAttribute`. |
| `CopyRazorGenerateFilesToPublishDirectory` | Lorsque `true`, copie `RazorGenerate` éléments ( *. cshtml*) dans le répertoire de publication. En règle générale, les fichiers Razor ne sont pas requis pour une application publiée s’ils participent à la compilation au moment de la génération ou de la publication. La valeur par défaut est `false`. |
| `CopyRefAssembliesToPublishDirectory` | Si la valeur est `true`, copie les éléments d’assembly de référence dans le répertoire de publication. En général, les assemblys de référence ne sont pas requis pour une application publiée si la compilation Razor se produit au moment de la génération ou de la publication. Affectez la valeur `true` si votre application publiée requiert la compilation au moment de l’exécution. Par exemple, affectez la valeur `true` si l’application modifie les fichiers *. cshtml* au moment de l’exécution ou utilise des vues incorporées. La valeur par défaut est `false`. |
| `IncludeRazorContentInPack` | Lorsque `true`, tous les éléments de contenu Razor (fichiers *. cshtml* ) sont marqués pour être inclus dans le package NuGet généré. La valeur par défaut est `false`. |
| `EmbedRazorGenerateSources` | Si la valeur est `true`, ajoute des éléments RazorGenerate ( *.cshtml*) comme fichiers incorporés à l’assembly Razor généré. La valeur par défaut est `false`. |
| `UseRazorBuildServer` | Si la valeur est `true`, utilise un processus de serveur de build persistant pour décharger le travail de génération de code. Utilise par défaut la valeur de `UseSharedCompilation`. |
| `GenerateMvcApplicationPartsAssemblyAttributes` | Lorsque `true`, le kit de développement logiciel (SDK) génère des attributs supplémentaires utilisés par MVC lors de l’exécution pour effectuer la détection des parties de l’application. |

Pour plus d’informations sur les propriétés, voir [Propriétés MSBuild](/visualstudio/msbuild/msbuild-properties).

### <a name="targets"></a>Cibles

Le SDK Razor définit deux cibles principales :

* `RazorGenerate` le code &ndash; génère des fichiers *. cs* à partir d’éléments `RazorGenerate` élément. Utilisez la propriété `RazorGenerateDependsOn` pour spécifier des cibles supplémentaires qui peuvent s’exécuter avant ou après cette cible.
* `RazorCompile` &ndash; compile les fichiers *. cs* générés dans en un assembly Razor. Utilisez la `RazorCompileDependsOn` pour spécifier des cibles supplémentaires qui peuvent s’exécuter avant ou après cette cible.
* `RazorComponentGenerate` le code &ndash; génère des fichiers *. cs* pour les éléments `RazorComponent` élément. Utilisez la propriété `RazorComponentGenerateDependsOn` pour spécifier des cibles supplémentaires qui peuvent s’exécuter avant ou après cette cible.

### <a name="runtime-compilation-of-razor-views"></a>Compilation au moment du runtime des vues Razor

* Par défaut, le SDK Razor ne publie pas les assemblys de référence nécessaires à l’exécution de la compilation au moment du runtime. Cela entraîne des échecs de compilation quand le modèle d’application s’appuie sur la compilation au moment du runtime&mdash;par exemple, l’application utilise des vues incorporées ou change les vues une fois l’application publiée. Définissez `CopyRefAssembliesToPublishDirectory` sur `true` pour continuer à publier les assemblys de référence.

* Pour une application Web, vérifiez que votre application cible le kit de développement logiciel (SDK) `Microsoft.NET.Sdk.Web`.

## <a name="razor-language-version"></a>Version du langage Razor

Quand vous ciblez le kit de développement logiciel (SDK) `Microsoft.NET.Sdk.Web`, la version du langage Razor est déduite de la version du Framework cible de l’application. Pour les projets ciblant le kit de développement logiciel (SDK) `Microsoft.NET.Sdk.Razor` ou dans le cas rare où l’application requiert une version de langage Razor différente de la valeur déduite, une version peut être configurée en définissant la propriété `<RazorLangVersion>` dans le fichier projet de l’application :

```xml
<PropertyGroup>
  <RazorLangVersion>{VERSION}</RazorLangVersion>
</PropertyGroup>
```

La version linguistique de Razor est étroitement intégrée à la version du runtime pour laquelle elle a été générée. Le ciblage d’une version de langage qui n’est pas conçue pour le runtime n’est pas pris en charge et génère probablement des erreurs de Build.

## <a name="additional-resources"></a>Ressources supplémentaires

* [Ajouts au format csproj pour .NET Core](/dotnet/core/tools/csproj)
* [Éléments communs des projets MSBuild](/visualstudio/msbuild/common-msbuild-project-items)
