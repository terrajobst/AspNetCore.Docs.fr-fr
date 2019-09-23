---
title: Développer des applications ASP.NET Core à l’aide de OpenAPI
author: ryanbrandenburg
description: Montre comment utiliser l’outil « Microsoft. dotnet-openapi » pour ajouter des références aux fichiers OpenAPI.
ms.author: rybrande
ms.date: 08/26/2019
monikerRange: '>= aspnetcore-3.0'
uid: web-api/Microsoft.dotnet-openapi
ms.openlocfilehash: a9b38bb7e69744d72867bf69cecf1fa92d7c15b3
ms.sourcegitcommit: d34b2627a69bc8940b76a949de830335db9701d3
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/23/2019
ms.locfileid: "71187459"
---
# <a name="develop-aspnet-core-apps-using-openapi-tools"></a><span data-ttu-id="9611a-103">Développer des applications ASP.NET Core à l’aide des outils OpenAPI</span><span class="sxs-lookup"><span data-stu-id="9611a-103">Develop ASP.NET Core apps using OpenAPI tools</span></span>

<span data-ttu-id="9611a-104">Par Ryan Brandenburg</span><span class="sxs-lookup"><span data-stu-id="9611a-104">By Ryan Brandenburg</span></span>

<span data-ttu-id="9611a-105">`Microsoft.dotnet-openapi`est un outil Global .NET Core pour la gestion des références [openapi](https://github.com/OAI/OpenAPI-Specification) dans un projet.</span><span class="sxs-lookup"><span data-stu-id="9611a-105">`Microsoft.dotnet-openapi` is a .NET Core global tool for managing [OpenAPI](https://github.com/OAI/OpenAPI-Specification) references within a project.</span></span>

## <a name="installation"></a><span data-ttu-id="9611a-106">Installation</span><span class="sxs-lookup"><span data-stu-id="9611a-106">Installation</span></span>

<span data-ttu-id="9611a-107">Pour installer `Microsoft.dotnet-openapi`, exécutez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="9611a-107">To install `Microsoft.dotnet-openapi`, run the following command:</span></span>

```console
dotnet tool install -g Microsoft.dotnet-openapi
```

<span data-ttu-id="9611a-108">`Microsoft.dotnet-openapi`est un [outil global de .net Core](/dotnet/core/tools/global-tools).</span><span class="sxs-lookup"><span data-stu-id="9611a-108">`Microsoft.dotnet-openapi` is a [.NET Core Global Tool](/dotnet/core/tools/global-tools).</span></span>

## <a name="add"></a><span data-ttu-id="9611a-109">Ajouter</span><span class="sxs-lookup"><span data-stu-id="9611a-109">Add</span></span>

<span data-ttu-id="9611a-110">L’ajout d’une référence openapi à l’aide de l’une des commandes `<OpenApiReference />` de cette page ajoute un élément semblable au suivant au fichier *. csproj* :</span><span class="sxs-lookup"><span data-stu-id="9611a-110">Adding an OpenAPI reference using any of the commands on this page adds an `<OpenApiReference />`  element similar to the following to the *.csproj* file:</span></span>

```xml
<OpenApiReference Include="openapi.json" />
```

<span data-ttu-id="9611a-111">La référence précédente est requise pour que l’application appelle le code client généré.</span><span class="sxs-lookup"><span data-stu-id="9611a-111">The preceding reference is required for the app to call the generated client code.</span></span>

<!-- TODO: Restore after https://github.com/aspnet/AspNetCore/issues/12738
### Add Project

#### Options

| Short option | Long option | Description | Example |
|-------|------|-------|---------|
| -v|--verbose | Show verbose output. |dotnet openapi add project *-v* ../Ref/ProjRef.csproj |
| -p|--project | The project to operate on. |dotnet openapi add project *--project .\Ref.csproj* ../Ref/ProjRef.csproj |

#### Arguments

|  Argument  | Description | Example |
|-------------|-------------|---------|
| source-file | The source to create a reference from. Must be a project file. |dotnet openapi add project *../Ref/ProjRef.csproj* | -->

### <a name="add-file"></a><span data-ttu-id="9611a-112">Ajouter un fichier</span><span class="sxs-lookup"><span data-stu-id="9611a-112">Add File</span></span>

#### <a name="options"></a><span data-ttu-id="9611a-113">Options</span><span class="sxs-lookup"><span data-stu-id="9611a-113">Options</span></span>

| <span data-ttu-id="9611a-114">Option Short</span><span class="sxs-lookup"><span data-stu-id="9611a-114">Short option</span></span>| <span data-ttu-id="9611a-115">Option longue</span><span class="sxs-lookup"><span data-stu-id="9611a-115">Long option</span></span>| <span data-ttu-id="9611a-116">Description</span><span class="sxs-lookup"><span data-stu-id="9611a-116">Description</span></span> | <span data-ttu-id="9611a-117">Exemple</span><span class="sxs-lookup"><span data-stu-id="9611a-117">Example</span></span> |
|-------|------|-------|---------|
| <span data-ttu-id="9611a-118">-v</span><span class="sxs-lookup"><span data-stu-id="9611a-118">-v</span></span>|<span data-ttu-id="9611a-119">--verbose</span><span class="sxs-lookup"><span data-stu-id="9611a-119">--verbose</span></span> | <span data-ttu-id="9611a-120">Affichez la sortie détaillée.</span><span class="sxs-lookup"><span data-stu-id="9611a-120">Show verbose output.</span></span> |<span data-ttu-id="9611a-121">dotnet openapi ajouter *un fichier-v* .\OpenAPI.JSON</span><span class="sxs-lookup"><span data-stu-id="9611a-121">dotnet openapi add file *-v* .\OpenAPI.json</span></span> |
| <span data-ttu-id="9611a-122">-p</span><span class="sxs-lookup"><span data-stu-id="9611a-122">-p</span></span>|<span data-ttu-id="9611a-123">--updateProject</span><span class="sxs-lookup"><span data-stu-id="9611a-123">--updateProject</span></span> | <span data-ttu-id="9611a-124">Projet sur lequel effectuer l’opération.</span><span class="sxs-lookup"><span data-stu-id="9611a-124">The project to operate on.</span></span> |<span data-ttu-id="9611a-125">Ajouter un fichier à dotnet openapi *--updateProject .\Ref.csproj* .\OpenAPI.JSON</span><span class="sxs-lookup"><span data-stu-id="9611a-125">dotnet openapi add file *--updateProject .\Ref.csproj* .\OpenAPI.json</span></span> |
| <span data-ttu-id="9611a-126">-c</span><span class="sxs-lookup"><span data-stu-id="9611a-126">-c</span></span>|<span data-ttu-id="9611a-127">--Code-Generator</span><span class="sxs-lookup"><span data-stu-id="9611a-127">--code-generator</span></span>| <span data-ttu-id="9611a-128">Générateur de code à appliquer à la référence.</span><span class="sxs-lookup"><span data-stu-id="9611a-128">The code generator to apply to the reference.</span></span> <span data-ttu-id="9611a-129">Les options `NSwagCSharp` sont `NSwagTypeScript`et.</span><span class="sxs-lookup"><span data-stu-id="9611a-129">Options are `NSwagCSharp` and `NSwagTypeScript`.</span></span> <span data-ttu-id="9611a-130">Si `--code-generator` n’est pas spécifié `NSwagCSharp`, l’outil prend par défaut la valeur.</span><span class="sxs-lookup"><span data-stu-id="9611a-130">If `--code-generator` is not specified the tooling defaults to `NSwagCSharp`.</span></span>|<span data-ttu-id="9611a-131">dotnet openapi ajouter un fichier .\OpenApi.json--Code-Generator</span><span class="sxs-lookup"><span data-stu-id="9611a-131">dotnet openapi add file .\OpenApi.json --code-generator</span></span>
| <span data-ttu-id="9611a-132">-h</span><span class="sxs-lookup"><span data-stu-id="9611a-132">-h</span></span>|<span data-ttu-id="9611a-133">--aide</span><span class="sxs-lookup"><span data-stu-id="9611a-133">--help</span></span>|<span data-ttu-id="9611a-134">Afficher les informations d’aide</span><span class="sxs-lookup"><span data-stu-id="9611a-134">Show help information</span></span>|<span data-ttu-id="9611a-135">Ajouter un fichier Dotnet openapi--Help</span><span class="sxs-lookup"><span data-stu-id="9611a-135">dotnet openapi add file --help</span></span>|

#### <a name="arguments"></a><span data-ttu-id="9611a-136">Arguments</span><span class="sxs-lookup"><span data-stu-id="9611a-136">Arguments</span></span>

|  <span data-ttu-id="9611a-137">Argument</span><span class="sxs-lookup"><span data-stu-id="9611a-137">Argument</span></span>  | <span data-ttu-id="9611a-138">Description</span><span class="sxs-lookup"><span data-stu-id="9611a-138">Description</span></span> | <span data-ttu-id="9611a-139">Exemple</span><span class="sxs-lookup"><span data-stu-id="9611a-139">Example</span></span> |
|-------------|-------------|---------|
| <span data-ttu-id="9611a-140">fichier source</span><span class="sxs-lookup"><span data-stu-id="9611a-140">source-file</span></span> | <span data-ttu-id="9611a-141">Source à partir de laquelle créer une référence.</span><span class="sxs-lookup"><span data-stu-id="9611a-141">The source to create a reference from.</span></span> <span data-ttu-id="9611a-142">Doit être un fichier OpenAPI.</span><span class="sxs-lookup"><span data-stu-id="9611a-142">Must be an OpenAPI file.</span></span> |<span data-ttu-id="9611a-143">dotnet openapi ajouter un fichier *.\OpenAPI.JSON*</span><span class="sxs-lookup"><span data-stu-id="9611a-143">dotnet openapi add file *.\OpenAPI.json*</span></span> |

### <a name="add-url"></a><span data-ttu-id="9611a-144">Ajouter une URL</span><span class="sxs-lookup"><span data-stu-id="9611a-144">Add URL</span></span>

#### <a name="options"></a><span data-ttu-id="9611a-145">Options</span><span class="sxs-lookup"><span data-stu-id="9611a-145">Options</span></span>

| <span data-ttu-id="9611a-146">Option Short</span><span class="sxs-lookup"><span data-stu-id="9611a-146">Short option</span></span>| <span data-ttu-id="9611a-147">Option longue</span><span class="sxs-lookup"><span data-stu-id="9611a-147">Long option</span></span>| <span data-ttu-id="9611a-148">Description</span><span class="sxs-lookup"><span data-stu-id="9611a-148">Description</span></span> | <span data-ttu-id="9611a-149">Exemple</span><span class="sxs-lookup"><span data-stu-id="9611a-149">Example</span></span> |
|-------|------|-------------|---------|
| <span data-ttu-id="9611a-150">-v</span><span class="sxs-lookup"><span data-stu-id="9611a-150">-v</span></span>|<span data-ttu-id="9611a-151">--verbose</span><span class="sxs-lookup"><span data-stu-id="9611a-151">--verbose</span></span> | <span data-ttu-id="9611a-152">Affichez la sortie détaillée.</span><span class="sxs-lookup"><span data-stu-id="9611a-152">Show verbose output.</span></span> |<span data-ttu-id="9611a-153">Ajouter l’URL dotnet openapi *-v*`https://contoso.com/openapi.json`</span><span class="sxs-lookup"><span data-stu-id="9611a-153">dotnet openapi add url *-v* `https://contoso.com/openapi.json`</span></span> |
| <span data-ttu-id="9611a-154">-p</span><span class="sxs-lookup"><span data-stu-id="9611a-154">-p</span></span>|<span data-ttu-id="9611a-155">--updateProject</span><span class="sxs-lookup"><span data-stu-id="9611a-155">--updateProject</span></span> | <span data-ttu-id="9611a-156">Projet sur lequel effectuer l’opération.</span><span class="sxs-lookup"><span data-stu-id="9611a-156">The project to operate on.</span></span> |<span data-ttu-id="9611a-157">Ajouter une URL dotnet openapi *--updateProject .\Ref.csproj*`https://contoso.com/openapi.json`</span><span class="sxs-lookup"><span data-stu-id="9611a-157">dotnet openapi add url *--updateProject .\Ref.csproj* `https://contoso.com/openapi.json`</span></span> |
| <span data-ttu-id="9611a-158">-o</span><span class="sxs-lookup"><span data-stu-id="9611a-158">-o</span></span>|<span data-ttu-id="9611a-159">--fichier de sortie</span><span class="sxs-lookup"><span data-stu-id="9611a-159">--output-file</span></span> | <span data-ttu-id="9611a-160">Où placer la copie locale du fichier OpenAPI.</span><span class="sxs-lookup"><span data-stu-id="9611a-160">Where to place the local copy of the OpenAPI file.</span></span> |<span data-ttu-id="9611a-161">dotnet openapi ajouter une `https://contoso.com/openapi.json` URL *--sortie-fichier MyClient. JSON*</span><span class="sxs-lookup"><span data-stu-id="9611a-161">dotnet openapi add url `https://contoso.com/openapi.json` *--output-file myclient.json*</span></span> |
| <span data-ttu-id="9611a-162">-c</span><span class="sxs-lookup"><span data-stu-id="9611a-162">-c</span></span>|<span data-ttu-id="9611a-163">--Code-Generator</span><span class="sxs-lookup"><span data-stu-id="9611a-163">--code-generator</span></span>| <span data-ttu-id="9611a-164">Générateur de code à appliquer à la référence.</span><span class="sxs-lookup"><span data-stu-id="9611a-164">The code generator to apply to the reference.</span></span> <span data-ttu-id="9611a-165">Les options `NSwagCSharp` sont `NSwagTypeScript`et.</span><span class="sxs-lookup"><span data-stu-id="9611a-165">Options are `NSwagCSharp` and `NSwagTypeScript`.</span></span> |<span data-ttu-id="9611a-166">dotnet openapi ajouter un fichier .\OpenApi.json--Code-Generator</span><span class="sxs-lookup"><span data-stu-id="9611a-166">dotnet openapi add file .\OpenApi.json --code-generator</span></span>
| <span data-ttu-id="9611a-167">-h</span><span class="sxs-lookup"><span data-stu-id="9611a-167">-h</span></span>|<span data-ttu-id="9611a-168">--aide</span><span class="sxs-lookup"><span data-stu-id="9611a-168">--help</span></span>|<span data-ttu-id="9611a-169">Afficher les informations d’aide</span><span class="sxs-lookup"><span data-stu-id="9611a-169">Show help information</span></span>|<span data-ttu-id="9611a-170">Ajouter une URL dotnet openapi--Help</span><span class="sxs-lookup"><span data-stu-id="9611a-170">dotnet openapi add url --help</span></span>|

#### <a name="arguments"></a><span data-ttu-id="9611a-171">Arguments</span><span class="sxs-lookup"><span data-stu-id="9611a-171">Arguments</span></span>

|  <span data-ttu-id="9611a-172">Argument</span><span class="sxs-lookup"><span data-stu-id="9611a-172">Argument</span></span>  | <span data-ttu-id="9611a-173">Description</span><span class="sxs-lookup"><span data-stu-id="9611a-173">Description</span></span> | <span data-ttu-id="9611a-174">Exemple</span><span class="sxs-lookup"><span data-stu-id="9611a-174">Example</span></span> |
|-------------|-------------|---------|
| <span data-ttu-id="9611a-175">URL source</span><span class="sxs-lookup"><span data-stu-id="9611a-175">source-URL</span></span> | <span data-ttu-id="9611a-176">Source à partir de laquelle créer une référence.</span><span class="sxs-lookup"><span data-stu-id="9611a-176">The source to create a reference from.</span></span> <span data-ttu-id="9611a-177">Doit être une URL.</span><span class="sxs-lookup"><span data-stu-id="9611a-177">Must be a URL.</span></span> |<span data-ttu-id="9611a-178">URL d’ajout de dotnet openapi`https://contoso.com/openapi.json`</span><span class="sxs-lookup"><span data-stu-id="9611a-178">dotnet openapi add url `https://contoso.com/openapi.json`</span></span> |

## <a name="remove"></a><span data-ttu-id="9611a-179">Remove</span><span class="sxs-lookup"><span data-stu-id="9611a-179">Remove</span></span>

<span data-ttu-id="9611a-180">Supprime la référence OpenAPI correspondant au nom de fichier donné du fichier *. csproj* .</span><span class="sxs-lookup"><span data-stu-id="9611a-180">Removes the OpenAPI reference matching the given filename from the *.csproj* file.</span></span> <span data-ttu-id="9611a-181">Lorsque la référence OpenAPI est supprimée, les clients ne sont pas générés.</span><span class="sxs-lookup"><span data-stu-id="9611a-181">When the OpenAPI reference is removed, clients won't be generated.</span></span> <span data-ttu-id="9611a-182">Les fichiers local *. JSON* et *. YAML* sont supprimés.</span><span class="sxs-lookup"><span data-stu-id="9611a-182">Local *.json* and *.yaml* files are deleted.</span></span>

### <a name="options"></a><span data-ttu-id="9611a-183">Options</span><span class="sxs-lookup"><span data-stu-id="9611a-183">Options</span></span>

| <span data-ttu-id="9611a-184">Option Short</span><span class="sxs-lookup"><span data-stu-id="9611a-184">Short option</span></span>| <span data-ttu-id="9611a-185">Option longue</span><span class="sxs-lookup"><span data-stu-id="9611a-185">Long option</span></span>| <span data-ttu-id="9611a-186">Description</span><span class="sxs-lookup"><span data-stu-id="9611a-186">Description</span></span>| <span data-ttu-id="9611a-187">Exemple</span><span class="sxs-lookup"><span data-stu-id="9611a-187">Example</span></span> |
|-------|------|------------|---------|
| <span data-ttu-id="9611a-188">-v</span><span class="sxs-lookup"><span data-stu-id="9611a-188">-v</span></span>|<span data-ttu-id="9611a-189">--verbose</span><span class="sxs-lookup"><span data-stu-id="9611a-189">--verbose</span></span> | <span data-ttu-id="9611a-190">Affichez la sortie détaillée.</span><span class="sxs-lookup"><span data-stu-id="9611a-190">Show verbose output.</span></span> |<span data-ttu-id="9611a-191">openapi de la suppression dotnet *-v*</span><span class="sxs-lookup"><span data-stu-id="9611a-191">dotnet openapi remove *-v*</span></span>|
| <span data-ttu-id="9611a-192">-p</span><span class="sxs-lookup"><span data-stu-id="9611a-192">-p</span></span>|<span data-ttu-id="9611a-193">--updateProject</span><span class="sxs-lookup"><span data-stu-id="9611a-193">--updateProject</span></span> | <span data-ttu-id="9611a-194">Projet sur lequel effectuer l’opération.</span><span class="sxs-lookup"><span data-stu-id="9611a-194">The project to operate on.</span></span> |<span data-ttu-id="9611a-195">dotnet openapi Remove *--updateProject .\Ref.csproj* .\OpenAPI.JSON</span><span class="sxs-lookup"><span data-stu-id="9611a-195">dotnet openapi remove *--updateProject .\Ref.csproj* .\OpenAPI.json</span></span> |
| <span data-ttu-id="9611a-196">-h</span><span class="sxs-lookup"><span data-stu-id="9611a-196">-h</span></span>|<span data-ttu-id="9611a-197">--aide</span><span class="sxs-lookup"><span data-stu-id="9611a-197">--help</span></span>|<span data-ttu-id="9611a-198">Afficher les informations d’aide</span><span class="sxs-lookup"><span data-stu-id="9611a-198">Show help information</span></span>|<span data-ttu-id="9611a-199">openapi de la suppression dotnet--Help</span><span class="sxs-lookup"><span data-stu-id="9611a-199">dotnet openapi remove --help</span></span>|

### <a name="arguments"></a><span data-ttu-id="9611a-200">Arguments</span><span class="sxs-lookup"><span data-stu-id="9611a-200">Arguments</span></span>

|  <span data-ttu-id="9611a-201">Argument</span><span class="sxs-lookup"><span data-stu-id="9611a-201">Argument</span></span>  | <span data-ttu-id="9611a-202">Description</span><span class="sxs-lookup"><span data-stu-id="9611a-202">Description</span></span>| <span data-ttu-id="9611a-203">Exemple</span><span class="sxs-lookup"><span data-stu-id="9611a-203">Example</span></span> |
| ------------|------------|---------|
| <span data-ttu-id="9611a-204">fichier source</span><span class="sxs-lookup"><span data-stu-id="9611a-204">source-file</span></span> | <span data-ttu-id="9611a-205">Source à laquelle supprimer la référence.</span><span class="sxs-lookup"><span data-stu-id="9611a-205">The source to remove the reference to.</span></span> |<span data-ttu-id="9611a-206">dotnet openapi supprimer *.\OpenAPI.JSON*</span><span class="sxs-lookup"><span data-stu-id="9611a-206">dotnet openapi remove *.\OpenAPI.json*</span></span> |

## <a name="refresh"></a><span data-ttu-id="9611a-207">Actualisation</span><span class="sxs-lookup"><span data-stu-id="9611a-207">Refresh</span></span>

<span data-ttu-id="9611a-208">Actualise la version locale d’un fichier qui a été téléchargé à l’aide du contenu le plus récent à partir de l’URL de téléchargement.</span><span class="sxs-lookup"><span data-stu-id="9611a-208">Refreshes the local version of a file that was downloaded using the latest content from the download URL.</span></span>

### <a name="options"></a><span data-ttu-id="9611a-209">Options</span><span class="sxs-lookup"><span data-stu-id="9611a-209">Options</span></span>

| <span data-ttu-id="9611a-210">Option Short</span><span class="sxs-lookup"><span data-stu-id="9611a-210">Short option</span></span>| <span data-ttu-id="9611a-211">Option longue</span><span class="sxs-lookup"><span data-stu-id="9611a-211">Long option</span></span>| <span data-ttu-id="9611a-212">Description</span><span class="sxs-lookup"><span data-stu-id="9611a-212">Description</span></span> | <span data-ttu-id="9611a-213">Exemple</span><span class="sxs-lookup"><span data-stu-id="9611a-213">Example</span></span> |
|-------|------|-------------|---------|
| <span data-ttu-id="9611a-214">-v</span><span class="sxs-lookup"><span data-stu-id="9611a-214">-v</span></span>|<span data-ttu-id="9611a-215">--verbose</span><span class="sxs-lookup"><span data-stu-id="9611a-215">--verbose</span></span> | <span data-ttu-id="9611a-216">Affichez la sortie détaillée.</span><span class="sxs-lookup"><span data-stu-id="9611a-216">Show verbose output.</span></span> | <span data-ttu-id="9611a-217">actualisation de dotnet openapi *-v*`https://contoso.com/openapi.json`</span><span class="sxs-lookup"><span data-stu-id="9611a-217">dotnet openapi refresh *-v* `https://contoso.com/openapi.json`</span></span> |
| <span data-ttu-id="9611a-218">-p</span><span class="sxs-lookup"><span data-stu-id="9611a-218">-p</span></span>|<span data-ttu-id="9611a-219">--updateProject</span><span class="sxs-lookup"><span data-stu-id="9611a-219">--updateProject</span></span> | <span data-ttu-id="9611a-220">Projet sur lequel effectuer l’opération.</span><span class="sxs-lookup"><span data-stu-id="9611a-220">The project to operate on.</span></span> | <span data-ttu-id="9611a-221">actualisation de dotnet openapi *--updateProject .\Ref.csproj*`https://contoso.com/openapi.json`</span><span class="sxs-lookup"><span data-stu-id="9611a-221">dotnet openapi refresh *--updateProject .\Ref.csproj* `https://contoso.com/openapi.json`</span></span> |
| <span data-ttu-id="9611a-222">-h</span><span class="sxs-lookup"><span data-stu-id="9611a-222">-h</span></span>|<span data-ttu-id="9611a-223">--aide</span><span class="sxs-lookup"><span data-stu-id="9611a-223">--help</span></span>|<span data-ttu-id="9611a-224">Afficher les informations d’aide</span><span class="sxs-lookup"><span data-stu-id="9611a-224">Show help information</span></span>|<span data-ttu-id="9611a-225">actualisation de dotnet openapi--aide</span><span class="sxs-lookup"><span data-stu-id="9611a-225">dotnet openapi refresh --help</span></span>|

### <a name="arguments"></a><span data-ttu-id="9611a-226">Arguments</span><span class="sxs-lookup"><span data-stu-id="9611a-226">Arguments</span></span>

|  <span data-ttu-id="9611a-227">Argument</span><span class="sxs-lookup"><span data-stu-id="9611a-227">Argument</span></span>  | <span data-ttu-id="9611a-228">Description</span><span class="sxs-lookup"><span data-stu-id="9611a-228">Description</span></span> | <span data-ttu-id="9611a-229">Exemple</span><span class="sxs-lookup"><span data-stu-id="9611a-229">Example</span></span> |
| ------------|-------------|---------|
| <span data-ttu-id="9611a-230">URL source</span><span class="sxs-lookup"><span data-stu-id="9611a-230">source-URL</span></span> | <span data-ttu-id="9611a-231">URL à partir de laquelle actualiser la référence.</span><span class="sxs-lookup"><span data-stu-id="9611a-231">The URL to refresh the reference from.</span></span> | <span data-ttu-id="9611a-232">actualisation de dotnet openapi`https://contoso.com/openapi.json`</span><span class="sxs-lookup"><span data-stu-id="9611a-232">dotnet openapi refresh `https://contoso.com/openapi.json`</span></span> |
