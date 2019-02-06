---
title: Configurer l’éditeur de liens pour Blazor
author: guardrex
description: Découvrez comment contrôler l’éditeur de liens de langage intermédiaire (IL) lors de la création d’une application Blazor.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 01/29/2019
uid: host-and-deploy/razor-components/configure-linker
ms.openlocfilehash: c3c38ec2509344cc02f3895d5d0c2d35059d1d8e
ms.sourcegitcommit: ed76cc752966c604a795fbc56d5a71d16ded0b58
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/02/2019
ms.locfileid: "55668012"
---
# <a name="configure-the-linker-for-blazor"></a><span data-ttu-id="1d65c-103">Configurer l’éditeur de liens pour Blazor</span><span class="sxs-lookup"><span data-stu-id="1d65c-103">Configure the Linker for Blazor</span></span>

<span data-ttu-id="1d65c-104">Par [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="1d65c-104">By [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/razor-components-preview-notice.md)]

<span data-ttu-id="1d65c-105">Blazor effectue la liaison de [langage intermédiaire (IL)](/dotnet/standard/managed-code#intermediate-language--execution) lors de chaque génération afin de supprimer tout langage intermédiaire inutile des assemblys de sortie.</span><span class="sxs-lookup"><span data-stu-id="1d65c-105">Blazor performs [Intermediate Language (IL)](/dotnet/standard/managed-code#intermediate-language--execution) linking during each Release mode build to remove unnecessary IL from the output assemblies.</span></span>

<span data-ttu-id="1d65c-106">Vous pouvez contrôler la liaison d’assembly avec l’une des approches suivantes :</span><span class="sxs-lookup"><span data-stu-id="1d65c-106">You can control assembly linking with either of the following approaches:</span></span>

* <span data-ttu-id="1d65c-107">Désactiver la liaison globalement avec une propriété MSBuild.</span><span class="sxs-lookup"><span data-stu-id="1d65c-107">Disable linking globally with an MSBuild property.</span></span>
* <span data-ttu-id="1d65c-108">Contrôler la liaison pour chaque assembly avec un fichier de configuration.</span><span class="sxs-lookup"><span data-stu-id="1d65c-108">Control linking on a per-assembly basis with a configuration file.</span></span>

## <a name="disable-linking-with-an-msbuild-property"></a><span data-ttu-id="1d65c-109">Désactiver la liaison avec une propriété MSBuild</span><span class="sxs-lookup"><span data-stu-id="1d65c-109">Disable linking with an MSBuild property</span></span>

<span data-ttu-id="1d65c-110">La liaison est activée par défaut dans le mode de mise en production quand une application est générée, ce qui inclut la publication.</span><span class="sxs-lookup"><span data-stu-id="1d65c-110">Linking is enabled by default in Release mode when an app is built, which includes publishing.</span></span> <span data-ttu-id="1d65c-111">Pour désactiver la liaison pour tous les assemblys, définissez la propriété MSBuild `<BlazorLinkOnBuild>` sur `false` dans le fichier projet :</span><span class="sxs-lookup"><span data-stu-id="1d65c-111">To disable linking for all assemblies, set the `<BlazorLinkOnBuild>` MSBuild property to `false` in the project file:</span></span>

```xml
<PropertyGroup>
  <BlazorLinkOnBuild>false</BlazorLinkOnBuild>
</PropertyGroup>
```

## <a name="control-linking-with-a-configuration-file"></a><span data-ttu-id="1d65c-112">Contrôler la liaison avec un fichier de configuration</span><span class="sxs-lookup"><span data-stu-id="1d65c-112">Control linking with a configuration file</span></span>

<span data-ttu-id="1d65c-113">La liaison peut être contrôlée pour chaque assembly en fournissant un fichier de configuration XML et en spécifiant le fichier en tant qu’élément MSBuild dans le fichier projet.</span><span class="sxs-lookup"><span data-stu-id="1d65c-113">Linking can be controlled on a per-assembly basis by providing an XML configuration file and specifying the file as an MSBuild item in the project file.</span></span>

<span data-ttu-id="1d65c-114">Voici un exemple de fichier de configuration (*Linker.xml*) :</span><span class="sxs-lookup"><span data-stu-id="1d65c-114">The following is an example configuration file (*Linker.xml*):</span></span>

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

<span data-ttu-id="1d65c-115">Pour en savoir plus sur le format de fichier pour le fichier de configuration, consultez [Éditeur de liens de langage intermédiaire : Syntaxe du descripteur xml](https://github.com/mono/linker/blob/master/linker/README.md#syntax-of-xml-descriptor).</span><span class="sxs-lookup"><span data-stu-id="1d65c-115">To learn more about the file format for the configuration file, see [IL Linker: Syntax of xml descriptor](https://github.com/mono/linker/blob/master/linker/README.md#syntax-of-xml-descriptor).</span></span>

<span data-ttu-id="1d65c-116">Spécifiez le fichier de configuration dans le fichier projet avec l’élément `BlazorLinkerDescriptor` :</span><span class="sxs-lookup"><span data-stu-id="1d65c-116">Specify the configuration file in the project file with the `BlazorLinkerDescriptor` item:</span></span>

```xml
<ItemGroup>
  <BlazorLinkerDescriptor Include="Linker.xml" />
</ItemGroup>
```
