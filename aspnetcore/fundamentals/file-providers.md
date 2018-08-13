---
title: Fournisseurs de fichiers dans ASP.NET Core
author: guardrex
description: Découvrez comment ASP.NET Core fournit un accès au système de fichiers en utilisant des fournisseurs de fichiers.
ms.author: riande
ms.custom: mvc
ms.date: 08/01/2018
uid: fundamentals/file-providers
ms.openlocfilehash: 512229cfe7d7efdcd9050fa13dbdbf793be29a0b
ms.sourcegitcommit: 571d76fbbff05e84406b6d909c8fe9cbea2c8ff1
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/01/2018
ms.locfileid: "39410154"
---
# <a name="file-providers-in-aspnet-core"></a><span data-ttu-id="6c667-103">Fournisseurs de fichiers dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="6c667-103">File Providers in ASP.NET Core</span></span>

<span data-ttu-id="6c667-104">Par [Steve Smith](https://ardalis.com/) et [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="6c667-104">By [Steve Smith](https://ardalis.com/) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="6c667-105">ASP.NET Core fournit un accès au système de fichiers en utilisant des fournisseurs de fichiers.</span><span class="sxs-lookup"><span data-stu-id="6c667-105">ASP.NET Core abstracts file system access through the use of File Providers.</span></span> <span data-ttu-id="6c667-106">Des fournisseurs de fichiers sont utilisés dans l’infrastructure ASP.NET Core :</span><span class="sxs-lookup"><span data-stu-id="6c667-106">File Providers are used throughout the ASP.NET Core framework:</span></span>

* <span data-ttu-id="6c667-107">[IHostingEnvironment](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment) expose la racine web et la racine de contenu de l’application sous la forme de types `IFileProvider`.</span><span class="sxs-lookup"><span data-stu-id="6c667-107">[IHostingEnvironment](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment) exposes the app's content root and web root as `IFileProvider` types.</span></span>
* <span data-ttu-id="6c667-108">[L’intergiciel (middleware) des fichiers statiques](xref:fundamentals/static-files) utilise des fournisseurs de fichiers pour localiser les fichiers statiques.</span><span class="sxs-lookup"><span data-stu-id="6c667-108">[Static Files Middleware](xref:fundamentals/static-files) uses File Providers to locate static files.</span></span>
* <span data-ttu-id="6c667-109">[Razor](xref:mvc/views/razor) utilise des fournisseurs de fichiers pour localiser des pages et des vues.</span><span class="sxs-lookup"><span data-stu-id="6c667-109">[Razor](xref:mvc/views/razor) uses File Providers to locate pages and views.</span></span>
* <span data-ttu-id="6c667-110">Les outils .NET Core utilisent des fournisseurs de fichiers et des modèles d’utilisation des caractères génériques pour spécifier les fichiers qui doivent être publiés.</span><span class="sxs-lookup"><span data-stu-id="6c667-110">.NET Core tooling uses File Providers and glob patterns to specify which files should be published.</span></span>

<span data-ttu-id="6c667-111">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/file-providers/samples) ([procédure de téléchargement](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="6c667-111">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/file-providers/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="file-provider-interfaces"></a><span data-ttu-id="6c667-112">Interfaces de fournisseur de fichiers</span><span class="sxs-lookup"><span data-stu-id="6c667-112">File Provider interfaces</span></span>

<span data-ttu-id="6c667-113">L’interface principale est [IFileProvider](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider).</span><span class="sxs-lookup"><span data-stu-id="6c667-113">The primary interface is [IFileProvider](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider).</span></span> <span data-ttu-id="6c667-114">`IFileProvider` expose des méthodes pour :</span><span class="sxs-lookup"><span data-stu-id="6c667-114">`IFileProvider` exposes methods to:</span></span>

* <span data-ttu-id="6c667-115">Obtenir des informations sur le fichier ([IFileInfo](/dotnet/api/microsoft.extensions.fileproviders.ifileinfo)).</span><span class="sxs-lookup"><span data-stu-id="6c667-115">Obtain file information ([IFileInfo](/dotnet/api/microsoft.extensions.fileproviders.ifileinfo)).</span></span>
* <span data-ttu-id="6c667-116">Obtenir des informations sur le répertoire ([IDirectoryContents](/dotnet/api/microsoft.extensions.fileproviders.idirectorycontents)).</span><span class="sxs-lookup"><span data-stu-id="6c667-116">Obtain directory information ([IDirectoryContents](/dotnet/api/microsoft.extensions.fileproviders.idirectorycontents)).</span></span>
* <span data-ttu-id="6c667-117">Configurer des notifications de modification (à l’aide un [IChangeToken](/dotnet/api/microsoft.extensions.primitives.ichangetoken)).</span><span class="sxs-lookup"><span data-stu-id="6c667-117">Set up change notifications (using an [IChangeToken](/dotnet/api/microsoft.extensions.primitives.ichangetoken)).</span></span>

<span data-ttu-id="6c667-118">`IFileInfo` fournit des méthodes et propriétés d’utilisation des fichiers :</span><span class="sxs-lookup"><span data-stu-id="6c667-118">`IFileInfo` provides methods and properties for working with files:</span></span>

* [<span data-ttu-id="6c667-119">Existe</span><span class="sxs-lookup"><span data-stu-id="6c667-119">Exists</span></span>](/dotnet/api/microsoft.extensions.fileproviders.ifileinfo.exists)
* [<span data-ttu-id="6c667-120">IsDirectory</span><span class="sxs-lookup"><span data-stu-id="6c667-120">IsDirectory</span></span>](/dotnet/api/microsoft.extensions.fileproviders.ifileinfo.isdirectory)
* [<span data-ttu-id="6c667-121">Nom</span><span class="sxs-lookup"><span data-stu-id="6c667-121">Name</span></span>](/dotnet/api/microsoft.extensions.fileproviders.ifileinfo.name)
* <span data-ttu-id="6c667-122">[Longueur](/dotnet/api/microsoft.extensions.fileproviders.ifileinfo.length) (en octets)</span><span class="sxs-lookup"><span data-stu-id="6c667-122">[Length](/dotnet/api/microsoft.extensions.fileproviders.ifileinfo.length) (in bytes)</span></span>
* <span data-ttu-id="6c667-123">Date [LastModified](/dotnet/api/microsoft.extensions.fileproviders.ifileinfo.lastmodified)</span><span class="sxs-lookup"><span data-stu-id="6c667-123">[LastModified](/dotnet/api/microsoft.extensions.fileproviders.ifileinfo.lastmodified) date</span></span>

<span data-ttu-id="6c667-124">Vous pouvez lire à partir du fichier en utilisant la méthode [IFileInfo.CreateReadStream](/dotnet/api/microsoft.extensions.fileproviders.ifileinfo.createreadstream).</span><span class="sxs-lookup"><span data-stu-id="6c667-124">You can read from the file using the [IFileInfo.CreateReadStream](/dotnet/api/microsoft.extensions.fileproviders.ifileinfo.createreadstream) method.</span></span>

<span data-ttu-id="6c667-125">L’exemple d’application montre comment configurer un fournisseur de fichiers dans `Startup.ConfigureServices` pour une utilisation dans toute l’application via [l’injection de dépendances](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="6c667-125">The sample app demonstrates how to configure a File Provider in `Startup.ConfigureServices` for use throughout the app via [dependency injection](xref:fundamentals/dependency-injection).</span></span>

## <a name="file-provider-implementations"></a><span data-ttu-id="6c667-126">Implémentations de fournisseur de fichiers</span><span class="sxs-lookup"><span data-stu-id="6c667-126">File Provider implementations</span></span>

<span data-ttu-id="6c667-127">Trois implémentations de `IFileProvider` sont disponibles.</span><span class="sxs-lookup"><span data-stu-id="6c667-127">Three implementations of `IFileProvider` are available.</span></span>

::: moniker range=">= aspnetcore-2.0"

| <span data-ttu-id="6c667-128">Implémentation</span><span class="sxs-lookup"><span data-stu-id="6c667-128">Implementation</span></span> | <span data-ttu-id="6c667-129">Description</span><span class="sxs-lookup"><span data-stu-id="6c667-129">Description</span></span> |
| -------------- | ----------- |
| [<span data-ttu-id="6c667-130">PhysicalFileProvider</span><span class="sxs-lookup"><span data-stu-id="6c667-130">PhysicalFileProvider</span></span>](#physicalfileprovider) | <span data-ttu-id="6c667-131">Le fournisseur physique est utilisé pour accéder aux fichiers physiques du système.</span><span class="sxs-lookup"><span data-stu-id="6c667-131">The physical provider is used to access the system's physical files.</span></span> |
| [<span data-ttu-id="6c667-132">ManifestEmbeddedFileProvider</span><span class="sxs-lookup"><span data-stu-id="6c667-132">ManifestEmbeddedFileProvider</span></span>](#manifestembeddedfileprovider) | <span data-ttu-id="6c667-133">Le fournisseur incorporé de manifeste est utilisé pour accéder à des fichiers incorporés dans des assemblys.</span><span class="sxs-lookup"><span data-stu-id="6c667-133">The manifest embedded provider is used to access files embedded in assemblies.</span></span> |
| [<span data-ttu-id="6c667-134">CompositeFileProvider</span><span class="sxs-lookup"><span data-stu-id="6c667-134">CompositeFileProvider</span></span>](#compositefileprovider) | <span data-ttu-id="6c667-135">Le fournisseur composite est utilisé pour fournir un accès combiné à des fichiers et à des répertoires à partir d’un ou de plusieurs fournisseurs.</span><span class="sxs-lookup"><span data-stu-id="6c667-135">The composite provider is used to provide combined access to files and directories from one or more other providers.</span></span> |

::: moniker-end

::: moniker range="< aspnetcore-2.0"

| <span data-ttu-id="6c667-136">Implémentation</span><span class="sxs-lookup"><span data-stu-id="6c667-136">Implementation</span></span> | <span data-ttu-id="6c667-137">Description</span><span class="sxs-lookup"><span data-stu-id="6c667-137">Description</span></span> |
| -------------- | ----------- |
| [<span data-ttu-id="6c667-138">PhysicalFileProvider</span><span class="sxs-lookup"><span data-stu-id="6c667-138">PhysicalFileProvider</span></span>](#physicalfileprovider) | <span data-ttu-id="6c667-139">Le fournisseur physique est utilisé pour accéder aux fichiers physiques du système.</span><span class="sxs-lookup"><span data-stu-id="6c667-139">The physical provider is used to access the system's physical files.</span></span> |
| [<span data-ttu-id="6c667-140">EmbeddedFileProvider</span><span class="sxs-lookup"><span data-stu-id="6c667-140">EmbeddedFileProvider</span></span>](#embeddedfileprovider) | <span data-ttu-id="6c667-141">Le fournisseur incorporé est utilisé pour accéder à des fichiers incorporés dans des assemblys.</span><span class="sxs-lookup"><span data-stu-id="6c667-141">The embedded provider is used to access files embedded in assemblies.</span></span> |
| [<span data-ttu-id="6c667-142">CompositeFileProvider</span><span class="sxs-lookup"><span data-stu-id="6c667-142">CompositeFileProvider</span></span>](#compositefileprovider) | <span data-ttu-id="6c667-143">Le fournisseur composite est utilisé pour fournir un accès combiné à des fichiers et à des répertoires à partir d’un ou de plusieurs fournisseurs.</span><span class="sxs-lookup"><span data-stu-id="6c667-143">The composite provider is used to provide combined access to files and directories from one or more other providers.</span></span> |

::: moniker-end

### <a name="physicalfileprovider"></a><span data-ttu-id="6c667-144">PhysicalFileProvider</span><span class="sxs-lookup"><span data-stu-id="6c667-144">PhysicalFileProvider</span></span>

<span data-ttu-id="6c667-145">La classe [PhysicalFileProvider](/dotnet/api/microsoft.extensions.fileproviders.physicalfileprovider) fournit un accès au système de fichiers physique.</span><span class="sxs-lookup"><span data-stu-id="6c667-145">The [PhysicalFileProvider](/dotnet/api/microsoft.extensions.fileproviders.physicalfileprovider) provides access to the physical file system.</span></span> <span data-ttu-id="6c667-146">`PhysicalFileProvider` utilise le type [System.IO.File](/dotnet/api/system.io.file) (pour le fournisseur physique), définissant comme portée tous les chemins à un répertoire et ses enfants.</span><span class="sxs-lookup"><span data-stu-id="6c667-146">`PhysicalFileProvider` uses the [System.IO.File](/dotnet/api/system.io.file) type (for the physical provider) and scopes all paths to a directory and its children.</span></span> <span data-ttu-id="6c667-147">Cette portée empêche l’accès au système de fichiers en dehors du répertoire spécifié et ses enfants.</span><span class="sxs-lookup"><span data-stu-id="6c667-147">This scoping prevents access to the file system outside of the specified directory and its children.</span></span> <span data-ttu-id="6c667-148">Lors de l’instanciation de ce fournisseur, un chemin de répertoire est obligatoire et sert de chemin de base pour toutes les demandes effectuées à l’aide du fournisseur.</span><span class="sxs-lookup"><span data-stu-id="6c667-148">When instantiating this provider, a directory path is required and serves as the base path for all requests made using the provider.</span></span> <span data-ttu-id="6c667-149">Vous pouvez instancier un fournisseur `PhysicalFileProvider` directement, ou vous pouvez demander un `IFileProvider` dans un constructeur à l’aide de [l’injection de dépendances](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="6c667-149">You can instantiate a `PhysicalFileProvider` provider directly, or you can request an `IFileProvider` in a constructor through [dependency injection](xref:fundamentals/dependency-injection).</span></span>

<span data-ttu-id="6c667-150">**Types statiques**</span><span class="sxs-lookup"><span data-stu-id="6c667-150">**Static types**</span></span>

<span data-ttu-id="6c667-151">Le code suivant montre comment créer un `PhysicalFileProvider` et l’utiliser pour obtenir le contenu du répertoire et les informations sur le fichier :</span><span class="sxs-lookup"><span data-stu-id="6c667-151">The following code shows how to create a `PhysicalFileProvider` and use it to obtain directory contents and file information:</span></span>

```csharp
var provider = new PhysicalFileProvider(applicationRoot);
var contents = provider.GetDirectoryContents(string.Empty);
var fileInfo = provider.GetFileInfo("wwwroot/js/site.js");
```

<span data-ttu-id="6c667-152">Types dans l’exemple précédent :</span><span class="sxs-lookup"><span data-stu-id="6c667-152">Types in the preceding example:</span></span>

* <span data-ttu-id="6c667-153">`provider` est un `IFileProvider`.</span><span class="sxs-lookup"><span data-stu-id="6c667-153">`provider` is an `IFileProvider`.</span></span>
* <span data-ttu-id="6c667-154">`contents` est un `IDirectoryContents`.</span><span class="sxs-lookup"><span data-stu-id="6c667-154">`contents` is an `IDirectoryContents`.</span></span>
* <span data-ttu-id="6c667-155">`fileInfo` est un `IFileInfo`.</span><span class="sxs-lookup"><span data-stu-id="6c667-155">`fileInfo` is an `IFileInfo`.</span></span>

<span data-ttu-id="6c667-156">Le fournisseur de fichiers peut être utilisé pour itérer au sein du répertoire spécifié par `applicationRoot` ou pour appeler `GetFileInfo` afin d’obtenir des informations sur un fichier.</span><span class="sxs-lookup"><span data-stu-id="6c667-156">The File Provider can be used to iterate through the directory specified by `applicationRoot` or call `GetFileInfo` to obtain a file's information.</span></span> <span data-ttu-id="6c667-157">Le fournisseur de fichiers n’a pas accès en dehors du répertoire `applicationRoot`.</span><span class="sxs-lookup"><span data-stu-id="6c667-157">The File Provider has no access outside of the `applicationRoot` directory.</span></span>

<span data-ttu-id="6c667-158">L’exemple d’application crée le fournisseur dans la classe `Startup.ConfigureServices` de l’application à l’aide de [IHostingEnvironment.ContentRootFileProvider](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment.contentrootfileprovider) :</span><span class="sxs-lookup"><span data-stu-id="6c667-158">The sample app creates the provider in the app's `Startup.ConfigureServices` class using [IHostingEnvironment.ContentRootFileProvider](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment.contentrootfileprovider):</span></span>

```csharp
var physicalProvider = _env.ContentRootFileProvider;
```

<span data-ttu-id="6c667-159">**Obtenir les types de fournisseurs de fichiers avec l’injection de dépendances**</span><span class="sxs-lookup"><span data-stu-id="6c667-159">**Obtain File Provider types with dependency injection**</span></span>

<span data-ttu-id="6c667-160">Injectez le fournisseur dans n’importe quel constructeur de classe et affectez-le à un champ local.</span><span class="sxs-lookup"><span data-stu-id="6c667-160">Inject the provider into any class constructor and assign it to a local field.</span></span> <span data-ttu-id="6c667-161">Utilisez le champ dans l’ensemble des méthodes de la classe pour accéder aux fichiers.</span><span class="sxs-lookup"><span data-stu-id="6c667-161">Use the field throughout the class's methods to access files.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="6c667-162">Dans l’exemple d’application, la classe `IndexModel` reçoit une instance `IFileProvider` pour obtenir le contenu du répertoire pour le chemin d’accès de la base de l’application.</span><span class="sxs-lookup"><span data-stu-id="6c667-162">In the sample app, the `IndexModel` class receives an `IFileProvider` instance to obtain directory contents for the app's base path.</span></span>

<span data-ttu-id="6c667-163">*Pages/Index.cshtml.cs* :</span><span class="sxs-lookup"><span data-stu-id="6c667-163">*Pages/Index.cshtml.cs*:</span></span>

[!code-csharp[](file-providers/samples/2.x/FileProviderSample/Pages/Index.cshtml.cs?name=snippet1)]

<span data-ttu-id="6c667-164">`IDirectoryContents` sont itérés dans la page.</span><span class="sxs-lookup"><span data-stu-id="6c667-164">The `IDirectoryContents` are iterated in the page.</span></span>

<span data-ttu-id="6c667-165">*Pages/Index.cshtml* :</span><span class="sxs-lookup"><span data-stu-id="6c667-165">*Pages/Index.cshtml*:</span></span>

[!code-cshtml[](file-providers/samples/2.x/FileProviderSample/Pages/Index.cshtml?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="6c667-166">Dans l’exemple d’application, la classe `HomeController` reçoit une instance `IFileProvider` pour obtenir le contenu du répertoire pour le chemin d’accès de la base de l’application.</span><span class="sxs-lookup"><span data-stu-id="6c667-166">In the sample app, the `HomeController` class receives an `IFileProvider` instance to obtain directory contents for the app's base path.</span></span>

<span data-ttu-id="6c667-167">*Controllers/HomeController.cs* :</span><span class="sxs-lookup"><span data-stu-id="6c667-167">*Controllers/HomeController.cs*:</span></span>

[!code-csharp[](file-providers/samples/1.x/FileProviderSample/Controllers/HomeController.cs?name=snippet1)]

<span data-ttu-id="6c667-168">`IDirectoryContents` sont itérés dans la vue.</span><span class="sxs-lookup"><span data-stu-id="6c667-168">The `IDirectoryContents` are iterated in the view.</span></span>

<span data-ttu-id="6c667-169">*Views/Home/Index.cshtml* :</span><span class="sxs-lookup"><span data-stu-id="6c667-169">*Views/Home/Index.cshtml*:</span></span>

[!code-cshtml[](file-providers/samples/1.x/FileProviderSample/Views/Home/Index.cshtml?name=snippet1)]

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

### <a name="manifestembeddedfileprovider"></a><span data-ttu-id="6c667-170">ManifestEmbeddedFileProvider</span><span class="sxs-lookup"><span data-stu-id="6c667-170">ManifestEmbeddedFileProvider</span></span>

<span data-ttu-id="6c667-171">[ManifestEmbeddedFileProvider](/dotnet/api/microsoft.extensions.fileproviders.manifestembeddedfileprovider) est utilisé pour accéder à des fichiers incorporés dans des assemblys.</span><span class="sxs-lookup"><span data-stu-id="6c667-171">The [ManifestEmbeddedFileProvider](/dotnet/api/microsoft.extensions.fileproviders.manifestembeddedfileprovider) is used to access files embedded within assemblies.</span></span> <span data-ttu-id="6c667-172">`ManifestEmbeddedFileProvider` utilise un manifeste compilé dans l’assembly pour reconstruire les chemins d’accès d’origine des fichiers intégrés.</span><span class="sxs-lookup"><span data-stu-id="6c667-172">The `ManifestEmbeddedFileProvider` uses a manifest compiled into the assembly to reconstruct the original paths of the embedded files.</span></span>

> [!NOTE]
> <span data-ttu-id="6c667-173">`ManifestEmbeddedFileProvider` est disponible dans ASP.NET Core 2.1 et versions ultérieures.</span><span class="sxs-lookup"><span data-stu-id="6c667-173">The `ManifestEmbeddedFileProvider` is available in ASP.NET Core 2.1 or later.</span></span> <span data-ttu-id="6c667-174">Pour accéder aux fichiers incorporés dans des assemblys dans ASP.NET Core 2.0 ou versions antérieures, consultez la [version d’ASP.NET Core 1.x de cette rubrique](xref:fundamentals/file-providers?view=aspnetcore-1.1).</span><span class="sxs-lookup"><span data-stu-id="6c667-174">To access files embedded in assemblies in ASP.NET Core 2.0 or earlier, see the [ASP.NET Core 1.x version of this topic](xref:fundamentals/file-providers?view=aspnetcore-1.1).</span></span>

<span data-ttu-id="6c667-175">Pour générer un manifeste des fichiers incorporés, définissez la propriété `<GenerateEmbeddedFilesManifest>` sur `true`.</span><span class="sxs-lookup"><span data-stu-id="6c667-175">To generate a manifest of the embedded files, set the `<GenerateEmbeddedFilesManifest>` property to `true`.</span></span> <span data-ttu-id="6c667-176">Spécifiez les fichiers à incorporer avec [ &lt;EmbeddedResource&gt;](/dotnet/core/tools/csproj#default-compilation-includes-in-net-core-projects) :</span><span class="sxs-lookup"><span data-stu-id="6c667-176">Specify the files to embed with [&lt;EmbeddedResource&gt;](/dotnet/core/tools/csproj#default-compilation-includes-in-net-core-projects):</span></span>

[!code-csharp[](file-providers/samples/2.x/FileProviderSample/FileProviderSample.csproj?highlight=5,13)]

<span data-ttu-id="6c667-177">Utilisez les [modèles glob](#glob-patterns) pour spécifier un ou plusieurs fichiers à incorporer dans l’assembly.</span><span class="sxs-lookup"><span data-stu-id="6c667-177">Use [glob patterns](#glob-patterns) to specify one or more files to embed into the assembly.</span></span>

<span data-ttu-id="6c667-178">L’exemple d’application crée un `ManifestEmbeddedFileProvider` et transmet l’assembly en cours d’exécution à son constructeur.</span><span class="sxs-lookup"><span data-stu-id="6c667-178">The sample app creates an `ManifestEmbeddedFileProvider` and passes the currently executing assembly to its constructor.</span></span>

<span data-ttu-id="6c667-179">*Startup.cs* :</span><span class="sxs-lookup"><span data-stu-id="6c667-179">*Startup.cs*:</span></span>

```csharp
var manifestEmbeddedProvider = 
    new ManifestEmbeddedFileProvider(Assembly.GetEntryAssembly());
```

<span data-ttu-id="6c667-180">Des surcharges supplémentaires vous permettent de :</span><span class="sxs-lookup"><span data-stu-id="6c667-180">Additional overloads allow you to:</span></span>

* <span data-ttu-id="6c667-181">Spécifier un chemin de fichier relatif.</span><span class="sxs-lookup"><span data-stu-id="6c667-181">Specify a relative file path.</span></span>
* <span data-ttu-id="6c667-182">Définir la portée de fichiers sur une date de dernière modification.</span><span class="sxs-lookup"><span data-stu-id="6c667-182">Scope files to a last modified date.</span></span>
* <span data-ttu-id="6c667-183">Nommer la ressource incorporée contenant le manifeste de fichier incorporé.</span><span class="sxs-lookup"><span data-stu-id="6c667-183">Name the embedded resource containing the embedded file manifest.</span></span>

| <span data-ttu-id="6c667-184">Surcharge</span><span class="sxs-lookup"><span data-stu-id="6c667-184">Overload</span></span> | <span data-ttu-id="6c667-185">Description</span><span class="sxs-lookup"><span data-stu-id="6c667-185">Description</span></span> |
| -------- | ----------- |
| [<span data-ttu-id="6c667-186">ManifestEmbeddedFileProvider(Assembly, String)</span><span class="sxs-lookup"><span data-stu-id="6c667-186">ManifestEmbeddedFileProvider(Assembly, String)</span></span>](/dotnet/api/microsoft.extensions.fileproviders.manifestembeddedfileprovider.-ctor#Microsoft_Extensions_FileProviders_ManifestEmbeddedFileProvider__ctor_System_Reflection_Assembly_System_String_) | <span data-ttu-id="6c667-187">Accepte un paramètre de chemin d'accès relatif `root` facultatif.</span><span class="sxs-lookup"><span data-stu-id="6c667-187">Accepts an optional `root` relative path parameter.</span></span> <span data-ttu-id="6c667-188">Spécifiez `root` pour définir la portée des appels à [GetDirectoryContents](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider.getdirectorycontents) sur ces ressources sous le chemin d’accès fourni.</span><span class="sxs-lookup"><span data-stu-id="6c667-188">Specify the `root` to scope calls to [GetDirectoryContents](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider.getdirectorycontents) to those resources under the provided path.</span></span> |
| [<span data-ttu-id="6c667-189">ManifestEmbeddedFileProvider(Assembly, String, DateTimeOffset)</span><span class="sxs-lookup"><span data-stu-id="6c667-189">ManifestEmbeddedFileProvider(Assembly, String, DateTimeOffset)</span></span>](/dotnet/api/microsoft.extensions.fileproviders.manifestembeddedfileprovider.-ctor#Microsoft_Extensions_FileProviders_ManifestEmbeddedFileProvider__ctor_System_Reflection_Assembly_System_String_System_DateTimeOffset_) | <span data-ttu-id="6c667-190">Accepte un paramètre de chemin d’accès relatif `root` facultatif et un paramètre de date `lastModified` ([DateTimeOffset](/dotnet/api/system.datetimeoffset)).</span><span class="sxs-lookup"><span data-stu-id="6c667-190">Accepts an optional `root` relative path parameter and a `lastModified` date ([DateTimeOffset](/dotnet/api/system.datetimeoffset)) parameter.</span></span> <span data-ttu-id="6c667-191">La date `lastModified` définit la portée de la dernière date de modification pour les instances [IFileInfo](/dotnet/api/microsoft.extensions.fileproviders.ifileinfo) retournées par [IFileProvider](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider).</span><span class="sxs-lookup"><span data-stu-id="6c667-191">The `lastModified` date scopes the last modification date for the [IFileInfo](/dotnet/api/microsoft.extensions.fileproviders.ifileinfo) instances returned by the [IFileProvider](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider).</span></span> |
| [<span data-ttu-id="6c667-192">ManifestEmbeddedFileProvider(Assembly, String, String, DateTimeOffset)</span><span class="sxs-lookup"><span data-stu-id="6c667-192">ManifestEmbeddedFileProvider(Assembly, String, String, DateTimeOffset)</span></span>](/dotnet/api/microsoft.extensions.fileproviders.manifestembeddedfileprovider.-ctor#Microsoft_Extensions_FileProviders_ManifestEmbeddedFileProvider__ctor_System_Reflection_Assembly_System_String_System_String_System_DateTimeOffset_) | <span data-ttu-id="6c667-193">Accepte un chemin d'accès relatif `root` facultatif, une date `lastModified` et des paramètres `manifestName`.</span><span class="sxs-lookup"><span data-stu-id="6c667-193">Accepts an optional `root` relative path, `lastModified` date, and `manifestName` parameters.</span></span> <span data-ttu-id="6c667-194">`manifestName` représente le nom de la ressource incorporée contenant la manifeste.</span><span class="sxs-lookup"><span data-stu-id="6c667-194">The `manifestName` represents the name of the embedded resource containing the manifest.</span></span> |

::: moniker-end

::: moniker range="< aspnetcore-2.0"

### <a name="embeddedfileprovider"></a><span data-ttu-id="6c667-195">EmbeddedFileProvider</span><span class="sxs-lookup"><span data-stu-id="6c667-195">EmbeddedFileProvider</span></span>

<span data-ttu-id="6c667-196">[EmbeddedFileProvider](/dotnet/api/microsoft.extensions.fileproviders.embeddedfileprovider) est utilisé pour accéder à des fichiers incorporés dans des assemblys.</span><span class="sxs-lookup"><span data-stu-id="6c667-196">The [EmbeddedFileProvider](/dotnet/api/microsoft.extensions.fileproviders.embeddedfileprovider) is used to access files embedded within assemblies.</span></span> <span data-ttu-id="6c667-197">Spécifiez les fichiers à incorporer avec la propriété [ &lt;EmbeddedResource&gt;](/dotnet/core/tools/csproj#default-compilation-includes-in-net-core-projects) dans le fichier projet :</span><span class="sxs-lookup"><span data-stu-id="6c667-197">Specify the files to embed with the [&lt;EmbeddedResource&gt;](/dotnet/core/tools/csproj#default-compilation-includes-in-net-core-projects) property in the project file:</span></span>

```xml
<ItemGroup>
  <EmbeddedResource Include="Resource.txt" />
</ItemGroup>
```

<span data-ttu-id="6c667-198">Utilisez les [modèles glob](#glob-patterns) pour spécifier un ou plusieurs fichiers à incorporer dans l’assembly.</span><span class="sxs-lookup"><span data-stu-id="6c667-198">Use [glob patterns](#glob-patterns) to specify one or more files to embed into the assembly.</span></span>

<span data-ttu-id="6c667-199">L’exemple d’application crée un `EmbeddedFileProvider` et transmet l’assembly en cours d’exécution à son constructeur.</span><span class="sxs-lookup"><span data-stu-id="6c667-199">The sample app creates an `EmbeddedFileProvider` and passes the currently executing assembly to its constructor.</span></span>

<span data-ttu-id="6c667-200">*Startup.cs* :</span><span class="sxs-lookup"><span data-stu-id="6c667-200">*Startup.cs*:</span></span>

```csharp
var embeddedProvider = new EmbeddedFileProvider(Assembly.GetEntryAssembly());
```

<span data-ttu-id="6c667-201">Les ressources incorporées n’exposent pas les répertoires.</span><span class="sxs-lookup"><span data-stu-id="6c667-201">Embedded resources don't expose directories.</span></span> <span data-ttu-id="6c667-202">Au lieu de cela, le chemin de la ressource (par l’intermédiaire de son espace de noms) est incorporé dans son nom de fichier à l’aide de séparateurs `.`.</span><span class="sxs-lookup"><span data-stu-id="6c667-202">Rather, the path to the resource (via its namespace) is embedded in its filename using `.` separators.</span></span> <span data-ttu-id="6c667-203">Dans l’exemple d’application, `baseNamespace` est `FileProviderSample.`.</span><span class="sxs-lookup"><span data-stu-id="6c667-203">In the sample app, the `baseNamespace` is `FileProviderSample.`.</span></span>

<span data-ttu-id="6c667-204">Le constructeur [EmbeddedFileProvider (Assembly, String)](/dotnet/api/microsoft.extensions.fileproviders.embeddedfileprovider.-ctor#Microsoft_Extensions_FileProviders_EmbeddedFileProvider__ctor_System_Reflection_Assembly_) accepte un paramètre `baseNamespace` facultatif.</span><span class="sxs-lookup"><span data-stu-id="6c667-204">The [EmbeddedFileProvider(Assembly, String)](/dotnet/api/microsoft.extensions.fileproviders.embeddedfileprovider.-ctor#Microsoft_Extensions_FileProviders_EmbeddedFileProvider__ctor_System_Reflection_Assembly_) constructor accepts an optional `baseNamespace` parameter.</span></span> <span data-ttu-id="6c667-205">Spécifiez l’espace de noms de base pour définir la portée des appels à [GetDirectoryContents](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider.getdirectorycontents) sur ces ressources sous l’espace de noms fourni.</span><span class="sxs-lookup"><span data-stu-id="6c667-205">Specify the base namespace to scope calls to [GetDirectoryContents](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider.getdirectorycontents) to those resources under the provided namespace.</span></span>

::: moniker-end

### <a name="compositefileprovider"></a><span data-ttu-id="6c667-206">CompositeFileProvider</span><span class="sxs-lookup"><span data-stu-id="6c667-206">CompositeFileProvider</span></span>

<span data-ttu-id="6c667-207">[CompositeFileProvider](/dotnet/api/microsoft.extensions.fileproviders.compositefileprovider) combine des instances `IFileProvider` en exposant une interface unique qui permet d’utiliser des fichiers de différents fournisseurs.</span><span class="sxs-lookup"><span data-stu-id="6c667-207">The [CompositeFileProvider](/dotnet/api/microsoft.extensions.fileproviders.compositefileprovider) combines `IFileProvider` instances, exposing a single interface for working with files from multiple providers.</span></span> <span data-ttu-id="6c667-208">Quand vous créez `CompositeFileProvider`, vous passez une ou plusieurs instances `IFileProvider` à son constructeur.</span><span class="sxs-lookup"><span data-stu-id="6c667-208">When creating the `CompositeFileProvider`, pass one or more `IFileProvider` instances to its constructor.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="6c667-209">Dans l’exemple d’application, un `PhysicalFileProvider` et un `ManifestEmbeddedFileProvider` fournissent des fichiers à un `CompositeFileProvider` inscrit dans le conteneur de service de l’application :</span><span class="sxs-lookup"><span data-stu-id="6c667-209">In the sample app, a `PhysicalFileProvider` and a `ManifestEmbeddedFileProvider` provide files to a `CompositeFileProvider` registered in the app's service container:</span></span>

[!code-csharp[](file-providers/samples/2.x/FileProviderSample/Startup.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="6c667-210">Dans l’exemple d’application, un `PhysicalFileProvider` et un `EmbeddedFileProvider` fournissent des fichiers à un `CompositeFileProvider` inscrit dans le conteneur de service de l’application :</span><span class="sxs-lookup"><span data-stu-id="6c667-210">In the sample app, a `PhysicalFileProvider` and an `EmbeddedFileProvider` provide files to a `CompositeFileProvider` registered in the app's service container:</span></span>

[!code-csharp[](file-providers/samples/1.x/FileProviderSample/Startup.cs?name=snippet1)]

::: moniker-end

## <a name="watch-for-changes"></a><span data-ttu-id="6c667-211">Suivre les modifications apportées</span><span class="sxs-lookup"><span data-stu-id="6c667-211">Watch for changes</span></span>

<span data-ttu-id="6c667-212">La méthode [IFileProvider.Watch](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider.watch) offre un moyen d’observer un ou plusieurs fichiers ou répertoires afin de détecter les changements.</span><span class="sxs-lookup"><span data-stu-id="6c667-212">The [IFileProvider.Watch](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider.watch) method provides a scenario to watch one or more files or directories for changes.</span></span> <span data-ttu-id="6c667-213">`Watch` accepte une chaîne de chemin, qui peut utiliser des [modèles d’utilisation des caractères génériques](#glob-patterns) pour spécifier plusieurs fichiers.</span><span class="sxs-lookup"><span data-stu-id="6c667-213">`Watch` accepts a path string, which can use [glob patterns](#glob-patterns) to specify multiple files.</span></span> <span data-ttu-id="6c667-214">`Watch` retourne un [IChangeToken](/dotnet/api/microsoft.extensions.primitives.ichangetoken).</span><span class="sxs-lookup"><span data-stu-id="6c667-214">`Watch` returns an [IChangeToken](/dotnet/api/microsoft.extensions.primitives.ichangetoken).</span></span> <span data-ttu-id="6c667-215">Le jeton de modification expose :</span><span class="sxs-lookup"><span data-stu-id="6c667-215">The change token exposes:</span></span>

* <span data-ttu-id="6c667-216">[HasChanged](/dotnet/api/microsoft.extensions.primitives.ichangetoken.haschanged) : une propriété qui peut être inspectée pour déterminer si une modification a eu lieu.</span><span class="sxs-lookup"><span data-stu-id="6c667-216">[HasChanged](/dotnet/api/microsoft.extensions.primitives.ichangetoken.haschanged): A property that can be inspected to determine if a change has occurred.</span></span>
* <span data-ttu-id="6c667-217">[RegisterChangeCallback](/dotnet/api/microsoft.extensions.primitives.ichangetoken.registerchangecallback) : appelée lorsque des modifications sont détectées dans la chaîne de chemin d’accès spécifiée.</span><span class="sxs-lookup"><span data-stu-id="6c667-217">[RegisterChangeCallback](/dotnet/api/microsoft.extensions.primitives.ichangetoken.registerchangecallback): Called when changes are detected to the specified path string.</span></span> <span data-ttu-id="6c667-218">Chaque jeton de modification appelle uniquement son rappel associé en réponse à un changement unique.</span><span class="sxs-lookup"><span data-stu-id="6c667-218">Each change token only calls its associated callback in response to a single change.</span></span> <span data-ttu-id="6c667-219">Pour activer une surveillance constante, vous pouvez utiliser une [TaskCompletionSource](/dotnet/api/system.threading.tasks.taskcompletionsource-1) comme indiqué ci-dessous, ou recréer des instances `IChangeToken` en réponse aux changements.</span><span class="sxs-lookup"><span data-stu-id="6c667-219">To enable constant monitoring, use a [TaskCompletionSource](/dotnet/api/system.threading.tasks.taskcompletionsource-1) (shown below) or recreate `IChangeToken` instances in response to changes.</span></span>

<span data-ttu-id="6c667-220">Dans l’exemple d’application, l’application console *WatchConsole* est configurée pour afficher un message chaque fois qu’un fichier texte est modifié :</span><span class="sxs-lookup"><span data-stu-id="6c667-220">In the sample app, the *WatchConsole* console app is configured to display a message whenever a text file is modified:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](file-providers/samples/2.x/WatchConsole/Program.cs?name=snippet1&highlight=1-2,16,19-20)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](file-providers/samples/1.x/WatchConsole/Program.cs?name=snippet1&highlight=1-2,16,19-20)]

::: moniker-end

<span data-ttu-id="6c667-221">Certains systèmes de fichiers, comme les conteneurs Docker et les partages réseau, peuvent ne pas envoyer de manière fiable les notifications de modifications.</span><span class="sxs-lookup"><span data-stu-id="6c667-221">Some file systems, such as Docker containers and network shares, may not reliably send change notifications.</span></span> <span data-ttu-id="6c667-222">Définissez la variable d’environnement `DOTNET_USE_POLLING_FILE_WATCHER` sur `1` ou `true` pour interroger le système de fichiers à la recherche de changements toutes les quatre secondes (non configurable).</span><span class="sxs-lookup"><span data-stu-id="6c667-222">Set the `DOTNET_USE_POLLING_FILE_WATCHER` environment variable to `1` or `true` to poll the file system for changes every four seconds (not configurable).</span></span>

## <a name="glob-patterns"></a><span data-ttu-id="6c667-223">Modèles Glob</span><span class="sxs-lookup"><span data-stu-id="6c667-223">Glob patterns</span></span>

<span data-ttu-id="6c667-224">Les chemins de système de fichiers utilisent des modèles à caractères génériques appelés *modèles Glob (ou d’utilisation des caractères génériques)*.</span><span class="sxs-lookup"><span data-stu-id="6c667-224">File system paths use wildcard patterns called *glob (or globbing) patterns*.</span></span> <span data-ttu-id="6c667-225">Spécifiez les groupes de fichiers avec ces modèles.</span><span class="sxs-lookup"><span data-stu-id="6c667-225">Specify groups of files with these patterns.</span></span> <span data-ttu-id="6c667-226">Les deux caractères génériques sont `*` et `**` :</span><span class="sxs-lookup"><span data-stu-id="6c667-226">The two wildcard characters are `*` and `**`:</span></span>

**`*`**  
<span data-ttu-id="6c667-227">Établit une correspondance avec n’importe quel élément au niveau de dossier actuel, avec n’importe quel nom de fichier ou avec n’importe quelle extension de fichier.</span><span class="sxs-lookup"><span data-stu-id="6c667-227">Matches anything at the current folder level, any filename, or any file extension.</span></span> <span data-ttu-id="6c667-228">Les correspondances sont terminées par des caractères `/` et `.` dans le chemin des fichiers.</span><span class="sxs-lookup"><span data-stu-id="6c667-228">Matches are terminated by `/` and `.` characters in the file path.</span></span>

**`**`**  
<span data-ttu-id="6c667-229">Établit une correspondance avec n’importe quel élément sur plusieurs niveaux de répertoire.</span><span class="sxs-lookup"><span data-stu-id="6c667-229">Matches anything across multiple directory levels.</span></span> <span data-ttu-id="6c667-230">Peut être utilisé pour établir une correspondance avec plusieurs fichiers dans une hiérarchie de répertoires de manière récursive.</span><span class="sxs-lookup"><span data-stu-id="6c667-230">Can be used to recursively match many files within a directory hierarchy.</span></span>

<span data-ttu-id="6c667-231">**Exemples de modèles d’utilisation des caractères génériques**</span><span class="sxs-lookup"><span data-stu-id="6c667-231">**Glob pattern examples**</span></span>

**`directory/file.txt`**  
<span data-ttu-id="6c667-232">Établit une correspondance avec un fichier spécifique dans un répertoire spécifique.</span><span class="sxs-lookup"><span data-stu-id="6c667-232">Matches a specific file in a specific directory.</span></span>

**`directory/*.txt`**  
<span data-ttu-id="6c667-233">Établit une correspondance avec tous les fichiers ayant l’extension *.txt* dans un répertoire spécifique.</span><span class="sxs-lookup"><span data-stu-id="6c667-233">Matches all files with *.txt* extension in a specific directory.</span></span>

**`directory/*/appsettings.json`**  
<span data-ttu-id="6c667-234">Établit une correspondance avec tous les fichiers `appsettings.json` dans les répertoires situés exactement un niveau en dessous du dossier *répertoire*.</span><span class="sxs-lookup"><span data-stu-id="6c667-234">Matches all `appsettings.json` files in directories exactly one level below the *directory* folder.</span></span>

**`directory/**/*.txt`**  
<span data-ttu-id="6c667-235">Établit une correspondance avec tous les fichiers ayant l’extension *.txt* et se trouvant n’importe où sous le dossier *répertoire*.</span><span class="sxs-lookup"><span data-stu-id="6c667-235">Matches all files with *.txt* extension found anywhere under the *directory* folder.</span></span>
