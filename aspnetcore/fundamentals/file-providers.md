---
title: Fournisseurs de fichiers dans ASP.NET Core
author: guardrex
description: Découvrez comment ASP.NET Core fournit un accès au système de fichiers en utilisant des fournisseurs de fichiers.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 11/07/2019
uid: fundamentals/file-providers
ms.openlocfilehash: a454ca394546184968222ca2ca44d7159b19a12a
ms.sourcegitcommit: 851b921080fe8d719f54871770ccf6f78052584e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/09/2019
ms.locfileid: "74944306"
---
# <a name="file-providers-in-aspnet-core"></a><span data-ttu-id="1f9e5-103">Fournisseurs de fichiers dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="1f9e5-103">File Providers in ASP.NET Core</span></span>

<span data-ttu-id="1f9e5-104">Par [Steve Smith](https://ardalis.com/) et [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="1f9e5-104">By [Steve Smith](https://ardalis.com/) and [Luke Latham](https://github.com/guardrex)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="1f9e5-105">ASP.NET Core fournit un accès au système de fichiers en utilisant des fournisseurs de fichiers.</span><span class="sxs-lookup"><span data-stu-id="1f9e5-105">ASP.NET Core abstracts file system access through the use of File Providers.</span></span> <span data-ttu-id="1f9e5-106">Des fournisseurs de fichiers sont utilisés dans l’infrastructure ASP.NET Core :</span><span class="sxs-lookup"><span data-stu-id="1f9e5-106">File Providers are used throughout the ASP.NET Core framework:</span></span>

* <span data-ttu-id="1f9e5-107">`IWebHostEnvironment` expose la racine de [contenu](xref:fundamentals/index#content-root) et la [racine Web](xref:fundamentals/index#web-root) de l’application en tant que types de `IFileProvider`.</span><span class="sxs-lookup"><span data-stu-id="1f9e5-107">`IWebHostEnvironment` exposes the app's [content root](xref:fundamentals/index#content-root) and [web root](xref:fundamentals/index#web-root) as `IFileProvider` types.</span></span>
* <span data-ttu-id="1f9e5-108">[L’intergiciel (middleware) de fichiers statiques](xref:fundamentals/static-files) utilise des fournisseurs de fichiers pour localiser les fichiers statiques.</span><span class="sxs-lookup"><span data-stu-id="1f9e5-108">[Static File Middleware](xref:fundamentals/static-files) uses File Providers to locate static files.</span></span>
* <span data-ttu-id="1f9e5-109">[Razor](xref:mvc/views/razor) utilise des fournisseurs de fichiers pour localiser des pages et des vues.</span><span class="sxs-lookup"><span data-stu-id="1f9e5-109">[Razor](xref:mvc/views/razor) uses File Providers to locate pages and views.</span></span>
* <span data-ttu-id="1f9e5-110">Les outils .NET Core utilisent des fournisseurs de fichiers et des modèles d’utilisation des caractères génériques pour spécifier les fichiers qui doivent être publiés.</span><span class="sxs-lookup"><span data-stu-id="1f9e5-110">.NET Core tooling uses File Providers and glob patterns to specify which files should be published.</span></span>

<span data-ttu-id="1f9e5-111">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/file-providers/samples) ([procédure de téléchargement](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="1f9e5-111">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/file-providers/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="file-provider-interfaces"></a><span data-ttu-id="1f9e5-112">Interfaces de fournisseur de fichiers</span><span class="sxs-lookup"><span data-stu-id="1f9e5-112">File Provider interfaces</span></span>

<span data-ttu-id="1f9e5-113">L’interface principale est <xref:Microsoft.Extensions.FileProviders.IFileProvider>.</span><span class="sxs-lookup"><span data-stu-id="1f9e5-113">The primary interface is <xref:Microsoft.Extensions.FileProviders.IFileProvider>.</span></span> <span data-ttu-id="1f9e5-114">`IFileProvider` expose des méthodes pour :</span><span class="sxs-lookup"><span data-stu-id="1f9e5-114">`IFileProvider` exposes methods to:</span></span>

* <span data-ttu-id="1f9e5-115">Obtenir des informations sur le fichier (<xref:Microsoft.Extensions.FileProviders.IFileInfo>).</span><span class="sxs-lookup"><span data-stu-id="1f9e5-115">Obtain file information (<xref:Microsoft.Extensions.FileProviders.IFileInfo>).</span></span>
* <span data-ttu-id="1f9e5-116">Obtenir des informations sur le répertoire (<xref:Microsoft.Extensions.FileProviders.IDirectoryContents>).</span><span class="sxs-lookup"><span data-stu-id="1f9e5-116">Obtain directory information (<xref:Microsoft.Extensions.FileProviders.IDirectoryContents>).</span></span>
* <span data-ttu-id="1f9e5-117">Configurer des notifications de modification (à l’aide un <xref:Microsoft.Extensions.Primitives.IChangeToken>).</span><span class="sxs-lookup"><span data-stu-id="1f9e5-117">Set up change notifications (using an <xref:Microsoft.Extensions.Primitives.IChangeToken>).</span></span>

<span data-ttu-id="1f9e5-118">`IFileInfo` fournit des méthodes et propriétés d’utilisation des fichiers :</span><span class="sxs-lookup"><span data-stu-id="1f9e5-118">`IFileInfo` provides methods and properties for working with files:</span></span>

* <xref:Microsoft.Extensions.FileProviders.IFileInfo.Exists>
* <xref:Microsoft.Extensions.FileProviders.IFileInfo.IsDirectory>
* <xref:Microsoft.Extensions.FileProviders.IFileInfo.Name>
* <span data-ttu-id="1f9e5-119"><xref:Microsoft.Extensions.FileProviders.IFileInfo.Length> (en octets)</span><span class="sxs-lookup"><span data-stu-id="1f9e5-119"><xref:Microsoft.Extensions.FileProviders.IFileInfo.Length> (in bytes)</span></span>
* <span data-ttu-id="1f9e5-120"><xref:Microsoft.Extensions.FileProviders.IFileInfo.LastModified>date</span><span class="sxs-lookup"><span data-stu-id="1f9e5-120"><xref:Microsoft.Extensions.FileProviders.IFileInfo.LastModified> date</span></span>

<span data-ttu-id="1f9e5-121">Vous pouvez lire à partir du fichier en utilisant la méthode [IFileInfo.CreateReadStream](xref:Microsoft.Extensions.FileProviders.IFileInfo.CreateReadStream*).</span><span class="sxs-lookup"><span data-stu-id="1f9e5-121">You can read from the file using the [IFileInfo.CreateReadStream](xref:Microsoft.Extensions.FileProviders.IFileInfo.CreateReadStream*) method.</span></span>

<span data-ttu-id="1f9e5-122">L’exemple d’application montre comment configurer un fournisseur de fichiers dans `Startup.ConfigureServices` pour une utilisation dans toute l’application via [l’injection de dépendances](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="1f9e5-122">The sample app demonstrates how to configure a File Provider in `Startup.ConfigureServices` for use throughout the app via [dependency injection](xref:fundamentals/dependency-injection).</span></span>

## <a name="file-provider-implementations"></a><span data-ttu-id="1f9e5-123">Implémentations de fournisseur de fichiers</span><span class="sxs-lookup"><span data-stu-id="1f9e5-123">File Provider implementations</span></span>

<span data-ttu-id="1f9e5-124">Trois implémentations de `IFileProvider` sont disponibles.</span><span class="sxs-lookup"><span data-stu-id="1f9e5-124">Three implementations of `IFileProvider` are available.</span></span>

| <span data-ttu-id="1f9e5-125">Implémentation</span><span class="sxs-lookup"><span data-stu-id="1f9e5-125">Implementation</span></span> | <span data-ttu-id="1f9e5-126">Description</span><span class="sxs-lookup"><span data-stu-id="1f9e5-126">Description</span></span> |
| -------------- | ----------- |
| [<span data-ttu-id="1f9e5-127">PhysicalFileProvider</span><span class="sxs-lookup"><span data-stu-id="1f9e5-127">PhysicalFileProvider</span></span>](#physicalfileprovider) | <span data-ttu-id="1f9e5-128">Le fournisseur physique est utilisé pour accéder aux fichiers physiques du système.</span><span class="sxs-lookup"><span data-stu-id="1f9e5-128">The physical provider is used to access the system's physical files.</span></span> |
| [<span data-ttu-id="1f9e5-129">ManifestEmbeddedFileProvider</span><span class="sxs-lookup"><span data-stu-id="1f9e5-129">ManifestEmbeddedFileProvider</span></span>](#manifestembeddedfileprovider) | <span data-ttu-id="1f9e5-130">Le fournisseur incorporé de manifeste est utilisé pour accéder à des fichiers incorporés dans des assemblys.</span><span class="sxs-lookup"><span data-stu-id="1f9e5-130">The manifest embedded provider is used to access files embedded in assemblies.</span></span> |
| [<span data-ttu-id="1f9e5-131">CompositeFileProvider</span><span class="sxs-lookup"><span data-stu-id="1f9e5-131">CompositeFileProvider</span></span>](#compositefileprovider) | <span data-ttu-id="1f9e5-132">Le fournisseur composite est utilisé pour fournir un accès combiné à des fichiers et à des répertoires à partir d’un ou de plusieurs fournisseurs.</span><span class="sxs-lookup"><span data-stu-id="1f9e5-132">The composite provider is used to provide combined access to files and directories from one or more other providers.</span></span> |

### <a name="physicalfileprovider"></a><span data-ttu-id="1f9e5-133">PhysicalFileProvider</span><span class="sxs-lookup"><span data-stu-id="1f9e5-133">PhysicalFileProvider</span></span>

<span data-ttu-id="1f9e5-134">La classe <xref:Microsoft.Extensions.FileProviders.PhysicalFileProvider> fournit un accès au système de fichiers physique.</span><span class="sxs-lookup"><span data-stu-id="1f9e5-134">The <xref:Microsoft.Extensions.FileProviders.PhysicalFileProvider> provides access to the physical file system.</span></span> <span data-ttu-id="1f9e5-135">`PhysicalFileProvider` utilise le type <xref:System.IO.File?displayProperty=fullName> (pour le fournisseur physique) et définissant comme portée tous les chemins d’un répertoire et de ses enfants.</span><span class="sxs-lookup"><span data-stu-id="1f9e5-135">`PhysicalFileProvider` uses the <xref:System.IO.File?displayProperty=fullName> type (for the physical provider) and scopes all paths to a directory and its children.</span></span> <span data-ttu-id="1f9e5-136">Cette portée empêche l’accès au système de fichiers en dehors du répertoire spécifié et ses enfants.</span><span class="sxs-lookup"><span data-stu-id="1f9e5-136">This scoping prevents access to the file system outside of the specified directory and its children.</span></span> <span data-ttu-id="1f9e5-137">Le scénario le plus courant pour créer et utiliser un `PhysicalFileProvider` consiste à demander un `IFileProvider` dans un constructeur par le biais de l’[injection de dépendances](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="1f9e5-137">The most common scenario for creating and using a `PhysicalFileProvider` is to request an `IFileProvider` in a constructor through [dependency injection](xref:fundamentals/dependency-injection).</span></span>

<span data-ttu-id="1f9e5-138">Lors de l’instanciation directe de ce fournisseur, un chemin de répertoire est obligatoire et sert de chemin de base pour toutes les demandes effectuées à l’aide du fournisseur.</span><span class="sxs-lookup"><span data-stu-id="1f9e5-138">When instantiating this provider directly, a directory path is required and serves as the base path for all requests made using the provider.</span></span>

<span data-ttu-id="1f9e5-139">Le code suivant montre comment créer un `PhysicalFileProvider` et l’utiliser pour obtenir le contenu du répertoire et les informations sur le fichier :</span><span class="sxs-lookup"><span data-stu-id="1f9e5-139">The following code shows how to create a `PhysicalFileProvider` and use it to obtain directory contents and file information:</span></span>

```csharp
var provider = new PhysicalFileProvider(applicationRoot);
var contents = provider.GetDirectoryContents(string.Empty);
var fileInfo = provider.GetFileInfo("wwwroot/js/site.js");
```

<span data-ttu-id="1f9e5-140">Types dans l’exemple précédent :</span><span class="sxs-lookup"><span data-stu-id="1f9e5-140">Types in the preceding example:</span></span>

* <span data-ttu-id="1f9e5-141">`provider` est un `IFileProvider`.</span><span class="sxs-lookup"><span data-stu-id="1f9e5-141">`provider` is an `IFileProvider`.</span></span>
* <span data-ttu-id="1f9e5-142">`contents` est un `IDirectoryContents`.</span><span class="sxs-lookup"><span data-stu-id="1f9e5-142">`contents` is an `IDirectoryContents`.</span></span>
* <span data-ttu-id="1f9e5-143">`fileInfo` est un `IFileInfo`.</span><span class="sxs-lookup"><span data-stu-id="1f9e5-143">`fileInfo` is an `IFileInfo`.</span></span>

<span data-ttu-id="1f9e5-144">Le fournisseur de fichiers peut être utilisé pour itérer au sein du répertoire spécifié par `applicationRoot` ou pour appeler `GetFileInfo` afin d’obtenir des informations sur un fichier.</span><span class="sxs-lookup"><span data-stu-id="1f9e5-144">The File Provider can be used to iterate through the directory specified by `applicationRoot` or call `GetFileInfo` to obtain a file's information.</span></span> <span data-ttu-id="1f9e5-145">Le fournisseur de fichiers n’a pas accès en dehors du répertoire `applicationRoot`.</span><span class="sxs-lookup"><span data-stu-id="1f9e5-145">The File Provider has no access outside of the `applicationRoot` directory.</span></span>

<span data-ttu-id="1f9e5-146">L’exemple d’application crée le fournisseur dans la classe `Startup.ConfigureServices` de l’application à l’aide de [IHostingEnvironment.ContentRootFileProvider](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.ContentRootFileProvider) :</span><span class="sxs-lookup"><span data-stu-id="1f9e5-146">The sample app creates the provider in the app's `Startup.ConfigureServices` class using [IHostingEnvironment.ContentRootFileProvider](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.ContentRootFileProvider):</span></span>

```csharp
var physicalProvider = _env.ContentRootFileProvider;
```

### <a name="manifestembeddedfileprovider"></a><span data-ttu-id="1f9e5-147">ManifestEmbeddedFileProvider</span><span class="sxs-lookup"><span data-stu-id="1f9e5-147">ManifestEmbeddedFileProvider</span></span>

<span data-ttu-id="1f9e5-148"><xref:Microsoft.Extensions.FileProviders.ManifestEmbeddedFileProvider> est utilisé pour accéder à des fichiers incorporés dans des assemblys.</span><span class="sxs-lookup"><span data-stu-id="1f9e5-148">The <xref:Microsoft.Extensions.FileProviders.ManifestEmbeddedFileProvider> is used to access files embedded within assemblies.</span></span> <span data-ttu-id="1f9e5-149">`ManifestEmbeddedFileProvider` utilise un manifeste compilé dans l’assembly pour reconstruire les chemins d’accès d’origine des fichiers intégrés.</span><span class="sxs-lookup"><span data-stu-id="1f9e5-149">The `ManifestEmbeddedFileProvider` uses a manifest compiled into the assembly to reconstruct the original paths of the embedded files.</span></span>

<span data-ttu-id="1f9e5-150">Ajoutez une référence de package au projet pour le package [Microsoft.Extensions.FileProviders.Embedded](https://www.nuget.org/packages/Microsoft.Extensions.FileProviders.Embedded).</span><span class="sxs-lookup"><span data-stu-id="1f9e5-150">Add a package reference to the project for the [Microsoft.Extensions.FileProviders.Embedded](https://www.nuget.org/packages/Microsoft.Extensions.FileProviders.Embedded) package.</span></span>

<span data-ttu-id="1f9e5-151">Pour générer un manifeste des fichiers incorporés, définissez la propriété `<GenerateEmbeddedFilesManifest>` sur `true`.</span><span class="sxs-lookup"><span data-stu-id="1f9e5-151">To generate a manifest of the embedded files, set the `<GenerateEmbeddedFilesManifest>` property to `true`.</span></span> <span data-ttu-id="1f9e5-152">Spécifiez les fichiers à incorporer avec [\<EmbeddedResource](/dotnet/core/tools/csproj#default-compilation-includes-in-net-core-projects) :</span><span class="sxs-lookup"><span data-stu-id="1f9e5-152">Specify the files to embed with [\<EmbeddedResource>](/dotnet/core/tools/csproj#default-compilation-includes-in-net-core-projects):</span></span>

[!code-csharp[](file-providers/samples/3.x/FileProviderSample/FileProviderSample.csproj?highlight=5,13)]

<span data-ttu-id="1f9e5-153">Utilisez les [modèles glob](#glob-patterns) pour spécifier un ou plusieurs fichiers à incorporer dans l’assembly.</span><span class="sxs-lookup"><span data-stu-id="1f9e5-153">Use [glob patterns](#glob-patterns) to specify one or more files to embed into the assembly.</span></span>

<span data-ttu-id="1f9e5-154">L’exemple d’application crée un `ManifestEmbeddedFileProvider` et transmet l’assembly en cours d’exécution à son constructeur.</span><span class="sxs-lookup"><span data-stu-id="1f9e5-154">The sample app creates an `ManifestEmbeddedFileProvider` and passes the currently executing assembly to its constructor.</span></span>

<span data-ttu-id="1f9e5-155">*Startup.cs* :</span><span class="sxs-lookup"><span data-stu-id="1f9e5-155">*Startup.cs*:</span></span>

```csharp
var manifestEmbeddedProvider = 
    new ManifestEmbeddedFileProvider(typeof(Program).Assembly);
```

<span data-ttu-id="1f9e5-156">Des surcharges supplémentaires vous permettent de :</span><span class="sxs-lookup"><span data-stu-id="1f9e5-156">Additional overloads allow you to:</span></span>

* <span data-ttu-id="1f9e5-157">Spécifier un chemin de fichier relatif.</span><span class="sxs-lookup"><span data-stu-id="1f9e5-157">Specify a relative file path.</span></span>
* <span data-ttu-id="1f9e5-158">Définir la portée de fichiers sur une date de dernière modification.</span><span class="sxs-lookup"><span data-stu-id="1f9e5-158">Scope files to a last modified date.</span></span>
* <span data-ttu-id="1f9e5-159">Nommer la ressource incorporée contenant le manifeste de fichier incorporé.</span><span class="sxs-lookup"><span data-stu-id="1f9e5-159">Name the embedded resource containing the embedded file manifest.</span></span>

| <span data-ttu-id="1f9e5-160">Surcharge</span><span class="sxs-lookup"><span data-stu-id="1f9e5-160">Overload</span></span> | <span data-ttu-id="1f9e5-161">Description</span><span class="sxs-lookup"><span data-stu-id="1f9e5-161">Description</span></span> |
| -------- | ----------- |
| `ManifestEmbeddedFileProvider(Assembly, String)` | <span data-ttu-id="1f9e5-162">Accepte un paramètre de chemin d'accès relatif `root` facultatif.</span><span class="sxs-lookup"><span data-stu-id="1f9e5-162">Accepts an optional `root` relative path parameter.</span></span> <span data-ttu-id="1f9e5-163">Spécifiez `root` pour définir la portée des appels à <xref:Microsoft.Extensions.FileProviders.IFileProvider.GetDirectoryContents*> sur ces ressources sous le chemin d’accès fourni.</span><span class="sxs-lookup"><span data-stu-id="1f9e5-163">Specify the `root` to scope calls to <xref:Microsoft.Extensions.FileProviders.IFileProvider.GetDirectoryContents*> to those resources under the provided path.</span></span> |
| `ManifestEmbeddedFileProvider(Assembly, String, DateTimeOffset)` | <span data-ttu-id="1f9e5-164">Accepte un paramètre de chemin relatif `root` facultatif et un paramètre de date `lastModified` (<xref:System.DateTimeOffset>).</span><span class="sxs-lookup"><span data-stu-id="1f9e5-164">Accepts an optional `root` relative path parameter and a `lastModified` date (<xref:System.DateTimeOffset>) parameter.</span></span> <span data-ttu-id="1f9e5-165">La date `lastModified` définit la portée de la dernière date de modification pour les instances <xref:Microsoft.Extensions.FileProviders.IFileInfo> retournées par <xref:Microsoft.Extensions.FileProviders.IFileProvider>.</span><span class="sxs-lookup"><span data-stu-id="1f9e5-165">The `lastModified` date scopes the last modification date for the <xref:Microsoft.Extensions.FileProviders.IFileInfo> instances returned by the <xref:Microsoft.Extensions.FileProviders.IFileProvider>.</span></span> |
| `ManifestEmbeddedFileProvider(Assembly, String, String, DateTimeOffset)` | <span data-ttu-id="1f9e5-166">Accepte un chemin d'accès relatif `root` facultatif, une date `lastModified` et des paramètres `manifestName`.</span><span class="sxs-lookup"><span data-stu-id="1f9e5-166">Accepts an optional `root` relative path, `lastModified` date, and `manifestName` parameters.</span></span> <span data-ttu-id="1f9e5-167">`manifestName` représente le nom de la ressource incorporée contenant la manifeste.</span><span class="sxs-lookup"><span data-stu-id="1f9e5-167">The `manifestName` represents the name of the embedded resource containing the manifest.</span></span> |

### <a name="compositefileprovider"></a><span data-ttu-id="1f9e5-168">CompositeFileProvider</span><span class="sxs-lookup"><span data-stu-id="1f9e5-168">CompositeFileProvider</span></span>

<span data-ttu-id="1f9e5-169"><xref:Microsoft.Extensions.FileProviders.CompositeFileProvider> combine des instances `IFileProvider`, en exposant une interface unique qui permet d’utiliser des fichiers de différents fournisseurs.</span><span class="sxs-lookup"><span data-stu-id="1f9e5-169">The <xref:Microsoft.Extensions.FileProviders.CompositeFileProvider> combines `IFileProvider` instances, exposing a single interface for working with files from multiple providers.</span></span> <span data-ttu-id="1f9e5-170">Quand vous créez `CompositeFileProvider`, vous passez une ou plusieurs instances `IFileProvider` à son constructeur.</span><span class="sxs-lookup"><span data-stu-id="1f9e5-170">When creating the `CompositeFileProvider`, pass one or more `IFileProvider` instances to its constructor.</span></span>

<span data-ttu-id="1f9e5-171">Dans l’exemple d’application, un `PhysicalFileProvider` et un `ManifestEmbeddedFileProvider` fournissent des fichiers à un `CompositeFileProvider` inscrit dans le conteneur de service de l’application :</span><span class="sxs-lookup"><span data-stu-id="1f9e5-171">In the sample app, a `PhysicalFileProvider` and a `ManifestEmbeddedFileProvider` provide files to a `CompositeFileProvider` registered in the app's service container:</span></span>

[!code-csharp[](file-providers/samples/3.x/FileProviderSample/Startup.cs?name=snippet1)]

## <a name="watch-for-changes"></a><span data-ttu-id="1f9e5-172">Suivre les modifications apportées</span><span class="sxs-lookup"><span data-stu-id="1f9e5-172">Watch for changes</span></span>

<span data-ttu-id="1f9e5-173">La méthode [IFileProvider.Watch](xref:Microsoft.Extensions.FileProviders.IFileProvider.Watch*) offre un moyen d’observer un ou plusieurs fichiers ou répertoires afin de détecter les changements.</span><span class="sxs-lookup"><span data-stu-id="1f9e5-173">The [IFileProvider.Watch](xref:Microsoft.Extensions.FileProviders.IFileProvider.Watch*) method provides a scenario to watch one or more files or directories for changes.</span></span> <span data-ttu-id="1f9e5-174">`Watch` accepte une chaîne de chemin, qui peut utiliser des [modèles d’utilisation des caractères génériques](#glob-patterns) pour spécifier plusieurs fichiers.</span><span class="sxs-lookup"><span data-stu-id="1f9e5-174">`Watch` accepts a path string, which can use [glob patterns](#glob-patterns) to specify multiple files.</span></span> <span data-ttu-id="1f9e5-175">`Watch`Retourne un <xref:Microsoft.Extensions.Primitives.IChangeToken>.</span><span class="sxs-lookup"><span data-stu-id="1f9e5-175">`Watch` returns an <xref:Microsoft.Extensions.Primitives.IChangeToken>.</span></span> <span data-ttu-id="1f9e5-176">Le jeton de modification expose :</span><span class="sxs-lookup"><span data-stu-id="1f9e5-176">The change token exposes:</span></span>

* <span data-ttu-id="1f9e5-177"><xref:Microsoft.Extensions.Primitives.IChangeToken.HasChanged> &ndash; une propriété qui peut être inspectée pour déterminer si une modification a eu lieu.</span><span class="sxs-lookup"><span data-stu-id="1f9e5-177"><xref:Microsoft.Extensions.Primitives.IChangeToken.HasChanged> &ndash; A property that can be inspected to determine if a change has occurred.</span></span>
* <span data-ttu-id="1f9e5-178"><xref:Microsoft.Extensions.Primitives.IChangeToken.RegisterChangeCallback*>&ndash; appelée quand des modifications sont détectées dans la chaîne de chemin spécifiée.</span><span class="sxs-lookup"><span data-stu-id="1f9e5-178"><xref:Microsoft.Extensions.Primitives.IChangeToken.RegisterChangeCallback*> &ndash; Called when changes are detected to the specified path string.</span></span> <span data-ttu-id="1f9e5-179">Chaque jeton de modification appelle uniquement son rappel associé en réponse à un changement unique.</span><span class="sxs-lookup"><span data-stu-id="1f9e5-179">Each change token only calls its associated callback in response to a single change.</span></span> <span data-ttu-id="1f9e5-180">Pour activer une surveillance constante, vous pouvez utiliser une <xref:System.Threading.Tasks.TaskCompletionSource`1> comme indiqué ci-dessous, ou recréer des instances `IChangeToken` en réponse aux changements.</span><span class="sxs-lookup"><span data-stu-id="1f9e5-180">To enable constant monitoring, use a <xref:System.Threading.Tasks.TaskCompletionSource`1> (shown below) or recreate `IChangeToken` instances in response to changes.</span></span>

<span data-ttu-id="1f9e5-181">Dans l’exemple d’application, l’application console *WatchConsole* est configurée pour afficher un message chaque fois qu’un fichier texte est modifié :</span><span class="sxs-lookup"><span data-stu-id="1f9e5-181">In the sample app, the *WatchConsole* console app is configured to display a message whenever a text file is modified:</span></span>

[!code-csharp[](file-providers/samples/3.x/WatchConsole/Program.cs?name=snippet1&highlight=1-2,16,19-20)]

<span data-ttu-id="1f9e5-182">Certains systèmes de fichiers, comme les conteneurs Docker et les partages réseau, peuvent ne pas envoyer de manière fiable les notifications de modifications.</span><span class="sxs-lookup"><span data-stu-id="1f9e5-182">Some file systems, such as Docker containers and network shares, may not reliably send change notifications.</span></span> <span data-ttu-id="1f9e5-183">Définissez la variable d’environnement `DOTNET_USE_POLLING_FILE_WATCHER` sur `1` ou `true` pour interroger le système de fichiers à la recherche de changements toutes les quatre secondes (non configurable).</span><span class="sxs-lookup"><span data-stu-id="1f9e5-183">Set the `DOTNET_USE_POLLING_FILE_WATCHER` environment variable to `1` or `true` to poll the file system for changes every four seconds (not configurable).</span></span>

## <a name="glob-patterns"></a><span data-ttu-id="1f9e5-184">Modèles Glob</span><span class="sxs-lookup"><span data-stu-id="1f9e5-184">Glob patterns</span></span>

<span data-ttu-id="1f9e5-185">Les chemins de système de fichiers utilisent des modèles à caractères génériques appelés *modèles Glob (ou d’utilisation des caractères génériques)* .</span><span class="sxs-lookup"><span data-stu-id="1f9e5-185">File system paths use wildcard patterns called *glob (or globbing) patterns*.</span></span> <span data-ttu-id="1f9e5-186">Spécifiez les groupes de fichiers avec ces modèles.</span><span class="sxs-lookup"><span data-stu-id="1f9e5-186">Specify groups of files with these patterns.</span></span> <span data-ttu-id="1f9e5-187">Les deux caractères génériques sont `*` et `**` :</span><span class="sxs-lookup"><span data-stu-id="1f9e5-187">The two wildcard characters are `*` and `**`:</span></span>

**`*`**  
<span data-ttu-id="1f9e5-188">Établit une correspondance avec n’importe quel élément au niveau de dossier actuel, avec n’importe quel nom de fichier ou avec n’importe quelle extension de fichier.</span><span class="sxs-lookup"><span data-stu-id="1f9e5-188">Matches anything at the current folder level, any filename, or any file extension.</span></span> <span data-ttu-id="1f9e5-189">Les correspondances sont terminées par des caractères `/` et `.` dans le chemin des fichiers.</span><span class="sxs-lookup"><span data-stu-id="1f9e5-189">Matches are terminated by `/` and `.` characters in the file path.</span></span>

**`**`**  
<span data-ttu-id="1f9e5-190">Établit une correspondance avec n’importe quel élément sur plusieurs niveaux de répertoire.</span><span class="sxs-lookup"><span data-stu-id="1f9e5-190">Matches anything across multiple directory levels.</span></span> <span data-ttu-id="1f9e5-191">Peut être utilisé pour établir une correspondance avec plusieurs fichiers dans une hiérarchie de répertoires de manière récursive.</span><span class="sxs-lookup"><span data-stu-id="1f9e5-191">Can be used to recursively match many files within a directory hierarchy.</span></span>

<span data-ttu-id="1f9e5-192">**Exemples de modèles d’utilisation des caractères génériques**</span><span class="sxs-lookup"><span data-stu-id="1f9e5-192">**Glob pattern examples**</span></span>

**`directory/file.txt`**  
<span data-ttu-id="1f9e5-193">Établit une correspondance avec un fichier spécifique dans un répertoire spécifique.</span><span class="sxs-lookup"><span data-stu-id="1f9e5-193">Matches a specific file in a specific directory.</span></span>

**`directory/*.txt`**  
<span data-ttu-id="1f9e5-194">Établit une correspondance avec tous les fichiers ayant l’extension *.txt* dans un répertoire spécifique.</span><span class="sxs-lookup"><span data-stu-id="1f9e5-194">Matches all files with *.txt* extension in a specific directory.</span></span>

**`directory/*/appsettings.json`**  
<span data-ttu-id="1f9e5-195">Établit une correspondance avec tous les fichiers `appsettings.json` dans les répertoires situés exactement un niveau en dessous du dossier *répertoire*.</span><span class="sxs-lookup"><span data-stu-id="1f9e5-195">Matches all `appsettings.json` files in directories exactly one level below the *directory* folder.</span></span>

**`directory/**/*.txt`**  
<span data-ttu-id="1f9e5-196">Établit une correspondance avec tous les fichiers ayant l’extension *.txt* et se trouvant n’importe où sous le dossier *répertoire*.</span><span class="sxs-lookup"><span data-stu-id="1f9e5-196">Matches all files with *.txt* extension found anywhere under the *directory* folder.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="1f9e5-197">ASP.NET Core fournit un accès au système de fichiers en utilisant des fournisseurs de fichiers.</span><span class="sxs-lookup"><span data-stu-id="1f9e5-197">ASP.NET Core abstracts file system access through the use of File Providers.</span></span> <span data-ttu-id="1f9e5-198">Des fournisseurs de fichiers sont utilisés dans l’infrastructure ASP.NET Core :</span><span class="sxs-lookup"><span data-stu-id="1f9e5-198">File Providers are used throughout the ASP.NET Core framework:</span></span>

* <span data-ttu-id="1f9e5-199"><xref:Microsoft.Extensions.Hosting.IHostingEnvironment> expose la racine de [contenu](xref:fundamentals/index#content-root) et la [racine Web](xref:fundamentals/index#web-root) de l’application en tant que types de `IFileProvider`.</span><span class="sxs-lookup"><span data-stu-id="1f9e5-199"><xref:Microsoft.Extensions.Hosting.IHostingEnvironment> exposes the app's [content root](xref:fundamentals/index#content-root) and [web root](xref:fundamentals/index#web-root) as `IFileProvider` types.</span></span>
* <span data-ttu-id="1f9e5-200">[L’intergiciel (middleware) de fichiers statiques](xref:fundamentals/static-files) utilise des fournisseurs de fichiers pour localiser les fichiers statiques.</span><span class="sxs-lookup"><span data-stu-id="1f9e5-200">[Static File Middleware](xref:fundamentals/static-files) uses File Providers to locate static files.</span></span>
* <span data-ttu-id="1f9e5-201">[Razor](xref:mvc/views/razor) utilise des fournisseurs de fichiers pour localiser des pages et des vues.</span><span class="sxs-lookup"><span data-stu-id="1f9e5-201">[Razor](xref:mvc/views/razor) uses File Providers to locate pages and views.</span></span>
* <span data-ttu-id="1f9e5-202">Les outils .NET Core utilisent des fournisseurs de fichiers et des modèles d’utilisation des caractères génériques pour spécifier les fichiers qui doivent être publiés.</span><span class="sxs-lookup"><span data-stu-id="1f9e5-202">.NET Core tooling uses File Providers and glob patterns to specify which files should be published.</span></span>

<span data-ttu-id="1f9e5-203">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/file-providers/samples) ([procédure de téléchargement](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="1f9e5-203">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/file-providers/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="file-provider-interfaces"></a><span data-ttu-id="1f9e5-204">Interfaces de fournisseur de fichiers</span><span class="sxs-lookup"><span data-stu-id="1f9e5-204">File Provider interfaces</span></span>

<span data-ttu-id="1f9e5-205">L’interface principale est <xref:Microsoft.Extensions.FileProviders.IFileProvider>.</span><span class="sxs-lookup"><span data-stu-id="1f9e5-205">The primary interface is <xref:Microsoft.Extensions.FileProviders.IFileProvider>.</span></span> <span data-ttu-id="1f9e5-206">`IFileProvider` expose des méthodes pour :</span><span class="sxs-lookup"><span data-stu-id="1f9e5-206">`IFileProvider` exposes methods to:</span></span>

* <span data-ttu-id="1f9e5-207">Obtenir des informations sur le fichier (<xref:Microsoft.Extensions.FileProviders.IFileInfo>).</span><span class="sxs-lookup"><span data-stu-id="1f9e5-207">Obtain file information (<xref:Microsoft.Extensions.FileProviders.IFileInfo>).</span></span>
* <span data-ttu-id="1f9e5-208">Obtenir des informations sur le répertoire (<xref:Microsoft.Extensions.FileProviders.IDirectoryContents>).</span><span class="sxs-lookup"><span data-stu-id="1f9e5-208">Obtain directory information (<xref:Microsoft.Extensions.FileProviders.IDirectoryContents>).</span></span>
* <span data-ttu-id="1f9e5-209">Configurer des notifications de modification (à l’aide un <xref:Microsoft.Extensions.Primitives.IChangeToken>).</span><span class="sxs-lookup"><span data-stu-id="1f9e5-209">Set up change notifications (using an <xref:Microsoft.Extensions.Primitives.IChangeToken>).</span></span>

<span data-ttu-id="1f9e5-210">`IFileInfo` fournit des méthodes et propriétés d’utilisation des fichiers :</span><span class="sxs-lookup"><span data-stu-id="1f9e5-210">`IFileInfo` provides methods and properties for working with files:</span></span>

* <xref:Microsoft.Extensions.FileProviders.IFileInfo.Exists>
* <xref:Microsoft.Extensions.FileProviders.IFileInfo.IsDirectory>
* <xref:Microsoft.Extensions.FileProviders.IFileInfo.Name>
* <span data-ttu-id="1f9e5-211"><xref:Microsoft.Extensions.FileProviders.IFileInfo.Length> (en octets)</span><span class="sxs-lookup"><span data-stu-id="1f9e5-211"><xref:Microsoft.Extensions.FileProviders.IFileInfo.Length> (in bytes)</span></span>
* <span data-ttu-id="1f9e5-212"><xref:Microsoft.Extensions.FileProviders.IFileInfo.LastModified>date</span><span class="sxs-lookup"><span data-stu-id="1f9e5-212"><xref:Microsoft.Extensions.FileProviders.IFileInfo.LastModified> date</span></span>

<span data-ttu-id="1f9e5-213">Vous pouvez lire à partir du fichier en utilisant la méthode [IFileInfo.CreateReadStream](xref:Microsoft.Extensions.FileProviders.IFileInfo.CreateReadStream*).</span><span class="sxs-lookup"><span data-stu-id="1f9e5-213">You can read from the file using the [IFileInfo.CreateReadStream](xref:Microsoft.Extensions.FileProviders.IFileInfo.CreateReadStream*) method.</span></span>

<span data-ttu-id="1f9e5-214">L’exemple d’application montre comment configurer un fournisseur de fichiers dans `Startup.ConfigureServices` pour une utilisation dans toute l’application via [l’injection de dépendances](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="1f9e5-214">The sample app demonstrates how to configure a File Provider in `Startup.ConfigureServices` for use throughout the app via [dependency injection](xref:fundamentals/dependency-injection).</span></span>

## <a name="file-provider-implementations"></a><span data-ttu-id="1f9e5-215">Implémentations de fournisseur de fichiers</span><span class="sxs-lookup"><span data-stu-id="1f9e5-215">File Provider implementations</span></span>

<span data-ttu-id="1f9e5-216">Trois implémentations de `IFileProvider` sont disponibles.</span><span class="sxs-lookup"><span data-stu-id="1f9e5-216">Three implementations of `IFileProvider` are available.</span></span>

| <span data-ttu-id="1f9e5-217">Implémentation</span><span class="sxs-lookup"><span data-stu-id="1f9e5-217">Implementation</span></span> | <span data-ttu-id="1f9e5-218">Description</span><span class="sxs-lookup"><span data-stu-id="1f9e5-218">Description</span></span> |
| -------------- | ----------- |
| [<span data-ttu-id="1f9e5-219">PhysicalFileProvider</span><span class="sxs-lookup"><span data-stu-id="1f9e5-219">PhysicalFileProvider</span></span>](#physicalfileprovider) | <span data-ttu-id="1f9e5-220">Le fournisseur physique est utilisé pour accéder aux fichiers physiques du système.</span><span class="sxs-lookup"><span data-stu-id="1f9e5-220">The physical provider is used to access the system's physical files.</span></span> |
| [<span data-ttu-id="1f9e5-221">ManifestEmbeddedFileProvider</span><span class="sxs-lookup"><span data-stu-id="1f9e5-221">ManifestEmbeddedFileProvider</span></span>](#manifestembeddedfileprovider) | <span data-ttu-id="1f9e5-222">Le fournisseur incorporé de manifeste est utilisé pour accéder à des fichiers incorporés dans des assemblys.</span><span class="sxs-lookup"><span data-stu-id="1f9e5-222">The manifest embedded provider is used to access files embedded in assemblies.</span></span> |
| [<span data-ttu-id="1f9e5-223">CompositeFileProvider</span><span class="sxs-lookup"><span data-stu-id="1f9e5-223">CompositeFileProvider</span></span>](#compositefileprovider) | <span data-ttu-id="1f9e5-224">Le fournisseur composite est utilisé pour fournir un accès combiné à des fichiers et à des répertoires à partir d’un ou de plusieurs fournisseurs.</span><span class="sxs-lookup"><span data-stu-id="1f9e5-224">The composite provider is used to provide combined access to files and directories from one or more other providers.</span></span> |

### <a name="physicalfileprovider"></a><span data-ttu-id="1f9e5-225">PhysicalFileProvider</span><span class="sxs-lookup"><span data-stu-id="1f9e5-225">PhysicalFileProvider</span></span>

<span data-ttu-id="1f9e5-226">La classe <xref:Microsoft.Extensions.FileProviders.PhysicalFileProvider> fournit un accès au système de fichiers physique.</span><span class="sxs-lookup"><span data-stu-id="1f9e5-226">The <xref:Microsoft.Extensions.FileProviders.PhysicalFileProvider> provides access to the physical file system.</span></span> <span data-ttu-id="1f9e5-227">`PhysicalFileProvider` utilise le type <xref:System.IO.File?displayProperty=fullName> (pour le fournisseur physique) et définissant comme portée tous les chemins d’un répertoire et de ses enfants.</span><span class="sxs-lookup"><span data-stu-id="1f9e5-227">`PhysicalFileProvider` uses the <xref:System.IO.File?displayProperty=fullName> type (for the physical provider) and scopes all paths to a directory and its children.</span></span> <span data-ttu-id="1f9e5-228">Cette portée empêche l’accès au système de fichiers en dehors du répertoire spécifié et ses enfants.</span><span class="sxs-lookup"><span data-stu-id="1f9e5-228">This scoping prevents access to the file system outside of the specified directory and its children.</span></span> <span data-ttu-id="1f9e5-229">Le scénario le plus courant pour créer et utiliser un `PhysicalFileProvider` consiste à demander un `IFileProvider` dans un constructeur par le biais de l’[injection de dépendances](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="1f9e5-229">The most common scenario for creating and using a `PhysicalFileProvider` is to request an `IFileProvider` in a constructor through [dependency injection](xref:fundamentals/dependency-injection).</span></span>

<span data-ttu-id="1f9e5-230">Lors de l’instanciation directe de ce fournisseur, un chemin de répertoire est obligatoire et sert de chemin de base pour toutes les demandes effectuées à l’aide du fournisseur.</span><span class="sxs-lookup"><span data-stu-id="1f9e5-230">When instantiating this provider directly, a directory path is required and serves as the base path for all requests made using the provider.</span></span>

<span data-ttu-id="1f9e5-231">Le code suivant montre comment créer un `PhysicalFileProvider` et l’utiliser pour obtenir le contenu du répertoire et les informations sur le fichier :</span><span class="sxs-lookup"><span data-stu-id="1f9e5-231">The following code shows how to create a `PhysicalFileProvider` and use it to obtain directory contents and file information:</span></span>

```csharp
var provider = new PhysicalFileProvider(applicationRoot);
var contents = provider.GetDirectoryContents(string.Empty);
var fileInfo = provider.GetFileInfo("wwwroot/js/site.js");
```

<span data-ttu-id="1f9e5-232">Types dans l’exemple précédent :</span><span class="sxs-lookup"><span data-stu-id="1f9e5-232">Types in the preceding example:</span></span>

* <span data-ttu-id="1f9e5-233">`provider` est un `IFileProvider`.</span><span class="sxs-lookup"><span data-stu-id="1f9e5-233">`provider` is an `IFileProvider`.</span></span>
* <span data-ttu-id="1f9e5-234">`contents` est un `IDirectoryContents`.</span><span class="sxs-lookup"><span data-stu-id="1f9e5-234">`contents` is an `IDirectoryContents`.</span></span>
* <span data-ttu-id="1f9e5-235">`fileInfo` est un `IFileInfo`.</span><span class="sxs-lookup"><span data-stu-id="1f9e5-235">`fileInfo` is an `IFileInfo`.</span></span>

<span data-ttu-id="1f9e5-236">Le fournisseur de fichiers peut être utilisé pour itérer au sein du répertoire spécifié par `applicationRoot` ou pour appeler `GetFileInfo` afin d’obtenir des informations sur un fichier.</span><span class="sxs-lookup"><span data-stu-id="1f9e5-236">The File Provider can be used to iterate through the directory specified by `applicationRoot` or call `GetFileInfo` to obtain a file's information.</span></span> <span data-ttu-id="1f9e5-237">Le fournisseur de fichiers n’a pas accès en dehors du répertoire `applicationRoot`.</span><span class="sxs-lookup"><span data-stu-id="1f9e5-237">The File Provider has no access outside of the `applicationRoot` directory.</span></span>

<span data-ttu-id="1f9e5-238">L’exemple d’application crée le fournisseur dans la classe `Startup.ConfigureServices` de l’application à l’aide de [IHostingEnvironment.ContentRootFileProvider](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.ContentRootFileProvider) :</span><span class="sxs-lookup"><span data-stu-id="1f9e5-238">The sample app creates the provider in the app's `Startup.ConfigureServices` class using [IHostingEnvironment.ContentRootFileProvider](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.ContentRootFileProvider):</span></span>

```csharp
var physicalProvider = _env.ContentRootFileProvider;
```

### <a name="manifestembeddedfileprovider"></a><span data-ttu-id="1f9e5-239">ManifestEmbeddedFileProvider</span><span class="sxs-lookup"><span data-stu-id="1f9e5-239">ManifestEmbeddedFileProvider</span></span>

<span data-ttu-id="1f9e5-240"><xref:Microsoft.Extensions.FileProviders.ManifestEmbeddedFileProvider> est utilisé pour accéder à des fichiers incorporés dans des assemblys.</span><span class="sxs-lookup"><span data-stu-id="1f9e5-240">The <xref:Microsoft.Extensions.FileProviders.ManifestEmbeddedFileProvider> is used to access files embedded within assemblies.</span></span> <span data-ttu-id="1f9e5-241">`ManifestEmbeddedFileProvider` utilise un manifeste compilé dans l’assembly pour reconstruire les chemins d’accès d’origine des fichiers intégrés.</span><span class="sxs-lookup"><span data-stu-id="1f9e5-241">The `ManifestEmbeddedFileProvider` uses a manifest compiled into the assembly to reconstruct the original paths of the embedded files.</span></span>

<span data-ttu-id="1f9e5-242">Pour générer un manifeste des fichiers incorporés, définissez la propriété `<GenerateEmbeddedFilesManifest>` sur `true`.</span><span class="sxs-lookup"><span data-stu-id="1f9e5-242">To generate a manifest of the embedded files, set the `<GenerateEmbeddedFilesManifest>` property to `true`.</span></span> <span data-ttu-id="1f9e5-243">Spécifiez les fichiers à incorporer avec [ &lt;EmbeddedResource&gt;](/dotnet/core/tools/csproj#default-compilation-includes-in-net-core-projects) :</span><span class="sxs-lookup"><span data-stu-id="1f9e5-243">Specify the files to embed with [&lt;EmbeddedResource&gt;](/dotnet/core/tools/csproj#default-compilation-includes-in-net-core-projects):</span></span>

[!code-csharp[](file-providers/samples/2.x/FileProviderSample/FileProviderSample.csproj?highlight=6,14)]

<span data-ttu-id="1f9e5-244">Utilisez les [modèles glob](#glob-patterns) pour spécifier un ou plusieurs fichiers à incorporer dans l’assembly.</span><span class="sxs-lookup"><span data-stu-id="1f9e5-244">Use [glob patterns](#glob-patterns) to specify one or more files to embed into the assembly.</span></span>

<span data-ttu-id="1f9e5-245">L’exemple d’application crée un `ManifestEmbeddedFileProvider` et transmet l’assembly en cours d’exécution à son constructeur.</span><span class="sxs-lookup"><span data-stu-id="1f9e5-245">The sample app creates an `ManifestEmbeddedFileProvider` and passes the currently executing assembly to its constructor.</span></span>

<span data-ttu-id="1f9e5-246">*Startup.cs* :</span><span class="sxs-lookup"><span data-stu-id="1f9e5-246">*Startup.cs*:</span></span>

```csharp
var manifestEmbeddedProvider = 
    new ManifestEmbeddedFileProvider(typeof(Program).Assembly);
```

<span data-ttu-id="1f9e5-247">Des surcharges supplémentaires vous permettent de :</span><span class="sxs-lookup"><span data-stu-id="1f9e5-247">Additional overloads allow you to:</span></span>

* <span data-ttu-id="1f9e5-248">Spécifier un chemin de fichier relatif.</span><span class="sxs-lookup"><span data-stu-id="1f9e5-248">Specify a relative file path.</span></span>
* <span data-ttu-id="1f9e5-249">Définir la portée de fichiers sur une date de dernière modification.</span><span class="sxs-lookup"><span data-stu-id="1f9e5-249">Scope files to a last modified date.</span></span>
* <span data-ttu-id="1f9e5-250">Nommer la ressource incorporée contenant le manifeste de fichier incorporé.</span><span class="sxs-lookup"><span data-stu-id="1f9e5-250">Name the embedded resource containing the embedded file manifest.</span></span>

| <span data-ttu-id="1f9e5-251">Surcharge</span><span class="sxs-lookup"><span data-stu-id="1f9e5-251">Overload</span></span> | <span data-ttu-id="1f9e5-252">Description</span><span class="sxs-lookup"><span data-stu-id="1f9e5-252">Description</span></span> |
| -------- | ----------- |
| `ManifestEmbeddedFileProvider(Assembly, String)` | <span data-ttu-id="1f9e5-253">Accepte un paramètre de chemin d'accès relatif `root` facultatif.</span><span class="sxs-lookup"><span data-stu-id="1f9e5-253">Accepts an optional `root` relative path parameter.</span></span> <span data-ttu-id="1f9e5-254">Spécifiez `root` pour définir la portée des appels à <xref:Microsoft.Extensions.FileProviders.IFileProvider.GetDirectoryContents*> sur ces ressources sous le chemin d’accès fourni.</span><span class="sxs-lookup"><span data-stu-id="1f9e5-254">Specify the `root` to scope calls to <xref:Microsoft.Extensions.FileProviders.IFileProvider.GetDirectoryContents*> to those resources under the provided path.</span></span> |
| `ManifestEmbeddedFileProvider(Assembly, String, DateTimeOffset)` | <span data-ttu-id="1f9e5-255">Accepte un paramètre de chemin relatif `root` facultatif et un paramètre de date `lastModified` (<xref:System.DateTimeOffset>).</span><span class="sxs-lookup"><span data-stu-id="1f9e5-255">Accepts an optional `root` relative path parameter and a `lastModified` date (<xref:System.DateTimeOffset>) parameter.</span></span> <span data-ttu-id="1f9e5-256">La date `lastModified` définit la portée de la dernière date de modification pour les instances <xref:Microsoft.Extensions.FileProviders.IFileInfo> retournées par <xref:Microsoft.Extensions.FileProviders.IFileProvider>.</span><span class="sxs-lookup"><span data-stu-id="1f9e5-256">The `lastModified` date scopes the last modification date for the <xref:Microsoft.Extensions.FileProviders.IFileInfo> instances returned by the <xref:Microsoft.Extensions.FileProviders.IFileProvider>.</span></span> |
| `ManifestEmbeddedFileProvider(Assembly, String, String, DateTimeOffset)` | <span data-ttu-id="1f9e5-257">Accepte un chemin d'accès relatif `root` facultatif, une date `lastModified` et des paramètres `manifestName`.</span><span class="sxs-lookup"><span data-stu-id="1f9e5-257">Accepts an optional `root` relative path, `lastModified` date, and `manifestName` parameters.</span></span> <span data-ttu-id="1f9e5-258">`manifestName` représente le nom de la ressource incorporée contenant la manifeste.</span><span class="sxs-lookup"><span data-stu-id="1f9e5-258">The `manifestName` represents the name of the embedded resource containing the manifest.</span></span> |

### <a name="compositefileprovider"></a><span data-ttu-id="1f9e5-259">CompositeFileProvider</span><span class="sxs-lookup"><span data-stu-id="1f9e5-259">CompositeFileProvider</span></span>

<span data-ttu-id="1f9e5-260"><xref:Microsoft.Extensions.FileProviders.CompositeFileProvider> combine des instances `IFileProvider`, en exposant une interface unique qui permet d’utiliser des fichiers de différents fournisseurs.</span><span class="sxs-lookup"><span data-stu-id="1f9e5-260">The <xref:Microsoft.Extensions.FileProviders.CompositeFileProvider> combines `IFileProvider` instances, exposing a single interface for working with files from multiple providers.</span></span> <span data-ttu-id="1f9e5-261">Quand vous créez `CompositeFileProvider`, vous passez une ou plusieurs instances `IFileProvider` à son constructeur.</span><span class="sxs-lookup"><span data-stu-id="1f9e5-261">When creating the `CompositeFileProvider`, pass one or more `IFileProvider` instances to its constructor.</span></span>

<span data-ttu-id="1f9e5-262">Dans l’exemple d’application, un `PhysicalFileProvider` et un `ManifestEmbeddedFileProvider` fournissent des fichiers à un `CompositeFileProvider` inscrit dans le conteneur de service de l’application :</span><span class="sxs-lookup"><span data-stu-id="1f9e5-262">In the sample app, a `PhysicalFileProvider` and a `ManifestEmbeddedFileProvider` provide files to a `CompositeFileProvider` registered in the app's service container:</span></span>

[!code-csharp[](file-providers/samples/2.x/FileProviderSample/Startup.cs?name=snippet1)]

## <a name="watch-for-changes"></a><span data-ttu-id="1f9e5-263">Suivre les modifications apportées</span><span class="sxs-lookup"><span data-stu-id="1f9e5-263">Watch for changes</span></span>

<span data-ttu-id="1f9e5-264">La méthode [IFileProvider.Watch](xref:Microsoft.Extensions.FileProviders.IFileProvider.Watch*) offre un moyen d’observer un ou plusieurs fichiers ou répertoires afin de détecter les changements.</span><span class="sxs-lookup"><span data-stu-id="1f9e5-264">The [IFileProvider.Watch](xref:Microsoft.Extensions.FileProviders.IFileProvider.Watch*) method provides a scenario to watch one or more files or directories for changes.</span></span> <span data-ttu-id="1f9e5-265">`Watch` accepte une chaîne de chemin, qui peut utiliser des [modèles d’utilisation des caractères génériques](#glob-patterns) pour spécifier plusieurs fichiers.</span><span class="sxs-lookup"><span data-stu-id="1f9e5-265">`Watch` accepts a path string, which can use [glob patterns](#glob-patterns) to specify multiple files.</span></span> <span data-ttu-id="1f9e5-266">`Watch`Retourne un <xref:Microsoft.Extensions.Primitives.IChangeToken>.</span><span class="sxs-lookup"><span data-stu-id="1f9e5-266">`Watch` returns an <xref:Microsoft.Extensions.Primitives.IChangeToken>.</span></span> <span data-ttu-id="1f9e5-267">Le jeton de modification expose :</span><span class="sxs-lookup"><span data-stu-id="1f9e5-267">The change token exposes:</span></span>

* <span data-ttu-id="1f9e5-268"><xref:Microsoft.Extensions.Primitives.IChangeToken.HasChanged> &ndash; une propriété qui peut être inspectée pour déterminer si une modification a eu lieu.</span><span class="sxs-lookup"><span data-stu-id="1f9e5-268"><xref:Microsoft.Extensions.Primitives.IChangeToken.HasChanged> &ndash; A property that can be inspected to determine if a change has occurred.</span></span>
* <span data-ttu-id="1f9e5-269"><xref:Microsoft.Extensions.Primitives.IChangeToken.RegisterChangeCallback*>&ndash; appelée quand des modifications sont détectées dans la chaîne de chemin spécifiée.</span><span class="sxs-lookup"><span data-stu-id="1f9e5-269"><xref:Microsoft.Extensions.Primitives.IChangeToken.RegisterChangeCallback*> &ndash; Called when changes are detected to the specified path string.</span></span> <span data-ttu-id="1f9e5-270">Chaque jeton de modification appelle uniquement son rappel associé en réponse à un changement unique.</span><span class="sxs-lookup"><span data-stu-id="1f9e5-270">Each change token only calls its associated callback in response to a single change.</span></span> <span data-ttu-id="1f9e5-271">Pour activer une surveillance constante, vous pouvez utiliser une <xref:System.Threading.Tasks.TaskCompletionSource`1> comme indiqué ci-dessous, ou recréer des instances `IChangeToken` en réponse aux changements.</span><span class="sxs-lookup"><span data-stu-id="1f9e5-271">To enable constant monitoring, use a <xref:System.Threading.Tasks.TaskCompletionSource`1> (shown below) or recreate `IChangeToken` instances in response to changes.</span></span>

<span data-ttu-id="1f9e5-272">Dans l’exemple d’application, l’application console *WatchConsole* est configurée pour afficher un message chaque fois qu’un fichier texte est modifié :</span><span class="sxs-lookup"><span data-stu-id="1f9e5-272">In the sample app, the *WatchConsole* console app is configured to display a message whenever a text file is modified:</span></span>

[!code-csharp[](file-providers/samples/2.x/WatchConsole/Program.cs?name=snippet1&highlight=1-2,16,19-20)]

<span data-ttu-id="1f9e5-273">Certains systèmes de fichiers, comme les conteneurs Docker et les partages réseau, peuvent ne pas envoyer de manière fiable les notifications de modifications.</span><span class="sxs-lookup"><span data-stu-id="1f9e5-273">Some file systems, such as Docker containers and network shares, may not reliably send change notifications.</span></span> <span data-ttu-id="1f9e5-274">Définissez la variable d’environnement `DOTNET_USE_POLLING_FILE_WATCHER` sur `1` ou `true` pour interroger le système de fichiers à la recherche de changements toutes les quatre secondes (non configurable).</span><span class="sxs-lookup"><span data-stu-id="1f9e5-274">Set the `DOTNET_USE_POLLING_FILE_WATCHER` environment variable to `1` or `true` to poll the file system for changes every four seconds (not configurable).</span></span>

## <a name="glob-patterns"></a><span data-ttu-id="1f9e5-275">Modèles Glob</span><span class="sxs-lookup"><span data-stu-id="1f9e5-275">Glob patterns</span></span>

<span data-ttu-id="1f9e5-276">Les chemins de système de fichiers utilisent des modèles à caractères génériques appelés *modèles Glob (ou d’utilisation des caractères génériques)* .</span><span class="sxs-lookup"><span data-stu-id="1f9e5-276">File system paths use wildcard patterns called *glob (or globbing) patterns*.</span></span> <span data-ttu-id="1f9e5-277">Spécifiez les groupes de fichiers avec ces modèles.</span><span class="sxs-lookup"><span data-stu-id="1f9e5-277">Specify groups of files with these patterns.</span></span> <span data-ttu-id="1f9e5-278">Les deux caractères génériques sont `*` et `**` :</span><span class="sxs-lookup"><span data-stu-id="1f9e5-278">The two wildcard characters are `*` and `**`:</span></span>

**`*`**  
<span data-ttu-id="1f9e5-279">Établit une correspondance avec n’importe quel élément au niveau de dossier actuel, avec n’importe quel nom de fichier ou avec n’importe quelle extension de fichier.</span><span class="sxs-lookup"><span data-stu-id="1f9e5-279">Matches anything at the current folder level, any filename, or any file extension.</span></span> <span data-ttu-id="1f9e5-280">Les correspondances sont terminées par des caractères `/` et `.` dans le chemin des fichiers.</span><span class="sxs-lookup"><span data-stu-id="1f9e5-280">Matches are terminated by `/` and `.` characters in the file path.</span></span>

**`**`**  
<span data-ttu-id="1f9e5-281">Établit une correspondance avec n’importe quel élément sur plusieurs niveaux de répertoire.</span><span class="sxs-lookup"><span data-stu-id="1f9e5-281">Matches anything across multiple directory levels.</span></span> <span data-ttu-id="1f9e5-282">Peut être utilisé pour établir une correspondance avec plusieurs fichiers dans une hiérarchie de répertoires de manière récursive.</span><span class="sxs-lookup"><span data-stu-id="1f9e5-282">Can be used to recursively match many files within a directory hierarchy.</span></span>

<span data-ttu-id="1f9e5-283">**Exemples de modèles d’utilisation des caractères génériques**</span><span class="sxs-lookup"><span data-stu-id="1f9e5-283">**Glob pattern examples**</span></span>

**`directory/file.txt`**  
<span data-ttu-id="1f9e5-284">Établit une correspondance avec un fichier spécifique dans un répertoire spécifique.</span><span class="sxs-lookup"><span data-stu-id="1f9e5-284">Matches a specific file in a specific directory.</span></span>

**`directory/*.txt`**  
<span data-ttu-id="1f9e5-285">Établit une correspondance avec tous les fichiers ayant l’extension *.txt* dans un répertoire spécifique.</span><span class="sxs-lookup"><span data-stu-id="1f9e5-285">Matches all files with *.txt* extension in a specific directory.</span></span>

**`directory/*/appsettings.json`**  
<span data-ttu-id="1f9e5-286">Établit une correspondance avec tous les fichiers `appsettings.json` dans les répertoires situés exactement un niveau en dessous du dossier *répertoire*.</span><span class="sxs-lookup"><span data-stu-id="1f9e5-286">Matches all `appsettings.json` files in directories exactly one level below the *directory* folder.</span></span>

**`directory/**/*.txt`**  
<span data-ttu-id="1f9e5-287">Établit une correspondance avec tous les fichiers ayant l’extension *.txt* et se trouvant n’importe où sous le dossier *répertoire*.</span><span class="sxs-lookup"><span data-stu-id="1f9e5-287">Matches all files with *.txt* extension found anywhere under the *directory* folder.</span></span>

::: moniker-end
