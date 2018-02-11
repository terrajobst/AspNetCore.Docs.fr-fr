---
title: "Précompilation et compilation de vues Razor"
author: rick-anderson
description: "Document de référence expliquant comment activer la précompilation et la compilation de vues Razor MVC dans les applications ASP.NET Core."
manager: wpickett
ms.author: riande
ms.date: 12/13/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: mvc/views/view-compilation
ms.openlocfilehash: bd3f4470035b0375fc79aa7caa73b60ba6fc4f53
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2018
---
# <a name="razor-view-compilation-and-precompilation-in-aspnet-core"></a>Précompilation et compilation de vues Razor dans ASP.NET Core

Par [Rick Anderson](https://twitter.com/RickAndMSFT)

Les vues Razor sont compilées au moment de l’exécution quand une vue est appelée. Dans ASP.NET Core 1.1.0 et ultérieur, les vues Razor peuvent éventuellement être compilées et déployées avec l’application &mdash; ce processus est appelé précompilation. Les modèles de projet ASP.NET Core 2.x activent la précompilation par défaut.

> [!IMPORTANT]
> La précompilation de vues Razor n’est pas disponible quand vous effectuez un [déploiement autonome (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd) dans ASP.NET Core 2.0. Cette fonctionnalité sera disponible pour les déploiements SCD dans la future version 2.1. Pour plus d’informations, consultez [View compilation fails when cross-compiling for Linux on Windows](https://github.com/aspnet/MvcPrecompilation/issues/102).

Considérations relatives à la précompilation :

* La précompilation des vues réduit la taille du bundle publié et le temps de démarrage.
* Vous ne pouvez pas modifier les fichiers Razor après avoir précompilé des vues. Les vues modifiées ne seraient pas présentes dans le bundle publié. 

Pour déployer des vues précompilées :

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Si votre projet cible .NET Framework, ajoutez une référence de package à [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation/) :

```xml
<PackageReference Include="Microsoft.AspNetCore.Mvc.Razor.ViewCompilation" Version="2.0.0" PrivateAssets="All" />
```

Si votre projet cible .NET Core, aucune modification n’est nécessaire.

Les modèles de projet ASP.NET Core 2.x définissent implicitement `MvcRazorCompileOnPublish` à la valeur `true` par défaut. Ce nœud peut donc être supprimé en toute sécurité dans le fichier *.csproj*. Si vous préférez une définition explicite, vous pouvez sans problème définir la propriété `MvcRazorCompileOnPublish` à la valeur `true`. L’exemple *.csproj* suivant illustre ce paramètre :

[!code-xml[Main](view-compilation\sample\MvcRazorCompileOnPublish2.csproj?highlight=5)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Définissez `MvcRazorCompileOnPublish` avec la valeur `true` et ajoutez une référence de package à `Microsoft.AspNetCore.Mvc.Razor.ViewCompilation`. L’exemple *.csproj* suivant illustre ces paramètres :

[!code-xml[Main](view-compilation\sample\MvcRazorCompileOnPublish.csproj?highlight=5,12)]

---

Préparez l’application pour un [déploiement dépendant du framework (FDD)](/dotnet/core/deploying/#framework-dependent-deployments-fdd) en exécutant une commande similaire à la suivante à la racine du projet :

```console
dotnet publish -c Release
```

Quand la précompilation réussit, un fichier *<nom_projet>.PrecompiledViews.dll* contenant les vues Razor compilées est créé. Par exemple, la capture d’écran ci-dessous illustre le contenu des vues *Index.cshtml* dans le fichier *WebApplication1.PrecompiledViews.dll* :

![Vues Razor dans la DLL](view-compilation/_static/razor-views-in-dll.png)
