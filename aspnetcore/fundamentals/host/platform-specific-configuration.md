---
title: Améliorer une application à partir d’un assembly externe dans ASP.NET Core avec IHostingStartup
author: guardrex
description: Découvrez comment améliorer une application ASP.NET Core à partir d’un assembly externe à l’aide d’une implémentation IHostingStartup.
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.custom: mvc
ms.date: 08/13/2018
uid: fundamentals/configuration/platform-specific-configuration
ms.openlocfilehash: 2eddfa03b28564fcca7cc098e353b05e23b7c6f6
ms.sourcegitcommit: a669c4e3f42e387e214a354ac4143555602e6f66
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2018
ms.locfileid: "43336272"
---
# <a name="enhance-an-app-from-an-external-assembly-in-aspnet-core-with-ihostingstartup"></a>Améliorer une application à partir d’un assembly externe dans ASP.NET Core avec IHostingStartup

Par [Luke Latham](https://github.com/guardrex)

Une implémentation [IHostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup) (hébergement au démarrage) ajoute des améliorations à une application au démarrage à partir d’un assembly externe. Par exemple, une bibliothèque externe peut utiliser une implémentation d’hébergement au démarrage pour fournir des fournisseurs ou services de configuration supplémentaires à une application. `IHostingStartup` *est disponible dans ASP.NET Core 2.0 ou versions ultérieures.*

[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/platform-specific-configuration/samples/) ([procédure de téléchargement](xref:tutorials/index#how-to-download-a-sample))

## <a name="hostingstartup-attribute"></a>Attribut HostingStartup

Un attribut [HostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.hostingstartupattribute) indique la présence d’un assembly d’hébergement au démarrage à activer au moment de l’exécution.

L’assembly d’entrée ou l’assembly contenant la classe `Startup` est automatiquement analysé pour détecter l’attribut `HostingStartup`. La liste des assemblys où les attributs `HostingStartup` doivent être recherchés est chargée au moment de l’exécution à partir de la configuration spécifiée dans [WebHostDefaults.HostingStartupAssembliesKey](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults.hostingstartupassemblieskey). La liste des assemblys à exclure de cette détection est chargée de [WebHostDefaults.HostingStartupExcludeAssembliesKey](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults.hostingstartupexcludeassemblieskey). Pour plus d’informations, consultez [Hôte web : Assemblys d’hébergement au démarrage](xref:fundamentals/host/web-host#hosting-startup-assemblies) et [Hôte web : Assemblys exclus de l’hébergement au démarrage](xref:fundamentals/host/web-host#hosting-startup-exclude-assemblies).

Dans l’exemple suivant, l’espace de noms de l’assembly d’hébergement au démarrage est `StartupEnhancement`. La classe contenant le code d’hébergement au démarrage est `StartupEnhancementHostingStartup` :

[!code-csharp[](platform-specific-configuration/samples-snapshot/2.x/StartupEnhancement.cs?name=snippet1)]

L’attribut `HostingStartup` se trouve généralement dans le fichier de classe d’implémentation `IHostingStartup` de l’assembly d’hébergement au démarrage.

## <a name="discover-loaded-hosting-startup-assemblies"></a>Découvrir les assemblys d’hébergement au démarrage chargés

Pour détecter les assemblys d’hébergement au démarrage chargés, activez la journalisation et analysez les journaux de l’application. Les erreurs qui se produisent durant le chargement des assemblys sont journalisées. Les assemblys d’hébergement au démarrage chargés sont journalisés au niveau Débogage, et toutes les erreurs sont journalisées.

## <a name="disable-automatic-loading-of-hosting-startup-assemblies"></a>Désactiver le chargement automatique des assemblys d’hébergement au démarrage

::: moniker range=">= aspnetcore-2.1"

Pour désactiver le chargement automatique des assemblys d’hébergement au démarrage, choisissez l’une des approches suivantes :

* Pour bloquer le chargement de tous les assemblys d’hébergement au démarrage, définissez l’une des valeurs suivantes sur `true` ou `1` :
  * Le paramètre de configuration d’hôte [PreventHostingStartup](xref:fundamentals/host/web-host#prevent-hosting-startup).
  * La variable d’environnement `ASPNETCORE_PREVENTHOSTINGSTARTUP`.
* Pour bloquer le chargement de certains assemblys d’hébergement au démarrage, définissez l’une des valeurs suivantes sous la forme d’une liste délimitée par des points-virgules contenant les assemblys d’hébergement au démarrage à exclure au moment du démarrage :
  * Le paramètre de configuration d’hôte [HostingStartupExcludeAssemblies](xref:fundamentals/host/web-host#hosting-startup-exclude-assemblies).
  * La variable d’environnement `ASPNETCORE_HOSTINGSTARTUPEXCLUDEASSEMBLIES`.

::: moniker-end

::: moniker range="= aspnetcore-2.0"

Pour désactiver le chargement automatique des assemblys d’hébergement au démarrage, définissez l’une des valeurs suivantes sur `true` ou `1` :

* Le paramètre de configuration d’hôte [PreventHostingStartup](xref:fundamentals/host/web-host#prevent-hosting-startup).
* La variable d’environnement `ASPNETCORE_PREVENTHOSTINGSTARTUP`.

::: moniker-end

Si le paramètre de configuration d’hôte et la variable d’environnement sont définis tous les deux, c’est le paramètre d’hôte qui détermine le comportement.

La désactivation des assemblys d’hébergement au démarrage à l’aide du paramètre d’hôte ou de la variable d’environnement désactive l’assembly globalement et peut donc désactiver plusieurs caractéristiques d’une application.

## <a name="project"></a>Projet

Créez un hébergement au démarrage avec un des types de projet suivants :

* [Bibliothèque de classes](#class-library)
* [Application console sans point d’entrée](#console-app-without-an-entry-point)

### <a name="class-library"></a>Bibliothèque de classes

Une amélioration de l’hébergement au démarrage peut être fournie dans une bibliothèque de classes. La bibliothèque contient un attribut `HostingStartup`.

[L’exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/platform-specific-configuration/samples/) inclut une application Razor Pages (*HostingStartupApp*) et une bibliothèque de classes (*HostingStartupLibrary*). La bibliothèque de classes :

* Contient une classe d’hébergement au démarrage, `ServiceKeyInjection`, qui implémente `IHostingStartup`. `ServiceKeyInjection` ajoute une paire de chaînes de service à la configuration de l’application par le biais du fournisseur de configuration en mémoire ([AddInMemoryCollection](/dotnet/api/microsoft.extensions.configuration.memoryconfigurationbuilderextensions.addinmemorycollection)).
* Inclut un attribut `HostingStartup` qui identifie l’espace de noms et la classe d’hébergement au démarrage.

La méthode [Configure](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup.configure) de la classe `ServiceKeyInjection` utilise un [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder) pour ajouter des améliorations à une application. `IHostingStartup.Configure` dans l’assembly d’hébergement au démarrage est appelé par le runtime avant `Startup.Configure` dans le code utilisateur, ce qui permet au code utilisateur de remplacer la configuration fournie par l’assembly d’hébergement au démarrage.

*HostingStartupLibrary/ServiceKeyInjection.cs* :

[!code-csharp[](platform-specific-configuration/samples/2.x/HostingStartupLibrary/ServiceKeyInjection.cs?name=snippet1)]

La page d’index de l’application lit et affiche les valeurs de configuration des deux clés définies par l’assembly d’hébergement au démarrage de la bibliothèque de classes :

*HostingStartupApp/Pages/Index.cshtml.cs* :

[!code-csharp[](platform-specific-configuration/samples/2.x/HostingStartupApp/Pages/Index.cshtml.cs?name=snippet1&highlight=5-6,11-12)]

[L’exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/platform-specific-configuration/samples/) inclut également un projet de package NuGet qui fournit un hébergement au démarrage distinct, *HostingStartupPackage*. Le package a les mêmes caractéristiques que la bibliothèque de classes décrite précédemment. Le package :

* Contient une classe d’hébergement au démarrage, `ServiceKeyInjection`, qui implémente `IHostingStartup`. `ServiceKeyInjection` ajoute une paire de chaînes de service à la configuration de l’application.
* Inclut un attribut `HostingStartup`.

*HostingStartupPackage/ServiceKeyInjection.cs* :

[!code-csharp[](platform-specific-configuration/samples/2.x/HostingStartupPackage/ServiceKeyInjection.cs?name=snippet1)]

La page d’index de l’application lit et affiche les valeurs de configuration des deux clés définies par l’assembly d’hébergement au démarrage du package :

*HostingStartupApp/Pages/Index.cshtml.cs* :

[!code-csharp[](platform-specific-configuration/samples/2.x/HostingStartupApp/Pages/Index.cshtml.cs?name=snippet1&highlight=7-8,13-14)]

### <a name="console-app-without-an-entry-point"></a>Application console sans point d’entrée

*Cette approche s’applique aux applications .NET Core, mais pas aux applications .NET Framework.*

Une amélioration d’hébergement au démarrage dynamique qui ne nécessite pas de référence au moment de la compilation pour l’activation peut être fournie dans une application console sans point d’entrée. L’application contient un attribut `HostingStartup`. Pour créer un hébergement au démarrage dynamique :

1. Une bibliothèque d’implémentation est créée à partir de la classe contenant l’implémentation `IHostingStartup`. Cette bibliothèque est considérée comme un package standard.
1. Une application console sans point d’entrée référence le package de la bibliothèque d’implémentation. Une application console est utilisée pour les raisons suivantes :
   * Un fichier de dépendances est une ressource d’application exécutable, et donc une bibliothèque ne peut pas fournir un fichier de dépendances.
   * Une bibliothèque ne peut pas être ajoutée directement au [magasin de packages de runtime](/dotnet/core/deploying/runtime-store), qui nécessite un projet exécutable ciblant le runtime partagé.
1. L’application console est publiée pour obtenir les dépendances de l’hébergement au démarrage. La publication de l’application console entraîne la suppression des dépendances inutilisées dans le fichier de dépendances.
1. L’application et le fichier de dépendances associé sont placés dans le magasin de packages de runtime. Pour permettre leur détection, l’assembly d’hébergement au démarrage et le fichier de dépendances associé sont référencés dans une paire de variables d’environnement.

L’application console référence le package [Microsoft.AspNetCore.Hosting.Abstractions](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.Abstractions/) :

[!code-xml[](platform-specific-configuration/samples-snapshot/2.x/StartupEnhancement.csproj)]

Un attribut [HostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.hostingstartupattribute) identifie une classe en tant qu’implémentation de `IHostingStartup` pour le chargement et l’exécution durant la génération de [IWebHost](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost). Dans l’exemple suivant, l’espace de noms est `StartupEnhancement`, et la classe est `StartupEnhancementHostingStartup` :

[!code-csharp[](platform-specific-configuration/samples-snapshot/2.x/StartupEnhancement.cs?name=snippet1)]

Une classe implémente `IHostingStartup`. La méthode [Configure](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup.configure) de la classe utilise un [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder) pour ajouter des améliorations à une application. `IHostingStartup.Configure` dans l’assembly d’hébergement au démarrage est appelé par le runtime avant `Startup.Configure` dans le code utilisateur, ce qui permet au code utilisateur de remplacer la configuration fournie par l’assembly d’hébergement au démarrage.

[!code-csharp[](platform-specific-configuration/samples-snapshot/2.x/StartupEnhancement.cs?name=snippet2&highlight=3,5)]

Durant la génération d’un projet `IHostingStartup`, le fichier de dépendances (*\*. deps.json*) définit le dossier *bin* comme emplacement du `runtime` :

[!code-json[](platform-specific-configuration/samples-snapshot/2.x/StartupEnhancement1.deps.json?range=2-13&highlight=8)]

Seule une partie du fichier est affichée. Le nom de l’assembly dans l’exemple est `StartupEnhancement`.

## <a name="specify-the-hosting-startup-assembly"></a>Spécifier l’assembly d’hébergement au démarrage

Pour l’hébergement au démarrage fourni par une bibliothèque de classes ou une application console, spécifiez le nom de l’assembly d’hébergement au démarrage dans la variable d’environnement `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`. Cette variable est spécifiée sous la forme d’une liste d’assemblys délimitée par des points-virgules.

L’analyse de détection de l’attribut `HostingStartup` porte uniquement sur les assemblys d’hébergement au démarrage. Dans l’exemple d’application (*HostingStartupApp*), pour permettre la détection des hébergements au démarrage décrits précédemment, la variable d’environnement est définie à la valeur suivante :

```
HostingStartupLibrary;HostingStartupPackage;StartupDiagnostics
```

Un assembly d’hébergement au démarrage peut également être défini à l’aide du paramètre de configuration d’hôte [HostingStartupAssemblies](xref:fundamentals/host/web-host#hosting-startup-assemblies).

Quand plusieurs assemblys de démarrage d’hébergement sont présents, leurs méthodes [Configure](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup.configure) sont exécutées dans l’ordre dans lequel ils sont répertoriés.

## <a name="activation"></a>Activation

Les options d’activation de l’hébergement au démarrage sont les suivantes :

* [Magasin de runtime](#runtime-store) &ndash; L’activation ne nécessite pas de référence au moment de la compilation pour l’activation. L’exemple d’application place les fichiers de l’assembly d’hébergement au démarrage et de ses dépendances dans le dossier *deployment* pour faciliter le déploiement de l’hébergement au démarrage dans un environnement multimachine. Le dossier *deployment* inclut également un script PowerShell qui crée ou modifie des variables d’environnement sur le système de déploiement pour activer l’hébergement au démarrage.
* Référence au moment de la compilation requise pour l’activation
  * [Package NuGet](#nuget-package)
  * [Dossier bin du projet](#project-bin-folder)

### <a name="runtime-store"></a>Magasin de runtime

L’implémentation d’hébergement au démarrage est placée dans le [magasin de runtime](/dotnet/core/deploying/runtime-store). L’application améliorée ne nécessite pas de référence à l’assembly au moment de la compilation.

Une fois l’hébergement au démarrage généré, le fichier projet correspondant est utilisé comme fichier manifeste dans la commande [dotnet store](/dotnet/core/tools/dotnet-store).

```console
dotnet store --manifest <PROJECT_FILE> --runtime <RUNTIME_IDENTIFIER>
```

Cette commande place l’assembly d’hébergement au démarrage et d’autres dépendances qui ne font pas partie du framework partagé dans le magasin de runtime du profil utilisateur, à l’emplacement ci-dessous :

# <a name="windowstabwindows"></a>[Windows](#tab/windows)

```
%USERPROFILE%\.dotnet\store\x64\<TARGET_FRAMEWORK_MONIKER>\<ENHANCEMENT_ASSEMBLY_NAME>\<ENHANCEMENT_VERSION>\lib\<TARGET_FRAMEWORK_MONIKER>\
```

# <a name="macostabmacos"></a>[macOS](#tab/macos)

```
/Users/<USER>/.dotnet/store/x64/<TARGET_FRAMEWORK_MONIKER>/<ENHANCEMENT_ASSEMBLY_NAME>/<ENHANCEMENT_VERSION>/lib/<TARGET_FRAMEWORK_MONIKER>/
```

# <a name="linuxtablinux"></a>[Linux](#tab/linux)

```
/Users/<USER>/.dotnet/store/x64/<TARGET_FRAMEWORK_MONIKER>/<ENHANCEMENT_ASSEMBLY_NAME>/<ENHANCEMENT_VERSION>/lib/<TARGET_FRAMEWORK_MONIKER>/
```

---

Si vous souhaitez placer l’assembly et les dépendances à un emplacement permettant une utilisation globale, ajoutez l’option `-o|--output` à la commande `dotnet store` dans le chemin suivant :

# <a name="windowstabwindows"></a>[Windows](#tab/windows)

```
%PROGRAMFILES%\dotnet\store\x64\<TARGET_FRAMEWORK_MONIKER>\<ENHANCEMENT_ASSEMBLY_NAME>\<ENHANCEMENT_VERSION>\lib\<TARGET_FRAMEWORK_MONIKER>\
```

# <a name="macostabmacos"></a>[macOS](#tab/macos)

```
/usr/local/share/dotnet/store/x64/<TARGET_FRAMEWORK_MONIKER>/<ENHANCEMENT_ASSEMBLY_NAME>/<ENHANCEMENT_VERSION>/lib/<TARGET_FRAMEWORK_MONIKER>/
```

# <a name="linuxtablinux"></a>[Linux](#tab/linux)

```
/usr/local/share/dotnet/store/x64/<TARGET_FRAMEWORK_MONIKER>/<ENHANCEMENT_ASSEMBLY_NAME>/<ENHANCEMENT_VERSION>/lib/<TARGET_FRAMEWORK_MONIKER>/
```

---

**Modifier et placer le fichier de dépendances de l’hébergement au démarrage**

L’emplacement du runtime est spécifié dans le fichier *\*.deps.json*. Pour activer l’amélioration, l’élément `runtime` doit spécifier l’emplacement de l’assembly du runtime de l’amélioration. Attribuez à l’emplacement du `runtime` le préfixe `lib/<TARGET_FRAMEWORK_MONIKER>/` :

[!code-json[](platform-specific-configuration/samples-snapshot/2.x/StartupEnhancement2.deps.json?range=2-13&highlight=8)]

Dans l’exemple de code (projet *StartupDiagnostics*), la modification du fichier *\*.deps.json* est effectuée par un script [PowerShell](/powershell/scripting/powershell-scripting). Le script PowerShell est automatiquement déclenché par une cible de build dans le fichier projet.

Le fichier *\*.deps.json* de l’implémentation doit se trouver dans un emplacement accessible.

Pour une utilisation individuelle, placez le fichier dans le dossier *additonalDeps* des paramètres `.dotnet` du profil utilisateur :

# <a name="windowstabwindows"></a>[Windows](#tab/windows)

```
%USERPROFILE%\.dotnet\x64\additionalDeps\<ENHANCEMENT_ASSEMBLY_NAME>\shared\Microsoft.NETCore.App\<SHARED_FRAMEWORK_VERSION>\
```

# <a name="macostabmacos"></a>[macOS](#tab/macos)

```
/Users/<USER>/.dotnet/x64/additionalDeps/<ENHANCEMENT_ASSEMBLY_NAME>/shared/Microsoft.NETCore.App/<SHARED_FRAMEWORK_VERSION>/
```

# <a name="linuxtablinux"></a>[Linux](#tab/linux)

```
/Users/<USER>/.dotnet/x64/additionalDeps/<ENHANCEMENT_ASSEMBLY_NAME>/shared/Microsoft.NETCore.App/<SHARED_FRAMEWORK_VERSION>/
```

---

Pour une utilisation globale, placez le fichier dans le dossier *additonalDeps* de l’installation .NET Core :

# <a name="windowstabwindows"></a>[Windows](#tab/windows)

```
%PROGRAMFILES%\dotnet\additionalDeps\<ENHANCEMENT_ASSEMBLY_NAME>\shared\Microsoft.NETCore.App\<SHARED_FRAMEWORK_VERSION>\
```

# <a name="macostabmacos"></a>[macOS](#tab/macos)

```
/usr/local/share/dotnet/additionalDeps/<ENHANCEMENT_ASSEMBLY_NAME>/shared/Microsoft.NETCore.App/<SHARED_FRAMEWORK_VERSION>/
```

# <a name="linuxtablinux"></a>[Linux](#tab/linux)

```
/usr/local/share/dotnet/additionalDeps/<ENHANCEMENT_ASSEMBLY_NAME>/shared/Microsoft.NETCore.App/<SHARED_FRAMEWORK_VERSION>/
```

---

La version de framework partagé reflète la version du runtime partagé qu’utilise l’application cible. Le runtime partagé est indiqué dans le fichier *\*.runtimeconfig.json*. Dans l’exemple d’application (*HostingStartupApp*), le runtime partagé est spécifié dans le fichier *HostingStartupApp.runtimeconfig.json*.

**Détecter le fichier de dépendances de l’hébergement au démarrage**

L’emplacement du fichier *\*.deps.json* de l’implémentation est indiqué dans la variable d’environnement `DOTNET_ADDITIONAL_DEPS`.

Si le fichier est placé dans le dossier *.dotnet* du profil utilisateur, définissez la variable d’environnement à la valeur ci-dessous :

# <a name="windowstabwindows"></a>[Windows](#tab/windows)

```
%USERPROFILE%\.dotnet\x64\additionalDeps\
```

# <a name="macostabmacos"></a>[macOS](#tab/macos)

```
/Users/<USER>/.dotnet/x64/additionalDeps/
```

# <a name="linuxtablinux"></a>[Linux](#tab/linux)

```
/Users/<USER>/.dotnet/x64/additionalDeps/
```

---

Si le fichier est placé dans l’installation .NET Core pour une utilisation globale, fournissez le chemin complet au fichier :

# <a name="windowstabwindows"></a>[Windows](#tab/windows)

```
%PROGRAMFILES%\dotnet\additionalDeps\<ENHANCEMENT_ASSEMBLY_NAME>\shared\Microsoft.NETCore.App\<SHARED_FRAMEWORK_VERSION>\<ENHANCEMENT_ASSEMBLY_NAME>.deps.json
```

# <a name="macostabmacos"></a>[macOS](#tab/macos)

```
/usr/local/share/dotnet/additionalDeps/<ENHANCEMENT_ASSEMBLY_NAME>/shared/Microsoft.NETCore.App/<SHARED_FRAMEWORK_VERSION>/<ENHANCEMENT_ASSEMBLY_NAME>.deps.json
```

# <a name="linuxtablinux"></a>[Linux](#tab/linux)

```
/usr/local/share/dotnet/additionalDeps/<ENHANCEMENT_ASSEMBLY_NAME>/shared/Microsoft.NETCore.App/<SHARED_FRAMEWORK_VERSION>/<ENHANCEMENT_ASSEMBLY_NAME>.deps.json
```

---

Dans l’exemple d’application (*HostingStartupApp*), pour permettre la détection du fichier de dépendances (*HostingStartupApp.runtimeconfig.json*), ce fichier est placé dans le profil utilisateur.

# <a name="windowstabwindows"></a>[Windows](#tab/windows)

Définissez la variable d’environnement `DOTNET_ADDITIONAL_DEPS` à la valeur suivante :

```
%UserProfile%\.dotnet\x64\additionalDeps\StartupDiagnostics\
```

# <a name="macostabmacos"></a>[macOS](#tab/macos)

Définissez la variable d’environnement `DOTNET_ADDITIONAL_DEPS` à la valeur suivante :

```
/Users/<USER>/.dotnet/x64/additionalDeps/StartupDiagnostics/
```

# <a name="linuxtablinux"></a>[Linux](#tab/linux)

Définissez la variable d’environnement `DOTNET_ADDITIONAL_DEPS` à la valeur suivante :

```
/Users/<USER>/.dotnet/x64/additionalDeps/StartupDiagnostics/
```

---

Pour obtenir des exemples montrant comment définir des variables d’environnement pour différents systèmes d’exploitation, consultez [Utiliser plusieurs environnements](xref:fundamentals/environments).

**Déploiement**

Pour faciliter le déploiement d’un hébergement au démarrage dans un environnement multimachine, l’exemple d’application crée un dossier *deployment* dans la sortie publiée qui contient les éléments suivants :

* L’assembly d’hébergement au démarrage.
* Le fichier des dépendances de l’hébergement au démarrage.
* Un script PowerShell qui crée ou modifie les variables `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES` et `DOTNET_ADDITIONAL_DEPS` pour prendre en charge l’activation de l’hébergement au démarrage. Exécutez ce script à partir d’une invite de commandes PowerShell d’administration sur le système de déploiement.

### <a name="nuget-package"></a>Package NuGet

Une amélioration de l’hébergement au démarrage peut être fournie dans un package NuGet. Le package a un attribut `HostingStartup`. Les types d’hébergement au démarrage fournis par le package sont mis à la disposition de l’application au moyen de l’une des approches suivantes :

* Le fichier projet de l’application améliorée crée une référence de package à l’hébergement au démarrage dans le fichier projet de l’application (référence au moment de la compilation). Une fois la référence au moment de la compilation créée, l’assembly d’hébergement au démarrage et toutes ses dépendances sont ajoutés au fichier de dépendances de l’application (*\*.deps.json*). Cette approche s’applique à un package d’assembly d’hébergement au démarrage qui a été publié sur [nuget.org](https://www.nuget.org/).
* Le fichier de dépendances de l’hébergement au démarrage est mis à la disposition de l’application améliorée de la façon décrite dans la section [Magasin de runtime](#runtime-store) (sans référence au moment de la compilation).

Pour plus d’informations sur les packages NuGet et le magasin de runtime, consultez les rubriques suivantes :

* [Création d’un package NuGet avec les outils multiplateformes](/dotnet/core/deploying/creating-nuget-packages)
* [Publication de packages](/nuget/create-packages/publish-a-package)
* [Magasin de packages de runtime](/dotnet/core/deploying/runtime-store)

### <a name="project-bin-folder"></a>Dossier bin du projet

Une amélioration de l’hébergement au démarrage peut être fournie par un assembly déployé à partir du dossier *bin* dans l’application améliorée. Les types d’hébergement au démarrage fournis par l’assembly sont mis à la disposition de l’application au moyen de l’une des approches suivantes :

* Le fichier projet de l’application améliorée crée une référence d’assembly à l’hébergement au démarrage (référence au moment de la compilation). Une fois la référence au moment de la compilation créée, l’assembly d’hébergement au démarrage et toutes ses dépendances sont ajoutés au fichier de dépendances de l’application (*\*.deps.json*). Cette approche s’applique quand le scénario de déploiement appelle le déplacement de l’assembly de la bibliothèque d’hébergement au démarrage compilée (fichier DLL) vers le projet de consommation, ou vers un emplacement auquel ce projet a accès, et quand une référence au moment de la compilation est créée vers l’assembly d’hébergement au démarrage.
* Le fichier de dépendances de l’hébergement au démarrage est mis à la disposition de l’application améliorée de la façon décrite dans la section [Magasin de runtime](#runtime-store) (sans référence au moment de la compilation).

## <a name="sample-code"></a>Exemple de code

[L’exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/platform-specific-configuration/samples/) ([Comment télécharger un exemple](xref:tutorials/index#how-to-download-a-sample)) montre des scénarios d’implémentation de l’hébergement au démarrage :

* Deux assemblys d’hébergement au démarrage (bibliothèques de classes) définissent chacun une paire clé-valeur de configuration en mémoire :
  * Package NuGet (*HostingStartupPackage*)
  * Bibliothèque de classes (*HostingStartupLibrary*)
* Un hébergement au démarrage est activé à partir d’un assembly déployé depuis le magasin de runtime (*StartupDiagnostics*). L’assembly ajoute deux middleware (intergiciels) à l’application au démarrage, qui fournissent des informations de diagnostic :
  * Services inscrits
  * Adresse (schéma, hôte, chemin de base, chemin, chaîne de requête)
  * Connexion (adresse IP distante, port distant, adresse IP locale, port local, certificat client)
  * En-têtes de requête
  * Variables d’environnement

Pour exécuter l’exemple :

**Activation à partir d’un package NuGet**

1. Compilez le package *HostingStartupPackage* à l’aide de la commande [dotnet pack](/dotnet/core/tools/dotnet-pack).
1. Ajoutez le nom de l’assembly du package *HostingStartupPackage* à la variable d’environnement `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`.
1. Compilez et exécutez l’application. Une référence de package est présente dans l’application améliorée (référence au moment de la compilation). Un `<PropertyGroup>` dans le fichier projet de l’application spécifie la sortie du projet de package (*../HostingStartupPackage/bin/Debug*) comme source de package. L’application peut ainsi utiliser le package sans avoir à le charger sur [nuget.org](https://www.nuget.org/). Pour plus d’informations, consultez les remarques dans le fichier projet de l’application HostingStartupApp.

   ```xml
   <PropertyGroup>
     <RestoreSources>$(RestoreSources);https://api.nuget.org/v3/index.json;../HostingStartupPackage/bin/Debug</RestoreSources>
   </PropertyGroup>
   ```
1. Notez que les valeurs des clés de configuration de service affichées dans la page d’index correspondent aux valeurs définies par la méthode `ServiceKeyInjection.Configure` du package.

Si vous modifiez le projet *HostingStartupPackage* et le recompilez, effacez les caches locaux du package NuGet pour vous assurer que *HostingStartupApp* reçoit le package mis à jour et pas un package périmé du cache local. Pour effacer les caches NuGet locaux, exécutez la commande [dotnet nuget locals](/dotnet/core/tools/dotnet-nuget-locals) suivante :

```console
dotnet nuget locals all --clear
```

**Activation à partir d’une bibliothèque de classes**

1. Compilez la bibliothèque de classes *HostingStartupLibrary* à l’aide de la commande [dotnet build](/dotnet/core/tools/dotnet-build).
1. Ajoutez le nom de l’assembly de la bibliothèque de classes *HostingStartupLibrary* à la variable d’environnement `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`.
1. À partir du dossier *bin*, déployez l’assembly de la bibliothèque de classes dans l’application en copiant le fichier *HostingStartupLibrary.dll* du résultat de la compilation de la bibliothèque de classes dans le dossier *bin/Debug* de l’application.
1. Compilez et exécutez l’application. Dans le fichier projet de l’application, un `<ItemGroup>` référence l’assembly de la bibliothèque de classes (*.\bin\Debug\netcoreapp2.1\HostingStartupLibrary.dll*) (référence au moment de la compilation). Pour plus d’informations, consultez les remarques dans le fichier projet de l’application HostingStartupApp.

   ```xml
   <ItemGroup>
     <Reference Include=".\bin\Debug\netcoreapp2.1\HostingStartupLibrary.dll">
       <HintPath>.\bin\Debug\netcoreapp2.1\HostingStartupLibrary.dll</HintPath>
       <SpecificVersion>False</SpecificVersion>
     </Reference>
   </ItemGroup>
   ```
1. Notez que les valeurs des clés de configuration de service affichées dans la page d’index correspondent aux valeurs définies par la méthode `ServiceKeyInjection.Configure` de la bibliothèque de classes.

**Activation à partir d’un assembly déployé depuis le magasin de runtime**

1. Le projet *StartupDiagnostics* utilise [PowerShell](/powershell/scripting/powershell-scripting) pour modifier le fichier *StartupDiagnostics.deps.json* associé. PowerShell est installé par défaut sur Windows à compter de Windows 7 SP1 et de Windows Server 2008 R2 SP1. Pour obtenir PowerShell sur d’autres plateformes, consultez [Installation de Windows PowerShell](/powershell/scripting/setup/installing-powershell#powershell-core).
1. Compilez le projet *StartupDiagnostics*. Une fois le projet compilé, une build cible dans le fichier projet lance automatiquement les actions suivantes :
   * Déclenche le script PowerShell pour modifier le fichier *StartupDiagnostics.deps.json*.
   * Déplace le fichier *StartupDiagnostics.deps.json* dans le dossier *additionalDeps* du profil utilisateur.
1. Exécutez la commande `dotnet store` à partir d’une invite de commandes dans le répertoire de l’hébergement au démarrage pour stocker l’assembly et ses dépendances dans le magasin de runtime du profil utilisateur :

   ```console
   dotnet store --manifest StartupDiagnostics.csproj --runtime <RID>
   ```
   Sur Windows, la commande utilise [l’identificateur de runtime](/dotnet/core/rid-catalog) `win7-x64`. Si vous fournissez l’hébergement au démarrage pour un autre runtime, spécifiez l’identificateur de runtime approprié.
1. Définissez les variables d’environnement :
   * Ajoutez le nom d’assembly de *StartupDiagnostics* à la variable d’environnement `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`.
   * Sur Windows, définissez la variable d’environnement `DOTNET_ADDITIONAL_DEPS` à `%UserProfile%\.dotnet\x64\additionalDeps\StartupDiagnostics\`. Sur macOS/Linux, définissez la variable d’environnement `DOTNET_ADDITIONAL_DEPS` à `/Users/<USER>/.dotnet/x64/additionalDeps/StartupDiagnostics/`, où `<USER>` est le profil utilisateur contenant l’hébergement au démarrage.
1. Exécutez l’exemple d’application.
1. Demandez le point de terminaison `/services` pour voir les services inscrits de l’application. Demandez le point de terminaison `/diag` pour voir les informations de diagnostic.
