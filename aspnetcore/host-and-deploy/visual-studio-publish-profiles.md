---
title: Profils de publication Visual Studio pour le déploiement d’applications ASP.NET Core
author: rick-anderson
description: Apprendre à créer des profils de publication dans Visual Studio et les utiliser pour la gestion des déploiements d’applications ASP.NET Core pour différentes cibles.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 04/10/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/visual-studio-publish-profiles
ms.openlocfilehash: 3dc858793cd4ddb2630d05a5084f4b7caeaa30eb
ms.sourcegitcommit: c79fd3592f444d58e17518914f8873d0a11219c0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/18/2018
---
# <a name="visual-studio-publish-profiles-for-aspnet-core-app-deployment"></a><span data-ttu-id="0cd8f-103">Profils de publication Visual Studio pour le déploiement d’applications ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="0cd8f-103">Visual Studio publish profiles for ASP.NET Core app deployment</span></span>

<span data-ttu-id="0cd8f-104">De [Sayed Ibrahim Hashimi](https://github.com/sayedihashimi) et [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="0cd8f-104">By [Sayed Ibrahim Hashimi](https://github.com/sayedihashimi) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="0cd8f-105">Ce document se concentre sur l’utilisation de Visual Studio 2017 pour créer et utiliser des profils de publication.</span><span class="sxs-lookup"><span data-stu-id="0cd8f-105">This document focuses on using Visual Studio 2017 to create and use publish profiles.</span></span> <span data-ttu-id="0cd8f-106">Les profils de publication créées avec Visual Studio peuvent être exécutés à partir de MSBuild et Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="0cd8f-106">The publish profiles created with Visual Studio can be run from MSBuild and Visual Studio 2017.</span></span> <span data-ttu-id="0cd8f-107">Consultez [Publier une application web ASP.NET Core sur Azure App Service à l’aide de Visual Studio](xref:tutorials/publish-to-azure-webapp-using-vs) pour obtenir des instructions de publication sur Azure.</span><span class="sxs-lookup"><span data-stu-id="0cd8f-107">See [Publish an ASP.NET Core web app to Azure App Service using Visual Studio](xref:tutorials/publish-to-azure-webapp-using-vs) for instructions on publishing to Azure.</span></span>

<span data-ttu-id="0cd8f-108">Le fichier projet suivant a été créé avec la commande `dotnet new mvc`:</span><span class="sxs-lookup"><span data-stu-id="0cd8f-108">The following project file was created with the command `dotnet new mvc`:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="0cd8f-109">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="0cd8f-109">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```xml
<Project Sdk="Microsoft.NET.Sdk.Web">

  <PropertyGroup>
    <TargetFramework>netcoreapp2.0</TargetFramework>
  </PropertyGroup>

  <ItemGroup>
    <PackageReference Include="Microsoft.AspNetCore.All" Version="2.0.0" />
  </ItemGroup>

  <ItemGroup>
    <DotNetCliToolReference Include="Microsoft.VisualStudio.Web.CodeGeneration.Tools" Version="2.0.0" />
  </ItemGroup>

</Project>
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="0cd8f-110">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="0cd8f-110">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```xml
<Project Sdk="Microsoft.NET.Sdk.Web">

  <PropertyGroup>
    <TargetFramework>netcoreapp1.1</TargetFramework>
  </PropertyGroup>

  <ItemGroup>
    <PackageReference Include="Microsoft.AspNetCore" Version="1.1.5" />
    <PackageReference Include="Microsoft.AspNetCore.Mvc" Version="1.1.6" />
    <PackageReference Include="Microsoft.AspNetCore.StaticFiles" Version="1.1.3" />
  </ItemGroup>

</Project>
```

---

<span data-ttu-id="0cd8f-111">Le `<Project>` l’élément `Sdk` attribut effectue les tâches suivantes :</span><span class="sxs-lookup"><span data-stu-id="0cd8f-111">The `<Project>` element's `Sdk` attribute accomplishes the following tasks:</span></span>

* <span data-ttu-id="0cd8f-112">Importe le fichier de propriétés de *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.Props* au début.</span><span class="sxs-lookup"><span data-stu-id="0cd8f-112">Imports the properties file from *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.Props* at the beginning.</span></span>
* <span data-ttu-id="0cd8f-113">Importe le fichier des cibles à partir de *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.targets* à la fin.</span><span class="sxs-lookup"><span data-stu-id="0cd8f-113">Imports the targets file from *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.targets* at the end.</span></span>

<span data-ttu-id="0cd8f-114">L’emplacement par défaut de `MSBuildSDKsPath` (avec Visual Studio 2017 Enterprise) est le dossier *%programfiles(x86)%\Microsoft Visual Studio\2017\Enterprise\MSBuild\Sdks*.</span><span class="sxs-lookup"><span data-stu-id="0cd8f-114">The default location for `MSBuildSDKsPath` (with Visual Studio 2017 Enterprise) is the *%programfiles(x86)%\Microsoft Visual Studio\2017\Enterprise\MSBuild\Sdks* folder.</span></span>

<span data-ttu-id="0cd8f-115">Le `Microsoft.NET.Sdk.Web` dépend de kit de développement logiciel :</span><span class="sxs-lookup"><span data-stu-id="0cd8f-115">The `Microsoft.NET.Sdk.Web` SDK depends on:</span></span>

* <span data-ttu-id="0cd8f-116">*Microsoft.NET.Sdk.Web.ProjectSystem*</span><span class="sxs-lookup"><span data-stu-id="0cd8f-116">*Microsoft.NET.Sdk.Web.ProjectSystem*</span></span>
* <span data-ttu-id="0cd8f-117">*Microsoft.NET.Sdk.Publish*</span><span class="sxs-lookup"><span data-stu-id="0cd8f-117">*Microsoft.NET.Sdk.Publish*</span></span>

<span data-ttu-id="0cd8f-118">Ce qui entraîne l'import des propriétés et des cibles suivantes :</span><span class="sxs-lookup"><span data-stu-id="0cd8f-118">Which causes the following properties and targets to be imported:</span></span>

* <span data-ttu-id="0cd8f-119">*$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web.ProjectSystem\Sdk\Sdk.Props*</span><span class="sxs-lookup"><span data-stu-id="0cd8f-119">*$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web.ProjectSystem\Sdk\Sdk.Props*</span></span>
* <span data-ttu-id="0cd8f-120">*$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web.ProjectSystem\Sdk\Sdk.targets*</span><span class="sxs-lookup"><span data-stu-id="0cd8f-120">*$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web.ProjectSystem\Sdk\Sdk.targets*</span></span>
* <span data-ttu-id="0cd8f-121">*$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Publish\Sdk\Sdk.Props*</span><span class="sxs-lookup"><span data-stu-id="0cd8f-121">*$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Publish\Sdk\Sdk.Props*</span></span>
* <span data-ttu-id="0cd8f-122">*$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Publish\Sdk\Sdk.targets*</span><span class="sxs-lookup"><span data-stu-id="0cd8f-122">*$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Publish\Sdk\Sdk.targets*</span></span>

<span data-ttu-id="0cd8f-123">Les cibles de publication importent le bon jeu de cibles en fonction de la méthode de publication utilisée.</span><span class="sxs-lookup"><span data-stu-id="0cd8f-123">Publish targets import the right set of targets based on the publish method used.</span></span>

<span data-ttu-id="0cd8f-124">Lorsque MSBuild ou Visual Studio charge un projet, les actions de niveau supérieur suivantes se produisent :</span><span class="sxs-lookup"><span data-stu-id="0cd8f-124">When MSBuild or Visual Studio loads a project, the following high-level actions occur:</span></span>

* <span data-ttu-id="0cd8f-125">Génération du projet</span><span class="sxs-lookup"><span data-stu-id="0cd8f-125">Build project</span></span>
* <span data-ttu-id="0cd8f-126">Traitement des fichiers à publier</span><span class="sxs-lookup"><span data-stu-id="0cd8f-126">Compute files to publish</span></span>
* <span data-ttu-id="0cd8f-127">Publication des fichiers sur la destination</span><span class="sxs-lookup"><span data-stu-id="0cd8f-127">Publish files to destination</span></span>

## <a name="compute-project-items"></a><span data-ttu-id="0cd8f-128">Traitement des éléments du projet</span><span class="sxs-lookup"><span data-stu-id="0cd8f-128">Compute project items</span></span>

<span data-ttu-id="0cd8f-129">Quand le projet est chargé, les éléments du projet (fichiers) sont traités.</span><span class="sxs-lookup"><span data-stu-id="0cd8f-129">When the project is loaded, the project items (files) are computed.</span></span> <span data-ttu-id="0cd8f-130">L’attribut `item type` détermine comment le fichier est traité.</span><span class="sxs-lookup"><span data-stu-id="0cd8f-130">The `item type` attribute determines how the file is processed.</span></span> <span data-ttu-id="0cd8f-131">Par défaut, les fichiers *.cs* sont inclus dans la liste d’éléments `Compile`.</span><span class="sxs-lookup"><span data-stu-id="0cd8f-131">By default, *.cs* files are included in the `Compile` item list.</span></span> <span data-ttu-id="0cd8f-132">Les fichiers dans la liste d’éléments `Compile` sont compilés.</span><span class="sxs-lookup"><span data-stu-id="0cd8f-132">Files in the `Compile` item list are compiled.</span></span>

<span data-ttu-id="0cd8f-133">Le `Content` liste d’éléments contient des fichiers qui sont publiés en plus les sorties de génération.</span><span class="sxs-lookup"><span data-stu-id="0cd8f-133">The `Content` item list contains files that are published in addition to the build outputs.</span></span> <span data-ttu-id="0cd8f-134">Par défaut, les fichiers correspondant au modèle `wwwroot/**` sont inclus dans le `Content` élément.</span><span class="sxs-lookup"><span data-stu-id="0cd8f-134">By default, files matching the pattern `wwwroot/**` are included in the `Content` item.</span></span> <span data-ttu-id="0cd8f-135">Le `wwwroot/\*\*` [motif de la globalisation](https://gruntjs.com/configuring-tasks#globbing-patterns) correspond à tous les fichiers dans le *wwwroot* dossier **et** sous-dossiers.</span><span class="sxs-lookup"><span data-stu-id="0cd8f-135">The `wwwroot/\*\*` [globbing pattern](https://gruntjs.com/configuring-tasks#globbing-patterns) matches all files in the *wwwroot* folder **and** subfolders.</span></span> <span data-ttu-id="0cd8f-136">Pour ajouter explicitement un fichier à la liste de publication, ajoutez le fichier directement dans le *.csproj* de fichiers comme dans [des fichiers Include](#include-files).</span><span class="sxs-lookup"><span data-stu-id="0cd8f-136">To explicitly add a file to the publish list, add the file directly in the *.csproj* file as shown in [Include Files](#include-files).</span></span>

<span data-ttu-id="0cd8f-137">Lorsque vous sélectionnez le bouton **publier** dans Visual Studio, ou lors de la publication à partir de la ligne de commande :</span><span class="sxs-lookup"><span data-stu-id="0cd8f-137">When selecting the **Publish** button in Visual Studio or when publishing from the command line:</span></span>

* <span data-ttu-id="0cd8f-138">Les éléments/propriétés sont traités (il s’agit des fichiers nécessaires à la génération).</span><span class="sxs-lookup"><span data-stu-id="0cd8f-138">The properties/items are computed (the files that are needed to build).</span></span>
* <span data-ttu-id="0cd8f-139">**Visual Studio seulement**: les packages NuGet sont restaurés.</span><span class="sxs-lookup"><span data-stu-id="0cd8f-139">**Visual Studio only**: NuGet packages are restored.</span></span> <span data-ttu-id="0cd8f-140">(La restauration doit être explicite par l’utilisateur sur l’interface en ligne de commande.)</span><span class="sxs-lookup"><span data-stu-id="0cd8f-140">(Restore needs to be explicit by the user on the CLI.)</span></span>
* <span data-ttu-id="0cd8f-141">Le projet est généré.</span><span class="sxs-lookup"><span data-stu-id="0cd8f-141">The project builds.</span></span>
* <span data-ttu-id="0cd8f-142">Les éléments de publication sont traités (il s’agit des fichiers nécessaires à la publication).</span><span class="sxs-lookup"><span data-stu-id="0cd8f-142">The publish items are computed (the files that are needed to publish).</span></span>
* <span data-ttu-id="0cd8f-143">Le projet est publié (les fichiers calculées sont copiés vers la destination de publication).</span><span class="sxs-lookup"><span data-stu-id="0cd8f-143">The project is published (the computed files are copied to the publish destination).</span></span>

<span data-ttu-id="0cd8f-144">Lorsque un projet ASP.NET Core référence `Microsoft.NET.Sdk.Web` dans le fichier projet, un fichier *app_offline.htm* est placé à la racine du répertoire de l'application web.</span><span class="sxs-lookup"><span data-stu-id="0cd8f-144">When an ASP.NET Core project references `Microsoft.NET.Sdk.Web` in the project file, an *app_offline.htm* file is placed at the root of the web app directory.</span></span> <span data-ttu-id="0cd8f-145">Quand le fichier est présent, le module ASP.NET Core ferme l’application normalement et sert le fichier *app_offline.htm* pendant le déploiement.</span><span class="sxs-lookup"><span data-stu-id="0cd8f-145">When the file is present, the ASP.NET Core Module gracefully shuts down the app and serves the *app_offline.htm* file during the deployment.</span></span> <span data-ttu-id="0cd8f-146">Pour plus d’informations, consultez les [Informations de référence sur la configuration du module ASP.NET Core](xref:host-and-deploy/aspnet-core-module#app_offlinehtm).</span><span class="sxs-lookup"><span data-stu-id="0cd8f-146">For more information, see the [ASP.NET Core Module configuration reference](xref:host-and-deploy/aspnet-core-module#app_offlinehtm).</span></span>

## <a name="basic-command-line-publishing"></a><span data-ttu-id="0cd8f-147">Publication en ligne de commande de base</span><span class="sxs-lookup"><span data-stu-id="0cd8f-147">Basic command-line publishing</span></span>

<span data-ttu-id="0cd8f-148">Publication en ligne de commande fonctionne sur toutes les plateformes prises en charge de .NET Core et ne nécessite pas Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="0cd8f-148">Command-line publishing works on all .NET Core-supported platforms and doesn't require Visual Studio.</span></span> <span data-ttu-id="0cd8f-149">Dans les exemples ci-dessous, la [dotnet publier](/dotnet/core/tools/dotnet-publish) commande est exécutée à partir du répertoire de projet (qui contient le *.csproj* fichier).</span><span class="sxs-lookup"><span data-stu-id="0cd8f-149">In the samples below, the [dotnet publish](/dotnet/core/tools/dotnet-publish) command is run from the project directory (which contains the *.csproj* file).</span></span> <span data-ttu-id="0cd8f-150">S’il n’est pas dans le dossier du projet, passez explicitement le chemin du fichier projet.</span><span class="sxs-lookup"><span data-stu-id="0cd8f-150">If not in the project folder, explicitly pass in the project file path.</span></span> <span data-ttu-id="0cd8f-151">Par exemple :</span><span class="sxs-lookup"><span data-stu-id="0cd8f-151">For example:</span></span>

```console
dotnet publish C:\Webs\Web1
```

<span data-ttu-id="0cd8f-152">Exécutez les commandes suivantes pour créer et publier une application web :</span><span class="sxs-lookup"><span data-stu-id="0cd8f-152">Run the following commands to create and publish a web app:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="0cd8f-153">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="0cd8f-153">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```console
dotnet new mvc
dotnet publish
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="0cd8f-154">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="0cd8f-154">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```console
dotnet new mvc
dotnet restore
dotnet publish
```

---

<span data-ttu-id="0cd8f-155">Le [dotnet publier](/dotnet/core/tools/dotnet-publish) commande produit un résultat semblable au suivant :</span><span class="sxs-lookup"><span data-stu-id="0cd8f-155">The [dotnet publish](/dotnet/core/tools/dotnet-publish) command produces output similar to the following:</span></span>

```console
C:\Webs\Web1>dotnet publish
Microsoft (R) Build Engine version 15.3.409.57025 for .NET Core
Copyright (C) Microsoft Corporation. All rights reserved.

  Web1 -> C:\Webs\Web1\bin\Debug\netcoreapp2.0\Web1.dll
  Web1 -> C:\Webs\Web1\bin\Debug\netcoreapp2.0\publish\
```

<span data-ttu-id="0cd8f-156">Le dossier de publication par défaut est `bin\$(Configuration)\netcoreapp<version>\publish`.</span><span class="sxs-lookup"><span data-stu-id="0cd8f-156">The default publish folder is `bin\$(Configuration)\netcoreapp<version>\publish`.</span></span> <span data-ttu-id="0cd8f-157">La valeur par défaut pour `$(Configuration)` est *déboguer*.</span><span class="sxs-lookup"><span data-stu-id="0cd8f-157">The default for `$(Configuration)` is *Debug*.</span></span> <span data-ttu-id="0cd8f-158">Dans l’exemple précédent, le `<TargetFramework>` est `netcoreapp2.0`.</span><span class="sxs-lookup"><span data-stu-id="0cd8f-158">In the preceding sample, the `<TargetFramework>` is `netcoreapp2.0`.</span></span>

<span data-ttu-id="0cd8f-159">`dotnet publish -h` affiche des informations d’aide relatives à la publication.</span><span class="sxs-lookup"><span data-stu-id="0cd8f-159">`dotnet publish -h` displays help information for publish.</span></span>

<span data-ttu-id="0cd8f-160">La commande suivante spécifie une build `Release` et le répertoire de publication :</span><span class="sxs-lookup"><span data-stu-id="0cd8f-160">The following command specifies a `Release` build and the publishing directory:</span></span>

```console
dotnet publish -c Release -o C:\MyWebs\test
```

<span data-ttu-id="0cd8f-161">Le [dotnet publier](/dotnet/core/tools/dotnet-publish) commande appels MSBuild, ce qui permet d’appeler le `Publish` cible.</span><span class="sxs-lookup"><span data-stu-id="0cd8f-161">The [dotnet publish](/dotnet/core/tools/dotnet-publish) command calls MSBuild, which invokes the `Publish` target.</span></span> <span data-ttu-id="0cd8f-162">Les paramètres passés à `dotnet publish` sont passés à MSBuild.</span><span class="sxs-lookup"><span data-stu-id="0cd8f-162">Any parameters passed to `dotnet publish` are passed to MSBuild.</span></span> <span data-ttu-id="0cd8f-163">Le paramètre `-c` est mappé à la propriété MSBuild `Configuration`.</span><span class="sxs-lookup"><span data-stu-id="0cd8f-163">The `-c` parameter maps to the `Configuration` MSBuild property.</span></span> <span data-ttu-id="0cd8f-164">Le paramètre `-o` est mappé à `OutputPath`.</span><span class="sxs-lookup"><span data-stu-id="0cd8f-164">The `-o` parameter maps to `OutputPath`.</span></span>

<span data-ttu-id="0cd8f-165">Les propriétés MSBuild peuvent être passées à l’aide d’un des formats suivants :</span><span class="sxs-lookup"><span data-stu-id="0cd8f-165">MSBuild properties can be passed using either of the following formats:</span></span>

* ` p:<NAME>=<VALUE>`
* `/p:<NAME>=<VALUE>`

<span data-ttu-id="0cd8f-166">La commande suivante publie une build `Release` sur un partage réseau :</span><span class="sxs-lookup"><span data-stu-id="0cd8f-166">The following command publishes a `Release` build to a network share:</span></span>

`dotnet publish -c Release /p:PublishDir=//r8/release/AdminWeb`

<span data-ttu-id="0cd8f-167">Le partage réseau est spécifié avec des barres obliques (*//r8/*) et fonctionne sur toutes les plateformes .NET Core prises en charge.</span><span class="sxs-lookup"><span data-stu-id="0cd8f-167">The network share is specified with forward slashes (*//r8/*) and works on all .NET Core supported platforms.</span></span>

<span data-ttu-id="0cd8f-168">Vérifiez que l’application publiée pour le déploiement n’est pas en cours d’exécution.</span><span class="sxs-lookup"><span data-stu-id="0cd8f-168">Confirm that the published app for deployment isn't running.</span></span> <span data-ttu-id="0cd8f-169">Les fichiers dans le dossier *publish* sont verrouillés quand l’application est en cours d’exécution.</span><span class="sxs-lookup"><span data-stu-id="0cd8f-169">Files in the *publish* folder are locked when the app is running.</span></span> <span data-ttu-id="0cd8f-170">Le déploiement ne peut pas se produire car les fichiers verrouillés ne peuvent pas être copiés.</span><span class="sxs-lookup"><span data-stu-id="0cd8f-170">Deployment can't occur because locked files can't be copied.</span></span>

## <a name="publish-profiles"></a><span data-ttu-id="0cd8f-171">Profils de publication</span><span class="sxs-lookup"><span data-stu-id="0cd8f-171">Publish profiles</span></span>

<span data-ttu-id="0cd8f-172">Cette section utilise Visual Studio 2017 pour créer un profil de publication.</span><span class="sxs-lookup"><span data-stu-id="0cd8f-172">This section uses Visual Studio 2017 to create a publishing profile.</span></span> <span data-ttu-id="0cd8f-173">Une fois créé, la publication à partir de Visual Studio ou de la ligne de commande est disponible.</span><span class="sxs-lookup"><span data-stu-id="0cd8f-173">Once created, publishing from Visual Studio or the command line is available.</span></span>

<span data-ttu-id="0cd8f-174">Publier les profils peuvent simplifier le processus de publication, et n’importe quel nombre de profils peut exister.</span><span class="sxs-lookup"><span data-stu-id="0cd8f-174">Publish profiles can simplify the publishing process, and any number of profiles can exist.</span></span> <span data-ttu-id="0cd8f-175">Créer un profil de publication dans Visual Studio en choisissant l’un des chemins suivants :</span><span class="sxs-lookup"><span data-stu-id="0cd8f-175">Create a publish profile in Visual Studio by choosing one of the following paths:</span></span>

* <span data-ttu-id="0cd8f-176">Cliquez sur le projet dans l’Explorateur de solutions et sélectionnez **publier**.</span><span class="sxs-lookup"><span data-stu-id="0cd8f-176">Right-click the project in Solution Explorer and select **Publish**.</span></span>
* <span data-ttu-id="0cd8f-177">Sélectionnez **publier &lt;project_name&gt;**  à partir de la **générer** menu.</span><span class="sxs-lookup"><span data-stu-id="0cd8f-177">Select **Publish &lt;project_name&gt;** from the **Build** menu.</span></span>

<span data-ttu-id="0cd8f-178">Le **publier** onglet de la page de capacités de l’application s’affiche.</span><span class="sxs-lookup"><span data-stu-id="0cd8f-178">The **Publish** tab of the app capacities page is displayed.</span></span> <span data-ttu-id="0cd8f-179">Si le projet ne dispose pas d’un profil de publication, la page suivante s’affiche :</span><span class="sxs-lookup"><span data-stu-id="0cd8f-179">If the project lacks a publish profile, the following page is displayed:</span></span>

![L’onglet de la publication de la page de capacités de l’application](visual-studio-publish-profiles/_static/az.png)

<span data-ttu-id="0cd8f-181">Lorsque **dossier** est sélectionnée, spécifiez un chemin d’accès de dossier pour stocker les ressources publiées.</span><span class="sxs-lookup"><span data-stu-id="0cd8f-181">When **Folder** is selected, specify a folder path to store the published assets.</span></span> <span data-ttu-id="0cd8f-182">Le dossier par défaut est *bin\Release\PublishOutput*.</span><span class="sxs-lookup"><span data-stu-id="0cd8f-182">The default folder is *bin\Release\PublishOutput*.</span></span> <span data-ttu-id="0cd8f-183">Cliquez sur le **créer un profil** bouton Terminer.</span><span class="sxs-lookup"><span data-stu-id="0cd8f-183">Click the **Create Profile** button to finish.</span></span>

<span data-ttu-id="0cd8f-184">Une fois qu’un profil de publication est créé, le **publier** onglet modifications.</span><span class="sxs-lookup"><span data-stu-id="0cd8f-184">Once a publish profile is created, the **Publish** tab changes.</span></span> <span data-ttu-id="0cd8f-185">Le profil nouvellement créé s’affiche dans une liste déroulante.</span><span class="sxs-lookup"><span data-stu-id="0cd8f-185">The newly created profile appears in a drop-down list.</span></span> <span data-ttu-id="0cd8f-186">Cliquez sur **créer nouveau profil** pour créer un autre profil.</span><span class="sxs-lookup"><span data-stu-id="0cd8f-186">Click **Create new profile** to create another new profile.</span></span>

![L’onglet de la publication de la page application capacités FolderProfile](visual-studio-publish-profiles/_static/create_new.png)

<span data-ttu-id="0cd8f-188">L’Assistant Publication prend en charge les cibles de publication suivantes :</span><span class="sxs-lookup"><span data-stu-id="0cd8f-188">The Publish wizard supports the following publish targets:</span></span>

* <span data-ttu-id="0cd8f-189">Azure App Service</span><span class="sxs-lookup"><span data-stu-id="0cd8f-189">Azure App Service</span></span>
* <span data-ttu-id="0cd8f-190">Machines virtuelles Azure</span><span class="sxs-lookup"><span data-stu-id="0cd8f-190">Azure Virtual Machines</span></span>
* <span data-ttu-id="0cd8f-191">IIS, FTP, etc. (pour n’importe quel serveur web)</span><span class="sxs-lookup"><span data-stu-id="0cd8f-191">IIS, FTP, etc. (for any web server)</span></span>
* <span data-ttu-id="0cd8f-192">Dossier</span><span class="sxs-lookup"><span data-stu-id="0cd8f-192">Folder</span></span>
* <span data-ttu-id="0cd8f-193">Importer un profil</span><span class="sxs-lookup"><span data-stu-id="0cd8f-193">Import Profile</span></span>

<span data-ttu-id="0cd8f-194">Pour plus d’informations, consultez [les options de publication sont adaptées à mes besoins](/visualstudio/ide/not-in-toc/web-publish-options).</span><span class="sxs-lookup"><span data-stu-id="0cd8f-194">For more information, see [What publishing options are right for me](/visualstudio/ide/not-in-toc/web-publish-options).</span></span>

<span data-ttu-id="0cd8f-195">Lors de la création d’un profil de publication avec Visual Studio, un *propriétés/PublishProfiles/&lt;profile_name&gt;.pubxml* fichier MSBuild est créé.</span><span class="sxs-lookup"><span data-stu-id="0cd8f-195">When creating a publish profile with Visual Studio, a *Properties/PublishProfiles/&lt;profile_name&gt;.pubxml* MSBuild file is created.</span></span> <span data-ttu-id="0cd8f-196">Le *.pubxml* est un fichier de MSBuild et contient les paramètres de configuration de publication.</span><span class="sxs-lookup"><span data-stu-id="0cd8f-196">The *.pubxml* file is a MSBuild file and contains publish configuration settings.</span></span> <span data-ttu-id="0cd8f-197">Ce fichier peut être modifié pour personnaliser la génération et le processus de publication.</span><span class="sxs-lookup"><span data-stu-id="0cd8f-197">This file can be changed to customize the build and publish process.</span></span> <span data-ttu-id="0cd8f-198">Ce fichier est lu par le processus de publication.</span><span class="sxs-lookup"><span data-stu-id="0cd8f-198">This file is read by the publishing process.</span></span> <span data-ttu-id="0cd8f-199">`<LastUsedBuildConfiguration>` est spécial, car elle est une propriété globale et ne doivent pas être dans n’importe quel fichier qui est importé dans la build.</span><span class="sxs-lookup"><span data-stu-id="0cd8f-199">`<LastUsedBuildConfiguration>` is special because it's a global property and shouldn't be in any file that's imported in the build.</span></span> <span data-ttu-id="0cd8f-200">Consultez [MSBuild : comment définir la propriété de configuration](http://sedodream.com/2012/10/27/MSBuildHowToSetTheConfigurationProperty.aspx) pour plus d’informations.</span><span class="sxs-lookup"><span data-stu-id="0cd8f-200">See [MSBuild: how to set the configuration property](http://sedodream.com/2012/10/27/MSBuildHowToSetTheConfigurationProperty.aspx) for more information.</span></span>

<span data-ttu-id="0cd8f-201">Lors de la publication vers une cible Azure, le *.pubxml* fichier contient l’identificateur de votre abonnement Azure.</span><span class="sxs-lookup"><span data-stu-id="0cd8f-201">When publishing to an Azure target, the *.pubxml* file contains your Azure subscription identifier.</span></span> <span data-ttu-id="0cd8f-202">Avec ce type de cible, il est déconseillé d’ajouter ce fichier au contrôle de code source.</span><span class="sxs-lookup"><span data-stu-id="0cd8f-202">With that target type, adding this file to source control is discouraged.</span></span> <span data-ttu-id="0cd8f-203">Lors de la publication vers une cible non-Azure, il est recommandé d’archiver le *.pubxml* fichier.</span><span class="sxs-lookup"><span data-stu-id="0cd8f-203">When publishing to a non-Azure target, it's safe to check in the *.pubxml* file.</span></span>

<span data-ttu-id="0cd8f-204">Les informations sensibles (telles que le mot de passe de publication) sont chiffrées sur un par niveau de l’utilisateur/ordinateur.</span><span class="sxs-lookup"><span data-stu-id="0cd8f-204">Sensitive information (like the publish password) is encrypted on a per user/machine level.</span></span> <span data-ttu-id="0cd8f-205">Il est stocké dans le *propriétés/PublishProfiles/&lt;profile_name&gt;. pubxml.user* fichier.</span><span class="sxs-lookup"><span data-stu-id="0cd8f-205">It's stored in the *Properties/PublishProfiles/&lt;profile_name&gt;.pubxml.user* file.</span></span> <span data-ttu-id="0cd8f-206">Étant donné que ce fichier peut stocker des informations sensibles, il ne doit pas être archivé dans le contrôle de code source.</span><span class="sxs-lookup"><span data-stu-id="0cd8f-206">Because this file can store sensitive information, it shouldn't be checked into source control.</span></span>

<span data-ttu-id="0cd8f-207">Pour une vue d’ensemble de la publication d’une application web sur ASP.NET Core, consultez [hôte et déployer](xref:host-and-deploy/index).</span><span class="sxs-lookup"><span data-stu-id="0cd8f-207">For an overview of how to publish a web app on ASP.NET Core, see [Host and deploy](xref:host-and-deploy/index).</span></span> <span data-ttu-id="0cd8f-208">Les tâches et cibles MSBuild nécessaires pour publier une application ASP.NET Core sont open source à https://github.com/aspnet/websdk.</span><span class="sxs-lookup"><span data-stu-id="0cd8f-208">The MSBuild tasks and targets necessary to publish an ASP.NET Core app are open-source at https://github.com/aspnet/websdk.</span></span>

<span data-ttu-id="0cd8f-209">`dotnet publish` permet de dossier, MSDeploy, et [Kudu](https://github.com/projectkudu/kudu/wiki) des profils de publication :</span><span class="sxs-lookup"><span data-stu-id="0cd8f-209">`dotnet publish` can use folder, MSDeploy, and [Kudu](https://github.com/projectkudu/kudu/wiki) publish profiles:</span></span>

<span data-ttu-id="0cd8f-210">Dossier (fonctionne sur plusieurs plateformes) :</span><span class="sxs-lookup"><span data-stu-id="0cd8f-210">Folder (works cross-platform):</span></span>

```console
dotnet publish WebApplication.csproj /p:PublishProfile=<FolderProfileName>
```

<span data-ttu-id="0cd8f-211">MSDeploy (actuellement ce fonctionne uniquement dans Windows depuis MSDeploy n’est pas inter-plateformes) :</span><span class="sxs-lookup"><span data-stu-id="0cd8f-211">MSDeploy (currently this only works in Windows since MSDeploy isn't cross-platform):</span></span>

```console
dotnet publish WebApplication.csproj /p:PublishProfile=<MsDeployProfileName> /p:Password=<DeploymentPassword>
```

<span data-ttu-id="0cd8f-212">Package MSDeploy (actuellement ce fonctionne uniquement dans Windows depuis MSDeploy n’est pas inter-plateformes) :</span><span class="sxs-lookup"><span data-stu-id="0cd8f-212">MSDeploy package (currently this only works in Windows since MSDeploy isn't cross-platform):</span></span>

```console
dotnet publish WebApplication.csproj /p:PublishProfile=<MsDeployPackageProfileName>
```

<span data-ttu-id="0cd8f-213">Dans les exemples précédents, **ne** passer `deployonbuild` à `dotnet publish`.</span><span class="sxs-lookup"><span data-stu-id="0cd8f-213">In the preceding samples, **don't** pass `deployonbuild` to `dotnet publish`.</span></span>

<span data-ttu-id="0cd8f-214">Pour plus d’informations, consultez [Microsoft.NET.Sdk.Publish](https://github.com/aspnet/websdk#microsoftnetsdkpublish).</span><span class="sxs-lookup"><span data-stu-id="0cd8f-214">For more information, see [Microsoft.NET.Sdk.Publish](https://github.com/aspnet/websdk#microsoftnetsdkpublish).</span></span>

<span data-ttu-id="0cd8f-215">`dotnet publish` prend en charge les API Kudu publier sur Azure à partir de n’importe quelle plateforme.</span><span class="sxs-lookup"><span data-stu-id="0cd8f-215">`dotnet publish` supports Kudu APIs to publish to Azure from any platform.</span></span> <span data-ttu-id="0cd8f-216">Prend en charge les API Kudu, mais il est pris en charge par WebSDK pour inter-plateformes publier sur Azure de publication de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="0cd8f-216">Visual Studio publish supports the Kudu APIs, but it's supported by WebSDK for cross-platform publish to Azure.</span></span>

<span data-ttu-id="0cd8f-217">Ajouter un profil de publication pour le *propriétés/PublishProfiles* dossier avec le contenu suivant :</span><span class="sxs-lookup"><span data-stu-id="0cd8f-217">Add a publish profile to the *Properties/PublishProfiles* folder with the following content:</span></span>

```xml
<Project>
  <PropertyGroup>
    <PublishProtocol>Kudu</PublishProtocol>
    <PublishSiteName>nodewebapp</PublishSiteName>
    <UserName>username</UserName>
    <Password>password</Password>
  </PropertyGroup>
</Project>
```

<span data-ttu-id="0cd8f-218">Exécutez la commande suivante pour compresser le contenu de la publication et le publier sur Azure à l’aide de l’API Kudu :</span><span class="sxs-lookup"><span data-stu-id="0cd8f-218">Run the following command to zip up the publish contents and publish it to Azure using the Kudu APIs:</span></span>

```console
dotnet publish /p:PublishProfile=Azure /p:Configuration=Release
```

<span data-ttu-id="0cd8f-219">Définissez les propriétés MSBuild suivantes lors de l’utilisation d’un profil de publication :</span><span class="sxs-lookup"><span data-stu-id="0cd8f-219">Set the following MSBuild properties when using a publish profile:</span></span>

* `DeployOnBuild=true`
* `PublishProfile=<Publish profile name>`

<span data-ttu-id="0cd8f-220">Lors de la publication avec un profil nommé *FolderProfile*, une des commandes ci-dessous peuvent être exécutée :</span><span class="sxs-lookup"><span data-stu-id="0cd8f-220">When publishing with a profile named *FolderProfile*, either of the commands below can be executed:</span></span>

* `dotnet build /p:DeployOnBuild=true /p:PublishProfile=FolderProfile`
* `msbuild      /p:DeployOnBuild=true /p:PublishProfile=FolderProfile`

<span data-ttu-id="0cd8f-221">Lors de l’appel [dotnet build](/dotnet/core/tools/dotnet-build), elle appelle `msbuild` pour exécuter la build et de processus de publication.</span><span class="sxs-lookup"><span data-stu-id="0cd8f-221">When invoking [dotnet build](/dotnet/core/tools/dotnet-build), it calls `msbuild` to run the build and publish process.</span></span> <span data-ttu-id="0cd8f-222">Appelant `dotnet build` ou `msbuild` équivaut lors du passage d’un profil de dossier.</span><span class="sxs-lookup"><span data-stu-id="0cd8f-222">Calling either `dotnet build` or `msbuild` is equivalent when passing in a folder profile.</span></span> <span data-ttu-id="0cd8f-223">Lorsque vous appelez MSBuild directement sur Windows, la version de .NET Framework de MSBuild est utilisée.</span><span class="sxs-lookup"><span data-stu-id="0cd8f-223">When calling MSBuild directly on Windows, the .NET Framework version of MSBuild is used.</span></span> <span data-ttu-id="0cd8f-224">MSDeploy est actuellement limité aux ordinateurs Windows pour la publication.</span><span class="sxs-lookup"><span data-stu-id="0cd8f-224">MSDeploy is currently limited to Windows machines for publishing.</span></span> <span data-ttu-id="0cd8f-225">L’appel de `dotnet build` sur un profil autre qu’un profil de dossier entraîne l’appel de MSBuild, et MSBuild utilise MSDeploy sur les profils autres que les dossiers de profil.</span><span class="sxs-lookup"><span data-stu-id="0cd8f-225">Calling `dotnet build` on a non-folder profile invokes MSBuild, and MSBuild uses MSDeploy on non-folder profiles.</span></span> <span data-ttu-id="0cd8f-226">L’appel de `dotnet build` sur un profil autre qu’un profil de dossier entraîne l’appel de MSBuild (à l’aide de MSDeploy) et échoue (même en cas d’exécution sur une plateforme Windows).</span><span class="sxs-lookup"><span data-stu-id="0cd8f-226">Calling `dotnet build` on a non-folder profile invokes MSBuild (using MSDeploy) and results in a failure (even when running on a Windows platform).</span></span> <span data-ttu-id="0cd8f-227">Pour publier avec un profil autre qu’un profil de dossier, appelez MSBuild directement.</span><span class="sxs-lookup"><span data-stu-id="0cd8f-227">To publish with a non-folder profile, call MSBuild directly.</span></span>

<span data-ttu-id="0cd8f-228">Le profil de publication de type dossier suivant a été créé avec Visual Studio et publie sur un partage réseau :</span><span class="sxs-lookup"><span data-stu-id="0cd8f-228">The following folder publish profile was created with Visual Studio and publishes to a network share:</span></span>

```xml
<?xml version="1.0" encoding="utf-8"?>
<!--
This file is used by the publish/package process of your Web project.
You can customize the behavior of this process by editing this 
MSBuild file.
-->
<Project ToolsVersion="4.0" xmlns="http://schemas.microsoft.com/developer/msbuild/2003">
  <PropertyGroup>
    <WebPublishMethod>FileSystem</WebPublishMethod>
    <PublishProvider>FileSystem</PublishProvider>
    <LastUsedBuildConfiguration>Release</LastUsedBuildConfiguration>
    <LastUsedPlatform>Any CPU</LastUsedPlatform>
    <SiteUrlToLaunchAfterPublish />
    <LaunchSiteAfterPublish>True</LaunchSiteAfterPublish>
    <ExcludeApp_Data>False</ExcludeApp_Data>
    <PublishFramework>netcoreapp1.1</PublishFramework>
    <ProjectGuid>c30c453c-312e-40c4-aec9-394a145dee0b</ProjectGuid>
    <publishUrl>\\r8\Release\AdminWeb</publishUrl>
    <DeleteExistingFiles>False</DeleteExistingFiles>
  </PropertyGroup>
</Project>
```

<span data-ttu-id="0cd8f-229">Notez que `<LastUsedBuildConfiguration>` a la valeur `Release`.</span><span class="sxs-lookup"><span data-stu-id="0cd8f-229">Note `<LastUsedBuildConfiguration>` is set to `Release`.</span></span> <span data-ttu-id="0cd8f-230">Lors de la publication à partir de Visual Studio, la propriété de configuration `<LastUsedBuildConfiguration>` prend la valeur en vigueur lors du démarrage du processus de publication.</span><span class="sxs-lookup"><span data-stu-id="0cd8f-230">When publishing from Visual Studio, the `<LastUsedBuildConfiguration>` configuration property value is set using the value when the publish process is started.</span></span> <span data-ttu-id="0cd8f-231">Le `<LastUsedBuildConfiguration>` propriété de configuration est spéciale et ne doit pas être substituée dans un fichier MSBuild importé.</span><span class="sxs-lookup"><span data-stu-id="0cd8f-231">The `<LastUsedBuildConfiguration>` configuration property is special and shouldn't be overridden in an imported MSBuild file.</span></span> <span data-ttu-id="0cd8f-232">Cette propriété peut être substituée à partir de la ligne de commande.</span><span class="sxs-lookup"><span data-stu-id="0cd8f-232">This property can be overridden from the command line.</span></span>

<span data-ttu-id="0cd8f-233">À l’aide du Kit de développement .NET Core CLI :</span><span class="sxs-lookup"><span data-stu-id="0cd8f-233">Using the .NET Core CLI:</span></span>

```console
dotnet build -c Release /p:DeployOnBuild=true /p:PublishProfile=FolderProfile
```

<span data-ttu-id="0cd8f-234">À l’aide de MSBuild :</span><span class="sxs-lookup"><span data-stu-id="0cd8f-234">Using MSBuild:</span></span>

```console
msbuild /p:Configuration=Release /p:DeployOnBuild=true /p:PublishProfile=FolderProfile
```

## <a name="publish-to-an-msdeploy-endpoint-from-the-command-line"></a><span data-ttu-id="0cd8f-235">Publier sur un point de terminaison MSDeploy à partir de la ligne de commande</span><span class="sxs-lookup"><span data-stu-id="0cd8f-235">Publish to an MSDeploy endpoint from the command line</span></span>

<span data-ttu-id="0cd8f-236">La publication peut être accomplie à l’aide du .NET Core CLI ou MSBuild.</span><span class="sxs-lookup"><span data-stu-id="0cd8f-236">Publishing can be accomplished using the .NET Core CLI or MSBuild.</span></span> <span data-ttu-id="0cd8f-237">`dotnet publish` s’exécute dans le contexte de .NET Core.</span><span class="sxs-lookup"><span data-stu-id="0cd8f-237">`dotnet publish` runs in the context of .NET Core.</span></span> <span data-ttu-id="0cd8f-238">Le `msbuild` commande nécessite .NET Framework, qui restreint aux environnements Windows.</span><span class="sxs-lookup"><span data-stu-id="0cd8f-238">The `msbuild` command requires .NET Framework, which limits it to Windows environments.</span></span>

<span data-ttu-id="0cd8f-239">Pour publier avec MSDeploy, le plus simple est d’abord de créer un profil de publication dans Visual Studio 2017, puis d’utiliser le profil à partir de la ligne de commande.</span><span class="sxs-lookup"><span data-stu-id="0cd8f-239">The easiest way to publish with MSDeploy is to first create a publish profile in Visual Studio 2017 and use the profile from the command line.</span></span>

<span data-ttu-id="0cd8f-240">Dans l’exemple suivant, une application de web ASP.NET Core est créée (à l’aide de `dotnet new mvc`), et un profil de publication Azure est ajouté avec Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="0cd8f-240">In the following sample, an ASP.NET Core web app is created (using `dotnet new mvc`), and an Azure publish profile is added with Visual Studio.</span></span>

<span data-ttu-id="0cd8f-241">Exécutez `msbuild` d’un **invite de commandes développeur pour VS 2017**.</span><span class="sxs-lookup"><span data-stu-id="0cd8f-241">Run `msbuild` from a **Developer Command Prompt for VS 2017**.</span></span> <span data-ttu-id="0cd8f-242">L’invite de commandes développeur possède la bon *msbuild.exe* dans son chemin d’accès avec un ensemble de variables MSBuild.</span><span class="sxs-lookup"><span data-stu-id="0cd8f-242">The Developer Command Prompt has the correct *msbuild.exe* in its path with some MSBuild variables set.</span></span>

<span data-ttu-id="0cd8f-243">MSBuild utilise la syntaxe suivante :</span><span class="sxs-lookup"><span data-stu-id="0cd8f-243">MSBuild uses the following syntax:</span></span>

```console
msbuild <path-to-project-file> /p:DeployOnBuild=true /p:PublishProfile=<Publish Profile> /p:Username=<USERNAME> /p:Password=<PASSWORD>
```

<span data-ttu-id="0cd8f-244">Récupérez le `Password` auprès du fichier  *\<nom de publication >. PublishSettings*.</span><span class="sxs-lookup"><span data-stu-id="0cd8f-244">Get the `Password` from the *\<Publish name>.PublishSettings* file.</span></span> <span data-ttu-id="0cd8f-245">Téléchargez le *.PublishSettings* à partir d’un fichier de :</span><span class="sxs-lookup"><span data-stu-id="0cd8f-245">Download the *.PublishSettings* file from either:</span></span>

* <span data-ttu-id="0cd8f-246">L’Explorateur de solutions : Avec le bouton droit sur l’application Web et sélectionnez **télécharger le profil de publication**.</span><span class="sxs-lookup"><span data-stu-id="0cd8f-246">Solution Explorer: Right-click on the Web App and select **Download Publish Profile**.</span></span>
* <span data-ttu-id="0cd8f-247">Portail Azure : cliquez sur **obtenir le profil de publication** sur l’application Web **vue d’ensemble** Panneau de configuration.</span><span class="sxs-lookup"><span data-stu-id="0cd8f-247">Azure portal: Click **Get publish profile** on the Web App's **Overview** panel.</span></span>

<span data-ttu-id="0cd8f-248">`Username` figure dans le profil de publication.</span><span class="sxs-lookup"><span data-stu-id="0cd8f-248">`Username` can be found in the publish profile.</span></span>

<span data-ttu-id="0cd8f-249">L’exemple suivant utilise le *Web11112 - Web Deploy* profil de publication :</span><span class="sxs-lookup"><span data-stu-id="0cd8f-249">The following sample uses the *Web11112 - Web Deploy* publish profile:</span></span>

```console
msbuild "C:\Webs\Web1\Web1.csproj" /p:DeployOnBuild=true
 /p:PublishProfile="Web11112 - Web Deploy"  /p:Username="$Web11112"
 /p:Password="<password removed>"
```

## <a name="exclude-files"></a><span data-ttu-id="0cd8f-250">Exclure des fichiers</span><span class="sxs-lookup"><span data-stu-id="0cd8f-250">Exclude files</span></span>

<span data-ttu-id="0cd8f-251">Lors de la publication d’applications web ASP.NET Core, les artefacts de build et le contenu du dossier *wwwroot* sont inclus.</span><span class="sxs-lookup"><span data-stu-id="0cd8f-251">When publishing ASP.NET Core web apps, the build artifacts and contents of the *wwwroot* folder are included.</span></span> <span data-ttu-id="0cd8f-252">`msbuild` prend en charge les [modèles d’utilisation des caractères génériques](https://gruntjs.com/configuring-tasks#globbing-patterns).</span><span class="sxs-lookup"><span data-stu-id="0cd8f-252">`msbuild` supports [globbing patterns](https://gruntjs.com/configuring-tasks#globbing-patterns).</span></span> <span data-ttu-id="0cd8f-253">Par exemple, `<Content>` élément exclut tout le texte (*.txt*) fichiers à partir de la *wwwroot/contenu* dossier et tous ses sous-dossiers.</span><span class="sxs-lookup"><span data-stu-id="0cd8f-253">For example, the following `<Content>` element excludes all text (*.txt*) files from the *wwwroot/content* folder and all its subfolders.</span></span>

```xml
<ItemGroup>
  <Content Update="wwwroot/content/**/*.txt" CopyToPublishDirectory="Never" />
</ItemGroup>
```

<span data-ttu-id="0cd8f-254">Le balisage précédent peut être ajouté à un profil de publication ou le *.csproj* fichier.</span><span class="sxs-lookup"><span data-stu-id="0cd8f-254">The preceding markup can be added to a publish profile or the *.csproj* file.</span></span> <span data-ttu-id="0cd8f-255">En cas d’ajout au fichier *.csproj*, la règle est ajoutée à tous les profils de publication dans le projet.</span><span class="sxs-lookup"><span data-stu-id="0cd8f-255">When added to the *.csproj* file, the rule is added to all publish profiles in the project.</span></span>

<span data-ttu-id="0cd8f-256">Les éléments suivants `<MsDeploySkipRules>` élément exclut tous les fichiers à partir de la *wwwroot/contenu* dossier :</span><span class="sxs-lookup"><span data-stu-id="0cd8f-256">The following `<MsDeploySkipRules>` element excludes all files from the *wwwroot/content* folder:</span></span>

```xml
<ItemGroup>
  <MsDeploySkipRules Include="CustomSkipFolder">
    <ObjectName>dirPath</ObjectName>
    <AbsolutePath>wwwroot\\content</AbsolutePath>
  </MsDeploySkipRules>
</ItemGroup>
```

<span data-ttu-id="0cd8f-257">`<MsDeploySkipRules>` ne supprime pas les cibles *skip* du site de déploiement.</span><span class="sxs-lookup"><span data-stu-id="0cd8f-257">`<MsDeploySkipRules>` won't delete the *skip* targets from the deployment site.</span></span> <span data-ttu-id="0cd8f-258">Les fichiers et dossiers `<Content>` ciblés sont supprimés du site de déploiement.</span><span class="sxs-lookup"><span data-stu-id="0cd8f-258">`<Content>` targeted files and folders are deleted from the deployment site.</span></span> <span data-ttu-id="0cd8f-259">Par exemple,  supposez qu'une application web déployée avait les fichiers suivants :</span><span class="sxs-lookup"><span data-stu-id="0cd8f-259">For example, suppose a deployed web app had the following files:</span></span>

* <span data-ttu-id="0cd8f-260">*Views/Home/About1.cshtml*</span><span class="sxs-lookup"><span data-stu-id="0cd8f-260">*Views/Home/About1.cshtml*</span></span>
* <span data-ttu-id="0cd8f-261">*Views/Home/About2.cshtml*</span><span class="sxs-lookup"><span data-stu-id="0cd8f-261">*Views/Home/About2.cshtml*</span></span>
* <span data-ttu-id="0cd8f-262">*Views/Home/About3.cshtml*</span><span class="sxs-lookup"><span data-stu-id="0cd8f-262">*Views/Home/About3.cshtml*</span></span>

<span data-ttu-id="0cd8f-263">Si les éléments suivants `<MsDeploySkipRules>` les éléments sont ajoutés, ces fichiers ne pourraient être supprimées sur le site de déploiement.</span><span class="sxs-lookup"><span data-stu-id="0cd8f-263">If the following `<MsDeploySkipRules>` elements are added, those files wouldn't be deleted on the deployment site.</span></span>

```xml
<ItemGroup>
  <MsDeploySkipRules Include="CustomSkipFile">
    <ObjectName>filePath</ObjectName>
    <AbsolutePath>Views\\Home\\About1.cshtml</AbsolutePath>
  </MsDeploySkipRules>

  <MsDeploySkipRules Include="CustomSkipFile">
    <ObjectName>filePath</ObjectName>
    <AbsolutePath>Views\\Home\\About2.cshtml</AbsolutePath>
  </MsDeploySkipRules>

  <MsDeploySkipRules Include="CustomSkipFile">
    <ObjectName>filePath</ObjectName>
    <AbsolutePath>Views\\Home\\About3.cshtml</AbsolutePath>
  </MsDeploySkipRules>
</ItemGroup>
```

<span data-ttu-id="0cd8f-264">L’exemple précédent `<MsDeploySkipRules>` empêchent les éléments le *ignorée* fichiers d’être déployé.</span><span class="sxs-lookup"><span data-stu-id="0cd8f-264">The preceding `<MsDeploySkipRules>` elements prevent the *skipped* files from being deployed.</span></span> <span data-ttu-id="0cd8f-265">Elle ne supprime pas ces fichiers une fois qu’elles sont déployées.</span><span class="sxs-lookup"><span data-stu-id="0cd8f-265">It won't delete those files once they're deployed.</span></span>

<span data-ttu-id="0cd8f-266">Les éléments suivants `<Content>` élément supprime les fichiers ciblés sur le site de déploiement :</span><span class="sxs-lookup"><span data-stu-id="0cd8f-266">The following `<Content>` element deletes the targeted files at the deployment site:</span></span>

```xml
<ItemGroup>
  <Content Update="Views/Home/About?.cshtml" CopyToPublishDirectory="Never" />
</ItemGroup>
```

<span data-ttu-id="0cd8f-267">À l’aide du déploiement de ligne de commande avec l’exemple précédent `<Content>` élément génère la sortie suivante :</span><span class="sxs-lookup"><span data-stu-id="0cd8f-267">Using command-line deployment with the preceding `<Content>` element yields the following output:</span></span>

```console
MSDeployPublish:
  Starting Web deployment task from source: manifest(C:\Webs\Web1\obj\Release\netcoreapp1.1\PubTmp\Web1.SourceManifest.
  xml) to Destination: auto().
  Deleting file (Web11112\Views\Home\About1.cshtml).
  Deleting file (Web11112\Views\Home\About2.cshtml).
  Deleting file (Web11112\Views\Home\About3.cshtml).
  Updating file (Web11112\web.config).
  Updating file (Web11112\Web1.deps.json).
  Updating file (Web11112\Web1.dll).
  Updating file (Web11112\Web1.pdb).
  Updating file (Web11112\Web1.runtimeconfig.json).
  Successfully executed Web deployment task.
  Publish Succeeded.
Done Building Project "C:\Webs\Web1\Web1.csproj" (default targets).
```

## <a name="include-files"></a><span data-ttu-id="0cd8f-268">Fichiers Include</span><span class="sxs-lookup"><span data-stu-id="0cd8f-268">Include files</span></span>

<span data-ttu-id="0cd8f-269">Le balisage suivant inclut un dossier *images* en dehors du répertoire de projet pour le dossier *wwwroot/images* du site de publication :</span><span class="sxs-lookup"><span data-stu-id="0cd8f-269">The following markup includes an *images* folder outside the project directory to the *wwwroot/images* folder of the publish site:</span></span>

```xml
<ItemGroup>
  <_CustomFiles Include="$(MSBuildProjectDirectory)/../images/**/*" />
  <DotnetPublishFiles Include="@(_CustomFiles)">
    <DestinationRelativePath>wwwroot/images/%(RecursiveDir)%(Filename)%(Extension)</DestinationRelativePath>
  </DotnetPublishFiles>
</ItemGroup>
```

<span data-ttu-id="0cd8f-270">Vous pouvez ajouter le balisage au fichier *.csproj* ou au profil de publication.</span><span class="sxs-lookup"><span data-stu-id="0cd8f-270">The markup can be added to the *.csproj* file or the publish profile.</span></span> <span data-ttu-id="0cd8f-271">S’il est ajouté au fichier *.csproj*, il est inclus dans chaque profil de publication dans le projet.</span><span class="sxs-lookup"><span data-stu-id="0cd8f-271">If it's added to the *.csproj* file, it's included in each publish profile in the project.</span></span>

<span data-ttu-id="0cd8f-272">Le balisage mis en surbrillance ci-dessous montre comment :</span><span class="sxs-lookup"><span data-stu-id="0cd8f-272">The following highlighted markup shows how to:</span></span>

* <span data-ttu-id="0cd8f-273">Copier un fichier situé en dehors du projet dans le dossier *wwwroot*</span><span class="sxs-lookup"><span data-stu-id="0cd8f-273">Copy a file from outside the project into the *wwwroot* folder.</span></span>
* <span data-ttu-id="0cd8f-274">Exclure le dossier *wwwroot\Content*</span><span class="sxs-lookup"><span data-stu-id="0cd8f-274">Exclude the *wwwroot\Content* folder.</span></span>
* <span data-ttu-id="0cd8f-275">Exclure *Views\Home\About2.cshtml*</span><span class="sxs-lookup"><span data-stu-id="0cd8f-275">Exclude *Views\Home\About2.cshtml*.</span></span>

```xml
<?xml version="1.0" encoding="utf-8"?>
<!--
This file is used by the publish/package process of your Web project.
You can customize the behavior of this process by editing this 
MSBuild file.
-->
<Project ToolsVersion="4.0" xmlns="http://schemas.microsoft.com/developer/msbuild/2003">
  <PropertyGroup>
    <WebPublishMethod>FileSystem</WebPublishMethod>
    <PublishProvider>FileSystem</PublishProvider>
    <LastUsedBuildConfiguration>Release</LastUsedBuildConfiguration>
    <LastUsedPlatform>Any CPU</LastUsedPlatform>
    <SiteUrlToLaunchAfterPublish />
    <LaunchSiteAfterPublish>True</LaunchSiteAfterPublish>
    <ExcludeApp_Data>False</ExcludeApp_Data>
    <PublishFramework />
    <ProjectGuid>afa9f185-7ce0-4935-9da1-ab676229d68a</ProjectGuid>
    <publishUrl>bin\Release\PublishOutput</publishUrl>
    <DeleteExistingFiles>False</DeleteExistingFiles>
  </PropertyGroup>
  <ItemGroup>
    <ResolvedFileToPublish Include="..\ReadMe2.MD">
      <RelativePath>wwwroot\ReadMe2.MD</RelativePath>
    </ResolvedFileToPublish>

    <Content Update="wwwroot\Content\**\*" CopyToPublishDirectory="Never" />
    <Content Update="Views\Home\About2.cshtml" CopyToPublishDirectory="Never" />

  </ItemGroup>
</Project>
```

<span data-ttu-id="0cd8f-276">Pour obtenir d’autres exemples de déploiement, consultez le fichier [Lisez-moi webSDK](https://github.com/aspnet/websdk).</span><span class="sxs-lookup"><span data-stu-id="0cd8f-276">See the [WebSDK Readme](https://github.com/aspnet/websdk) for more deployment samples.</span></span>

## <a name="run-a-target-before-or-after-publishing"></a><span data-ttu-id="0cd8f-277">Exécuter une cible avant ou après la publication</span><span class="sxs-lookup"><span data-stu-id="0cd8f-277">Run a target before or after publishing</span></span>

<span data-ttu-id="0cd8f-278">La fonction intégrée `BeforePublish` et `AfterPublish` cibles d’exécutent d’une cible avant ou après la cible de publication.</span><span class="sxs-lookup"><span data-stu-id="0cd8f-278">The built-in `BeforePublish` and `AfterPublish` targets execute a target before or after the publish target.</span></span> <span data-ttu-id="0cd8f-279">Pour le profil de publication pour enregistrer des messages de console avant et après la publication, ajoutez les éléments suivants :</span><span class="sxs-lookup"><span data-stu-id="0cd8f-279">Add the following elements to the publish profile to log console messages both before and after publishing:</span></span>

```xml
<Target Name="CustomActionsBeforePublish" BeforeTargets="BeforePublish">
    <Message Text="Inside BeforePublish" Importance="high" />
  </Target>
  <Target Name="CustomActionsAfterPublish" AfterTargets="AfterPublish">
    <Message Text="Inside AfterPublish" Importance="high" />
</Target>
```

## <a name="publish-to-a-server-using-an-untrusted-certificate"></a><span data-ttu-id="0cd8f-280">Publier sur un serveur à l’aide d’un certificat non approuvé</span><span class="sxs-lookup"><span data-stu-id="0cd8f-280">Publish to a server using an untrusted certificate</span></span>

<span data-ttu-id="0cd8f-281">Ajouter le `<AllowUntrustedCertificate>` propriété avec la valeur `True` pour le profil de publication :</span><span class="sxs-lookup"><span data-stu-id="0cd8f-281">Add the `<AllowUntrustedCertificate>` property with a value of `True` to the publish profile:</span></span>

```xml
<PropertyGroup>
  <AllowUntrustedCertificate>True</AllowUntrustedCertificate>
</PropertyGroup>
```

## <a name="the-kudu-service"></a><span data-ttu-id="0cd8f-282">Le service Kudu</span><span class="sxs-lookup"><span data-stu-id="0cd8f-282">The Kudu service</span></span>

<span data-ttu-id="0cd8f-283">Pour afficher les fichiers dans un déploiement d’application Azure App Service web, utilisez le [service de Kudu](https://github.com/projectkudu/kudu/wiki/Accessing-the-kudu-service).</span><span class="sxs-lookup"><span data-stu-id="0cd8f-283">To view the files in an Azure App Service web app deployment, use the [Kudu service](https://github.com/projectkudu/kudu/wiki/Accessing-the-kudu-service).</span></span> <span data-ttu-id="0cd8f-284">Ajouter le `scm` le jeton dans le nom de l’application web.</span><span class="sxs-lookup"><span data-stu-id="0cd8f-284">Append the `scm` token to the web app name.</span></span> <span data-ttu-id="0cd8f-285">Par exemple :</span><span class="sxs-lookup"><span data-stu-id="0cd8f-285">For example:</span></span>

| <span data-ttu-id="0cd8f-286">URL</span><span class="sxs-lookup"><span data-stu-id="0cd8f-286">URL</span></span>                                    | <span data-ttu-id="0cd8f-287">Résultat</span><span class="sxs-lookup"><span data-stu-id="0cd8f-287">Result</span></span>       |
| -------------------------------------- | ------------ |
| `http://mysite.azurewebsites.net/`     | <span data-ttu-id="0cd8f-288">Application web</span><span class="sxs-lookup"><span data-stu-id="0cd8f-288">Web App</span></span>      |
| `http://mysite.scm.azurewebsites.net/` | <span data-ttu-id="0cd8f-289">Service de kudu</span><span class="sxs-lookup"><span data-stu-id="0cd8f-289">Kudu service</span></span> |

<span data-ttu-id="0cd8f-290">Sélectionnez le [Console de débogage](https://github.com/projectkudu/kudu/wiki/Kudu-console) élément de menu à afficher, modifier, supprimer ou ajouter des fichiers.</span><span class="sxs-lookup"><span data-stu-id="0cd8f-290">Select the [Debug Console](https://github.com/projectkudu/kudu/wiki/Kudu-console) menu item to view, edit, delete, or add files.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="0cd8f-291">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="0cd8f-291">Additional resources</span></span>

* <span data-ttu-id="0cd8f-292">[Web Deploy](https://www.iis.net/downloads/microsoft/web-deploy) (MSDeploy) simplifie le déploiement des applications web et des sites web sur des serveurs IIS.</span><span class="sxs-lookup"><span data-stu-id="0cd8f-292">[Web Deploy](https://www.iis.net/downloads/microsoft/web-deploy) (MSDeploy) simplifies deployment of web apps and websites to IIS servers.</span></span>
* <span data-ttu-id="0cd8f-293">[https://github.com/aspnet/websdk](https://github.com/aspnet/websdk/issues): Problèmes de fichiers et de demander des fonctionnalités pour le déploiement.</span><span class="sxs-lookup"><span data-stu-id="0cd8f-293">[https://github.com/aspnet/websdk](https://github.com/aspnet/websdk/issues): File issues and request features for deployment.</span></span>
