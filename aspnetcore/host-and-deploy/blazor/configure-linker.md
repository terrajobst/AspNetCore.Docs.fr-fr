---
title: Configurez l’éditeur de liens pour ASP.NET Core Blazor
author: guardrex
description: Découvrez comment contrôler l’éditeur de liens en langage intermédiaire (IL) lors de la génération d’une application Blazor.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 11/21/2019
no-loc:
- Blazor
uid: host-and-deploy/blazor/configure-linker
ms.openlocfilehash: 0bc987d72d2f684b1ecbd4a883e9a09fac7c801e
ms.sourcegitcommit: 3e503ef510008e77be6dd82ee79213c9f7b97607
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/22/2019
ms.locfileid: "74317291"
---
# <a name="configure-the-linker-for-aspnet-core-opno-locblazor"></a><span data-ttu-id="2a4dd-103">Configurez l’éditeur de liens pour ASP.NET Core Blazor</span><span class="sxs-lookup"><span data-stu-id="2a4dd-103">Configure the Linker for ASP.NET Core Blazor</span></span>

<span data-ttu-id="2a4dd-104">Par [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="2a4dd-104">By [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

Blazor<span data-ttu-id="2a4dd-105"> effectue une liaison [il (Intermediate Language)](/dotnet/standard/managed-code#intermediate-language--execution) au cours d’une génération pour supprimer l’il inutile des assemblys de sortie de l’application.</span><span class="sxs-lookup"><span data-stu-id="2a4dd-105"> performs [Intermediate Language (IL)](/dotnet/standard/managed-code#intermediate-language--execution) linking during a build to remove unnecessary IL from the app's output assemblies.</span></span>

<span data-ttu-id="2a4dd-106">Contrôlez la liaison d’assembly avec l’une des approches suivantes :</span><span class="sxs-lookup"><span data-stu-id="2a4dd-106">Control assembly linking using either of the following approaches:</span></span>

* <span data-ttu-id="2a4dd-107">Désactiver la liaison globalement avec une [propriété MSBuild](#disable-linking-with-a-msbuild-property).</span><span class="sxs-lookup"><span data-stu-id="2a4dd-107">Disable linking globally with a [MSBuild property](#disable-linking-with-a-msbuild-property).</span></span>
* <span data-ttu-id="2a4dd-108">Contrôler la liaison pour chaque assembly avec un [fichier de configuration](#control-linking-with-a-configuration-file).</span><span class="sxs-lookup"><span data-stu-id="2a4dd-108">Control linking on a per-assembly basis with a [configuration file](#control-linking-with-a-configuration-file).</span></span>

## <a name="disable-linking-with-a-msbuild-property"></a><span data-ttu-id="2a4dd-109">Désactiver la liaison avec une propriété MSBuild</span><span class="sxs-lookup"><span data-stu-id="2a4dd-109">Disable linking with a MSBuild property</span></span>

<span data-ttu-id="2a4dd-110">La liaison est activée par défaut lors de la génération d’une application, ce qui comprend la publication.</span><span class="sxs-lookup"><span data-stu-id="2a4dd-110">Linking is enabled by default when an app is built, which includes publishing.</span></span> <span data-ttu-id="2a4dd-111">Pour désactiver la liaison pour tous les assemblys, définissez la propriété MSBuild `BlazorLinkOnBuild` sur `false` dans le fichier projet :</span><span class="sxs-lookup"><span data-stu-id="2a4dd-111">To disable linking for all assemblies, set the `BlazorLinkOnBuild` MSBuild property to `false` in the project file:</span></span>

```xml
<PropertyGroup>
  <BlazorLinkOnBuild>false</BlazorLinkOnBuild>
</PropertyGroup>
```

## <a name="control-linking-with-a-configuration-file"></a><span data-ttu-id="2a4dd-112">Contrôler la liaison avec un fichier de configuration</span><span class="sxs-lookup"><span data-stu-id="2a4dd-112">Control linking with a configuration file</span></span>

<span data-ttu-id="2a4dd-113">Contrôlez la liaison pour chaque assembly en fournissant un fichier de configuration XML et en spécifiant le fichier en tant qu’élément MSBuild dans le fichier projet :</span><span class="sxs-lookup"><span data-stu-id="2a4dd-113">Control linking on a per-assembly basis by providing an XML configuration file and specifying the file as a MSBuild item in the project file:</span></span>

```xml
<ItemGroup>
  <BlazorLinkerDescriptor Include="Linker.xml" />
</ItemGroup>
```

<span data-ttu-id="2a4dd-114">*Linker.xml* :</span><span class="sxs-lookup"><span data-stu-id="2a4dd-114">*Linker.xml*:</span></span>

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

<span data-ttu-id="2a4dd-115">Pour plus d’informations, consultez l' [éditeur de liens il : syntaxe du descripteur XML](https://github.com/mono/linker/blob/master/src/linker/README.md#syntax-of-xml-descriptor).</span><span class="sxs-lookup"><span data-stu-id="2a4dd-115">For more information, see [IL Linker: Syntax of xml descriptor](https://github.com/mono/linker/blob/master/src/linker/README.md#syntax-of-xml-descriptor).</span></span>

### <a name="configure-the-linker-for-internationalization"></a><span data-ttu-id="2a4dd-116">Configurer l’éditeur de liens pour l’internationalisation</span><span class="sxs-lookup"><span data-stu-id="2a4dd-116">Configure the linker for internationalization</span></span>

<span data-ttu-id="2a4dd-117">Par défaut, la configuration de l’éditeur de liens de Blazorpour les applications webassembly Blazor supprime les informations d’internationalisation, à l’exception des paramètres régionaux demandés explicitement.</span><span class="sxs-lookup"><span data-stu-id="2a4dd-117">By default, Blazor's linker configuration for Blazor WebAssembly apps strips out internationalization information except for locales explicitly requested.</span></span> <span data-ttu-id="2a4dd-118">La suppression de ces assemblys réduit la taille de l’application.</span><span class="sxs-lookup"><span data-stu-id="2a4dd-118">Removing these assemblies minimizes the app's size.</span></span>

<span data-ttu-id="2a4dd-119">Pour contrôler les assemblys I18N qui sont conservés, définissez la `<MonoLinkerI18NAssemblies>` propriété MSBuild dans le fichier projet :</span><span class="sxs-lookup"><span data-stu-id="2a4dd-119">To control which I18N assemblies are retained, set the `<MonoLinkerI18NAssemblies>` MSBuild property in the project file:</span></span>

```xml
<PropertyGroup>
  <MonoLinkerI18NAssemblies>{all|none|REGION1,REGION2,...}</MonoLinkerI18NAssemblies>
</PropertyGroup>
```

| <span data-ttu-id="2a4dd-120">Valeur de la région</span><span class="sxs-lookup"><span data-stu-id="2a4dd-120">Region Value</span></span>     | <span data-ttu-id="2a4dd-121">Assembly de région mono</span><span class="sxs-lookup"><span data-stu-id="2a4dd-121">Mono region assembly</span></span>    |
| ---------------- | ----------------------- |
| `all`            | <span data-ttu-id="2a4dd-122">Tous les assemblys inclus</span><span class="sxs-lookup"><span data-stu-id="2a4dd-122">All assemblies included</span></span> |
| `cjk`            | <span data-ttu-id="2a4dd-123">*I18n. CJK. dll*</span><span class="sxs-lookup"><span data-stu-id="2a4dd-123">*I18N.CJK.dll*</span></span>          |
| `mideast`        | <span data-ttu-id="2a4dd-124">*I18n. MidEast. dll*</span><span class="sxs-lookup"><span data-stu-id="2a4dd-124">*I18N.MidEast.dll*</span></span>      |
| <span data-ttu-id="2a4dd-125">`none` (valeur par défaut)</span><span class="sxs-lookup"><span data-stu-id="2a4dd-125">`none` (default)</span></span> | <span data-ttu-id="2a4dd-126">aucune.</span><span class="sxs-lookup"><span data-stu-id="2a4dd-126">None</span></span>                    |
| `other`          | <span data-ttu-id="2a4dd-127">*I18n. Other. dll*</span><span class="sxs-lookup"><span data-stu-id="2a4dd-127">*I18N.Other.dll*</span></span>        |
| `rare`           | <span data-ttu-id="2a4dd-128">*I18n. Rare. dll*</span><span class="sxs-lookup"><span data-stu-id="2a4dd-128">*I18N.Rare.dll*</span></span>         |
| `west`           | <span data-ttu-id="2a4dd-129">*I18n. West. dll*</span><span class="sxs-lookup"><span data-stu-id="2a4dd-129">*I18N.West.dll*</span></span>         |

<span data-ttu-id="2a4dd-130">Utilisez une virgule pour séparer plusieurs valeurs (par exemple, `mideast,west`).</span><span class="sxs-lookup"><span data-stu-id="2a4dd-130">Use a comma to separate multiple values (for example, `mideast,west`).</span></span>

<span data-ttu-id="2a4dd-131">Pour plus d’informations, consultez [i18n : Pnetlib internationalisation Framework bibliothèque (dépôt mono/mono GitHub)](https://github.com/mono/mono/tree/master/mcs/class/I18N).</span><span class="sxs-lookup"><span data-stu-id="2a4dd-131">For more information, see [I18N: Pnetlib Internationalization Framework Libary (mono/mono GitHub repository)](https://github.com/mono/mono/tree/master/mcs/class/I18N).</span></span>
