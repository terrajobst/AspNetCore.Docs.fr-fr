---
title: Gérer les références Protobuf avec dotnet-GRPC
author: juntaoluo
description: En savoir plus sur l’ajout, la mise à jour, la suppression et la liste de références Protobuf avec l’outil Global dotnet-GRPC.
monikerRange: '>= aspnetcore-3.0'
ms.author: johluo
ms.date: 09/24/2019
uid: grpc/dotnet-grpc
ms.openlocfilehash: ebd57419be24f7f4ed9765e36cf14189be8438b1
ms.sourcegitcommit: 020c3760492efed71b19e476f25392dda5dd7388
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/12/2019
ms.locfileid: "72290060"
---
# <a name="manage-protobuf-references-with-dotnet-grpc"></a><span data-ttu-id="13487-103">Gérer les références Protobuf avec dotnet-GRPC</span><span class="sxs-lookup"><span data-stu-id="13487-103">Manage Protobuf references with dotnet-grpc</span></span>

<span data-ttu-id="13487-104">Par [John Luo](https://github.com/juntaoluo)</span><span class="sxs-lookup"><span data-stu-id="13487-104">By [John Luo](https://github.com/juntaoluo)</span></span>

<span data-ttu-id="13487-105">`dotnet-grpc` est un outil Global .NET Core pour la gestion des références Protobuf dans un projet .NET gRPC.</span><span class="sxs-lookup"><span data-stu-id="13487-105">`dotnet-grpc` is a .NET Core Global Tool for managing Protobuf references within a .NET gRPC project.</span></span> <span data-ttu-id="13487-106">L’outil peut être utilisé pour ajouter, actualiser, supprimer et répertorier des références Protobuf.</span><span class="sxs-lookup"><span data-stu-id="13487-106">The tool can be used to add, refresh, remove, and list Protobuf references.</span></span>

## <a name="installation"></a><span data-ttu-id="13487-107">Installation</span><span class="sxs-lookup"><span data-stu-id="13487-107">Installation</span></span>

<span data-ttu-id="13487-108">Pour installer l' [outil Global .net Core](/dotnet/core/tools/global-tools)`dotnet-grpc`, exécutez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="13487-108">To install the `dotnet-grpc` [.NET Core Global Tool](/dotnet/core/tools/global-tools), run the following command:</span></span>

```dotnetcli
dotnet tool install -g dotnet-grpc
```

## <a name="add-references"></a><span data-ttu-id="13487-109">Ajoutez des références</span><span class="sxs-lookup"><span data-stu-id="13487-109">Add references</span></span>

<span data-ttu-id="13487-110">`dotnet-grpc` peut être utilisé pour ajouter des références Protobuf comme `<Protobuf />` éléments au fichier *. csproj* :</span><span class="sxs-lookup"><span data-stu-id="13487-110">`dotnet-grpc` can be used to add Protobuf references as `<Protobuf />` items to the *.csproj* file:</span></span>

```xml
<Protobuf Include="..\Proto\count.proto" GrpcServices="Server" Link="Protos\count.proto" />
```

<span data-ttu-id="13487-111">Les références Protobuf sont utilisées pour générer les C# ressources client et/ou serveur.</span><span class="sxs-lookup"><span data-stu-id="13487-111">The Protobuf references are used to generate the C# client and/or server assets.</span></span> <span data-ttu-id="13487-112">Le `dotnet-grpc`tool peut :</span><span class="sxs-lookup"><span data-stu-id="13487-112">The `dotnet-grpc`tool can:</span></span>

* <span data-ttu-id="13487-113">Créez une référence Protobuf à partir de fichiers locaux sur le disque.</span><span class="sxs-lookup"><span data-stu-id="13487-113">Create a Protobuf reference from local files on disk.</span></span>
* <span data-ttu-id="13487-114">Crée une référence Protobuf à partir d’un fichier distant spécifié par une URL.</span><span class="sxs-lookup"><span data-stu-id="13487-114">Create a Protobuf reference from a remote file specified by a URL.</span></span>
* <span data-ttu-id="13487-115">Vérifiez que les dépendances de package gRPC correctes sont ajoutées au projet.</span><span class="sxs-lookup"><span data-stu-id="13487-115">Ensure the correct gRPC package dependencies are added to the project.</span></span>

<span data-ttu-id="13487-116">Par exemple, le package `Grpc.AspNetCore` est ajouté à une application Web.</span><span class="sxs-lookup"><span data-stu-id="13487-116">For example, the `Grpc.AspNetCore` package is added to a web app.</span></span> <span data-ttu-id="13487-117">`Grpc.AspNetCore` contient les bibliothèques clientes et de serveur gRPC et la prise en charge des outils.</span><span class="sxs-lookup"><span data-stu-id="13487-117">`Grpc.AspNetCore` contains gRPC server and client libraries and tooling support.</span></span> <span data-ttu-id="13487-118">En guise d’alternative, les packages `Grpc.Net.Client`, `Grpc.Tools` et `Google.Protobuf`, qui contiennent uniquement les bibliothèques clientes gRPC et la prise en charge des outils, sont ajoutés à une application console.</span><span class="sxs-lookup"><span data-stu-id="13487-118">Alternatively, the `Grpc.Net.Client`, `Grpc.Tools` and `Google.Protobuf` packages, which contain only the gRPC client libraries and tooling support, are added to a Console app.</span></span>

### <a name="add-file"></a><span data-ttu-id="13487-119">Ajouter un fichier</span><span class="sxs-lookup"><span data-stu-id="13487-119">Add file</span></span>

<span data-ttu-id="13487-120">La commande `add-file` est utilisée pour ajouter des fichiers locaux sur le disque en tant que références Protobuf.</span><span class="sxs-lookup"><span data-stu-id="13487-120">The `add-file` command is used to add local files on disk as Protobuf references.</span></span> <span data-ttu-id="13487-121">Chemins d’accès aux fichiers fournis :</span><span class="sxs-lookup"><span data-stu-id="13487-121">The file paths provided:</span></span>

* <span data-ttu-id="13487-122">Peut être relatif au répertoire actif ou aux chemins d’accès absolus.</span><span class="sxs-lookup"><span data-stu-id="13487-122">Can be relative to the current directory or absolute paths.</span></span>
* <span data-ttu-id="13487-123">Peut contenir des caractères génériques pour les [globbing](https://wikipedia.org/wiki/Glob_(programming))de fichiers basés sur des modèles.</span><span class="sxs-lookup"><span data-stu-id="13487-123">May contain wild cards for pattern-based file [globbing](https://wikipedia.org/wiki/Glob_(programming)).</span></span>

<span data-ttu-id="13487-124">Si des fichiers se trouvent en dehors du répertoire du projet, un élément `Link` est ajouté pour afficher le fichier dans le dossier `Protos` dans Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="13487-124">If any files are outside the project directory, a `Link` element is added to display the file under the folder `Protos` in Visual Studio.</span></span>

### <a name="usage"></a><span data-ttu-id="13487-125">Usage</span><span class="sxs-lookup"><span data-stu-id="13487-125">Usage</span></span>

```dotnetcli
dotnet grpc add-file [options] <files>...
```

#### <a name="arguments"></a><span data-ttu-id="13487-126">Arguments</span><span class="sxs-lookup"><span data-stu-id="13487-126">Arguments</span></span>

| <span data-ttu-id="13487-127">Argument</span><span class="sxs-lookup"><span data-stu-id="13487-127">Argument</span></span> | <span data-ttu-id="13487-128">Description</span><span class="sxs-lookup"><span data-stu-id="13487-128">Description</span></span> |
|-|-|
| <span data-ttu-id="13487-129">fichiers</span><span class="sxs-lookup"><span data-stu-id="13487-129">files</span></span> | <span data-ttu-id="13487-130">Références du fichier protobuf.</span><span class="sxs-lookup"><span data-stu-id="13487-130">The protobuf file references.</span></span> <span data-ttu-id="13487-131">Il peut s’agir d’un chemin d’accès à glob pour les fichiers protobuf locaux.</span><span class="sxs-lookup"><span data-stu-id="13487-131">These can be a path to glob for local protobuf files.</span></span> |

#### <a name="options"></a><span data-ttu-id="13487-132">Options</span><span class="sxs-lookup"><span data-stu-id="13487-132">Options</span></span>

| <span data-ttu-id="13487-133">Option Short</span><span class="sxs-lookup"><span data-stu-id="13487-133">Short option</span></span> | <span data-ttu-id="13487-134">Option longue</span><span class="sxs-lookup"><span data-stu-id="13487-134">Long option</span></span> | <span data-ttu-id="13487-135">Description</span><span class="sxs-lookup"><span data-stu-id="13487-135">Description</span></span> |
|-|-|-|
| <span data-ttu-id="13487-136">-p</span><span class="sxs-lookup"><span data-stu-id="13487-136">-p</span></span> | <span data-ttu-id="13487-137">--projet</span><span class="sxs-lookup"><span data-stu-id="13487-137">--project</span></span> | <span data-ttu-id="13487-138">Chemin d’accès au fichier projet sur lequel effectuer l’opération.</span><span class="sxs-lookup"><span data-stu-id="13487-138">The path to the project file to operate on.</span></span> <span data-ttu-id="13487-139">Si aucun fichier n’est spécifié, la commande effectue une recherche dans le répertoire actif.</span><span class="sxs-lookup"><span data-stu-id="13487-139">If a file is not specified, the command searches the current directory for one.</span></span>
| <span data-ttu-id="13487-140">-s</span><span class="sxs-lookup"><span data-stu-id="13487-140">-s</span></span> | <span data-ttu-id="13487-141">--services</span><span class="sxs-lookup"><span data-stu-id="13487-141">--services</span></span> | <span data-ttu-id="13487-142">Type des services gRPC qui doivent être générés.</span><span class="sxs-lookup"><span data-stu-id="13487-142">The type of gRPC services that should be generated.</span></span> <span data-ttu-id="13487-143">Si `Default` est spécifié, `Both` est utilisé pour les projets Web et `Client` est utilisé pour les projets non Web.</span><span class="sxs-lookup"><span data-stu-id="13487-143">If `Default` is specified, `Both` is used for Web projects and `Client` is used for non-Web projects.</span></span> <span data-ttu-id="13487-144">Les valeurs acceptées sont `Both`, `Client`, `Default`, `None`, `Server`.</span><span class="sxs-lookup"><span data-stu-id="13487-144">Accepted values are `Both`, `Client`, `Default`, `None`, `Server`.</span></span>
| <span data-ttu-id="13487-145">-i</span><span class="sxs-lookup"><span data-stu-id="13487-145">-i</span></span> | <span data-ttu-id="13487-146">--Import-dirs</span><span class="sxs-lookup"><span data-stu-id="13487-146">--additional-import-dirs</span></span> | <span data-ttu-id="13487-147">Répertoires supplémentaires à utiliser lors de la résolution des importations des fichiers protobuf.</span><span class="sxs-lookup"><span data-stu-id="13487-147">Additional directories to be used when resolving imports for the protobuf files.</span></span> <span data-ttu-id="13487-148">Il s’agit d’une liste de chemins séparés par des points-virgules.</span><span class="sxs-lookup"><span data-stu-id="13487-148">This is a semicolon separated list of paths.</span></span>
| | <span data-ttu-id="13487-149">--accès</span><span class="sxs-lookup"><span data-stu-id="13487-149">--access</span></span> | <span data-ttu-id="13487-150">Modificateur d’accès à utiliser pour les classes C# générées.</span><span class="sxs-lookup"><span data-stu-id="13487-150">The access modifier to use for the generated C# classes.</span></span> <span data-ttu-id="13487-151">La valeur par défaut est `Public`.</span><span class="sxs-lookup"><span data-stu-id="13487-151">The default value is `Public`.</span></span> <span data-ttu-id="13487-152">Les valeurs acceptées sont `Internal` et `Public`.</span><span class="sxs-lookup"><span data-stu-id="13487-152">Accepted values are `Internal` and `Public`.</span></span>

### <a name="add-url"></a><span data-ttu-id="13487-153">Ajouter une URL</span><span class="sxs-lookup"><span data-stu-id="13487-153">Add URL</span></span>

<span data-ttu-id="13487-154">La commande `add-url` est utilisée pour ajouter un fichier distant spécifié par une URL source en tant que référence Protobuf.</span><span class="sxs-lookup"><span data-stu-id="13487-154">The `add-url` command is used to add a remote file specified by an source URL as Protobuf reference.</span></span> <span data-ttu-id="13487-155">Un chemin d’accès de fichier doit être fourni pour spécifier l’emplacement de téléchargement du fichier distant.</span><span class="sxs-lookup"><span data-stu-id="13487-155">A file path must be provided to specify where to download the remote file.</span></span> <span data-ttu-id="13487-156">Le chemin d’accès du fichier peut être relatif au répertoire actif ou à un chemin d’accès absolu.</span><span class="sxs-lookup"><span data-stu-id="13487-156">The file path can be relative to the current directory or an absolute path.</span></span> <span data-ttu-id="13487-157">Si le chemin d’accès au fichier se trouve en dehors du répertoire du projet, un élément `Link` est ajouté pour afficher le fichier sous le dossier virtuel `Protos` dans Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="13487-157">If the file path is outside the project directory, a `Link` element is added to display the file under the virtual folder `Protos` in Visual Studio.</span></span>

### <a name="usage"></a><span data-ttu-id="13487-158">Usage</span><span class="sxs-lookup"><span data-stu-id="13487-158">Usage</span></span>

```dotnetcli
dotnet-grpc add-url [options] <url>
```

#### <a name="arguments"></a><span data-ttu-id="13487-159">Arguments</span><span class="sxs-lookup"><span data-stu-id="13487-159">Arguments</span></span>

| <span data-ttu-id="13487-160">Argument</span><span class="sxs-lookup"><span data-stu-id="13487-160">Argument</span></span> | <span data-ttu-id="13487-161">Description</span><span class="sxs-lookup"><span data-stu-id="13487-161">Description</span></span> |
|-|-|
| <span data-ttu-id="13487-162">url</span><span class="sxs-lookup"><span data-stu-id="13487-162">url</span></span> | <span data-ttu-id="13487-163">URL d’un fichier protobuf distant.</span><span class="sxs-lookup"><span data-stu-id="13487-163">The URL to a remote protobuf file.</span></span> |

#### <a name="options"></a><span data-ttu-id="13487-164">Options</span><span class="sxs-lookup"><span data-stu-id="13487-164">Options</span></span>

| <span data-ttu-id="13487-165">Option Short</span><span class="sxs-lookup"><span data-stu-id="13487-165">Short option</span></span> | <span data-ttu-id="13487-166">Option longue</span><span class="sxs-lookup"><span data-stu-id="13487-166">Long option</span></span> | <span data-ttu-id="13487-167">Description</span><span class="sxs-lookup"><span data-stu-id="13487-167">Description</span></span> |
|-|-|-|
| <span data-ttu-id="13487-168">-o</span><span class="sxs-lookup"><span data-stu-id="13487-168">-o</span></span> | <span data-ttu-id="13487-169">--sortie</span><span class="sxs-lookup"><span data-stu-id="13487-169">--output</span></span> | <span data-ttu-id="13487-170">Spécifie le chemin de téléchargement du fichier protobuf distant.</span><span class="sxs-lookup"><span data-stu-id="13487-170">Specifies the download path for the remote protobuf file.</span></span> <span data-ttu-id="13487-171">C'est une option obligatoire.</span><span class="sxs-lookup"><span data-stu-id="13487-171">This is a required option.</span></span>
| <span data-ttu-id="13487-172">-p</span><span class="sxs-lookup"><span data-stu-id="13487-172">-p</span></span> | <span data-ttu-id="13487-173">--projet</span><span class="sxs-lookup"><span data-stu-id="13487-173">--project</span></span> | <span data-ttu-id="13487-174">Chemin d’accès au fichier projet sur lequel effectuer l’opération.</span><span class="sxs-lookup"><span data-stu-id="13487-174">The path to the project file to operate on.</span></span> <span data-ttu-id="13487-175">Si aucun fichier n’est spécifié, la commande effectue une recherche dans le répertoire actif.</span><span class="sxs-lookup"><span data-stu-id="13487-175">If a file is not specified, the command searches the current directory for one.</span></span>
| <span data-ttu-id="13487-176">-s</span><span class="sxs-lookup"><span data-stu-id="13487-176">-s</span></span> | <span data-ttu-id="13487-177">--services</span><span class="sxs-lookup"><span data-stu-id="13487-177">--services</span></span> | <span data-ttu-id="13487-178">Type des services gRPC qui doivent être générés.</span><span class="sxs-lookup"><span data-stu-id="13487-178">The type of gRPC services that should be generated.</span></span> <span data-ttu-id="13487-179">Si `Default` est spécifié, `Both` est utilisé pour les projets Web et `Client` est utilisé pour les projets non Web.</span><span class="sxs-lookup"><span data-stu-id="13487-179">If `Default` is specified, `Both` is used for Web projects and `Client` is used for non-Web projects.</span></span> <span data-ttu-id="13487-180">Les valeurs acceptées sont `Both`, `Client`, `Default`, `None`, `Server`.</span><span class="sxs-lookup"><span data-stu-id="13487-180">Accepted values are `Both`, `Client`, `Default`, `None`, `Server`.</span></span>
| <span data-ttu-id="13487-181">-i</span><span class="sxs-lookup"><span data-stu-id="13487-181">-i</span></span> | <span data-ttu-id="13487-182">--Import-dirs</span><span class="sxs-lookup"><span data-stu-id="13487-182">--additional-import-dirs</span></span> | <span data-ttu-id="13487-183">Répertoires supplémentaires à utiliser lors de la résolution des importations des fichiers protobuf.</span><span class="sxs-lookup"><span data-stu-id="13487-183">Additional directories to be used when resolving imports for the protobuf files.</span></span> <span data-ttu-id="13487-184">Il s’agit d’une liste de chemins séparés par des points-virgules.</span><span class="sxs-lookup"><span data-stu-id="13487-184">This is a semicolon separated list of paths.</span></span>
| | <span data-ttu-id="13487-185">--accès</span><span class="sxs-lookup"><span data-stu-id="13487-185">--access</span></span> | <span data-ttu-id="13487-186">Modificateur d’accès à utiliser pour les classes C# générées.</span><span class="sxs-lookup"><span data-stu-id="13487-186">The access modifier to use for the generated C# classes.</span></span> <span data-ttu-id="13487-187">La valeur par défaut est `Public`.</span><span class="sxs-lookup"><span data-stu-id="13487-187">Default value is `Public`.</span></span> <span data-ttu-id="13487-188">Les valeurs acceptées sont `Internal` et `Public`.</span><span class="sxs-lookup"><span data-stu-id="13487-188">Accepted values are `Internal` and `Public`.</span></span>

## <a name="remove"></a><span data-ttu-id="13487-189">Remove</span><span class="sxs-lookup"><span data-stu-id="13487-189">Remove</span></span>

<span data-ttu-id="13487-190">La commande `remove` est utilisée pour supprimer les références Protobuf du fichier *. csproj* .</span><span class="sxs-lookup"><span data-stu-id="13487-190">The `remove` command is used to remove Protobuf references from the *.csproj* file.</span></span> <span data-ttu-id="13487-191">La commande accepte les arguments de chemin d’accès et les URL source comme arguments.</span><span class="sxs-lookup"><span data-stu-id="13487-191">The command accepts path arguments and source URLs as arguments.</span></span> <span data-ttu-id="13487-192">L’outil :</span><span class="sxs-lookup"><span data-stu-id="13487-192">The tool:</span></span>

* <span data-ttu-id="13487-193">Supprime uniquement la référence Protobuf.</span><span class="sxs-lookup"><span data-stu-id="13487-193">Only removes the Protobuf reference.</span></span>
* <span data-ttu-id="13487-194">Ne supprime pas le fichier *. proto* , même s’il a été téléchargé à l’origine à partir d’une URL distante.</span><span class="sxs-lookup"><span data-stu-id="13487-194">Does not delete the *.proto* file, even if it was originally downloaded from a remote URL.</span></span>

### <a name="usage"></a><span data-ttu-id="13487-195">Usage</span><span class="sxs-lookup"><span data-stu-id="13487-195">Usage</span></span>

```dotnetcli
dotnet-grpc remove [options] <references>...
```

### <a name="arguments"></a><span data-ttu-id="13487-196">Arguments</span><span class="sxs-lookup"><span data-stu-id="13487-196">Arguments</span></span>

| <span data-ttu-id="13487-197">Argument</span><span class="sxs-lookup"><span data-stu-id="13487-197">Argument</span></span> | <span data-ttu-id="13487-198">Description</span><span class="sxs-lookup"><span data-stu-id="13487-198">Description</span></span> |
|-|-|
| <span data-ttu-id="13487-199">Références</span><span class="sxs-lookup"><span data-stu-id="13487-199">references</span></span> | <span data-ttu-id="13487-200">URL ou chemins de fichier des références protobuf à supprimer.</span><span class="sxs-lookup"><span data-stu-id="13487-200">The URLs or file paths of the protobuf references to remove.</span></span> |

### <a name="options"></a><span data-ttu-id="13487-201">Options</span><span class="sxs-lookup"><span data-stu-id="13487-201">Options</span></span>

| <span data-ttu-id="13487-202">Option Short</span><span class="sxs-lookup"><span data-stu-id="13487-202">Short option</span></span> | <span data-ttu-id="13487-203">Option longue</span><span class="sxs-lookup"><span data-stu-id="13487-203">Long option</span></span> | <span data-ttu-id="13487-204">Description</span><span class="sxs-lookup"><span data-stu-id="13487-204">Description</span></span> |
|-|-|-|
| <span data-ttu-id="13487-205">-p</span><span class="sxs-lookup"><span data-stu-id="13487-205">-p</span></span> | <span data-ttu-id="13487-206">--projet</span><span class="sxs-lookup"><span data-stu-id="13487-206">--project</span></span> | <span data-ttu-id="13487-207">Chemin d’accès au fichier projet sur lequel effectuer l’opération.</span><span class="sxs-lookup"><span data-stu-id="13487-207">The path to the project file to operate on.</span></span> <span data-ttu-id="13487-208">Si aucun fichier n’est spécifié, la commande effectue une recherche dans le répertoire actif.</span><span class="sxs-lookup"><span data-stu-id="13487-208">If a file is not specified, the command searches the current directory for one.</span></span>

## <a name="refresh"></a><span data-ttu-id="13487-209">Actualiser</span><span class="sxs-lookup"><span data-stu-id="13487-209">Refresh</span></span>

<span data-ttu-id="13487-210">La commande `refresh` est utilisée pour mettre à jour une référence distante avec le contenu le plus récent à partir de l’URL source.</span><span class="sxs-lookup"><span data-stu-id="13487-210">The `refresh` command is used to update a remote reference with the latest content from the source URL.</span></span> <span data-ttu-id="13487-211">Le chemin d’accès au fichier de téléchargement et l’URL source peuvent être utilisés pour spécifier la référence à mettre à jour.</span><span class="sxs-lookup"><span data-stu-id="13487-211">Both the download file path and the source URL can be used to specify the reference to be updated.</span></span> <span data-ttu-id="13487-212">Remarque :</span><span class="sxs-lookup"><span data-stu-id="13487-212">Note:</span></span>

* <span data-ttu-id="13487-213">Les hachages du contenu du fichier sont comparés pour déterminer si le fichier local doit être mis à jour.</span><span class="sxs-lookup"><span data-stu-id="13487-213">The hashes of the file contents are compared to determine whether the local file should be updated.</span></span>
* <span data-ttu-id="13487-214">Aucune information d’horodatage n’est comparée.</span><span class="sxs-lookup"><span data-stu-id="13487-214">No timestamp information is compared.</span></span>

<span data-ttu-id="13487-215">L’outil remplace toujours le fichier local par le fichier distant si une mise à jour est nécessaire.</span><span class="sxs-lookup"><span data-stu-id="13487-215">The tool always replaces the local file with the remote file if an update is needed.</span></span>

### <a name="usage"></a><span data-ttu-id="13487-216">Usage</span><span class="sxs-lookup"><span data-stu-id="13487-216">Usage</span></span>

```dotnetcli
dotnet-grpc refresh [options] [<references>...]
```

### <a name="arguments"></a><span data-ttu-id="13487-217">Arguments</span><span class="sxs-lookup"><span data-stu-id="13487-217">Arguments</span></span>

| <span data-ttu-id="13487-218">Argument</span><span class="sxs-lookup"><span data-stu-id="13487-218">Argument</span></span> | <span data-ttu-id="13487-219">Description</span><span class="sxs-lookup"><span data-stu-id="13487-219">Description</span></span> |
|-|-|
| <span data-ttu-id="13487-220">Références</span><span class="sxs-lookup"><span data-stu-id="13487-220">references</span></span> | <span data-ttu-id="13487-221">URL ou chemins d’accès aux références protobuf distantes qui doivent être mises à jour.</span><span class="sxs-lookup"><span data-stu-id="13487-221">The URLs or file paths to remote protobuf references that should be updated.</span></span> <span data-ttu-id="13487-222">Laissez cet argument vide pour actualiser toutes les références distantes.</span><span class="sxs-lookup"><span data-stu-id="13487-222">Leave this argument empty to refresh all remote references.</span></span> |

### <a name="options"></a><span data-ttu-id="13487-223">Options</span><span class="sxs-lookup"><span data-stu-id="13487-223">Options</span></span>

| <span data-ttu-id="13487-224">Option Short</span><span class="sxs-lookup"><span data-stu-id="13487-224">Short option</span></span> | <span data-ttu-id="13487-225">Option longue</span><span class="sxs-lookup"><span data-stu-id="13487-225">Long option</span></span> | <span data-ttu-id="13487-226">Description</span><span class="sxs-lookup"><span data-stu-id="13487-226">Description</span></span> |
|-|-|-|
| <span data-ttu-id="13487-227">-p</span><span class="sxs-lookup"><span data-stu-id="13487-227">-p</span></span> | <span data-ttu-id="13487-228">--projet</span><span class="sxs-lookup"><span data-stu-id="13487-228">--project</span></span> | <span data-ttu-id="13487-229">Chemin d’accès au fichier projet sur lequel effectuer l’opération.</span><span class="sxs-lookup"><span data-stu-id="13487-229">The path to the project file to operate on.</span></span> <span data-ttu-id="13487-230">Si aucun fichier n’est spécifié, la commande effectue une recherche dans le répertoire actif.</span><span class="sxs-lookup"><span data-stu-id="13487-230">If a file is not specified, the command searches the current directory for one.</span></span>
| | <span data-ttu-id="13487-231">--à sec-exécution</span><span class="sxs-lookup"><span data-stu-id="13487-231">--dry-run</span></span> | <span data-ttu-id="13487-232">Génère une liste des fichiers qui seraient mis à jour sans télécharger de nouveau contenu.</span><span class="sxs-lookup"><span data-stu-id="13487-232">Outputs a list of files that would be updated without downloading any new content.</span></span>

## <a name="list"></a><span data-ttu-id="13487-233">Liste</span><span class="sxs-lookup"><span data-stu-id="13487-233">List</span></span>

<span data-ttu-id="13487-234">La commande `list` est utilisée pour afficher toutes les références Protobuf dans le fichier projet.</span><span class="sxs-lookup"><span data-stu-id="13487-234">The `list` command is used to display all the Protobuf references in the project file.</span></span> <span data-ttu-id="13487-235">Si toutes les valeurs d’une colonne sont des valeurs par défaut, la colonne peut être omise.</span><span class="sxs-lookup"><span data-stu-id="13487-235">If all values of a column are default values, the column may be omitted.</span></span>

### <a name="usage"></a><span data-ttu-id="13487-236">Usage</span><span class="sxs-lookup"><span data-stu-id="13487-236">Usage</span></span>

```dotnetcli
dotnet-grpc list [options]
```

### <a name="options"></a><span data-ttu-id="13487-237">Options</span><span class="sxs-lookup"><span data-stu-id="13487-237">Options</span></span>

| <span data-ttu-id="13487-238">Option Short</span><span class="sxs-lookup"><span data-stu-id="13487-238">Short option</span></span> | <span data-ttu-id="13487-239">Option longue</span><span class="sxs-lookup"><span data-stu-id="13487-239">Long option</span></span> | <span data-ttu-id="13487-240">Description</span><span class="sxs-lookup"><span data-stu-id="13487-240">Description</span></span> |
|-|-|-|
| <span data-ttu-id="13487-241">-p</span><span class="sxs-lookup"><span data-stu-id="13487-241">-p</span></span> | <span data-ttu-id="13487-242">--projet</span><span class="sxs-lookup"><span data-stu-id="13487-242">--project</span></span> | <span data-ttu-id="13487-243">Chemin d’accès au fichier projet sur lequel effectuer l’opération.</span><span class="sxs-lookup"><span data-stu-id="13487-243">The path to the project file to operate on.</span></span> <span data-ttu-id="13487-244">Si aucun fichier n’est spécifié, la commande effectue une recherche dans le répertoire actif.</span><span class="sxs-lookup"><span data-stu-id="13487-244">If a file is not specified, the command searches the current directory for one.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="13487-245">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="13487-245">Additional resources</span></span>

* <xref:grpc/index>
* <xref:grpc/basics>
* <xref:grpc/aspnetcore>
