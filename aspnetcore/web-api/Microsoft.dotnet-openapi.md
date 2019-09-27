---
title: Développer des applications ASP.NET Core à l’aide de OpenAPI
author: ryanbrandenburg
description: Montre comment utiliser l’outil « Microsoft. dotnet-openapi » pour ajouter des références aux fichiers OpenAPI.
ms.author: rybrande
ms.date: 09/26/2019
monikerRange: '>= aspnetcore-3.0'
uid: web-api/Microsoft.dotnet-openapi
ms.openlocfilehash: f5eae9e871bc8efc30d500769adb845ff244a90c
ms.sourcegitcommit: e644258c95dd50a82284f107b9bf3becbc43b2b2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/26/2019
ms.locfileid: "71317773"
---
# <a name="develop-aspnet-core-apps-using-openapi-tools"></a><span data-ttu-id="689a9-103">Développer des applications ASP.NET Core à l’aide des outils OpenAPI</span><span class="sxs-lookup"><span data-stu-id="689a9-103">Develop ASP.NET Core apps using OpenAPI tools</span></span>

<span data-ttu-id="689a9-104">Par Ryan Brandenburg</span><span class="sxs-lookup"><span data-stu-id="689a9-104">By Ryan Brandenburg</span></span>

<span data-ttu-id="689a9-105">[Microsoft. dotnet-openapi](https://www.nuget.org/packages/Microsoft.dotnet-openapi) est un [outil Global .net Core](/dotnet/core/tools/global-tools) pour la gestion des références [openapi](https://github.com/OAI/OpenAPI-Specification) dans un projet.</span><span class="sxs-lookup"><span data-stu-id="689a9-105">[Microsoft.dotnet-openapi](https://www.nuget.org/packages/Microsoft.dotnet-openapi) is a [.NET Core Global Tool](/dotnet/core/tools/global-tools) for managing [OpenAPI](https://github.com/OAI/OpenAPI-Specification) references within a project.</span></span>

## <a name="installation"></a><span data-ttu-id="689a9-106">Installation</span><span class="sxs-lookup"><span data-stu-id="689a9-106">Installation</span></span>

<span data-ttu-id="689a9-107">Pour installer `Microsoft.dotnet-openapi`, exécutez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="689a9-107">To install `Microsoft.dotnet-openapi`, run the following command:</span></span>

```dotnetcli
dotnet tool install -g Microsoft.dotnet-openapi
```

## <a name="add"></a><span data-ttu-id="689a9-108">Ajouter</span><span class="sxs-lookup"><span data-stu-id="689a9-108">Add</span></span>

<span data-ttu-id="689a9-109">L’ajout d’une référence openapi à l’aide de l’une des commandes `<OpenApiReference />` de cette page ajoute un élément semblable au suivant au fichier *. csproj* :</span><span class="sxs-lookup"><span data-stu-id="689a9-109">Adding an OpenAPI reference using any of the commands on this page adds an `<OpenApiReference />` element similar to the following to the *.csproj* file:</span></span>

```xml
<OpenApiReference Include="openapi.json" />
```

<span data-ttu-id="689a9-110">La référence précédente est requise pour que l’application appelle le code client généré.</span><span class="sxs-lookup"><span data-stu-id="689a9-110">The preceding reference is required for the app to call the generated client code.</span></span>

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

### <a name="add-file"></a><span data-ttu-id="689a9-111">Ajouter un fichier</span><span class="sxs-lookup"><span data-stu-id="689a9-111">Add File</span></span>

#### <a name="options"></a><span data-ttu-id="689a9-112">Options</span><span class="sxs-lookup"><span data-stu-id="689a9-112">Options</span></span>

| <span data-ttu-id="689a9-113">Option Short</span><span class="sxs-lookup"><span data-stu-id="689a9-113">Short option</span></span>| <span data-ttu-id="689a9-114">Option longue</span><span class="sxs-lookup"><span data-stu-id="689a9-114">Long option</span></span>| <span data-ttu-id="689a9-115">Description</span><span class="sxs-lookup"><span data-stu-id="689a9-115">Description</span></span> | <span data-ttu-id="689a9-116">Exemple</span><span class="sxs-lookup"><span data-stu-id="689a9-116">Example</span></span> |
|-------|------|-------|---------|
| <span data-ttu-id="689a9-117">-v</span><span class="sxs-lookup"><span data-stu-id="689a9-117">-v</span></span>|<span data-ttu-id="689a9-118">--verbose</span><span class="sxs-lookup"><span data-stu-id="689a9-118">--verbose</span></span> | <span data-ttu-id="689a9-119">Affichez la sortie détaillée.</span><span class="sxs-lookup"><span data-stu-id="689a9-119">Show verbose output.</span></span> |<span data-ttu-id="689a9-120">dotnet openapi ajouter *un fichier-v* .\OpenAPI.JSON</span><span class="sxs-lookup"><span data-stu-id="689a9-120">dotnet openapi add file *-v* .\OpenAPI.json</span></span> |
| <span data-ttu-id="689a9-121">-p</span><span class="sxs-lookup"><span data-stu-id="689a9-121">-p</span></span>|<span data-ttu-id="689a9-122">--updateProject</span><span class="sxs-lookup"><span data-stu-id="689a9-122">--updateProject</span></span> | <span data-ttu-id="689a9-123">Projet sur lequel effectuer l’opération.</span><span class="sxs-lookup"><span data-stu-id="689a9-123">The project to operate on.</span></span> |<span data-ttu-id="689a9-124">Ajouter un fichier à dotnet openapi *--updateProject .\Ref.csproj* .\OpenAPI.JSON</span><span class="sxs-lookup"><span data-stu-id="689a9-124">dotnet openapi add file *--updateProject .\Ref.csproj* .\OpenAPI.json</span></span> |
| <span data-ttu-id="689a9-125">-c</span><span class="sxs-lookup"><span data-stu-id="689a9-125">-c</span></span>|<span data-ttu-id="689a9-126">--Code-Generator</span><span class="sxs-lookup"><span data-stu-id="689a9-126">--code-generator</span></span>| <span data-ttu-id="689a9-127">Générateur de code à appliquer à la référence.</span><span class="sxs-lookup"><span data-stu-id="689a9-127">The code generator to apply to the reference.</span></span> <span data-ttu-id="689a9-128">Les options `NSwagCSharp` sont `NSwagTypeScript`et.</span><span class="sxs-lookup"><span data-stu-id="689a9-128">Options are `NSwagCSharp` and `NSwagTypeScript`.</span></span> <span data-ttu-id="689a9-129">Si `--code-generator` n’est pas spécifié `NSwagCSharp`, l’outil prend par défaut la valeur.</span><span class="sxs-lookup"><span data-stu-id="689a9-129">If `--code-generator` is not specified the tooling defaults to `NSwagCSharp`.</span></span>|<span data-ttu-id="689a9-130">dotnet openapi ajouter un fichier .\OpenApi.json--Code-Generator</span><span class="sxs-lookup"><span data-stu-id="689a9-130">dotnet openapi add file .\OpenApi.json --code-generator</span></span>
| <span data-ttu-id="689a9-131">-h</span><span class="sxs-lookup"><span data-stu-id="689a9-131">-h</span></span>|<span data-ttu-id="689a9-132">--aide</span><span class="sxs-lookup"><span data-stu-id="689a9-132">--help</span></span>|<span data-ttu-id="689a9-133">Afficher les informations d’aide</span><span class="sxs-lookup"><span data-stu-id="689a9-133">Show help information</span></span>|<span data-ttu-id="689a9-134">Ajouter un fichier Dotnet openapi--Help</span><span class="sxs-lookup"><span data-stu-id="689a9-134">dotnet openapi add file --help</span></span>|

#### <a name="arguments"></a><span data-ttu-id="689a9-135">Arguments</span><span class="sxs-lookup"><span data-stu-id="689a9-135">Arguments</span></span>

|  <span data-ttu-id="689a9-136">Argument</span><span class="sxs-lookup"><span data-stu-id="689a9-136">Argument</span></span>  | <span data-ttu-id="689a9-137">Description</span><span class="sxs-lookup"><span data-stu-id="689a9-137">Description</span></span> | <span data-ttu-id="689a9-138">Exemple</span><span class="sxs-lookup"><span data-stu-id="689a9-138">Example</span></span> |
|-------------|-------------|---------|
| <span data-ttu-id="689a9-139">fichier source</span><span class="sxs-lookup"><span data-stu-id="689a9-139">source-file</span></span> | <span data-ttu-id="689a9-140">Source à partir de laquelle créer une référence.</span><span class="sxs-lookup"><span data-stu-id="689a9-140">The source to create a reference from.</span></span> <span data-ttu-id="689a9-141">Doit être un fichier OpenAPI.</span><span class="sxs-lookup"><span data-stu-id="689a9-141">Must be an OpenAPI file.</span></span> |<span data-ttu-id="689a9-142">dotnet openapi ajouter un fichier *.\OpenAPI.JSON*</span><span class="sxs-lookup"><span data-stu-id="689a9-142">dotnet openapi add file *.\OpenAPI.json*</span></span> |

### <a name="add-url"></a><span data-ttu-id="689a9-143">Ajouter une URL</span><span class="sxs-lookup"><span data-stu-id="689a9-143">Add URL</span></span>

#### <a name="options"></a><span data-ttu-id="689a9-144">Options</span><span class="sxs-lookup"><span data-stu-id="689a9-144">Options</span></span>

| <span data-ttu-id="689a9-145">Option Short</span><span class="sxs-lookup"><span data-stu-id="689a9-145">Short option</span></span>| <span data-ttu-id="689a9-146">Option longue</span><span class="sxs-lookup"><span data-stu-id="689a9-146">Long option</span></span>| <span data-ttu-id="689a9-147">Description</span><span class="sxs-lookup"><span data-stu-id="689a9-147">Description</span></span> | <span data-ttu-id="689a9-148">Exemple</span><span class="sxs-lookup"><span data-stu-id="689a9-148">Example</span></span> |
|-------|------|-------------|---------|
| <span data-ttu-id="689a9-149">-v</span><span class="sxs-lookup"><span data-stu-id="689a9-149">-v</span></span>|<span data-ttu-id="689a9-150">--verbose</span><span class="sxs-lookup"><span data-stu-id="689a9-150">--verbose</span></span> | <span data-ttu-id="689a9-151">Affichez la sortie détaillée.</span><span class="sxs-lookup"><span data-stu-id="689a9-151">Show verbose output.</span></span> |<span data-ttu-id="689a9-152">Ajouter l’URL dotnet openapi *-v*`https://contoso.com/openapi.json`</span><span class="sxs-lookup"><span data-stu-id="689a9-152">dotnet openapi add url *-v* `https://contoso.com/openapi.json`</span></span> |
| <span data-ttu-id="689a9-153">-p</span><span class="sxs-lookup"><span data-stu-id="689a9-153">-p</span></span>|<span data-ttu-id="689a9-154">--updateProject</span><span class="sxs-lookup"><span data-stu-id="689a9-154">--updateProject</span></span> | <span data-ttu-id="689a9-155">Projet sur lequel effectuer l’opération.</span><span class="sxs-lookup"><span data-stu-id="689a9-155">The project to operate on.</span></span> |<span data-ttu-id="689a9-156">Ajouter une URL dotnet openapi *--updateProject .\Ref.csproj*`https://contoso.com/openapi.json`</span><span class="sxs-lookup"><span data-stu-id="689a9-156">dotnet openapi add url *--updateProject .\Ref.csproj* `https://contoso.com/openapi.json`</span></span> |
| <span data-ttu-id="689a9-157">-o</span><span class="sxs-lookup"><span data-stu-id="689a9-157">-o</span></span>|<span data-ttu-id="689a9-158">--fichier de sortie</span><span class="sxs-lookup"><span data-stu-id="689a9-158">--output-file</span></span> | <span data-ttu-id="689a9-159">Où placer la copie locale du fichier OpenAPI.</span><span class="sxs-lookup"><span data-stu-id="689a9-159">Where to place the local copy of the OpenAPI file.</span></span> |<span data-ttu-id="689a9-160">dotnet openapi ajouter une `https://contoso.com/openapi.json` URL *--sortie-fichier MyClient. JSON*</span><span class="sxs-lookup"><span data-stu-id="689a9-160">dotnet openapi add url `https://contoso.com/openapi.json` *--output-file myclient.json*</span></span> |
| <span data-ttu-id="689a9-161">-c</span><span class="sxs-lookup"><span data-stu-id="689a9-161">-c</span></span>|<span data-ttu-id="689a9-162">--Code-Generator</span><span class="sxs-lookup"><span data-stu-id="689a9-162">--code-generator</span></span>| <span data-ttu-id="689a9-163">Générateur de code à appliquer à la référence.</span><span class="sxs-lookup"><span data-stu-id="689a9-163">The code generator to apply to the reference.</span></span> <span data-ttu-id="689a9-164">Les options `NSwagCSharp` sont `NSwagTypeScript`et.</span><span class="sxs-lookup"><span data-stu-id="689a9-164">Options are `NSwagCSharp` and `NSwagTypeScript`.</span></span> |<span data-ttu-id="689a9-165">dotnet openapi ajouter un fichier .\OpenApi.json--Code-Generator</span><span class="sxs-lookup"><span data-stu-id="689a9-165">dotnet openapi add file .\OpenApi.json --code-generator</span></span>
| <span data-ttu-id="689a9-166">-h</span><span class="sxs-lookup"><span data-stu-id="689a9-166">-h</span></span>|<span data-ttu-id="689a9-167">--aide</span><span class="sxs-lookup"><span data-stu-id="689a9-167">--help</span></span>|<span data-ttu-id="689a9-168">Afficher les informations d’aide</span><span class="sxs-lookup"><span data-stu-id="689a9-168">Show help information</span></span>|<span data-ttu-id="689a9-169">Ajouter une URL dotnet openapi--Help</span><span class="sxs-lookup"><span data-stu-id="689a9-169">dotnet openapi add url --help</span></span>|

#### <a name="arguments"></a><span data-ttu-id="689a9-170">Arguments</span><span class="sxs-lookup"><span data-stu-id="689a9-170">Arguments</span></span>

|  <span data-ttu-id="689a9-171">Argument</span><span class="sxs-lookup"><span data-stu-id="689a9-171">Argument</span></span>  | <span data-ttu-id="689a9-172">Description</span><span class="sxs-lookup"><span data-stu-id="689a9-172">Description</span></span> | <span data-ttu-id="689a9-173">Exemple</span><span class="sxs-lookup"><span data-stu-id="689a9-173">Example</span></span> |
|-------------|-------------|---------|
| <span data-ttu-id="689a9-174">URL source</span><span class="sxs-lookup"><span data-stu-id="689a9-174">source-URL</span></span> | <span data-ttu-id="689a9-175">Source à partir de laquelle créer une référence.</span><span class="sxs-lookup"><span data-stu-id="689a9-175">The source to create a reference from.</span></span> <span data-ttu-id="689a9-176">Doit être une URL.</span><span class="sxs-lookup"><span data-stu-id="689a9-176">Must be a URL.</span></span> |<span data-ttu-id="689a9-177">URL d’ajout de dotnet openapi`https://contoso.com/openapi.json`</span><span class="sxs-lookup"><span data-stu-id="689a9-177">dotnet openapi add url `https://contoso.com/openapi.json`</span></span> |

## <a name="remove"></a><span data-ttu-id="689a9-178">Remove</span><span class="sxs-lookup"><span data-stu-id="689a9-178">Remove</span></span>

<span data-ttu-id="689a9-179">Supprime la référence OpenAPI correspondant au nom de fichier donné du fichier *. csproj* .</span><span class="sxs-lookup"><span data-stu-id="689a9-179">Removes the OpenAPI reference matching the given filename from the *.csproj* file.</span></span> <span data-ttu-id="689a9-180">Lorsque la référence OpenAPI est supprimée, les clients ne sont pas générés.</span><span class="sxs-lookup"><span data-stu-id="689a9-180">When the OpenAPI reference is removed, clients won't be generated.</span></span> <span data-ttu-id="689a9-181">Les fichiers local *. JSON* et *. YAML* sont supprimés.</span><span class="sxs-lookup"><span data-stu-id="689a9-181">Local *.json* and *.yaml* files are deleted.</span></span>

### <a name="options"></a><span data-ttu-id="689a9-182">Options</span><span class="sxs-lookup"><span data-stu-id="689a9-182">Options</span></span>

| <span data-ttu-id="689a9-183">Option Short</span><span class="sxs-lookup"><span data-stu-id="689a9-183">Short option</span></span>| <span data-ttu-id="689a9-184">Option longue</span><span class="sxs-lookup"><span data-stu-id="689a9-184">Long option</span></span>| <span data-ttu-id="689a9-185">Description</span><span class="sxs-lookup"><span data-stu-id="689a9-185">Description</span></span>| <span data-ttu-id="689a9-186">Exemple</span><span class="sxs-lookup"><span data-stu-id="689a9-186">Example</span></span> |
|-------|------|------------|---------|
| <span data-ttu-id="689a9-187">-v</span><span class="sxs-lookup"><span data-stu-id="689a9-187">-v</span></span>|<span data-ttu-id="689a9-188">--verbose</span><span class="sxs-lookup"><span data-stu-id="689a9-188">--verbose</span></span> | <span data-ttu-id="689a9-189">Affichez la sortie détaillée.</span><span class="sxs-lookup"><span data-stu-id="689a9-189">Show verbose output.</span></span> |<span data-ttu-id="689a9-190">openapi de la suppression dotnet *-v*</span><span class="sxs-lookup"><span data-stu-id="689a9-190">dotnet openapi remove *-v*</span></span>|
| <span data-ttu-id="689a9-191">-p</span><span class="sxs-lookup"><span data-stu-id="689a9-191">-p</span></span>|<span data-ttu-id="689a9-192">--updateProject</span><span class="sxs-lookup"><span data-stu-id="689a9-192">--updateProject</span></span> | <span data-ttu-id="689a9-193">Projet sur lequel effectuer l’opération.</span><span class="sxs-lookup"><span data-stu-id="689a9-193">The project to operate on.</span></span> |<span data-ttu-id="689a9-194">dotnet openapi Remove *--updateProject .\Ref.csproj* .\OpenAPI.JSON</span><span class="sxs-lookup"><span data-stu-id="689a9-194">dotnet openapi remove *--updateProject .\Ref.csproj* .\OpenAPI.json</span></span> |
| <span data-ttu-id="689a9-195">-h</span><span class="sxs-lookup"><span data-stu-id="689a9-195">-h</span></span>|<span data-ttu-id="689a9-196">--aide</span><span class="sxs-lookup"><span data-stu-id="689a9-196">--help</span></span>|<span data-ttu-id="689a9-197">Afficher les informations d’aide</span><span class="sxs-lookup"><span data-stu-id="689a9-197">Show help information</span></span>|<span data-ttu-id="689a9-198">openapi de la suppression dotnet--Help</span><span class="sxs-lookup"><span data-stu-id="689a9-198">dotnet openapi remove --help</span></span>|

### <a name="arguments"></a><span data-ttu-id="689a9-199">Arguments</span><span class="sxs-lookup"><span data-stu-id="689a9-199">Arguments</span></span>

|  <span data-ttu-id="689a9-200">Argument</span><span class="sxs-lookup"><span data-stu-id="689a9-200">Argument</span></span>  | <span data-ttu-id="689a9-201">Description</span><span class="sxs-lookup"><span data-stu-id="689a9-201">Description</span></span>| <span data-ttu-id="689a9-202">Exemple</span><span class="sxs-lookup"><span data-stu-id="689a9-202">Example</span></span> |
| ------------|------------|---------|
| <span data-ttu-id="689a9-203">fichier source</span><span class="sxs-lookup"><span data-stu-id="689a9-203">source-file</span></span> | <span data-ttu-id="689a9-204">Source à laquelle supprimer la référence.</span><span class="sxs-lookup"><span data-stu-id="689a9-204">The source to remove the reference to.</span></span> |<span data-ttu-id="689a9-205">dotnet openapi supprimer *.\OpenAPI.JSON*</span><span class="sxs-lookup"><span data-stu-id="689a9-205">dotnet openapi remove *.\OpenAPI.json*</span></span> |

## <a name="refresh"></a><span data-ttu-id="689a9-206">Actualiser</span><span class="sxs-lookup"><span data-stu-id="689a9-206">Refresh</span></span>

<span data-ttu-id="689a9-207">Actualise la version locale d’un fichier qui a été téléchargé à l’aide du contenu le plus récent à partir de l’URL de téléchargement.</span><span class="sxs-lookup"><span data-stu-id="689a9-207">Refreshes the local version of a file that was downloaded using the latest content from the download URL.</span></span>

### <a name="options"></a><span data-ttu-id="689a9-208">Options</span><span class="sxs-lookup"><span data-stu-id="689a9-208">Options</span></span>

| <span data-ttu-id="689a9-209">Option Short</span><span class="sxs-lookup"><span data-stu-id="689a9-209">Short option</span></span>| <span data-ttu-id="689a9-210">Option longue</span><span class="sxs-lookup"><span data-stu-id="689a9-210">Long option</span></span>| <span data-ttu-id="689a9-211">Description</span><span class="sxs-lookup"><span data-stu-id="689a9-211">Description</span></span> | <span data-ttu-id="689a9-212">Exemple</span><span class="sxs-lookup"><span data-stu-id="689a9-212">Example</span></span> |
|-------|------|-------------|---------|
| <span data-ttu-id="689a9-213">-v</span><span class="sxs-lookup"><span data-stu-id="689a9-213">-v</span></span>|<span data-ttu-id="689a9-214">--verbose</span><span class="sxs-lookup"><span data-stu-id="689a9-214">--verbose</span></span> | <span data-ttu-id="689a9-215">Affichez la sortie détaillée.</span><span class="sxs-lookup"><span data-stu-id="689a9-215">Show verbose output.</span></span> | <span data-ttu-id="689a9-216">actualisation de dotnet openapi *-v*`https://contoso.com/openapi.json`</span><span class="sxs-lookup"><span data-stu-id="689a9-216">dotnet openapi refresh *-v* `https://contoso.com/openapi.json`</span></span> |
| <span data-ttu-id="689a9-217">-p</span><span class="sxs-lookup"><span data-stu-id="689a9-217">-p</span></span>|<span data-ttu-id="689a9-218">--updateProject</span><span class="sxs-lookup"><span data-stu-id="689a9-218">--updateProject</span></span> | <span data-ttu-id="689a9-219">Projet sur lequel effectuer l’opération.</span><span class="sxs-lookup"><span data-stu-id="689a9-219">The project to operate on.</span></span> | <span data-ttu-id="689a9-220">actualisation de dotnet openapi *--updateProject .\Ref.csproj*`https://contoso.com/openapi.json`</span><span class="sxs-lookup"><span data-stu-id="689a9-220">dotnet openapi refresh *--updateProject .\Ref.csproj* `https://contoso.com/openapi.json`</span></span> |
| <span data-ttu-id="689a9-221">-h</span><span class="sxs-lookup"><span data-stu-id="689a9-221">-h</span></span>|<span data-ttu-id="689a9-222">--aide</span><span class="sxs-lookup"><span data-stu-id="689a9-222">--help</span></span>|<span data-ttu-id="689a9-223">Afficher les informations d’aide</span><span class="sxs-lookup"><span data-stu-id="689a9-223">Show help information</span></span>|<span data-ttu-id="689a9-224">actualisation de dotnet openapi--aide</span><span class="sxs-lookup"><span data-stu-id="689a9-224">dotnet openapi refresh --help</span></span>|

### <a name="arguments"></a><span data-ttu-id="689a9-225">Arguments</span><span class="sxs-lookup"><span data-stu-id="689a9-225">Arguments</span></span>

|  <span data-ttu-id="689a9-226">Argument</span><span class="sxs-lookup"><span data-stu-id="689a9-226">Argument</span></span>  | <span data-ttu-id="689a9-227">Description</span><span class="sxs-lookup"><span data-stu-id="689a9-227">Description</span></span> | <span data-ttu-id="689a9-228">Exemple</span><span class="sxs-lookup"><span data-stu-id="689a9-228">Example</span></span> |
| ------------|-------------|---------|
| <span data-ttu-id="689a9-229">URL source</span><span class="sxs-lookup"><span data-stu-id="689a9-229">source-URL</span></span> | <span data-ttu-id="689a9-230">URL à partir de laquelle actualiser la référence.</span><span class="sxs-lookup"><span data-stu-id="689a9-230">The URL to refresh the reference from.</span></span> | <span data-ttu-id="689a9-231">actualisation de dotnet openapi`https://contoso.com/openapi.json`</span><span class="sxs-lookup"><span data-stu-id="689a9-231">dotnet openapi refresh `https://contoso.com/openapi.json`</span></span> |
