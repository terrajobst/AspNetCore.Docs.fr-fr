---
title: Gérer les références Protobuf avec dotnet-GRPC
author: juntaoluo
description: En savoir plus sur l’ajout, la mise à jour, la suppression et la liste de références Protobuf avec l’outil Global dotnet-GRPC.
monikerRange: '>= aspnetcore-3.0'
ms.author: johluo
ms.date: 10/17/2019
uid: grpc/dotnet-grpc
ms.openlocfilehash: 994597c854a95bb33de1686ab025cb3744cf6845
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78667334"
---
# <a name="manage-protobuf-references-with-dotnet-grpc"></a><span data-ttu-id="b0758-103">Gérer les références Protobuf avec dotnet-GRPC</span><span class="sxs-lookup"><span data-stu-id="b0758-103">Manage Protobuf references with dotnet-grpc</span></span>

<span data-ttu-id="b0758-104">Par [John Luo](https://github.com/juntaoluo)</span><span class="sxs-lookup"><span data-stu-id="b0758-104">By [John Luo](https://github.com/juntaoluo)</span></span>

<span data-ttu-id="b0758-105">`dotnet-grpc` est un outil Global .NET Core pour la gestion des références [Protobuf ( *. proto*)](xref:grpc/basics#proto-file) dans un projet .net gRPC.</span><span class="sxs-lookup"><span data-stu-id="b0758-105">`dotnet-grpc` is a .NET Core Global Tool for managing [Protobuf (*.proto*)](xref:grpc/basics#proto-file) references within a .NET gRPC project.</span></span> <span data-ttu-id="b0758-106">L’outil peut être utilisé pour ajouter, actualiser, supprimer et répertorier des références Protobuf.</span><span class="sxs-lookup"><span data-stu-id="b0758-106">The tool can be used to add, refresh, remove, and list Protobuf references.</span></span>

## <a name="installation"></a><span data-ttu-id="b0758-107">Installation</span><span class="sxs-lookup"><span data-stu-id="b0758-107">Installation</span></span>

<span data-ttu-id="b0758-108">Pour installer l' [outil Global .net Core](/dotnet/core/tools/global-tools)`dotnet-grpc`, exécutez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="b0758-108">To install the `dotnet-grpc` [.NET Core Global Tool](/dotnet/core/tools/global-tools), run the following command:</span></span>

```dotnetcli
dotnet tool install -g dotnet-grpc
```

## <a name="add-references"></a><span data-ttu-id="b0758-109">Ajouter des références</span><span class="sxs-lookup"><span data-stu-id="b0758-109">Add references</span></span>

<span data-ttu-id="b0758-110">`dotnet-grpc` peut être utilisé pour ajouter des références Protobuf en tant qu’éléments de `<Protobuf />` au fichier *. csproj* :</span><span class="sxs-lookup"><span data-stu-id="b0758-110">`dotnet-grpc` can be used to add Protobuf references as `<Protobuf />` items to the *.csproj* file:</span></span>

```xml
<Protobuf Include="Protos\greet.proto" GrpcServices="Server" />
```

<span data-ttu-id="b0758-111">Les références Protobuf sont utilisées pour générer les C# ressources client et/ou serveur.</span><span class="sxs-lookup"><span data-stu-id="b0758-111">The Protobuf references are used to generate the C# client and/or server assets.</span></span> <span data-ttu-id="b0758-112">L’outil `dotnet-grpc` peut :</span><span class="sxs-lookup"><span data-stu-id="b0758-112">The `dotnet-grpc` tool can:</span></span>

* <span data-ttu-id="b0758-113">Créez une référence Protobuf à partir de fichiers locaux sur le disque.</span><span class="sxs-lookup"><span data-stu-id="b0758-113">Create a Protobuf reference from local files on disk.</span></span>
* <span data-ttu-id="b0758-114">Crée une référence Protobuf à partir d’un fichier distant spécifié par une URL.</span><span class="sxs-lookup"><span data-stu-id="b0758-114">Create a Protobuf reference from a remote file specified by a URL.</span></span>
* <span data-ttu-id="b0758-115">Vérifiez que les dépendances de package gRPC correctes sont ajoutées au projet.</span><span class="sxs-lookup"><span data-stu-id="b0758-115">Ensure the correct gRPC package dependencies are added to the project.</span></span>

<span data-ttu-id="b0758-116">Par exemple, le package `Grpc.AspNetCore` est ajouté à une application Web.</span><span class="sxs-lookup"><span data-stu-id="b0758-116">For example, the `Grpc.AspNetCore` package is added to a web app.</span></span> <span data-ttu-id="b0758-117">`Grpc.AspNetCore` contient les bibliothèques clientes et de serveur gRPC et la prise en charge des outils.</span><span class="sxs-lookup"><span data-stu-id="b0758-117">`Grpc.AspNetCore` contains gRPC server and client libraries and tooling support.</span></span> <span data-ttu-id="b0758-118">En guise d’alternative, les packages `Grpc.Net.Client`, `Grpc.Tools` et `Google.Protobuf`, qui contiennent uniquement les bibliothèques clientes gRPC et la prise en charge des outils, sont ajoutés à une application console.</span><span class="sxs-lookup"><span data-stu-id="b0758-118">Alternatively, the `Grpc.Net.Client`, `Grpc.Tools` and `Google.Protobuf` packages, which contain only the gRPC client libraries and tooling support, are added to a Console app.</span></span>

### <a name="add-file"></a><span data-ttu-id="b0758-119">Ajouter un fichier</span><span class="sxs-lookup"><span data-stu-id="b0758-119">Add file</span></span>

<span data-ttu-id="b0758-120">La commande `add-file` est utilisée pour ajouter des fichiers locaux sur le disque en tant que références Protobuf.</span><span class="sxs-lookup"><span data-stu-id="b0758-120">The `add-file` command is used to add local files on disk as Protobuf references.</span></span> <span data-ttu-id="b0758-121">Chemins d’accès aux fichiers fournis :</span><span class="sxs-lookup"><span data-stu-id="b0758-121">The file paths provided:</span></span>

* <span data-ttu-id="b0758-122">Peut être relatif au répertoire actif ou aux chemins d’accès absolus.</span><span class="sxs-lookup"><span data-stu-id="b0758-122">Can be relative to the current directory or absolute paths.</span></span>
* <span data-ttu-id="b0758-123">Peut contenir des caractères génériques pour les [globbing](https://wikipedia.org/wiki/Glob_(programming))de fichiers basés sur des modèles.</span><span class="sxs-lookup"><span data-stu-id="b0758-123">May contain wild cards for pattern-based file [globbing](https://wikipedia.org/wiki/Glob_(programming)).</span></span>

<span data-ttu-id="b0758-124">Si des fichiers se trouvent en dehors du répertoire du projet, un élément `Link` est ajouté pour afficher le fichier dans le dossier `Protos` dans Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="b0758-124">If any files are outside the project directory, a `Link` element is added to display the file under the folder `Protos` in Visual Studio.</span></span>

### <a name="usage"></a><span data-ttu-id="b0758-125">Usage</span><span class="sxs-lookup"><span data-stu-id="b0758-125">Usage</span></span>

```dotnetcli
dotnet grpc add-file [options] <files>...
```

#### <a name="arguments"></a><span data-ttu-id="b0758-126">Arguments</span><span class="sxs-lookup"><span data-stu-id="b0758-126">Arguments</span></span>

| <span data-ttu-id="b0758-127">Argument</span><span class="sxs-lookup"><span data-stu-id="b0758-127">Argument</span></span> | <span data-ttu-id="b0758-128">Description</span><span class="sxs-lookup"><span data-stu-id="b0758-128">Description</span></span> |
|-|-|
| <span data-ttu-id="b0758-129">files</span><span class="sxs-lookup"><span data-stu-id="b0758-129">files</span></span> | <span data-ttu-id="b0758-130">Références du fichier protobuf.</span><span class="sxs-lookup"><span data-stu-id="b0758-130">The protobuf file references.</span></span> <span data-ttu-id="b0758-131">Il peut s’agir d’un chemin d’accès à glob pour les fichiers protobuf locaux.</span><span class="sxs-lookup"><span data-stu-id="b0758-131">These can be a path to glob for local protobuf files.</span></span> |

#### <a name="options"></a><span data-ttu-id="b0758-132">Options</span><span class="sxs-lookup"><span data-stu-id="b0758-132">Options</span></span>

| <span data-ttu-id="b0758-133">Option Short</span><span class="sxs-lookup"><span data-stu-id="b0758-133">Short option</span></span> | <span data-ttu-id="b0758-134">Option longue</span><span class="sxs-lookup"><span data-stu-id="b0758-134">Long option</span></span> | <span data-ttu-id="b0758-135">Description</span><span class="sxs-lookup"><span data-stu-id="b0758-135">Description</span></span> |
|-|-|-|
| <span data-ttu-id="b0758-136">-p</span><span class="sxs-lookup"><span data-stu-id="b0758-136">-p</span></span> | <span data-ttu-id="b0758-137">--projet</span><span class="sxs-lookup"><span data-stu-id="b0758-137">--project</span></span> | <span data-ttu-id="b0758-138">Chemin d’accès au fichier projet sur lequel effectuer l’opération.</span><span class="sxs-lookup"><span data-stu-id="b0758-138">The path to the project file to operate on.</span></span> <span data-ttu-id="b0758-139">Si aucun fichier n’est spécifié, la commande effectue une recherche dans le répertoire actif.</span><span class="sxs-lookup"><span data-stu-id="b0758-139">If a file is not specified, the command searches the current directory for one.</span></span>
| <span data-ttu-id="b0758-140">-S</span><span class="sxs-lookup"><span data-stu-id="b0758-140">-s</span></span> | <span data-ttu-id="b0758-141">--services</span><span class="sxs-lookup"><span data-stu-id="b0758-141">--services</span></span> | <span data-ttu-id="b0758-142">Type des services gRPC qui doivent être générés.</span><span class="sxs-lookup"><span data-stu-id="b0758-142">The type of gRPC services that should be generated.</span></span> <span data-ttu-id="b0758-143">Si `Default` est spécifié, `Both` est utilisé pour les projets Web et `Client` est utilisé pour les projets non Web.</span><span class="sxs-lookup"><span data-stu-id="b0758-143">If `Default` is specified, `Both` is used for Web projects and `Client` is used for non-Web projects.</span></span> <span data-ttu-id="b0758-144">Les valeurs acceptées sont `Both`, `Client`, `Default`, `None`, `Server`.</span><span class="sxs-lookup"><span data-stu-id="b0758-144">Accepted values are `Both`, `Client`, `Default`, `None`, `Server`.</span></span>
| <span data-ttu-id="b0758-145">-i</span><span class="sxs-lookup"><span data-stu-id="b0758-145">-i</span></span> | <span data-ttu-id="b0758-146">--Import-dirs</span><span class="sxs-lookup"><span data-stu-id="b0758-146">--additional-import-dirs</span></span> | <span data-ttu-id="b0758-147">Répertoires supplémentaires à utiliser lors de la résolution des importations des fichiers protobuf.</span><span class="sxs-lookup"><span data-stu-id="b0758-147">Additional directories to be used when resolving imports for the protobuf files.</span></span> <span data-ttu-id="b0758-148">Il s’agit d’une liste de chemins séparés par des points-virgules.</span><span class="sxs-lookup"><span data-stu-id="b0758-148">This is a semicolon separated list of paths.</span></span>
| | <span data-ttu-id="b0758-149">--accès</span><span class="sxs-lookup"><span data-stu-id="b0758-149">--access</span></span> | <span data-ttu-id="b0758-150">Modificateur d’accès à utiliser pour les classes C# générées.</span><span class="sxs-lookup"><span data-stu-id="b0758-150">The access modifier to use for the generated C# classes.</span></span> <span data-ttu-id="b0758-151">La valeur par défaut est `Public`.</span><span class="sxs-lookup"><span data-stu-id="b0758-151">The default value is `Public`.</span></span> <span data-ttu-id="b0758-152">Les valeurs acceptées sont `Internal` et `Public`.</span><span class="sxs-lookup"><span data-stu-id="b0758-152">Accepted values are `Internal` and `Public`.</span></span>

### <a name="add-url"></a><span data-ttu-id="b0758-153">Ajouter une URL</span><span class="sxs-lookup"><span data-stu-id="b0758-153">Add URL</span></span>

<span data-ttu-id="b0758-154">La commande `add-url` est utilisée pour ajouter un fichier distant spécifié par une URL source en tant que référence Protobuf.</span><span class="sxs-lookup"><span data-stu-id="b0758-154">The `add-url` command is used to add a remote file specified by an source URL as Protobuf reference.</span></span> <span data-ttu-id="b0758-155">Un chemin d’accès de fichier doit être fourni pour spécifier l’emplacement de téléchargement du fichier distant.</span><span class="sxs-lookup"><span data-stu-id="b0758-155">A file path must be provided to specify where to download the remote file.</span></span> <span data-ttu-id="b0758-156">Le chemin d’accès du fichier peut être relatif au répertoire actif ou à un chemin d’accès absolu.</span><span class="sxs-lookup"><span data-stu-id="b0758-156">The file path can be relative to the current directory or an absolute path.</span></span> <span data-ttu-id="b0758-157">Si le chemin d’accès au fichier se trouve en dehors du répertoire du projet, un élément `Link` est ajouté pour afficher le fichier dans le dossier virtuel `Protos` dans Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="b0758-157">If the file path is outside the project directory, a `Link` element is added to display the file under the virtual folder `Protos` in Visual Studio.</span></span>

### <a name="usage"></a><span data-ttu-id="b0758-158">Usage</span><span class="sxs-lookup"><span data-stu-id="b0758-158">Usage</span></span>

```dotnetcli
dotnet-grpc add-url [options] <url>
```

#### <a name="arguments"></a><span data-ttu-id="b0758-159">Arguments</span><span class="sxs-lookup"><span data-stu-id="b0758-159">Arguments</span></span>

| <span data-ttu-id="b0758-160">Argument</span><span class="sxs-lookup"><span data-stu-id="b0758-160">Argument</span></span> | <span data-ttu-id="b0758-161">Description</span><span class="sxs-lookup"><span data-stu-id="b0758-161">Description</span></span> |
|-|-|
| <span data-ttu-id="b0758-162">url</span><span class="sxs-lookup"><span data-stu-id="b0758-162">url</span></span> | <span data-ttu-id="b0758-163">URL d’un fichier protobuf distant.</span><span class="sxs-lookup"><span data-stu-id="b0758-163">The URL to a remote protobuf file.</span></span> |

#### <a name="options"></a><span data-ttu-id="b0758-164">Options</span><span class="sxs-lookup"><span data-stu-id="b0758-164">Options</span></span>

| <span data-ttu-id="b0758-165">Option Short</span><span class="sxs-lookup"><span data-stu-id="b0758-165">Short option</span></span> | <span data-ttu-id="b0758-166">Option longue</span><span class="sxs-lookup"><span data-stu-id="b0758-166">Long option</span></span> | <span data-ttu-id="b0758-167">Description</span><span class="sxs-lookup"><span data-stu-id="b0758-167">Description</span></span> |
|-|-|-|
| <span data-ttu-id="b0758-168">-o</span><span class="sxs-lookup"><span data-stu-id="b0758-168">-o</span></span> | <span data-ttu-id="b0758-169">--output</span><span class="sxs-lookup"><span data-stu-id="b0758-169">--output</span></span> | <span data-ttu-id="b0758-170">Spécifie le chemin de téléchargement du fichier protobuf distant.</span><span class="sxs-lookup"><span data-stu-id="b0758-170">Specifies the download path for the remote protobuf file.</span></span> <span data-ttu-id="b0758-171">C'est une option obligatoire.</span><span class="sxs-lookup"><span data-stu-id="b0758-171">This is a required option.</span></span>
| <span data-ttu-id="b0758-172">-p</span><span class="sxs-lookup"><span data-stu-id="b0758-172">-p</span></span> | <span data-ttu-id="b0758-173">--projet</span><span class="sxs-lookup"><span data-stu-id="b0758-173">--project</span></span> | <span data-ttu-id="b0758-174">Chemin d’accès au fichier projet sur lequel effectuer l’opération.</span><span class="sxs-lookup"><span data-stu-id="b0758-174">The path to the project file to operate on.</span></span> <span data-ttu-id="b0758-175">Si aucun fichier n’est spécifié, la commande effectue une recherche dans le répertoire actif.</span><span class="sxs-lookup"><span data-stu-id="b0758-175">If a file is not specified, the command searches the current directory for one.</span></span>
| <span data-ttu-id="b0758-176">-S</span><span class="sxs-lookup"><span data-stu-id="b0758-176">-s</span></span> | <span data-ttu-id="b0758-177">--services</span><span class="sxs-lookup"><span data-stu-id="b0758-177">--services</span></span> | <span data-ttu-id="b0758-178">Type des services gRPC qui doivent être générés.</span><span class="sxs-lookup"><span data-stu-id="b0758-178">The type of gRPC services that should be generated.</span></span> <span data-ttu-id="b0758-179">Si `Default` est spécifié, `Both` est utilisé pour les projets Web et `Client` est utilisé pour les projets non Web.</span><span class="sxs-lookup"><span data-stu-id="b0758-179">If `Default` is specified, `Both` is used for Web projects and `Client` is used for non-Web projects.</span></span> <span data-ttu-id="b0758-180">Les valeurs acceptées sont `Both`, `Client`, `Default`, `None`, `Server`.</span><span class="sxs-lookup"><span data-stu-id="b0758-180">Accepted values are `Both`, `Client`, `Default`, `None`, `Server`.</span></span>
| <span data-ttu-id="b0758-181">-i</span><span class="sxs-lookup"><span data-stu-id="b0758-181">-i</span></span> | <span data-ttu-id="b0758-182">--Import-dirs</span><span class="sxs-lookup"><span data-stu-id="b0758-182">--additional-import-dirs</span></span> | <span data-ttu-id="b0758-183">Répertoires supplémentaires à utiliser lors de la résolution des importations des fichiers protobuf.</span><span class="sxs-lookup"><span data-stu-id="b0758-183">Additional directories to be used when resolving imports for the protobuf files.</span></span> <span data-ttu-id="b0758-184">Il s’agit d’une liste de chemins séparés par des points-virgules.</span><span class="sxs-lookup"><span data-stu-id="b0758-184">This is a semicolon separated list of paths.</span></span>
| | <span data-ttu-id="b0758-185">--accès</span><span class="sxs-lookup"><span data-stu-id="b0758-185">--access</span></span> | <span data-ttu-id="b0758-186">Modificateur d’accès à utiliser pour les classes C# générées.</span><span class="sxs-lookup"><span data-stu-id="b0758-186">The access modifier to use for the generated C# classes.</span></span> <span data-ttu-id="b0758-187">La valeur par défaut est `Public`.</span><span class="sxs-lookup"><span data-stu-id="b0758-187">Default value is `Public`.</span></span> <span data-ttu-id="b0758-188">Les valeurs acceptées sont `Internal` et `Public`.</span><span class="sxs-lookup"><span data-stu-id="b0758-188">Accepted values are `Internal` and `Public`.</span></span>

## <a name="remove"></a><span data-ttu-id="b0758-189">Supprimer</span><span class="sxs-lookup"><span data-stu-id="b0758-189">Remove</span></span>

<span data-ttu-id="b0758-190">La commande `remove` est utilisée pour supprimer les références Protobuf du fichier *. csproj* .</span><span class="sxs-lookup"><span data-stu-id="b0758-190">The `remove` command is used to remove Protobuf references from the *.csproj* file.</span></span> <span data-ttu-id="b0758-191">La commande accepte les arguments de chemin d’accès et les URL source comme arguments.</span><span class="sxs-lookup"><span data-stu-id="b0758-191">The command accepts path arguments and source URLs as arguments.</span></span> <span data-ttu-id="b0758-192">L’outil :</span><span class="sxs-lookup"><span data-stu-id="b0758-192">The tool:</span></span>

* <span data-ttu-id="b0758-193">Supprime uniquement la référence Protobuf.</span><span class="sxs-lookup"><span data-stu-id="b0758-193">Only removes the Protobuf reference.</span></span>
* <span data-ttu-id="b0758-194">Ne supprime pas le fichier *. proto* , même s’il a été téléchargé à l’origine à partir d’une URL distante.</span><span class="sxs-lookup"><span data-stu-id="b0758-194">Does not delete the *.proto* file, even if it was originally downloaded from a remote URL.</span></span>

### <a name="usage"></a><span data-ttu-id="b0758-195">Usage</span><span class="sxs-lookup"><span data-stu-id="b0758-195">Usage</span></span>

```dotnetcli
dotnet-grpc remove [options] <references>...
```

### <a name="arguments"></a><span data-ttu-id="b0758-196">Arguments</span><span class="sxs-lookup"><span data-stu-id="b0758-196">Arguments</span></span>

| <span data-ttu-id="b0758-197">Argument</span><span class="sxs-lookup"><span data-stu-id="b0758-197">Argument</span></span> | <span data-ttu-id="b0758-198">Description</span><span class="sxs-lookup"><span data-stu-id="b0758-198">Description</span></span> |
|-|-|
| <span data-ttu-id="b0758-199">Références</span><span class="sxs-lookup"><span data-stu-id="b0758-199">references</span></span> | <span data-ttu-id="b0758-200">URL ou chemins de fichier des références protobuf à supprimer.</span><span class="sxs-lookup"><span data-stu-id="b0758-200">The URLs or file paths of the protobuf references to remove.</span></span> |

### <a name="options"></a><span data-ttu-id="b0758-201">Options</span><span class="sxs-lookup"><span data-stu-id="b0758-201">Options</span></span>

| <span data-ttu-id="b0758-202">Option Short</span><span class="sxs-lookup"><span data-stu-id="b0758-202">Short option</span></span> | <span data-ttu-id="b0758-203">Option longue</span><span class="sxs-lookup"><span data-stu-id="b0758-203">Long option</span></span> | <span data-ttu-id="b0758-204">Description</span><span class="sxs-lookup"><span data-stu-id="b0758-204">Description</span></span> |
|-|-|-|
| <span data-ttu-id="b0758-205">-p</span><span class="sxs-lookup"><span data-stu-id="b0758-205">-p</span></span> | <span data-ttu-id="b0758-206">--projet</span><span class="sxs-lookup"><span data-stu-id="b0758-206">--project</span></span> | <span data-ttu-id="b0758-207">Chemin d’accès au fichier projet sur lequel effectuer l’opération.</span><span class="sxs-lookup"><span data-stu-id="b0758-207">The path to the project file to operate on.</span></span> <span data-ttu-id="b0758-208">Si aucun fichier n’est spécifié, la commande effectue une recherche dans le répertoire actif.</span><span class="sxs-lookup"><span data-stu-id="b0758-208">If a file is not specified, the command searches the current directory for one.</span></span>

## <a name="refresh"></a><span data-ttu-id="b0758-209">Actualiser</span><span class="sxs-lookup"><span data-stu-id="b0758-209">Refresh</span></span>

<span data-ttu-id="b0758-210">La commande `refresh` est utilisée pour mettre à jour une référence distante avec le contenu le plus récent à partir de l’URL source.</span><span class="sxs-lookup"><span data-stu-id="b0758-210">The `refresh` command is used to update a remote reference with the latest content from the source URL.</span></span> <span data-ttu-id="b0758-211">Le chemin d’accès au fichier de téléchargement et l’URL source peuvent être utilisés pour spécifier la référence à mettre à jour.</span><span class="sxs-lookup"><span data-stu-id="b0758-211">Both the download file path and the source URL can be used to specify the reference to be updated.</span></span> <span data-ttu-id="b0758-212">Remarque :</span><span class="sxs-lookup"><span data-stu-id="b0758-212">Note:</span></span>

* <span data-ttu-id="b0758-213">Les hachages du contenu du fichier sont comparés pour déterminer si le fichier local doit être mis à jour.</span><span class="sxs-lookup"><span data-stu-id="b0758-213">The hashes of the file contents are compared to determine whether the local file should be updated.</span></span>
* <span data-ttu-id="b0758-214">Aucune information d’horodatage n’est comparée.</span><span class="sxs-lookup"><span data-stu-id="b0758-214">No timestamp information is compared.</span></span>

<span data-ttu-id="b0758-215">L’outil remplace toujours le fichier local par le fichier distant si une mise à jour est nécessaire.</span><span class="sxs-lookup"><span data-stu-id="b0758-215">The tool always replaces the local file with the remote file if an update is needed.</span></span>

### <a name="usage"></a><span data-ttu-id="b0758-216">Usage</span><span class="sxs-lookup"><span data-stu-id="b0758-216">Usage</span></span>

```dotnetcli
dotnet-grpc refresh [options] [<references>...]
```

### <a name="arguments"></a><span data-ttu-id="b0758-217">Arguments</span><span class="sxs-lookup"><span data-stu-id="b0758-217">Arguments</span></span>

| <span data-ttu-id="b0758-218">Argument</span><span class="sxs-lookup"><span data-stu-id="b0758-218">Argument</span></span> | <span data-ttu-id="b0758-219">Description</span><span class="sxs-lookup"><span data-stu-id="b0758-219">Description</span></span> |
|-|-|
| <span data-ttu-id="b0758-220">Références</span><span class="sxs-lookup"><span data-stu-id="b0758-220">references</span></span> | <span data-ttu-id="b0758-221">URL ou chemins d’accès aux références protobuf distantes qui doivent être mises à jour.</span><span class="sxs-lookup"><span data-stu-id="b0758-221">The URLs or file paths to remote protobuf references that should be updated.</span></span> <span data-ttu-id="b0758-222">Laissez cet argument vide pour actualiser toutes les références distantes.</span><span class="sxs-lookup"><span data-stu-id="b0758-222">Leave this argument empty to refresh all remote references.</span></span> |

### <a name="options"></a><span data-ttu-id="b0758-223">Options</span><span class="sxs-lookup"><span data-stu-id="b0758-223">Options</span></span>

| <span data-ttu-id="b0758-224">Option Short</span><span class="sxs-lookup"><span data-stu-id="b0758-224">Short option</span></span> | <span data-ttu-id="b0758-225">Option longue</span><span class="sxs-lookup"><span data-stu-id="b0758-225">Long option</span></span> | <span data-ttu-id="b0758-226">Description</span><span class="sxs-lookup"><span data-stu-id="b0758-226">Description</span></span> |
|-|-|-|
| <span data-ttu-id="b0758-227">-p</span><span class="sxs-lookup"><span data-stu-id="b0758-227">-p</span></span> | <span data-ttu-id="b0758-228">--projet</span><span class="sxs-lookup"><span data-stu-id="b0758-228">--project</span></span> | <span data-ttu-id="b0758-229">Chemin d’accès au fichier projet sur lequel effectuer l’opération.</span><span class="sxs-lookup"><span data-stu-id="b0758-229">The path to the project file to operate on.</span></span> <span data-ttu-id="b0758-230">Si aucun fichier n’est spécifié, la commande effectue une recherche dans le répertoire actif.</span><span class="sxs-lookup"><span data-stu-id="b0758-230">If a file is not specified, the command searches the current directory for one.</span></span>
| | <span data-ttu-id="b0758-231">--à sec-exécution</span><span class="sxs-lookup"><span data-stu-id="b0758-231">--dry-run</span></span> | <span data-ttu-id="b0758-232">Génère une liste des fichiers qui seraient mis à jour sans télécharger de nouveau contenu.</span><span class="sxs-lookup"><span data-stu-id="b0758-232">Outputs a list of files that would be updated without downloading any new content.</span></span>

## <a name="list"></a><span data-ttu-id="b0758-233">List</span><span class="sxs-lookup"><span data-stu-id="b0758-233">List</span></span>

<span data-ttu-id="b0758-234">La commande `list` est utilisée pour afficher toutes les références Protobuf dans le fichier projet.</span><span class="sxs-lookup"><span data-stu-id="b0758-234">The `list` command is used to display all the Protobuf references in the project file.</span></span> <span data-ttu-id="b0758-235">Si toutes les valeurs d’une colonne sont des valeurs par défaut, la colonne peut être omise.</span><span class="sxs-lookup"><span data-stu-id="b0758-235">If all values of a column are default values, the column may be omitted.</span></span>

### <a name="usage"></a><span data-ttu-id="b0758-236">Usage</span><span class="sxs-lookup"><span data-stu-id="b0758-236">Usage</span></span>

```dotnetcli
dotnet-grpc list [options]
```

### <a name="options"></a><span data-ttu-id="b0758-237">Options</span><span class="sxs-lookup"><span data-stu-id="b0758-237">Options</span></span>

| <span data-ttu-id="b0758-238">Option Short</span><span class="sxs-lookup"><span data-stu-id="b0758-238">Short option</span></span> | <span data-ttu-id="b0758-239">Option longue</span><span class="sxs-lookup"><span data-stu-id="b0758-239">Long option</span></span> | <span data-ttu-id="b0758-240">Description</span><span class="sxs-lookup"><span data-stu-id="b0758-240">Description</span></span> |
|-|-|-|
| <span data-ttu-id="b0758-241">-p</span><span class="sxs-lookup"><span data-stu-id="b0758-241">-p</span></span> | <span data-ttu-id="b0758-242">--projet</span><span class="sxs-lookup"><span data-stu-id="b0758-242">--project</span></span> | <span data-ttu-id="b0758-243">Chemin d’accès au fichier projet sur lequel effectuer l’opération.</span><span class="sxs-lookup"><span data-stu-id="b0758-243">The path to the project file to operate on.</span></span> <span data-ttu-id="b0758-244">Si aucun fichier n’est spécifié, la commande effectue une recherche dans le répertoire actif.</span><span class="sxs-lookup"><span data-stu-id="b0758-244">If a file is not specified, the command searches the current directory for one.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="b0758-245">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="b0758-245">Additional resources</span></span>

* <xref:grpc/index>
* <xref:grpc/basics>
* <xref:grpc/aspnetcore>
