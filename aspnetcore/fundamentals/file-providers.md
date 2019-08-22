---
title: Fournisseurs de fichiers dans ASP.NET Core
author: guardrex
description: Découvrez comment ASP.NET Core fournit un accès au système de fichiers en utilisant des fournisseurs de fichiers.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/30/2019
uid: fundamentals/file-providers
ms.openlocfilehash: b93b2df7fad7c173f43ad69aec865f09de6c9c34
ms.sourcegitcommit: 7a46973998623aead757ad386fe33602b1658793
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/15/2019
ms.locfileid: "69487578"
---
# <a name="file-providers-in-aspnet-core"></a><span data-ttu-id="1c977-103">Fournisseurs de fichiers dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="1c977-103">File Providers in ASP.NET Core</span></span>

<span data-ttu-id="1c977-104">Par [Steve Smith](https://ardalis.com/) et [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="1c977-104">By [Steve Smith](https://ardalis.com/) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="1c977-105">ASP.NET Core fournit un accès au système de fichiers en utilisant des fournisseurs de fichiers.</span><span class="sxs-lookup"><span data-stu-id="1c977-105">ASP.NET Core abstracts file system access through the use of File Providers.</span></span> <span data-ttu-id="1c977-106">Des fournisseurs de fichiers sont utilisés dans l’infrastructure ASP.NET Core :</span><span class="sxs-lookup"><span data-stu-id="1c977-106">File Providers are used throughout the ASP.NET Core framework:</span></span>

* <span data-ttu-id="1c977-107">[IHostingEnvironment](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment) expose la racine web et la racine de contenu de l’application sous la forme de types `IFileProvider`.</span><span class="sxs-lookup"><span data-stu-id="1c977-107">[IHostingEnvironment](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment) exposes the app's content root and web root as `IFileProvider` types.</span></span>
* <span data-ttu-id="1c977-108">[L’intergiciel (middleware) de fichiers statiques](xref:fundamentals/static-files) utilise des fournisseurs de fichiers pour localiser les fichiers statiques.</span><span class="sxs-lookup"><span data-stu-id="1c977-108">[Static File Middleware](xref:fundamentals/static-files) uses File Providers to locate static files.</span></span>
* <span data-ttu-id="1c977-109">[Razor](xref:mvc/views/razor) utilise des fournisseurs de fichiers pour localiser des pages et des vues.</span><span class="sxs-lookup"><span data-stu-id="1c977-109">[Razor](xref:mvc/views/razor) uses File Providers to locate pages and views.</span></span>
* <span data-ttu-id="1c977-110">Les outils .NET Core utilisent des fournisseurs de fichiers et des modèles d’utilisation des caractères génériques pour spécifier les fichiers qui doivent être publiés.</span><span class="sxs-lookup"><span data-stu-id="1c977-110">.NET Core tooling uses File Providers and glob patterns to specify which files should be published.</span></span>

<span data-ttu-id="1c977-111">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/file-providers/samples) ([procédure de téléchargement](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="1c977-111">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/file-providers/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="file-provider-interfaces"></a><span data-ttu-id="1c977-112">Interfaces de fournisseur de fichiers</span><span class="sxs-lookup"><span data-stu-id="1c977-112">File Provider interfaces</span></span>

<span data-ttu-id="1c977-113">L’interface principale est [IFileProvider](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider).</span><span class="sxs-lookup"><span data-stu-id="1c977-113">The primary interface is [IFileProvider](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider).</span></span> <span data-ttu-id="1c977-114">`IFileProvider` expose des méthodes pour :</span><span class="sxs-lookup"><span data-stu-id="1c977-114">`IFileProvider` exposes methods to:</span></span>

* <span data-ttu-id="1c977-115">Obtenir des informations sur le fichier ([IFileInfo](/dotnet/api/microsoft.extensions.fileproviders.ifileinfo)).</span><span class="sxs-lookup"><span data-stu-id="1c977-115">Obtain file information ([IFileInfo](/dotnet/api/microsoft.extensions.fileproviders.ifileinfo)).</span></span>
* <span data-ttu-id="1c977-116">Obtenir des informations sur le répertoire ([IDirectoryContents](/dotnet/api/microsoft.extensions.fileproviders.idirectorycontents)).</span><span class="sxs-lookup"><span data-stu-id="1c977-116">Obtain directory information ([IDirectoryContents](/dotnet/api/microsoft.extensions.fileproviders.idirectorycontents)).</span></span>
* <span data-ttu-id="1c977-117">Configurer des notifications de modification (à l’aide un [IChangeToken](/dotnet/api/microsoft.extensions.primitives.ichangetoken)).</span><span class="sxs-lookup"><span data-stu-id="1c977-117">Set up change notifications (using an [IChangeToken](/dotnet/api/microsoft.extensions.primitives.ichangetoken)).</span></span>

<span data-ttu-id="1c977-118">`IFileInfo` fournit des méthodes et propriétés d’utilisation des fichiers :</span><span class="sxs-lookup"><span data-stu-id="1c977-118">`IFileInfo` provides methods and properties for working with files:</span></span>

* [<span data-ttu-id="1c977-119">Existe</span><span class="sxs-lookup"><span data-stu-id="1c977-119">Exists</span></span>](/dotnet/api/microsoft.extensions.fileproviders.ifileinfo.exists)
* [<span data-ttu-id="1c977-120">IsDirectory</span><span class="sxs-lookup"><span data-stu-id="1c977-120">IsDirectory</span></span>](/dotnet/api/microsoft.extensions.fileproviders.ifileinfo.isdirectory)
* [<span data-ttu-id="1c977-121">Name</span><span class="sxs-lookup"><span data-stu-id="1c977-121">Name</span></span>](/dotnet/api/microsoft.extensions.fileproviders.ifileinfo.name)
* <span data-ttu-id="1c977-122">[Longueur](/dotnet/api/microsoft.extensions.fileproviders.ifileinfo.length) (en octets)</span><span class="sxs-lookup"><span data-stu-id="1c977-122">[Length](/dotnet/api/microsoft.extensions.fileproviders.ifileinfo.length) (in bytes)</span></span>
* <span data-ttu-id="1c977-123">Date [LastModified](/dotnet/api/microsoft.extensions.fileproviders.ifileinfo.lastmodified)</span><span class="sxs-lookup"><span data-stu-id="1c977-123">[LastModified](/dotnet/api/microsoft.extensions.fileproviders.ifileinfo.lastmodified) date</span></span>

<span data-ttu-id="1c977-124">Vous pouvez lire à partir du fichier en utilisant la méthode [IFileInfo.CreateReadStream](/dotnet/api/microsoft.extensions.fileproviders.ifileinfo.createreadstream).</span><span class="sxs-lookup"><span data-stu-id="1c977-124">You can read from the file using the [IFileInfo.CreateReadStream](/dotnet/api/microsoft.extensions.fileproviders.ifileinfo.createreadstream) method.</span></span>

<span data-ttu-id="1c977-125">L’exemple d’application montre comment configurer un fournisseur de fichiers dans `Startup.ConfigureServices` pour une utilisation dans toute l’application via [l’injection de dépendances](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="1c977-125">The sample app demonstrates how to configure a File Provider in `Startup.ConfigureServices` for use throughout the app via [dependency injection](xref:fundamentals/dependency-injection).</span></span>

## <a name="file-provider-implementations"></a><span data-ttu-id="1c977-126">Implémentations de fournisseur de fichiers</span><span class="sxs-lookup"><span data-stu-id="1c977-126">File Provider implementations</span></span>

<span data-ttu-id="1c977-127">Trois implémentations de `IFileProvider` sont disponibles.</span><span class="sxs-lookup"><span data-stu-id="1c977-127">Three implementations of `IFileProvider` are available.</span></span>

| <span data-ttu-id="1c977-128">Implémentation</span><span class="sxs-lookup"><span data-stu-id="1c977-128">Implementation</span></span> | <span data-ttu-id="1c977-129">Description</span><span class="sxs-lookup"><span data-stu-id="1c977-129">Description</span></span> |
| -------------- | ----------- |
| [<span data-ttu-id="1c977-130">PhysicalFileProvider</span><span class="sxs-lookup"><span data-stu-id="1c977-130">PhysicalFileProvider</span></span>](#physicalfileprovider) | <span data-ttu-id="1c977-131">Le fournisseur physique est utilisé pour accéder aux fichiers physiques du système.</span><span class="sxs-lookup"><span data-stu-id="1c977-131">The physical provider is used to access the system's physical files.</span></span> |
| [<span data-ttu-id="1c977-132">ManifestEmbeddedFileProvider</span><span class="sxs-lookup"><span data-stu-id="1c977-132">ManifestEmbeddedFileProvider</span></span>](#manifestembeddedfileprovider) | <span data-ttu-id="1c977-133">Le fournisseur incorporé de manifeste est utilisé pour accéder à des fichiers incorporés dans des assemblys.</span><span class="sxs-lookup"><span data-stu-id="1c977-133">The manifest embedded provider is used to access files embedded in assemblies.</span></span> |
| [<span data-ttu-id="1c977-134">CompositeFileProvider</span><span class="sxs-lookup"><span data-stu-id="1c977-134">CompositeFileProvider</span></span>](#compositefileprovider) | <span data-ttu-id="1c977-135">Le fournisseur composite est utilisé pour fournir un accès combiné à des fichiers et à des répertoires à partir d’un ou de plusieurs fournisseurs.</span><span class="sxs-lookup"><span data-stu-id="1c977-135">The composite provider is used to provide combined access to files and directories from one or more other providers.</span></span> |

### <a name="physicalfileprovider"></a><span data-ttu-id="1c977-136">PhysicalFileProvider</span><span class="sxs-lookup"><span data-stu-id="1c977-136">PhysicalFileProvider</span></span>

<span data-ttu-id="1c977-137">La classe [PhysicalFileProvider](/dotnet/api/microsoft.extensions.fileproviders.physicalfileprovider) fournit un accès au système de fichiers physique.</span><span class="sxs-lookup"><span data-stu-id="1c977-137">The [PhysicalFileProvider](/dotnet/api/microsoft.extensions.fileproviders.physicalfileprovider) provides access to the physical file system.</span></span> <span data-ttu-id="1c977-138">`PhysicalFileProvider` utilise le type [System.IO.File](/dotnet/api/system.io.file) (pour le fournisseur physique), définissant comme portée tous les chemins à un répertoire et ses enfants.</span><span class="sxs-lookup"><span data-stu-id="1c977-138">`PhysicalFileProvider` uses the [System.IO.File](/dotnet/api/system.io.file) type (for the physical provider) and scopes all paths to a directory and its children.</span></span> <span data-ttu-id="1c977-139">Cette portée empêche l’accès au système de fichiers en dehors du répertoire spécifié et ses enfants.</span><span class="sxs-lookup"><span data-stu-id="1c977-139">This scoping prevents access to the file system outside of the specified directory and its children.</span></span> <span data-ttu-id="1c977-140">Lors de l’instanciation de ce fournisseur, un chemin de répertoire est obligatoire et sert de chemin de base pour toutes les demandes effectuées à l’aide du fournisseur.</span><span class="sxs-lookup"><span data-stu-id="1c977-140">When instantiating this provider, a directory path is required and serves as the base path for all requests made using the provider.</span></span> <span data-ttu-id="1c977-141">Vous pouvez instancier un fournisseur `PhysicalFileProvider` directement, ou vous pouvez demander un `IFileProvider` dans un constructeur à l’aide de [l’injection de dépendances](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="1c977-141">You can instantiate a `PhysicalFileProvider` provider directly, or you can request an `IFileProvider` in a constructor through [dependency injection](xref:fundamentals/dependency-injection).</span></span>

<span data-ttu-id="1c977-142">**Types statiques**</span><span class="sxs-lookup"><span data-stu-id="1c977-142">**Static types**</span></span>

<span data-ttu-id="1c977-143">Le code suivant montre comment créer un `PhysicalFileProvider` et l’utiliser pour obtenir le contenu du répertoire et les informations sur le fichier :</span><span class="sxs-lookup"><span data-stu-id="1c977-143">The following code shows how to create a `PhysicalFileProvider` and use it to obtain directory contents and file information:</span></span>

```csharp
var provider = new PhysicalFileProvider(applicationRoot);
var contents = provider.GetDirectoryContents(string.Empty);
var fileInfo = provider.GetFileInfo("wwwroot/js/site.js");
```

<span data-ttu-id="1c977-144">Types dans l’exemple précédent :</span><span class="sxs-lookup"><span data-stu-id="1c977-144">Types in the preceding example:</span></span>

* <span data-ttu-id="1c977-145">`provider` est un `IFileProvider`.</span><span class="sxs-lookup"><span data-stu-id="1c977-145">`provider` is an `IFileProvider`.</span></span>
* <span data-ttu-id="1c977-146">`contents` est un `IDirectoryContents`.</span><span class="sxs-lookup"><span data-stu-id="1c977-146">`contents` is an `IDirectoryContents`.</span></span>
* <span data-ttu-id="1c977-147">`fileInfo` est un `IFileInfo`.</span><span class="sxs-lookup"><span data-stu-id="1c977-147">`fileInfo` is an `IFileInfo`.</span></span>

<span data-ttu-id="1c977-148">Le fournisseur de fichiers peut être utilisé pour itérer au sein du répertoire spécifié par `applicationRoot` ou pour appeler `GetFileInfo` afin d’obtenir des informations sur un fichier.</span><span class="sxs-lookup"><span data-stu-id="1c977-148">The File Provider can be used to iterate through the directory specified by `applicationRoot` or call `GetFileInfo` to obtain a file's information.</span></span> <span data-ttu-id="1c977-149">Le fournisseur de fichiers n’a pas accès en dehors du répertoire `applicationRoot`.</span><span class="sxs-lookup"><span data-stu-id="1c977-149">The File Provider has no access outside of the `applicationRoot` directory.</span></span>

<span data-ttu-id="1c977-150">L’exemple d’application crée le fournisseur dans la classe `Startup.ConfigureServices` de l’application à l’aide de [IHostingEnvironment.ContentRootFileProvider](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment.contentrootfileprovider) :</span><span class="sxs-lookup"><span data-stu-id="1c977-150">The sample app creates the provider in the app's `Startup.ConfigureServices` class using [IHostingEnvironment.ContentRootFileProvider](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment.contentrootfileprovider):</span></span>

```csharp
var physicalProvider = _env.ContentRootFileProvider;
```

<span data-ttu-id="1c977-151">**Obtenir les types de fournisseurs de fichiers avec l’injection de dépendances**</span><span class="sxs-lookup"><span data-stu-id="1c977-151">**Obtain File Provider types with dependency injection**</span></span>

<span data-ttu-id="1c977-152">Injectez le fournisseur dans n’importe quel constructeur de classe et affectez-le à un champ local.</span><span class="sxs-lookup"><span data-stu-id="1c977-152">Inject the provider into any class constructor and assign it to a local field.</span></span> <span data-ttu-id="1c977-153">Utilisez le champ dans l’ensemble des méthodes de la classe pour accéder aux fichiers.</span><span class="sxs-lookup"><span data-stu-id="1c977-153">Use the field throughout the class's methods to access files.</span></span>

<span data-ttu-id="1c977-154">Dans l’exemple d’application, la classe `IndexModel` reçoit une instance `IFileProvider` pour obtenir le contenu du répertoire pour le chemin d’accès de la base de l’application.</span><span class="sxs-lookup"><span data-stu-id="1c977-154">In the sample app, the `IndexModel` class receives an `IFileProvider` instance to obtain directory contents for the app's base path.</span></span>

<span data-ttu-id="1c977-155">*Pages/Index.cshtml.cs* :</span><span class="sxs-lookup"><span data-stu-id="1c977-155">*Pages/Index.cshtml.cs*:</span></span>

[!code-csharp[](file-providers/samples/2.x/FileProviderSample/Pages/Index.cshtml.cs?name=snippet1)]

<span data-ttu-id="1c977-156">`IDirectoryContents` sont itérés dans la page.</span><span class="sxs-lookup"><span data-stu-id="1c977-156">The `IDirectoryContents` are iterated in the page.</span></span>

<span data-ttu-id="1c977-157">*Pages/Index.cshtml* :</span><span class="sxs-lookup"><span data-stu-id="1c977-157">*Pages/Index.cshtml*:</span></span>

[!code-cshtml[](file-providers/samples/2.x/FileProviderSample/Pages/Index.cshtml?name=snippet1)]

### <a name="manifestembeddedfileprovider"></a><span data-ttu-id="1c977-158">ManifestEmbeddedFileProvider</span><span class="sxs-lookup"><span data-stu-id="1c977-158">ManifestEmbeddedFileProvider</span></span>

<span data-ttu-id="1c977-159">[ManifestEmbeddedFileProvider](/dotnet/api/microsoft.extensions.fileproviders.manifestembeddedfileprovider) est utilisé pour accéder à des fichiers incorporés dans des assemblys.</span><span class="sxs-lookup"><span data-stu-id="1c977-159">The [ManifestEmbeddedFileProvider](/dotnet/api/microsoft.extensions.fileproviders.manifestembeddedfileprovider) is used to access files embedded within assemblies.</span></span> <span data-ttu-id="1c977-160">`ManifestEmbeddedFileProvider` utilise un manifeste compilé dans l’assembly pour reconstruire les chemins d’accès d’origine des fichiers intégrés.</span><span class="sxs-lookup"><span data-stu-id="1c977-160">The `ManifestEmbeddedFileProvider` uses a manifest compiled into the assembly to reconstruct the original paths of the embedded files.</span></span>

<span data-ttu-id="1c977-161">Pour générer un manifeste des fichiers incorporés, définissez la propriété `<GenerateEmbeddedFilesManifest>` sur `true`.</span><span class="sxs-lookup"><span data-stu-id="1c977-161">To generate a manifest of the embedded files, set the `<GenerateEmbeddedFilesManifest>` property to `true`.</span></span> <span data-ttu-id="1c977-162">Spécifiez les fichiers à incorporer avec [ &lt;EmbeddedResource&gt;](/dotnet/core/tools/csproj#default-compilation-includes-in-net-core-projects) :</span><span class="sxs-lookup"><span data-stu-id="1c977-162">Specify the files to embed with [&lt;EmbeddedResource&gt;](/dotnet/core/tools/csproj#default-compilation-includes-in-net-core-projects):</span></span>

[!code-csharp[](file-providers/samples/2.x/FileProviderSample/FileProviderSample.csproj?highlight=6,14)]

<span data-ttu-id="1c977-163">Utilisez les [modèles glob](#glob-patterns) pour spécifier un ou plusieurs fichiers à incorporer dans l’assembly.</span><span class="sxs-lookup"><span data-stu-id="1c977-163">Use [glob patterns](#glob-patterns) to specify one or more files to embed into the assembly.</span></span>

<span data-ttu-id="1c977-164">L’exemple d’application crée un `ManifestEmbeddedFileProvider` et transmet l’assembly en cours d’exécution à son constructeur.</span><span class="sxs-lookup"><span data-stu-id="1c977-164">The sample app creates an `ManifestEmbeddedFileProvider` and passes the currently executing assembly to its constructor.</span></span>

<span data-ttu-id="1c977-165">*Startup.cs* :</span><span class="sxs-lookup"><span data-stu-id="1c977-165">*Startup.cs*:</span></span>

```csharp
var manifestEmbeddedProvider = 
    new ManifestEmbeddedFileProvider(Assembly.GetEntryAssembly());
```

<span data-ttu-id="1c977-166">Des surcharges supplémentaires vous permettent de :</span><span class="sxs-lookup"><span data-stu-id="1c977-166">Additional overloads allow you to:</span></span>

* <span data-ttu-id="1c977-167">Spécifier un chemin de fichier relatif.</span><span class="sxs-lookup"><span data-stu-id="1c977-167">Specify a relative file path.</span></span>
* <span data-ttu-id="1c977-168">Définir la portée de fichiers sur une date de dernière modification.</span><span class="sxs-lookup"><span data-stu-id="1c977-168">Scope files to a last modified date.</span></span>
* <span data-ttu-id="1c977-169">Nommer la ressource incorporée contenant le manifeste de fichier incorporé.</span><span class="sxs-lookup"><span data-stu-id="1c977-169">Name the embedded resource containing the embedded file manifest.</span></span>

| <span data-ttu-id="1c977-170">Surcharge</span><span class="sxs-lookup"><span data-stu-id="1c977-170">Overload</span></span> | <span data-ttu-id="1c977-171">Description</span><span class="sxs-lookup"><span data-stu-id="1c977-171">Description</span></span> |
| -------- | ----------- |
| [<span data-ttu-id="1c977-172">ManifestEmbeddedFileProvider(Assembly, String)</span><span class="sxs-lookup"><span data-stu-id="1c977-172">ManifestEmbeddedFileProvider(Assembly, String)</span></span>](/dotnet/api/microsoft.extensions.fileproviders.manifestembeddedfileprovider.-ctor#Microsoft_Extensions_FileProviders_ManifestEmbeddedFileProvider__ctor_System_Reflection_Assembly_System_String_) | <span data-ttu-id="1c977-173">Accepte un paramètre de chemin d'accès relatif `root` facultatif.</span><span class="sxs-lookup"><span data-stu-id="1c977-173">Accepts an optional `root` relative path parameter.</span></span> <span data-ttu-id="1c977-174">Spécifiez `root` pour définir la portée des appels à [GetDirectoryContents](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider.getdirectorycontents) sur ces ressources sous le chemin d’accès fourni.</span><span class="sxs-lookup"><span data-stu-id="1c977-174">Specify the `root` to scope calls to [GetDirectoryContents](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider.getdirectorycontents) to those resources under the provided path.</span></span> |
| [<span data-ttu-id="1c977-175">ManifestEmbeddedFileProvider(Assembly, String, DateTimeOffset)</span><span class="sxs-lookup"><span data-stu-id="1c977-175">ManifestEmbeddedFileProvider(Assembly, String, DateTimeOffset)</span></span>](/dotnet/api/microsoft.extensions.fileproviders.manifestembeddedfileprovider.-ctor#Microsoft_Extensions_FileProviders_ManifestEmbeddedFileProvider__ctor_System_Reflection_Assembly_System_String_System_DateTimeOffset_) | <span data-ttu-id="1c977-176">Accepte un paramètre de chemin d’accès relatif `root` facultatif et un paramètre de date `lastModified` ([DateTimeOffset](/dotnet/api/system.datetimeoffset)).</span><span class="sxs-lookup"><span data-stu-id="1c977-176">Accepts an optional `root` relative path parameter and a `lastModified` date ([DateTimeOffset](/dotnet/api/system.datetimeoffset)) parameter.</span></span> <span data-ttu-id="1c977-177">La date `lastModified` définit la portée de la dernière date de modification pour les instances [IFileInfo](/dotnet/api/microsoft.extensions.fileproviders.ifileinfo) retournées par [IFileProvider](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider).</span><span class="sxs-lookup"><span data-stu-id="1c977-177">The `lastModified` date scopes the last modification date for the [IFileInfo](/dotnet/api/microsoft.extensions.fileproviders.ifileinfo) instances returned by the [IFileProvider](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider).</span></span> |
| [<span data-ttu-id="1c977-178">ManifestEmbeddedFileProvider(Assembly, String, String, DateTimeOffset)</span><span class="sxs-lookup"><span data-stu-id="1c977-178">ManifestEmbeddedFileProvider(Assembly, String, String, DateTimeOffset)</span></span>](/dotnet/api/microsoft.extensions.fileproviders.manifestembeddedfileprovider.-ctor#Microsoft_Extensions_FileProviders_ManifestEmbeddedFileProvider__ctor_System_Reflection_Assembly_System_String_System_String_System_DateTimeOffset_) | <span data-ttu-id="1c977-179">Accepte un chemin d'accès relatif `root` facultatif, une date `lastModified` et des paramètres `manifestName`.</span><span class="sxs-lookup"><span data-stu-id="1c977-179">Accepts an optional `root` relative path, `lastModified` date, and `manifestName` parameters.</span></span> <span data-ttu-id="1c977-180">`manifestName` représente le nom de la ressource incorporée contenant la manifeste.</span><span class="sxs-lookup"><span data-stu-id="1c977-180">The `manifestName` represents the name of the embedded resource containing the manifest.</span></span> |

### <a name="compositefileprovider"></a><span data-ttu-id="1c977-181">CompositeFileProvider</span><span class="sxs-lookup"><span data-stu-id="1c977-181">CompositeFileProvider</span></span>

<span data-ttu-id="1c977-182">[CompositeFileProvider](/dotnet/api/microsoft.extensions.fileproviders.compositefileprovider) combine des instances `IFileProvider` en exposant une interface unique qui permet d’utiliser des fichiers de différents fournisseurs.</span><span class="sxs-lookup"><span data-stu-id="1c977-182">The [CompositeFileProvider](/dotnet/api/microsoft.extensions.fileproviders.compositefileprovider) combines `IFileProvider` instances, exposing a single interface for working with files from multiple providers.</span></span> <span data-ttu-id="1c977-183">Quand vous créez `CompositeFileProvider`, vous passez une ou plusieurs instances `IFileProvider` à son constructeur.</span><span class="sxs-lookup"><span data-stu-id="1c977-183">When creating the `CompositeFileProvider`, pass one or more `IFileProvider` instances to its constructor.</span></span>

<span data-ttu-id="1c977-184">Dans l’exemple d’application, un `PhysicalFileProvider` et un `ManifestEmbeddedFileProvider` fournissent des fichiers à un `CompositeFileProvider` inscrit dans le conteneur de service de l’application :</span><span class="sxs-lookup"><span data-stu-id="1c977-184">In the sample app, a `PhysicalFileProvider` and a `ManifestEmbeddedFileProvider` provide files to a `CompositeFileProvider` registered in the app's service container:</span></span>

[!code-csharp[](file-providers/samples/2.x/FileProviderSample/Startup.cs?name=snippet1)]

## <a name="watch-for-changes"></a><span data-ttu-id="1c977-185">Suivre les modifications apportées</span><span class="sxs-lookup"><span data-stu-id="1c977-185">Watch for changes</span></span>

<span data-ttu-id="1c977-186">La méthode [IFileProvider.Watch](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider.watch) offre un moyen d’observer un ou plusieurs fichiers ou répertoires afin de détecter les changements.</span><span class="sxs-lookup"><span data-stu-id="1c977-186">The [IFileProvider.Watch](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider.watch) method provides a scenario to watch one or more files or directories for changes.</span></span> <span data-ttu-id="1c977-187">`Watch` accepte une chaîne de chemin, qui peut utiliser des [modèles d’utilisation des caractères génériques](#glob-patterns) pour spécifier plusieurs fichiers.</span><span class="sxs-lookup"><span data-stu-id="1c977-187">`Watch` accepts a path string, which can use [glob patterns](#glob-patterns) to specify multiple files.</span></span> <span data-ttu-id="1c977-188">`Watch` retourne un [IChangeToken](/dotnet/api/microsoft.extensions.primitives.ichangetoken).</span><span class="sxs-lookup"><span data-stu-id="1c977-188">`Watch` returns an [IChangeToken](/dotnet/api/microsoft.extensions.primitives.ichangetoken).</span></span> <span data-ttu-id="1c977-189">Le jeton de modification expose :</span><span class="sxs-lookup"><span data-stu-id="1c977-189">The change token exposes:</span></span>

* <span data-ttu-id="1c977-190">[HasChanged](/dotnet/api/microsoft.extensions.primitives.ichangetoken.haschanged) : une propriété qui peut être inspectée pour déterminer si une modification a eu lieu.</span><span class="sxs-lookup"><span data-stu-id="1c977-190">[HasChanged](/dotnet/api/microsoft.extensions.primitives.ichangetoken.haschanged): A property that can be inspected to determine if a change has occurred.</span></span>
* <span data-ttu-id="1c977-191">[RegisterChangeCallback](/dotnet/api/microsoft.extensions.primitives.ichangetoken.registerchangecallback) : appelée quand des modifications sont détectées dans la chaîne de chemin spécifiée.</span><span class="sxs-lookup"><span data-stu-id="1c977-191">[RegisterChangeCallback](/dotnet/api/microsoft.extensions.primitives.ichangetoken.registerchangecallback): Called when changes are detected to the specified path string.</span></span> <span data-ttu-id="1c977-192">Chaque jeton de modification appelle uniquement son rappel associé en réponse à un changement unique.</span><span class="sxs-lookup"><span data-stu-id="1c977-192">Each change token only calls its associated callback in response to a single change.</span></span> <span data-ttu-id="1c977-193">Pour activer une surveillance constante, vous pouvez utiliser une [TaskCompletionSource](/dotnet/api/system.threading.tasks.taskcompletionsource-1) comme indiqué ci-dessous, ou recréer des instances `IChangeToken` en réponse aux changements.</span><span class="sxs-lookup"><span data-stu-id="1c977-193">To enable constant monitoring, use a [TaskCompletionSource](/dotnet/api/system.threading.tasks.taskcompletionsource-1) (shown below) or recreate `IChangeToken` instances in response to changes.</span></span>

<span data-ttu-id="1c977-194">Dans l’exemple d’application, l’application console *WatchConsole* est configurée pour afficher un message chaque fois qu’un fichier texte est modifié :</span><span class="sxs-lookup"><span data-stu-id="1c977-194">In the sample app, the *WatchConsole* console app is configured to display a message whenever a text file is modified:</span></span>

[!code-csharp[](file-providers/samples/2.x/WatchConsole/Program.cs?name=snippet1&highlight=1-2,16,19-20)]

<span data-ttu-id="1c977-195">Certains systèmes de fichiers, comme les conteneurs Docker et les partages réseau, peuvent ne pas envoyer de manière fiable les notifications de modifications.</span><span class="sxs-lookup"><span data-stu-id="1c977-195">Some file systems, such as Docker containers and network shares, may not reliably send change notifications.</span></span> <span data-ttu-id="1c977-196">Définissez la variable d’environnement `DOTNET_USE_POLLING_FILE_WATCHER` sur `1` ou `true` pour interroger le système de fichiers à la recherche de changements toutes les quatre secondes (non configurable).</span><span class="sxs-lookup"><span data-stu-id="1c977-196">Set the `DOTNET_USE_POLLING_FILE_WATCHER` environment variable to `1` or `true` to poll the file system for changes every four seconds (not configurable).</span></span>

## <a name="glob-patterns"></a><span data-ttu-id="1c977-197">Modèles Glob</span><span class="sxs-lookup"><span data-stu-id="1c977-197">Glob patterns</span></span>

<span data-ttu-id="1c977-198">Les chemins de système de fichiers utilisent des modèles à caractères génériques appelés *modèles Glob (ou d’utilisation des caractères génériques)* .</span><span class="sxs-lookup"><span data-stu-id="1c977-198">File system paths use wildcard patterns called *glob (or globbing) patterns*.</span></span> <span data-ttu-id="1c977-199">Spécifiez les groupes de fichiers avec ces modèles.</span><span class="sxs-lookup"><span data-stu-id="1c977-199">Specify groups of files with these patterns.</span></span> <span data-ttu-id="1c977-200">Les deux caractères génériques sont `*` et `**` :</span><span class="sxs-lookup"><span data-stu-id="1c977-200">The two wildcard characters are `*` and `**`:</span></span>

**`*`**  
<span data-ttu-id="1c977-201">Établit une correspondance avec n’importe quel élément au niveau de dossier actuel, avec n’importe quel nom de fichier ou avec n’importe quelle extension de fichier.</span><span class="sxs-lookup"><span data-stu-id="1c977-201">Matches anything at the current folder level, any filename, or any file extension.</span></span> <span data-ttu-id="1c977-202">Les correspondances sont terminées par des caractères `/` et `.` dans le chemin des fichiers.</span><span class="sxs-lookup"><span data-stu-id="1c977-202">Matches are terminated by `/` and `.` characters in the file path.</span></span>

**`**`**  
<span data-ttu-id="1c977-203">Établit une correspondance avec n’importe quel élément sur plusieurs niveaux de répertoire.</span><span class="sxs-lookup"><span data-stu-id="1c977-203">Matches anything across multiple directory levels.</span></span> <span data-ttu-id="1c977-204">Peut être utilisé pour établir une correspondance avec plusieurs fichiers dans une hiérarchie de répertoires de manière récursive.</span><span class="sxs-lookup"><span data-stu-id="1c977-204">Can be used to recursively match many files within a directory hierarchy.</span></span>

<span data-ttu-id="1c977-205">**Exemples de modèles d’utilisation des caractères génériques**</span><span class="sxs-lookup"><span data-stu-id="1c977-205">**Glob pattern examples**</span></span>

**`directory/file.txt`**  
<span data-ttu-id="1c977-206">Établit une correspondance avec un fichier spécifique dans un répertoire spécifique.</span><span class="sxs-lookup"><span data-stu-id="1c977-206">Matches a specific file in a specific directory.</span></span>

**`directory/*.txt`**  
<span data-ttu-id="1c977-207">Établit une correspondance avec tous les fichiers ayant l’extension *.txt* dans un répertoire spécifique.</span><span class="sxs-lookup"><span data-stu-id="1c977-207">Matches all files with *.txt* extension in a specific directory.</span></span>

**`directory/*/appsettings.json`**  
<span data-ttu-id="1c977-208">Établit une correspondance avec tous les fichiers `appsettings.json` dans les répertoires situés exactement un niveau en dessous du dossier *répertoire*.</span><span class="sxs-lookup"><span data-stu-id="1c977-208">Matches all `appsettings.json` files in directories exactly one level below the *directory* folder.</span></span>

**`directory/**/*.txt`**  
<span data-ttu-id="1c977-209">Établit une correspondance avec tous les fichiers ayant l’extension *.txt* et se trouvant n’importe où sous le dossier *répertoire*.</span><span class="sxs-lookup"><span data-stu-id="1c977-209">Matches all files with *.txt* extension found anywhere under the *directory* folder.</span></span>
