---
title: Configurer l’éditeur de liens pour Blazor
author: guardrex
description: Découvrez comment contrôler l’éditeur de liens de langage intermédiaire (IL) lors de la création d’une application Blazor.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 02/20/2019
uid: host-and-deploy/razor-components/configure-linker
ms.openlocfilehash: 7c53e7912ec3b0ae471ea38777f874f55a32487d
ms.sourcegitcommit: 0945078a09c372f17e9b003758ed87e99c2449f4
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/21/2019
ms.locfileid: "56647939"
---
# <a name="configure-the-linker-for-blazor"></a>Configurer l’éditeur de liens pour Blazor

Par [Luke Latham](https://github.com/guardrex)

[!INCLUDE[](~/includes/razor-components-preview-notice.md)]

Blazor effectue la liaison de [langage intermédiaire (IL)](/dotnet/standard/managed-code#intermediate-language--execution) lors de chaque génération afin de supprimer tout langage intermédiaire inutile des assemblys de sortie.

Vous pouvez contrôler la liaison d’assembly avec l’une des approches suivantes :

* Désactiver la liaison globalement avec une propriété MSBuild.
* Contrôler la liaison pour chaque assembly avec un fichier de configuration.

## <a name="disable-linking-with-an-msbuild-property"></a>Désactiver la liaison avec une propriété MSBuild

La liaison est activée par défaut dans le mode de mise en production quand une application est générée, ce qui inclut la publication. Pour désactiver la liaison pour tous les assemblys, définissez la propriété MSBuild `<BlazorLinkOnBuild>` sur `false` dans le fichier projet :

```xml
<PropertyGroup>
  <BlazorLinkOnBuild>false</BlazorLinkOnBuild>
</PropertyGroup>
```

## <a name="control-linking-with-a-configuration-file"></a>Contrôler la liaison avec un fichier de configuration

La liaison peut être contrôlée pour chaque assembly en fournissant un fichier de configuration XML et en spécifiant le fichier en tant qu’élément MSBuild dans le fichier projet.

Voici un exemple de fichier de configuration (*Linker.xml*) :

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

Pour en savoir plus sur le format de fichier pour le fichier de configuration, consultez [Éditeur de liens de langage intermédiaire : Syntaxe du descripteur xml](https://github.com/mono/linker/blob/master/src/linker/README.md#syntax-of-xml-descriptor).

Spécifiez le fichier de configuration dans le fichier projet avec l’élément `BlazorLinkerDescriptor` :

```xml
<ItemGroup>
  <BlazorLinkerDescriptor Include="Linker.xml" />
</ItemGroup>
```
