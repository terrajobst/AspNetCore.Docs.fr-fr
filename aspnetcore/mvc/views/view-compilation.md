---
title: Précompilation et compilation de fichiers Razor dans ASP.NET Core
author: rick-anderson
description: Découvrez les avantages de la précompilation des fichiers Razor et comment effectuer la précompilation des fichiers Razor dans une application ASP.NET Core.
monikerRange: '>= aspnetcore-1.1'
ms.author: riande
ms.custom: mvc
ms.date: 01/23/2019
uid: mvc/views/view-compilation
ms.openlocfilehash: 2720708f8e58fdc55b82bfb56665005170e79934
ms.sourcegitcommit: d5223cf6a2cf80b4f5dc54169b0e376d493d2d3a
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/24/2019
ms.locfileid: "54889754"
---
# <a name="razor-file-compilation-in-aspnet-core"></a>Compilation de fichiers Razor dans ASP.NET Core

Par [Rick Anderson](https://twitter.com/RickAndMSFT)

::: moniker range="= aspnetcore-1.1"

Un fichier Razor est compilé au moment de l’exécution, quand la vue MVC associée est appelée. La publication de fichiers Razor au moment de la génération n’est pas prise en charge. Vous pouvez éventuellement compiler les fichiers Razor au moment de la publication et les déployer avec l’application&mdash;en utilisant l’outil de précompilation.

::: moniker-end

::: moniker range="= aspnetcore-2.0"

Un fichier Razor est compilé au moment de l’exécution, quand la vue MVC ou la page Razor associée est appelée. La publication de fichiers Razor au moment de la génération n’est pas prise en charge. Vous pouvez éventuellement compiler les fichiers Razor au moment de la publication et les déployer avec l’application&mdash;en utilisant l’outil de précompilation.

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

Un fichier Razor est compilé au moment de l’exécution, quand la vue MVC ou la page Razor associée est appelée. Les fichiers Razor sont compilés au moment de la génération et de la publication à l’aide du kit [SDK Razor](xref:razor-pages/sdk).

::: moniker-end

## <a name="precompilation-considerations"></a>Considérations relatives à la précompilation

Les effets secondaires de la précompilation des fichiers Razor sont les suivants :

* Un plus petit bundle publié
* Un temps de démarrage plus court
* Vous ne pouvez pas modifier les fichiers Razor&mdash;le contenu associé est absent du bundle publié.

## <a name="deploy-precompiled-files"></a>Déployer les fichiers précompilés

::: moniker range=">= aspnetcore-2.1"

La compilation au moment de la génération et de la publication des fichiers Razor est activée par défaut par le kit SDK Razor. La modification des fichiers Razor une fois que ceux-ci sont mis à jour est prise en charge au moment de la génération. Par défaut, seul le fichier *Views.dll* compilé est déployé avec votre application. Aucun fichier *.cshtml* n’est déployé.

> [!IMPORTANT]
> L’outil de précompilation sera supprimé dans ASP.NET Core 3.0. Nous vous recommandons de migrer vers le [SDK Razor](xref:razor-pages/sdk).
>
> Le kit SDK Razor est actif seulement si aucune propriété spécifique à la précompilation n’est définie dans le fichier projet. Par exemple, la définition de la propriété `MvcRazorCompileOnPublish` du fichier *.csproj* sur la valeur `true` désactive le kit SDK Razor.

::: moniker-end

::: moniker range="= aspnetcore-2.0"

Si votre projet cible le .NET Framework, installez le package NuGet [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation/) :

[!code-xml[](view-compilation/sample/DotNetFrameworkProject.csproj?name=snippet_ViewCompilationPackage)]

Si votre projet cible .NET Core, aucune modification n’est nécessaire.

Les modèles de projet ASP.NET Core 2.x définissent implicitement la propriété `MvcRazorCompileOnPublish` sur la valeur `true` par défaut. Par conséquent, cet élément peut être supprimé sans problème du fichier *.csproj*.

> [!IMPORTANT]
> L’outil de précompilation sera supprimé dans ASP.NET Core 3.0. Nous vous recommandons de migrer vers le [SDK Razor](xref:razor-pages/sdk).
>
> La précompilation de fichiers Razor n’est pas disponible quand vous effectuez un [déploiement autonome (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd) dans ASP.NET Core 2.0.

::: moniker-end

::: moniker range="= aspnetcore-1.1"

Définissez la propriété `MvcRazorCompileOnPublish` sur la valeur `true`, puis installez le package NuGet [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation/). Le fichier *.csproj* suivant en est un exemple, avec en surbrillance ces paramètres :

[!code-xml[](view-compilation/sample/MvcRazorCompileOnPublish.csproj?highlight=4,10)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

Préparer l’application pour un [déploiement dépendant de l’infrastructure](/dotnet/core/deploying/#framework-dependent-deployments-fdd) avec la [commande de publication .NET Core CLI](/dotnet/core/tools/dotnet-publish). Par exemple, exécutez la commande suivante à la racine du projet :

```console
dotnet publish -c Release
```

Un fichier *<nom_projet>.PrecompiledViews.dll*, contenant les fichiers Razor compilés, est généré lorsque la précompilation réussit. Par exemple, la capture d’écran ci-dessous illustre le contenu du fichier *Index.cshtml* à l’intérieur de *WebApplication1.PrecompiledViews.dll*:

![Vues Razor dans la DLL](view-compilation/_static/razor-views-in-dll.png)

::: moniker-end

## <a name="recompile-razor-files-on-change"></a>Recompiler les fichiers Razor en cas de modification

<xref:Microsoft.AspNetCore.Mvc.Razor.RazorViewEngineOptions> `AllowRecompilingViewsOnFileChange` obtient ou définit une valeur qui détermine si les fichiers Razor (vues Razor et Razor Pages) sont recompilés et mis à jour si les fichiers changent sur le disque.

Lorsqu’il est défini sur `true`, [IFileProvider.Watch](xref:Microsoft.Extensions.FileProviders.IFileProvider.Watch*) observe si des modifications sont apportées aux fichiers Razor dans les instances <xref:Microsoft.Extensions.FileProviders.IFileProvider> configurées.

La valeur par défaut de `true` pour :

* Applications ASP.NET Core 2.1 ou versions antérieures.
* Applications ASP.NET Core 2.2 ou versions ultérieures dans l’environnement de développement.

`AllowRecompilingViewsOnFileChange` est associé à un commutateur de compatibilité et peut fournir un comportement différent selon la version de compatibilité configurée pour l’application. La configuration de l’application en définissant `AllowRecompilingViewsOnFileChange` est prioritaire sur la valeur déduite à partir de la version de compatibilité de l’application.

Si la version de compatibilité de l’application est définie sur <xref:Microsoft.AspNetCore.Mvc.CompatibilityVersion.Version_2_1> ou une version antérieure, `AllowRecompilingViewsOnFileChange` a la valeur `true` sauf configuration explicite.

Si la version de compatibilité de l’application est définie sur `CompatibilityVersion.Version_2_2` ou une version ultérieure, `AllowRecompilingViewsOnFileChange` a la valeur `false`, sauf si l’environnement est de type Développement ou si la valeur est explicitement configurée.

Pour des conseils et des exemples concernant la définition de la version de compatibilité de l’application, consultez <xref:mvc/compatibility-version>.

## <a name="additional-resources"></a>Ressources supplémentaires

::: moniker range="= aspnetcore-1.1"

* <xref:mvc/views/overview>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

* <xref:razor-pages/index>
* <xref:mvc/views/overview>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

* <xref:razor-pages/index>
* <xref:mvc/views/overview>
* <xref:razor-pages/sdk>

::: moniker-end
