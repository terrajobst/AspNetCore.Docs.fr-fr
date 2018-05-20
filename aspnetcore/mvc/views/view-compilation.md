---
title: Précompilation et compilation de vues Razor dans ASP.NET Core
author: rick-anderson
description: Découvrez comment activer la précompilation et la compilation de vues Razor MVC dans les applications ASP.Net Core.
manager: wpickett
ms.author: riande
ms.date: 12/13/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: mvc/views/view-compilation
ms.openlocfilehash: 013ca0d149c6415b5e6825aa5a48e93ae48f6728
ms.sourcegitcommit: 0063338c2e130409081bb60fcffa0c3f190cd46a
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/12/2018
---
# <a name="razor-file-cshtml-compilation-in-aspnet-core"></a>Compilation du fichier Razor (.cshtml) dans ASP.NET Core

Par [Rick Anderson](https://twitter.com/RickAndMSFT)

Les vues Razor sont compilées à l'exécution lors de leur premier appel. ASP.NET Core 2.1.0 ou ultérieur compile les vues au moment de la génération et de la publication à l’aide du [SDK Razor](/aspnetcore/mvc/razor-pages/sdk). Dans ASP.NET Core 1.1 et ASP.NET Core 2.0, vous pouvez compiler les vues au moment de la publication et les déployer avec l’application à l’aide de l’outil de précompilation. 



Considérations relatives à la précompilation :

* La précompilation des vues réduit la taille du bundle publié et le temps de démarrage.
* Vous ne pouvez pas modifier les vues précompilées. Elles ne font pas partie de l’application publiée. 

Pour déployer des vues précompilées :

# <a name="aspnet-core-21tabaspnetcore21"></a>[ASP.NET Core 2.1](#tab/aspnetcore21/)
La compilation au moment de la génération et de la publication des fichiers Razor est activée par défaut par le SDK Razor. La modification des fichiers Razor une fois qu’ils sont mis à jour est prise en charge au moment de la génération. Par défaut, seul le fichier *Views.dll* compilé est déployé avec votre application (aucun fichier cshtml n’est déployé). 
    
> [!IMPORTANT]
> Le SDK Razor est actif uniquement si aucune propriété de précompilation spécifique n’est définie dans votre fichier projet. Par exemple, le fait de définir `MvcRazorCompileOnPublish` dans votre fichier *.csproj* désactive le SDK Razor.

# <a name="aspnet-core-20tabaspnetcore20"></a>[ASP.NET Core 2.0](#tab/aspnetcore20/)

Si votre projet cible .NET Framework, ajoutez une référence de package à [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation/) :

```xml
<PackageReference Include="Microsoft.AspNetCore.Mvc.Razor.ViewCompilation" Version="2.0.0" PrivateAssets="All" />
```

Si votre projet cible .NET Core, aucune modification n’est nécessaire.

Les modèles de projet ASP.NET Core 2.x définissent `MvcRazorCompileOnPublish` avec la valeur `true` par défaut, ce qui signifie que ce nœud peut être supprimé en toute sécurité du fichier *.csproj*.
    
> [!IMPORTANT]
> La précompilation de vue Razor n’est pas disponible quand vous effectuez un [déploiement autonome (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd) dans ASP.NET Core 2.0. 

Préparer l’application pour un [déploiement dépendant de l’infrastructure](/dotnet/core/deploying/#framework-dependent-deployments-fdd) avec la [commande de publication .NET Core CLI](/dotnet/core/tools/dotnet-publish). Par exemple, exécutez la commande suivante à la racine du projet :

```console
dotnet publish -c Release
```

A *< nom_projet >. PrecompiledViews.dll* fichier, qui contient les vues Razor compilés, est généré lorsque la précompilation réussit. Par exemple, la capture d’écran ci-dessous illustre le contenu de *Index.cshtml* à l’intérieur de *WebApplication1.PrecompiledViews.dll*:

![Vues Razor dans la DLL](view-compilation/_static/razor-views-in-dll.png)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

Définissez `MvcRazorCompileOnPublish` avec la valeur `true` et incluez une référence de package à `Microsoft.AspNetCore.Mvc.Razor.ViewCompilation`. Le fichier *.csproj* suivant en est un exemple, avec en surbrillance ces paramètres :

[!code-xml[](view-compilation/sample/MvcRazorCompileOnPublish.csproj?highlight=5,12)]

---

