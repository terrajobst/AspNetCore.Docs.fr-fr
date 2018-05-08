---
title: Fournisseurs de fichiers dans ASP.NET Core
author: ardalis
description: Découvrez comment ASP.NET Core fournit un accès au système de fichiers en utilisant des fournisseurs de fichiers.
manager: wpickett
ms.author: riande
ms.date: 02/14/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/file-providers
ms.openlocfilehash: cdbffdadd9616fe941809d67dc2c0bbd52149561
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/02/2018
---
# <a name="file-providers-in-aspnet-core"></a><span data-ttu-id="fdf43-103">Fournisseurs de fichiers dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="fdf43-103">File Providers in ASP.NET Core</span></span>

<span data-ttu-id="fdf43-104">Par [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="fdf43-104">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="fdf43-105">ASP.NET Core fournit un accès au système de fichiers en utilisant des fournisseurs de fichiers.</span><span class="sxs-lookup"><span data-stu-id="fdf43-105">ASP.NET Core abstracts file system access through the use of File Providers.</span></span>

<span data-ttu-id="fdf43-106">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/file-providers/sample) ([procédure de téléchargement](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="fdf43-106">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/file-providers/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="file-provider-abstractions"></a><span data-ttu-id="fdf43-107">Abstractions de fournisseurs de fichiers</span><span class="sxs-lookup"><span data-stu-id="fdf43-107">File Provider abstractions</span></span>

<span data-ttu-id="fdf43-108">Les fournisseurs de fichiers sont une abstraction sur les systèmes de fichiers.</span><span class="sxs-lookup"><span data-stu-id="fdf43-108">File Providers are an abstraction over file systems.</span></span> <span data-ttu-id="fdf43-109">L’interface principale est `IFileProvider`.</span><span class="sxs-lookup"><span data-stu-id="fdf43-109">The main interface is `IFileProvider`.</span></span> <span data-ttu-id="fdf43-110">`IFileProvider` expose des méthodes permettant d’obtenir des informations de fichier (`IFileInfo`) et des informations de répertoire (`IDirectoryContents`), et de configurer des notifications de modifications (à l’aide d’un `IChangeToken`).</span><span class="sxs-lookup"><span data-stu-id="fdf43-110">`IFileProvider` exposes methods to get file information (`IFileInfo`), directory information (`IDirectoryContents`), and to set up change notifications (using an `IChangeToken`).</span></span>

<span data-ttu-id="fdf43-111">`IFileInfo` fournit des méthodes et des propriétés sur des fichiers ou des répertoires individuels.</span><span class="sxs-lookup"><span data-stu-id="fdf43-111">`IFileInfo` provides methods and properties about individual files or directories.</span></span> <span data-ttu-id="fdf43-112">Cette méthode possède deux propriétés booléennes, `Exists` et `IsDirectory`, ainsi que des propriétés décrivant le nom (`Name`), la longueur (`Length`, en octets) et la date de dernière modification (`LastModified`) du fichier.</span><span class="sxs-lookup"><span data-stu-id="fdf43-112">It has two boolean properties, `Exists` and `IsDirectory`, as well as properties describing the file's `Name`, `Length` (in bytes), and `LastModified` date.</span></span> <span data-ttu-id="fdf43-113">Vous pouvez lire dans le fichier en utilisant sa méthode `CreateReadStream`.</span><span class="sxs-lookup"><span data-stu-id="fdf43-113">You can read from the file using its `CreateReadStream` method.</span></span>

## <a name="file-provider-implementations"></a><span data-ttu-id="fdf43-114">Implémentations de fournisseur de fichiers</span><span class="sxs-lookup"><span data-stu-id="fdf43-114">File Provider implementations</span></span>

<span data-ttu-id="fdf43-115">Trois implémentations d’`IFileProvider` sont disponibles : Physique, Incorporé et Composite.</span><span class="sxs-lookup"><span data-stu-id="fdf43-115">Three implementations of `IFileProvider` are available: Physical, Embedded, and Composite.</span></span> <span data-ttu-id="fdf43-116">Le fournisseur physique est utilisé pour accéder à des fichiers du système réel.</span><span class="sxs-lookup"><span data-stu-id="fdf43-116">The physical provider is used to access the actual system's files.</span></span> <span data-ttu-id="fdf43-117">Le fournisseur incorporé est utilisé pour accéder à des fichiers incorporés dans des assemblys.</span><span class="sxs-lookup"><span data-stu-id="fdf43-117">The embedded provider is used to access files embedded in assemblies.</span></span> <span data-ttu-id="fdf43-118">Le fournisseur composite est utilisé pour fournir un accès combiné à des fichiers et à des répertoires à partir d’un ou de plusieurs fournisseurs.</span><span class="sxs-lookup"><span data-stu-id="fdf43-118">The composite provider is used to provide combined access to files and directories from one or more other providers.</span></span>

### <a name="physicalfileprovider"></a><span data-ttu-id="fdf43-119">PhysicalFileProvider</span><span class="sxs-lookup"><span data-stu-id="fdf43-119">PhysicalFileProvider</span></span>

<span data-ttu-id="fdf43-120">La classe `PhysicalFileProvider` fournit un accès au système de fichiers physique.</span><span class="sxs-lookup"><span data-stu-id="fdf43-120">The `PhysicalFileProvider` provides access to the physical file system.</span></span> <span data-ttu-id="fdf43-121">Elle encapsule le type `System.IO.File` (pour le fournisseur physique), définissant comme portée tous les chemins à un répertoire et ses enfants.</span><span class="sxs-lookup"><span data-stu-id="fdf43-121">It wraps the `System.IO.File` type (for the physical provider), scoping all paths to a directory and its children.</span></span> <span data-ttu-id="fdf43-122">Cette portée limite l’accès à un répertoire donné et à ses enfants, ce qui empêche l’accès au système de fichiers en dehors de cette limite.</span><span class="sxs-lookup"><span data-stu-id="fdf43-122">This scoping limits access to a certain directory and its children, preventing access to the file system outside of this boundary.</span></span> <span data-ttu-id="fdf43-123">Lors de l’instanciation de ce fournisseur, vous devez lui fournir un chemin de répertoire qui sert de chemin de base pour toutes les requêtes effectuées auprès de ce fournisseur (et qui limite l’accès en dehors de ce chemin).</span><span class="sxs-lookup"><span data-stu-id="fdf43-123">When instantiating this provider, you must provide it with a directory path, which serves as the base path for all requests made to this provider (and which restricts access outside of this path).</span></span> <span data-ttu-id="fdf43-124">Dans une application ASP.NET Core, vous pouvez instancier un fournisseur `PhysicalFileProvider` directement, ou vous pouvez demander un `IFileProvider` dans un contrôleur ou dans le constructeur d’un service à l’aide de [l’injection de dépendances](dependency-injection.md).</span><span class="sxs-lookup"><span data-stu-id="fdf43-124">In an ASP.NET Core app, you can instantiate a `PhysicalFileProvider` provider directly, or you can request an `IFileProvider` in a Controller or service's constructor through [dependency injection](dependency-injection.md).</span></span> <span data-ttu-id="fdf43-125">Cette dernière approche génère habituellement une solution plus souple et plus facile à tester.</span><span class="sxs-lookup"><span data-stu-id="fdf43-125">The latter approach will typically yield a more flexible and testable solution.</span></span>

<span data-ttu-id="fdf43-126">L’exemple ci-dessous montre comment créer un `PhysicalFileProvider`.</span><span class="sxs-lookup"><span data-stu-id="fdf43-126">The sample below shows how to create a `PhysicalFileProvider`.</span></span>


```csharp
IFileProvider provider = new PhysicalFileProvider(applicationRoot);
IDirectoryContents contents = provider.GetDirectoryContents(""); // the applicationRoot contents
IFileInfo fileInfo = provider.GetFileInfo("wwwroot/js/site.js"); // a file under applicationRoot
```

<span data-ttu-id="fdf43-127">Vous pouvez itérer au sein du contenu de ses répertoires ou obtenir les informations d’un fichier spécifique en fournissant un sous-chemin.</span><span class="sxs-lookup"><span data-stu-id="fdf43-127">You can iterate through its directory contents or get a specific file's information by providing a subpath.</span></span>

<span data-ttu-id="fdf43-128">Pour demander un fournisseur à partir d’un contrôleur, spécifiez-le dans le constructeur du contrôleur et attribuez-le à un champ local.</span><span class="sxs-lookup"><span data-stu-id="fdf43-128">To request a provider from a controller, specify it in the controller's constructor and assign it to a local field.</span></span> <span data-ttu-id="fdf43-129">Utilisez l’instance locale à partir de vos méthodes d’action :</span><span class="sxs-lookup"><span data-stu-id="fdf43-129">Use the local instance from your action methods:</span></span>

[!code-csharp[](file-providers/sample/src/FileProviderSample/Controllers/HomeController.cs?highlight=5,7,12&range=6-19)]

<span data-ttu-id="fdf43-130">Créez ensuite le fournisseur dans la classe `Startup` de l’application :</span><span class="sxs-lookup"><span data-stu-id="fdf43-130">Then, create the provider in the app's `Startup` class:</span></span>

[!code-csharp[](file-providers/sample/src/FileProviderSample/Startup.cs?highlight=35,40&range=1-43)]

<span data-ttu-id="fdf43-131">Dans la vue *Index.cshtml*, itérez au sein du `IDirectoryContents` fourni :</span><span class="sxs-lookup"><span data-stu-id="fdf43-131">In the *Index.cshtml* view, iterate through the `IDirectoryContents` provided:</span></span>

[!code-html[](file-providers/sample/src/FileProviderSample/Views/Home/Index.cshtml?highlight=2,7,9,11,15)]

<span data-ttu-id="fdf43-132">Le résultat :</span><span class="sxs-lookup"><span data-stu-id="fdf43-132">The result:</span></span>

![Exemple d’application de fournisseur de fichiers répertoriant des fichiers et dossiers physiques](file-providers/_static/physical-directory-listing.png)

### <a name="embeddedfileprovider"></a><span data-ttu-id="fdf43-134">EmbeddedFileProvider</span><span class="sxs-lookup"><span data-stu-id="fdf43-134">EmbeddedFileProvider</span></span>

<span data-ttu-id="fdf43-135">`EmbeddedFileProvider` est utilisé pour accéder à des fichiers incorporés dans des assemblys.</span><span class="sxs-lookup"><span data-stu-id="fdf43-135">The `EmbeddedFileProvider` is used to access files embedded in assemblies.</span></span> <span data-ttu-id="fdf43-136">Dans .NET Core, vous incorporez des fichiers dans un assembly avec l’élément `<EmbeddedResource>` dans le fichier *.csproj* :</span><span class="sxs-lookup"><span data-stu-id="fdf43-136">In .NET Core, you embed files in an assembly with the `<EmbeddedResource>` element in the *.csproj* file:</span></span>

[!code-json[](file-providers/sample/src/FileProviderSample/FileProviderSample.csproj?range=13-18)]

<span data-ttu-id="fdf43-137">Vous pouvez utiliser des [modèles d’utilisation des caractères génériques](#globbing-patterns) lors de la spécification des fichiers à incorporer dans l’assembly.</span><span class="sxs-lookup"><span data-stu-id="fdf43-137">You can use [globbing patterns](#globbing-patterns) when specifying files to embed in the assembly.</span></span> <span data-ttu-id="fdf43-138">Ces modèles peuvent être utilisés pour faire correspondre un ou plusieurs fichiers.</span><span class="sxs-lookup"><span data-stu-id="fdf43-138">These patterns can be used to match one or more files.</span></span>

> [!NOTE]
> <span data-ttu-id="fdf43-139">Il est peu probable que vous souhaitiez réellement incorporer tous les fichiers .js de votre projet dans son assembly. L’exemple ci-dessus est fourni à des fins de démonstration uniquement.</span><span class="sxs-lookup"><span data-stu-id="fdf43-139">It's unlikely you would ever want to actually embed every .js file in your project in its assembly; the above sample is for demo purposes only.</span></span>

<span data-ttu-id="fdf43-140">Quand vous créez un `EmbeddedFileProvider`, passez l’assembly qu’il lira à son constructeur.</span><span class="sxs-lookup"><span data-stu-id="fdf43-140">When creating an `EmbeddedFileProvider`, pass the assembly it will read to its constructor.</span></span>

```csharp
var embeddedProvider = new EmbeddedFileProvider(Assembly.GetEntryAssembly());
```

<span data-ttu-id="fdf43-141">L’extrait de code ci-dessus montre comment créer un `EmbeddedFileProvider` avec accès à l’assembly en cours d’exécution.</span><span class="sxs-lookup"><span data-stu-id="fdf43-141">The snippet above demonstrates how to create an `EmbeddedFileProvider` with access to the currently executing assembly.</span></span>

<span data-ttu-id="fdf43-142">La mise à jour de l’exemple d’application pour utiliser un `EmbeddedFileProvider` génère la sortie suivante :</span><span class="sxs-lookup"><span data-stu-id="fdf43-142">Updating the sample app to use an `EmbeddedFileProvider` results in the following output:</span></span>

![Exemple d’application de fournisseur de fichiers répertoriant des fichiers incorporés](file-providers/_static/embedded-directory-listing.png)

> [!NOTE]
> <span data-ttu-id="fdf43-144">Les ressources incorporées n’exposent pas les répertoires.</span><span class="sxs-lookup"><span data-stu-id="fdf43-144">Embedded resources don't expose directories.</span></span> <span data-ttu-id="fdf43-145">Au lieu de cela, le chemin de la ressource (par l’intermédiaire de son espace de noms) est incorporé dans son nom de fichier à l’aide de séparateurs `.`.</span><span class="sxs-lookup"><span data-stu-id="fdf43-145">Rather, the path to the resource (via its namespace) is embedded in its filename using `.` separators.</span></span>

> [!TIP]
> <span data-ttu-id="fdf43-146">Le constructeur `EmbeddedFileProvider` accepte un paramètre facultatif `baseNamespace`.</span><span class="sxs-lookup"><span data-stu-id="fdf43-146">The `EmbeddedFileProvider` constructor accepts an optional `baseNamespace` parameter.</span></span> <span data-ttu-id="fdf43-147">La spécification de ce paramètre définit `GetDirectoryContents` comme portée des appels aux ressources se trouvant sous l’espace de noms fourni.</span><span class="sxs-lookup"><span data-stu-id="fdf43-147">Specifying this will scope calls to `GetDirectoryContents` to those resources under the provided namespace.</span></span>

### <a name="compositefileprovider"></a><span data-ttu-id="fdf43-148">CompositeFileProvider</span><span class="sxs-lookup"><span data-stu-id="fdf43-148">CompositeFileProvider</span></span>

<span data-ttu-id="fdf43-149">`CompositeFileProvider` combine des instances `IFileProvider`, en exposant une interface unique qui permet d’utiliser des fichiers de différents fournisseurs.</span><span class="sxs-lookup"><span data-stu-id="fdf43-149">The `CompositeFileProvider` combines `IFileProvider` instances, exposing a single interface for working with files from multiple providers.</span></span> <span data-ttu-id="fdf43-150">Quand vous créez `CompositeFileProvider`, vous passez une ou plusieurs instances `IFileProvider` à son constructeur :</span><span class="sxs-lookup"><span data-stu-id="fdf43-150">When creating the `CompositeFileProvider`, you pass one or more `IFileProvider` instances to its constructor:</span></span>

[!code-csharp[](file-providers/sample/src/FileProviderSample/Startup.cs?highlight=3&range=35-37)]

<span data-ttu-id="fdf43-151">La mise à jour de l’exemple d’application pour utiliser un `CompositeFileProvider` qui inclut à la fois le fournisseur physique et le fournisseur incorporé configurés précédemment génère la sortie suivante :</span><span class="sxs-lookup"><span data-stu-id="fdf43-151">Updating the sample app to use a `CompositeFileProvider` that includes both the physical and embedded providers configured previously, results in the following output:</span></span>

![Exemple d’application de fournisseur de fichiers répertoriant à la fois des fichiers et dossiers physiques et des fichiers incorporés](file-providers/_static/composite-directory-listing.png)

## <a name="watching-for-changes"></a><span data-ttu-id="fdf43-153">Détection des changements</span><span class="sxs-lookup"><span data-stu-id="fdf43-153">Watching for changes</span></span>

<span data-ttu-id="fdf43-154">La méthode `IFileProvider` `Watch` offre un moyen d’observer un ou plusieurs fichiers ou répertoires afin de détecter les changements.</span><span class="sxs-lookup"><span data-stu-id="fdf43-154">The `IFileProvider` `Watch` method provides a way to watch one or more files or directories for changes.</span></span> <span data-ttu-id="fdf43-155">Cette méthode accepte une chaîne de chemin, qui peut utiliser des [modèles d’utilisation des caractères génériques](#globbing-patterns) pour spécifier plusieurs fichiers, et retourne un `IChangeToken`.</span><span class="sxs-lookup"><span data-stu-id="fdf43-155">This method accepts a path string, which can use [globbing patterns](#globbing-patterns) to specify multiple files, and returns an `IChangeToken`.</span></span> <span data-ttu-id="fdf43-156">Ce jeton expose une propriété `HasChanged` qui peut être inspectée, et une méthode `RegisterChangeCallback` qui est appelée quand des changements sont détectés dans la chaîne de chemin spécifiée.</span><span class="sxs-lookup"><span data-stu-id="fdf43-156">This token exposes a `HasChanged` property that can be inspected, and a `RegisterChangeCallback` method that's called when changes are detected to the specified path string.</span></span> <span data-ttu-id="fdf43-157">Notez que chaque jeton de modification appelle uniquement son rappel associé en réponse à un changement unique.</span><span class="sxs-lookup"><span data-stu-id="fdf43-157">Note that each change token only calls its associated callback in response to a single change.</span></span> <span data-ttu-id="fdf43-158">Pour activer une surveillance constante, vous pouvez utiliser un `TaskCompletionSource` comme indiqué ci-dessous, ou recréer des instances `IChangeToken` en réponse aux changements.</span><span class="sxs-lookup"><span data-stu-id="fdf43-158">To enable constant monitoring, you can use a `TaskCompletionSource` as shown below, or re-create `IChangeToken` instances in response to changes.</span></span>

<span data-ttu-id="fdf43-159">Dans l’exemple de cet article, une application console est configurée pour afficher un message chaque fois qu’un fichier texte est modifié :</span><span class="sxs-lookup"><span data-stu-id="fdf43-159">In this article's sample, a console application is configured to display a message whenever a text file is modified:</span></span>

[!code-csharp[](file-providers/sample/src/WatchConsole/Program.cs?name=snippet1&highlight=1-2,16,19-20)]

<span data-ttu-id="fdf43-160">Voici le résultat, après avoir enregistré plusieurs fois le fichier :</span><span class="sxs-lookup"><span data-stu-id="fdf43-160">The result, after saving the file several times:</span></span>

![Après l’exécution de dotnet run, la fenêtre Commande présente l’application surveillant le fichier quotes.txt afin de détecter les changements et montre que le fichier a changé cinq fois.](file-providers/_static/watch-console.png)

> [!NOTE]
> <span data-ttu-id="fdf43-162">Certains systèmes de fichiers, comme les conteneurs Docker et les partages réseau, peuvent ne pas envoyer de manière fiable les notifications de modifications.</span><span class="sxs-lookup"><span data-stu-id="fdf43-162">Some file systems, such as Docker containers and network shares, may not reliably send change notifications.</span></span> <span data-ttu-id="fdf43-163">Définissez la variable d’environnement `DOTNET_USE_POLLINGFILEWATCHER` sur `1` ou `true` pour interroger le système de fichiers à la recherche de changements toutes les 4 secondes.</span><span class="sxs-lookup"><span data-stu-id="fdf43-163">Set the `DOTNET_USE_POLLINGFILEWATCHER` environment variable to `1` or `true` to poll the file system for changes every 4 seconds.</span></span>

## <a name="globbing-patterns"></a><span data-ttu-id="fdf43-164">Modèles d’utilisation des caractères génériques</span><span class="sxs-lookup"><span data-stu-id="fdf43-164">Globbing patterns</span></span>

<span data-ttu-id="fdf43-165">Les chemins de système de fichiers utilisent des modèles à caractères génériques appelés *modèles d’utilisation des caractères génériques*.</span><span class="sxs-lookup"><span data-stu-id="fdf43-165">File system paths use wildcard patterns called *globbing patterns*.</span></span> <span data-ttu-id="fdf43-166">Ces modèles simples peuvent être utilisés pour spécifier des groupes de fichiers.</span><span class="sxs-lookup"><span data-stu-id="fdf43-166">These simple patterns can be used to specify groups of files.</span></span> <span data-ttu-id="fdf43-167">Les deux caractères génériques sont `*` et `**`.</span><span class="sxs-lookup"><span data-stu-id="fdf43-167">The two wildcard characters are `*` and `**`.</span></span>

**`*`**

   <span data-ttu-id="fdf43-168">Établit une correspondance avec n’importe quel élément au niveau de dossier actuel, avec n’importe quel nom de fichier ou avec n’importe quelle extension de fichier.</span><span class="sxs-lookup"><span data-stu-id="fdf43-168">Matches anything at the current folder level, or any filename, or any file extension.</span></span> <span data-ttu-id="fdf43-169">Les correspondances sont terminées par des caractères `/` et `.` dans le chemin des fichiers.</span><span class="sxs-lookup"><span data-stu-id="fdf43-169">Matches are terminated by `/` and `.` characters in the file path.</span></span>

<strong><code>**</code></strong>

   <span data-ttu-id="fdf43-170">Établit une correspondance avec n’importe quel élément sur plusieurs niveaux de répertoire.</span><span class="sxs-lookup"><span data-stu-id="fdf43-170">Matches anything across multiple directory levels.</span></span> <span data-ttu-id="fdf43-171">Peut être utilisé pour établir une correspondance avec plusieurs fichiers dans une hiérarchie de répertoires de manière récursive.</span><span class="sxs-lookup"><span data-stu-id="fdf43-171">Can be used to recursively match many files within a directory hierarchy.</span></span>

### <a name="globbing-pattern-examples"></a><span data-ttu-id="fdf43-172">Exemples de modèles d’utilisation des caractères génériques</span><span class="sxs-lookup"><span data-stu-id="fdf43-172">Globbing pattern examples</span></span>

**`directory/file.txt`**

   <span data-ttu-id="fdf43-173">Établit une correspondance avec un fichier spécifique dans un répertoire spécifique.</span><span class="sxs-lookup"><span data-stu-id="fdf43-173">Matches a specific file in a specific directory.</span></span>

**<code>directory/*.txt</code>**

   <span data-ttu-id="fdf43-174">Établit une correspondance avec tous les fichiers ayant l’extension `.txt` dans un répertoire spécifique.</span><span class="sxs-lookup"><span data-stu-id="fdf43-174">Matches all files with `.txt` extension in a specific directory.</span></span>

**`directory/*/bower.json`**

   <span data-ttu-id="fdf43-175">Établit une correspondance avec tous les fichiers `bower.json` dans les répertoires situés exactement un niveau en dessous du répertoire `directory`.</span><span class="sxs-lookup"><span data-stu-id="fdf43-175">Matches all `bower.json` files in directories exactly one level below the `directory` directory.</span></span>

**<code>directory/&#42;&#42;/&#42;.txt</code>**

   <span data-ttu-id="fdf43-176">Établit une correspondance avec tous les fichiers ayant l’extension `.txt` et se trouvant n’importe où sous le répertoire `directory`.</span><span class="sxs-lookup"><span data-stu-id="fdf43-176">Matches all files with `.txt` extension found anywhere under the `directory` directory.</span></span>

## <a name="file-provider-usage-in-aspnet-core"></a><span data-ttu-id="fdf43-177">Utilisation des fournisseurs de fichiers dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="fdf43-177">File Provider usage in ASP.NET Core</span></span>

<span data-ttu-id="fdf43-178">Plusieurs parties d’ASP.NET Core utilisent des fournisseurs de fichiers.</span><span class="sxs-lookup"><span data-stu-id="fdf43-178">Several parts of ASP.NET Core utilize file providers.</span></span> <span data-ttu-id="fdf43-179">`IHostingEnvironment` expose la racine web et la racine de contenu de l’application sous la forme de types `IFileProvider`.</span><span class="sxs-lookup"><span data-stu-id="fdf43-179">`IHostingEnvironment` exposes the app's content root and web root as `IFileProvider` types.</span></span> <span data-ttu-id="fdf43-180">L’intergiciel (middleware) des fichiers statiques utilise des fournisseurs de fichiers pour localiser les fichiers statiques.</span><span class="sxs-lookup"><span data-stu-id="fdf43-180">The static files middleware uses file providers to locate static files.</span></span> <span data-ttu-id="fdf43-181">Razor utilise `IFileProvider` de façon intensive dans la localisation de vues.</span><span class="sxs-lookup"><span data-stu-id="fdf43-181">Razor makes heavy use of `IFileProvider` in locating views.</span></span> <span data-ttu-id="fdf43-182">La fonctionnalité de publication de Dotnet utilise des fournisseurs de fichiers et des modèles d’utilisation des caractères génériques pour spécifier les fichiers qui doivent être publiés.</span><span class="sxs-lookup"><span data-stu-id="fdf43-182">Dotnet's publish functionality uses file providers and globbing patterns to specify which files should be published.</span></span>

## <a name="recommendations-for-use-in-apps"></a><span data-ttu-id="fdf43-183">Recommandations d’utilisation dans des applications</span><span class="sxs-lookup"><span data-stu-id="fdf43-183">Recommendations for use in apps</span></span>

<span data-ttu-id="fdf43-184">Si votre application ASP.NET Core exige un accès au système de fichiers, vous pouvez demander une instance d’`IFileProvider` par le biais d’une injection de dépendances, puis utiliser ses méthodes pour effectuer l’accès, comme illustré dans cet exemple.</span><span class="sxs-lookup"><span data-stu-id="fdf43-184">If your ASP.NET Core app requires file system access, you can request an instance of `IFileProvider` through dependency injection, and then use its methods to perform the access, as shown in this sample.</span></span> <span data-ttu-id="fdf43-185">Cela vous permet de configurer le fournisseur une seule fois, au démarrage de l’application, et réduit le nombre de types d’implémentation que votre application instancie.</span><span class="sxs-lookup"><span data-stu-id="fdf43-185">This allows you to configure the provider once, when the app starts up, and reduces the number of implementation types your app instantiates.</span></span>
