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
# <a name="configure-the-linker-for-aspnet-core-blazor"></a>Configurer l’éditeur de liens pour ASP.NET Core Blazor

Par [Luke Latham](https://github.com/guardrex)

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

Le webassembly éblouissant effectue une liaison [il (Intermediate Language)](/dotnet/standard/managed-code#intermediate-language--execution) au cours d’une génération pour supprimer l’il inutile des assemblys de sortie de l’application. L’éditeur de liens est désactivé lors de la génération dans la configuration Debug. Les applications doivent être générées dans la configuration Release pour activer l’éditeur de liens. Nous vous recommandons de créer la version finale lors du déploiement de vos applications de webassembly éblouissantes. 

La liaison d’une application optimise sa taille, mais peut avoir des effets néfastes. Les applications qui utilisent la réflexion ou les fonctionnalités dynamiques associées peuvent s’arrêter en cas de troncation, car l’éditeur de liens ne connaît pas ce comportement dynamique et ne peut pas déterminer en général les types requis pour la réflexion au moment de l’exécution. Pour supprimer de telles applications, l’éditeur de liens doit être informé des types requis par la réflexion dans le code et dans les packages ou infrastructures dont dépend l’application. 

Pour garantir le bon fonctionnement de l’application tronquée une fois déployée, il est important de tester fréquemment les versions release de l’application lors du développement.

La liaison des applications éblouissants peut être configurée à l’aide de ces fonctionnalités MSBuild :

* Configurez la liaison globale avec une [propriété MSBuild](#control-linking-with-an-msbuild-property).
* Contrôler la liaison pour chaque assembly avec un [fichier de configuration](#control-linking-with-a-configuration-file).

## <a name="control-linking-with-an-msbuild-property"></a>Liaison de contrôle avec une propriété MSBuild

La liaison est activée lorsqu’une application est générée dans `Release` la connexion. Pour modifier cela, configurez la propriété `BlazorWebAssemblyEnableLinking` MSBuild dans le fichier projet :

```xml
<PropertyGroup>
  <BlazorWebAssemblyEnableLinking>false</BlazorWebAssemblyEnableLinking>
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

Par défaut, la configuration de l’éditeur de liens de éblouissant pour les applications de webassembly éblouissantes supprime les informations d’internationalisation, à l’exception des paramètres régionaux demandés explicitement. La suppression de ces assemblys réduit la taille de l’application.

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
| `none` (par défaut) | None                    |
| `other`          | *I18n. Other. dll*        |
| `rare`           | *I18n. Rare. dll*         |
| `west`           | *I18n. West. dll*         |

Utilisez une virgule pour séparer plusieurs valeurs (par exemple, `mideast,west`).

Pour plus d’informations, consultez [i18n : Pnetlib internationalisation Framework Library (référentiel mono/mono GitHub)](https://github.com/mono/mono/tree/master/mcs/class/I18N).
