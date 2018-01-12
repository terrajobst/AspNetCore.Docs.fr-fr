---
title: Fournisseurs de fichier dans ASP.NET Core
author: ardalis
description: "Découvrez comment ASP.NET Core résume les accès au système de fichiers via l’utilisation de fournisseurs de fichier."
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 02/14/2017
ms.topic: article
ms.assetid: 1e35d362-0005-4f84-a187-274ca203a787
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/file-providers
ms.openlocfilehash: fd847db992b20ab096b54378418d2b9bccff67be
ms.sourcegitcommit: 2b263e87217658caa42eedc4f9d2d21ef0ab5d59
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/12/2018
---
# <a name="file-providers-in-aspnet-core"></a><span data-ttu-id="478bb-104">Fournisseurs de fichier dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="478bb-104">File Providers in ASP.NET Core</span></span>

<span data-ttu-id="478bb-105">Par [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="478bb-105">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="478bb-106">ASP.NET Core résume l’accès au système de fichiers via l’utilisation de fournisseurs de fichier.</span><span class="sxs-lookup"><span data-stu-id="478bb-106">ASP.NET Core abstracts file system access through the use of File Providers.</span></span>

<span data-ttu-id="478bb-107">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/file-providers/sample) ([procédure de téléchargement](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="478bb-107">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/file-providers/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="file-provider-abstractions"></a><span data-ttu-id="478bb-108">Abstractions de fournisseur de fichier</span><span class="sxs-lookup"><span data-stu-id="478bb-108">File Provider abstractions</span></span>

<span data-ttu-id="478bb-109">Les fournisseurs de fichier sont une abstraction sur les systèmes de fichiers.</span><span class="sxs-lookup"><span data-stu-id="478bb-109">File Providers are an abstraction over file systems.</span></span> <span data-ttu-id="478bb-110">L’interface principale est `IFileProvider`.</span><span class="sxs-lookup"><span data-stu-id="478bb-110">The main interface is `IFileProvider`.</span></span> <span data-ttu-id="478bb-111">`IFileProvider`expose des méthodes pour obtenir des informations de fichier (`IFileInfo`), les informations de répertoire (`IDirectoryContents`) et pour configurer des notifications de modification (à l’aide un `IChangeToken`).</span><span class="sxs-lookup"><span data-stu-id="478bb-111">`IFileProvider` exposes methods to get file information (`IFileInfo`), directory information (`IDirectoryContents`), and to set up change notifications (using an `IChangeToken`).</span></span>

<span data-ttu-id="478bb-112">`IFileInfo`Fournit des méthodes et propriétés sur des fichiers individuels ou des répertoires.</span><span class="sxs-lookup"><span data-stu-id="478bb-112">`IFileInfo` provides methods and properties about individual files or directories.</span></span> <span data-ttu-id="478bb-113">Il a deux propriétés booléennes, `Exists` et `IsDirectory`, ainsi que les propriétés de description du fichier `Name`, `Length` (en octets), et `LastModified` date.</span><span class="sxs-lookup"><span data-stu-id="478bb-113">It has two boolean properties, `Exists` and `IsDirectory`, as well as properties describing the file's `Name`, `Length` (in bytes), and `LastModified` date.</span></span> <span data-ttu-id="478bb-114">Vous pouvez lire à partir du fichier en utilisant son `CreateReadStream` (méthode).</span><span class="sxs-lookup"><span data-stu-id="478bb-114">You can read from the file using its `CreateReadStream` method.</span></span>

## <a name="file-provider-implementations"></a><span data-ttu-id="478bb-115">Implémentations de fournisseur de fichiers</span><span class="sxs-lookup"><span data-stu-id="478bb-115">File Provider implementations</span></span>

<span data-ttu-id="478bb-116">Trois implémentations de `IFileProvider` sont disponibles : physique, incorporé et Composite.</span><span class="sxs-lookup"><span data-stu-id="478bb-116">Three implementations of `IFileProvider` are available: Physical, Embedded, and Composite.</span></span> <span data-ttu-id="478bb-117">Le fournisseur physique permet d’accéder aux fichiers du système réel.</span><span class="sxs-lookup"><span data-stu-id="478bb-117">The physical provider is used to access the actual system's files.</span></span> <span data-ttu-id="478bb-118">Le fournisseur incorporé est utilisé pour accéder aux fichiers incorporés dans des assemblys.</span><span class="sxs-lookup"><span data-stu-id="478bb-118">The embedded provider is used to access files embedded in assemblies.</span></span> <span data-ttu-id="478bb-119">Le fournisseur composite est utilisé pour fournir l’accès aux fichiers et répertoires à partir d’un ou plusieurs fournisseurs.</span><span class="sxs-lookup"><span data-stu-id="478bb-119">The composite provider is used to provide combined access to files and directories from one or more other providers.</span></span>

### <a name="physicalfileprovider"></a><span data-ttu-id="478bb-120">PhysicalFileProvider</span><span class="sxs-lookup"><span data-stu-id="478bb-120">PhysicalFileProvider</span></span>

<span data-ttu-id="478bb-121">Le `PhysicalFileProvider` fournit l’accès au système de fichiers physique.</span><span class="sxs-lookup"><span data-stu-id="478bb-121">The `PhysicalFileProvider` provides access to the physical file system.</span></span> <span data-ttu-id="478bb-122">Elle encapsule le `System.IO.File` type (pour le fournisseur physique), l’étendue de tous les chemins d’accès à un répertoire et de ses enfants.</span><span class="sxs-lookup"><span data-stu-id="478bb-122">It wraps the `System.IO.File` type (for the physical provider), scoping all paths to a directory and its children.</span></span> <span data-ttu-id="478bb-123">Cette portée limite l’accès à un répertoire particulier et ses enfants, ce qui empêche l’accès au système de fichiers en dehors de cette limite.</span><span class="sxs-lookup"><span data-stu-id="478bb-123">This scoping limits access to a certain directory and its children, preventing access to the file system outside of this boundary.</span></span> <span data-ttu-id="478bb-124">Lors de l’instanciation de ce fournisseur, vous devez fournir un chemin de répertoire qui sert de chemin de base pour toutes les demandes effectuées auprès de ce fournisseur (ce qui limite l’accès en dehors de ce chemin d’accès).</span><span class="sxs-lookup"><span data-stu-id="478bb-124">When instantiating this provider, you must provide it with a directory path, which serves as the base path for all requests made to this provider (and which restricts access outside of this path).</span></span> <span data-ttu-id="478bb-125">Dans une application ASP.NET Core, vous pouvez instancier un `PhysicalFileProvider` fournisseur directement, ou vous pouvez demander un `IFileProvider` dans un contrôleur ou le constructeur du service via [injection de dépendance](dependency-injection.md).</span><span class="sxs-lookup"><span data-stu-id="478bb-125">In an ASP.NET Core app, you can instantiate a `PhysicalFileProvider` provider directly, or you can request an `IFileProvider` in a Controller or service's constructor through [dependency injection](dependency-injection.md).</span></span> <span data-ttu-id="478bb-126">Cette dernière approche génère habituellement une solution plus souple et testable.</span><span class="sxs-lookup"><span data-stu-id="478bb-126">The latter approach will typically yield a more flexible and testable solution.</span></span>

<span data-ttu-id="478bb-127">L’exemple ci-dessous montre comment créer un `PhysicalFileProvider`.</span><span class="sxs-lookup"><span data-stu-id="478bb-127">The sample below shows how to create a `PhysicalFileProvider`.</span></span>


```csharp
IFileProvider provider = new PhysicalFileProvider(applicationRoot);
IDirectoryContents contents = provider.GetDirectoryContents(""); // the applicationRoot contents
IFileInfo fileInfo = provider.GetFileInfo("wwwroot/js/site.js"); // a file under applicationRoot
```

<span data-ttu-id="478bb-128">Vous pouvez parcourir le contenu du répertoire ou obtenir des informations d’un fichier spécifique en fournissant un sous-chemin.</span><span class="sxs-lookup"><span data-stu-id="478bb-128">You can iterate through its directory contents or get a specific file's information by providing a subpath.</span></span>

<span data-ttu-id="478bb-129">Pour demander un fournisseur à partir d’un contrôleur, spécifiez-le dans le constructeur du contrôleur et l’assigner à un champ local.</span><span class="sxs-lookup"><span data-stu-id="478bb-129">To request a provider from a controller, specify it in the controller's constructor and assign it to a local field.</span></span> <span data-ttu-id="478bb-130">Utilisez l’instance locale de vos méthodes d’action :</span><span class="sxs-lookup"><span data-stu-id="478bb-130">Use the local instance from your action methods:</span></span>

[!code-csharp[Main](file-providers/sample/src/FileProviderSample/Controllers/HomeController.cs?highlight=5,7,12&range=6-19)]

<span data-ttu-id="478bb-131">Ensuite, créez le fournisseur dans l’application `Startup` classe :</span><span class="sxs-lookup"><span data-stu-id="478bb-131">Then, create the provider in the app's `Startup` class:</span></span>

[!code-csharp[Main](file-providers/sample/src/FileProviderSample/Startup.cs?highlight=35,40&range=1-43)]

<span data-ttu-id="478bb-132">Dans le *Index.cshtml* afficher, parcourir le `IDirectoryContents` fourni :</span><span class="sxs-lookup"><span data-stu-id="478bb-132">In the *Index.cshtml* view, iterate through the `IDirectoryContents` provided:</span></span>

[!code-html[Main](file-providers/sample/src/FileProviderSample/Views/Home/Index.cshtml?highlight=2,7,9,11,15)]

<span data-ttu-id="478bb-133">Le résultat :</span><span class="sxs-lookup"><span data-stu-id="478bb-133">The result:</span></span>

![Fournisseur exemples application liste physique de fichiers et des dossiers de fichiers](file-providers/_static/physical-directory-listing.png)

### <a name="embeddedfileprovider"></a><span data-ttu-id="478bb-135">EmbeddedFileProvider</span><span class="sxs-lookup"><span data-stu-id="478bb-135">EmbeddedFileProvider</span></span>

<span data-ttu-id="478bb-136">Le `EmbeddedFileProvider` est utilisé pour accéder aux fichiers incorporés dans des assemblys.</span><span class="sxs-lookup"><span data-stu-id="478bb-136">The `EmbeddedFileProvider` is used to access files embedded in assemblies.</span></span> <span data-ttu-id="478bb-137">Dans le .NET Core, vous incorporez des fichiers dans un assembly avec le `<EmbeddedResource>` élément dans le *.csproj* fichier :</span><span class="sxs-lookup"><span data-stu-id="478bb-137">In .NET Core, you embed files in an assembly with the `<EmbeddedResource>` element in the *.csproj* file:</span></span>

[!code-json[Main](file-providers/sample/src/FileProviderSample/FileProviderSample.csproj?range=13-18)]

<span data-ttu-id="478bb-138">Vous pouvez utiliser [les modèles de la globalisation](#globbing-patterns) lors de la spécification des fichiers à incorporer dans l’assembly.</span><span class="sxs-lookup"><span data-stu-id="478bb-138">You can use [globbing patterns](#globbing-patterns) when specifying files to embed in the assembly.</span></span> <span data-ttu-id="478bb-139">Ces modèles peuvent être utilisés pour faire correspondre un ou plusieurs fichiers.</span><span class="sxs-lookup"><span data-stu-id="478bb-139">These patterns can be used to match one or more files.</span></span>

> [!NOTE]
> <span data-ttu-id="478bb-140">Il est improbable que vous souhaitez jamais réellement incorporer chaque fichier .js dans votre projet dans l’assembly ; l’exemple ci-dessus est uniquement à des fins de démonstration.</span><span class="sxs-lookup"><span data-stu-id="478bb-140">It's unlikely you would ever want to actually embed every .js file in your project in its assembly; the above sample is for demo purposes only.</span></span>

<span data-ttu-id="478bb-141">Lorsque vous créez un `EmbeddedFileProvider`, passez l’assembly il lira à son constructeur.</span><span class="sxs-lookup"><span data-stu-id="478bb-141">When creating an `EmbeddedFileProvider`, pass the assembly it will read to its constructor.</span></span>

```csharp
var embeddedProvider = new EmbeddedFileProvider(Assembly.GetEntryAssembly());
```

<span data-ttu-id="478bb-142">L’extrait de code ci-dessus montre comment créer un `EmbeddedFileProvider` avec accès à l’assembly en cours d’exécution.</span><span class="sxs-lookup"><span data-stu-id="478bb-142">The snippet above demonstrates how to create an `EmbeddedFileProvider` with access to the currently executing assembly.</span></span>

<span data-ttu-id="478bb-143">Mise à jour de l’exemple d’application à utiliser un `EmbeddedFileProvider` génère la sortie suivante :</span><span class="sxs-lookup"><span data-stu-id="478bb-143">Updating the sample app to use an `EmbeddedFileProvider` results in the following output:</span></span>

![Fichier d’application d’exemple de fournisseur qui répertorie les fichiers incorporés](file-providers/_static/embedded-directory-listing.png)

> [!NOTE]
> <span data-ttu-id="478bb-145">Les ressources incorporées n’exposent pas les répertoires.</span><span class="sxs-lookup"><span data-stu-id="478bb-145">Embedded resources do not expose directories.</span></span> <span data-ttu-id="478bb-146">Au lieu de cela, le chemin d’accès à la ressource (par l’intermédiaire de son espace de noms) est incorporé dans sa à l’aide du nom de fichier `.` séparateurs.</span><span class="sxs-lookup"><span data-stu-id="478bb-146">Rather, the path to the resource (via its namespace) is embedded in its filename using `.` separators.</span></span>

> [!TIP]
> <span data-ttu-id="478bb-147">Le `EmbeddedFileProvider` constructeur accepte un facultatif `baseNamespace` paramètre.</span><span class="sxs-lookup"><span data-stu-id="478bb-147">The `EmbeddedFileProvider` constructor accepts an optional `baseNamespace` parameter.</span></span> <span data-ttu-id="478bb-148">Les appels à la spécification de ce étend sa portée `GetDirectoryContents` à ces ressources sous l’espace de noms fourni.</span><span class="sxs-lookup"><span data-stu-id="478bb-148">Specifying this will scope calls to `GetDirectoryContents` to those resources under the provided namespace.</span></span>

### <a name="compositefileprovider"></a><span data-ttu-id="478bb-149">CompositeFileProvider</span><span class="sxs-lookup"><span data-stu-id="478bb-149">CompositeFileProvider</span></span>

<span data-ttu-id="478bb-150">Le `CompositeFileProvider` combine `IFileProvider` instances, l’exposition d’une interface unique pour l’utilisation des fichiers à partir de plusieurs fournisseurs.</span><span class="sxs-lookup"><span data-stu-id="478bb-150">The `CompositeFileProvider` combines `IFileProvider` instances, exposing a single interface for working with files from multiple providers.</span></span> <span data-ttu-id="478bb-151">Lorsque vous créez le `CompositeFileProvider`, vous passez un ou plusieurs `IFileProvider` instances à son constructeur :</span><span class="sxs-lookup"><span data-stu-id="478bb-151">When creating the `CompositeFileProvider`, you pass one or more `IFileProvider` instances to its constructor:</span></span>

[!code-csharp[Main](file-providers/sample/src/FileProviderSample/Startup.cs?highlight=3&range=35-37)]

<span data-ttu-id="478bb-152">Mise à jour de l’exemple d’application à utiliser un `CompositeFileProvider` qui inclut les deux fournisseurs physiques et incorporés configurées précédemment, génère la sortie suivante :</span><span class="sxs-lookup"><span data-stu-id="478bb-152">Updating the sample app to use a `CompositeFileProvider` that includes both the physical and embedded providers configured previously, results in the following output:</span></span>

![Fichier fournisseur exemple d’application qui répertorie les fichiers physiques et des dossiers et des fichiers incorporés](file-providers/_static/composite-directory-listing.png)

## <a name="watching-for-changes"></a><span data-ttu-id="478bb-154">La surveillance des modifications</span><span class="sxs-lookup"><span data-stu-id="478bb-154">Watching for changes</span></span>

<span data-ttu-id="478bb-155">Le `IFileProvider` `Watch` méthode offre un moyen d’observer un ou plusieurs fichiers ou répertoires pour les modifications.</span><span class="sxs-lookup"><span data-stu-id="478bb-155">The `IFileProvider` `Watch` method provides a way to watch one or more files or directories for changes.</span></span> <span data-ttu-id="478bb-156">Cette méthode accepte une chaîne de chemin d’accès, ce qui permet de [les modèles de la globalisation](#globbing-patterns) pour spécifier plusieurs fichiers et retourne un `IChangeToken`.</span><span class="sxs-lookup"><span data-stu-id="478bb-156">This method accepts a path string, which can use [globbing patterns](#globbing-patterns) to specify multiple files, and returns an `IChangeToken`.</span></span> <span data-ttu-id="478bb-157">Ce jeton expose un `HasChanged` propriété qui peut être inspectée, et un `RegisterChangeCallback` méthode qui est appelée lorsque des modifications sont détectées dans la chaîne de chemin d’accès spécifié.</span><span class="sxs-lookup"><span data-stu-id="478bb-157">This token exposes a `HasChanged` property that can be inspected, and a `RegisterChangeCallback` method that is called when changes are detected to the specified path string.</span></span> <span data-ttu-id="478bb-158">Notez que chaque jeton de modification appelle uniquement son rappel associé en réponse à une modification.</span><span class="sxs-lookup"><span data-stu-id="478bb-158">Note that each change token only calls its associated callback in response to a single change.</span></span> <span data-ttu-id="478bb-159">Pour activer une surveillance permanente, vous pouvez utiliser un `TaskCompletionSource` comme indiqué ci-dessous, ou recréez `IChangeToken` instances en réponse aux modifications.</span><span class="sxs-lookup"><span data-stu-id="478bb-159">To enable constant monitoring, you can use a `TaskCompletionSource` as shown below, or re-create `IChangeToken` instances in response to changes.</span></span>

<span data-ttu-id="478bb-160">Dans l’exemple de cet article, une application console est configurée pour afficher un message chaque fois qu’un fichier texte est modifié :</span><span class="sxs-lookup"><span data-stu-id="478bb-160">In this article's sample, a console application is configured to display a message whenever a text file is modified:</span></span>

[!code-csharp[Main](file-providers/sample/src/WatchConsole/Program.cs?name=snippet1&highlight=1-2,16,19-20)]

<span data-ttu-id="478bb-161">Le résultat, après avoir enregistré le fichier plusieurs fois :</span><span class="sxs-lookup"><span data-stu-id="478bb-161">The result, after saving the file several times:</span></span>

![Fenêtre de commande une fois l’exécution dotnet exécuter montre les modifications du fichier quotes.txt d’analyse des applications et que le fichier a changé de cinq fois.](file-providers/_static/watch-console.png)

> [!NOTE]
> <span data-ttu-id="478bb-163">Certains systèmes de fichiers, tels que les conteneurs Docker et les partages réseau, peut ne pas fiable à envoyer des notifications de modification.</span><span class="sxs-lookup"><span data-stu-id="478bb-163">Some file systems, such as Docker containers and network shares, may not reliably send change notifications.</span></span> <span data-ttu-id="478bb-164">Définir le `DOTNET_USE_POLLINGFILEWATCHER` variable d’environnement `1` ou `true` pour interroger le système de fichiers pour les modifications à toutes les 4 secondes.</span><span class="sxs-lookup"><span data-stu-id="478bb-164">Set the `DOTNET_USE_POLLINGFILEWATCHER` environment variable to `1` or `true` to poll the file system for changes every 4 seconds.</span></span>

## <a name="globbing-patterns"></a><span data-ttu-id="478bb-165">Modèles de la globalisation</span><span class="sxs-lookup"><span data-stu-id="478bb-165">Globbing patterns</span></span>

<span data-ttu-id="478bb-166">Chemins d’accès de système de fichiers utilisent des modèles à caractère générique appelées *les modèles de la globalisation*.</span><span class="sxs-lookup"><span data-stu-id="478bb-166">File system paths use wildcard patterns called *globbing patterns*.</span></span> <span data-ttu-id="478bb-167">Ces modèles simples peuvent être utilisés pour spécifier les groupes de fichiers.</span><span class="sxs-lookup"><span data-stu-id="478bb-167">These simple patterns can be used to specify groups of files.</span></span> <span data-ttu-id="478bb-168">Les deux caractères génériques sont `*` et `**`.</span><span class="sxs-lookup"><span data-stu-id="478bb-168">The two wildcard characters are `*` and `**`.</span></span>

**`*`**

   <span data-ttu-id="478bb-169">Correspond à quoi que ce soit au niveau du dossier actuel, ou n’importe quel nom de fichier ou de n’importe quelle extension de fichier.</span><span class="sxs-lookup"><span data-stu-id="478bb-169">Matches anything at the current folder level, or any filename, or any file extension.</span></span> <span data-ttu-id="478bb-170">Correspondances sont terminent par `/` et `.` caractères dans le chemin d’accès de fichier.</span><span class="sxs-lookup"><span data-stu-id="478bb-170">Matches are terminated by `/` and `.` characters in the file path.</span></span>

<strong><code>**</code></strong>

   <span data-ttu-id="478bb-171">Met en correspondance n’est pas défini sur plusieurs niveaux de répertoire.</span><span class="sxs-lookup"><span data-stu-id="478bb-171">Matches anything across multiple directory levels.</span></span> <span data-ttu-id="478bb-172">Peut être utilisé pour de manière récursive correspond à plusieurs fichiers au sein d’une hiérarchie de répertoires.</span><span class="sxs-lookup"><span data-stu-id="478bb-172">Can be used to recursively match many files within a directory hierarchy.</span></span>

### <a name="globbing-pattern-examples"></a><span data-ttu-id="478bb-173">Exemples de modèle de la globalisation</span><span class="sxs-lookup"><span data-stu-id="478bb-173">Globbing pattern examples</span></span>

**`directory/file.txt`**

   <span data-ttu-id="478bb-174">Correspond à un fichier spécifique dans un répertoire spécifique.</span><span class="sxs-lookup"><span data-stu-id="478bb-174">Matches a specific file in a specific directory.</span></span>

**<code>directory/*.txt</code>**

   <span data-ttu-id="478bb-175">Correspond à tous les fichiers avec `.txt` extension dans un répertoire spécifique.</span><span class="sxs-lookup"><span data-stu-id="478bb-175">Matches all files with `.txt` extension in a specific directory.</span></span>

**`directory/*/bower.json`**

   <span data-ttu-id="478bb-176">Correspond à toutes les `bower.json` fichiers dans les répertoires exactement un niveau en dessous du `directory` active.</span><span class="sxs-lookup"><span data-stu-id="478bb-176">Matches all `bower.json` files in directories exactly one level below the `directory` directory.</span></span>

**<code>directory/&#42;&#42;/&#42;.txt</code>**

   <span data-ttu-id="478bb-177">Correspond à tous les fichiers avec `.txt` extension se trouve sous le `directory` active.</span><span class="sxs-lookup"><span data-stu-id="478bb-177">Matches all files with `.txt` extension found anywhere under the `directory` directory.</span></span>

## <a name="file-provider-usage-in-aspnet-core"></a><span data-ttu-id="478bb-178">L’utilisation du fournisseur de fichiers dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="478bb-178">File Provider usage in ASP.NET Core</span></span>

<span data-ttu-id="478bb-179">Plusieurs parties d’ASP.NET Core utilisent des fournisseurs de fichier.</span><span class="sxs-lookup"><span data-stu-id="478bb-179">Several parts of ASP.NET Core utilize file providers.</span></span> <span data-ttu-id="478bb-180">`IHostingEnvironment`expose la racine du contenu de l’application et la racine web en tant que `IFileProvider` types.</span><span class="sxs-lookup"><span data-stu-id="478bb-180">`IHostingEnvironment` exposes the app's content root and web root as `IFileProvider` types.</span></span> <span data-ttu-id="478bb-181">L’intergiciel (middleware) des fichiers statiques utilise des fournisseurs de fichier pour localiser les fichiers statiques.</span><span class="sxs-lookup"><span data-stu-id="478bb-181">The static files middleware uses file providers to locate static files.</span></span> <span data-ttu-id="478bb-182">Razor utilise de façon intensive `IFileProvider` pour la localisation de vues.</span><span class="sxs-lookup"><span data-stu-id="478bb-182">Razor makes heavy use of `IFileProvider` in locating views.</span></span> <span data-ttu-id="478bb-183">Dotnet publie des fournisseurs de fichier utilise des fonctionnalités et les modèles de la globalisation pour spécifier les fichiers qui doivent être publiés.</span><span class="sxs-lookup"><span data-stu-id="478bb-183">Dotnet's publish functionality uses file providers and globbing patterns to specify which files should be published.</span></span>

## <a name="recommendations-for-use-in-apps"></a><span data-ttu-id="478bb-184">Recommandations pour une utilisation dans les applications</span><span class="sxs-lookup"><span data-stu-id="478bb-184">Recommendations for use in apps</span></span>

<span data-ttu-id="478bb-185">Si votre application ASP.NET Core nécessite l’accès au système de fichiers, vous pouvez demander une instance de `IFileProvider` par injection de dépendance, puis utilisez ses méthodes pour y accéder, comme illustré dans cet exemple.</span><span class="sxs-lookup"><span data-stu-id="478bb-185">If your ASP.NET Core app requires file system access, you can request an instance of `IFileProvider` through dependency injection, and then use its methods to perform the access, as shown in this sample.</span></span> <span data-ttu-id="478bb-186">Cela vous permet de vous permet de configurer le fournisseur d’une seule fois, lorsque l’application démarre et réduit le nombre de types d’implémentation instancie de votre application.</span><span class="sxs-lookup"><span data-stu-id="478bb-186">This allows you to configure the provider once, when the app starts up, and reduces the number of implementation types your app instantiates.</span></span>
