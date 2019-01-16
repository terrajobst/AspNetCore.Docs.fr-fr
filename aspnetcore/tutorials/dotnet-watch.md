---
title: Développer des applications ASP.NET Core à l’aide d’un observateur de fichiers
author: rick-anderson
description: Ce tutoriel montre comment installer et utiliser l’outil Observateur de fichiers (dotnet watch) de l’interface de ligne de commande .NET Core dans une application ASP.NET Core.
ms.author: riande
ms.date: 05/31/2018
uid: tutorials/dotnet-watch
ms.openlocfilehash: f1e0d91b27df4af7cbfb6f2547c94c0370c65d0d
ms.sourcegitcommit: cec77d5ad8a0cedb1ecbec32834111492afd0cd2
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/10/2019
ms.locfileid: "54207500"
---
# <a name="develop-aspnet-core-apps-using-a-file-watcher"></a><span data-ttu-id="fb391-103">Développer des applications ASP.NET Core à l’aide d’un observateur de fichiers</span><span class="sxs-lookup"><span data-stu-id="fb391-103">Develop ASP.NET Core apps using a file watcher</span></span>

<span data-ttu-id="fb391-104">Par [Rick Anderson](https://twitter.com/RickAndMSFT) et [Victor Hurdugaci](https://twitter.com/victorhurdugaci)</span><span class="sxs-lookup"><span data-stu-id="fb391-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Victor Hurdugaci](https://twitter.com/victorhurdugaci)</span></span>

<span data-ttu-id="fb391-105">`dotnet watch` est un outil qui exécute une commande [.NET Core CLI](/dotnet/core/tools) quand des fichiers sources changent.</span><span class="sxs-lookup"><span data-stu-id="fb391-105">`dotnet watch` is a tool that runs a [.NET Core CLI](/dotnet/core/tools) command when source files change.</span></span> <span data-ttu-id="fb391-106">Par exemple, un changement de fichier peut déclencher la compilation, l’exécution de tests ou le déploiement.</span><span class="sxs-lookup"><span data-stu-id="fb391-106">For example, a file change can trigger compilation, test execution, or deployment.</span></span>

<span data-ttu-id="fb391-107">Ce tutoriel utilise une API web existante avec deux points de terminaison : un qui retourne une somme et un qui retourne un produit.</span><span class="sxs-lookup"><span data-stu-id="fb391-107">This tutorial uses an existing web API with two endpoints: one that returns a sum and one that returns a product.</span></span> <span data-ttu-id="fb391-108">La méthode du produit comporte un bogue, qui est résolu dans ce tutoriel.</span><span class="sxs-lookup"><span data-stu-id="fb391-108">The product method has a bug, which is fixed in this tutorial.</span></span>

<span data-ttu-id="fb391-109">Téléchargez l’[application exemple](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/dotnet-watch/sample).</span><span class="sxs-lookup"><span data-stu-id="fb391-109">Download the [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/dotnet-watch/sample).</span></span> <span data-ttu-id="fb391-110">Elle est composée de deux projets : *WebApp* (API web ASP.NET Core) et *WebAppTests* (API de tests unitaires pour le web).</span><span class="sxs-lookup"><span data-stu-id="fb391-110">It consists of two projects: *WebApp* (an ASP.NET Core web API) and *WebAppTests* (unit tests for the web API).</span></span>

<span data-ttu-id="fb391-111">Dans une interface de commande, accédez au dossier *WebApp*.</span><span class="sxs-lookup"><span data-stu-id="fb391-111">In a command shell, navigate to the *WebApp* folder.</span></span> <span data-ttu-id="fb391-112">Exécutez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="fb391-112">Run the following command:</span></span>

```console
dotnet run
```

<span data-ttu-id="fb391-113">La sortie de la console affiche des messages semblables à ce qui suit (indiquant que l’application est en cours d’exécution et en attente de demandes) :</span><span class="sxs-lookup"><span data-stu-id="fb391-113">The console output shows messages similar to the following (indicating that the app is running and awaiting requests):</span></span>

```console
$ dotnet run
Hosting environment: Development
Content root path: C:/Docs/aspnetcore/tutorials/dotnet-watch/sample/WebApp
Now listening on: http://localhost:5000
Application started. Press Ctrl+C to shut down.
```

<span data-ttu-id="fb391-114">Dans un navigateur web, accédez à `http://localhost:<port number>/api/math/sum?a=4&b=5`.</span><span class="sxs-lookup"><span data-stu-id="fb391-114">In a web browser, navigate to `http://localhost:<port number>/api/math/sum?a=4&b=5`.</span></span> <span data-ttu-id="fb391-115">Vous devez voir le résultat `9`.</span><span class="sxs-lookup"><span data-stu-id="fb391-115">You should see the result of `9`.</span></span>

<span data-ttu-id="fb391-116">Accédez à l’API du produit (`http://localhost:<port number>/api/math/product?a=4&b=5`).</span><span class="sxs-lookup"><span data-stu-id="fb391-116">Navigate to the product API (`http://localhost:<port number>/api/math/product?a=4&b=5`).</span></span> <span data-ttu-id="fb391-117">Elle retourne `9`, et non pas `20` comme prévu.</span><span class="sxs-lookup"><span data-stu-id="fb391-117">It returns `9`, not `20` as you'd expect.</span></span> <span data-ttu-id="fb391-118">Ce problème est résolu plus tard dans le tutoriel.</span><span class="sxs-lookup"><span data-stu-id="fb391-118">That problem is fixed later in the tutorial.</span></span>

::: moniker range="<= aspnetcore-2.0"

## <a name="add-dotnet-watch-to-a-project"></a><span data-ttu-id="fb391-119">Ajouter `dotnet watch` à un projet</span><span class="sxs-lookup"><span data-stu-id="fb391-119">Add `dotnet watch` to a project</span></span>

<span data-ttu-id="fb391-120">L’outil Observateur de fichiers `dotnet watch` est inclus dans la version 2.1.300 du kit SDK .NET Core.</span><span class="sxs-lookup"><span data-stu-id="fb391-120">The `dotnet watch` file watcher tool is included with version 2.1.300 of the .NET Core SDK.</span></span> <span data-ttu-id="fb391-121">Les étapes suivantes sont requises quand vous utilisez une version antérieure du kit SDK .NET Core.</span><span class="sxs-lookup"><span data-stu-id="fb391-121">The following steps are required when using an earlier version of the .NET Core SDK.</span></span>

1. <span data-ttu-id="fb391-122">Ajoutez une référence de package `Microsoft.DotNet.Watcher.Tools` dans le fichier *.csproj* :</span><span class="sxs-lookup"><span data-stu-id="fb391-122">Add a `Microsoft.DotNet.Watcher.Tools` package reference to the *.csproj* file:</span></span>

    ```xml
    <ItemGroup>
        <DotNetCliToolReference Include="Microsoft.DotNet.Watcher.Tools" Version="2.0.0" />
    </ItemGroup>
    ```

1. <span data-ttu-id="fb391-123">Installez le package `Microsoft.DotNet.Watcher.Tools` en exécutant la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="fb391-123">Install the `Microsoft.DotNet.Watcher.Tools` package by running the following command:</span></span>

    ```console
    dotnet restore
    ```

::: moniker-end

## <a name="run-net-core-cli-commands-using-dotnet-watch"></a><span data-ttu-id="fb391-124">Exécuter les commandes de l’interface CLI de .NET Core avec `dotnet watch`</span><span class="sxs-lookup"><span data-stu-id="fb391-124">Run .NET Core CLI commands using `dotnet watch`</span></span>

<span data-ttu-id="fb391-125">Toutes les [commandes de l’interface CLI de .NET Core](/dotnet/core/tools#cli-commands) peuvent être exécutées avec `dotnet watch`.</span><span class="sxs-lookup"><span data-stu-id="fb391-125">Any [.NET Core CLI command](/dotnet/core/tools#cli-commands) can be run with `dotnet watch`.</span></span> <span data-ttu-id="fb391-126">Par exemple :</span><span class="sxs-lookup"><span data-stu-id="fb391-126">For example:</span></span>

| <span data-ttu-id="fb391-127">Commande</span><span class="sxs-lookup"><span data-stu-id="fb391-127">Command</span></span> | <span data-ttu-id="fb391-128">Commande avec watch</span><span class="sxs-lookup"><span data-stu-id="fb391-128">Command with watch</span></span> |
| ---- | ----- |
| <span data-ttu-id="fb391-129">dotnet run</span><span class="sxs-lookup"><span data-stu-id="fb391-129">dotnet run</span></span> | <span data-ttu-id="fb391-130">dotnet watch run</span><span class="sxs-lookup"><span data-stu-id="fb391-130">dotnet watch run</span></span> |
| <span data-ttu-id="fb391-131">dotnet run -f netcoreapp2.0</span><span class="sxs-lookup"><span data-stu-id="fb391-131">dotnet run -f netcoreapp2.0</span></span> | <span data-ttu-id="fb391-132">dotnet watch run -f netcoreapp2.0</span><span class="sxs-lookup"><span data-stu-id="fb391-132">dotnet watch run -f netcoreapp2.0</span></span> |
| <span data-ttu-id="fb391-133">dotnet run -f netcoreapp2.0 -- --arg1</span><span class="sxs-lookup"><span data-stu-id="fb391-133">dotnet run -f netcoreapp2.0 -- --arg1</span></span> | <span data-ttu-id="fb391-134">dotnet watch run -f netcoreapp2.0 -- --arg1</span><span class="sxs-lookup"><span data-stu-id="fb391-134">dotnet watch run -f netcoreapp2.0 -- --arg1</span></span> |
| <span data-ttu-id="fb391-135">dotnet test</span><span class="sxs-lookup"><span data-stu-id="fb391-135">dotnet test</span></span> | <span data-ttu-id="fb391-136">dotnet watch test</span><span class="sxs-lookup"><span data-stu-id="fb391-136">dotnet watch test</span></span> |

<span data-ttu-id="fb391-137">Exécutez `dotnet watch run` dans le dossier *WebApp*.</span><span class="sxs-lookup"><span data-stu-id="fb391-137">Run `dotnet watch run` in the *WebApp* folder.</span></span> <span data-ttu-id="fb391-138">La sortie de la console indique que `watch` a démarré.</span><span class="sxs-lookup"><span data-stu-id="fb391-138">The console output indicates `watch` has started.</span></span>

## <a name="make-changes-with-dotnet-watch"></a><span data-ttu-id="fb391-139">Apporter des modifications avec `dotnet watch`</span><span class="sxs-lookup"><span data-stu-id="fb391-139">Make changes with `dotnet watch`</span></span>

<span data-ttu-id="fb391-140">Vérifiez que `dotnet watch` est en cours d’exécution.</span><span class="sxs-lookup"><span data-stu-id="fb391-140">Make sure `dotnet watch` is running.</span></span>

<span data-ttu-id="fb391-141">Corrigez le bogue présent dans la méthode `Product` de *MathController.cs* afin qu’elle retourne le produit et non la somme :</span><span class="sxs-lookup"><span data-stu-id="fb391-141">Fix the bug in the `Product` method of *MathController.cs* so it returns the product and not the sum:</span></span>

```csharp
public static int Product(int a, int b)
{
  return a * b;
}
```

<span data-ttu-id="fb391-142">Enregistrez le fichier.</span><span class="sxs-lookup"><span data-stu-id="fb391-142">Save the file.</span></span> <span data-ttu-id="fb391-143">La sortie de la console indique que `dotnet watch` a détecté un changement de fichier et a redémarré l’application.</span><span class="sxs-lookup"><span data-stu-id="fb391-143">The console output indicates that `dotnet watch` detected a file change and restarted the app.</span></span>

<span data-ttu-id="fb391-144">Vérifiez que `http://localhost:<port number>/api/math/product?a=4&b=5` retourne le résultat correct.</span><span class="sxs-lookup"><span data-stu-id="fb391-144">Verify `http://localhost:<port number>/api/math/product?a=4&b=5` returns the correct result.</span></span>

## <a name="run-tests-using-dotnet-watch"></a><span data-ttu-id="fb391-145">Exécuter les tests à l’aide de `dotnet watch`</span><span class="sxs-lookup"><span data-stu-id="fb391-145">Run tests using `dotnet watch`</span></span>

1. <span data-ttu-id="fb391-146">Changez la méthode `Product` de *MathController.cs* pour qu’elle retourne à nouveau la somme.</span><span class="sxs-lookup"><span data-stu-id="fb391-146">Change the `Product` method of *MathController.cs* back to returning the sum.</span></span> <span data-ttu-id="fb391-147">Enregistrez le fichier.</span><span class="sxs-lookup"><span data-stu-id="fb391-147">Save the file.</span></span>
1. <span data-ttu-id="fb391-148">Dans une interface de commande, accédez au dossier *WebAppTests*.</span><span class="sxs-lookup"><span data-stu-id="fb391-148">In a command shell, navigate to the *WebAppTests* folder.</span></span>
1. <span data-ttu-id="fb391-149">Exécutez [dotnet restore](/dotnet/core/tools/dotnet-restore).</span><span class="sxs-lookup"><span data-stu-id="fb391-149">Run [dotnet restore](/dotnet/core/tools/dotnet-restore).</span></span>
1. <span data-ttu-id="fb391-150">Exécutez `dotnet watch test`.</span><span class="sxs-lookup"><span data-stu-id="fb391-150">Run `dotnet watch test`.</span></span> <span data-ttu-id="fb391-151">Sa sortie indique qu’un test a échoué et que l’observateur est en attente de changement de fichier :</span><span class="sxs-lookup"><span data-stu-id="fb391-151">Its output indicates that a test failed and that the watcher is awaiting file changes:</span></span>

     ```console
     Total tests: 2. Passed: 1. Failed: 1. Skipped: 0.
     Test Run Failed.
     ```

1. <span data-ttu-id="fb391-152">Corrigez le code de la méthode `Product` afin qu’elle retourne le produit.</span><span class="sxs-lookup"><span data-stu-id="fb391-152">Fix the `Product` method code so it returns the product.</span></span> <span data-ttu-id="fb391-153">Enregistrez le fichier.</span><span class="sxs-lookup"><span data-stu-id="fb391-153">Save the file.</span></span>

<span data-ttu-id="fb391-154">`dotnet watch` détecte le changement de fichier et réexécute les tests.</span><span class="sxs-lookup"><span data-stu-id="fb391-154">`dotnet watch` detects the file change and reruns the tests.</span></span> <span data-ttu-id="fb391-155">La sortie de la console indique que les tests ont réussi.</span><span class="sxs-lookup"><span data-stu-id="fb391-155">The console output indicates the tests passed.</span></span>

## <a name="customize-files-list-to-watch"></a><span data-ttu-id="fb391-156">Personnaliser la liste de fichiers à observer</span><span class="sxs-lookup"><span data-stu-id="fb391-156">Customize files list to watch</span></span>

<span data-ttu-id="fb391-157">Par défaut, `dotnet-watch` effectue le suivi de tous les fichiers qui correspondent aux modèles Glob suivants :</span><span class="sxs-lookup"><span data-stu-id="fb391-157">By default, `dotnet-watch` tracks all files matching the following glob patterns:</span></span>

* `**/*.cs`
* `*.csproj`
* `**/*.resx`

<span data-ttu-id="fb391-158">Vous pouvez ajouter plusieurs éléments à la liste de suivi en modifiant le fichier *.csproj*.</span><span class="sxs-lookup"><span data-stu-id="fb391-158">More items can be added to the watch list by editing the *.csproj* file.</span></span> <span data-ttu-id="fb391-159">Vous pouvez spécifier des éléments individuellement ou en utilisant des modèles Glob.</span><span class="sxs-lookup"><span data-stu-id="fb391-159">Items can be specified individually or by using glob patterns.</span></span>

```xml
<ItemGroup>
    <!-- extends watching group to include *.js files -->
    <Watch Include="**\*.js" Exclude="node_modules\**\*;**\*.js.map;obj\**\*;bin\**\*" />
</ItemGroup>
```

## <a name="opt-out-of-files-to-be-watched"></a><span data-ttu-id="fb391-160">Exclusion de fichiers à observer</span><span class="sxs-lookup"><span data-stu-id="fb391-160">Opt-out of files to be watched</span></span>

<span data-ttu-id="fb391-161">`dotnet-watch` peut être configuré pour ignorer ses paramètres par défaut.</span><span class="sxs-lookup"><span data-stu-id="fb391-161">`dotnet-watch` can be configured to ignore its default settings.</span></span> <span data-ttu-id="fb391-162">Pour ignorer des fichiers spécifiques, ajoutez l’attribut `Watch="false"` à la définition d’un élément dans le fichier *.csproj* :</span><span class="sxs-lookup"><span data-stu-id="fb391-162">To ignore specific files, add the `Watch="false"` attribute to an item's definition in the *.csproj* file:</span></span>

```xml
<ItemGroup>
    <!-- exclude Generated.cs from dotnet-watch -->
    <Compile Include="Generated.cs" Watch="false" />

    <!-- exclude Strings.resx from dotnet-watch -->
    <EmbeddedResource Include="Strings.resx" Watch="false" />

    <!-- exclude changes in this referenced project -->
    <ProjectReference Include="..\ClassLibrary1\ClassLibrary1.csproj" Watch="false" />
</ItemGroup>
```

## <a name="custom-watch-projects"></a><span data-ttu-id="fb391-163">Personnaliser les projets de suivi</span><span class="sxs-lookup"><span data-stu-id="fb391-163">Custom watch projects</span></span>

<span data-ttu-id="fb391-164">`dotnet-watch` n’est pas limité aux projets C#.</span><span class="sxs-lookup"><span data-stu-id="fb391-164">`dotnet-watch` isn't restricted to C# projects.</span></span> <span data-ttu-id="fb391-165">Vous pouvez créer des projets de suivi personnalisés pour gérer différents scénarios.</span><span class="sxs-lookup"><span data-stu-id="fb391-165">Custom watch projects can be created to handle different scenarios.</span></span> <span data-ttu-id="fb391-166">Considérez l’organisation de projets suivante :</span><span class="sxs-lookup"><span data-stu-id="fb391-166">Consider the following project layout:</span></span>

* <span data-ttu-id="fb391-167">**test/**</span><span class="sxs-lookup"><span data-stu-id="fb391-167">**test/**</span></span>
  * <span data-ttu-id="fb391-168">*UnitTests/UnitTests.csproj*</span><span class="sxs-lookup"><span data-stu-id="fb391-168">*UnitTests/UnitTests.csproj*</span></span>
  * <span data-ttu-id="fb391-169">*IntegrationTests/IntegrationTests.csproj*</span><span class="sxs-lookup"><span data-stu-id="fb391-169">*IntegrationTests/IntegrationTests.csproj*</span></span>

<span data-ttu-id="fb391-170">Si l’objectif est d’observer les deux projets, créez un fichier projet personnalisé configuré à cette fin :</span><span class="sxs-lookup"><span data-stu-id="fb391-170">If the goal is to watch both projects, create a custom project file configured to watch both projects:</span></span>

```xml
<Project>
    <ItemGroup>
        <TestProjects Include="**\*.csproj" />
        <Watch Include="**\*.cs" />
    </ItemGroup>

    <Target Name="Test">
        <MSBuild Targets="VSTest" Projects="@(TestProjects)" />
    </Target>

    <Import Project="$(MSBuildExtensionsPath)\Microsoft.Common.targets" />
</Project>
```

<span data-ttu-id="fb391-171">Pour démarrer l’observation de fichiers sur les deux projets, passez au dossier *test*.</span><span class="sxs-lookup"><span data-stu-id="fb391-171">To start file watching on both projects, change to the *test* folder.</span></span> <span data-ttu-id="fb391-172">Exécutez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="fb391-172">Execute the following command:</span></span>

```console
dotnet watch msbuild /t:Test
```

<span data-ttu-id="fb391-173">VSTest s’exécute quand n’importe quel fichier change dans un des projets de test.</span><span class="sxs-lookup"><span data-stu-id="fb391-173">VSTest executes when any file changes in either test project.</span></span>

## <a name="dotnet-watch-in-github"></a><span data-ttu-id="fb391-174">`dotnet-watch` dans GitHub</span><span class="sxs-lookup"><span data-stu-id="fb391-174">`dotnet-watch` in GitHub</span></span>

<span data-ttu-id="fb391-175">`dotnet-watch` fait partie du [référentiel aspnet/AspNetCore](https://github.com/aspnet/AspNetCore/tree/master/src/Tools/dotnet-watch) GitHub.</span><span class="sxs-lookup"><span data-stu-id="fb391-175">`dotnet-watch` is part of the GitHub [aspnet/AspNetCore repository](https://github.com/aspnet/AspNetCore/tree/master/src/Tools/dotnet-watch).</span></span>
