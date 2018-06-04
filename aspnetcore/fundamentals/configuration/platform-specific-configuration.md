---
title: Améliorer une application à partir d’un assembly externe dans ASP.NET Core avec IHostingStartup
author: guardrex
description: Découvrez comment améliorer une application ASP.NET Core à partir d’un assembly externe à l’aide d’une implémentation IHostingStartup.
manager: wpickett
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.custom: mvc
ms.date: 12/07/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/configuration/platform-specific-configuration
ms.openlocfilehash: 618cb4349dcff696db37012af3aee844b82974f2
ms.sourcegitcommit: 43bd79667bbdc8a07bd39fb4cd6f7ad3e70212fb
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/04/2018
ms.locfileid: "34729049"
---
# <a name="enhance-an-app-from-an-external-assembly-in-aspnet-core-with-ihostingstartup"></a>Améliorer une application à partir d’un assembly externe dans ASP.NET Core avec IHostingStartup

Par [Luke Latham](https://github.com/guardrex)

L’implémentation de [IHostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup) permet d’ajouter des améliorations à une application au démarrage à partir d’un assembly externe, en dehors de la classe `Startup` de l’application. Par exemple, une bibliothèque d’outils externe peut utiliser une implémentation `IHostingStartup` pour fournir des fournisseurs ou des services de configuration supplémentaires à une application. `IHostingStartup` *est disponible dans ASP.NET Core 2.0 et ultérieur.*

[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/platform-specific-configuration/sample/) ([procédure de téléchargement](xref:tutorials/index#how-to-download-a-sample))

## <a name="discover-loaded-hosting-startup-assemblies"></a>Découvrir les assemblys d’hébergement au démarrage chargés

Pour découvrir les assemblys d’hébergement au démarrage chargés par l’application ou des bibliothèques, activez la journalisation, puis vérifiez les journaux des applications. Les erreurs qui se produisent durant le chargement des assemblys sont journalisées. Les assemblys d’hébergement au démarrage chargés sont journalisés au niveau Débogage, et toutes les erreurs sont journalisées.

L’exemple d’application lit [HostingStartupAssembliesKey](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults.hostingstartupassemblieskey) dans un tableau `string` et affiche le résultat dans la page Index de l’application :

[!code-csharp[](platform-specific-configuration/sample/HostingStartupSample/Pages/Index.cshtml.cs?name=snippet1&highlight=14-16)]

## <a name="disable-automatic-loading-of-hosting-startup-assemblies"></a>Désactiver le chargement automatique des assemblys d’hébergement au démarrage

Pour désactiver le chargement automatique des assemblys d’hébergement au démarrage, procédez de l’une des manières suivantes :

* Définissez le paramètre de configuration d’hôte [Empêcher l’hébergement au démarrage](xref:fundamentals/host/web-host#prevent-hosting-startup).
* Définissez la variable d’environnement `ASPNETCORE_PREVENTHOSTINGSTARTUP`.

Si le paramètre d’hôte ou la variable d’environnement a la valeur `true` ou `1`, les assemblys d’hébergement au démarrage ne sont pas chargés automatiquement. Si les deux sont définis, le paramètre d’hôte contrôle le comportement.

La désactivation des assemblys d’hébergement au démarrage à l’aide du paramètre d’hôte ou de la variable d’environnement est globale et peut donc désactiver plusieurs caractéristiques d’une application. Il n’est pas possible pour le moment de désactiver de manière sélective un assembly d’hébergement au démarrage ajouté par une bibliothèque, à moins que la bibliothèque n’offre sa propre option de configuration. Dans une version ultérieure, il sera possible de désactiver de manière sélective les assemblys d’hébergement au démarrage (consultez le [problème aspnet/Hosting 1243 sur GitHub](https://github.com/aspnet/Hosting/pull/1243)).

## <a name="implement-ihostingstartup"></a>Implémenter IHostingStartup

### <a name="create-the-assembly"></a>Créer l’assembly

Une amélioration apportée à `IHostingStartup` est déployée sous la forme d’un assembly basé sur une application console sans point d’entrée. L’assembly référence le package [Microsoft.AspNetCore.Hosting.Abstractions](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.Abstractions/) :

[!code-xml[](platform-specific-configuration/snapshot_sample/StartupEnhancement.csproj)]

Un attribut [HostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.hostingstartupattribute) identifie une classe en tant qu’implémentation de `IHostingStartup` pour le chargement et l’exécution durant la génération de [IWebHost](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost). Dans l’exemple suivant, l’espace de noms est `StartupEnhancement`, et la classe est `StartupEnhancementHostingStartup` :

[!code-csharp[](platform-specific-configuration/snapshot_sample/StartupEnhancement.cs?name=snippet1)]

Une classe implémente `IHostingStartup`. La méthode [Configure](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup.configure) de la classe utilise un [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder) pour ajouter des améliorations à une application :

[!code-csharp[](platform-specific-configuration/snapshot_sample/StartupEnhancement.cs?name=snippet2&highlight=3,5)]

Durant la génération d’un projet `IHostingStartup`, le fichier de dépendances (*\*. deps.json*) définit le dossier *bin* comme emplacement du `runtime` :

[!code-json[](platform-specific-configuration/snapshot_sample/StartupEnhancement1.deps.json?range=2-13&highlight=8)]

Seule une partie du fichier est affichée. Le nom de l’assembly dans l’exemple est `StartupEnhancement`.

### <a name="update-the-dependencies-file"></a>Mettre à jour le fichier de dépendances

L’emplacement du runtime est spécifié dans le fichier *\*.deps.json*. Pour activer l’amélioration, l’élément `runtime` doit spécifier l’emplacement de l’assembly du runtime de l’amélioration. Attribuez à l’emplacement du `runtime` le préfixe `lib/<TARGET_FRAMEWORK_MONIKER>/` :

[!code-json[](platform-specific-configuration/snapshot_sample/StartupEnhancement2.deps.json?range=2-13&highlight=8)]

Dans l’exemple d’application, la modification du fichier *\*.deps.json* est effectuée par un script [PowerShell](/powershell/scripting/powershell-scripting). Le script PowerShell est automatiquement déclenché par une cible de build dans le fichier projet.

### <a name="enhancement-activation"></a>Activation de l’amélioration

**Placer le fichier d’assembly**

Le fichier d’assembly de l’implémentation `IHostingStartup` doit être déployé dans le dossier *bin* de l’application ou placé dans le [magasin de runtime](/dotnet/core/deploying/runtime-store) :

Pour une utilisation par utilisateur, placez l’assembly dans le magasin de runtime du profil utilisateur à l’emplacement suivant :

```
<DRIVE>\Users\<USER>\.dotnet\store\x64\<TARGET_FRAMEWORK_MONIKER>\<ENHANCEMENT_ASSEMBLY_NAME>\<ENHANCEMENT_VERSION>\lib\<TARGET_FRAMEWORK_MONIKER>\
```

Pour une utilisation globale, placez l’assembly dans le magasin de runtime de l’installation .NET Core :

```
<DRIVE>\Program Files\dotnet\store\x64\<TARGET_FRAMEWORK_MONIKER>\<ENHANCEMENT_ASSEMBLY_NAME>\<ENHANCEMENT_VERSION>\lib\<TARGET_FRAMEWORK_MONIKER>\
```

En cas de déploiement de l’assembly dans le magasin de runtime, le fichier de symboles peut également être déployé, mais il n’est pas nécessaire au bon fonctionnement de l’amélioration.

**Placer le fichier de dépendances**

Le fichier *\*.deps.json* de l’implémentation doit se trouver dans un emplacement accessible.

Pour une utilisation par utilisateur, placez le fichier dans le dossier `additonalDeps` des paramètres `.dotnet` du profil utilisateur : 

```
<DRIVE>\Users\<USER>\.dotnet\x64\additionalDeps\<ENHANCEMENT_ASSEMBLY_NAME>\shared\Microsoft.NETCore.App\2.1.0\
```

Pour une utilisation globale, placez le fichier dans le dossier `additonalDeps` de l’installation .NET Core :

```
<DRIVE>\Program Files\dotnet\additionalDeps\<ENHANCEMENT_ASSEMBLY_NAME>\shared\Microsoft.NETCore.App\2.1.0\
```

Notez que la version, `2.1.0`, reflète la version du runtime partagé qu’utilise l’application cible. Le runtime partagé est indiqué dans le fichier *\*.runtimeconfig.json*. Dans l’exemple d’application, le runtime partagé est spécifié dans le fichier *HostingStartupSample.runtimeconfig.json*.

**Définir des variables d’environnement**

Définissez les variables d’environnement suivantes dans le contexte de l’application qui utilise l’amélioration.

ASPNETCORE\_HOSTINGSTARTUPASSEMBLIES

Seuls les assemblys d’hébergement au démarrage sont analysés à la recherche de `HostingStartupAttribute`. Le nom d’assembly de l’implémentation est fourni dans cette variable d’environnement. L’exemple d’application définit la valeur `StartupDiagnostics`.

Vous pouvez également définir la valeur à l’aide du paramètre de configuration d’hôte [Assemblys d’hébergement au démarrage](xref:fundamentals/host/web-host#hosting-startup-assemblies).

DOTNET\_ADDITIONAL\_DEPS

L’emplacement du fichier *\*.deps.json* de l’implémentation.

Si le fichier est placé dans le dossier *.dotnet* du profil utilisateur pour une utilisation par utilisateur :

```
<DRIVE>\Users\<USER>\.dotnet\x64\additionalDeps\
```

Si le fichier est placé dans l’installation .NET Core pour une utilisation globale, fournissez le chemin complet au fichier :

```
<DRIVE>\Program Files\dotnet\additionalDeps\<ENHANCEMENT_ASSEMBLY_NAME>\shared\Microsoft.NETCore.App\2.1.0\<ENHANCEMENT_ASSEMBLY_NAME>.deps.json
```

L’exemple d’application définit la valeur suivante :

```
%UserProfile%\.dotnet\x64\additionalDeps\StartupDiagnostics\
```

Pour obtenir des exemples montrant comment définir des variables d’environnement pour différents systèmes d’exploitation, consultez [Utiliser plusieurs environnements](xref:fundamentals/environments).

## <a name="sample-app"></a>Exemple d’application

[L’exemple d’application](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/platform-specific-configuration/sample/) ([comment télécharger](xref:tutorials/index#how-to-download-a-sample)) utilise `IHostingStartup` pour créer un outil de diagnostic. L’outil ajoute deux middlewares (intergiciels) à l’application au démarrage, qui fournissent des informations de diagnostic :

* Services inscrits
* Adresse : schéma, hôte, base du chemin, chemin, chaîne de requête
* Connexion : adresse IP distante, port distant, adresse IP locale, port local, certificat client
* En-têtes de requête
* Variables d’environnement

Pour exécuter l’exemple :

1. Le projet StartupDiagnostics utilise [PowerShell](/powershell/scripting/powershell-scripting) pour modifier son fichier *StartupDiagnostics.deps.json*. PowerShell est installé par défaut sur les systèmes d’exploitation Windows à compter de Windows 7 SP1 et de Windows Server 2008 R2 SP1. Pour obtenir PowerShell sur d’autres plateformes, consultez [Installation de Windows PowerShell](/powershell/scripting/setup/installing-windows-powershell).
2. Générez le projet StartupDiagnostics. Une cible de build dans le fichier projet :
   * Déplace l’assembly et les fichiers de symboles dans le magasin de runtime du profil utilisateur.
   * Déclenche le script PowerShell pour modifier le fichier *StartupDiagnostics.deps.json*.
   * Déplace le fichier *StartupDiagnostics.deps.json* dans le dossier `additionalDeps` du profil utilisateur.
3. Définissez les variables d’environnement :
    * `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`: `StartupDiagnostics`
    * `DOTNET_ADDITIONAL_DEPS`: `%UserProfile%\.dotnet\x64\additionalDeps\StartupDiagnostics\`
4. Exécutez l’exemple d’application.
5. Demandez le point de terminaison `/services` pour voir les services inscrits de l’application. Demandez le point de terminaison `/diag` pour voir les informations de diagnostic.
