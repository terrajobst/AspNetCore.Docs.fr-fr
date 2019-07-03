---
title: Configurer l’éditeur de liens pour ASP.NET Core Blazor
author: guardrex
description: Découvrez comment contrôler l’éditeur de liens de langage intermédiaire (IL) lors de la création d’une application Blazor.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 06/14/2019
uid: host-and-deploy/blazor/configure-linker
ms.openlocfilehash: bdddae16885f45df2c10e4d98b1c33eb11dfdf24
ms.sourcegitcommit: 4ef0362ef8b6e5426fc5af18f22734158fe587e1
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/17/2019
ms.locfileid: "67153207"
---
# <a name="configure-the-linker-for-aspnet-core-blazor"></a><span data-ttu-id="1b12b-103">Configurer l’éditeur de liens pour ASP.NET Core Blazor</span><span class="sxs-lookup"><span data-stu-id="1b12b-103">Configure the Linker for ASP.NET Core Blazor</span></span>

<span data-ttu-id="1b12b-104">Par [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="1b12b-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="1b12b-105">Blazor effectue une liaison de [langage intermédiaire (IL)](/dotnet/standard/managed-code#intermediate-language--execution) pendant un build de mise en production pour supprimer un langage intermédiaire inutile des assemblys de sortie de l’application.</span><span class="sxs-lookup"><span data-stu-id="1b12b-105">Blazor performs [Intermediate Language (IL)](/dotnet/standard/managed-code#intermediate-language--execution) linking during a Release build to remove unnecessary IL from the app's output assemblies.</span></span>

<span data-ttu-id="1b12b-106">Contrôlez la liaison d’assembly avec l’une des approches suivantes :</span><span class="sxs-lookup"><span data-stu-id="1b12b-106">Control assembly linking using either of the following approaches:</span></span>

* <span data-ttu-id="1b12b-107">Désactiver la liaison globalement avec une [propriété MSBuild](#disable-linking-with-a-msbuild-property).</span><span class="sxs-lookup"><span data-stu-id="1b12b-107">Disable linking globally with a [MSBuild property](#disable-linking-with-a-msbuild-property).</span></span>
* <span data-ttu-id="1b12b-108">Contrôler la liaison pour chaque assembly avec un [fichier de configuration](#control-linking-with-a-configuration-file).</span><span class="sxs-lookup"><span data-stu-id="1b12b-108">Control linking on a per-assembly basis with a [configuration file](#control-linking-with-a-configuration-file).</span></span>

## <a name="disable-linking-with-a-msbuild-property"></a><span data-ttu-id="1b12b-109">Désactiver la liaison avec une propriété MSBuild</span><span class="sxs-lookup"><span data-stu-id="1b12b-109">Disable linking with a MSBuild property</span></span>

<span data-ttu-id="1b12b-110">La liaison est activée par défaut dans le mode de mise en production quand une application est générée, ce qui inclut la publication.</span><span class="sxs-lookup"><span data-stu-id="1b12b-110">Linking is enabled by default in Release mode when an app is built, which includes publishing.</span></span> <span data-ttu-id="1b12b-111">Pour désactiver la liaison pour tous les assemblys, définissez la propriété MSBuild `<BlazorLinkOnBuild>` sur `false` dans le fichier projet :</span><span class="sxs-lookup"><span data-stu-id="1b12b-111">To disable linking for all assemblies, set the `<BlazorLinkOnBuild>` MSBuild property to `false` in the project file:</span></span>

```xml
<PropertyGroup>
  <BlazorLinkOnBuild>false</BlazorLinkOnBuild>
</PropertyGroup>
```

## <a name="control-linking-with-a-configuration-file"></a><span data-ttu-id="1b12b-112">Contrôler la liaison avec un fichier de configuration</span><span class="sxs-lookup"><span data-stu-id="1b12b-112">Control linking with a configuration file</span></span>

<span data-ttu-id="1b12b-113">Contrôlez la liaison pour chaque assembly en fournissant un fichier de configuration XML et en spécifiant le fichier en tant qu’élément MSBuild dans le fichier projet :</span><span class="sxs-lookup"><span data-stu-id="1b12b-113">Control linking on a per-assembly basis by providing an XML configuration file and specifying the file as a MSBuild item in the project file:</span></span>

```xml
<ItemGroup>
  <BlazorLinkerDescriptor Include="Linker.xml" />
</ItemGroup>
```

<span data-ttu-id="1b12b-114">*Linker.xml* :</span><span class="sxs-lookup"><span data-stu-id="1b12b-114">*Linker.xml*:</span></span>

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!--
  This file specifies which parts of the BCL or Blazor packages must not be
  stripped by the IL Linker even if they aren't referenced by user code.
-->
<linker>
  <assembly fullname="mscorlib">
    <!--
      Preserve the methods in WasmRuntime because its methods are called by 
      JavaScript client-side code to implement timers.
      Fixes: https://github.com/aspnet/Blazor/issues/239
    -->
    <type fullname="System.Threading.WasmRuntime" />
  </assembly>
  <assembly fullname="System.Core">
    <!--
      System.Linq.Expressions* is required by Json.NET and any 
      expression.Compile caller. The assembly isn't stripped.
    -->
    <type fullname="System.Linq.Expressions*" />
  </assembly>
  <!--
    In this example, the app's entry point assembly is listed. The assembly
    isn't stripped by the IL Linker.
  -->
  <assembly fullname="MyCoolBlazorApp" />
</linker>
```

<span data-ttu-id="1b12b-115">Pour plus d’informations, consultez [Éditeur de liens de langage intermédiaire : Syntaxe du descripteur xml](https://github.com/mono/linker/blob/master/src/linker/README.md#syntax-of-xml-descriptor).</span><span class="sxs-lookup"><span data-stu-id="1b12b-115">For more information, see [IL Linker: Syntax of xml descriptor](https://github.com/mono/linker/blob/master/src/linker/README.md#syntax-of-xml-descriptor).</span></span>
