---
title: Compilation de fichiers Razor dans ASP.NET Core
author: rick-anderson
description: Découvrez comment se produit la compilation de fichiers Razor dans une application ASP.NET Core.
monikerRange: '>= aspnetcore-1.1'
ms.author: riande
ms.custom: mvc
ms.date: 06/20/2019
uid: mvc/views/view-compilation
ms.openlocfilehash: ff66148fc9aad2871f9f55ce76b5a0dacb0ad10c
ms.sourcegitcommit: 9f11685382eb1f4dd0fb694dea797adacedf9e20
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/21/2019
ms.locfileid: "67313787"
---
# <a name="razor-file-compilation-in-aspnet-core"></a>Compilation de fichiers Razor dans ASP.NET Core

Par [Rick Anderson](https://twitter.com/RickAndMSFT)

::: moniker range="= aspnetcore-1.1"

Un fichier Razor est compilé au moment de l’exécution, quand la vue MVC associée est appelée. La publication de fichiers Razor au moment de la génération n’est pas prise en charge. Vous pouvez éventuellement compiler les fichiers Razor au moment de la publication et les déployer avec l’application&mdash;en utilisant l’outil de précompilation.

::: moniker-end

::: moniker range="= aspnetcore-2.0"

Un fichier Razor est compilé au moment de l’exécution, quand la vue MVC ou la page Razor associée est appelée. La publication de fichiers Razor au moment de la génération n’est pas prise en charge. Vous pouvez éventuellement compiler les fichiers Razor au moment de la publication et les déployer avec l’application&mdash;en utilisant l’outil de précompilation.

::: moniker-end

::: moniker range=">= aspnetcore-2.1 <= aspnetcore-2.2"

Un fichier Razor est compilé au moment de l’exécution, quand la vue MVC ou la page Razor associée est appelée. Les fichiers Razor sont compilés au moment de la génération et de la publication à l’aide du kit [SDK Razor](xref:razor-pages/sdk).

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

Les fichiers Razor sont compilés au moment de la génération et de la publication à l’aide du kit [SDK Razor](xref:razor-pages/sdk). La compilation au moment de l’exécution peut éventuellement être activée en configurant votre application.

::: moniker-end

## <a name="razor-compilation"></a>Compilation Razor

::: moniker range=">= aspnetcore-3.0"
La compilation au moment de la génération et de la publication des fichiers Razor est activée par défaut par le kit SDK Razor. Quand elle est activée, la compilation au moment de l’exécution complète la compilation au moment de la génération, ce qui permet aux fichiers Razor d’être mis à jour s’ils sont modifiés.

::: moniker-end

::: moniker range=">= aspnetcore-2.1 <= aspnetcore-2.2"

La compilation au moment de la génération et de la publication des fichiers Razor est activée par défaut par le kit SDK Razor. La modification des fichiers Razor une fois que ceux-ci sont mis à jour est prise en charge au moment de la génération. Par défaut, seuls les fichiers *Views.dll* et les fichiers autres que *.cshtml* compilés ou les assemblys de référence nécessaires à la compilation des fichiers Razor sont déployés avec votre application.

> [!IMPORTANT]
> L’outil de précompilation est désormais déprécié et sera supprimé dans ASP.NET Core 3.0. Nous vous recommandons de migrer vers le [SDK Razor](xref:razor-pages/sdk).
>
> Le kit SDK Razor est actif seulement si aucune propriété spécifique à la précompilation n’est définie dans le fichier projet. Par exemple, la définition de la propriété `MvcRazorCompileOnPublish` du fichier *.csproj* sur la valeur `true` désactive le kit SDK Razor.

::: moniker-end

::: moniker range="= aspnetcore-2.0"

Si votre projet cible le .NET Framework, installez le package NuGet [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation/) :

[!code-xml[](view-compilation/sample/DotNetFrameworkProject.csproj?name=snippet_ViewCompilationPackage)]

Si votre projet cible .NET Core, aucune modification n’est nécessaire.

Les modèles de projet ASP.NET Core 2.x définissent implicitement la propriété `MvcRazorCompileOnPublish` sur la valeur `true` par défaut. Par conséquent, cet élément peut être supprimé sans problème du fichier *.csproj*.

> [!IMPORTANT]
> L’outil de précompilation est désormais déprécié et sera supprimé dans ASP.NET Core 3.0. Nous vous recommandons de migrer vers le [SDK Razor](xref:razor-pages/sdk).
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

Un fichier *\<nom_projet>.PrecompiledViews.dll*, contenant les fichiers Razor compilés, est généré lorsque la précompilation réussit. Par exemple, la capture d’écran ci-dessous illustre le contenu du fichier *Index.cshtml* à l’intérieur de *WebApplication1.PrecompiledViews.dll*:

![Vues Razor dans la DLL](view-compilation/_static/razor-views-in-dll.png)

::: moniker-end

## <a name="runtime-compilation"></a>Compilation du runtime

::: moniker range="= aspnetcore-2.1"

La compilation au moment de la génération est complétée par la compilation au moment de l’exécution des fichiers Razor. ASP.NET Core MVC recompile les fichiers Razor quand le contenu d’un fichier *.cshtml* change.

::: moniker-end

::: moniker range="= aspnetcore-2.2"

La compilation au moment de la génération est complétée par la compilation au moment de l’exécution des fichiers Razor. <xref:Microsoft.AspNetCore.Mvc.Razor.RazorViewEngineOptions> <xref:Microsoft.AspNetCore.Mvc.Razor.RazorViewEngineOptions.AllowRecompilingViewsOnFileChange> obtient ou définit une valeur qui détermine si les fichiers Razor (vues Razor et Razor Pages) sont recompilés et mis à jour si les fichiers changent sur le disque.

La valeur par défaut de `true` pour :

* Si la version de compatibilité de l’application est définie sur <xref:Microsoft.AspNetCore.Mvc.CompatibilityVersion.Version_2_1> ou une version antérieure
* Si la version de compatibilité de l’application est définie sur <xref:Microsoft.AspNetCore.Mvc.CompatibilityVersion.Version_2_2> ou ultérieur et que l’application se trouve dans l’environnement de développement <xref:Microsoft.AspNetCore.Hosting.HostingEnvironmentExtensions.IsDevelopment*>. En d’autres termes, les fichiers Razor ne se recompilent pas dans un environnement non centré sur le développement, sauf si <xref:Microsoft.AspNetCore.Mvc.Razor.RazorViewEngineOptions.AllowRecompilingViewsOnFileChange> est explicitement défini.

Pour des conseils et des exemples concernant la définition de la version de compatibilité de l’application, consultez <xref:mvc/compatibility-version>.

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

La compilation au moment de l’exécution est activée à l’aide du package `Microsoft.AspNetCore.Mvc.Razor.RuntimeCompilation`. Pour activer la compilation au moment de l’exécution, les applications doivent :

* Installer le package NuGet [Microsoft.AspNetCore.Mvc.Razor.RuntimeCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.RuntimeCompilation/).
* Mettre à jour la méthode `Startup.ConfigureServices` du projet pour inclure un appel à `AddRazorRuntimeCompilation` :

  ```csharp
  services
      .AddControllersWithViews()
      .AddRazorRuntimeCompilation();
  ```

::: moniker-end

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
