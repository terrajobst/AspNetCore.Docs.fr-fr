---
title: Fournisseurs de fichiers dans ASP.NET Core
author: guardrex
description: Découvrez comment ASP.NET Core fournit un accès au système de fichiers en utilisant des fournisseurs de fichiers.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/30/2019
uid: fundamentals/file-providers
ms.openlocfilehash: 93eb48d81a853061a874641e84b4875849690a93
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/27/2019
ms.locfileid: "64886614"
---
# <a name="file-providers-in-aspnet-core"></a>Fournisseurs de fichiers dans ASP.NET Core

Par [Steve Smith](https://ardalis.com/) et [Luke Latham](https://github.com/guardrex)

ASP.NET Core fournit un accès au système de fichiers en utilisant des fournisseurs de fichiers. Des fournisseurs de fichiers sont utilisés dans l’infrastructure ASP.NET Core :

* [IHostingEnvironment](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment) expose la racine web et la racine de contenu de l’application sous la forme de types `IFileProvider`.
* [L’intergiciel (middleware) de fichiers statiques](xref:fundamentals/static-files) utilise des fournisseurs de fichiers pour localiser les fichiers statiques.
* [Razor](xref:mvc/views/razor) utilise des fournisseurs de fichiers pour localiser des pages et des vues.
* Les outils .NET Core utilisent des fournisseurs de fichiers et des modèles d’utilisation des caractères génériques pour spécifier les fichiers qui doivent être publiés.

[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/file-providers/samples) ([procédure de téléchargement](xref:index#how-to-download-a-sample))

## <a name="file-provider-interfaces"></a>Interfaces de fournisseur de fichiers

L’interface principale est [IFileProvider](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider). `IFileProvider` expose des méthodes pour :

* Obtenir des informations sur le fichier ([IFileInfo](/dotnet/api/microsoft.extensions.fileproviders.ifileinfo)).
* Obtenir des informations sur le répertoire ([IDirectoryContents](/dotnet/api/microsoft.extensions.fileproviders.idirectorycontents)).
* Configurer des notifications de modification (à l’aide un [IChangeToken](/dotnet/api/microsoft.extensions.primitives.ichangetoken)).

`IFileInfo` fournit des méthodes et propriétés d’utilisation des fichiers :

* [Existe](/dotnet/api/microsoft.extensions.fileproviders.ifileinfo.exists)
* [IsDirectory](/dotnet/api/microsoft.extensions.fileproviders.ifileinfo.isdirectory)
* [Name](/dotnet/api/microsoft.extensions.fileproviders.ifileinfo.name)
* [Longueur](/dotnet/api/microsoft.extensions.fileproviders.ifileinfo.length) (en octets)
* Date [LastModified](/dotnet/api/microsoft.extensions.fileproviders.ifileinfo.lastmodified)

Vous pouvez lire à partir du fichier en utilisant la méthode [IFileInfo.CreateReadStream](/dotnet/api/microsoft.extensions.fileproviders.ifileinfo.createreadstream).

L’exemple d’application montre comment configurer un fournisseur de fichiers dans `Startup.ConfigureServices` pour une utilisation dans toute l’application via [l’injection de dépendances](xref:fundamentals/dependency-injection).

## <a name="file-provider-implementations"></a>Implémentations de fournisseur de fichiers

Trois implémentations de `IFileProvider` sont disponibles.

| Implémentation | Description |
| -------------- | ----------- |
| [PhysicalFileProvider](#physicalfileprovider) | Le fournisseur physique est utilisé pour accéder aux fichiers physiques du système. |
| [ManifestEmbeddedFileProvider](#manifestembeddedfileprovider) | Le fournisseur incorporé de manifeste est utilisé pour accéder à des fichiers incorporés dans des assemblys. |
| [CompositeFileProvider](#compositefileprovider) | Le fournisseur composite est utilisé pour fournir un accès combiné à des fichiers et à des répertoires à partir d’un ou de plusieurs fournisseurs. |

### <a name="physicalfileprovider"></a>PhysicalFileProvider

La classe [PhysicalFileProvider](/dotnet/api/microsoft.extensions.fileproviders.physicalfileprovider) fournit un accès au système de fichiers physique. `PhysicalFileProvider` utilise le type [System.IO.File](/dotnet/api/system.io.file) (pour le fournisseur physique), définissant comme portée tous les chemins à un répertoire et ses enfants. Cette portée empêche l’accès au système de fichiers en dehors du répertoire spécifié et ses enfants. Lors de l’instanciation de ce fournisseur, un chemin de répertoire est obligatoire et sert de chemin de base pour toutes les demandes effectuées à l’aide du fournisseur. Vous pouvez instancier un fournisseur `PhysicalFileProvider` directement, ou vous pouvez demander un `IFileProvider` dans un constructeur à l’aide de [l’injection de dépendances](xref:fundamentals/dependency-injection).

**Types statiques**

Le code suivant montre comment créer un `PhysicalFileProvider` et l’utiliser pour obtenir le contenu du répertoire et les informations sur le fichier :

```csharp
var provider = new PhysicalFileProvider(applicationRoot);
var contents = provider.GetDirectoryContents(string.Empty);
var fileInfo = provider.GetFileInfo("wwwroot/js/site.js");
```

Types dans l’exemple précédent :

* `provider` est un `IFileProvider`.
* `contents` est un `IDirectoryContents`.
* `fileInfo` est un `IFileInfo`.

Le fournisseur de fichiers peut être utilisé pour itérer au sein du répertoire spécifié par `applicationRoot` ou pour appeler `GetFileInfo` afin d’obtenir des informations sur un fichier. Le fournisseur de fichiers n’a pas accès en dehors du répertoire `applicationRoot`.

L’exemple d’application crée le fournisseur dans la classe `Startup.ConfigureServices` de l’application à l’aide de [IHostingEnvironment.ContentRootFileProvider](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment.contentrootfileprovider) :

```csharp
var physicalProvider = _env.ContentRootFileProvider;
```

**Obtenir les types de fournisseurs de fichiers avec l’injection de dépendances**

Injectez le fournisseur dans n’importe quel constructeur de classe et affectez-le à un champ local. Utilisez le champ dans l’ensemble des méthodes de la classe pour accéder aux fichiers.

Dans l’exemple d’application, la classe `IndexModel` reçoit une instance `IFileProvider` pour obtenir le contenu du répertoire pour le chemin d’accès de la base de l’application.

*Pages/Index.cshtml.cs* :

[!code-csharp[](file-providers/samples/2.x/FileProviderSample/Pages/Index.cshtml.cs?name=snippet1)]

`IDirectoryContents` sont itérés dans la page.

*Pages/Index.cshtml* :

[!code-cshtml[](file-providers/samples/2.x/FileProviderSample/Pages/Index.cshtml?name=snippet1)]

### <a name="manifestembeddedfileprovider"></a>ManifestEmbeddedFileProvider

[ManifestEmbeddedFileProvider](/dotnet/api/microsoft.extensions.fileproviders.manifestembeddedfileprovider) est utilisé pour accéder à des fichiers incorporés dans des assemblys. `ManifestEmbeddedFileProvider` utilise un manifeste compilé dans l’assembly pour reconstruire les chemins d’accès d’origine des fichiers intégrés.

Pour générer un manifeste des fichiers incorporés, définissez la propriété `<GenerateEmbeddedFilesManifest>` sur `true`. Spécifiez les fichiers à incorporer avec [ &lt;EmbeddedResource&gt;](/dotnet/core/tools/csproj#default-compilation-includes-in-net-core-projects) :

[!code-csharp[](file-providers/samples/2.x/FileProviderSample/FileProviderSample.csproj?highlight=5,13)]

Utilisez les [modèles glob](#glob-patterns) pour spécifier un ou plusieurs fichiers à incorporer dans l’assembly.

L’exemple d’application crée un `ManifestEmbeddedFileProvider` et transmet l’assembly en cours d’exécution à son constructeur.

*Startup.cs* :

```csharp
var manifestEmbeddedProvider = 
    new ManifestEmbeddedFileProvider(Assembly.GetEntryAssembly());
```

Des surcharges supplémentaires vous permettent de :

* Spécifier un chemin de fichier relatif.
* Définir la portée de fichiers sur une date de dernière modification.
* Nommer la ressource incorporée contenant le manifeste de fichier incorporé.

| Surcharge | Description |
| -------- | ----------- |
| [ManifestEmbeddedFileProvider(Assembly, String)](/dotnet/api/microsoft.extensions.fileproviders.manifestembeddedfileprovider.-ctor#Microsoft_Extensions_FileProviders_ManifestEmbeddedFileProvider__ctor_System_Reflection_Assembly_System_String_) | Accepte un paramètre de chemin d'accès relatif `root` facultatif. Spécifiez `root` pour définir la portée des appels à [GetDirectoryContents](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider.getdirectorycontents) sur ces ressources sous le chemin d’accès fourni. |
| [ManifestEmbeddedFileProvider(Assembly, String, DateTimeOffset)](/dotnet/api/microsoft.extensions.fileproviders.manifestembeddedfileprovider.-ctor#Microsoft_Extensions_FileProviders_ManifestEmbeddedFileProvider__ctor_System_Reflection_Assembly_System_String_System_DateTimeOffset_) | Accepte un paramètre de chemin d’accès relatif `root` facultatif et un paramètre de date `lastModified` ([DateTimeOffset](/dotnet/api/system.datetimeoffset)). La date `lastModified` définit la portée de la dernière date de modification pour les instances [IFileInfo](/dotnet/api/microsoft.extensions.fileproviders.ifileinfo) retournées par [IFileProvider](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider). |
| [ManifestEmbeddedFileProvider(Assembly, String, String, DateTimeOffset)](/dotnet/api/microsoft.extensions.fileproviders.manifestembeddedfileprovider.-ctor#Microsoft_Extensions_FileProviders_ManifestEmbeddedFileProvider__ctor_System_Reflection_Assembly_System_String_System_String_System_DateTimeOffset_) | Accepte un chemin d'accès relatif `root` facultatif, une date `lastModified` et des paramètres `manifestName`. `manifestName` représente le nom de la ressource incorporée contenant la manifeste. |

### <a name="compositefileprovider"></a>CompositeFileProvider

[CompositeFileProvider](/dotnet/api/microsoft.extensions.fileproviders.compositefileprovider) combine des instances `IFileProvider` en exposant une interface unique qui permet d’utiliser des fichiers de différents fournisseurs. Quand vous créez `CompositeFileProvider`, vous passez une ou plusieurs instances `IFileProvider` à son constructeur.

Dans l’exemple d’application, un `PhysicalFileProvider` et un `ManifestEmbeddedFileProvider` fournissent des fichiers à un `CompositeFileProvider` inscrit dans le conteneur de service de l’application :

[!code-csharp[](file-providers/samples/2.x/FileProviderSample/Startup.cs?name=snippet1)]

## <a name="watch-for-changes"></a>Suivre les modifications apportées

La méthode [IFileProvider.Watch](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider.watch) offre un moyen d’observer un ou plusieurs fichiers ou répertoires afin de détecter les changements. `Watch` accepte une chaîne de chemin, qui peut utiliser des [modèles d’utilisation des caractères génériques](#glob-patterns) pour spécifier plusieurs fichiers. `Watch` retourne un [IChangeToken](/dotnet/api/microsoft.extensions.primitives.ichangetoken). Le jeton de modification expose :

* [HasChanged](/dotnet/api/microsoft.extensions.primitives.ichangetoken.haschanged) : une propriété qui peut être inspectée pour déterminer si une modification a eu lieu.
* [RegisterChangeCallback](/dotnet/api/microsoft.extensions.primitives.ichangetoken.registerchangecallback) : appelée quand des modifications sont détectées dans la chaîne de chemin spécifiée. Chaque jeton de modification appelle uniquement son rappel associé en réponse à un changement unique. Pour activer une surveillance constante, vous pouvez utiliser une [TaskCompletionSource](/dotnet/api/system.threading.tasks.taskcompletionsource-1) comme indiqué ci-dessous, ou recréer des instances `IChangeToken` en réponse aux changements.

Dans l’exemple d’application, l’application console *WatchConsole* est configurée pour afficher un message chaque fois qu’un fichier texte est modifié :

[!code-csharp[](file-providers/samples/2.x/WatchConsole/Program.cs?name=snippet1&highlight=1-2,16,19-20)]

Certains systèmes de fichiers, comme les conteneurs Docker et les partages réseau, peuvent ne pas envoyer de manière fiable les notifications de modifications. Définissez la variable d’environnement `DOTNET_USE_POLLING_FILE_WATCHER` sur `1` ou `true` pour interroger le système de fichiers à la recherche de changements toutes les quatre secondes (non configurable).

## <a name="glob-patterns"></a>Modèles Glob

Les chemins de système de fichiers utilisent des modèles à caractères génériques appelés *modèles Glob (ou d’utilisation des caractères génériques)*. Spécifiez les groupes de fichiers avec ces modèles. Les deux caractères génériques sont `*` et `**` :

**`*`**  
Établit une correspondance avec n’importe quel élément au niveau de dossier actuel, avec n’importe quel nom de fichier ou avec n’importe quelle extension de fichier. Les correspondances sont terminées par des caractères `/` et `.` dans le chemin des fichiers.

**`**`**  
Établit une correspondance avec n’importe quel élément sur plusieurs niveaux de répertoire. Peut être utilisé pour établir une correspondance avec plusieurs fichiers dans une hiérarchie de répertoires de manière récursive.

**Exemples de modèles d’utilisation des caractères génériques**

**`directory/file.txt`**  
Établit une correspondance avec un fichier spécifique dans un répertoire spécifique.

**`directory/*.txt`**  
Établit une correspondance avec tous les fichiers ayant l’extension *.txt* dans un répertoire spécifique.

**`directory/*/appsettings.json`**  
Établit une correspondance avec tous les fichiers `appsettings.json` dans les répertoires situés exactement un niveau en dessous du dossier *répertoire*.

**`directory/**/*.txt`**  
Établit une correspondance avec tous les fichiers ayant l’extension *.txt* et se trouvant n’importe où sous le dossier *répertoire*.
