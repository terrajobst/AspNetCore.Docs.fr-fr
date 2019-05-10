---
title: Profils de publication Visual Studio pour le déploiement d’applications ASP.NET Core
author: rick-anderson
description: Découvrez comment créer des profils de publication dans Visual Studio et les utiliser pour gérer les déploiements d’applications ASP.NET Core sur différentes cibles.
ms.author: riande
ms.custom: mvc
ms.date: 01/22/2019
uid: host-and-deploy/visual-studio-publish-profiles
ms.openlocfilehash: e1e8f99be18d6f395a146bda805f71c46cd0346d
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/27/2019
ms.locfileid: "64889454"
---
# <a name="visual-studio-publish-profiles-for-aspnet-core-app-deployment"></a><span data-ttu-id="e8485-103">Profils de publication Visual Studio pour le déploiement d’applications ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e8485-103">Visual Studio publish profiles for ASP.NET Core app deployment</span></span>

<span data-ttu-id="e8485-104">De [Sayed Ibrahim Hashimi](https://github.com/sayedihashimi) et [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="e8485-104">By [Sayed Ibrahim Hashimi](https://github.com/sayedihashimi) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range="<= aspnetcore-1.1"

<span data-ttu-id="e8485-105">Pour obtenir la version 1.1 de cette rubrique, téléchargez les [profils de publication Visual Studio pour un déploiement d’application ASP.NET Core (version 1.1, PDF)](https://webpifeed.blob.core.windows.net/webpifeed/Partners/VS_Publish_Profiles_1.1.pdf).</span><span class="sxs-lookup"><span data-stu-id="e8485-105">For the 1.1 version of this topic, download [Visual Studio publish profiles for ASP.NET Core app deployment (version 1.1, PDF)](https://webpifeed.blob.core.windows.net/webpifeed/Partners/VS_Publish_Profiles_1.1.pdf).</span></span>

::: moniker-end

<span data-ttu-id="e8485-106">Ce document est axé sur l’utilisation de Visual Studio 2017 ou d’une version ultérieure pour créer et utiliser des profils de publication.</span><span class="sxs-lookup"><span data-stu-id="e8485-106">This document focuses on using Visual Studio 2017 or later to create and use publish profiles.</span></span> <span data-ttu-id="e8485-107">Les profils de publication créés avec Visual Studio peuvent être exécutés à partir de MSBuild et Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="e8485-107">The publish profiles created with Visual Studio can be run from MSBuild and Visual Studio.</span></span> <span data-ttu-id="e8485-108">Consultez [Publier une application web ASP.NET Core sur Azure App Service à l’aide de Visual Studio](xref:tutorials/publish-to-azure-webapp-using-vs) pour obtenir des instructions de publication sur Azure.</span><span class="sxs-lookup"><span data-stu-id="e8485-108">See [Publish an ASP.NET Core web app to Azure App Service using Visual Studio](xref:tutorials/publish-to-azure-webapp-using-vs) for instructions on publishing to Azure.</span></span>

<span data-ttu-id="e8485-109">Le fichier projet suivant a été créé avec la commande `dotnet new mvc` :</span><span class="sxs-lookup"><span data-stu-id="e8485-109">The following project file was created with the command `dotnet new mvc`:</span></span>

::: moniker range=">= aspnetcore-2.2"

```xml
<Project Sdk="Microsoft.NET.Sdk.Web">

  <PropertyGroup>
    <TargetFramework>netcoreapp2.2</TargetFramework>
  </PropertyGroup>

  <ItemGroup>
    <PackageReference Include="Microsoft.AspNetCore.App" />
  </ItemGroup>

</Project>
```

::: moniker-end

::: moniker range="< aspnetcore-2.2"

```xml
<Project Sdk="Microsoft.NET.Sdk.Web">

  <PropertyGroup>
    <TargetFramework>netcoreapp2.1</TargetFramework>
  </PropertyGroup>

  <ItemGroup>
    <PackageReference Include="Microsoft.AspNetCore.App" />
  </ItemGroup>

</Project>
```

::: moniker-end

<span data-ttu-id="e8485-110">L’attribut `Sdk` de l’élément `<Project>` effectue les tâches suivantes :</span><span class="sxs-lookup"><span data-stu-id="e8485-110">The `<Project>` element's `Sdk` attribute accomplishes the following tasks:</span></span>

* <span data-ttu-id="e8485-111">Importation du fichier de propriétés à partir de *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.Props* au début.</span><span class="sxs-lookup"><span data-stu-id="e8485-111">Imports the properties file from *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.Props* at the beginning.</span></span>
* <span data-ttu-id="e8485-112">Importation du fichier targets à partir de *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.targets* à la fin.</span><span class="sxs-lookup"><span data-stu-id="e8485-112">Imports the targets file from *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.targets* at the end.</span></span>

<span data-ttu-id="e8485-113">L’emplacement par défaut de `MSBuildSDKsPath` (avec Visual Studio 2017 Enterprise) est le dossier *%programfiles(x86)%\Microsoft Visual Studio\2017\Enterprise\MSBuild\Sdks*.</span><span class="sxs-lookup"><span data-stu-id="e8485-113">The default location for `MSBuildSDKsPath` (with Visual Studio 2017 Enterprise) is the *%programfiles(x86)%\Microsoft Visual Studio\2017\Enterprise\MSBuild\Sdks* folder.</span></span>

<span data-ttu-id="e8485-114">Le SDK `Microsoft.NET.Sdk.Web` dépend des éléments suivants :</span><span class="sxs-lookup"><span data-stu-id="e8485-114">The `Microsoft.NET.Sdk.Web` SDK depends on:</span></span>

* <span data-ttu-id="e8485-115">*Microsoft.NET.Sdk.Web.ProjectSystem*</span><span class="sxs-lookup"><span data-stu-id="e8485-115">*Microsoft.NET.Sdk.Web.ProjectSystem*</span></span>
* <span data-ttu-id="e8485-116">*Microsoft.NET.Sdk.Publish*</span><span class="sxs-lookup"><span data-stu-id="e8485-116">*Microsoft.NET.Sdk.Publish*</span></span>

<span data-ttu-id="e8485-117">Ce qui entraîne l’importation des propriétés et cibles suivantes :</span><span class="sxs-lookup"><span data-stu-id="e8485-117">Which causes the following properties and targets to be imported:</span></span>

* <span data-ttu-id="e8485-118">*$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web.ProjectSystem\Sdk\Sdk.Props*</span><span class="sxs-lookup"><span data-stu-id="e8485-118">*$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web.ProjectSystem\Sdk\Sdk.Props*</span></span>
* <span data-ttu-id="e8485-119">*$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web.ProjectSystem\Sdk\Sdk.targets*</span><span class="sxs-lookup"><span data-stu-id="e8485-119">*$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web.ProjectSystem\Sdk\Sdk.targets*</span></span>
* <span data-ttu-id="e8485-120">*$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Publish\Sdk\Sdk.Props*</span><span class="sxs-lookup"><span data-stu-id="e8485-120">*$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Publish\Sdk\Sdk.Props*</span></span>
* <span data-ttu-id="e8485-121">*$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Publish\Sdk\Sdk.targets*</span><span class="sxs-lookup"><span data-stu-id="e8485-121">*$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Publish\Sdk\Sdk.targets*</span></span>

<span data-ttu-id="e8485-122">Les cibles de publication importent l’ensemble approprié de cibles en fonction de la méthode de publication utilisée.</span><span class="sxs-lookup"><span data-stu-id="e8485-122">Publish targets import the right set of targets based on the publish method used.</span></span>

<span data-ttu-id="e8485-123">Quand MSBuild ou Visual Studio charge un projet, les actions principales suivantes se produisent :</span><span class="sxs-lookup"><span data-stu-id="e8485-123">When MSBuild or Visual Studio loads a project, the following high-level actions occur:</span></span>

* <span data-ttu-id="e8485-124">Génération du projet</span><span class="sxs-lookup"><span data-stu-id="e8485-124">Build project</span></span>
* <span data-ttu-id="e8485-125">Calcul des fichiers à publier</span><span class="sxs-lookup"><span data-stu-id="e8485-125">Compute files to publish</span></span>
* <span data-ttu-id="e8485-126">Publication des fichiers sur la destination</span><span class="sxs-lookup"><span data-stu-id="e8485-126">Publish files to destination</span></span>

## <a name="compute-project-items"></a><span data-ttu-id="e8485-127">Calcul des éléments du projet</span><span class="sxs-lookup"><span data-stu-id="e8485-127">Compute project items</span></span>

<span data-ttu-id="e8485-128">Quand le projet est chargé, les éléments du projet (fichiers) sont calculés.</span><span class="sxs-lookup"><span data-stu-id="e8485-128">When the project is loaded, the project items (files) are computed.</span></span> <span data-ttu-id="e8485-129">L’attribut `item type` détermine comment le fichier est traité.</span><span class="sxs-lookup"><span data-stu-id="e8485-129">The `item type` attribute determines how the file is processed.</span></span> <span data-ttu-id="e8485-130">Par défaut, les fichiers *.cs* sont inclus dans la liste d’éléments `Compile`.</span><span class="sxs-lookup"><span data-stu-id="e8485-130">By default, *.cs* files are included in the `Compile` item list.</span></span> <span data-ttu-id="e8485-131">Les fichiers dans la liste d’éléments `Compile` sont compilés.</span><span class="sxs-lookup"><span data-stu-id="e8485-131">Files in the `Compile` item list are compiled.</span></span>

<span data-ttu-id="e8485-132">La liste d’éléments `Content` contient des fichiers qui sont publiés en plus des sorties de génération.</span><span class="sxs-lookup"><span data-stu-id="e8485-132">The `Content` item list contains files that are published in addition to the build outputs.</span></span> <span data-ttu-id="e8485-133">Par défaut, les fichiers qui correspondent au modèle `wwwroot/**` sont inclus dans l’élément `Content`.</span><span class="sxs-lookup"><span data-stu-id="e8485-133">By default, files matching the pattern `wwwroot/**` are included in the `Content` item.</span></span> <span data-ttu-id="e8485-134">Le [modèle d’utilisation des caractères génériques](https://gruntjs.com/configuring-tasks#globbing-patterns) `wwwroot/\*\*` correspond à tous les fichiers dans le dossier *wwwroot* **et** les sous-dossiers.</span><span class="sxs-lookup"><span data-stu-id="e8485-134">The `wwwroot/\*\*` [globbing pattern](https://gruntjs.com/configuring-tasks#globbing-patterns) matches all files in the *wwwroot* folder **and** subfolders.</span></span> <span data-ttu-id="e8485-135">Pour ajouter explicitement un fichier à la liste de publication, ajoutez-le directement au fichier *.csproj* comme indiqué dans [ Inclure des fichiers](#include-files).</span><span class="sxs-lookup"><span data-stu-id="e8485-135">To explicitly add a file to the publish list, add the file directly in the *.csproj* file as shown in [Include Files](#include-files).</span></span>

<span data-ttu-id="e8485-136">Quand vous sélectionnez le bouton **Publier** dans Visual Studio ou quand vous publiez à partir de la ligne de commande :</span><span class="sxs-lookup"><span data-stu-id="e8485-136">When selecting the **Publish** button in Visual Studio or when publishing from the command line:</span></span>

* <span data-ttu-id="e8485-137">Les éléments/propriétés sont calculés (il s’agit des fichiers nécessaires à la génération).</span><span class="sxs-lookup"><span data-stu-id="e8485-137">The properties/items are computed (the files that are needed to build).</span></span>
* <span data-ttu-id="e8485-138">**Visual Studio uniquement** : Les packages NuGet sont restaurés.</span><span class="sxs-lookup"><span data-stu-id="e8485-138">**Visual Studio only**: NuGet packages are restored.</span></span> <span data-ttu-id="e8485-139">(La restauration doit être explicite par l’utilisateur sur l’interface CLI.)</span><span class="sxs-lookup"><span data-stu-id="e8485-139">(Restore needs to be explicit by the user on the CLI.)</span></span>
* <span data-ttu-id="e8485-140">Le projet est généré.</span><span class="sxs-lookup"><span data-stu-id="e8485-140">The project builds.</span></span>
* <span data-ttu-id="e8485-141">Les éléments de publication sont calculés (il s’agit des fichiers nécessaires à la publication).</span><span class="sxs-lookup"><span data-stu-id="e8485-141">The publish items are computed (the files that are needed to publish).</span></span>
* <span data-ttu-id="e8485-142">Le projet est publié (les fichiers calculés sont copiés sur la destination de publication).</span><span class="sxs-lookup"><span data-stu-id="e8485-142">The project is published (the computed files are copied to the publish destination).</span></span>

<span data-ttu-id="e8485-143">Quand un projet ASP.NET Core référence `Microsoft.NET.Sdk.Web` dans le fichier projet, un fichier *app_offline.htm* est placé à la racine du répertoire des applications web.</span><span class="sxs-lookup"><span data-stu-id="e8485-143">When an ASP.NET Core project references `Microsoft.NET.Sdk.Web` in the project file, an *app_offline.htm* file is placed at the root of the web app directory.</span></span> <span data-ttu-id="e8485-144">Quand le fichier est présent, le module ASP.NET Core ferme l’application normalement et sert le fichier *app_offline.htm* pendant le déploiement.</span><span class="sxs-lookup"><span data-stu-id="e8485-144">When the file is present, the ASP.NET Core Module gracefully shuts down the app and serves the *app_offline.htm* file during the deployment.</span></span> <span data-ttu-id="e8485-145">Pour plus d’informations, consultez les [Informations de référence sur la configuration du module ASP.NET Core](xref:host-and-deploy/aspnet-core-module#app_offlinehtm).</span><span class="sxs-lookup"><span data-stu-id="e8485-145">For more information, see the [ASP.NET Core Module configuration reference](xref:host-and-deploy/aspnet-core-module#app_offlinehtm).</span></span>

## <a name="basic-command-line-publishing"></a><span data-ttu-id="e8485-146">Publication de base à partir d’une ligne de commande</span><span class="sxs-lookup"><span data-stu-id="e8485-146">Basic command-line publishing</span></span>

<span data-ttu-id="e8485-147">La publication à partir d’une ligne de commande fonctionne sur toutes les plateformes .NET Core prises en charge et ne nécessite pas Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="e8485-147">Command-line publishing works on all .NET Core-supported platforms and doesn't require Visual Studio.</span></span> <span data-ttu-id="e8485-148">Dans les exemples ci-dessous, la commande [dotnet publish](/dotnet/core/tools/dotnet-publish) est exécutée à partir du répertoire du projet (qui contient le fichier *.csproj*).</span><span class="sxs-lookup"><span data-stu-id="e8485-148">In the samples below, the [dotnet publish](/dotnet/core/tools/dotnet-publish) command is run from the project directory (which contains the *.csproj* file).</span></span> <span data-ttu-id="e8485-149">Si le dossier actuel n’est pas le dossier du projet, transmettez explicitement le chemin du fichier projet.</span><span class="sxs-lookup"><span data-stu-id="e8485-149">If not in the project folder, explicitly pass in the project file path.</span></span> <span data-ttu-id="e8485-150">Par exemple :</span><span class="sxs-lookup"><span data-stu-id="e8485-150">For example:</span></span>

```console
dotnet publish C:\Webs\Web1
```

<span data-ttu-id="e8485-151">Exécutez les commandes suivantes pour créer et publier une application web :</span><span class="sxs-lookup"><span data-stu-id="e8485-151">Run the following commands to create and publish a web app:</span></span>

```console
dotnet new mvc
dotnet publish
```

<span data-ttu-id="e8485-152">La commande [dotnet publish](/dotnet/core/tools/dotnet-publish) produit un résultat similaire au suivant :</span><span class="sxs-lookup"><span data-stu-id="e8485-152">The [dotnet publish](/dotnet/core/tools/dotnet-publish) command produces output similar to the following:</span></span>

```console
C:\Webs\Web1>dotnet publish
Microsoft (R) Build Engine version 15.3.409.57025 for .NET Core
Copyright (C) Microsoft Corporation. All rights reserved.

  Web1 -> C:\Webs\Web1\bin\Debug\netcoreapp2.0\Web1.dll
  Web1 -> C:\Webs\Web1\bin\Debug\netcoreapp2.0\publish\
```

<span data-ttu-id="e8485-153">Le dossier de publication par défaut est `bin\$(Configuration)\netcoreapp<version>\publish`.</span><span class="sxs-lookup"><span data-stu-id="e8485-153">The default publish folder is `bin\$(Configuration)\netcoreapp<version>\publish`.</span></span> <span data-ttu-id="e8485-154">La valeur par défaut de `$(Configuration)` est *Debug*.</span><span class="sxs-lookup"><span data-stu-id="e8485-154">The default for `$(Configuration)` is *Debug*.</span></span> <span data-ttu-id="e8485-155">Dans l’exemple précédent, `<TargetFramework>` a pour valeur `netcoreapp2.0`.</span><span class="sxs-lookup"><span data-stu-id="e8485-155">In the preceding sample, the `<TargetFramework>` is `netcoreapp2.0`.</span></span>

<span data-ttu-id="e8485-156">`dotnet publish -h` affiche des informations d’aide relatives à la publication.</span><span class="sxs-lookup"><span data-stu-id="e8485-156">`dotnet publish -h` displays help information for publish.</span></span>

<span data-ttu-id="e8485-157">La commande suivante spécifie une build `Release` et le répertoire de publication :</span><span class="sxs-lookup"><span data-stu-id="e8485-157">The following command specifies a `Release` build and the publishing directory:</span></span>

```console
dotnet publish -c Release -o C:\MyWebs\test
```

<span data-ttu-id="e8485-158">La commande [dotnet publish](/dotnet/core/tools/dotnet-publish) appelle MSBuild, qui appelle la cible `Publish`.</span><span class="sxs-lookup"><span data-stu-id="e8485-158">The [dotnet publish](/dotnet/core/tools/dotnet-publish) command calls MSBuild, which invokes the `Publish` target.</span></span> <span data-ttu-id="e8485-159">Les paramètres passés à `dotnet publish` sont passés à MSBuild.</span><span class="sxs-lookup"><span data-stu-id="e8485-159">Any parameters passed to `dotnet publish` are passed to MSBuild.</span></span> <span data-ttu-id="e8485-160">Le paramètre `-c` est mappé à la propriété MSBuild `Configuration`.</span><span class="sxs-lookup"><span data-stu-id="e8485-160">The `-c` parameter maps to the `Configuration` MSBuild property.</span></span> <span data-ttu-id="e8485-161">Le paramètre `-o` est mappé à `OutputPath`.</span><span class="sxs-lookup"><span data-stu-id="e8485-161">The `-o` parameter maps to `OutputPath`.</span></span>

<span data-ttu-id="e8485-162">Les propriétés MSBuild peuvent être passées à l’aide de l’un des formats suivants :</span><span class="sxs-lookup"><span data-stu-id="e8485-162">MSBuild properties can be passed using either of the following formats:</span></span>

* `p:<NAME>=<VALUE>`
* `/p:<NAME>=<VALUE>`

<span data-ttu-id="e8485-163">La commande suivante publie une build `Release` sur un partage réseau :</span><span class="sxs-lookup"><span data-stu-id="e8485-163">The following command publishes a `Release` build to a network share:</span></span>

`dotnet publish -c Release /p:PublishDir=//r8/release/AdminWeb`

<span data-ttu-id="e8485-164">Le partage réseau est spécifié avec des barres obliques (*//r8/*) et fonctionne sur toutes les plateformes .NET Core prises en charge.</span><span class="sxs-lookup"><span data-stu-id="e8485-164">The network share is specified with forward slashes (*//r8/*) and works on all .NET Core supported platforms.</span></span>

<span data-ttu-id="e8485-165">Vérifiez que l’application publiée pour le déploiement n’est pas en cours d’exécution.</span><span class="sxs-lookup"><span data-stu-id="e8485-165">Confirm that the published app for deployment isn't running.</span></span> <span data-ttu-id="e8485-166">Les fichiers dans le dossier *publish* sont verrouillés quand l’application est en cours d’exécution.</span><span class="sxs-lookup"><span data-stu-id="e8485-166">Files in the *publish* folder are locked when the app is running.</span></span> <span data-ttu-id="e8485-167">Le déploiement ne peut pas se produire car les fichiers verrouillés ne peuvent pas être copiés.</span><span class="sxs-lookup"><span data-stu-id="e8485-167">Deployment can't occur because locked files can't be copied.</span></span>

## <a name="publish-profiles"></a><span data-ttu-id="e8485-168">Profils de publication</span><span class="sxs-lookup"><span data-stu-id="e8485-168">Publish profiles</span></span>

<span data-ttu-id="e8485-169">Cette section utilise Visual Studio 2017 ou une version ultérieure pour créer un profil de publication.</span><span class="sxs-lookup"><span data-stu-id="e8485-169">This section uses Visual Studio 2017 or later to create a publishing profile.</span></span> <span data-ttu-id="e8485-170">Une fois le profil créé, la publication à partir de Visual Studio ou de la ligne de commande est disponible.</span><span class="sxs-lookup"><span data-stu-id="e8485-170">Once the profile is created, publishing from Visual Studio or the command line is available.</span></span>

<span data-ttu-id="e8485-171">Les profils de publication peuvent simplifier le processus de publication, et n’importe quel nombre de profils peut exister.</span><span class="sxs-lookup"><span data-stu-id="e8485-171">Publish profiles can simplify the publishing process, and any number of profiles can exist.</span></span> <span data-ttu-id="e8485-172">Créez un profil de publication dans Visual Studio en choisissant l’une des méthodes suivantes :</span><span class="sxs-lookup"><span data-stu-id="e8485-172">Create a publish profile in Visual Studio by choosing one of the following paths:</span></span>

* <span data-ttu-id="e8485-173">Cliquez avec le bouton droit sur le projet dans l’Explorateur de solutions, puis sélectionnez **Publier**.</span><span class="sxs-lookup"><span data-stu-id="e8485-173">Right-click the project in Solution Explorer and select **Publish**.</span></span>
* <span data-ttu-id="e8485-174">Sélectionner **Publier {NOM DU PROJET}** dans le menu **Générer**.</span><span class="sxs-lookup"><span data-stu-id="e8485-174">Select **Publish {PROJECT NAME}** from the **Build** menu.</span></span>

<span data-ttu-id="e8485-175">L’onglet **Publier** de la page de capacités d’application s’affiche.</span><span class="sxs-lookup"><span data-stu-id="e8485-175">The **Publish** tab of the app capacities page is displayed.</span></span> <span data-ttu-id="e8485-176">Si le projet n’a pas de profil de publication, la page suivante s’affiche :</span><span class="sxs-lookup"><span data-stu-id="e8485-176">If the project lacks a publish profile, the following page is displayed:</span></span>

![Onglet Publier de la page de capacités d’application](visual-studio-publish-profiles/_static/az.png)

<span data-ttu-id="e8485-178">Quand **Dossier** est sélectionné, spécifiez un chemin de dossier pour stocker les ressources publiées.</span><span class="sxs-lookup"><span data-stu-id="e8485-178">When **Folder** is selected, specify a folder path to store the published assets.</span></span> <span data-ttu-id="e8485-179">Le dossier par défaut est *bin\Release\PublishOutput*.</span><span class="sxs-lookup"><span data-stu-id="e8485-179">The default folder is *bin\Release\PublishOutput*.</span></span> <span data-ttu-id="e8485-180">Cliquez sur le bouton **Créer un profil** pour terminer.</span><span class="sxs-lookup"><span data-stu-id="e8485-180">Click the **Create Profile** button to finish.</span></span>

<span data-ttu-id="e8485-181">Une fois créé un profil de publication, l’onglet **Publier** change.</span><span class="sxs-lookup"><span data-stu-id="e8485-181">Once a publish profile is created, the **Publish** tab changes.</span></span> <span data-ttu-id="e8485-182">Le profil créé apparaît dans une liste déroulante.</span><span class="sxs-lookup"><span data-stu-id="e8485-182">The newly created profile appears in a drop-down list.</span></span> <span data-ttu-id="e8485-183">Cliquez sur **Créer un profil** pour créer un autre profil.</span><span class="sxs-lookup"><span data-stu-id="e8485-183">Click **Create new profile** to create another new profile.</span></span>

![Onglet Publier de la page de capacités d’application montrant FolderProfile](visual-studio-publish-profiles/_static/create_new.png)

<span data-ttu-id="e8485-185">L’Assistant Publication prend en charge les cibles de publication suivantes :</span><span class="sxs-lookup"><span data-stu-id="e8485-185">The Publish wizard supports the following publish targets:</span></span>

* <span data-ttu-id="e8485-186">Azure App Service</span><span class="sxs-lookup"><span data-stu-id="e8485-186">Azure App Service</span></span>
* <span data-ttu-id="e8485-187">Machines virtuelles Azure</span><span class="sxs-lookup"><span data-stu-id="e8485-187">Azure Virtual Machines</span></span>
* <span data-ttu-id="e8485-188">IIS, FTP, etc. (pour n’importe quel serveur web)</span><span class="sxs-lookup"><span data-stu-id="e8485-188">IIS, FTP, etc. (for any web server)</span></span>
* <span data-ttu-id="e8485-189">Dossier</span><span class="sxs-lookup"><span data-stu-id="e8485-189">Folder</span></span>
* <span data-ttu-id="e8485-190">Profil d’importation</span><span class="sxs-lookup"><span data-stu-id="e8485-190">Import Profile</span></span>

<span data-ttu-id="e8485-191">Consultez [Quelles options de publication choisir ?](/visualstudio/ide/not-in-toc/web-publish-options) pour plus d’informations.</span><span class="sxs-lookup"><span data-stu-id="e8485-191">For more information, see [What publishing options are right for me](/visualstudio/ide/not-in-toc/web-publish-options).</span></span>

<span data-ttu-id="e8485-192">Quand vous créez un profil de publication avec Visual Studio, un fichier MSBuild *Properties/PublishProfiles/{NOM DU PROFIL}.pubxml* est créé.</span><span class="sxs-lookup"><span data-stu-id="e8485-192">When creating a publish profile with Visual Studio, a *Properties/PublishProfiles/{PROFILE NAME}.pubxml* MSBuild file is created.</span></span> <span data-ttu-id="e8485-193">Le fichier *.pubxml* est un fichier MSBuild qui contient des paramètres de configuration de publication.</span><span class="sxs-lookup"><span data-stu-id="e8485-193">The *.pubxml* file is a MSBuild file and contains publish configuration settings.</span></span> <span data-ttu-id="e8485-194">Vous pouvez changer ce fichier pour personnaliser le processus de génération et de publication.</span><span class="sxs-lookup"><span data-stu-id="e8485-194">This file can be changed to customize the build and publish process.</span></span> <span data-ttu-id="e8485-195">Ce fichier est lu par le processus de publication.</span><span class="sxs-lookup"><span data-stu-id="e8485-195">This file is read by the publishing process.</span></span> <span data-ttu-id="e8485-196">`<LastUsedBuildConfiguration>` est spéciale, car il s’agit d’une propriété globale qui ne doit être dans aucun fichier importé dans la build.</span><span class="sxs-lookup"><span data-stu-id="e8485-196">`<LastUsedBuildConfiguration>` is special because it's a global property and shouldn't be in any file that's imported in the build.</span></span> <span data-ttu-id="e8485-197">Pour plus d’informations, consultez [MSBuild: how to set the configuration property](http://sedodream.com/2012/10/27/MSBuildHowToSetTheConfigurationProperty.aspx) (Comment définir la propriété de configuration).</span><span class="sxs-lookup"><span data-stu-id="e8485-197">See [MSBuild: how to set the configuration property](http://sedodream.com/2012/10/27/MSBuildHowToSetTheConfigurationProperty.aspx) for more information.</span></span>

<span data-ttu-id="e8485-198">Dans le cas d’une publication sur une cible Azure, le fichier *.pubxml* contient l’identificateur de votre abonnement Azure.</span><span class="sxs-lookup"><span data-stu-id="e8485-198">When publishing to an Azure target, the *.pubxml* file contains your Azure subscription identifier.</span></span> <span data-ttu-id="e8485-199">Avec ce type de cible, nous vous déconseillons d’ajouter ce fichier au contrôle de code source.</span><span class="sxs-lookup"><span data-stu-id="e8485-199">With that target type, adding this file to source control is discouraged.</span></span> <span data-ttu-id="e8485-200">Dans le cas d’une publication sur une cible non-Azure, nous vous recommandons d’archiver le fichier *.pubxml*.</span><span class="sxs-lookup"><span data-stu-id="e8485-200">When publishing to a non-Azure target, it's safe to check in the *.pubxml* file.</span></span>

<span data-ttu-id="e8485-201">Les informations sensibles (telles que le mot de passe de publication) sont chiffrées par utilisateur/machine.</span><span class="sxs-lookup"><span data-stu-id="e8485-201">Sensitive information (like the publish password) is encrypted on a per user/machine level.</span></span> <span data-ttu-id="e8485-202">Elles sont stockées dans le fichier *Properties/PublishProfiles/{NOM DU PROFIL}.pubxml.user*.</span><span class="sxs-lookup"><span data-stu-id="e8485-202">It's stored in the *Properties/PublishProfiles/{PROFILE NAME}.pubxml.user* file.</span></span> <span data-ttu-id="e8485-203">Ce fichier pouvant stocker des informations sensibles, il ne doit pas être archivé dans le contrôle de code source.</span><span class="sxs-lookup"><span data-stu-id="e8485-203">Because this file can store sensitive information, it shouldn't be checked into source control.</span></span>

<span data-ttu-id="e8485-204">Pour obtenir une vue d’ensemble expliquant comment publier une application web sur ASP.NET Core, consultez [Héberger et déployer](xref:host-and-deploy/index).</span><span class="sxs-lookup"><span data-stu-id="e8485-204">For an overview of how to publish a web app on ASP.NET Core, see [Host and deploy](xref:host-and-deploy/index).</span></span> <span data-ttu-id="e8485-205">Les tâches et cibles MSBuild nécessaires pour publier une application ASP.NET Core sont disponibles au format open source à l’adresse https://github.com/aspnet/websdk.</span><span class="sxs-lookup"><span data-stu-id="e8485-205">The MSBuild tasks and targets necessary to publish an ASP.NET Core app are open-source at https://github.com/aspnet/websdk.</span></span>

<span data-ttu-id="e8485-206">`dotnet publish` peut utiliser les profils de publication de dossier, MSDeploy et [Kudu](https://github.com/projectkudu/kudu/wiki) :</span><span class="sxs-lookup"><span data-stu-id="e8485-206">`dotnet publish` can use folder, MSDeploy, and [Kudu](https://github.com/projectkudu/kudu/wiki) publish profiles:</span></span>

<span data-ttu-id="e8485-207">Dossier (fonctionne sur plusieurs plateformes) :</span><span class="sxs-lookup"><span data-stu-id="e8485-207">Folder (works cross-platform):</span></span>

```console
dotnet publish WebApplication.csproj /p:PublishProfile=<FolderProfileName>
```

<span data-ttu-id="e8485-208">MSDeploy (ne fonctionne que sur Windows car MSDeploy n’est pas multiplateforme) :</span><span class="sxs-lookup"><span data-stu-id="e8485-208">MSDeploy (currently this only works in Windows since MSDeploy isn't cross-platform):</span></span>

```console
dotnet publish WebApplication.csproj /p:PublishProfile=<MsDeployProfileName> /p:Password=<DeploymentPassword>
```

<span data-ttu-id="e8485-209">Package MSDeploy (ne fonctionne que sur Windows car MSDeploy n’est pas multiplateforme) :</span><span class="sxs-lookup"><span data-stu-id="e8485-209">MSDeploy package (currently this only works in Windows since MSDeploy isn't cross-platform):</span></span>

```console
dotnet publish WebApplication.csproj /p:PublishProfile=<MsDeployPackageProfileName>
```

<span data-ttu-id="e8485-210">Dans les exemples précédents, **ne** transmettez pas `deployonbuild` à `dotnet publish`.</span><span class="sxs-lookup"><span data-stu-id="e8485-210">In the preceding samples, **don't** pass `deployonbuild` to `dotnet publish`.</span></span>

<span data-ttu-id="e8485-211">Pour plus d’informations, consultez [Microsoft.NET.Sdk.Publish](https://github.com/aspnet/websdk#microsoftnetsdkpublish).</span><span class="sxs-lookup"><span data-stu-id="e8485-211">For more information, see [Microsoft.NET.Sdk.Publish](https://github.com/aspnet/websdk#microsoftnetsdkpublish).</span></span>

<span data-ttu-id="e8485-212">`dotnet publish` prend en charge les API Kudu pour publier sur Azure à partir de n’importe quelle plateforme.</span><span class="sxs-lookup"><span data-stu-id="e8485-212">`dotnet publish` supports Kudu APIs to publish to Azure from any platform.</span></span> <span data-ttu-id="e8485-213">La publication Visual Studio prend en charge les API Kudu, mais avec la prise en charge par WebSDK pour la publication multiplateforme sur Azure.</span><span class="sxs-lookup"><span data-stu-id="e8485-213">Visual Studio publish supports the Kudu APIs, but it's supported by WebSDK for cross-platform publish to Azure.</span></span>

<span data-ttu-id="e8485-214">Ajoutez un profil de publication au dossier *Properties/PublishProfiles* avec le contenu suivant :</span><span class="sxs-lookup"><span data-stu-id="e8485-214">Add a publish profile to the *Properties/PublishProfiles* folder with the following content:</span></span>

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

<span data-ttu-id="e8485-215">Exécutez la commande suivante pour compresser le contenu de la publication et le publier sur Azure à l’aide des API Kudu :</span><span class="sxs-lookup"><span data-stu-id="e8485-215">Run the following command to zip up the publish contents and publish it to Azure using the Kudu APIs:</span></span>

```console
dotnet publish /p:PublishProfile=Azure /p:Configuration=Release
```

<span data-ttu-id="e8485-216">Définissez les propriétés MSBuild suivantes lors de l’utilisation d’un profil de publication :</span><span class="sxs-lookup"><span data-stu-id="e8485-216">Set the following MSBuild properties when using a publish profile:</span></span>

* `DeployOnBuild=true`
* `PublishProfile={PUBLISH PROFILE}`

<span data-ttu-id="e8485-217">Pendant la publication avec un profil nommé *FolderProfile*, l’une des commandes ci-dessous peut être exécutée :</span><span class="sxs-lookup"><span data-stu-id="e8485-217">When publishing with a profile named *FolderProfile*, either of the commands below can be executed:</span></span>

* `dotnet build /p:DeployOnBuild=true /p:PublishProfile=FolderProfile`
* `msbuild      /p:DeployOnBuild=true /p:PublishProfile=FolderProfile`

<span data-ttu-id="e8485-218">Quand elle est appelée, la commande [dotnet build](/dotnet/core/tools/dotnet-build) appelle `msbuild` pour exécuter les processus de génération et de publication.</span><span class="sxs-lookup"><span data-stu-id="e8485-218">When invoking [dotnet build](/dotnet/core/tools/dotnet-build), it calls `msbuild` to run the build and publish process.</span></span> <span data-ttu-id="e8485-219">L’appel de `dotnet build` ou `msbuild` est équivalent au moment du passage d’un profil de dossier.</span><span class="sxs-lookup"><span data-stu-id="e8485-219">Calling either `dotnet build` or `msbuild` is equivalent when passing in a folder profile.</span></span> <span data-ttu-id="e8485-220">Quand vous appelez MSBuild directement sur Windows, la version du .NET Framework de MSBuild est utilisée.</span><span class="sxs-lookup"><span data-stu-id="e8485-220">When calling MSBuild directly on Windows, the .NET Framework version of MSBuild is used.</span></span> <span data-ttu-id="e8485-221">MSDeploy est actuellement limité aux ordinateurs Windows pour la publication.</span><span class="sxs-lookup"><span data-stu-id="e8485-221">MSDeploy is currently limited to Windows machines for publishing.</span></span> <span data-ttu-id="e8485-222">L’appel de `dotnet build` sur un profil autre qu’un profil de dossier entraîne l’appel de MSBuild, et MSBuild utilise MSDeploy sur les profils autres que les dossiers de profil.</span><span class="sxs-lookup"><span data-stu-id="e8485-222">Calling `dotnet build` on a non-folder profile invokes MSBuild, and MSBuild uses MSDeploy on non-folder profiles.</span></span> <span data-ttu-id="e8485-223">L’appel de `dotnet build` sur un profil autre qu’un profil de dossier entraîne l’appel de MSBuild (à l’aide de MSDeploy) et échoue (même en cas d’exécution sur une plateforme Windows).</span><span class="sxs-lookup"><span data-stu-id="e8485-223">Calling `dotnet build` on a non-folder profile invokes MSBuild (using MSDeploy) and results in a failure (even when running on a Windows platform).</span></span> <span data-ttu-id="e8485-224">Pour publier avec un profil autre qu’un profil de dossier, appelez MSBuild directement.</span><span class="sxs-lookup"><span data-stu-id="e8485-224">To publish with a non-folder profile, call MSBuild directly.</span></span>

<span data-ttu-id="e8485-225">Le profil de publication de dossier suivant a été créé avec Visual Studio et publie sur un partage réseau :</span><span class="sxs-lookup"><span data-stu-id="e8485-225">The following folder publish profile was created with Visual Studio and publishes to a network share:</span></span>

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

<span data-ttu-id="e8485-226">Notez que `<LastUsedBuildConfiguration>` a la valeur `Release`.</span><span class="sxs-lookup"><span data-stu-id="e8485-226">Note `<LastUsedBuildConfiguration>` is set to `Release`.</span></span> <span data-ttu-id="e8485-227">Lors de la publication à partir de Visual Studio, la propriété de configuration `<LastUsedBuildConfiguration>` prend la valeur en vigueur lors du démarrage du processus de publication.</span><span class="sxs-lookup"><span data-stu-id="e8485-227">When publishing from Visual Studio, the `<LastUsedBuildConfiguration>` configuration property value is set using the value when the publish process is started.</span></span> <span data-ttu-id="e8485-228">La propriété de configuration `<LastUsedBuildConfiguration>` est spéciale et ne doit pas être substituée dans un fichier MSBuild importé.</span><span class="sxs-lookup"><span data-stu-id="e8485-228">The `<LastUsedBuildConfiguration>` configuration property is special and shouldn't be overridden in an imported MSBuild file.</span></span> <span data-ttu-id="e8485-229">Cette propriété peut être substituée à partir de la ligne de commande.</span><span class="sxs-lookup"><span data-stu-id="e8485-229">This property can be overridden from the command line.</span></span>

<span data-ttu-id="e8485-230">À l’aide de CLI .NET Core :</span><span class="sxs-lookup"><span data-stu-id="e8485-230">Using the .NET Core CLI:</span></span>

```console
dotnet build -c Release /p:DeployOnBuild=true /p:PublishProfile=FolderProfile
```

<span data-ttu-id="e8485-231">À l’aide de MSBuild :</span><span class="sxs-lookup"><span data-stu-id="e8485-231">Using MSBuild:</span></span>

```console
msbuild /p:Configuration=Release /p:DeployOnBuild=true /p:PublishProfile=FolderProfile
```

## <a name="publish-to-an-msdeploy-endpoint-from-the-command-line"></a><span data-ttu-id="e8485-232">Publier sur un point de terminaison MSDeploy à partir de la ligne de commande</span><span class="sxs-lookup"><span data-stu-id="e8485-232">Publish to an MSDeploy endpoint from the command line</span></span>

<span data-ttu-id="e8485-233">L’exemple suivant utilise une application web ASP.NET Core créée par Visual Studio et nommée *AzureWebApp*.</span><span class="sxs-lookup"><span data-stu-id="e8485-233">The following example uses an ASP.NET Core web app created by Visual Studio named *AzureWebApp*.</span></span> <span data-ttu-id="e8485-234">Un profil de publication Azure Apps est ajouté avec Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="e8485-234">An Azure Apps publish profile is added with Visual Studio.</span></span> <span data-ttu-id="e8485-235">Pour plus d’informations sur la création d’un profil, consultez la section [profils de publication](#publish-profiles).</span><span class="sxs-lookup"><span data-stu-id="e8485-235">For more information on how to create a profile, see the [Publish profiles](#publish-profiles) section.</span></span>

<span data-ttu-id="e8485-236">Pour déployer l’application à l’aide d’un profil de publication, exécutez la `msbuild` commande à partir d’une **invite de commandes développeur** Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="e8485-236">To deploy the app using a publish profile, execute the `msbuild` command from a Visual Studio **Developer Command Prompt**.</span></span> <span data-ttu-id="e8485-237">L’invite de commandes est disponible dans le dossier *Visual Studio* du menu **Démarrer** sur la barre des tâches Windows.</span><span class="sxs-lookup"><span data-stu-id="e8485-237">The command prompt is available in the *Visual Studio* folder of the **Start** menu on the Windows taskbar.</span></span> <span data-ttu-id="e8485-238">Pour en faciliter accès, vous pouvez ajouter à l’invite de commandes au menu **Outils** dans Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="e8485-238">For easier access, you can add the command prompt to the **Tools** menu in Visual Studio.</span></span> <span data-ttu-id="e8485-239">Pour plus d’informations, consultez [Invite de commandes développeur pour Visual Studio](/dotnet/framework/tools/developer-command-prompt-for-vs#run-the-command-prompt-from-inside-visual-studio).</span><span class="sxs-lookup"><span data-stu-id="e8485-239">For more information, see [Developer Command Prompt for Visual Studio](/dotnet/framework/tools/developer-command-prompt-for-vs#run-the-command-prompt-from-inside-visual-studio).</span></span>

<span data-ttu-id="e8485-240">MSBuild utilise la syntaxe de commande suivante :</span><span class="sxs-lookup"><span data-stu-id="e8485-240">MSBuild uses the following command syntax:</span></span>

```console
msbuild {PATH} 
    /p:DeployOnBuild=true 
    /p:PublishProfile={PROFILE} 
    /p:Username={USERNAME} 
    /p:Password={PASSWORD}
```

* <span data-ttu-id="e8485-241">{PATH} &ndash; Chemin d’accès vers le fichier de projet de l’application.</span><span class="sxs-lookup"><span data-stu-id="e8485-241">{PATH} &ndash; Path to the app's project file.</span></span>
* <span data-ttu-id="e8485-242">{PROFILE} &ndash; Nom du profil de publication.</span><span class="sxs-lookup"><span data-stu-id="e8485-242">{PROFILE} &ndash; Name of the publish profile.</span></span>
* <span data-ttu-id="e8485-243">{USERNAME} &ndash; Nom d’utilisateur MSDeploy.</span><span class="sxs-lookup"><span data-stu-id="e8485-243">{USERNAME} &ndash; MSDeploy username.</span></span> <span data-ttu-id="e8485-244">{USERNAME} figure dans le profil de publication.</span><span class="sxs-lookup"><span data-stu-id="e8485-244">The {USERNAME} can be found in the publish profile.</span></span>
* <span data-ttu-id="e8485-245">{PASSWORD} &ndash; Mot de passe MSDeploy.</span><span class="sxs-lookup"><span data-stu-id="e8485-245">{PASSWORD} &ndash; MSDeploy password.</span></span> <span data-ttu-id="e8485-246">Obtenez le {PASSWORD} à partir du fichier *{PROFILE}. PublishSettings*.</span><span class="sxs-lookup"><span data-stu-id="e8485-246">Obtain the {PASSWORD} from the *{PROFILE}.PublishSettings* file.</span></span> <span data-ttu-id="e8485-247">Téléchargez le fichier *.PublishSettings* à partir d’un des emplacements suivants :</span><span class="sxs-lookup"><span data-stu-id="e8485-247">Download the *.PublishSettings* file from either:</span></span>
  * <span data-ttu-id="e8485-248">Explorateur de solutions : Sélectionnez **Vue** > **Cloud Explorer**.</span><span class="sxs-lookup"><span data-stu-id="e8485-248">Solution Explorer: Select **View** > **Cloud Explorer**.</span></span> <span data-ttu-id="e8485-249">Connectez-vous avec votre abonnement Azure.</span><span class="sxs-lookup"><span data-stu-id="e8485-249">Connect with your Azure subscription.</span></span> <span data-ttu-id="e8485-250">Ouvrez **App Services**.</span><span class="sxs-lookup"><span data-stu-id="e8485-250">Open **App Services**.</span></span> <span data-ttu-id="e8485-251">Faites un clic droit sur l’application.</span><span class="sxs-lookup"><span data-stu-id="e8485-251">Right-click the app.</span></span> <span data-ttu-id="e8485-252">Sélectionnez **Télecharger un profil de publication**.</span><span class="sxs-lookup"><span data-stu-id="e8485-252">Select **Download Publish Profile**.</span></span>
  * <span data-ttu-id="e8485-253">Portail Azure : cliquez sur **Obtenir le profil de publication** dans le panneau **Vue d’ensemble** de l’application web.</span><span class="sxs-lookup"><span data-stu-id="e8485-253">Azure portal: Select **Get publish profile** in the web app's **Overview** panel.</span></span>

<span data-ttu-id="e8485-254">L’exemple suivant utilise un profil de publication nommé *AzureWebApp - Web Deploy*:</span><span class="sxs-lookup"><span data-stu-id="e8485-254">The following example uses a publish profile named *AzureWebApp - Web Deploy*:</span></span>

```console
msbuild "AzureWebApp.csproj" 
    /p:DeployOnBuild=true 
    /p:PublishProfile="AzureWebApp - Web Deploy" 
    /p:Username="$AzureWebApp" 
    /p:Password=".........."
```

<span data-ttu-id="e8485-255">Un profil de publication peut également être utilisé avec la commande [dotnet msbuild](/dotnet/core/tools/dotnet-msbuild) de l’interface CLI .NET Core depuis une invite de commandes Windows :</span><span class="sxs-lookup"><span data-stu-id="e8485-255">A publish profile can also be used with the .NET Core CLI [dotnet msbuild](/dotnet/core/tools/dotnet-msbuild) command from a Windows command prompt:</span></span>

```console
dotnet msbuild "AzureWebApp.csproj"
    /p:DeployOnBuild=true 
    /p:PublishProfile="AzureWebApp - Web Deploy" 
    /p:Username="$AzureWebApp" 
    /p:Password=".........."
```

> [!NOTE]
> <span data-ttu-id="e8485-256">La commande [dotnet msbuild](/dotnet/core/tools/dotnet-msbuild) est multiplateforme et peut compiler des applications ASP.NET Core sur macOS et Linux.</span><span class="sxs-lookup"><span data-stu-id="e8485-256">The [dotnet msbuild](/dotnet/core/tools/dotnet-msbuild) command is available cross-platform and can compile ASP.NET Core apps on macOS and Linux.</span></span> <span data-ttu-id="e8485-257">Toutefois, MSBuild sur macOS et Linux n’est pas capable de déployer une application dans Azure ou un autre point de terminaison MSDeploy.</span><span class="sxs-lookup"><span data-stu-id="e8485-257">However, MSBuild on macOS and Linux isn't capable of deploying an app to Azure or other MSDeploy endpoint.</span></span> <span data-ttu-id="e8485-258">MSDeploy est disponible uniquement sur Windows.</span><span class="sxs-lookup"><span data-stu-id="e8485-258">MSDeploy is only available on Windows.</span></span>

## <a name="set-the-environment"></a><span data-ttu-id="e8485-259">Définir l’environnement</span><span class="sxs-lookup"><span data-stu-id="e8485-259">Set the environment</span></span>

<span data-ttu-id="e8485-260">Incluez la propriété `<EnvironmentName>` dans le profil de facturation (*.pubxml*) ou le fichier projet pour définir [l’environnement](xref:fundamentals/environments) de l’application :</span><span class="sxs-lookup"><span data-stu-id="e8485-260">Include the `<EnvironmentName>` property in the publish profile (*.pubxml*) or project file to set the app's [environment](xref:fundamentals/environments):</span></span>

```xml
<PropertyGroup>
  <EnvironmentName>Development</EnvironmentName>
</PropertyGroup>
```

<span data-ttu-id="e8485-261">Si vous avez besoin de transformations de *web.config* (par exemple, définir les variables d’environnement basées sur la configuration, le profil ou l’environnement), consultez <xref:host-and-deploy/iis/transform-webconfig>.</span><span class="sxs-lookup"><span data-stu-id="e8485-261">If you require *web.config* transformations (for example, setting environment variables based on the configuration, profile, or environment), see <xref:host-and-deploy/iis/transform-webconfig>.</span></span>

## <a name="exclude-files"></a><span data-ttu-id="e8485-262">Exclure des fichiers</span><span class="sxs-lookup"><span data-stu-id="e8485-262">Exclude files</span></span>

<span data-ttu-id="e8485-263">Lors de la publication d’applications web ASP.NET Core, les artefacts de build et le contenu du dossier *wwwroot* sont inclus.</span><span class="sxs-lookup"><span data-stu-id="e8485-263">When publishing ASP.NET Core web apps, the build artifacts and contents of the *wwwroot* folder are included.</span></span> <span data-ttu-id="e8485-264">`msbuild` prend en charge les [modèles d’utilisation des caractères génériques](https://gruntjs.com/configuring-tasks#globbing-patterns).</span><span class="sxs-lookup"><span data-stu-id="e8485-264">`msbuild` supports [globbing patterns](https://gruntjs.com/configuring-tasks#globbing-patterns).</span></span> <span data-ttu-id="e8485-265">Par exemple, l’élément `<Content>` suivant exclut tous les fichiers texte (*.txt*) du dossier *wwwroot/content* et tous ses sous-dossiers.</span><span class="sxs-lookup"><span data-stu-id="e8485-265">For example, the following `<Content>` element excludes all text (*.txt*) files from the *wwwroot/content* folder and all its subfolders.</span></span>

```xml
<ItemGroup>
  <Content Update="wwwroot/content/**/*.txt" CopyToPublishDirectory="Never" />
</ItemGroup>
```

<span data-ttu-id="e8485-266">Vous pouvez ajouter le balisage précédent à un profil de publication ou au fichier *.csproj*.</span><span class="sxs-lookup"><span data-stu-id="e8485-266">The preceding markup can be added to a publish profile or the *.csproj* file.</span></span> <span data-ttu-id="e8485-267">En cas d’ajout au fichier *.csproj*, la règle est ajoutée à tous les profils de publication dans le projet.</span><span class="sxs-lookup"><span data-stu-id="e8485-267">When added to the *.csproj* file, the rule is added to all publish profiles in the project.</span></span>

<span data-ttu-id="e8485-268">L’élément `<MsDeploySkipRules>` suivant exclut tous les fichiers du dossier *wwwroot/content* :</span><span class="sxs-lookup"><span data-stu-id="e8485-268">The following `<MsDeploySkipRules>` element excludes all files from the *wwwroot/content* folder:</span></span>

```xml
<ItemGroup>
  <MsDeploySkipRules Include="CustomSkipFolder">
    <ObjectName>dirPath</ObjectName>
    <AbsolutePath>wwwroot\\content</AbsolutePath>
  </MsDeploySkipRules>
</ItemGroup>
```

<span data-ttu-id="e8485-269">`<MsDeploySkipRules>` ne supprime pas les cibles *skip* du site de déploiement.</span><span class="sxs-lookup"><span data-stu-id="e8485-269">`<MsDeploySkipRules>` won't delete the *skip* targets from the deployment site.</span></span> <span data-ttu-id="e8485-270">Les dossiers et fichiers ciblés par `<Content>` sont supprimés du site de déploiement.</span><span class="sxs-lookup"><span data-stu-id="e8485-270">`<Content>` targeted files and folders are deleted from the deployment site.</span></span> <span data-ttu-id="e8485-271">Par exemple, supposez qu’une application web déployée avait les fichiers suivants :</span><span class="sxs-lookup"><span data-stu-id="e8485-271">For example, suppose a deployed web app had the following files:</span></span>

* <span data-ttu-id="e8485-272">*Views/Home/About1.cshtml*</span><span class="sxs-lookup"><span data-stu-id="e8485-272">*Views/Home/About1.cshtml*</span></span>
* <span data-ttu-id="e8485-273">*Views/Home/About2.cshtml*</span><span class="sxs-lookup"><span data-stu-id="e8485-273">*Views/Home/About2.cshtml*</span></span>
* <span data-ttu-id="e8485-274">*Views/Home/About3.cshtml*</span><span class="sxs-lookup"><span data-stu-id="e8485-274">*Views/Home/About3.cshtml*</span></span>

<span data-ttu-id="e8485-275">Si les éléments `<MsDeploySkipRules>` suivants sont ajoutés, ces fichiers ne sont pas supprimés sur le site de déploiement.</span><span class="sxs-lookup"><span data-stu-id="e8485-275">If the following `<MsDeploySkipRules>` elements are added, those files wouldn't be deleted on the deployment site.</span></span>

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

<span data-ttu-id="e8485-276">Les éléments `<MsDeploySkipRules>` précédents empêchent le déploiement des fichiers *skipped*.</span><span class="sxs-lookup"><span data-stu-id="e8485-276">The preceding `<MsDeploySkipRules>` elements prevent the *skipped* files from being deployed.</span></span> <span data-ttu-id="e8485-277">Ces fichiers ne sont pas supprimés une fois déployés.</span><span class="sxs-lookup"><span data-stu-id="e8485-277">It won't delete those files once they're deployed.</span></span>

<span data-ttu-id="e8485-278">L’élément `<Content>` suivant supprime les fichiers ciblés sur le site de déploiement :</span><span class="sxs-lookup"><span data-stu-id="e8485-278">The following `<Content>` element deletes the targeted files at the deployment site:</span></span>

```xml
<ItemGroup>
  <Content Update="Views/Home/About?.cshtml" CopyToPublishDirectory="Never" />
</ItemGroup>
```

<span data-ttu-id="e8485-279">L’utilisation du déploiement à partir de la ligne de commande avec l’élément `<Content>` précédent génère la sortie suivante :</span><span class="sxs-lookup"><span data-stu-id="e8485-279">Using command-line deployment with the preceding `<Content>` element yields the following output:</span></span>

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

## <a name="include-files"></a><span data-ttu-id="e8485-280">Fichiers Include</span><span class="sxs-lookup"><span data-stu-id="e8485-280">Include files</span></span>

<span data-ttu-id="e8485-281">Le balisage suivant inclut un dossier *images* situé en dehors du répertoire de projet dans le dossier *wwwroot/images* du site de publication :</span><span class="sxs-lookup"><span data-stu-id="e8485-281">The following markup includes an *images* folder outside the project directory to the *wwwroot/images* folder of the publish site:</span></span>

```xml
<ItemGroup>
  <_CustomFiles Include="$(MSBuildProjectDirectory)/../images/**/*" />
  <DotnetPublishFiles Include="@(_CustomFiles)">
    <DestinationRelativePath>wwwroot/images/%(RecursiveDir)%(Filename)%(Extension)</DestinationRelativePath>
  </DotnetPublishFiles>
</ItemGroup>
```

<span data-ttu-id="e8485-282">Vous pouvez ajouter le balisage au fichier *.csproj* ou au profil de publication.</span><span class="sxs-lookup"><span data-stu-id="e8485-282">The markup can be added to the *.csproj* file or the publish profile.</span></span> <span data-ttu-id="e8485-283">Si vous l’ajoutez au fichier *csproj*, il est inclus dans chaque profil de publication du projet.</span><span class="sxs-lookup"><span data-stu-id="e8485-283">If it's added to the *.csproj* file, it's included in each publish profile in the project.</span></span>

<span data-ttu-id="e8485-284">Le balisage mis en surbrillance ci-dessous montre comment :</span><span class="sxs-lookup"><span data-stu-id="e8485-284">The following highlighted markup shows how to:</span></span>

* <span data-ttu-id="e8485-285">Copier un fichier situé en dehors du projet dans le dossier *wwwroot*</span><span class="sxs-lookup"><span data-stu-id="e8485-285">Copy a file from outside the project into the *wwwroot* folder.</span></span>
* <span data-ttu-id="e8485-286">Exclure le dossier *wwwroot\Content*</span><span class="sxs-lookup"><span data-stu-id="e8485-286">Exclude the *wwwroot\Content* folder.</span></span>
* <span data-ttu-id="e8485-287">Exclure *Views\Home\About2.cshtml*</span><span class="sxs-lookup"><span data-stu-id="e8485-287">Exclude *Views\Home\About2.cshtml*.</span></span>

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

<span data-ttu-id="e8485-288">Pour obtenir d’autres exemples de déploiement, consultez le fichier [Lisez-moi webSDK](https://github.com/aspnet/websdk).</span><span class="sxs-lookup"><span data-stu-id="e8485-288">See the [WebSDK Readme](https://github.com/aspnet/websdk) for more deployment samples.</span></span>

## <a name="run-a-target-before-or-after-publishing"></a><span data-ttu-id="e8485-289">Exécuter une cible avant ou après la publication</span><span class="sxs-lookup"><span data-stu-id="e8485-289">Run a target before or after publishing</span></span>

<span data-ttu-id="e8485-290">Les cibles intégrées `BeforePublish` et `AfterPublish` exécutent une cible avant ou après la cible de publication.</span><span class="sxs-lookup"><span data-stu-id="e8485-290">The built-in `BeforePublish` and `AfterPublish` targets execute a target before or after the publish target.</span></span> <span data-ttu-id="e8485-291">Ajoutez les éléments suivants au profil de publication pour journaliser les messages de console avant et après la publication :</span><span class="sxs-lookup"><span data-stu-id="e8485-291">Add the following elements to the publish profile to log console messages both before and after publishing:</span></span>

```xml
<Target Name="CustomActionsBeforePublish" BeforeTargets="BeforePublish">
    <Message Text="Inside BeforePublish" Importance="high" />
  </Target>
  <Target Name="CustomActionsAfterPublish" AfterTargets="AfterPublish">
    <Message Text="Inside AfterPublish" Importance="high" />
</Target>
```

## <a name="publish-to-a-server-using-an-untrusted-certificate"></a><span data-ttu-id="e8485-292">Publier sur un serveur à l’aide d’un certificat non approuvé</span><span class="sxs-lookup"><span data-stu-id="e8485-292">Publish to a server using an untrusted certificate</span></span>

<span data-ttu-id="e8485-293">Ajoutez la propriété `<AllowUntrustedCertificate>` avec la valeur `True` au profil de publication :</span><span class="sxs-lookup"><span data-stu-id="e8485-293">Add the `<AllowUntrustedCertificate>` property with a value of `True` to the publish profile:</span></span>

```xml
<PropertyGroup>
  <AllowUntrustedCertificate>True</AllowUntrustedCertificate>
</PropertyGroup>
```

## <a name="the-kudu-service"></a><span data-ttu-id="e8485-294">Le service Kudu</span><span class="sxs-lookup"><span data-stu-id="e8485-294">The Kudu service</span></span>

<span data-ttu-id="e8485-295">Pour afficher les fichiers dans un déploiement d’application web Azure App Service, utilisez le [service Kudu](https://github.com/projectkudu/kudu/wiki/Accessing-the-kudu-service).</span><span class="sxs-lookup"><span data-stu-id="e8485-295">To view the files in an Azure App Service web app deployment, use the [Kudu service](https://github.com/projectkudu/kudu/wiki/Accessing-the-kudu-service).</span></span> <span data-ttu-id="e8485-296">Ajoutez le jeton `scm` au nom de l’application web.</span><span class="sxs-lookup"><span data-stu-id="e8485-296">Append the `scm` token to the web app name.</span></span> <span data-ttu-id="e8485-297">Par exemple :</span><span class="sxs-lookup"><span data-stu-id="e8485-297">For example:</span></span>

| <span data-ttu-id="e8485-298">URL</span><span class="sxs-lookup"><span data-stu-id="e8485-298">URL</span></span>                                    | <span data-ttu-id="e8485-299">Résultat</span><span class="sxs-lookup"><span data-stu-id="e8485-299">Result</span></span>       |
| -------------------------------------- | ------------ |
| `http://mysite.azurewebsites.net/`     | <span data-ttu-id="e8485-300">Application web</span><span class="sxs-lookup"><span data-stu-id="e8485-300">Web App</span></span>      |
| `http://mysite.scm.azurewebsites.net/` | <span data-ttu-id="e8485-301">Service Kudu</span><span class="sxs-lookup"><span data-stu-id="e8485-301">Kudu service</span></span> |

<span data-ttu-id="e8485-302">Sélectionnez l’élément de menu [Console de débogage](https://github.com/projectkudu/kudu/wiki/Kudu-console) pour afficher, modifier, supprimer ou ajouter des fichiers.</span><span class="sxs-lookup"><span data-stu-id="e8485-302">Select the [Debug Console](https://github.com/projectkudu/kudu/wiki/Kudu-console) menu item to view, edit, delete, or add files.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="e8485-303">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="e8485-303">Additional resources</span></span>

* <span data-ttu-id="e8485-304">[Web Deploy](https://www.iis.net/downloads/microsoft/web-deploy) (MSDeploy) simplifie le déploiement des applications web et des sites web sur les serveurs IIS.</span><span class="sxs-lookup"><span data-stu-id="e8485-304">[Web Deploy](https://www.iis.net/downloads/microsoft/web-deploy) (MSDeploy) simplifies deployment of web apps and websites to IIS servers.</span></span>
* <span data-ttu-id="e8485-305">[https://github.com/aspnet/websdk](https://github.com/aspnet/websdk/issues) : soumettez vos problèmes et demandez des fonctionnalités pour le déploiement.</span><span class="sxs-lookup"><span data-stu-id="e8485-305">[https://github.com/aspnet/websdk](https://github.com/aspnet/websdk/issues): File issues and request features for deployment.</span></span>
* [<span data-ttu-id="e8485-306">Publier une application web ASP.NET sur une machine virtuelle Azure à partir de Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e8485-306">Publish an ASP.NET Web App to an Azure VM from Visual Studio</span></span>](/azure/virtual-machines/windows/publish-web-app-from-visual-studio)
* <xref:host-and-deploy/iis/transform-webconfig>
