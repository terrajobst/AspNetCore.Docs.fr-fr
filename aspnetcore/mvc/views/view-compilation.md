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

Les vues Razor sont compilées à l'exécution lors de leur premier appel. A partir de  ASP.NET Core 1.1.0 il est possible de compiler les vues Razor et les déployer avec l’application &mdash; (précompilation).  Les modèles de projet ASP.NET Core 2.x activent la précompilation par défaut.

> [!IMPORTANT]
> La précompilation de vue Razor est actuellement pas disponible lorsque vous effectuez un [déploiement autonome (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd) dans ASP.NET 2.0 de base. La fonctionnalité sera disponible les applications SCD (Self-Contained Deployment) lorsque Asp.Net Core 2.1 sera publié.  Pour plus d’informations, consultez [la compilation de la vue échoue lors de la compilation croisée pour Linux sur Windows](https://github.com/aspnet/MvcPrecompilation/issues/102).

Considérations relatives à la précompilation :

* La précompilation des vues réduit la taille du bundle publié et le temps de démarrage.
* Vous ne pouvez pas modifier les vues précompilées. Elles ne font pas partie de l’application publiée. 

Pour déployer des vues précompilées :

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Si votre projet cible .NET Framework, ajoutez une référence de package à [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation/) :

```xml
<PackageReference Include="Microsoft.AspNetCore.Mvc.Razor.ViewCompilation" Version="2.0.0" PrivateAssets="All" />
```

Si votre projet cible .NET Core, aucune modification n’est nécessaire.

Les modèles de projet ASP.NET Core 2.x définissent la valeur `MvcRazorCompileOnPublish` à `true` par défaut, ce qui signifie que ce nœud peut être supprimé en toute sécurité le fihier *.csproj*. Si vous préférez être explicite, il n’existe aucun risque à définir le paramètre de la propriété `MvcRazorCompileOnPublish` à `true`. Le fichier *.csproj* suivant en est un exemple exemple, avec en surbrillance ce paramètre :

[!code-xml[Main](view-compilation\sample\MvcRazorCompileOnPublish2.csproj?highlight=5)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Définissez `MvcRazorCompileOnPublish` à `true` et inclure une référence de package à `Microsoft.AspNetCore.Mvc.Razor.ViewCompilation`. Le fichier *.csproj* suivant en est un exemple, avec en surbrillance ces paramètres :

[!code-xml[Main](view-compilation\sample\MvcRazorCompileOnPublish.csproj?highlight=5,12)]

---

Préparez l’application pour un [déploiement dépendant du framework (FDD)](/dotnet/core/deploying/#framework-dependent-deployments-fdd) en exécutant une commande similaire à la suivante à la racine du projet :

```console
dotnet publish -c Release
```

A *< nom_projet >. PrecompiledViews.dll* fichier, qui contient les vues Razor compilés, est généré lorsque la précompilation réussit. Par exemple, la capture d’écran ci-dessous illustre le contenu de *Index.cshtml* à l’intérieur de *WebApplication1.PrecompiledViews.dll*:

![Vues Razor dans la DLL](view-compilation/_static/razor-views-in-dll.png)
