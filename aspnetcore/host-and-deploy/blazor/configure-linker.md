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
ms.openlocfilehash: cdf506f0c0fa720df64e59342d352ef41271d24b
ms.sourcegitcommit: 7dfe6cc8408ac6a4549c29ca57b0c67ec4baa8de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/10/2020
ms.locfileid: "75866044"
---
# <a name="configure-the-linker-for-aspnet-core-opno-locblazor"></a>Configurez l’éditeur de liens pour ASP.NET Core Blazor

Par [Luke Latham](https://github.com/guardrex)

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

Blazor effectue une liaison [il (Intermediate Language)](/dotnet/standard/managed-code#intermediate-language--execution) au cours d’une génération pour supprimer l’il inutile des assemblys de sortie de l’application.

Contrôlez la liaison d’assembly avec l’une des approches suivantes :

* Désactiver la liaison globalement avec une [propriété MSBuild](#disable-linking-with-a-msbuild-property).
* Contrôler la liaison pour chaque assembly avec un [fichier de configuration](#control-linking-with-a-configuration-file).

## <a name="disable-linking-with-a-msbuild-property"></a>Désactiver la liaison avec une propriété MSBuild

La liaison est activée par défaut lors de la génération d’une application, ce qui comprend la publication. Pour désactiver la liaison pour tous les assemblys, définissez la propriété MSBuild `BlazorLinkOnBuild` sur `false` dans le fichier projet :

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

Pour plus d’informations, consultez l' [éditeur de liens il : syntaxe du descripteur XML](https://github.com/mono/linker/blob/master/src/linker/README.md#syntax-of-xml-descriptor).

### <a name="configure-the-linker-for-internationalization"></a>Configurer l’éditeur de liens pour l’internationalisation

Par défaut, la configuration de l’éditeur de liens de Blazorpour les applications webassembly Blazor supprime les informations d’internationalisation, à l’exception des paramètres régionaux demandés explicitement. La suppression de ces assemblys réduit la taille de l’application.

Pour contrôler les assemblys I18N qui sont conservés, définissez la `<MonoLinkerI18NAssemblies>` propriété MSBuild dans le fichier projet :

```xml
<PropertyGroup>
  <MonoLinkerI18NAssemblies>{all|none|REGION1,REGION2,...}</MonoLinkerI18NAssemblies>
</PropertyGroup>
```

| Valeur de la région     | Assembly de région mono    |
| ---------------- | ----------------------- |
| `all`            | Tous les assemblys inclus |
| `cjk`            | *I18n. CJK. dll*          |
| `mideast`        | *I18n. MidEast. dll*      |
| `none` (par défaut) | Aucun                    |
| `other`          | *I18n. Other. dll*        |
| `rare`           | *I18n. Rare. dll*         |
| `west`           | *I18n. West. dll*         |

Utilisez une virgule pour séparer plusieurs valeurs (par exemple, `mideast,west`).

Pour plus d’informations, consultez [i18n : Pnetlib internationalisation Framework bibliothèque (dépôt mono/mono GitHub)](https://github.com/mono/mono/tree/master/mcs/class/I18N).
