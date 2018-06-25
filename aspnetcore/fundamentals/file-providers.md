---
title: Fournisseurs de fichiers dans ASP.NET Core
author: ardalis
description: Découvrez comment ASP.NET Core fournit un accès au système de fichiers en utilisant des fournisseurs de fichiers.
ms.author: riande
ms.date: 02/14/2017
uid: fundamentals/file-providers
ms.openlocfilehash: 0d356322ea9f4cc2caead81746bf9ede4a87923f
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/20/2018
ms.locfileid: "36276237"
---
# <a name="file-providers-in-aspnet-core"></a>Fournisseurs de fichiers dans ASP.NET Core

Par [Steve Smith](https://ardalis.com/)

ASP.NET Core fournit un accès au système de fichiers en utilisant des fournisseurs de fichiers.

[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/file-providers/sample) ([procédure de téléchargement](xref:tutorials/index#how-to-download-a-sample))

## <a name="file-provider-abstractions"></a>Abstractions de fournisseurs de fichiers

Les fournisseurs de fichiers sont une abstraction sur les systèmes de fichiers. L’interface principale est `IFileProvider`. `IFileProvider` expose des méthodes permettant d’obtenir des informations de fichier (`IFileInfo`) et des informations de répertoire (`IDirectoryContents`), et de configurer des notifications de modifications (à l’aide d’un `IChangeToken`).

`IFileInfo` fournit des méthodes et des propriétés sur des fichiers ou des répertoires individuels. Cette méthode possède deux propriétés booléennes, `Exists` et `IsDirectory`, ainsi que des propriétés décrivant le nom (`Name`), la longueur (`Length`, en octets) et la date de dernière modification (`LastModified`) du fichier. Vous pouvez lire dans le fichier en utilisant sa méthode `CreateReadStream`.

## <a name="file-provider-implementations"></a>Implémentations de fournisseur de fichiers

Trois implémentations d’`IFileProvider` sont disponibles : Physique, Incorporé et Composite. Le fournisseur physique est utilisé pour accéder à des fichiers du système réel. Le fournisseur incorporé est utilisé pour accéder à des fichiers incorporés dans des assemblys. Le fournisseur composite est utilisé pour fournir un accès combiné à des fichiers et à des répertoires à partir d’un ou de plusieurs fournisseurs.

### <a name="physicalfileprovider"></a>PhysicalFileProvider

La classe `PhysicalFileProvider` fournit un accès au système de fichiers physique. Elle encapsule le type `System.IO.File` (pour le fournisseur physique), définissant comme portée tous les chemins à un répertoire et ses enfants. Cette portée limite l’accès à un répertoire donné et à ses enfants, ce qui empêche l’accès au système de fichiers en dehors de cette limite. Lors de l’instanciation de ce fournisseur, vous devez lui fournir un chemin de répertoire qui sert de chemin de base pour toutes les requêtes effectuées auprès de ce fournisseur (et qui limite l’accès en dehors de ce chemin). Dans une application ASP.NET Core, vous pouvez instancier un fournisseur `PhysicalFileProvider` directement, ou vous pouvez demander un `IFileProvider` dans un contrôleur ou dans le constructeur d’un service à l’aide de [l’injection de dépendances](dependency-injection.md). Cette dernière approche génère habituellement une solution plus souple et plus facile à tester.

L’exemple ci-dessous montre comment créer un `PhysicalFileProvider`.


```csharp
IFileProvider provider = new PhysicalFileProvider(applicationRoot);
IDirectoryContents contents = provider.GetDirectoryContents(""); // the applicationRoot contents
IFileInfo fileInfo = provider.GetFileInfo("wwwroot/js/site.js"); // a file under applicationRoot
```

Vous pouvez itérer au sein du contenu de ses répertoires ou obtenir les informations d’un fichier spécifique en fournissant un sous-chemin.

Pour demander un fournisseur à partir d’un contrôleur, spécifiez-le dans le constructeur du contrôleur et attribuez-le à un champ local. Utilisez l’instance locale à partir de vos méthodes d’action :

[!code-csharp[](file-providers/sample/src/FileProviderSample/Controllers/HomeController.cs?highlight=5,7,12&range=6-19)]

Créez ensuite le fournisseur dans la classe `Startup` de l’application :

[!code-csharp[](file-providers/sample/src/FileProviderSample/Startup.cs?highlight=35,40&range=1-43)]

Dans la vue *Index.cshtml*, itérez au sein du `IDirectoryContents` fourni :

[!code-html[](file-providers/sample/src/FileProviderSample/Views/Home/Index.cshtml?highlight=2,7,9,11,15)]

Le résultat :

![Exemple d’application de fournisseur de fichiers répertoriant des fichiers et dossiers physiques](file-providers/_static/physical-directory-listing.png)

### <a name="embeddedfileprovider"></a>EmbeddedFileProvider

`EmbeddedFileProvider` est utilisé pour accéder à des fichiers incorporés dans des assemblys. Dans .NET Core, vous incorporez des fichiers dans un assembly avec l’élément `<EmbeddedResource>` dans le fichier *.csproj* :

[!code-json[](file-providers/sample/src/FileProviderSample/FileProviderSample.csproj?range=13-18)]

Vous pouvez utiliser des [modèles d’utilisation des caractères génériques](#globbing-patterns) lors de la spécification des fichiers à incorporer dans l’assembly. Ces modèles peuvent être utilisés pour faire correspondre un ou plusieurs fichiers.

> [!NOTE]
> Il est peu probable que vous souhaitiez réellement incorporer tous les fichiers .js de votre projet dans son assembly. L’exemple ci-dessus est fourni à des fins de démonstration uniquement.

Quand vous créez un `EmbeddedFileProvider`, passez l’assembly qu’il lira à son constructeur.

```csharp
var embeddedProvider = new EmbeddedFileProvider(Assembly.GetEntryAssembly());
```

L’extrait de code ci-dessus montre comment créer un `EmbeddedFileProvider` avec accès à l’assembly en cours d’exécution.

La mise à jour de l’exemple d’application pour utiliser un `EmbeddedFileProvider` génère la sortie suivante :

![Exemple d’application de fournisseur de fichiers répertoriant des fichiers incorporés](file-providers/_static/embedded-directory-listing.png)

> [!NOTE]
> Les ressources incorporées n’exposent pas les répertoires. Au lieu de cela, le chemin de la ressource (par l’intermédiaire de son espace de noms) est incorporé dans son nom de fichier à l’aide de séparateurs `.`.

> [!TIP]
> Le constructeur `EmbeddedFileProvider` accepte un paramètre facultatif `baseNamespace`. La spécification de ce paramètre définit `GetDirectoryContents` comme portée des appels aux ressources se trouvant sous l’espace de noms fourni.

### <a name="compositefileprovider"></a>CompositeFileProvider

`CompositeFileProvider` combine des instances `IFileProvider`, en exposant une interface unique qui permet d’utiliser des fichiers de différents fournisseurs. Quand vous créez `CompositeFileProvider`, vous passez une ou plusieurs instances `IFileProvider` à son constructeur :

[!code-csharp[](file-providers/sample/src/FileProviderSample/Startup.cs?highlight=3&range=35-37)]

La mise à jour de l’exemple d’application pour utiliser un `CompositeFileProvider` qui inclut à la fois le fournisseur physique et le fournisseur incorporé configurés précédemment génère la sortie suivante :

![Exemple d’application de fournisseur de fichiers répertoriant à la fois des fichiers et dossiers physiques et des fichiers incorporés](file-providers/_static/composite-directory-listing.png)

## <a name="watching-for-changes"></a>Détection des changements

La méthode `IFileProvider` `Watch` offre un moyen d’observer un ou plusieurs fichiers ou répertoires afin de détecter les changements. Cette méthode accepte une chaîne de chemin, qui peut utiliser des [modèles d’utilisation des caractères génériques](#globbing-patterns) pour spécifier plusieurs fichiers, et retourne un `IChangeToken`. Ce jeton expose une propriété `HasChanged` qui peut être inspectée, et une méthode `RegisterChangeCallback` qui est appelée quand des changements sont détectés dans la chaîne de chemin spécifiée. Notez que chaque jeton de modification appelle uniquement son rappel associé en réponse à un changement unique. Pour activer une surveillance constante, vous pouvez utiliser un `TaskCompletionSource` comme indiqué ci-dessous, ou recréer des instances `IChangeToken` en réponse aux changements.

Dans l’exemple de cet article, une application console est configurée pour afficher un message chaque fois qu’un fichier texte est modifié :

[!code-csharp[](file-providers/sample/src/WatchConsole/Program.cs?name=snippet1&highlight=1-2,16,19-20)]

Voici le résultat, après avoir enregistré plusieurs fois le fichier :

![Après l’exécution de dotnet run, la fenêtre Commande présente l’application surveillant le fichier quotes.txt afin de détecter les changements et montre que le fichier a changé cinq fois.](file-providers/_static/watch-console.png)

> [!NOTE]
> Certains systèmes de fichiers, comme les conteneurs Docker et les partages réseau, peuvent ne pas envoyer de manière fiable les notifications de modifications. Définissez la variable d’environnement `DOTNET_USE_POLLINGFILEWATCHER` sur `1` ou `true` pour interroger le système de fichiers à la recherche de changements toutes les 4 secondes.

## <a name="globbing-patterns"></a>Modèles d’utilisation des caractères génériques

Les chemins de système de fichiers utilisent des modèles à caractères génériques appelés *modèles d’utilisation des caractères génériques*. Ces modèles simples peuvent être utilisés pour spécifier des groupes de fichiers. Les deux caractères génériques sont `*` et `**`.

**`*`**

   Établit une correspondance avec n’importe quel élément au niveau de dossier actuel, avec n’importe quel nom de fichier ou avec n’importe quelle extension de fichier. Les correspondances sont terminées par des caractères `/` et `.` dans le chemin des fichiers.

<strong><code>**</code></strong>

   Établit une correspondance avec n’importe quel élément sur plusieurs niveaux de répertoire. Peut être utilisé pour établir une correspondance avec plusieurs fichiers dans une hiérarchie de répertoires de manière récursive.

### <a name="globbing-pattern-examples"></a>Exemples de modèles d’utilisation des caractères génériques

**`directory/file.txt`**

   Établit une correspondance avec un fichier spécifique dans un répertoire spécifique.

**<code>directory/*.txt</code>**

   Établit une correspondance avec tous les fichiers ayant l’extension `.txt` dans un répertoire spécifique.

**`directory/*/bower.json`**

   Établit une correspondance avec tous les fichiers `bower.json` dans les répertoires situés exactement un niveau en dessous du répertoire `directory`.

**<code>directory/&#42;&#42;/&#42;.txt</code>**

   Établit une correspondance avec tous les fichiers ayant l’extension `.txt` et se trouvant n’importe où sous le répertoire `directory`.

## <a name="file-provider-usage-in-aspnet-core"></a>Utilisation des fournisseurs de fichiers dans ASP.NET Core

Plusieurs parties d’ASP.NET Core utilisent des fournisseurs de fichiers. `IHostingEnvironment` expose la racine web et la racine de contenu de l’application sous la forme de types `IFileProvider`. L’intergiciel (middleware) des fichiers statiques utilise des fournisseurs de fichiers pour localiser les fichiers statiques. Razor utilise `IFileProvider` de façon intensive dans la localisation de vues. La fonctionnalité de publication de Dotnet utilise des fournisseurs de fichiers et des modèles d’utilisation des caractères génériques pour spécifier les fichiers qui doivent être publiés.

## <a name="recommendations-for-use-in-apps"></a>Recommandations d’utilisation dans des applications

Si votre application ASP.NET Core exige un accès au système de fichiers, vous pouvez demander une instance d’`IFileProvider` par le biais d’une injection de dépendances, puis utiliser ses méthodes pour effectuer l’accès, comme illustré dans cet exemple. Cela vous permet de configurer le fournisseur une seule fois, au démarrage de l’application, et réduit le nombre de types d’implémentation que votre application instancie.
