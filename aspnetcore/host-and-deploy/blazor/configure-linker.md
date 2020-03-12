---
title: Configurez l’éditeur de liens pour ASP.NET Core Blazor
author: guardrex
description: Découvrez comment contrôler l’éditeur de liens en langage intermédiaire (IL) lors de la génération d’une application Blazor.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/10/2020
no-loc:
- Blazor
- SignalR
uid: host-and-deploy/blazor/configure-linker
ms.openlocfilehash: b08ec26fb8d139223c57774600bc3cb19a56ac49
ms.sourcegitcommit: 98bcf5fe210931e3eb70f82fd675d8679b33f5d6
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/11/2020
ms.locfileid: "79083290"
---
# <a name="configure-the-linker-for-aspnet-core-blazor"></a><span data-ttu-id="26abf-103">Configurer l’éditeur de liens pour ASP.NET Core Blazor</span><span class="sxs-lookup"><span data-stu-id="26abf-103">Configure the Linker for ASP.NET Core Blazor</span></span>

<span data-ttu-id="26abf-104">Par [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="26abf-104">By [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

<span data-ttu-id="26abf-105">Le webassembly éblouissant effectue une liaison [il (Intermediate Language)](/dotnet/standard/managed-code#intermediate-language--execution) au cours d’une génération pour supprimer l’il inutile des assemblys de sortie de l’application.</span><span class="sxs-lookup"><span data-stu-id="26abf-105">Blazor WebAssembly performs [Intermediate Language (IL)](/dotnet/standard/managed-code#intermediate-language--execution) linking during a build to trim unnecessary IL from the app's output assemblies.</span></span> <span data-ttu-id="26abf-106">L’éditeur de liens est désactivé lors de la génération dans la configuration Debug.</span><span class="sxs-lookup"><span data-stu-id="26abf-106">The linker is disabled when building in Debug configuration.</span></span> <span data-ttu-id="26abf-107">Les applications doivent être générées dans la configuration Release pour activer l’éditeur de liens.</span><span class="sxs-lookup"><span data-stu-id="26abf-107">Apps must build in Release configuration to enable the linker.</span></span> <span data-ttu-id="26abf-108">Nous vous recommandons de créer la version finale lors du déploiement de vos applications de webassembly éblouissantes.</span><span class="sxs-lookup"><span data-stu-id="26abf-108">We recommend building in Release when deploying your Blazor WebAssembly apps.</span></span> 

<span data-ttu-id="26abf-109">La liaison d’une application optimise sa taille, mais peut avoir des effets néfastes.</span><span class="sxs-lookup"><span data-stu-id="26abf-109">Linking an app optimizes for size but may have detrimental effects.</span></span> <span data-ttu-id="26abf-110">Les applications qui utilisent la réflexion ou les fonctionnalités dynamiques associées peuvent s’arrêter en cas de troncation, car l’éditeur de liens ne connaît pas ce comportement dynamique et ne peut pas déterminer en général les types requis pour la réflexion au moment de l’exécution.</span><span class="sxs-lookup"><span data-stu-id="26abf-110">Apps that use reflection or related dynamic features may break when trimmed because the linker doesn't know about this dynamic behavior and can't determine in general which types are required for reflection at runtime.</span></span> <span data-ttu-id="26abf-111">Pour supprimer de telles applications, l’éditeur de liens doit être informé des types requis par la réflexion dans le code et dans les packages ou infrastructures dont dépend l’application.</span><span class="sxs-lookup"><span data-stu-id="26abf-111">To trim such apps, the linker must be informed about any types required by reflection in the code and in packages or frameworks that the app depends on.</span></span> 

<span data-ttu-id="26abf-112">Pour garantir le bon fonctionnement de l’application tronquée une fois déployée, il est important de tester fréquemment les versions release de l’application lors du développement.</span><span class="sxs-lookup"><span data-stu-id="26abf-112">To ensure the trimmed app works correctly once deployed, it's important to test Release builds of the app frequently while developing.</span></span>

<span data-ttu-id="26abf-113">La liaison des applications éblouissants peut être configurée à l’aide de ces fonctionnalités MSBuild :</span><span class="sxs-lookup"><span data-stu-id="26abf-113">Linking for Blazor apps can be configured using these MSBuild features:</span></span>

* <span data-ttu-id="26abf-114">Configurez la liaison globale avec une [propriété MSBuild](#control-linking-with-an-msbuild-property).</span><span class="sxs-lookup"><span data-stu-id="26abf-114">Configure linking globally with a [MSBuild property](#control-linking-with-an-msbuild-property).</span></span>
* <span data-ttu-id="26abf-115">Contrôler la liaison pour chaque assembly avec un [fichier de configuration](#control-linking-with-a-configuration-file).</span><span class="sxs-lookup"><span data-stu-id="26abf-115">Control linking on a per-assembly basis with a [configuration file](#control-linking-with-a-configuration-file).</span></span>

## <a name="control-linking-with-an-msbuild-property"></a><span data-ttu-id="26abf-116">Liaison de contrôle avec une propriété MSBuild</span><span class="sxs-lookup"><span data-stu-id="26abf-116">Control linking with an MSBuild property</span></span>

<span data-ttu-id="26abf-117">La liaison est activée lorsqu’une application est générée dans `Release` la connexion.</span><span class="sxs-lookup"><span data-stu-id="26abf-117">Linking is enabled when an app is built in `Release` configuation.</span></span> <span data-ttu-id="26abf-118">Pour modifier cela, configurez la propriété `BlazorWebAssemblyEnableLinking` MSBuild dans le fichier projet :</span><span class="sxs-lookup"><span data-stu-id="26abf-118">To change this, configure the `BlazorWebAssemblyEnableLinking` MSBuild property in the project file:</span></span>

```xml
<PropertyGroup>
  <BlazorWebAssemblyEnableLinking>false</BlazorWebAssemblyEnableLinking>
</PropertyGroup>
```

## <a name="control-linking-with-a-configuration-file"></a><span data-ttu-id="26abf-119">Contrôler la liaison avec un fichier de configuration</span><span class="sxs-lookup"><span data-stu-id="26abf-119">Control linking with a configuration file</span></span>

<span data-ttu-id="26abf-120">Contrôlez la liaison pour chaque assembly en fournissant un fichier de configuration XML et en spécifiant le fichier en tant qu’élément MSBuild dans le fichier projet :</span><span class="sxs-lookup"><span data-stu-id="26abf-120">Control linking on a per-assembly basis by providing an XML configuration file and specifying the file as a MSBuild item in the project file:</span></span>

```xml
<ItemGroup>
  <BlazorLinkerDescriptor Include="Linker.xml" />
</ItemGroup>
```

<span data-ttu-id="26abf-121">*Linker.xml* :</span><span class="sxs-lookup"><span data-stu-id="26abf-121">*Linker.xml*:</span></span>

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
      Fixes: https://github.com/dotnet/blazor/issues/239
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

<span data-ttu-id="26abf-122">Pour plus d’informations, consultez l' [éditeur de liens il : syntaxe du descripteur XML](https://github.com/mono/linker/blob/master/src/linker/README.md#syntax-of-xml-descriptor).</span><span class="sxs-lookup"><span data-stu-id="26abf-122">For more information, see [IL Linker: Syntax of xml descriptor](https://github.com/mono/linker/blob/master/src/linker/README.md#syntax-of-xml-descriptor).</span></span>

### <a name="configure-the-linker-for-internationalization"></a><span data-ttu-id="26abf-123">Configurer l’éditeur de liens pour l’internationalisation</span><span class="sxs-lookup"><span data-stu-id="26abf-123">Configure the linker for internationalization</span></span>

<span data-ttu-id="26abf-124">Par défaut, la configuration de l’éditeur de liens de éblouissant pour les applications de webassembly éblouissantes supprime les informations d’internationalisation, à l’exception des paramètres régionaux demandés explicitement.</span><span class="sxs-lookup"><span data-stu-id="26abf-124">By default, Blazor's linker configuration for Blazor WebAssembly apps strips out internationalization information except for locales explicitly requested.</span></span> <span data-ttu-id="26abf-125">La suppression de ces assemblys réduit la taille de l’application.</span><span class="sxs-lookup"><span data-stu-id="26abf-125">Removing these assemblies minimizes the app's size.</span></span>

<span data-ttu-id="26abf-126">Pour contrôler les assemblys I18N qui sont conservés, définissez la `<MonoLinkerI18NAssemblies>` propriété MSBuild dans le fichier projet :</span><span class="sxs-lookup"><span data-stu-id="26abf-126">To control which I18N assemblies are retained, set the `<MonoLinkerI18NAssemblies>` MSBuild property in the project file:</span></span>

```xml
<PropertyGroup>
  <MonoLinkerI18NAssemblies>{all|none|REGION1,REGION2,...}</MonoLinkerI18NAssemblies>
</PropertyGroup>
```

| <span data-ttu-id="26abf-127">Valeur de la région</span><span class="sxs-lookup"><span data-stu-id="26abf-127">Region Value</span></span>     | <span data-ttu-id="26abf-128">Assembly de région mono</span><span class="sxs-lookup"><span data-stu-id="26abf-128">Mono region assembly</span></span>    |
| ---------------- | ----------------------- |
| `all`            | <span data-ttu-id="26abf-129">Tous les assemblys inclus</span><span class="sxs-lookup"><span data-stu-id="26abf-129">All assemblies included</span></span> |
| `cjk`            | <span data-ttu-id="26abf-130">*I18n. CJK. dll*</span><span class="sxs-lookup"><span data-stu-id="26abf-130">*I18N.CJK.dll*</span></span>          |
| `mideast`        | <span data-ttu-id="26abf-131">*I18n. MidEast. dll*</span><span class="sxs-lookup"><span data-stu-id="26abf-131">*I18N.MidEast.dll*</span></span>      |
| <span data-ttu-id="26abf-132">`none` (par défaut)</span><span class="sxs-lookup"><span data-stu-id="26abf-132">`none` (default)</span></span> | <span data-ttu-id="26abf-133">None</span><span class="sxs-lookup"><span data-stu-id="26abf-133">None</span></span>                    |
| `other`          | <span data-ttu-id="26abf-134">*I18n. Other. dll*</span><span class="sxs-lookup"><span data-stu-id="26abf-134">*I18N.Other.dll*</span></span>        |
| `rare`           | <span data-ttu-id="26abf-135">*I18n. Rare. dll*</span><span class="sxs-lookup"><span data-stu-id="26abf-135">*I18N.Rare.dll*</span></span>         |
| `west`           | <span data-ttu-id="26abf-136">*I18n. West. dll*</span><span class="sxs-lookup"><span data-stu-id="26abf-136">*I18N.West.dll*</span></span>         |

<span data-ttu-id="26abf-137">Utilisez une virgule pour séparer plusieurs valeurs (par exemple, `mideast,west`).</span><span class="sxs-lookup"><span data-stu-id="26abf-137">Use a comma to separate multiple values (for example, `mideast,west`).</span></span>

<span data-ttu-id="26abf-138">Pour plus d’informations, consultez [i18n : Pnetlib internationalisation Framework Library (référentiel mono/mono GitHub)](https://github.com/mono/mono/tree/master/mcs/class/I18N).</span><span class="sxs-lookup"><span data-stu-id="26abf-138">For more information, see [I18N: Pnetlib Internationalization Framework Library (mono/mono GitHub repository)](https://github.com/mono/mono/tree/master/mcs/class/I18N).</span></span>
