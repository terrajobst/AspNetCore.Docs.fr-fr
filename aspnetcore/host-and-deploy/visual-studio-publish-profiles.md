---
title: "Profils pour le déploiement d’applications ASP.NET Core de publication de Visual Studio"
author: rick-anderson
description: "Découvrez comment créer des profils de publication pour les applications ASP.NET Core dans Visual Studio."
ms.author: riande
manager: wpickett
ms.custom: mvc
ms.date: 09/26/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: host-and-deploy/visual-studio-publish-profiles
ms.openlocfilehash: 1f403447c39db4ebfe3dafda591602f0dc9db8c3
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/24/2018
---
# <a name="visual-studio-publish-profiles-for-aspnet-core-app-deployment"></a><span data-ttu-id="b7913-103">Profils pour le déploiement d’applications ASP.NET Core de publication de Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b7913-103">Visual Studio publish profiles for ASP.NET Core app deployment</span></span>

<span data-ttu-id="b7913-104">De [Sayed Ibrahim Hashimi](https://github.com/sayedihashimi) et [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="b7913-104">By [Sayed Ibrahim Hashimi](https://github.com/sayedihashimi) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="b7913-105">Cet article est axé sur l’utilisation de Visual Studio 2017 pour créer des profils de publication.</span><span class="sxs-lookup"><span data-stu-id="b7913-105">This article focuses on using Visual Studio 2017 to create publish profiles.</span></span> <span data-ttu-id="b7913-106">Les profils de publication créées avec Visual Studio peuvent être exécutés à partir de MSBuild et Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="b7913-106">The publish profiles created with Visual Studio can be run from MSBuild and Visual Studio 2017.</span></span> <span data-ttu-id="b7913-107">L’article fournit des détails sur le processus de publication.</span><span class="sxs-lookup"><span data-stu-id="b7913-107">The article provides details of the publishing process.</span></span> <span data-ttu-id="b7913-108">Consultez [Publier une application web ASP.NET Core sur Azure App Service à l’aide de Visual Studio](xref:tutorials/publish-to-azure-webapp-using-vs) pour obtenir des instructions de publication sur Azure.</span><span class="sxs-lookup"><span data-stu-id="b7913-108">See [Publish an ASP.NET Core web app to Azure App Service using Visual Studio](xref:tutorials/publish-to-azure-webapp-using-vs) for instructions on publishing to Azure.</span></span>

<span data-ttu-id="b7913-109">Le fichier *.csproj* suivant a été créé avec la commande `dotnet new mvc` :</span><span class="sxs-lookup"><span data-stu-id="b7913-109">The following *.csproj* file was created with the command `dotnet new mvc`:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="b7913-110">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="b7913-110">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

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

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="b7913-111">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="b7913-111">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

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

<span data-ttu-id="b7913-112">L’attribut `Sdk` dans l’élément `<Project>` (sur la première ligne) du balisage ci-dessus effectue les opérations suivantes :</span><span class="sxs-lookup"><span data-stu-id="b7913-112">The `Sdk` attribute in the `<Project>` element (in the first line) of the markup above does the following:</span></span>

* <span data-ttu-id="b7913-113">Importe le fichier de propriétés de *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.Props* au début.</span><span class="sxs-lookup"><span data-stu-id="b7913-113">Imports the properties file from *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.Props* at the beginning.</span></span>
* <span data-ttu-id="b7913-114">Importation du fichier targets à partir de *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.targets* à la fin.</span><span class="sxs-lookup"><span data-stu-id="b7913-114">Imports the targets file from *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.targets* at the end.</span></span>

<span data-ttu-id="b7913-115">L’emplacement par défaut de `MSBuildSDKsPath` (avec Visual Studio 2017 Enterprise) est le dossier *%programfiles(x86)%\Microsoft Visual Studio\2017\Enterprise\MSBuild\Sdks*.</span><span class="sxs-lookup"><span data-stu-id="b7913-115">The default location for `MSBuildSDKsPath` (with Visual Studio 2017 Enterprise) is the *%programfiles(x86)%\Microsoft Visual Studio\2017\Enterprise\MSBuild\Sdks* folder.</span></span>

<span data-ttu-id="b7913-116">`Microsoft.NET.Sdk.Web` dépend de :</span><span class="sxs-lookup"><span data-stu-id="b7913-116">`Microsoft.NET.Sdk.Web` depends on:</span></span>

* <span data-ttu-id="b7913-117">*Microsoft.NET.Sdk.Web.ProjectSystem*</span><span class="sxs-lookup"><span data-stu-id="b7913-117">*Microsoft.NET.Sdk.Web.ProjectSystem*</span></span>
* <span data-ttu-id="b7913-118">*Microsoft.NET.Sdk.Publish*</span><span class="sxs-lookup"><span data-stu-id="b7913-118">*Microsoft.NET.Sdk.Publish*</span></span>

<span data-ttu-id="b7913-119">Ce qui entraîne les propriétés et les cibles à importer suivants :</span><span class="sxs-lookup"><span data-stu-id="b7913-119">Which causes the following properties and targets to be imported:</span></span>

* <span data-ttu-id="b7913-120">$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web.ProjectSystem\Sdk\Sdk.Props</span><span class="sxs-lookup"><span data-stu-id="b7913-120">$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web.ProjectSystem\Sdk\Sdk.Props</span></span>
* <span data-ttu-id="b7913-121">$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web.ProjectSystem\Sdk\Sdk.targets</span><span class="sxs-lookup"><span data-stu-id="b7913-121">$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web.ProjectSystem\Sdk\Sdk.targets</span></span>
* <span data-ttu-id="b7913-122">$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Publish\Sdk\Sdk.Props</span><span class="sxs-lookup"><span data-stu-id="b7913-122">$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Publish\Sdk\Sdk.Props</span></span>
* <span data-ttu-id="b7913-123">$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Publish\Sdk\Sdk.targets</span><span class="sxs-lookup"><span data-stu-id="b7913-123">$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Publish\Sdk\Sdk.targets</span></span>

<span data-ttu-id="b7913-124">Publier l’importation des cibles la droite du jeu de cibles en fonction de la méthode de publication utilisée.</span><span class="sxs-lookup"><span data-stu-id="b7913-124">Publish targets import the right set of targets based on the publish method used.</span></span>

<span data-ttu-id="b7913-125">Quand MSBuild ou Visual Studio charge un projet, les actions principales suivantes sont effectuées :</span><span class="sxs-lookup"><span data-stu-id="b7913-125">When MSBuild or Visual Studio loads a project, the following high level actions are performed:</span></span>

* <span data-ttu-id="b7913-126">Génération du projet</span><span class="sxs-lookup"><span data-stu-id="b7913-126">Build project</span></span>
* <span data-ttu-id="b7913-127">Calcul des fichiers à publier</span><span class="sxs-lookup"><span data-stu-id="b7913-127">Compute files to publish</span></span>
* <span data-ttu-id="b7913-128">Publication des fichiers sur la destination</span><span class="sxs-lookup"><span data-stu-id="b7913-128">Publish files to destination</span></span>

### <a name="compute-project-items"></a><span data-ttu-id="b7913-129">Calcul des éléments du projet</span><span class="sxs-lookup"><span data-stu-id="b7913-129">Compute project items</span></span>

<span data-ttu-id="b7913-130">Quand le projet est chargé, les éléments du projet (fichiers) sont calculés.</span><span class="sxs-lookup"><span data-stu-id="b7913-130">When the project is loaded, the project items (files) are computed.</span></span> <span data-ttu-id="b7913-131">L’attribut `item type` détermine comment le fichier est traité.</span><span class="sxs-lookup"><span data-stu-id="b7913-131">The `item type` attribute determines how the file is processed.</span></span> <span data-ttu-id="b7913-132">Par défaut, les fichiers *.cs* sont inclus dans la liste d’éléments `Compile`.</span><span class="sxs-lookup"><span data-stu-id="b7913-132">By default, *.cs* files are included in the `Compile` item list.</span></span> <span data-ttu-id="b7913-133">Les fichiers dans la liste d’éléments `Compile` sont compilés.</span><span class="sxs-lookup"><span data-stu-id="b7913-133">Files in the `Compile` item list are compiled.</span></span>

<span data-ttu-id="b7913-134">Le `Content` liste d’éléments contient des fichiers qui sont publiés en plus les sorties de génération.</span><span class="sxs-lookup"><span data-stu-id="b7913-134">The `Content` item list contains files that are published in addition to the build outputs.</span></span> <span data-ttu-id="b7913-135">Par défaut, les fichiers correspondant au modèle `wwwroot/**` sont inclus dans le `Content` élément.</span><span class="sxs-lookup"><span data-stu-id="b7913-135">By default, files matching the pattern `wwwroot/**` are included in the `Content` item.</span></span> <span data-ttu-id="b7913-136">[wwwroot /\* \* est un modèle de la globalisation](https://gruntjs.com/configuring-tasks#globbing-patterns) qui spécifie tous les fichiers dans le *wwwroot* dossier **et** sous-dossiers.</span><span class="sxs-lookup"><span data-stu-id="b7913-136">[wwwroot/\*\* is a globbing pattern](https://gruntjs.com/configuring-tasks#globbing-patterns) that specifies all files in the *wwwroot* folder **and** subfolders.</span></span> <span data-ttu-id="b7913-137">Pour ajouter explicitement un fichier à la liste de publication, ajoutez-le directement au fichier *.csproj* comme indiqué dans [Inclusion de fichiers](#including-files).</span><span class="sxs-lookup"><span data-stu-id="b7913-137">To explicitly add a file to the publish list, add the file directly in the *.csproj* file as shown in [Including Files](#including-files).</span></span>

<span data-ttu-id="b7913-138">Lorsque vous sélectionnez le **publier** bouton dans Visual Studio, ou lors de la publication à partir de la ligne de commande :</span><span class="sxs-lookup"><span data-stu-id="b7913-138">When selecting the **Publish** button in Visual Studio or when publishing from the command line:</span></span>

* <span data-ttu-id="b7913-139">Les éléments/propriétés sont calculés (il s’agit des fichiers nécessaires à la génération).</span><span class="sxs-lookup"><span data-stu-id="b7913-139">The properties/items are computed (the files that are needed to build).</span></span>
* <span data-ttu-id="b7913-140">Visual Studio uniquement : les packages NuGet sont restaurés.</span><span class="sxs-lookup"><span data-stu-id="b7913-140">Visual Studio only: NuGet packages are restored.</span></span> <span data-ttu-id="b7913-141">(La restauration doit être explicite par l’utilisateur sur l’interface CLI.)</span><span class="sxs-lookup"><span data-stu-id="b7913-141">(Restore needs to be explicit by the user on the CLI.)</span></span>
* <span data-ttu-id="b7913-142">Le projet est généré.</span><span class="sxs-lookup"><span data-stu-id="b7913-142">The project builds.</span></span>
* <span data-ttu-id="b7913-143">Les éléments de publication sont calculés (il s’agit des fichiers nécessaires à la publication).</span><span class="sxs-lookup"><span data-stu-id="b7913-143">The publish items are computed (the files that are needed to publish).</span></span>
* <span data-ttu-id="b7913-144">Le projet est publié.</span><span class="sxs-lookup"><span data-stu-id="b7913-144">The project is published.</span></span> <span data-ttu-id="b7913-145">(Les fichiers calculés sont copiés sur la destination de publication.)</span><span class="sxs-lookup"><span data-stu-id="b7913-145">(The computed files are copied to the publish destination.)</span></span>

<span data-ttu-id="b7913-146">Lorsque fait référence à un projet ASP.NET Core `Microsoft.NET.Sdk.Web` dans le fichier projet, un *app_offline.htm* fichier est placé à la racine du répertoire d’application web.</span><span class="sxs-lookup"><span data-stu-id="b7913-146">When an ASP.NET Core project references `Microsoft.NET.Sdk.Web` in the project file, an *app_offline.htm* file is placed at the root of the web app directory.</span></span> <span data-ttu-id="b7913-147">Quand le fichier est présent, le module ASP.NET Core ferme l’application normalement et sert le fichier *app_offline.htm* pendant le déploiement.</span><span class="sxs-lookup"><span data-stu-id="b7913-147">When the file is present, the ASP.NET Core Module gracefully shuts down the app and serves the *app_offline.htm* file during the deployment.</span></span> <span data-ttu-id="b7913-148">Pour plus d’informations, consultez les [Informations de référence sur la configuration du module ASP.NET Core](xref:host-and-deploy/aspnet-core-module#appofflinehtm).</span><span class="sxs-lookup"><span data-stu-id="b7913-148">For more information, see the [ASP.NET Core Module configuration reference](xref:host-and-deploy/aspnet-core-module#appofflinehtm).</span></span>

## <a name="basic-command-line-publishing"></a><span data-ttu-id="b7913-149">Publication en ligne de commande de base</span><span class="sxs-lookup"><span data-stu-id="b7913-149">Basic command-line publishing</span></span>

<span data-ttu-id="b7913-150">Publication en ligne de commande fonctionne sur toutes les plateformes prises en charge de .NET Core et ne nécessite pas Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="b7913-150">Command-line publishing works on all .NET Core supported platforms and doesn't require Visual Studio.</span></span> <span data-ttu-id="b7913-151">Dans les exemples ci-dessous, la commande `dotnet publish` est exécutée à partir du répertoire du projet (qui contient le fichier *.csproj*).</span><span class="sxs-lookup"><span data-stu-id="b7913-151">In the samples below, the `dotnet publish` command is run from the project directory (which contains the *.csproj* file).</span></span> <span data-ttu-id="b7913-152">Si la valeur n’est pas dans le dossier du projet, passer explicitement dans le chemin d’accès du fichier projet.</span><span class="sxs-lookup"><span data-stu-id="b7913-152">If not in the project folder, explicitly pass in the project file path.</span></span> <span data-ttu-id="b7913-153">Exemple :</span><span class="sxs-lookup"><span data-stu-id="b7913-153">For example:</span></span>

```console
dotnet publish c:/webs/web1
```

<span data-ttu-id="b7913-154">Exécutez les commandes suivantes pour créer et publier une application web :</span><span class="sxs-lookup"><span data-stu-id="b7913-154">Run the following commands to create and publish a web app:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="b7913-155">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="b7913-155">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```console
dotnet new mvc
dotnet publish
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="b7913-156">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="b7913-156">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```console
dotnet new mvc
dotnet restore
dotnet publish
```

---

<span data-ttu-id="b7913-157">`dotnet publish` produit une sortie semblable à la suivante :</span><span class="sxs-lookup"><span data-stu-id="b7913-157">The `dotnet publish` produces output similar to the following:</span></span>

```console
C:\Webs\Web1>dotnet publish
Microsoft (R) Build Engine version 15.3.409.57025 for .NET Core
Copyright (C) Microsoft Corporation. All rights reserved.

  Web1 -> C:\Webs\Web1\bin\Debug\netcoreapp2.0\Web1.dll
  Web1 -> C:\Webs\Web1\bin\Debug\netcoreapp2.0\publish\
```

<span data-ttu-id="b7913-158">Le dossier de publication par défaut est `bin\$(Configuration)\netcoreapp<version>\publish`.</span><span class="sxs-lookup"><span data-stu-id="b7913-158">The default publish folder is `bin\$(Configuration)\netcoreapp<version>\publish`.</span></span> <span data-ttu-id="b7913-159">La valeur par défaut de `$(Configuration)` est Debug.</span><span class="sxs-lookup"><span data-stu-id="b7913-159">The default for `$(Configuration)` is Debug.</span></span> <span data-ttu-id="b7913-160">Dans l’exemple ci-dessus, le `<TargetFramework>` est `netcoreapp2.0`.</span><span class="sxs-lookup"><span data-stu-id="b7913-160">In the sample above, the `<TargetFramework>` is `netcoreapp2.0`.</span></span>

<span data-ttu-id="b7913-161">`dotnet publish -h` affiche des informations d’aide relatives à la publication.</span><span class="sxs-lookup"><span data-stu-id="b7913-161">`dotnet publish -h` displays help information for publish.</span></span>

<span data-ttu-id="b7913-162">La commande suivante spécifie une build `Release` et le répertoire de publication :</span><span class="sxs-lookup"><span data-stu-id="b7913-162">The following command specifies a `Release` build and the publishing directory:</span></span>

```console
dotnet publish -c Release -o C:/MyWebs/test
```

<span data-ttu-id="b7913-163">Le `dotnet publish` commande appelle MSBuild qui appelle le `Publish` cible.</span><span class="sxs-lookup"><span data-stu-id="b7913-163">The `dotnet publish` command calls MSBuild which invokes the `Publish` target.</span></span> <span data-ttu-id="b7913-164">Les paramètres passés à `dotnet publish` sont passés à MSBuild.</span><span class="sxs-lookup"><span data-stu-id="b7913-164">Any parameters passed to `dotnet publish` are passed to MSBuild.</span></span> <span data-ttu-id="b7913-165">Le paramètre `-c` est mappé à la propriété MSBuild `Configuration`.</span><span class="sxs-lookup"><span data-stu-id="b7913-165">The `-c` parameter maps to the `Configuration` MSBuild property.</span></span> <span data-ttu-id="b7913-166">Le paramètre `-o` est mappé à `OutputPath`.</span><span class="sxs-lookup"><span data-stu-id="b7913-166">The `-o` parameter maps to `OutputPath`.</span></span>

<span data-ttu-id="b7913-167">Propriétés MSBuild peuvent être passées à l’aide d’un des formats suivants :</span><span class="sxs-lookup"><span data-stu-id="b7913-167">MSBuild properties can be passed using either of the following formats:</span></span>

* ` p:<NAME>=<VALUE>`
* `/p:<NAME>=<VALUE>`

<span data-ttu-id="b7913-168">La commande suivante publie une build `Release` sur un partage réseau :</span><span class="sxs-lookup"><span data-stu-id="b7913-168">The following command publishes a `Release` build to a network share:</span></span>

`dotnet publish -c Release /p:PublishDir=//r8/release/AdminWeb`

<span data-ttu-id="b7913-169">Le partage réseau est spécifié avec des barres obliques (*//r8/*) et fonctionne sur toutes les plateformes .NET Core prises en charge.</span><span class="sxs-lookup"><span data-stu-id="b7913-169">The network share is specified with forward slashes (*//r8/*) and works on all .NET Core supported platforms.</span></span>

<span data-ttu-id="b7913-170">Vérifiez que l’application publiée pour le déploiement n’est pas en cours d’exécution.</span><span class="sxs-lookup"><span data-stu-id="b7913-170">Confirm that the published app for deployment isn't running.</span></span> <span data-ttu-id="b7913-171">Les fichiers dans le dossier *publish* sont verrouillés quand l’application est en cours d’exécution.</span><span class="sxs-lookup"><span data-stu-id="b7913-171">Files in the *publish* folder are locked when the app is running.</span></span> <span data-ttu-id="b7913-172">Le déploiement ne peut pas se produire car les fichiers verrouillés ne peuvent pas être copiés.</span><span class="sxs-lookup"><span data-stu-id="b7913-172">Deployment can't occur because locked files can't be copied.</span></span>

## <a name="publish-profiles"></a><span data-ttu-id="b7913-173">Profils de publication</span><span class="sxs-lookup"><span data-stu-id="b7913-173">Publish profiles</span></span>

<span data-ttu-id="b7913-174">Cette section utilise Visual Studio 2017 et versions ultérieures pour créer des profils de publication.</span><span class="sxs-lookup"><span data-stu-id="b7913-174">This section uses Visual Studio 2017 and higher to create publishing profiles.</span></span> <span data-ttu-id="b7913-175">Une fois créé, la publication à partir de Visual Studio ou de la ligne de commande est disponible.</span><span class="sxs-lookup"><span data-stu-id="b7913-175">Once created, publishing from Visual Studio or the command line is available.</span></span>

<span data-ttu-id="b7913-176">Les profils de publication peuvent simplifier le processus de publication.</span><span class="sxs-lookup"><span data-stu-id="b7913-176">Publish profiles can simplify the publishing process.</span></span> <span data-ttu-id="b7913-177">Plusieurs profils de publication peut exister.</span><span class="sxs-lookup"><span data-stu-id="b7913-177">Multiple publish profiles can exist.</span></span> <span data-ttu-id="b7913-178">Pour créer un profil de publication dans Visual Studio, avec le bouton droit sur le projet dans l’Explorateur de solutions et sélectionnez **publier**.</span><span class="sxs-lookup"><span data-stu-id="b7913-178">To create a publish profile in Visual Studio, right-click on the project in Solution Explorer and select **Publish**.</span></span> <span data-ttu-id="b7913-179">Vous pouvez également sélectionner **publier \<nom du projet >** dans le menu Générer.</span><span class="sxs-lookup"><span data-stu-id="b7913-179">Alternatively, select **Publish \<project name>** from the build menu.</span></span> <span data-ttu-id="b7913-180">L’onglet **Publier** de la page de capacités d’application s’affiche.</span><span class="sxs-lookup"><span data-stu-id="b7913-180">The **Publish** tab of the application capacities page is displayed.</span></span> <span data-ttu-id="b7913-181">Si le projet ne contient pas de profil de publication, la page suivante s’affiche :</span><span class="sxs-lookup"><span data-stu-id="b7913-181">If the project doesn't contain a publish profile, the following page is displayed:</span></span>

![L’onglet de la publication de la page de capacités application affichant Azure, IIS, FTB, dossier avec Azure sélectionné.](visual-studio-publish-profiles/_static/az.png)

<span data-ttu-id="b7913-184">Quand **Dossier** est sélectionné, le bouton **Publier** crée un dossier de profil de publication et le publie.</span><span class="sxs-lookup"><span data-stu-id="b7913-184">When **Folder** is selected, the **Publish** button creates a folder publish profile and publishes.</span></span>

![L’onglet **Publier** de la page de capacités d’application montrant Azure, IIS, FTB, Dossier](visual-studio-publish-profiles/_static/pub1.png)

<span data-ttu-id="b7913-186">Une fois qu’un profil de publication est créé, le **publier** onglet modifications, puis sélectionnez **créer nouveau profil** pour créer un nouveau profil.</span><span class="sxs-lookup"><span data-stu-id="b7913-186">Once a publish profile is created, the **Publish** tab changes, and select **Create new profile** to create a new profile.</span></span>

![L’onglet **Publier** de la page de capacités d’application montrant FolderProfile (créé lors de la dernière étape) et le lien Créer un profil](visual-studio-publish-profiles/_static/create_new.png)

<span data-ttu-id="b7913-188">L’Assistant Publication prend en charge les cibles de publication suivantes :</span><span class="sxs-lookup"><span data-stu-id="b7913-188">The Publish wizard supports the following publish targets:</span></span>

* <span data-ttu-id="b7913-189">Microsoft Azure App Service</span><span class="sxs-lookup"><span data-stu-id="b7913-189">Microsoft Azure App Service</span></span>
* <span data-ttu-id="b7913-190">IIS, FTP, etc. (pour n’importe quel serveur web)</span><span class="sxs-lookup"><span data-stu-id="b7913-190">IIS, FTP, etc (for any web server)</span></span>
* <span data-ttu-id="b7913-191">Dossier</span><span class="sxs-lookup"><span data-stu-id="b7913-191">Folder</span></span>
* <span data-ttu-id="b7913-192">Importer un profil (permet l’importation du profil).</span><span class="sxs-lookup"><span data-stu-id="b7913-192">Import profile (allows profile import).</span></span>
* <span data-ttu-id="b7913-193">Machines virtuelles Microsoft Azure</span><span class="sxs-lookup"><span data-stu-id="b7913-193">Microsoft Azure Virtual Machines</span></span>

<span data-ttu-id="b7913-194">Pour plus d’informations, consultez [Quelles options de publication choisir ?](https://docs.microsoft.com/visualstudio/ide/not-in-toc/web-publish-options).</span><span class="sxs-lookup"><span data-stu-id="b7913-194">See [What publishing options are right for me?](https://docs.microsoft.com/visualstudio/ide/not-in-toc/web-publish-options) for more information.</span></span>

<span data-ttu-id="b7913-195">Lors de la création d’un profil de publication avec Visual Studio, un *propriétés/PublishProfiles/\<nom de publication > .pubxml* fichier MSBuild est créé.</span><span class="sxs-lookup"><span data-stu-id="b7913-195">When creating a publish profile with Visual Studio, a *Properties/PublishProfiles/\<publish name>.pubxml* MSBuild file is created.</span></span> <span data-ttu-id="b7913-196">Ce fichier *.pubxml* est un fichier MSBuild qui contient des paramètres de configuration de publication.</span><span class="sxs-lookup"><span data-stu-id="b7913-196">This *.pubxml* file is a MSBuild file and contains publish configuration settings.</span></span> <span data-ttu-id="b7913-197">Ce fichier peut être modifié pour personnaliser la génération et le processus de publication.</span><span class="sxs-lookup"><span data-stu-id="b7913-197">This file can be changed to customize the build and publish process.</span></span> <span data-ttu-id="b7913-198">Ce fichier est lu par le processus de publication.</span><span class="sxs-lookup"><span data-stu-id="b7913-198">This file is read by the publishing process.</span></span> <span data-ttu-id="b7913-199">`<LastUsedBuildConfiguration>`est spécial, car elle est une propriété globale et ne doivent pas être dans n’importe quel fichier qui est importé dans la build.</span><span class="sxs-lookup"><span data-stu-id="b7913-199">`<LastUsedBuildConfiguration>` is special because it's a global property and shouldn't be in any file that's imported in the build.</span></span> <span data-ttu-id="b7913-200">Pour plus d’informations, consultez [MSBuild: how to set the configuration property (Comment définir la propriété de configuration)](http://sedodream.com/2012/10/27/MSBuildHowToSetTheConfigurationProperty.aspx).</span><span class="sxs-lookup"><span data-stu-id="b7913-200">See [MSBuild: how to set the configuration property](http://sedodream.com/2012/10/27/MSBuildHowToSetTheConfigurationProperty.aspx) for more info.</span></span>
<span data-ttu-id="b7913-201">Le *.pubxml* fichier ne doit pas être archivé dans le contrôle de code source, car elle dépend du *.user* fichier.</span><span class="sxs-lookup"><span data-stu-id="b7913-201">The *.pubxml* file shouldn't be checked into source control because it depends on the *.user* file.</span></span> <span data-ttu-id="b7913-202">Le fichier *.user* ne doit jamais être archivé dans le contrôle de code source, car il peut contenir des informations sensibles et il est valide uniquement pour un utilisateur et un ordinateur.</span><span class="sxs-lookup"><span data-stu-id="b7913-202">The *.user* file should never be checked into source control because it can contain sensitive information and it's only valid for one user and machine.</span></span>

<span data-ttu-id="b7913-203">Les informations sensibles (telles que le mot de passe de publication) sont chiffrées pour chaque utilisateur/ordinateur et stockées dans le fichier *Properties/PublishProfiles/\<nom_publication>.pubxml.user*.</span><span class="sxs-lookup"><span data-stu-id="b7913-203">Sensitive information (like the publish password) is encrypted on a per user/machine level and stored in the *Properties/PublishProfiles/\<publish name>.pubxml.user* file.</span></span> <span data-ttu-id="b7913-204">Ce fichier pouvant contenir des informations sensibles, il ne doit **pas** être archivé dans le contrôle de code source.</span><span class="sxs-lookup"><span data-stu-id="b7913-204">Because this file can contain sensitive information, it should **not** be checked into source control.</span></span>

<span data-ttu-id="b7913-205">Pour une vue d’ensemble de la publication d’une application web sur ASP.NET Core, consultez [hôte et déployer](index.md).</span><span class="sxs-lookup"><span data-stu-id="b7913-205">For an overview of how to publish a web app on ASP.NET Core see [Host and deploy](index.md).</span></span> <span data-ttu-id="b7913-206">[Héberger et déployer](index.md) est un projet open source à https://github.com/aspnet/websdk.</span><span class="sxs-lookup"><span data-stu-id="b7913-206">[Host and deploy](index.md) is an open source project at https://github.com/aspnet/websdk.</span></span>

<span data-ttu-id="b7913-207">`dotnet publish`permet de dossier, MSDeploy, et [KUDU](https://github.com/projectkudu/kudu/wiki) des profils de publication :</span><span class="sxs-lookup"><span data-stu-id="b7913-207">`dotnet publish` can use folder, MSDeploy, and [KUDU](https://github.com/projectkudu/kudu/wiki) publish profiles:</span></span>
 
<span data-ttu-id="b7913-208">Dossier (fonctionne sur plusieurs plateformes) :`dotnet publish WebApplication.csproj /p:PublishProfile=<FolderProfileName>`</span><span class="sxs-lookup"><span data-stu-id="b7913-208">Folder (works cross-platform): `dotnet publish WebApplication.csproj /p:PublishProfile=<FolderProfileName>`</span></span>

<span data-ttu-id="b7913-209">MSDeploy (actuellement ce fonctionne uniquement dans windows depuis MSDeploy n’est pas inter-plateformes) :`dotnet publish WebApplication.csproj /p:PublishProfile=<MsDeployProfileName> /p:Password=<DeploymentPassword>`</span><span class="sxs-lookup"><span data-stu-id="b7913-209">MSDeploy (currently this only works in windows since MSDeploy isn't cross-platform): `dotnet publish WebApplication.csproj /p:PublishProfile=<MsDeployProfileName> /p:Password=<DeploymentPassword>`</span></span>

<span data-ttu-id="b7913-210">Package MSDeploy (actuellement ce fonctionne uniquement dans windows depuis MSDeploy n’est pas inter-plateformes) :`dotnet publish WebApplication.csproj /p:PublishProfile=<MsDeployPackageProfileName>`</span><span class="sxs-lookup"><span data-stu-id="b7913-210">MSDeploy package(currently this only works in windows since MSDeploy isn't cross-platform): `dotnet publish WebApplication.csproj /p:PublishProfile=<MsDeployPackageProfileName>`</span></span>

<span data-ttu-id="b7913-211">Dans les exemples précédents, **ne** passer `deployonbuild` à `dotnet publish`.</span><span class="sxs-lookup"><span data-stu-id="b7913-211">In the preceeding samples, **don't** pass `deployonbuild` to `dotnet publish`.</span></span>

<span data-ttu-id="b7913-212">Pour plus d’informations, consultez [Microsoft.NET.Sdk.Publish](https://github.com/aspnet/websdk#microsoftnetsdkpublish).</span><span class="sxs-lookup"><span data-stu-id="b7913-212">For more information, see [Microsoft.NET.Sdk.Publish](https://github.com/aspnet/websdk#microsoftnetsdkpublish).</span></span>

<span data-ttu-id="b7913-213">`dotnet publish` prend en charge les API KUDU pour publier sur Azure à partir de n’importe quelle plateforme.</span><span class="sxs-lookup"><span data-stu-id="b7913-213">`dotnet publish` supports KUDU apis to publish to Azure from any platform.</span></span> <span data-ttu-id="b7913-214">Prend en charge les API KUDU, mais il est pris en charge par websdk pour plat croisée publier sur Azure de publication de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="b7913-214">Visual Studio publish does support the KUDU APIs but it's supported by websdk for cross plat publish to Azure.</span></span>

<span data-ttu-id="b7913-215">Ajoutez un profil de publication au dossier *Properties/PublishProfiles* avec le contenu suivant :</span><span class="sxs-lookup"><span data-stu-id="b7913-215">Add a publish profile to *Properties/PublishProfiles* folder with the following content:</span></span>

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

<span data-ttu-id="b7913-216">Exécutant la commande suivante compresse le contenu de la publication et le publier sur Azure à l’aide de l’API KUDU :</span><span class="sxs-lookup"><span data-stu-id="b7913-216">Running the following command zips up the publish contents and publish it to Azure using the KUDU APIs:</span></span>

`dotnet publish /p:PublishProfile=Azure /p:Configuration=Release`

<span data-ttu-id="b7913-217">Définissez les propriétés MSBuild suivantes lors de l’utilisation d’un profil de publication :</span><span class="sxs-lookup"><span data-stu-id="b7913-217">Set the following MSBuild properties when using a publish profile:</span></span>

* `DeployOnBuild=true`
* `PublishProfile=<Publish profile name>`

<span data-ttu-id="b7913-218">Lors de la publication avec un profil nommé *FolderProfile*, une des commandes ci-dessous peuvent être exécutée :</span><span class="sxs-lookup"><span data-stu-id="b7913-218">When publishing with a profile named *FolderProfile*, either of the commands below can be executed:</span></span>

* `dotnet build /p:DeployOnBuild=true /p:PublishProfile=FolderProfile`
* `msbuild      /p:DeployOnBuild=true /p:PublishProfile=FolderProfile`

<span data-ttu-id="b7913-219">Lors de l’appel `dotnet build`, elle appelle `msbuild` pour exécuter la build et de processus de publication.</span><span class="sxs-lookup"><span data-stu-id="b7913-219">When invoking `dotnet build`, it calls `msbuild` to run the build and publish process.</span></span> <span data-ttu-id="b7913-220">Appel de `dotnet build` ou `msbuild` est fondamentalement équivalent lors du passage d’un profil de dossier.</span><span class="sxs-lookup"><span data-stu-id="b7913-220">Calling `dotnet build` or `msbuild` is essentially equivalent when passing in a folder profile.</span></span> <span data-ttu-id="b7913-221">Lorsque vous appelez MSBuild directement sur Windows, la version de .NET Framework de MSBuild est utilisée.</span><span class="sxs-lookup"><span data-stu-id="b7913-221">When calling MSBuild directly on Windows, the .NET Framework version of MSBuild is used.</span></span> <span data-ttu-id="b7913-222">MSDeploy est actuellement limité aux ordinateurs Windows pour la publication.</span><span class="sxs-lookup"><span data-stu-id="b7913-222">MSDeploy is currently limited to Windows machines for publishing.</span></span> <span data-ttu-id="b7913-223">L’appel de `dotnet build` sur un profil autre qu’un profil de dossier entraîne l’appel de MSBuild, et MSBuild utilise MSDeploy sur les profils autres que les dossiers de profil.</span><span class="sxs-lookup"><span data-stu-id="b7913-223">Calling `dotnet build` on a non-folder profile invokes MSBuild, and MSBuild uses MSDeploy on non-folder profiles.</span></span> <span data-ttu-id="b7913-224">L’appel de `dotnet build` sur un profil autre qu’un profil de dossier entraîne l’appel de MSBuild (à l’aide de MSDeploy) et échoue (même en cas d’exécution sur une plateforme Windows).</span><span class="sxs-lookup"><span data-stu-id="b7913-224">Calling `dotnet build` on a non-folder profile invokes MSBuild (using MSDeploy) and results in a failure (even when running on a Windows platform).</span></span> <span data-ttu-id="b7913-225">Pour publier avec un profil autre qu’un profil de dossier, appelez MSBuild directement.</span><span class="sxs-lookup"><span data-stu-id="b7913-225">To publish with a non-folder profile, call MSBuild directly.</span></span>

<span data-ttu-id="b7913-226">Le profil de publication de dossier suivant a été créé avec Visual Studio et publie sur un partage réseau :</span><span class="sxs-lookup"><span data-stu-id="b7913-226">The following folder publish profile was created with Visual Studio and publishes to a network share:</span></span>

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

<span data-ttu-id="b7913-227">Notez que `<LastUsedBuildConfiguration>` a la valeur `Release`.</span><span class="sxs-lookup"><span data-stu-id="b7913-227">Note `<LastUsedBuildConfiguration>` is set to `Release`.</span></span> <span data-ttu-id="b7913-228">Lors de la publication à partir de Visual Studio, la propriété de configuration `<LastUsedBuildConfiguration>` prend la valeur en vigueur lors du démarrage du processus de publication.</span><span class="sxs-lookup"><span data-stu-id="b7913-228">When publishing from Visual Studio, the `<LastUsedBuildConfiguration>` configuration property value is set using the value when the publish process is started.</span></span> <span data-ttu-id="b7913-229">Le `<LastUsedBuildConfiguration>` propriété de configuration est spéciale et ne doit pas être substituée dans un fichier MSBuild importé.</span><span class="sxs-lookup"><span data-stu-id="b7913-229">The `<LastUsedBuildConfiguration>` configuration property is special and shouldn't be overridden in an imported MSBuild file.</span></span> <span data-ttu-id="b7913-230">Cette propriété peut être substituée à partir de la ligne de commande.</span><span class="sxs-lookup"><span data-stu-id="b7913-230">This property can be overridden from the command line.</span></span> <span data-ttu-id="b7913-231">Exemple :</span><span class="sxs-lookup"><span data-stu-id="b7913-231">For example:</span></span>

`dotnet build -c Release /p:DeployOnBuild=true /p:PublishProfile=FolderProfile`

<span data-ttu-id="b7913-232">À l’aide de MSBuild :</span><span class="sxs-lookup"><span data-stu-id="b7913-232">Using MSBuild:</span></span>

`msbuild /p:Configuration=Release /p:DeployOnBuild=true /p:PublishProfile=FolderProfile`

## <a name="publish-to-an-msdeploy-endpoint-from-the-command-line"></a><span data-ttu-id="b7913-233">Publier sur un point de terminaison MSDeploy à partir de la ligne de commande</span><span class="sxs-lookup"><span data-stu-id="b7913-233">Publish to an MSDeploy endpoint from the command line</span></span>

<span data-ttu-id="b7913-234">Comme mentionné précédemment, la publication peut être effectuée à l’aide de `dotnet publish` ou `msbuild` commande.</span><span class="sxs-lookup"><span data-stu-id="b7913-234">As previously mentioned, publishing can be accomplished using `dotnet publish` or the `msbuild` command.</span></span> <span data-ttu-id="b7913-235">`dotnet publish` s’exécute dans le contexte de .NET Core.</span><span class="sxs-lookup"><span data-stu-id="b7913-235">`dotnet publish` runs in the context of .NET Core.</span></span> <span data-ttu-id="b7913-236">Le `msbuild` commande nécessite .NET framework et est donc limitée aux environnements Windows.</span><span class="sxs-lookup"><span data-stu-id="b7913-236">The `msbuild` command requires .NET framework, and is therefore limited to Windows environments.</span></span>

<span data-ttu-id="b7913-237">Pour publier avec MSDeploy, le plus simple est d’abord de créer un profil de publication dans Visual Studio 2017, puis d’utiliser le profil à partir de la ligne de commande.</span><span class="sxs-lookup"><span data-stu-id="b7913-237">The easiest way to publish with MSDeploy is to first create a publish profile in Visual Studio 2017 and use the profile from the command line.</span></span>

<span data-ttu-id="b7913-238">Dans l’exemple suivant, une application de web ASP.NET Core est créée (à l’aide de `dotnet new mvc`), et un profil de publication Azure est ajouté avec Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="b7913-238">In the following sample, an ASP.NET Core web app is created (using `dotnet new mvc`), and an Azure publish profile is added with Visual Studio.</span></span>

<span data-ttu-id="b7913-239">Exécutez `msbuild` d’un **invite de commandes développeur pour VS 2017**.</span><span class="sxs-lookup"><span data-stu-id="b7913-239">Run `msbuild` from a **Developer Command Prompt for VS 2017**.</span></span> <span data-ttu-id="b7913-240">L’invite de commandes développeur possède la bonne *msbuild.exe* dans son chemin d’accès avec un ensemble de variables MSBuild.</span><span class="sxs-lookup"><span data-stu-id="b7913-240">The Developer Command Prompt has the correct *msbuild.exe* in its path with some MSBuild variables set.</span></span>

<span data-ttu-id="b7913-241">MSBuild utilise la syntaxe suivante :</span><span class="sxs-lookup"><span data-stu-id="b7913-241">MSBuild uses the following syntax:</span></span>

`msbuild <path-to-project-file> /p:DeployOnBuild=true /p:PublishProfile=<Publish Profile> /p:Username=<USERNAME> /p:Password=<PASSWORD>`

<span data-ttu-id="b7913-242">Obtenir le `Password` à partir de la  *\<nom de publication >. Paramètres de publication* fichier.</span><span class="sxs-lookup"><span data-stu-id="b7913-242">Get the `Password` from the *\<Publish name>.PublishSettings* file.</span></span> <span data-ttu-id="b7913-243">Téléchargez le *. Paramètres de publication* à partir d’un fichier de :</span><span class="sxs-lookup"><span data-stu-id="b7913-243">Download the *.PublishSettings* file from either:</span></span>

* <span data-ttu-id="b7913-244">L’Explorateur de solutions : Avec le bouton droit sur l’application Web et sélectionnez **télécharger le profil de publication**.</span><span class="sxs-lookup"><span data-stu-id="b7913-244">Solution Explorer: Right-click on the Web App and select **Download Publish Profile**.</span></span>
* <span data-ttu-id="b7913-245">Le portail de gestion Azure : Sélectionnez **obtenir le profil de publication** à partir du Panneau de l’application Web.</span><span class="sxs-lookup"><span data-stu-id="b7913-245">The Azure Management Portal: Select **Get publish profile** from the Web App blade.</span></span>

<span data-ttu-id="b7913-246">`Username` figure dans le profil de publication.</span><span class="sxs-lookup"><span data-stu-id="b7913-246">`Username` can be found in the publish profile.</span></span>

<span data-ttu-id="b7913-247">L’exemple suivant utilise le profil de publication « Web11112 – Web Deploy » :</span><span class="sxs-lookup"><span data-stu-id="b7913-247">The following sample uses the "Web11112 - Web Deploy" publish profile:</span></span>

```console
msbuild "C:\Webs\Web1\Web1.csproj" /p:DeployOnBuild=true
 /p:PublishProfile="Web11112 - Web Deploy"  /p:Username="$Web11112"
 /p:Password="<password removed>"
```

## <a name="excluding-files"></a><span data-ttu-id="b7913-248">Exclusion de fichiers</span><span class="sxs-lookup"><span data-stu-id="b7913-248">Excluding files</span></span>

<span data-ttu-id="b7913-249">Lors de la publication d’applications web ASP.NET Core, les artefacts de build et le contenu du dossier *wwwroot* sont inclus.</span><span class="sxs-lookup"><span data-stu-id="b7913-249">When publishing ASP.NET Core web apps, the build artifacts and contents of the *wwwroot* folder are included.</span></span> <span data-ttu-id="b7913-250">`msbuild` prend en charge les [modèles d’utilisation des caractères génériques](https://gruntjs.com/configuring-tasks#globbing-patterns).</span><span class="sxs-lookup"><span data-stu-id="b7913-250">`msbuild` supports [globbing patterns](https://gruntjs.com/configuring-tasks#globbing-patterns).</span></span> <span data-ttu-id="b7913-251">Par exemple, `<Content>` balisage d’élément exclut tout le texte (*.txt*) fichiers à partir de la *wwwroot/contenu* dossier et tous ses sous-dossiers.</span><span class="sxs-lookup"><span data-stu-id="b7913-251">For example, the following `<Content>` element markup excludes all text (*.txt*) files from the *wwwroot/content* folder and all its subfolders.</span></span>

```xml
<ItemGroup>
  <Content Update="wwwroot/content/**/*.txt" CopyToPublishDirectory="Never" />
</ItemGroup>
```

<span data-ttu-id="b7913-252">Vous pouvez ajouter le balisage ci-dessus à un profil de publication ou au fichier *.csproj*.</span><span class="sxs-lookup"><span data-stu-id="b7913-252">The markup above can be added to a publish profile or the *.csproj* file.</span></span> <span data-ttu-id="b7913-253">En cas d’ajout au fichier *.csproj*, la règle est ajoutée à tous les profils de publication dans le projet.</span><span class="sxs-lookup"><span data-stu-id="b7913-253">When added to the *.csproj* file, the rule is added to all publish profiles in the project.</span></span>

<span data-ttu-id="b7913-254">Le balisage d’éléments `<MsDeploySkipRules>` suivant exclut tous les fichiers du dossier *wwwroot/content* :</span><span class="sxs-lookup"><span data-stu-id="b7913-254">The following `<MsDeploySkipRules>` element markup exludes all files from the *wwwroot/content* folder:</span></span>

```xml
<ItemGroup>
  <MsDeploySkipRules Include="CustomSkipFolder">
    <ObjectName>dirPath</ObjectName>
    <AbsolutePath>wwwroot\\content</AbsolutePath>
  </MsDeploySkipRules>
</ItemGroup>
```

<span data-ttu-id="b7913-255">`<MsDeploySkipRules>`ne supprime pas le *ignorer* cibles à partir du site de déploiement.</span><span class="sxs-lookup"><span data-stu-id="b7913-255">`<MsDeploySkipRules>` won't delete the *skip* targets from the deployment site.</span></span> <span data-ttu-id="b7913-256">`<Content>`dossiers et fichiers ciblés sont supprimés à partir du site de déploiement.</span><span class="sxs-lookup"><span data-stu-id="b7913-256">`<Content>` targeted files and folders are deleted from the deployment site.</span></span> <span data-ttu-id="b7913-257">Par exemple, qu'une application web déployée avait les fichiers suivants :</span><span class="sxs-lookup"><span data-stu-id="b7913-257">For example, suppose a deployed web app had the following files:</span></span>

* <span data-ttu-id="b7913-258">*Views/Home/About1.cshtml*</span><span class="sxs-lookup"><span data-stu-id="b7913-258">*Views/Home/About1.cshtml*</span></span>
* <span data-ttu-id="b7913-259">*Views/Home/About2.cshtml*</span><span class="sxs-lookup"><span data-stu-id="b7913-259">*Views/Home/About2.cshtml*</span></span>
* <span data-ttu-id="b7913-260">*Views/Home/About3.cshtml*</span><span class="sxs-lookup"><span data-stu-id="b7913-260">*Views/Home/About3.cshtml*</span></span>

<span data-ttu-id="b7913-261">Si les éléments suivants `<MsDeploySkipRules>` balisage est ajouté, ces fichiers ne pourraient être supprimées sur le site de déploiement.</span><span class="sxs-lookup"><span data-stu-id="b7913-261">If the following `<MsDeploySkipRules>` markup is added, those files wouldn't be deleted on the deployment site.</span></span>

``` xml
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

<span data-ttu-id="b7913-262">Le `<MsDeploySkipRules>` balisage ci-dessus empêche le *ignorée* fichiers ne soient pas depoyed mais ne supprime pas ces fichiers une fois qu’elles sont déployées.</span><span class="sxs-lookup"><span data-stu-id="b7913-262">The `<MsDeploySkipRules>` markup shown above prevents the *skipped* files from being depoyed but won't delete those files once they're deployed.</span></span>

<span data-ttu-id="b7913-263">Les éléments suivants `<Content>` balisage supprime les fichiers ciblés sur le site de déploiement :</span><span class="sxs-lookup"><span data-stu-id="b7913-263">The following `<Content>` markup deletes the targeted files at the deployment site:</span></span>

``` xml
<ItemGroup>
  <Content Update="Views/Home/About?.cshtml" CopyToPublishDirectory="Never" />
</ItemGroup>
```

<span data-ttu-id="b7913-264">À l’aide d’un déploiement de ligne de commande avec le `<Content>` balisage au-dessus des résultats de sortie similaire à ce qui suit :</span><span class="sxs-lookup"><span data-stu-id="b7913-264">Using command-line deployment with the `<Content>` markup above results in output similar to the following:</span></span>

``` console
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

## <a name="including-files"></a><span data-ttu-id="b7913-265">Inclusion de fichiers</span><span class="sxs-lookup"><span data-stu-id="b7913-265">Including files</span></span>

<span data-ttu-id="b7913-266">Le balisage suivant inclut une *images* dossier en dehors du répertoire de projet pour le *wwwroot/images* dossier du site de publication :</span><span class="sxs-lookup"><span data-stu-id="b7913-266">The following markup includes an *images* folder outside the project directory to the *wwwroot/images* folder of the publish site:</span></span>

``` xml
<ItemGroup>
  <_CustomFiles Include="$(MSBuildProjectDirectory)/../images/**/*" />
  <DotnetPublishFiles Include="@(_CustomFiles)">
    <DestinationRelativePath>wwwroot/images/%(RecursiveDir)%(Filename)%(Extension)</DestinationRelativePath>
  </DotnetPublishFiles>
</ItemGroup>
```

<span data-ttu-id="b7913-267">Vous pouvez ajouter le balisage au fichier *.csproj* ou au profil de publication.</span><span class="sxs-lookup"><span data-stu-id="b7913-267">The markup can be added to the *.csproj* file or the publish profile.</span></span> <span data-ttu-id="b7913-268">S’il est ajouté à la *.csproj* fichier, il est inclus dans chaque profil de publication dans le projet.</span><span class="sxs-lookup"><span data-stu-id="b7913-268">If it's added to the *.csproj* file, it's included in each publish profile in the project.</span></span>

<span data-ttu-id="b7913-269">Le balisage mis en surbrillance ci-dessous montre comment :</span><span class="sxs-lookup"><span data-stu-id="b7913-269">The following highlighted markup shows how to:</span></span>

* <span data-ttu-id="b7913-270">Copier un fichier situé en dehors du projet dans le dossier *wwwroot*</span><span class="sxs-lookup"><span data-stu-id="b7913-270">Copy a file from outside the project into the *wwwroot* folder.</span></span>
* <span data-ttu-id="b7913-271">Exclure le dossier *wwwroot\Content*</span><span class="sxs-lookup"><span data-stu-id="b7913-271">Exclude the *wwwroot\Content* folder.</span></span>
* <span data-ttu-id="b7913-272">Exclure *Views\Home\About2.cshtml*</span><span class="sxs-lookup"><span data-stu-id="b7913-272">Exclude *Views\Home\About2.cshtml*.</span></span>

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

<span data-ttu-id="b7913-273">Pour obtenir d’autres exemples de déploiement, consultez le fichier [Lisez-moi webSDK](https://github.com/aspnet/websdk).</span><span class="sxs-lookup"><span data-stu-id="b7913-273">See the [WebSDK Readme](https://github.com/aspnet/websdk) for more deployment samples.</span></span>

### <a name="run-a-target-before-or-after-publishing"></a><span data-ttu-id="b7913-274">Exécuter une cible avant ou après la publication</span><span class="sxs-lookup"><span data-stu-id="b7913-274">Run a target before or after publishing</span></span>

<span data-ttu-id="b7913-275">La fonction intégrée `BeforePublish` et `AfterPublish` cibles peuvent servir à exécuter une cible avant ou après la cible de publication.</span><span class="sxs-lookup"><span data-stu-id="b7913-275">The built-in `BeforePublish` and `AfterPublish` targets can be used to execute a target before or after the publish target.</span></span> <span data-ttu-id="b7913-276">Vous pouvez ajouter le balisage suivant au profil de publication pour enregistrer les messages dans la sortie de console avant et après la publication :</span><span class="sxs-lookup"><span data-stu-id="b7913-276">The following markup can be added to the publish profile to log messages to the console output before and after publishing:</span></span>

``` xml
<Target Name="CustomActionsBeforePublish" BeforeTargets="BeforePublish">
    <Message Text="Inside BeforePublish" Importance="high" />
  </Target>
  <Target Name="CustomActionsAfterPublish" AfterTargets="AfterPublish">
    <Message Text="Inside AfterPublish" Importance="high" />
</Target>
```

## <a name="the-kudu-service"></a><span data-ttu-id="b7913-277">Le service Kudu</span><span class="sxs-lookup"><span data-stu-id="b7913-277">The Kudu service</span></span>

<span data-ttu-id="b7913-278">Pour afficher les fichiers dans l’un déploiement d’application web Service d’applications Azure, utilisez le [service de Kudu](https://github.com/projectkudu/kudu/wiki/Accessing-the-kudu-service).</span><span class="sxs-lookup"><span data-stu-id="b7913-278">To view the files in the an Azure Apps Service web app deployment, use the [Kudu service](https://github.com/projectkudu/kudu/wiki/Accessing-the-kudu-service).</span></span> <span data-ttu-id="b7913-279">Ajouter le `scm` le jeton dans le nom de l’application web.</span><span class="sxs-lookup"><span data-stu-id="b7913-279">Append the `scm` token to the name of the web app.</span></span> <span data-ttu-id="b7913-280">Exemple :</span><span class="sxs-lookup"><span data-stu-id="b7913-280">For example:</span></span>

| <span data-ttu-id="b7913-281">URL</span><span class="sxs-lookup"><span data-stu-id="b7913-281">URL</span></span>                                    | <span data-ttu-id="b7913-282">Résultat</span><span class="sxs-lookup"><span data-stu-id="b7913-282">Result</span></span>      |
| -------------------------------------- | ----------- |
| `http://mysite.azurewebsites.net/`     | <span data-ttu-id="b7913-283">Application web</span><span class="sxs-lookup"><span data-stu-id="b7913-283">Web App</span></span>     |
| `http://mysite.scm.azurewebsites.net/` | <span data-ttu-id="b7913-284">Service Kudu</span><span class="sxs-lookup"><span data-stu-id="b7913-284">Kudu sevice</span></span> |

<span data-ttu-id="b7913-285">Sélectionnez l’élément de menu [Console de débogage](https://github.com/projectkudu/kudu/wiki/Kudu-console) pour afficher/modifier/supprimer/ajouter des fichiers.</span><span class="sxs-lookup"><span data-stu-id="b7913-285">Select the [Debug Console](https://github.com/projectkudu/kudu/wiki/Kudu-console) menu item to view/edit/delete/add files.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="b7913-286">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="b7913-286">Additional resources</span></span>

* <span data-ttu-id="b7913-287">[Web Deploy](https://www.iis.net/downloads/microsoft/web-deploy) (MSDeploy) simplifie le déploiement des applications web et sites Web aux serveurs IIS.</span><span class="sxs-lookup"><span data-stu-id="b7913-287">[Web Deploy](https://www.iis.net/downloads/microsoft/web-deploy) (MSDeploy) simplifies deployment of web apps and websites to IIS servers.</span></span>
* <span data-ttu-id="b7913-288">[https://github.com/ASPNET/websdk](https://github.com/aspnet/websdk/issues) : soumettez vos problèmes et demandez des fonctionnalités pour le déploiement.</span><span class="sxs-lookup"><span data-stu-id="b7913-288">[https://github.com/aspnet/websdk](https://github.com/aspnet/websdk/issues): File issues and request features for deployment.</span></span>
