---
title: Configurer l’éditeur de liens pour Blazor
author: guardrex
description: Découvrez comment contrôler l’éditeur de liens de langage intermédiaire (IL) lors de la création d’une application Blazor.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 04/24/2019
uid: host-and-deploy/blazor/configure-linker
ms.openlocfilehash: 00676d4311f8955c3c1ef38d31219d62ea9f4a25
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/27/2019
ms.locfileid: "64887774"
---
# <a name="configure-the-linker-for-blazor"></a>Configurer l’éditeur de liens pour Blazor

Par [Luke Latham](https://github.com/guardrex)

Blazor effectue une liaison de [langage intermédiaire (IL)](/dotnet/standard/managed-code#intermediate-language--execution) pendant un build de mise en production pour supprimer un langage intermédiaire inutile des assemblys de sortie de l’application.

Contrôlez la liaison d’assembly avec l’une des approches suivantes :

* Désactiver la liaison globalement avec une [propriété MSBuild](#disable-linking-with-a-msbuild-property).
* Contrôler la liaison pour chaque assembly avec un [fichier de configuration](#control-linking-with-a-configuration-file).

## <a name="disable-linking-with-a-msbuild-property"></a>Désactiver la liaison avec une propriété MSBuild

La liaison est activée par défaut dans le mode de mise en production quand une application est générée, ce qui inclut la publication. Pour désactiver la liaison pour tous les assemblys, définissez la propriété MSBuild `<BlazorLinkOnBuild>` sur `false` dans le fichier projet :

```xml
<PropertyGroup>
  <BlazorLinkOnBuild>false</BlazorLinkOnBuild>
</PropertyGroup>
```

## <a name="control-linking-with-a-configuration-file"></a>Contrôler la liaison avec un fichier de configuration

Contrôlez la liaison pour chaque assembly en fournissant un fichier de configuration XML et en spécifiant le fichier en tant qu’élément MSBuild dans le fichier projet :

```xml
<ItemGroup>
  <BlazorLinkerDescriptor Include="Linker.xml" />
</ItemGroup>
```

*Linker.xml* :

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

Pour plus d’informations, consultez [Éditeur de liens de langage intermédiaire : Syntaxe du descripteur xml](https://github.com/mono/linker/blob/master/src/linker/README.md#syntax-of-xml-descriptor).
