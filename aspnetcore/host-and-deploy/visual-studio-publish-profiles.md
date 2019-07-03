---
title: Profils de publication Visual Studio pour le déploiement d’applications ASP.NET Core
author: rick-anderson
description: Découvrez comment créer des profils de publication dans Visual Studio et les utiliser pour gérer les déploiements d’applications ASP.NET Core sur différentes cibles.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 06/18/2019
uid: host-and-deploy/visual-studio-publish-profiles
ms.openlocfilehash: ac243a3898553b2e14a6c15d311afaf62f112a24
ms.sourcegitcommit: a1283d486ac1dcedfc7ea302e1cc882833e2c515
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/18/2019
ms.locfileid: "67207812"
---
# <a name="visual-studio-publish-profiles-for-aspnet-core-app-deployment"></a><span data-ttu-id="592e2-103">Profils de publication Visual Studio pour le déploiement d’applications ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="592e2-103">Visual Studio publish profiles for ASP.NET Core app deployment</span></span>

<span data-ttu-id="592e2-104">De [Sayed Ibrahim Hashimi](https://github.com/sayedihashimi) et [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="592e2-104">By [Sayed Ibrahim Hashimi](https://github.com/sayedihashimi) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="592e2-105">Ce document est axé sur l’utilisation de Visual Studio 2017 ou d’une version ultérieure pour créer et utiliser des profils de publication.</span><span class="sxs-lookup"><span data-stu-id="592e2-105">This document focuses on using Visual Studio 2017 or later to create and use publish profiles.</span></span> <span data-ttu-id="592e2-106">Les profils de publication créés avec Visual Studio peuvent être exécutés à partir de MSBuild et Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="592e2-106">The publish profiles created with Visual Studio can be run from MSBuild and Visual Studio.</span></span> <span data-ttu-id="592e2-107">Consultez [Publier une application web ASP.NET Core sur Azure App Service à l’aide de Visual Studio](xref:tutorials/publish-to-azure-webapp-using-vs) pour obtenir des instructions de publication sur Azure.</span><span class="sxs-lookup"><span data-stu-id="592e2-107">See [Publish an ASP.NET Core web app to Azure App Service using Visual Studio](xref:tutorials/publish-to-azure-webapp-using-vs) for instructions on publishing to Azure.</span></span>

<span data-ttu-id="592e2-108">La commande `dotnet new mvc` produit un fichier de projet contenant l’élément `<Project>` de niveau supérieur suivant :</span><span class="sxs-lookup"><span data-stu-id="592e2-108">The `dotnet new mvc` command produces a project file containing the following top-level `<Project>` element:</span></span>

```xml
<Project Sdk="Microsoft.NET.Sdk.Web">
    <!-- omitted for brevity -->
</Project>
```

<span data-ttu-id="592e2-109">L’attribut `Sdk` de l’élément `<Project>` précédent importe les [propriétés](/visualstudio/msbuild/msbuild-properties) et [cibles](/visualstudio/msbuild/msbuild-targets) de MSBuild à partir de *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.props* et *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.targets*, respectivement.</span><span class="sxs-lookup"><span data-stu-id="592e2-109">The preceding `<Project>` element's `Sdk` attribute imports the MSBuild [properties](/visualstudio/msbuild/msbuild-properties) and [targets](/visualstudio/msbuild/msbuild-targets) from *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.props* and *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.targets*, respectively.</span></span> <span data-ttu-id="592e2-110">L’emplacement par défaut de `$(MSBuildSDKsPath)` (avec Visual Studio 2019 Enterprise) est le dossier *%programfiles(x86)%\Microsoft Visual Studio\2019\Enterprise\MSBuild\Sdks*.</span><span class="sxs-lookup"><span data-stu-id="592e2-110">The default location for `$(MSBuildSDKsPath)` (with Visual Studio 2019 Enterprise) is the *%programfiles(x86)%\Microsoft Visual Studio\2019\Enterprise\MSBuild\Sdks* folder.</span></span>

<span data-ttu-id="592e2-111">`Microsoft.NET.Sdk.Web` (kit de développement web) dépend d’autres kits de développement logiciel, dont `Microsoft.NET.Sdk` (SDK .NET Core) et `Microsoft.NET.Sdk.Razor` ([SDK Razor](xref:razor-pages/sdk)).</span><span class="sxs-lookup"><span data-stu-id="592e2-111">`Microsoft.NET.Sdk.Web` (Web SDK) depends on other SDKs, including `Microsoft.NET.Sdk` (.NET Core SDK) and `Microsoft.NET.Sdk.Razor` ([Razor SDK](xref:razor-pages/sdk)).</span></span> <span data-ttu-id="592e2-112">Les propriétés et cibles MSBuild associées à chaque kit de développement logiciel sont importées.</span><span class="sxs-lookup"><span data-stu-id="592e2-112">The MSBuild properties and targets associated with each dependent SDK are imported.</span></span> <span data-ttu-id="592e2-113">Les cibles de publication importent le bon ensemble de cibles en fonction de la méthode de publication utilisée.</span><span class="sxs-lookup"><span data-stu-id="592e2-113">Publish targets import the appropriate set of targets based on the publish method used.</span></span>

<span data-ttu-id="592e2-114">Quand MSBuild ou Visual Studio charge un projet, les actions principales suivantes se produisent :</span><span class="sxs-lookup"><span data-stu-id="592e2-114">When MSBuild or Visual Studio loads a project, the following high-level actions occur:</span></span>

* <span data-ttu-id="592e2-115">Génération du projet</span><span class="sxs-lookup"><span data-stu-id="592e2-115">Build project</span></span>
* <span data-ttu-id="592e2-116">Calcul des fichiers à publier</span><span class="sxs-lookup"><span data-stu-id="592e2-116">Compute files to publish</span></span>
* <span data-ttu-id="592e2-117">Publication des fichiers sur la destination</span><span class="sxs-lookup"><span data-stu-id="592e2-117">Publish files to destination</span></span>

## <a name="compute-project-items"></a><span data-ttu-id="592e2-118">Calcul des éléments du projet</span><span class="sxs-lookup"><span data-stu-id="592e2-118">Compute project items</span></span>

<span data-ttu-id="592e2-119">Quand le projet est chargé, les [éléments du projet MSBuild](/visualstudio/msbuild/common-msbuild-project-items) (fichiers) sont calculés.</span><span class="sxs-lookup"><span data-stu-id="592e2-119">When the project is loaded, the [MSBuild project items](/visualstudio/msbuild/common-msbuild-project-items) (files) are computed.</span></span> <span data-ttu-id="592e2-120">Le type d’élément détermine la façon dont le fichier est traité.</span><span class="sxs-lookup"><span data-stu-id="592e2-120">The item type determines how the file is processed.</span></span> <span data-ttu-id="592e2-121">Par défaut, les fichiers *.cs* sont inclus dans la liste d’éléments `Compile`.</span><span class="sxs-lookup"><span data-stu-id="592e2-121">By default, *.cs* files are included in the `Compile` item list.</span></span> <span data-ttu-id="592e2-122">Les fichiers dans la liste d’éléments `Compile` sont compilés.</span><span class="sxs-lookup"><span data-stu-id="592e2-122">Files in the `Compile` item list are compiled.</span></span>

<span data-ttu-id="592e2-123">La liste d’éléments `Content` contient des fichiers qui sont publiés en plus des sorties de génération.</span><span class="sxs-lookup"><span data-stu-id="592e2-123">The `Content` item list contains files that are published in addition to the build outputs.</span></span> <span data-ttu-id="592e2-124">Par défaut, les fichiers qui correspondent aux modèles `wwwroot\**`, `**\*.config` et `**\*.json` sont inclus dans l’élément `Content`.</span><span class="sxs-lookup"><span data-stu-id="592e2-124">By default, files matching the patterns `wwwroot\**`, `**\*.config`, and `**\*.json` are included in the `Content` item list.</span></span> <span data-ttu-id="592e2-125">Par exemple, le [modèle d’utilisation des caractères génériques](https://gruntjs.com/configuring-tasks#globbing-patterns) `wwwroot\**` correspond à tous les fichiers dans le dossier *wwwroot* **et** les sous-dossiers.</span><span class="sxs-lookup"><span data-stu-id="592e2-125">For example, the `wwwroot\**` [globbing pattern](https://gruntjs.com/configuring-tasks#globbing-patterns) matches all files in the *wwwroot* folder **and** its subfolders.</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="592e2-126">Le Kit de développement Web importe le [SDK Razor](xref:razor-pages/sdk).</span><span class="sxs-lookup"><span data-stu-id="592e2-126">The Web SDK imports the [Razor SDK](xref:razor-pages/sdk).</span></span> <span data-ttu-id="592e2-127">Par conséquent, les fichiers correspondant aux modèles `**\*.cshtml` et `**\*.razor` sont également inclus dans la liste d’éléments `Content`.</span><span class="sxs-lookup"><span data-stu-id="592e2-127">As a result, files matching the patterns `**\*.cshtml` and `**\*.razor` are also included in the `Content` item list.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1 <= aspnetcore-2.2"

<span data-ttu-id="592e2-128">Le Kit de développement Web importe le [SDK Razor](xref:razor-pages/sdk).</span><span class="sxs-lookup"><span data-stu-id="592e2-128">The Web SDK imports the [Razor SDK](xref:razor-pages/sdk).</span></span> <span data-ttu-id="592e2-129">Par conséquent, les fichiers correspondant au modèle `**\*.cshtml` sont également inclus dans la liste d’éléments `Content`.</span><span class="sxs-lookup"><span data-stu-id="592e2-129">As a result, files matching the `**\*.cshtml` pattern are also included in the `Content` item list.</span></span>

::: moniker-end

<span data-ttu-id="592e2-130">Pour ajouter explicitement un fichier à la liste de publication, ajoutez-le directement au fichier *.csproj* comme indiqué dans la section [Inclure des fichiers](#include-files).</span><span class="sxs-lookup"><span data-stu-id="592e2-130">To explicitly add a file to the publish list, add the file directly in the *.csproj* file as shown in the [Include Files](#include-files) section.</span></span>

<span data-ttu-id="592e2-131">Quand vous sélectionnez le bouton **Publier** dans Visual Studio ou quand vous publiez à partir de la ligne de commande :</span><span class="sxs-lookup"><span data-stu-id="592e2-131">When selecting the **Publish** button in Visual Studio or when publishing from the command line:</span></span>

* <span data-ttu-id="592e2-132">Les éléments/propriétés sont calculés (il s’agit des fichiers nécessaires à la génération).</span><span class="sxs-lookup"><span data-stu-id="592e2-132">The properties/items are computed (the files that are needed to build).</span></span>
* <span data-ttu-id="592e2-133">**Visual Studio uniquement** : Les packages NuGet sont restaurés.</span><span class="sxs-lookup"><span data-stu-id="592e2-133">**Visual Studio only**: NuGet packages are restored.</span></span> <span data-ttu-id="592e2-134">(La restauration doit être explicite par l’utilisateur sur l’interface CLI.)</span><span class="sxs-lookup"><span data-stu-id="592e2-134">(Restore needs to be explicit by the user on the CLI.)</span></span>
* <span data-ttu-id="592e2-135">Le projet est généré.</span><span class="sxs-lookup"><span data-stu-id="592e2-135">The project builds.</span></span>
* <span data-ttu-id="592e2-136">Les éléments de publication sont calculés (il s’agit des fichiers nécessaires à la publication).</span><span class="sxs-lookup"><span data-stu-id="592e2-136">The publish items are computed (the files that are needed to publish).</span></span>
* <span data-ttu-id="592e2-137">Le projet est publié (les fichiers calculés sont copiés sur la destination de publication).</span><span class="sxs-lookup"><span data-stu-id="592e2-137">The project is published (the computed files are copied to the publish destination).</span></span>

<span data-ttu-id="592e2-138">Quand un projet ASP.NET Core référence `Microsoft.NET.Sdk.Web` dans le fichier projet, un fichier *app_offline.htm* est placé à la racine du répertoire des applications web.</span><span class="sxs-lookup"><span data-stu-id="592e2-138">When an ASP.NET Core project references `Microsoft.NET.Sdk.Web` in the project file, an *app_offline.htm* file is placed at the root of the web app directory.</span></span> <span data-ttu-id="592e2-139">Quand le fichier est présent, le module ASP.NET Core ferme l’application normalement et sert le fichier *app_offline.htm* pendant le déploiement.</span><span class="sxs-lookup"><span data-stu-id="592e2-139">When the file is present, the ASP.NET Core Module gracefully shuts down the app and serves the *app_offline.htm* file during the deployment.</span></span> <span data-ttu-id="592e2-140">Pour plus d’informations, consultez les [Informations de référence sur la configuration du module ASP.NET Core](xref:host-and-deploy/aspnet-core-module#app_offlinehtm).</span><span class="sxs-lookup"><span data-stu-id="592e2-140">For more information, see the [ASP.NET Core Module configuration reference](xref:host-and-deploy/aspnet-core-module#app_offlinehtm).</span></span>

## <a name="basic-command-line-publishing"></a><span data-ttu-id="592e2-141">Publication de base à partir d’une ligne de commande</span><span class="sxs-lookup"><span data-stu-id="592e2-141">Basic command-line publishing</span></span>

<span data-ttu-id="592e2-142">La publication à partir d’une ligne de commande fonctionne sur toutes les plateformes .NET Core prises en charge et ne nécessite pas Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="592e2-142">Command-line publishing works on all .NET Core-supported platforms and doesn't require Visual Studio.</span></span> <span data-ttu-id="592e2-143">Dans les exemples suivants, la commande [dotnet publish](/dotnet/core/tools/dotnet-publish) est exécutée à partir du répertoire du projet (qui contient le fichier *.csproj*).</span><span class="sxs-lookup"><span data-stu-id="592e2-143">In the following samples, the [dotnet publish](/dotnet/core/tools/dotnet-publish) command is run from the project directory (which contains the *.csproj* file).</span></span> <span data-ttu-id="592e2-144">Si le dossier actuel n’est pas le dossier du projet, transmettez explicitement le chemin du fichier projet.</span><span class="sxs-lookup"><span data-stu-id="592e2-144">If not in the project folder, explicitly pass in the project file path.</span></span> <span data-ttu-id="592e2-145">Par exemple :</span><span class="sxs-lookup"><span data-stu-id="592e2-145">For example:</span></span>

```console
dotnet publish C:\Webs\Web1
```

<span data-ttu-id="592e2-146">Exécutez les commandes suivantes pour créer et publier une application web :</span><span class="sxs-lookup"><span data-stu-id="592e2-146">Run the following commands to create and publish a web app:</span></span>

```console
dotnet new mvc
dotnet publish
```

<span data-ttu-id="592e2-147">La commande [dotnet publish](/dotnet/core/tools/dotnet-publish) produit un résultat similaire au suivant :</span><span class="sxs-lookup"><span data-stu-id="592e2-147">The [dotnet publish](/dotnet/core/tools/dotnet-publish) command produces output similar to the following:</span></span>

```console
C:\Webs\Web1>dotnet publish
Microsoft (R) Build Engine version {version} for .NET Core
Copyright (C) Microsoft Corporation. All rights reserved.

  Restore completed in 36.81 ms for C:\Webs\Web1\Web1.csproj.
  Web1 -> C:\Webs\Web1\bin\Debug\netcoreapp{X.Y}\Web1.dll
  Web1 -> C:\Webs\Web1\bin\Debug\netcoreapp{X.Y}\Web1.Views.dll
  Web1 -> C:\Webs\Web1\bin\Debug\netcoreapp{X.Y}\publish\
```

<span data-ttu-id="592e2-148">Le dossier de publication par défaut est `bin\$(Configuration)\netcoreapp<version>\publish`.</span><span class="sxs-lookup"><span data-stu-id="592e2-148">The default publish folder is `bin\$(Configuration)\netcoreapp<version>\publish`.</span></span> <span data-ttu-id="592e2-149">La valeur par défaut de `$(Configuration)` est *Debug*.</span><span class="sxs-lookup"><span data-stu-id="592e2-149">The default for `$(Configuration)` is *Debug*.</span></span> <span data-ttu-id="592e2-150">Dans l’exemple précédent, `<TargetFramework>` a pour valeur `netcoreapp{X.Y}`.</span><span class="sxs-lookup"><span data-stu-id="592e2-150">In the preceding sample, the `<TargetFramework>` is `netcoreapp{X.Y}`.</span></span>

<span data-ttu-id="592e2-151">`dotnet publish -h` affiche des informations d’aide relatives à la publication.</span><span class="sxs-lookup"><span data-stu-id="592e2-151">`dotnet publish -h` displays help information for publish.</span></span>

<span data-ttu-id="592e2-152">La commande suivante spécifie une build `Release` et le répertoire de publication :</span><span class="sxs-lookup"><span data-stu-id="592e2-152">The following command specifies a `Release` build and the publishing directory:</span></span>

```console
dotnet publish -c Release -o C:\MyWebs\test
```

<span data-ttu-id="592e2-153">La commande [dotnet publish](/dotnet/core/tools/dotnet-publish) appelle MSBuild, qui appelle la cible `Publish`.</span><span class="sxs-lookup"><span data-stu-id="592e2-153">The [dotnet publish](/dotnet/core/tools/dotnet-publish) command calls MSBuild, which invokes the `Publish` target.</span></span> <span data-ttu-id="592e2-154">Les paramètres passés à `dotnet publish` sont passés à MSBuild.</span><span class="sxs-lookup"><span data-stu-id="592e2-154">Any parameters passed to `dotnet publish` are passed to MSBuild.</span></span> <span data-ttu-id="592e2-155">Le paramètre `-c` est mappé à la propriété MSBuild `Configuration`.</span><span class="sxs-lookup"><span data-stu-id="592e2-155">The `-c` parameter maps to the `Configuration` MSBuild property.</span></span> <span data-ttu-id="592e2-156">Le paramètre `-o` est mappé à `OutputPath`.</span><span class="sxs-lookup"><span data-stu-id="592e2-156">The `-o` parameter maps to `OutputPath`.</span></span>

<span data-ttu-id="592e2-157">Les propriétés MSBuild peuvent être passées à l’aide de l’un des formats suivants :</span><span class="sxs-lookup"><span data-stu-id="592e2-157">MSBuild properties can be passed using either of the following formats:</span></span>

* `p:<NAME>=<VALUE>`
* `/p:<NAME>=<VALUE>`

<span data-ttu-id="592e2-158">La commande suivante publie une build `Release` sur un partage réseau :</span><span class="sxs-lookup"><span data-stu-id="592e2-158">The following command publishes a `Release` build to a network share:</span></span>

`dotnet publish -c Release /p:PublishDir=//r8/release/AdminWeb`

<span data-ttu-id="592e2-159">Le partage réseau est spécifié avec des barres obliques ( *//r8/* ) et fonctionne sur toutes les plateformes .NET Core prises en charge.</span><span class="sxs-lookup"><span data-stu-id="592e2-159">The network share is specified with forward slashes (*//r8/*) and works on all .NET Core supported platforms.</span></span>

<span data-ttu-id="592e2-160">Vérifiez que l’application publiée pour le déploiement n’est pas en cours d’exécution.</span><span class="sxs-lookup"><span data-stu-id="592e2-160">Confirm that the published app for deployment isn't running.</span></span> <span data-ttu-id="592e2-161">Les fichiers dans le dossier *publish* sont verrouillés quand l’application est en cours d’exécution.</span><span class="sxs-lookup"><span data-stu-id="592e2-161">Files in the *publish* folder are locked when the app is running.</span></span> <span data-ttu-id="592e2-162">Le déploiement ne peut pas se produire car les fichiers verrouillés ne peuvent pas être copiés.</span><span class="sxs-lookup"><span data-stu-id="592e2-162">Deployment can't occur because locked files can't be copied.</span></span>

## <a name="publish-profiles"></a><span data-ttu-id="592e2-163">Profils de publication</span><span class="sxs-lookup"><span data-stu-id="592e2-163">Publish profiles</span></span>

<span data-ttu-id="592e2-164">Cette section utilise Visual Studio 2017 ou une version ultérieure pour créer un profil de publication.</span><span class="sxs-lookup"><span data-stu-id="592e2-164">This section uses Visual Studio 2017 or later to create a publishing profile.</span></span> <span data-ttu-id="592e2-165">Une fois le profil créé, la publication à partir de Visual Studio ou de la ligne de commande est disponible.</span><span class="sxs-lookup"><span data-stu-id="592e2-165">Once the profile is created, publishing from Visual Studio or the command line is available.</span></span>

<span data-ttu-id="592e2-166">Les profils de publication peuvent simplifier le processus de publication, et n’importe quel nombre de profils peut exister.</span><span class="sxs-lookup"><span data-stu-id="592e2-166">Publish profiles can simplify the publishing process, and any number of profiles can exist.</span></span> <span data-ttu-id="592e2-167">Créez un profil de publication dans Visual Studio en choisissant l’une des méthodes suivantes :</span><span class="sxs-lookup"><span data-stu-id="592e2-167">Create a publish profile in Visual Studio by choosing one of the following paths:</span></span>

* <span data-ttu-id="592e2-168">Dans **l’Explorateur de solutions**, cliquez avec le bouton droit sur le projet, puis sélectionnez **Publier**.</span><span class="sxs-lookup"><span data-stu-id="592e2-168">Right-click the project in **Solution Explorer** and select **Publish**.</span></span>
* <span data-ttu-id="592e2-169">Sélectionner **Publier {NOM DU PROJET}** dans le menu **Générer**.</span><span class="sxs-lookup"><span data-stu-id="592e2-169">Select **Publish {PROJECT NAME}** from the **Build** menu.</span></span>

<span data-ttu-id="592e2-170">L’onglet **Publier** de la page de capacités d’application s’affiche.</span><span class="sxs-lookup"><span data-stu-id="592e2-170">The **Publish** tab of the app capacities page is displayed.</span></span> <span data-ttu-id="592e2-171">Si le projet n’a pas de profil de publication, la page suivante s’affiche :</span><span class="sxs-lookup"><span data-stu-id="592e2-171">If the project lacks a publish profile, the following page is displayed:</span></span>

![Onglet Publier de la page de capacités d’application](visual-studio-publish-profiles/_static/az.png)

<span data-ttu-id="592e2-173">Quand **Dossier** est sélectionné, spécifiez un chemin de dossier pour stocker les ressources publiées.</span><span class="sxs-lookup"><span data-stu-id="592e2-173">When **Folder** is selected, specify a folder path to store the published assets.</span></span> <span data-ttu-id="592e2-174">Le dossier par défaut est *bin\Release\PublishOutput*.</span><span class="sxs-lookup"><span data-stu-id="592e2-174">The default folder is *bin\Release\PublishOutput*.</span></span> <span data-ttu-id="592e2-175">Cliquez sur le bouton **Créer un profil** pour terminer.</span><span class="sxs-lookup"><span data-stu-id="592e2-175">Click the **Create Profile** button to finish.</span></span>

<span data-ttu-id="592e2-176">Une fois créé un profil de publication, l’onglet **Publier** change.</span><span class="sxs-lookup"><span data-stu-id="592e2-176">Once a publish profile is created, the **Publish** tab changes.</span></span> <span data-ttu-id="592e2-177">Le profil créé apparaît dans une liste déroulante.</span><span class="sxs-lookup"><span data-stu-id="592e2-177">The newly created profile appears in a drop-down list.</span></span> <span data-ttu-id="592e2-178">Cliquez sur **Créer un profil** pour créer un autre profil.</span><span class="sxs-lookup"><span data-stu-id="592e2-178">Click **Create new profile** to create another new profile.</span></span>

![Onglet Publier de la page de capacités d’application montrant FolderProfile](visual-studio-publish-profiles/_static/create_new.png)

<span data-ttu-id="592e2-180">L’Assistant Publication prend en charge les cibles de publication suivantes :</span><span class="sxs-lookup"><span data-stu-id="592e2-180">The Publish wizard supports the following publish targets:</span></span>

* <span data-ttu-id="592e2-181">Azure App Service</span><span class="sxs-lookup"><span data-stu-id="592e2-181">Azure App Service</span></span>
* <span data-ttu-id="592e2-182">Machines virtuelles Azure</span><span class="sxs-lookup"><span data-stu-id="592e2-182">Azure Virtual Machines</span></span>
* <span data-ttu-id="592e2-183">IIS, FTP, etc. (pour n’importe quel serveur web)</span><span class="sxs-lookup"><span data-stu-id="592e2-183">IIS, FTP, etc. (for any web server)</span></span>
* <span data-ttu-id="592e2-184">Dossier</span><span class="sxs-lookup"><span data-stu-id="592e2-184">Folder</span></span>
* <span data-ttu-id="592e2-185">Profil d’importation</span><span class="sxs-lookup"><span data-stu-id="592e2-185">Import Profile</span></span>

<span data-ttu-id="592e2-186">Consultez [Quelles options de publication choisir ?](/visualstudio/ide/not-in-toc/web-publish-options) pour plus d’informations.</span><span class="sxs-lookup"><span data-stu-id="592e2-186">For more information, see [What publishing options are right for me](/visualstudio/ide/not-in-toc/web-publish-options).</span></span>

<span data-ttu-id="592e2-187">Quand vous créez un profil de publication avec Visual Studio, un fichier MSBuild *Properties/PublishProfiles/{NOM DU PROFIL}.pubxml* est créé.</span><span class="sxs-lookup"><span data-stu-id="592e2-187">When creating a publish profile with Visual Studio, a *Properties/PublishProfiles/{PROFILE NAME}.pubxml* MSBuild file is created.</span></span> <span data-ttu-id="592e2-188">Le fichier *.pubxml* est un fichier MSBuild qui contient des paramètres de configuration de publication.</span><span class="sxs-lookup"><span data-stu-id="592e2-188">The *.pubxml* file is a MSBuild file and contains publish configuration settings.</span></span> <span data-ttu-id="592e2-189">Vous pouvez changer ce fichier pour personnaliser le processus de génération et de publication.</span><span class="sxs-lookup"><span data-stu-id="592e2-189">This file can be changed to customize the build and publish process.</span></span> <span data-ttu-id="592e2-190">Ce fichier est lu par le processus de publication.</span><span class="sxs-lookup"><span data-stu-id="592e2-190">This file is read by the publishing process.</span></span> <span data-ttu-id="592e2-191">`<LastUsedBuildConfiguration>` est spéciale, car il s’agit d’une propriété globale qui ne doit être dans aucun fichier importé dans la build.</span><span class="sxs-lookup"><span data-stu-id="592e2-191">`<LastUsedBuildConfiguration>` is special because it's a global property and shouldn't be in any file that's imported in the build.</span></span> <span data-ttu-id="592e2-192">Pour plus d’informations, consultez [MSBuild: how to set the configuration property](http://sedodream.com/2012/10/27/MSBuildHowToSetTheConfigurationProperty.aspx) (Comment définir la propriété de configuration).</span><span class="sxs-lookup"><span data-stu-id="592e2-192">See [MSBuild: how to set the configuration property](http://sedodream.com/2012/10/27/MSBuildHowToSetTheConfigurationProperty.aspx) for more information.</span></span>

<span data-ttu-id="592e2-193">Dans le cas d’une publication sur une cible Azure, le fichier *.pubxml* contient l’identificateur de votre abonnement Azure.</span><span class="sxs-lookup"><span data-stu-id="592e2-193">When publishing to an Azure target, the *.pubxml* file contains your Azure subscription identifier.</span></span> <span data-ttu-id="592e2-194">Avec ce type de cible, nous vous déconseillons d’ajouter ce fichier au contrôle de code source.</span><span class="sxs-lookup"><span data-stu-id="592e2-194">With that target type, adding this file to source control is discouraged.</span></span> <span data-ttu-id="592e2-195">Dans le cas d’une publication sur une cible non-Azure, nous vous recommandons d’archiver le fichier *.pubxml*.</span><span class="sxs-lookup"><span data-stu-id="592e2-195">When publishing to a non-Azure target, it's safe to check in the *.pubxml* file.</span></span>

<span data-ttu-id="592e2-196">Les informations sensibles (telles que le mot de passe de publication) sont chiffrées par utilisateur/machine.</span><span class="sxs-lookup"><span data-stu-id="592e2-196">Sensitive information (like the publish password) is encrypted on a per user/machine level.</span></span> <span data-ttu-id="592e2-197">Elles sont stockées dans le fichier *Properties/PublishProfiles/{NOM DU PROFIL}.pubxml.user*.</span><span class="sxs-lookup"><span data-stu-id="592e2-197">It's stored in the *Properties/PublishProfiles/{PROFILE NAME}.pubxml.user* file.</span></span> <span data-ttu-id="592e2-198">Ce fichier pouvant stocker des informations sensibles, il ne doit pas être archivé dans le contrôle de code source.</span><span class="sxs-lookup"><span data-stu-id="592e2-198">Because this file can store sensitive information, it shouldn't be checked into source control.</span></span>

<span data-ttu-id="592e2-199">Pour obtenir une vue d’ensemble expliquant comment publier une application web sur ASP.NET Core, consultez [Héberger et déployer](xref:host-and-deploy/index).</span><span class="sxs-lookup"><span data-stu-id="592e2-199">For an overview of how to publish a web app on ASP.NET Core, see [Host and deploy](xref:host-and-deploy/index).</span></span> <span data-ttu-id="592e2-200">Les tâches et cibles MSBuild nécessaires pour publier une application ASP.NET Core sont disponibles au format open source dans le [référentiel aspnet/websdk](https://github.com/aspnet/websdk).</span><span class="sxs-lookup"><span data-stu-id="592e2-200">The MSBuild tasks and targets necessary to publish an ASP.NET Core app are open-source at the [aspnet/websdk repository](https://github.com/aspnet/websdk).</span></span>

<span data-ttu-id="592e2-201">`dotnet publish` peut utiliser les profils de publication de dossier, MSDeploy et [Kudu](https://github.com/projectkudu/kudu/wiki) :</span><span class="sxs-lookup"><span data-stu-id="592e2-201">`dotnet publish` can use folder, MSDeploy, and [Kudu](https://github.com/projectkudu/kudu/wiki) publish profiles:</span></span>

<span data-ttu-id="592e2-202">Dossier (fonctionne sur plusieurs plateformes) :</span><span class="sxs-lookup"><span data-stu-id="592e2-202">Folder (works cross-platform):</span></span>

```console
dotnet publish WebApplication.csproj /p:PublishProfile=<FolderProfileName>
```

<span data-ttu-id="592e2-203">MSDeploy (ne fonctionne que sur Windows car MSDeploy n’est pas multiplateforme) :</span><span class="sxs-lookup"><span data-stu-id="592e2-203">MSDeploy (currently this only works in Windows since MSDeploy isn't cross-platform):</span></span>

```console
dotnet publish WebApplication.csproj /p:PublishProfile=<MsDeployProfileName> /p:Password=<DeploymentPassword>
```

<span data-ttu-id="592e2-204">Package MSDeploy (ne fonctionne que sur Windows car MSDeploy n’est pas multiplateforme) :</span><span class="sxs-lookup"><span data-stu-id="592e2-204">MSDeploy package (currently this only works in Windows since MSDeploy isn't cross-platform):</span></span>

```console
dotnet publish WebApplication.csproj /p:PublishProfile=<MsDeployPackageProfileName>
```

<span data-ttu-id="592e2-205">Dans les exemples précédents, ne transmettez pas `deployonbuild` à `dotnet publish`.</span><span class="sxs-lookup"><span data-stu-id="592e2-205">In the preceding examples, don't pass `deployonbuild` to `dotnet publish`.</span></span>

<span data-ttu-id="592e2-206">Pour plus d’informations, consultez [Microsoft.NET.Sdk.Publish](https://github.com/aspnet/websdk#microsoftnetsdkpublish).</span><span class="sxs-lookup"><span data-stu-id="592e2-206">For more information, see [Microsoft.NET.Sdk.Publish](https://github.com/aspnet/websdk#microsoftnetsdkpublish).</span></span>

<span data-ttu-id="592e2-207">`dotnet publish` prend en charge les API Kudu pour publier sur Azure à partir de n’importe quelle plateforme.</span><span class="sxs-lookup"><span data-stu-id="592e2-207">`dotnet publish` supports Kudu APIs to publish to Azure from any platform.</span></span> <span data-ttu-id="592e2-208">La publication Visual Studio prend en charge les API Kudu, mais avec la prise en charge par WebSDK pour la publication multiplateforme sur Azure.</span><span class="sxs-lookup"><span data-stu-id="592e2-208">Visual Studio publish supports the Kudu APIs, but it's supported by WebSDK for cross-platform publish to Azure.</span></span>

<span data-ttu-id="592e2-209">Ajoutez un profil de publication au dossier *Properties/PublishProfiles* avec le contenu suivant :</span><span class="sxs-lookup"><span data-stu-id="592e2-209">Add a publish profile to the *Properties/PublishProfiles* folder with the following content:</span></span>

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

<span data-ttu-id="592e2-210">Exécutez la commande suivante pour compresser le contenu de la publication et le publier sur Azure à l’aide des API Kudu :</span><span class="sxs-lookup"><span data-stu-id="592e2-210">Run the following command to zip up the publish contents and publish it to Azure using the Kudu APIs:</span></span>

```console
dotnet publish /p:PublishProfile=Azure /p:Configuration=Release
```

<span data-ttu-id="592e2-211">Définissez les propriétés MSBuild suivantes lors de l’utilisation d’un profil de publication :</span><span class="sxs-lookup"><span data-stu-id="592e2-211">Set the following MSBuild properties when using a publish profile:</span></span>

* `DeployOnBuild=true`
* `PublishProfile={PUBLISH PROFILE}`

<span data-ttu-id="592e2-212">Pendant la publication avec un profil nommé *FolderProfile*, l’une des commandes ci-dessous peut être exécutée :</span><span class="sxs-lookup"><span data-stu-id="592e2-212">When publishing with a profile named *FolderProfile*, either of the commands below can be executed:</span></span>

* `dotnet build /p:DeployOnBuild=true /p:PublishProfile=FolderProfile`
* `msbuild      /p:DeployOnBuild=true /p:PublishProfile=FolderProfile`

<span data-ttu-id="592e2-213">Quand elle est appelée, la commande [dotnet build](/dotnet/core/tools/dotnet-build) appelle `msbuild` pour exécuter les processus de génération et de publication.</span><span class="sxs-lookup"><span data-stu-id="592e2-213">When invoking [dotnet build](/dotnet/core/tools/dotnet-build), it calls `msbuild` to run the build and publish process.</span></span> <span data-ttu-id="592e2-214">L’appel de `dotnet build` ou `msbuild` est équivalent au moment du passage d’un profil de dossier.</span><span class="sxs-lookup"><span data-stu-id="592e2-214">Calling either `dotnet build` or `msbuild` is equivalent when passing in a folder profile.</span></span> <span data-ttu-id="592e2-215">Quand vous appelez MSBuild directement sur Windows, la version du .NET Framework de MSBuild est utilisée.</span><span class="sxs-lookup"><span data-stu-id="592e2-215">When calling MSBuild directly on Windows, the .NET Framework version of MSBuild is used.</span></span> <span data-ttu-id="592e2-216">MSDeploy est actuellement limité aux ordinateurs Windows pour la publication.</span><span class="sxs-lookup"><span data-stu-id="592e2-216">MSDeploy is currently limited to Windows machines for publishing.</span></span> <span data-ttu-id="592e2-217">L’appel de `dotnet build` sur un profil autre qu’un profil de dossier entraîne l’appel de MSBuild, et MSBuild utilise MSDeploy sur les profils autres que les dossiers de profil.</span><span class="sxs-lookup"><span data-stu-id="592e2-217">Calling `dotnet build` on a non-folder profile invokes MSBuild, and MSBuild uses MSDeploy on non-folder profiles.</span></span> <span data-ttu-id="592e2-218">L’appel de `dotnet build` sur un profil autre qu’un profil de dossier entraîne l’appel de MSBuild (à l’aide de MSDeploy) et échoue (même en cas d’exécution sur une plateforme Windows).</span><span class="sxs-lookup"><span data-stu-id="592e2-218">Calling `dotnet build` on a non-folder profile invokes MSBuild (using MSDeploy) and results in a failure (even when running on a Windows platform).</span></span> <span data-ttu-id="592e2-219">Pour publier avec un profil autre qu’un profil de dossier, appelez MSBuild directement.</span><span class="sxs-lookup"><span data-stu-id="592e2-219">To publish with a non-folder profile, call MSBuild directly.</span></span>

<span data-ttu-id="592e2-220">Le profil de publication de dossier suivant a été créé avec Visual Studio et publie sur un partage réseau :</span><span class="sxs-lookup"><span data-stu-id="592e2-220">The following folder publish profile was created with Visual Studio and publishes to a network share:</span></span>

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

<span data-ttu-id="592e2-221">Dans l’exemple précédent, `<LastUsedBuildConfiguration>` est défini sur `Release`.</span><span class="sxs-lookup"><span data-stu-id="592e2-221">In the preceding example, `<LastUsedBuildConfiguration>` is set to `Release`.</span></span> <span data-ttu-id="592e2-222">Lors de la publication à partir de Visual Studio, la propriété de configuration `<LastUsedBuildConfiguration>` prend la valeur en vigueur lors du démarrage du processus de publication.</span><span class="sxs-lookup"><span data-stu-id="592e2-222">When publishing from Visual Studio, the `<LastUsedBuildConfiguration>` configuration property value is set using the value when the publish process is started.</span></span> <span data-ttu-id="592e2-223">La propriété de configuration `<LastUsedBuildConfiguration>` est spéciale et ne doit pas être substituée dans un fichier MSBuild importé.</span><span class="sxs-lookup"><span data-stu-id="592e2-223">The `<LastUsedBuildConfiguration>` configuration property is special and shouldn't be overridden in an imported MSBuild file.</span></span> <span data-ttu-id="592e2-224">Cette propriété peut être substituée à partir de la ligne de commande.</span><span class="sxs-lookup"><span data-stu-id="592e2-224">This property can be overridden from the command line.</span></span>

<span data-ttu-id="592e2-225">À l’aide de CLI .NET Core :</span><span class="sxs-lookup"><span data-stu-id="592e2-225">Using the .NET Core CLI:</span></span>

```console
dotnet build -c Release /p:DeployOnBuild=true /p:PublishProfile=FolderProfile
```

<span data-ttu-id="592e2-226">À l’aide de MSBuild :</span><span class="sxs-lookup"><span data-stu-id="592e2-226">Using MSBuild:</span></span>

```console
msbuild /p:Configuration=Release /p:DeployOnBuild=true /p:PublishProfile=FolderProfile
```

## <a name="publish-to-an-msdeploy-endpoint-from-the-command-line"></a><span data-ttu-id="592e2-227">Publier sur un point de terminaison MSDeploy à partir de la ligne de commande</span><span class="sxs-lookup"><span data-stu-id="592e2-227">Publish to an MSDeploy endpoint from the command line</span></span>

<span data-ttu-id="592e2-228">L’exemple suivant utilise une application web ASP.NET Core créée par Visual Studio et nommée *AzureWebApp*.</span><span class="sxs-lookup"><span data-stu-id="592e2-228">The following example uses an ASP.NET Core web app created by Visual Studio named *AzureWebApp*.</span></span> <span data-ttu-id="592e2-229">Un profil de publication Azure Apps est ajouté avec Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="592e2-229">An Azure Apps publish profile is added with Visual Studio.</span></span> <span data-ttu-id="592e2-230">Pour plus d’informations sur la création d’un profil, consultez la section [profils de publication](#publish-profiles).</span><span class="sxs-lookup"><span data-stu-id="592e2-230">For more information on how to create a profile, see the [Publish profiles](#publish-profiles) section.</span></span>

<span data-ttu-id="592e2-231">Pour déployer l’application à l’aide d’un profil de publication, exécutez la `msbuild` commande à partir d’une **invite de commandes développeur** Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="592e2-231">To deploy the app using a publish profile, execute the `msbuild` command from a Visual Studio **Developer Command Prompt**.</span></span> <span data-ttu-id="592e2-232">L’invite de commandes est disponible dans le dossier *Visual Studio* du menu **Démarrer** sur la barre des tâches Windows.</span><span class="sxs-lookup"><span data-stu-id="592e2-232">The command prompt is available in the *Visual Studio* folder of the **Start** menu on the Windows taskbar.</span></span> <span data-ttu-id="592e2-233">Pour en faciliter accès, vous pouvez ajouter à l’invite de commandes au menu **Outils** dans Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="592e2-233">For easier access, you can add the command prompt to the **Tools** menu in Visual Studio.</span></span> <span data-ttu-id="592e2-234">Pour plus d’informations, consultez [Invite de commandes développeur pour Visual Studio](/dotnet/framework/tools/developer-command-prompt-for-vs#run-the-command-prompt-from-inside-visual-studio).</span><span class="sxs-lookup"><span data-stu-id="592e2-234">For more information, see [Developer Command Prompt for Visual Studio](/dotnet/framework/tools/developer-command-prompt-for-vs#run-the-command-prompt-from-inside-visual-studio).</span></span>

<span data-ttu-id="592e2-235">MSBuild utilise la syntaxe de commande suivante :</span><span class="sxs-lookup"><span data-stu-id="592e2-235">MSBuild uses the following command syntax:</span></span>

```console
msbuild {PATH} 
    /p:DeployOnBuild=true 
    /p:PublishProfile={PROFILE} 
    /p:Username={USERNAME} 
    /p:Password={PASSWORD}
```

* <span data-ttu-id="592e2-236">{PATH} &ndash; Chemin d’accès vers le fichier de projet de l’application.</span><span class="sxs-lookup"><span data-stu-id="592e2-236">{PATH} &ndash; Path to the app's project file.</span></span>
* <span data-ttu-id="592e2-237">{PROFILE} &ndash; Nom du profil de publication.</span><span class="sxs-lookup"><span data-stu-id="592e2-237">{PROFILE} &ndash; Name of the publish profile.</span></span>
* <span data-ttu-id="592e2-238">{USERNAME} &ndash; Nom d’utilisateur MSDeploy.</span><span class="sxs-lookup"><span data-stu-id="592e2-238">{USERNAME} &ndash; MSDeploy username.</span></span> <span data-ttu-id="592e2-239">{USERNAME} figure dans le profil de publication.</span><span class="sxs-lookup"><span data-stu-id="592e2-239">The {USERNAME} can be found in the publish profile.</span></span>
* <span data-ttu-id="592e2-240">{PASSWORD} &ndash; Mot de passe MSDeploy.</span><span class="sxs-lookup"><span data-stu-id="592e2-240">{PASSWORD} &ndash; MSDeploy password.</span></span> <span data-ttu-id="592e2-241">Obtenez le {PASSWORD} à partir du fichier *{PROFILE}. PublishSettings*.</span><span class="sxs-lookup"><span data-stu-id="592e2-241">Obtain the {PASSWORD} from the *{PROFILE}.PublishSettings* file.</span></span> <span data-ttu-id="592e2-242">Téléchargez le fichier *.PublishSettings* à partir d’un des emplacements suivants :</span><span class="sxs-lookup"><span data-stu-id="592e2-242">Download the *.PublishSettings* file from either:</span></span>
  * <span data-ttu-id="592e2-243">Explorateur de solutions : Sélectionnez **Vue** > **Cloud Explorer**.</span><span class="sxs-lookup"><span data-stu-id="592e2-243">Solution Explorer: Select **View** > **Cloud Explorer**.</span></span> <span data-ttu-id="592e2-244">Connectez-vous avec votre abonnement Azure.</span><span class="sxs-lookup"><span data-stu-id="592e2-244">Connect with your Azure subscription.</span></span> <span data-ttu-id="592e2-245">Ouvrez **App Services**.</span><span class="sxs-lookup"><span data-stu-id="592e2-245">Open **App Services**.</span></span> <span data-ttu-id="592e2-246">Faites un clic droit sur l’application.</span><span class="sxs-lookup"><span data-stu-id="592e2-246">Right-click the app.</span></span> <span data-ttu-id="592e2-247">Sélectionnez **Télecharger un profil de publication**.</span><span class="sxs-lookup"><span data-stu-id="592e2-247">Select **Download Publish Profile**.</span></span>
  * <span data-ttu-id="592e2-248">Portail Azure : cliquez sur **Obtenir le profil de publication** dans le panneau **Vue d’ensemble** de l’application web.</span><span class="sxs-lookup"><span data-stu-id="592e2-248">Azure portal: Select **Get publish profile** in the web app's **Overview** panel.</span></span>

<span data-ttu-id="592e2-249">L’exemple suivant utilise un profil de publication nommé *AzureWebApp - Web Deploy*:</span><span class="sxs-lookup"><span data-stu-id="592e2-249">The following example uses a publish profile named *AzureWebApp - Web Deploy*:</span></span>

```console
msbuild "AzureWebApp.csproj" 
    /p:DeployOnBuild=true 
    /p:PublishProfile="AzureWebApp - Web Deploy" 
    /p:Username="$AzureWebApp" 
    /p:Password=".........."
```

<span data-ttu-id="592e2-250">Un profil de publication peut également être utilisé avec la commande [dotnet msbuild](/dotnet/core/tools/dotnet-msbuild) de l’interface CLI .NET Core depuis une invite de commandes Windows :</span><span class="sxs-lookup"><span data-stu-id="592e2-250">A publish profile can also be used with the .NET Core CLI [dotnet msbuild](/dotnet/core/tools/dotnet-msbuild) command from a Windows command prompt:</span></span>

```console
dotnet msbuild "AzureWebApp.csproj"
    /p:DeployOnBuild=true 
    /p:PublishProfile="AzureWebApp - Web Deploy" 
    /p:Username="$AzureWebApp" 
    /p:Password=".........."
```

> [!NOTE]
> <span data-ttu-id="592e2-251">La commande [dotnet msbuild](/dotnet/core/tools/dotnet-msbuild) est multiplateforme et peut compiler des applications ASP.NET Core sur macOS et Linux.</span><span class="sxs-lookup"><span data-stu-id="592e2-251">The [dotnet msbuild](/dotnet/core/tools/dotnet-msbuild) command is available cross-platform and can compile ASP.NET Core apps on macOS and Linux.</span></span> <span data-ttu-id="592e2-252">Toutefois, MSBuild sur macOS et Linux n’est pas capable de déployer une application dans Azure ou un autre point de terminaison MSDeploy.</span><span class="sxs-lookup"><span data-stu-id="592e2-252">However, MSBuild on macOS and Linux isn't capable of deploying an app to Azure or other MSDeploy endpoint.</span></span> <span data-ttu-id="592e2-253">MSDeploy est disponible uniquement sur Windows.</span><span class="sxs-lookup"><span data-stu-id="592e2-253">MSDeploy is only available on Windows.</span></span>

## <a name="set-the-environment"></a><span data-ttu-id="592e2-254">Définir l’environnement</span><span class="sxs-lookup"><span data-stu-id="592e2-254">Set the environment</span></span>

<span data-ttu-id="592e2-255">Incluez la propriété `<EnvironmentName>` dans le profil de facturation ( *.pubxml*) ou le fichier projet pour définir [l’environnement](xref:fundamentals/environments) de l’application :</span><span class="sxs-lookup"><span data-stu-id="592e2-255">Include the `<EnvironmentName>` property in the publish profile (*.pubxml*) or project file to set the app's [environment](xref:fundamentals/environments):</span></span>

```xml
<PropertyGroup>
  <EnvironmentName>Development</EnvironmentName>
</PropertyGroup>
```

<span data-ttu-id="592e2-256">Si vous avez besoin de transformations de *web.config* (par exemple, définir les variables d’environnement basées sur la configuration, le profil ou l’environnement), consultez <xref:host-and-deploy/iis/transform-webconfig>.</span><span class="sxs-lookup"><span data-stu-id="592e2-256">If you require *web.config* transformations (for example, setting environment variables based on the configuration, profile, or environment), see <xref:host-and-deploy/iis/transform-webconfig>.</span></span>

## <a name="exclude-files"></a><span data-ttu-id="592e2-257">Exclure des fichiers</span><span class="sxs-lookup"><span data-stu-id="592e2-257">Exclude files</span></span>

<span data-ttu-id="592e2-258">Lors de la publication d’applications web ASP.NET Core, les ressources suivantes sont incluses :</span><span class="sxs-lookup"><span data-stu-id="592e2-258">When publishing ASP.NET Core web apps, the following assets are included:</span></span>

* <span data-ttu-id="592e2-259">Artefacts de build</span><span class="sxs-lookup"><span data-stu-id="592e2-259">Build artifacts</span></span>
* <span data-ttu-id="592e2-260">Les dossiers et fichiers correspondant aux modèles d’utilisation des caractères génériques suivants :</span><span class="sxs-lookup"><span data-stu-id="592e2-260">Folders and files matching the following globbing patterns:</span></span>
  * <span data-ttu-id="592e2-261">`**\*.config` (par exemple *web.config*)</span><span class="sxs-lookup"><span data-stu-id="592e2-261">`**\*.config` (for example, *web.config*)</span></span>
  * <span data-ttu-id="592e2-262">`**\*.json` (par exemple *appsettings.json*)</span><span class="sxs-lookup"><span data-stu-id="592e2-262">`**\*.json` (for example, *appsettings.json*)</span></span>
  * `wwwroot\**`

<span data-ttu-id="592e2-263">MSBuild prend en charge les [modèles d’utilisation des caractères génériques](https://gruntjs.com/configuring-tasks#globbing-patterns).</span><span class="sxs-lookup"><span data-stu-id="592e2-263">MSBuild supports [globbing patterns](https://gruntjs.com/configuring-tasks#globbing-patterns).</span></span> <span data-ttu-id="592e2-264">Par exemple, l’élément `<Content>` suivant supprime la copie des fichiers texte ( *.txt*) dans le dossier *wwwroot/content* et ses sous-dossiers :</span><span class="sxs-lookup"><span data-stu-id="592e2-264">For example, the following `<Content>` element suppresses the copying of text (*.txt*) files in the *wwwroot\content* folder and its subfolders:</span></span>

```xml
<ItemGroup>
  <Content Update="wwwroot/content/**/*.txt" CopyToPublishDirectory="Never" />
</ItemGroup>
```

<span data-ttu-id="592e2-265">Vous pouvez ajouter le balisage précédent à un profil de publication ou au fichier *.csproj*.</span><span class="sxs-lookup"><span data-stu-id="592e2-265">The preceding markup can be added to a publish profile or the *.csproj* file.</span></span> <span data-ttu-id="592e2-266">En cas d’ajout au fichier *.csproj*, la règle est ajoutée à tous les profils de publication dans le projet.</span><span class="sxs-lookup"><span data-stu-id="592e2-266">When added to the *.csproj* file, the rule is added to all publish profiles in the project.</span></span>

<span data-ttu-id="592e2-267">L’élément `<MsDeploySkipRules>` suivant exclut tous les fichiers du dossier *wwwroot\content* :</span><span class="sxs-lookup"><span data-stu-id="592e2-267">The following `<MsDeploySkipRules>` element excludes all files from the *wwwroot\content* folder:</span></span>

```xml
<ItemGroup>
  <MsDeploySkipRules Include="CustomSkipFolder">
    <ObjectName>dirPath</ObjectName>
    <AbsolutePath>wwwroot\\content</AbsolutePath>
  </MsDeploySkipRules>
</ItemGroup>
```

<span data-ttu-id="592e2-268">`<MsDeploySkipRules>` ne supprime pas les cibles *skip* du site de déploiement.</span><span class="sxs-lookup"><span data-stu-id="592e2-268">`<MsDeploySkipRules>` won't delete the *skip* targets from the deployment site.</span></span> <span data-ttu-id="592e2-269">Les dossiers et fichiers ciblés par `<Content>` sont supprimés du site de déploiement.</span><span class="sxs-lookup"><span data-stu-id="592e2-269">`<Content>` targeted files and folders are deleted from the deployment site.</span></span> <span data-ttu-id="592e2-270">Par exemple, supposez qu’une application web déployée avait les fichiers suivants :</span><span class="sxs-lookup"><span data-stu-id="592e2-270">For example, suppose a deployed web app had the following files:</span></span>

* <span data-ttu-id="592e2-271">*Views/Home/About1.cshtml*</span><span class="sxs-lookup"><span data-stu-id="592e2-271">*Views/Home/About1.cshtml*</span></span>
* <span data-ttu-id="592e2-272">*Views/Home/About2.cshtml*</span><span class="sxs-lookup"><span data-stu-id="592e2-272">*Views/Home/About2.cshtml*</span></span>
* <span data-ttu-id="592e2-273">*Views/Home/About3.cshtml*</span><span class="sxs-lookup"><span data-stu-id="592e2-273">*Views/Home/About3.cshtml*</span></span>

<span data-ttu-id="592e2-274">Si les éléments `<MsDeploySkipRules>` suivants sont ajoutés, ces fichiers ne sont pas supprimés sur le site de déploiement.</span><span class="sxs-lookup"><span data-stu-id="592e2-274">If the following `<MsDeploySkipRules>` elements are added, those files wouldn't be deleted on the deployment site.</span></span>

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

<span data-ttu-id="592e2-275">Les éléments `<MsDeploySkipRules>` précédents empêchent le déploiement des fichiers *skipped*.</span><span class="sxs-lookup"><span data-stu-id="592e2-275">The preceding `<MsDeploySkipRules>` elements prevent the *skipped* files from being deployed.</span></span> <span data-ttu-id="592e2-276">Ces fichiers ne sont pas supprimés une fois déployés.</span><span class="sxs-lookup"><span data-stu-id="592e2-276">It won't delete those files once they're deployed.</span></span>

<span data-ttu-id="592e2-277">L’élément `<Content>` suivant supprime les fichiers ciblés sur le site de déploiement :</span><span class="sxs-lookup"><span data-stu-id="592e2-277">The following `<Content>` element deletes the targeted files at the deployment site:</span></span>

```xml
<ItemGroup>
  <Content Update="Views/Home/About?.cshtml" CopyToPublishDirectory="Never" />
</ItemGroup>
```

<span data-ttu-id="592e2-278">L’utilisation du déploiement à partir de la ligne de commande avec l’élément `<Content>` précédent génère la sortie suivante :</span><span class="sxs-lookup"><span data-stu-id="592e2-278">Using command-line deployment with the preceding `<Content>` element yields the following output:</span></span>

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

## <a name="include-files"></a><span data-ttu-id="592e2-279">Fichiers Include</span><span class="sxs-lookup"><span data-stu-id="592e2-279">Include files</span></span>

<span data-ttu-id="592e2-280">Le balisage suivant :</span><span class="sxs-lookup"><span data-stu-id="592e2-280">The following markup:</span></span>

* <span data-ttu-id="592e2-281">Inclut un dossier *images* situé en dehors du répertoire de projet dans le dossier *wwwroot/images* du site de publication.</span><span class="sxs-lookup"><span data-stu-id="592e2-281">Includes an *images* folder outside the project directory to the *wwwroot/images* folder of the publish site.</span></span>
* <span data-ttu-id="592e2-282">Vous pouvez l’ajouter au fichier *.csproj* ou au profil de publication.</span><span class="sxs-lookup"><span data-stu-id="592e2-282">Can be added to the *.csproj* file or the publish profile.</span></span> <span data-ttu-id="592e2-283">Si vous l’ajoutez au fichier *csproj*, il est inclus dans chaque profil de publication du projet.</span><span class="sxs-lookup"><span data-stu-id="592e2-283">If it's added to the *.csproj* file, it's included in each publish profile in the project.</span></span>

```xml
<ItemGroup>
  <_CustomFiles Include="$(MSBuildProjectDirectory)/../images/**/*" />
  <DotnetPublishFiles Include="@(_CustomFiles)">
    <DestinationRelativePath>wwwroot/images/%(RecursiveDir)%(Filename)%(Extension)</DestinationRelativePath>
  </DotnetPublishFiles>
</ItemGroup>
```

<span data-ttu-id="592e2-284">Le balisage mis en surbrillance ci-dessous montre comment :</span><span class="sxs-lookup"><span data-stu-id="592e2-284">The following highlighted markup shows how to:</span></span>

* <span data-ttu-id="592e2-285">Copier un fichier situé en dehors du projet dans le dossier *wwwroot*</span><span class="sxs-lookup"><span data-stu-id="592e2-285">Copy a file from outside the project into the *wwwroot* folder.</span></span>
* <span data-ttu-id="592e2-286">Exclure le dossier *wwwroot\Content*</span><span class="sxs-lookup"><span data-stu-id="592e2-286">Exclude the *wwwroot\Content* folder.</span></span>
* <span data-ttu-id="592e2-287">Exclure *Views\Home\About2.cshtml*</span><span class="sxs-lookup"><span data-stu-id="592e2-287">Exclude *Views\Home\About2.cshtml*.</span></span>

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

<span data-ttu-id="592e2-288">Consultez le [Fichier Lisez-moi du référentiel du SDK web](https://github.com/aspnet/websdk) pour d’autres exemples de déploiement.</span><span class="sxs-lookup"><span data-stu-id="592e2-288">See the [Web SDK repository Readme](https://github.com/aspnet/websdk) for more deployment samples.</span></span>

## <a name="run-a-target-before-or-after-publishing"></a><span data-ttu-id="592e2-289">Exécuter une cible avant ou après la publication</span><span class="sxs-lookup"><span data-stu-id="592e2-289">Run a target before or after publishing</span></span>

<span data-ttu-id="592e2-290">Les cibles intégrées `BeforePublish` et `AfterPublish` exécutent une cible avant ou après la cible de publication.</span><span class="sxs-lookup"><span data-stu-id="592e2-290">The built-in `BeforePublish` and `AfterPublish` targets execute a target before or after the publish target.</span></span> <span data-ttu-id="592e2-291">Ajoutez les éléments suivants au profil de publication pour journaliser les messages de console avant et après la publication :</span><span class="sxs-lookup"><span data-stu-id="592e2-291">Add the following elements to the publish profile to log console messages both before and after publishing:</span></span>

```xml
<Target Name="CustomActionsBeforePublish" BeforeTargets="BeforePublish">
    <Message Text="Inside BeforePublish" Importance="high" />
  </Target>
  <Target Name="CustomActionsAfterPublish" AfterTargets="AfterPublish">
    <Message Text="Inside AfterPublish" Importance="high" />
</Target>
```

## <a name="publish-to-a-server-using-an-untrusted-certificate"></a><span data-ttu-id="592e2-292">Publier sur un serveur à l’aide d’un certificat non approuvé</span><span class="sxs-lookup"><span data-stu-id="592e2-292">Publish to a server using an untrusted certificate</span></span>

<span data-ttu-id="592e2-293">Ajoutez la propriété `<AllowUntrustedCertificate>` avec la valeur `True` au profil de publication :</span><span class="sxs-lookup"><span data-stu-id="592e2-293">Add the `<AllowUntrustedCertificate>` property with a value of `True` to the publish profile:</span></span>

```xml
<PropertyGroup>
  <AllowUntrustedCertificate>True</AllowUntrustedCertificate>
</PropertyGroup>
```

## <a name="the-kudu-service"></a><span data-ttu-id="592e2-294">Le service Kudu</span><span class="sxs-lookup"><span data-stu-id="592e2-294">The Kudu service</span></span>

<span data-ttu-id="592e2-295">Pour afficher les fichiers dans un déploiement d’application web Azure App Service, utilisez le [service Kudu](https://github.com/projectkudu/kudu/wiki/Accessing-the-kudu-service).</span><span class="sxs-lookup"><span data-stu-id="592e2-295">To view the files in an Azure App Service web app deployment, use the [Kudu service](https://github.com/projectkudu/kudu/wiki/Accessing-the-kudu-service).</span></span> <span data-ttu-id="592e2-296">Ajoutez le jeton `scm` au nom de l’application web.</span><span class="sxs-lookup"><span data-stu-id="592e2-296">Append the `scm` token to the web app name.</span></span> <span data-ttu-id="592e2-297">Par exemple :</span><span class="sxs-lookup"><span data-stu-id="592e2-297">For example:</span></span>

| <span data-ttu-id="592e2-298">URL</span><span class="sxs-lookup"><span data-stu-id="592e2-298">URL</span></span>                                    | <span data-ttu-id="592e2-299">Résultat</span><span class="sxs-lookup"><span data-stu-id="592e2-299">Result</span></span>       |
| -------------------------------------- | ------------ |
| `http://mysite.azurewebsites.net/`     | <span data-ttu-id="592e2-300">Application web</span><span class="sxs-lookup"><span data-stu-id="592e2-300">Web App</span></span>      |
| `http://mysite.scm.azurewebsites.net/` | <span data-ttu-id="592e2-301">Service Kudu</span><span class="sxs-lookup"><span data-stu-id="592e2-301">Kudu service</span></span> |

<span data-ttu-id="592e2-302">Sélectionnez l’élément de menu [Console de débogage](https://github.com/projectkudu/kudu/wiki/Kudu-console) pour afficher, modifier, supprimer ou ajouter des fichiers.</span><span class="sxs-lookup"><span data-stu-id="592e2-302">Select the [Debug Console](https://github.com/projectkudu/kudu/wiki/Kudu-console) menu item to view, edit, delete, or add files.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="592e2-303">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="592e2-303">Additional resources</span></span>

* <span data-ttu-id="592e2-304">[Web Deploy](https://www.iis.net/downloads/microsoft/web-deploy) (MSDeploy) simplifie le déploiement des applications web et des sites web sur les serveurs IIS.</span><span class="sxs-lookup"><span data-stu-id="592e2-304">[Web Deploy](https://www.iis.net/downloads/microsoft/web-deploy) (MSDeploy) simplifies deployment of web apps and websites to IIS servers.</span></span>
* <span data-ttu-id="592e2-305">[Référentiel GitHub du SDK web](https://github.com/aspnet/websdk/issues) : soumettez vos problèmes et demandez des fonctionnalités pour le déploiement.</span><span class="sxs-lookup"><span data-stu-id="592e2-305">[Web SDK GitHub repository](https://github.com/aspnet/websdk/issues): File issues and request features for deployment.</span></span>
* [<span data-ttu-id="592e2-306">Publier une application web ASP.NET sur une machine virtuelle Azure à partir de Visual Studio</span><span class="sxs-lookup"><span data-stu-id="592e2-306">Publish an ASP.NET Web App to an Azure VM from Visual Studio</span></span>](/azure/virtual-machines/windows/publish-web-app-from-visual-studio)
* <xref:host-and-deploy/iis/transform-webconfig>
