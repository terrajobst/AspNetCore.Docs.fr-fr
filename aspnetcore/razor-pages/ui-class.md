---
title: Interface utilisateur Razor réutilisable dans les bibliothèques de classes avec ASP.NET Core
author: Rick-Anderson
description: Explique comment créer l’interface utilisateur de Razor réutilisables à l’aide de vues partielles dans une bibliothèque de classes dans ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 10/08/2019
ms.custom: mvc, seodec18
uid: razor-pages/ui-class
ms.openlocfilehash: d656e924033f1b217cdd8c86f7d00411c5d71beb
ms.sourcegitcommit: 73a451e9a58ac7102f90b608d661d8c23dd9bbaf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/08/2019
ms.locfileid: "72037587"
---
# <a name="create-reusable-ui-using-the-razor-class-library-project-in-aspnet-core"></a><span data-ttu-id="717f4-103">Créer une interface utilisateur réutilisable à l’aide du projet de bibliothèque de classes Razor dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="717f4-103">Create reusable UI using the Razor class library project in ASP.NET Core</span></span>

<span data-ttu-id="717f4-104">Par [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="717f4-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="717f4-105">Les vues Razor, les pages, les contrôleurs, les modèles de page, les [composants Razor](xref:blazor/class-libraries), les [composants de vue](xref:mvc/views/view-components)et les modèles de données peuvent être intégrés dans une bibliothèque de classes Razor (RCL).</span><span class="sxs-lookup"><span data-stu-id="717f4-105">Razor views, pages, controllers, page models, [Razor components](xref:blazor/class-libraries), [View components](xref:mvc/views/view-components), and data models can be built into a Razor class library (RCL).</span></span> <span data-ttu-id="717f4-106">La RCL peut être empaquetée et réutilisée.</span><span class="sxs-lookup"><span data-stu-id="717f4-106">The RCL can be packaged and reused.</span></span> <span data-ttu-id="717f4-107">Les applications peuvent inclure la RCL et remplacer les vues et les pages qu’elle contient.</span><span class="sxs-lookup"><span data-stu-id="717f4-107">Applications can include the RCL and override the views and pages it contains.</span></span> <span data-ttu-id="717f4-108">Quand une vue, une vue partielle ou une page Razor est présente dans l’application web et la RCL, le balisage Razor (fichier *.cshtml*) dans l’application web est prioritaire.</span><span class="sxs-lookup"><span data-stu-id="717f4-108">When a view, partial view, or Razor Page is found in both the web app and the RCL, the Razor markup (*.cshtml* file) in the web app takes precedence.</span></span>

<span data-ttu-id="717f4-109">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/ui-class/samples) ([procédure de téléchargement](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="717f4-109">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/ui-class/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="create-a-class-library-containing-razor-ui"></a><span data-ttu-id="717f4-110">Créer une bibliothèque de classes contenant l’interface utilisateur Razor</span><span class="sxs-lookup"><span data-stu-id="717f4-110">Create a class library containing Razor UI</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="717f4-111">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="717f4-111">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="717f4-112">Dans Visual Studio, dans le menu **Fichier**, sélectionnez **Nouveau** > **Projet**.</span><span class="sxs-lookup"><span data-stu-id="717f4-112">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="717f4-113">Sélectionnez **Nouvelle application web ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="717f4-113">Select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="717f4-114">Nommez la bibliothèque (par exemple, « RazorClassLib ») > **OK**.</span><span class="sxs-lookup"><span data-stu-id="717f4-114">Name the library (for example, "RazorClassLib") > **OK**.</span></span> <span data-ttu-id="717f4-115">Pour éviter une collision de nom de fichier avec la bibliothèque de vues générée, vérifiez que le nom de la bibliothèque ne se termine pas par `.Views`.</span><span class="sxs-lookup"><span data-stu-id="717f4-115">To avoid a file name collision with the generated view library, ensure the library name doesn't end in `.Views`.</span></span>
* <span data-ttu-id="717f4-116">Vérifiez **ASP.NET Core 3,0** ou une version ultérieure est sélectionné.</span><span class="sxs-lookup"><span data-stu-id="717f4-116">Verify **ASP.NET Core 3.0** or later is selected.</span></span>
* <span data-ttu-id="717f4-117">Sélectionnez **bibliothèque de classes Razor** > **OK**.</span><span class="sxs-lookup"><span data-stu-id="717f4-117">Select **Razor Class Library** > **OK**.</span></span>

<span data-ttu-id="717f4-118">Le modèle de bibliothèque de classes Razor (RCL) est par défaut le développement de composants Razor par défaut.</span><span class="sxs-lookup"><span data-stu-id="717f4-118">The Razor class library (RCL) template defaults to Razor component development by default.</span></span> <span data-ttu-id="717f4-119">Une option de modèle dans Visual Studio fournit la prise en charge des modèles pour les pages et les vues.</span><span class="sxs-lookup"><span data-stu-id="717f4-119">A template option in Visual Studio provides template support for pages and views.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="717f4-120">CLI .NET Core</span><span class="sxs-lookup"><span data-stu-id="717f4-120">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="717f4-121">À partir de la ligne de commande, exécutez `dotnet new razorclasslib`.</span><span class="sxs-lookup"><span data-stu-id="717f4-121">From the command line, run `dotnet new razorclasslib`.</span></span> <span data-ttu-id="717f4-122">Exemple :</span><span class="sxs-lookup"><span data-stu-id="717f4-122">For example:</span></span>

```dotnetcli
dotnet new razorclasslib -o RazorUIClassLib
```

<span data-ttu-id="717f4-123">Le modèle de bibliothèque de classes Razor (RCL) est par défaut le développement de composants Razor par défaut.</span><span class="sxs-lookup"><span data-stu-id="717f4-123">The Razor class library (RCL) template defaults to Razor component development by default.</span></span> <span data-ttu-id="717f4-124">Transmettez l’option `--support-pages-and-views` (`dotnet new razorclasslib --support-pages-and-views`) pour assurer la prise en charge des pages et des vues.</span><span class="sxs-lookup"><span data-stu-id="717f4-124">Pass the `--support-pages-and-views` option (`dotnet new razorclasslib --support-pages-and-views`) to provide support for pages and views.</span></span>

<span data-ttu-id="717f4-125">Pour plus d’informations, consultez [dotnet new](/dotnet/core/tools/dotnet-new).</span><span class="sxs-lookup"><span data-stu-id="717f4-125">For more information, see [dotnet new](/dotnet/core/tools/dotnet-new).</span></span> <span data-ttu-id="717f4-126">Pour éviter une collision de nom de fichier avec la bibliothèque de vues générée, vérifiez que le nom de la bibliothèque ne se termine pas par `.Views`.</span><span class="sxs-lookup"><span data-stu-id="717f4-126">To avoid a file name collision with the generated view library, ensure the library name doesn't end in `.Views`.</span></span>

---

<span data-ttu-id="717f4-127">Ajoutez des fichiers Razor à la RCL.</span><span class="sxs-lookup"><span data-stu-id="717f4-127">Add Razor files to the RCL.</span></span>

<span data-ttu-id="717f4-128">Les modèles ASP.NET Core supposent que le contenu RCL se trouve dans le *zones* dossier.</span><span class="sxs-lookup"><span data-stu-id="717f4-128">The ASP.NET Core templates assume the RCL content is in the *Areas* folder.</span></span> <span data-ttu-id="717f4-129">Consultez [RCL pages layout](#rcl-pages-layout) pour créer un RCL qui expose du contenu dans `~/Pages` plutôt que `~/Areas/Pages`.</span><span class="sxs-lookup"><span data-stu-id="717f4-129">See [RCL Pages layout](#rcl-pages-layout) to create an RCL that exposes content in `~/Pages` rather than `~/Areas/Pages`.</span></span>

## <a name="reference-rcl-content"></a><span data-ttu-id="717f4-130">Référencer le contenu RCL</span><span class="sxs-lookup"><span data-stu-id="717f4-130">Reference RCL content</span></span>

<span data-ttu-id="717f4-131">La RCL peut être référencée par :</span><span class="sxs-lookup"><span data-stu-id="717f4-131">The RCL can be referenced by:</span></span>

* <span data-ttu-id="717f4-132">Un package NuGet.</span><span class="sxs-lookup"><span data-stu-id="717f4-132">NuGet package.</span></span> <span data-ttu-id="717f4-133">Consultez [Création de packages NuGet](/nuget/create-packages/creating-a-package), [dotnet add package](/dotnet/core/tools/dotnet-add-package) et [Créer et publier un package NuGet](/nuget/quickstart/create-and-publish-a-package-using-visual-studio).</span><span class="sxs-lookup"><span data-stu-id="717f4-133">See [Creating NuGet packages](/nuget/create-packages/creating-a-package) and [dotnet add package](/dotnet/core/tools/dotnet-add-package) and [Create and publish a NuGet package](/nuget/quickstart/create-and-publish-a-package-using-visual-studio).</span></span>
* <span data-ttu-id="717f4-134">Un fichier *{NomProjet}.csproj*.</span><span class="sxs-lookup"><span data-stu-id="717f4-134">*{ProjectName}.csproj*.</span></span> <span data-ttu-id="717f4-135">Consultez [dotnet-add reference](/dotnet/core/tools/dotnet-add-reference).</span><span class="sxs-lookup"><span data-stu-id="717f4-135">See [dotnet-add reference](/dotnet/core/tools/dotnet-add-reference).</span></span>

## <a name="override-views-partial-views-and-pages"></a><span data-ttu-id="717f4-136">Substituer des vues, des vues partielles et des pages</span><span class="sxs-lookup"><span data-stu-id="717f4-136">Override views, partial views, and pages</span></span>

<span data-ttu-id="717f4-137">Quand une vue, une vue partielle ou une page Razor est présente dans l’application web et la RCL, le balisage Razor (fichier *.cshtml*) dans l’application web est prioritaire.</span><span class="sxs-lookup"><span data-stu-id="717f4-137">When a view, partial view, or Razor Page is found in both the web app and the RCL, the Razor markup (*.cshtml* file) in the web app takes precedence.</span></span> <span data-ttu-id="717f4-138">Par exemple, ajoutez *application Web 1/Areas/MyFeature/pages/Page1. cshtml* à application Web 1, et Page1 dans le application Web 1 aura priorité sur Page1 dans le RCL.</span><span class="sxs-lookup"><span data-stu-id="717f4-138">For example, add *WebApp1/Areas/MyFeature/Pages/Page1.cshtml* to WebApp1, and Page1 in the WebApp1 will take precedence over Page1 in the RCL.</span></span>

<span data-ttu-id="717f4-139">Dans l’exemple proposé sous forme de téléchargement, renommez *WebApp1/Areas/MyFeature2* en *WebApp1/Areas/MyFeature* pour tester la priorité.</span><span class="sxs-lookup"><span data-stu-id="717f4-139">In the sample download, rename *WebApp1/Areas/MyFeature2* to *WebApp1/Areas/MyFeature* to test precedence.</span></span>

<span data-ttu-id="717f4-140">Copiez la vue partielle *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* dans *WebApp1/Areas/MyFeature/Pages/Shared/_Message.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="717f4-140">Copy the *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* partial view to *WebApp1/Areas/MyFeature/Pages/Shared/_Message.cshtml*.</span></span> <span data-ttu-id="717f4-141">Mettez à jour le balisage pour indiquer le nouvel emplacement.</span><span class="sxs-lookup"><span data-stu-id="717f4-141">Update the markup to indicate the new location.</span></span> <span data-ttu-id="717f4-142">Générez et exécutez l’application pour vérifier que la version de la vue partielle de l’application est utilisée.</span><span class="sxs-lookup"><span data-stu-id="717f4-142">Build and run the app to verify the app's version of the partial is being used.</span></span>

### <a name="rcl-pages-layout"></a><span data-ttu-id="717f4-143">Disposition des Pages de RCL</span><span class="sxs-lookup"><span data-stu-id="717f4-143">RCL Pages layout</span></span>

<span data-ttu-id="717f4-144">Pour référence RCL de contenu comme s’il faisait partie de l’application web *Pages* dossier, créez le projet RCL avec la structure de fichier suivante :</span><span class="sxs-lookup"><span data-stu-id="717f4-144">To reference RCL content as though it is part of the web app's *Pages* folder, create the RCL project with the following file structure:</span></span>

* <span data-ttu-id="717f4-145">*RazorUIClassLib/Pages*</span><span class="sxs-lookup"><span data-stu-id="717f4-145">*RazorUIClassLib/Pages*</span></span>
* <span data-ttu-id="717f4-146">*RazorUIClassLib/Pages/Shared*</span><span class="sxs-lookup"><span data-stu-id="717f4-146">*RazorUIClassLib/Pages/Shared*</span></span>

<span data-ttu-id="717f4-147">Supposons que *RazorUIClassLib/Pages/Shared* contient deux fichiers partielles : *_Header.cshtml* et *_Footer.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="717f4-147">Suppose *RazorUIClassLib/Pages/Shared* contains two partial files: *_Header.cshtml* and *_Footer.cshtml*.</span></span> <span data-ttu-id="717f4-148">Le `<partial>` balises peut être ajoutés à *_Layout.cshtml* fichier :</span><span class="sxs-lookup"><span data-stu-id="717f4-148">The `<partial>` tags could be added to *_Layout.cshtml* file:</span></span>

```cshtml
<body>
  <partial name="_Header">
  @RenderBody()
  <partial name="_Footer">
</body>
```

## <a name="create-an-rcl-with-static-assets"></a><span data-ttu-id="717f4-149">Créer un RCL avec des ressources statiques</span><span class="sxs-lookup"><span data-stu-id="717f4-149">Create an RCL with static assets</span></span>

<span data-ttu-id="717f4-150">Un RCL peut nécessiter des ressources statiques auxiliaires qui peuvent être référencées par l’application consommatrice de RCL.</span><span class="sxs-lookup"><span data-stu-id="717f4-150">An RCL may require companion static assets that can be referenced by the consuming app of the RCL.</span></span> <span data-ttu-id="717f4-151">ASP.NET Core permet de créer des RCLs qui incluent des ressources statiques disponibles pour une application consommatrice.</span><span class="sxs-lookup"><span data-stu-id="717f4-151">ASP.NET Core allows creating RCLs that include static assets that are available to a consuming app.</span></span>

<span data-ttu-id="717f4-152">Pour inclure des ressources d’accompagnement dans le cadre d’un RCL, créez un dossier *wwwroot* dans la bibliothèque de classes et incluez tous les fichiers nécessaires dans ce dossier.</span><span class="sxs-lookup"><span data-stu-id="717f4-152">To include companion assets as part of an RCL, create a *wwwroot* folder in the class library and include any required files in that folder.</span></span>

<span data-ttu-id="717f4-153">Lors de la compression d’un RCL, toutes les ressources associées dans le dossier *wwwroot* sont automatiquement incluses dans le package.</span><span class="sxs-lookup"><span data-stu-id="717f4-153">When packing an RCL, all companion assets in the *wwwroot* folder are automatically included in the package.</span></span>

### <a name="exclude-static-assets"></a><span data-ttu-id="717f4-154">Exclure des ressources statiques</span><span class="sxs-lookup"><span data-stu-id="717f4-154">Exclude static assets</span></span>

<span data-ttu-id="717f4-155">Pour exclure des ressources statiques, ajoutez le chemin d’exclusion souhaité au groupe de propriétés `$(DefaultItemExcludes)` dans le fichier projet.</span><span class="sxs-lookup"><span data-stu-id="717f4-155">To exclude static assets, add the desired exclusion path to the `$(DefaultItemExcludes)` property group in the project file.</span></span> <span data-ttu-id="717f4-156">Séparez les entrées par un point-virgule (`;`).</span><span class="sxs-lookup"><span data-stu-id="717f4-156">Separate entries with a semicolon (`;`).</span></span>

<span data-ttu-id="717f4-157">Dans l’exemple suivant, la feuille de style *lib. CSS* du dossier *wwwroot* n’est pas considérée comme une ressource statique et n’est pas incluse dans le RCL publié :</span><span class="sxs-lookup"><span data-stu-id="717f4-157">In the following example, the *lib.css* stylesheet in the *wwwroot* folder isn't considered a static asset and isn't included in the published RCL:</span></span>

```xml
<PropertyGroup>
  <DefaultItemExcludes>$(DefaultItemExcludes);wwwroot\lib.css</DefaultItemExcludes>
</PropertyGroup>
```

### <a name="typescript-integration"></a><span data-ttu-id="717f4-158">Intégration de la machine à écrire</span><span class="sxs-lookup"><span data-stu-id="717f4-158">Typescript integration</span></span>

<span data-ttu-id="717f4-159">Pour inclure des fichiers de machine à écrire dans un RCL :</span><span class="sxs-lookup"><span data-stu-id="717f4-159">To include TypeScript files in an RCL:</span></span>

1. <span data-ttu-id="717f4-160">Placez les fichiers de machine à écrire ( *. TS*) en dehors du dossier *wwwroot* .</span><span class="sxs-lookup"><span data-stu-id="717f4-160">Place the TypeScript files (*.ts*) outside of the *wwwroot* folder.</span></span> <span data-ttu-id="717f4-161">Par exemple, placez les fichiers dans un dossier *client* .</span><span class="sxs-lookup"><span data-stu-id="717f4-161">For example, place the files in a *Client* folder.</span></span>

1. <span data-ttu-id="717f4-162">Configurez la sortie de génération de machine à écrire pour le dossier *wwwroot* .</span><span class="sxs-lookup"><span data-stu-id="717f4-162">Configure the TypeScript build output for the *wwwroot* folder.</span></span> <span data-ttu-id="717f4-163">Définissez la propriété `TypescriptOutDir` à l’intérieur d’une `PropertyGroup` dans le fichier projet :</span><span class="sxs-lookup"><span data-stu-id="717f4-163">Set the `TypescriptOutDir` property inside of a `PropertyGroup` in the project file:</span></span>

   ```xml
   <TypescriptOutDir>wwwroot</TypescriptOutDir>
   ```

1. <span data-ttu-id="717f4-164">Incluez la cible de la machine à écrire en tant que dépendance de la cible `ResolveCurrentProjectStaticWebAssets` en ajoutant la cible suivante à l’intérieur d’une `PropertyGroup` dans le fichier projet :</span><span class="sxs-lookup"><span data-stu-id="717f4-164">Include the TypeScript target as a dependency of the `ResolveCurrentProjectStaticWebAssets` target by adding the following target inside of a `PropertyGroup` in the project file:</span></span>

   ```xml
   <ResolveCurrentProjectStaticWebAssetsInputsDependsOn>
     TypeScriptCompile;
     $(ResolveCurrentProjectStaticWebAssetsInputs)
   </ResolveCurrentProjectStaticWebAssetsInputsDependsOn>
   ```

### <a name="consume-content-from-a-referenced-rcl"></a><span data-ttu-id="717f4-165">Consommer du contenu à partir d’un RCL référencé</span><span class="sxs-lookup"><span data-stu-id="717f4-165">Consume content from a referenced RCL</span></span>

<span data-ttu-id="717f4-166">Les fichiers inclus dans le dossier *wwwroot* du RCL sont exposés à l’application consommatrice sous le préfixe `_content/{LIBRARY NAME}/`.</span><span class="sxs-lookup"><span data-stu-id="717f4-166">The files included in the *wwwroot* folder of the RCL are exposed to the consuming app under the prefix `_content/{LIBRARY NAME}/`.</span></span> <span data-ttu-id="717f4-167">Par exemple, une bibliothèque nommée *Razor. class. lib* génère un chemin d’accès au contenu statique à `_content/Razor.Class.Lib/`.</span><span class="sxs-lookup"><span data-stu-id="717f4-167">For example, a library named *Razor.Class.Lib* results in a path to static content at `_content/Razor.Class.Lib/`.</span></span>

<span data-ttu-id="717f4-168">L’application consommatrice fait référence aux ressources statiques fournies par la bibliothèque avec `<script>`, `<style>`, `<img>` et d’autres balises HTML.</span><span class="sxs-lookup"><span data-stu-id="717f4-168">The consuming app references static assets provided by the library with `<script>`, `<style>`, `<img>`, and other HTML tags.</span></span> <span data-ttu-id="717f4-169">L’application consommatrice doit avoir la [prise en charge des fichiers statiques](xref:fundamentals/static-files) activée dans `Startup.Configure` :</span><span class="sxs-lookup"><span data-stu-id="717f4-169">The consuming app must have [static file support](xref:fundamentals/static-files) enabled in `Startup.Configure`:</span></span>

```csharp
public void Configure(IApplicationBuilder app, IWebHostEnvironment env)
{
    ...

    app.UseStaticFiles();

    ...
}
```

<span data-ttu-id="717f4-170">Lors de l’exécution de l’application consommatrice à partir de la sortie de génération (`dotnet run`), les ressources Web statiques sont activées par défaut dans l’environnement de développement.</span><span class="sxs-lookup"><span data-stu-id="717f4-170">When running the consuming app from build output (`dotnet run`), static web assets are enabled by default in the Development environment.</span></span> <span data-ttu-id="717f4-171">Pour prendre en charge les ressources dans d’autres environnements lors de l’exécution à partir de la sortie de génération, appelez `UseStaticWebAssets` sur le générateur d’ordinateur hôte dans *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="717f4-171">To support assets in other environments when running from build output, call `UseStaticWebAssets` on the host builder in *Program.cs*:</span></span>

```csharp
using Microsoft.AspNetCore.Hosting;
using Microsoft.Extensions.Hosting;

public class Program
{
    public static void Main(string[] args)
    {
        CreateHostBuilder(args).Build().Run();
    }

    public static IHostBuilder CreateHostBuilder(string[] args) =>
        Host.CreateDefaultBuilder(args)
            .ConfigureWebHostDefaults(webBuilder =>
            {
                webBuilder.UseStaticWebAssets();
                webBuilder.UseStartup<Startup>();
            });
}
```

<span data-ttu-id="717f4-172">L’appel de `UseStaticWebAssets` n’est pas nécessaire lors de l’exécution d’une application à partir de la sortie publiée (`dotnet publish`).</span><span class="sxs-lookup"><span data-stu-id="717f4-172">Calling `UseStaticWebAssets` isn't required when running an app from published output (`dotnet publish`).</span></span>

### <a name="multi-project-development-flow"></a><span data-ttu-id="717f4-173">Déroulement du développement de projets multiples</span><span class="sxs-lookup"><span data-stu-id="717f4-173">Multi-project development flow</span></span>

<span data-ttu-id="717f4-174">Lorsque l’application consommatrice s’exécute :</span><span class="sxs-lookup"><span data-stu-id="717f4-174">When the consuming app runs:</span></span>

* <span data-ttu-id="717f4-175">Les ressources du RCL restent dans leurs dossiers d’origine.</span><span class="sxs-lookup"><span data-stu-id="717f4-175">The assets in the RCL stay in their original folders.</span></span> <span data-ttu-id="717f4-176">Les ressources ne sont pas déplacées vers l’application consommatrice.</span><span class="sxs-lookup"><span data-stu-id="717f4-176">The assets aren't moved to the consuming app.</span></span>
* <span data-ttu-id="717f4-177">Toute modification dans le dossier *wwwroot* de RCL est reflétée dans l’application consommatrice une fois que le RCL est reconstruit et sans régénérer l’application consommateur.</span><span class="sxs-lookup"><span data-stu-id="717f4-177">Any change within the RCL's *wwwroot* folder is reflected in the consuming app after the RCL is rebuilt and without rebuilding the consuming app.</span></span>

<span data-ttu-id="717f4-178">Lorsque le RCL est généré, un manifeste qui décrit les emplacements des ressources Web statiques est généré.</span><span class="sxs-lookup"><span data-stu-id="717f4-178">When the RCL is built, a manifest is produced that describes the static web asset locations.</span></span> <span data-ttu-id="717f4-179">L’application consommatrice lit le manifeste au moment de l’exécution pour consommer les ressources des packages et des projets référencés.</span><span class="sxs-lookup"><span data-stu-id="717f4-179">The consuming app reads the manifest at runtime to consume the assets from referenced projects and packages.</span></span> <span data-ttu-id="717f4-180">Lorsqu’un nouvel élément multimédia est ajouté à un RCL, le RCL doit être régénéré pour mettre à jour son manifeste avant qu’une application consommatrice puisse accéder au nouvel élément multimédia.</span><span class="sxs-lookup"><span data-stu-id="717f4-180">When a new asset is added to an RCL, the RCL must be rebuilt to update its manifest before a consuming app can access the new asset.</span></span>

### <a name="publish"></a><span data-ttu-id="717f4-181">Publier</span><span class="sxs-lookup"><span data-stu-id="717f4-181">Publish</span></span>

<span data-ttu-id="717f4-182">Lorsque l’application est publiée, les ressources d’accompagnement de tous les packages et projets référencés sont copiées dans le dossier *wwwroot* de l’application publiée sous `_content/{LIBRARY NAME}/`.</span><span class="sxs-lookup"><span data-stu-id="717f4-182">When the app is published, the companion assets from all referenced projects and packages are copied into the *wwwroot* folder of the published app under `_content/{LIBRARY NAME}/`.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="717f4-183">Les vues Razor, les pages, les contrôleurs, les modèles de page, les [composants Razor](xref:blazor/class-libraries), les [composants de vue](xref:mvc/views/view-components)et les modèles de données peuvent être intégrés dans une bibliothèque de classes Razor (RCL).</span><span class="sxs-lookup"><span data-stu-id="717f4-183">Razor views, pages, controllers, page models, [Razor components](xref:blazor/class-libraries), [View components](xref:mvc/views/view-components), and data models can be built into a Razor class library (RCL).</span></span> <span data-ttu-id="717f4-184">La RCL peut être empaquetée et réutilisée.</span><span class="sxs-lookup"><span data-stu-id="717f4-184">The RCL can be packaged and reused.</span></span> <span data-ttu-id="717f4-185">Les applications peuvent inclure la RCL et remplacer les vues et les pages qu’elle contient.</span><span class="sxs-lookup"><span data-stu-id="717f4-185">Applications can include the RCL and override the views and pages it contains.</span></span> <span data-ttu-id="717f4-186">Quand une vue, une vue partielle ou une page Razor est présente dans l’application web et la RCL, le balisage Razor (fichier *.cshtml*) dans l’application web est prioritaire.</span><span class="sxs-lookup"><span data-stu-id="717f4-186">When a view, partial view, or Razor Page is found in both the web app and the RCL, the Razor markup (*.cshtml* file) in the web app takes precedence.</span></span>

<span data-ttu-id="717f4-187">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/ui-class/samples) ([procédure de téléchargement](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="717f4-187">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/ui-class/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="create-a-class-library-containing-razor-ui"></a><span data-ttu-id="717f4-188">Créer une bibliothèque de classes contenant l’interface utilisateur Razor</span><span class="sxs-lookup"><span data-stu-id="717f4-188">Create a class library containing Razor UI</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="717f4-189">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="717f4-189">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="717f4-190">Dans Visual Studio, dans le menu **Fichier**, sélectionnez **Nouveau** > **Projet**.</span><span class="sxs-lookup"><span data-stu-id="717f4-190">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="717f4-191">Sélectionnez **Nouvelle application web ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="717f4-191">Select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="717f4-192">Nommez la bibliothèque (par exemple, « RazorClassLib ») > **OK**.</span><span class="sxs-lookup"><span data-stu-id="717f4-192">Name the library (for example, "RazorClassLib") > **OK**.</span></span> <span data-ttu-id="717f4-193">Pour éviter une collision de nom de fichier avec la bibliothèque de vues générée, vérifiez que le nom de la bibliothèque ne se termine pas par `.Views`.</span><span class="sxs-lookup"><span data-stu-id="717f4-193">To avoid a file name collision with the generated view library, ensure the library name doesn't end in `.Views`.</span></span>
* <span data-ttu-id="717f4-194">Vérifiez que **ASP.NET Core 2.1** ou ultérieur est sélectionné.</span><span class="sxs-lookup"><span data-stu-id="717f4-194">Verify **ASP.NET Core 2.1** or later is selected.</span></span>
* <span data-ttu-id="717f4-195">Sélectionnez **bibliothèque de classes Razor** > **OK**.</span><span class="sxs-lookup"><span data-stu-id="717f4-195">Select **Razor Class Library** > **OK**.</span></span>

<span data-ttu-id="717f4-196">Un RCL a le fichier projet suivant :</span><span class="sxs-lookup"><span data-stu-id="717f4-196">An RCL has the following project file:</span></span>

[!code-xml[](ui-class/samples/cli/RazorUIClassLib/RazorUIClassLib.csproj)]

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="717f4-197">CLI .NET Core</span><span class="sxs-lookup"><span data-stu-id="717f4-197">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="717f4-198">À partir de la ligne de commande, exécutez `dotnet new razorclasslib`.</span><span class="sxs-lookup"><span data-stu-id="717f4-198">From the command line, run `dotnet new razorclasslib`.</span></span> <span data-ttu-id="717f4-199">Exemple :</span><span class="sxs-lookup"><span data-stu-id="717f4-199">For example:</span></span>

```dotnetcli
dotnet new razorclasslib -o RazorUIClassLib
```

<span data-ttu-id="717f4-200">Pour plus d’informations, consultez [dotnet new](/dotnet/core/tools/dotnet-new).</span><span class="sxs-lookup"><span data-stu-id="717f4-200">For more information, see [dotnet new](/dotnet/core/tools/dotnet-new).</span></span> <span data-ttu-id="717f4-201">Pour éviter une collision de nom de fichier avec la bibliothèque de vues générée, vérifiez que le nom de la bibliothèque ne se termine pas par `.Views`.</span><span class="sxs-lookup"><span data-stu-id="717f4-201">To avoid a file name collision with the generated view library, ensure the library name doesn't end in `.Views`.</span></span>

---

<span data-ttu-id="717f4-202">Ajoutez des fichiers Razor à la RCL.</span><span class="sxs-lookup"><span data-stu-id="717f4-202">Add Razor files to the RCL.</span></span>

<span data-ttu-id="717f4-203">Les modèles ASP.NET Core supposent que le contenu RCL se trouve dans le *zones* dossier.</span><span class="sxs-lookup"><span data-stu-id="717f4-203">The ASP.NET Core templates assume the RCL content is in the *Areas* folder.</span></span> <span data-ttu-id="717f4-204">Consultez [RCL pages layout](#rcl-pages-layout) pour créer un RCL qui expose du contenu dans `~/Pages` plutôt que `~/Areas/Pages`.</span><span class="sxs-lookup"><span data-stu-id="717f4-204">See [RCL Pages layout](#rcl-pages-layout) to create an RCL that exposes content in `~/Pages` rather than `~/Areas/Pages`.</span></span>

## <a name="reference-rcl-content"></a><span data-ttu-id="717f4-205">Référencer le contenu RCL</span><span class="sxs-lookup"><span data-stu-id="717f4-205">Reference RCL content</span></span>

<span data-ttu-id="717f4-206">La RCL peut être référencée par :</span><span class="sxs-lookup"><span data-stu-id="717f4-206">The RCL can be referenced by:</span></span>

* <span data-ttu-id="717f4-207">Un package NuGet.</span><span class="sxs-lookup"><span data-stu-id="717f4-207">NuGet package.</span></span> <span data-ttu-id="717f4-208">Consultez [Création de packages NuGet](/nuget/create-packages/creating-a-package), [dotnet add package](/dotnet/core/tools/dotnet-add-package) et [Créer et publier un package NuGet](/nuget/quickstart/create-and-publish-a-package-using-visual-studio).</span><span class="sxs-lookup"><span data-stu-id="717f4-208">See [Creating NuGet packages](/nuget/create-packages/creating-a-package) and [dotnet add package](/dotnet/core/tools/dotnet-add-package) and [Create and publish a NuGet package](/nuget/quickstart/create-and-publish-a-package-using-visual-studio).</span></span>
* <span data-ttu-id="717f4-209">Un fichier *{NomProjet}.csproj*.</span><span class="sxs-lookup"><span data-stu-id="717f4-209">*{ProjectName}.csproj*.</span></span> <span data-ttu-id="717f4-210">Consultez [dotnet-add reference](/dotnet/core/tools/dotnet-add-reference).</span><span class="sxs-lookup"><span data-stu-id="717f4-210">See [dotnet-add reference](/dotnet/core/tools/dotnet-add-reference).</span></span>

## <a name="walkthrough-create-an-rcl-project-and-use-from-a-razor-pages-project"></a><span data-ttu-id="717f4-211">Procédure pas à pas : Créer un projet RCL et l’utiliser à partir d’un projet Razor Pages</span><span class="sxs-lookup"><span data-stu-id="717f4-211">Walkthrough: Create an RCL project and use from a Razor Pages project</span></span>

<span data-ttu-id="717f4-212">Vous pouvez télécharger et tester le [projet complet](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/ui-class/samples) au lieu de le créer de toutes pièces.</span><span class="sxs-lookup"><span data-stu-id="717f4-212">You can download the [complete project](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/ui-class/samples) and test it rather than creating it.</span></span> <span data-ttu-id="717f4-213">L’exemple proposé sous forme de téléchargement contient du code et des liens supplémentaires qui facilitent le test du projet.</span><span class="sxs-lookup"><span data-stu-id="717f4-213">The sample download contains additional code and links that make the project easy to test.</span></span> <span data-ttu-id="717f4-214">Si vous souhaitez commenter le mode d’obtention des exemples (téléchargement ou création au moyen d’instructions détaillées), entrez vos commentaires dans [ce problème GitHub](https://github.com/aspnet/AspNetCore.Docs/issues/6098).</span><span class="sxs-lookup"><span data-stu-id="717f4-214">You can leave feedback in [this GitHub issue](https://github.com/aspnet/AspNetCore.Docs/issues/6098) with your comments on download samples versus step-by-step instructions.</span></span>

### <a name="test-the-download-app"></a><span data-ttu-id="717f4-215">Tester l’application téléchargée</span><span class="sxs-lookup"><span data-stu-id="717f4-215">Test the download app</span></span>

<span data-ttu-id="717f4-216">Si vous n’avez pas téléchargé l’application complète et que vous souhaitez créer le projet pas à pas, passez à la [section suivante](#create-an-rcl).</span><span class="sxs-lookup"><span data-stu-id="717f4-216">If you haven't downloaded the completed app and would rather create the walkthrough project, skip to the [next section](#create-an-rcl).</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="717f4-217">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="717f4-217">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="717f4-218">Ouvrez le fichier *.sln* dans Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="717f4-218">Open the *.sln* file in Visual Studio.</span></span> <span data-ttu-id="717f4-219">Exécuter l’application.</span><span class="sxs-lookup"><span data-stu-id="717f4-219">Run the app.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="717f4-220">CLI .NET Core</span><span class="sxs-lookup"><span data-stu-id="717f4-220">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="717f4-221">À partir d’une invite de commandes dans le répertoire *cli*, générez la RCL et l’application web.</span><span class="sxs-lookup"><span data-stu-id="717f4-221">From a command prompt in the *cli* directory, build the RCL and web app.</span></span>

```dotnetcli
dotnet build
```

<span data-ttu-id="717f4-222">Passez au répertoire *WebApp1* et exécutez l’application :</span><span class="sxs-lookup"><span data-stu-id="717f4-222">Move to the *WebApp1* directory and run the app:</span></span>

```dotnetcli
dotnet run
```

---

<span data-ttu-id="717f4-223">Suivez les instructions contenues dans [Test WebApp1](#test-webapp1)</span><span class="sxs-lookup"><span data-stu-id="717f4-223">Follow the instructions in [Test WebApp1](#test-webapp1)</span></span>

## <a name="create-an-rcl"></a><span data-ttu-id="717f4-224">Créer un RCL</span><span class="sxs-lookup"><span data-stu-id="717f4-224">Create an RCL</span></span>

<span data-ttu-id="717f4-225">Dans cette section, un RCL est créé.</span><span class="sxs-lookup"><span data-stu-id="717f4-225">In this section, an RCL is created.</span></span> <span data-ttu-id="717f4-226">Des fichiers Razor sont ajoutés à la RCL.</span><span class="sxs-lookup"><span data-stu-id="717f4-226">Razor files are added to the RCL.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="717f4-227">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="717f4-227">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="717f4-228">Créez le projet RCL :</span><span class="sxs-lookup"><span data-stu-id="717f4-228">Create the RCL project:</span></span>

* <span data-ttu-id="717f4-229">Dans Visual Studio, dans le menu **Fichier**, sélectionnez **Nouveau** > **Projet**.</span><span class="sxs-lookup"><span data-stu-id="717f4-229">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="717f4-230">Sélectionnez **Nouvelle application web ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="717f4-230">Select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="717f4-231">Nommez l’application **RazorUIClassLib** > **OK**.</span><span class="sxs-lookup"><span data-stu-id="717f4-231">Name the app **RazorUIClassLib** > **OK**.</span></span>
* <span data-ttu-id="717f4-232">Vérifiez que **ASP.NET Core 2.1** ou ultérieur est sélectionné.</span><span class="sxs-lookup"><span data-stu-id="717f4-232">Verify **ASP.NET Core 2.1** or later is selected.</span></span>
* <span data-ttu-id="717f4-233">Sélectionnez **bibliothèque de classes Razor** > **OK**.</span><span class="sxs-lookup"><span data-stu-id="717f4-233">Select **Razor Class Library** > **OK**.</span></span>
* <span data-ttu-id="717f4-234">Ajoutez un fichier de vue partielle Razor nommé *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="717f4-234">Add a Razor partial view file named *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml*.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="717f4-235">CLI .NET Core</span><span class="sxs-lookup"><span data-stu-id="717f4-235">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="717f4-236">À partir de la ligne de commande, exécutez ce qui suit :</span><span class="sxs-lookup"><span data-stu-id="717f4-236">From the command line, run the following:</span></span>

```dotnetcli
dotnet new razorclasslib -o RazorUIClassLib
dotnet new page -n _Message -np -o RazorUIClassLib/Areas/MyFeature/Pages/Shared
dotnet new viewstart -o RazorUIClassLib/Areas/MyFeature/Pages
```

<span data-ttu-id="717f4-237">Les commandes précédentes :</span><span class="sxs-lookup"><span data-stu-id="717f4-237">The preceding commands:</span></span>

* <span data-ttu-id="717f4-238">Crée le `RazorUIClassLib` RCL.</span><span class="sxs-lookup"><span data-stu-id="717f4-238">Creates the `RazorUIClassLib` RCL.</span></span>
* <span data-ttu-id="717f4-239">Créent une page _Message Razor et l’ajoutent à la RCL.</span><span class="sxs-lookup"><span data-stu-id="717f4-239">Creates a Razor _Message page, and adds it to the RCL.</span></span> <span data-ttu-id="717f4-240">Le paramètre `-np` crée la page sans `PageModel`.</span><span class="sxs-lookup"><span data-stu-id="717f4-240">The `-np` parameter creates the page without a `PageModel`.</span></span>
* <span data-ttu-id="717f4-241">Crée un [_ViewStart.cshtml](xref:mvc/views/layout#running-code-before-each-view) de fichiers et l’ajoute à la RCL.</span><span class="sxs-lookup"><span data-stu-id="717f4-241">Creates a [_ViewStart.cshtml](xref:mvc/views/layout#running-code-before-each-view) file and adds it to the RCL.</span></span>

<span data-ttu-id="717f4-242">Le *_ViewStart.cshtml* fichier est nécessaire pour utiliser la disposition du projet de Pages Razor (qui est ajoutée à la section suivante).</span><span class="sxs-lookup"><span data-stu-id="717f4-242">The *_ViewStart.cshtml* file is required to use the layout of the Razor Pages project (which is added in the next section).</span></span>

---

### <a name="add-razor-files-and-folders-to-the-project"></a><span data-ttu-id="717f4-243">Ajouter les fichiers Razor et des dossiers au projet</span><span class="sxs-lookup"><span data-stu-id="717f4-243">Add Razor files and folders to the project</span></span>

* <span data-ttu-id="717f4-244">Remplacez le balisage dans *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* par le code suivant :</span><span class="sxs-lookup"><span data-stu-id="717f4-244">Replace the markup in *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* with the following code:</span></span>

  [!code-cshtml[](ui-class/samples/cli/RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml)]

* <span data-ttu-id="717f4-245">Remplacez le balisage dans *RazorUIClassLib/Areas/MyFeature/Pages/Page1.cshtml* par le code suivant :</span><span class="sxs-lookup"><span data-stu-id="717f4-245">Replace the markup in *RazorUIClassLib/Areas/MyFeature/Pages/Page1.cshtml* with the following code:</span></span>

  [!code-cshtml[](ui-class/samples/cli/RazorUIClassLib/Areas/MyFeature/Pages/Page1.cshtml)]

  <span data-ttu-id="717f4-246">`@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers` est nécessaire pour utiliser la vue partielle (`<partial name="_Message" />`).</span><span class="sxs-lookup"><span data-stu-id="717f4-246">`@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers` is required to use the partial view (`<partial name="_Message" />`).</span></span> <span data-ttu-id="717f4-247">Au lieu d’inclure la directive `@addTagHelper`, vous pouvez ajouter un fichier *_ViewImports.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="717f4-247">Rather than including the `@addTagHelper` directive, you can add a *_ViewImports.cshtml* file.</span></span> <span data-ttu-id="717f4-248">Exemple :</span><span class="sxs-lookup"><span data-stu-id="717f4-248">For example:</span></span>

  ```dotnetcli
  dotnet new viewimports -o RazorUIClassLib/Areas/MyFeature/Pages
  ```

  <span data-ttu-id="717f4-249">Pour plus d’informations sur *_ViewImports.cshtml*, consultez [importation de Directives partagées](xref:mvc/views/layout#importing-shared-directives)</span><span class="sxs-lookup"><span data-stu-id="717f4-249">For more information on *_ViewImports.cshtml*, see [Importing Shared Directives](xref:mvc/views/layout#importing-shared-directives)</span></span>

* <span data-ttu-id="717f4-250">Générez la bibliothèque de classes pour vérifier l’absence d’erreurs de compilateur :</span><span class="sxs-lookup"><span data-stu-id="717f4-250">Build the class library to verify there are no compiler errors:</span></span>

  ```dotnetcli
  dotnet build RazorUIClassLib
  ```

<span data-ttu-id="717f4-251">La sortie de build contient *RazorUIClassLib.dll* et *RazorUIClassLib.Views.dll*.</span><span class="sxs-lookup"><span data-stu-id="717f4-251">The build output contains *RazorUIClassLib.dll* and *RazorUIClassLib.Views.dll*.</span></span> <span data-ttu-id="717f4-252">*RazorUIClassLib.Views.dll* contient le contenu Razor compilé.</span><span class="sxs-lookup"><span data-stu-id="717f4-252">*RazorUIClassLib.Views.dll* contains the compiled Razor content.</span></span>

### <a name="use-the-razor-ui-library-from-a-razor-pages-project"></a><span data-ttu-id="717f4-253">Utiliser la bibliothèque de l’interface utilisateur Razor à partir d’un projet Razor Pages</span><span class="sxs-lookup"><span data-stu-id="717f4-253">Use the Razor UI library from a Razor Pages project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="717f4-254">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="717f4-254">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="717f4-255">Créez l’application web Razor Pages :</span><span class="sxs-lookup"><span data-stu-id="717f4-255">Create the Razor Pages web app:</span></span>

* <span data-ttu-id="717f4-256">Dans **Explorateur de solutions**, cliquez avec le bouton droit sur la solution > **Ajouter** >  **nouveau projet**.</span><span class="sxs-lookup"><span data-stu-id="717f4-256">From **Solution Explorer**, right-click the solution > **Add** >  **New Project**.</span></span>
* <span data-ttu-id="717f4-257">Sélectionnez **Nouvelle application web ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="717f4-257">Select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="717f4-258">Nommez l’application **WebApp1**.</span><span class="sxs-lookup"><span data-stu-id="717f4-258">Name the app **WebApp1**.</span></span>
* <span data-ttu-id="717f4-259">Vérifiez que **ASP.NET Core 2.1** ou ultérieur est sélectionné.</span><span class="sxs-lookup"><span data-stu-id="717f4-259">Verify **ASP.NET Core 2.1** or later is selected.</span></span>
* <span data-ttu-id="717f4-260">Sélectionnez **application Web** > **OK**.</span><span class="sxs-lookup"><span data-stu-id="717f4-260">Select **Web Application** > **OK**.</span></span>

* <span data-ttu-id="717f4-261">Dans **l’Explorateur de solutions**, cliquez avec le bouton droit sur **WebApp1**, puis sélectionnez **Définir comme projet de démarrage**.</span><span class="sxs-lookup"><span data-stu-id="717f4-261">From **Solution Explorer**, right-click on **WebApp1** and select **Set as StartUp Project**.</span></span>
* <span data-ttu-id="717f4-262">Dans **Explorateur de solutions**, cliquez avec le bouton droit sur **application Web 1** et sélectionnez **dépendances de build** > **dépendances de projet**.</span><span class="sxs-lookup"><span data-stu-id="717f4-262">From **Solution Explorer**, right-click on **WebApp1** and select **Build Dependencies** > **Project Dependencies**.</span></span>
* <span data-ttu-id="717f4-263">Cochez **RazorUIClassLib** comme dépendance de **WebApp1**.</span><span class="sxs-lookup"><span data-stu-id="717f4-263">Check **RazorUIClassLib** as a dependency of **WebApp1**.</span></span>
* <span data-ttu-id="717f4-264">Dans **Explorateur de solutions**, cliquez avec le bouton droit sur **application Web 1** et sélectionnez **Ajouter** > **référence**.</span><span class="sxs-lookup"><span data-stu-id="717f4-264">From **Solution Explorer**, right-click on **WebApp1** and select **Add** > **Reference**.</span></span>
* <span data-ttu-id="717f4-265">Dans la boîte de dialogue **Gestionnaire de références** , activez la case à cocher **RazorUIClassLib** > **OK**.</span><span class="sxs-lookup"><span data-stu-id="717f4-265">In the **Reference Manager** dialog, check **RazorUIClassLib** > **OK**.</span></span>

<span data-ttu-id="717f4-266">Exécuter l’application.</span><span class="sxs-lookup"><span data-stu-id="717f4-266">Run the app.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="717f4-267">CLI .NET Core</span><span class="sxs-lookup"><span data-stu-id="717f4-267">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="717f4-268">Créez un Razor Pages application Web et un fichier solution contenant l’application Razor Pages et le RCL :</span><span class="sxs-lookup"><span data-stu-id="717f4-268">Create a Razor Pages web app and a solution file containing the Razor Pages app and the RCL:</span></span>

```dotnetcli
dotnet new webapp -o WebApp1
dotnet new sln
dotnet sln add WebApp1
dotnet sln add RazorUIClassLib
dotnet add WebApp1 reference RazorUIClassLib
```

<span data-ttu-id="717f4-269">Générez et exécutez l’application web :</span><span class="sxs-lookup"><span data-stu-id="717f4-269">Build and run the web app:</span></span>

```dotnetcli
cd WebApp1
dotnet run
```

---

### <a name="test-webapp1"></a><span data-ttu-id="717f4-270">Tester WebApp1</span><span class="sxs-lookup"><span data-stu-id="717f4-270">Test WebApp1</span></span>

<span data-ttu-id="717f4-271">Accédez à `/MyFeature/Page1` pour vérifier que la bibliothèque de classes de l’interface utilisateur Razor est en cours d’utilisation.</span><span class="sxs-lookup"><span data-stu-id="717f4-271">Browse to `/MyFeature/Page1` to verify that the Razor UI class library is in use.</span></span>

## <a name="override-views-partial-views-and-pages"></a><span data-ttu-id="717f4-272">Substituer des vues, des vues partielles et des pages</span><span class="sxs-lookup"><span data-stu-id="717f4-272">Override views, partial views, and pages</span></span>

<span data-ttu-id="717f4-273">Quand une vue, une vue partielle ou une page Razor est présente dans l’application web et la RCL, le balisage Razor (fichier *.cshtml*) dans l’application web est prioritaire.</span><span class="sxs-lookup"><span data-stu-id="717f4-273">When a view, partial view, or Razor Page is found in both the web app and the RCL, the Razor markup (*.cshtml* file) in the web app takes precedence.</span></span> <span data-ttu-id="717f4-274">Par exemple, ajoutez *application Web 1/Areas/MyFeature/pages/Page1. cshtml* à application Web 1, et Page1 dans le application Web 1 aura priorité sur Page1 dans le RCL.</span><span class="sxs-lookup"><span data-stu-id="717f4-274">For example, add *WebApp1/Areas/MyFeature/Pages/Page1.cshtml* to WebApp1, and Page1 in the WebApp1 will take precedence over Page1 in the RCL.</span></span>

<span data-ttu-id="717f4-275">Dans l’exemple proposé sous forme de téléchargement, renommez *WebApp1/Areas/MyFeature2* en *WebApp1/Areas/MyFeature* pour tester la priorité.</span><span class="sxs-lookup"><span data-stu-id="717f4-275">In the sample download, rename *WebApp1/Areas/MyFeature2* to *WebApp1/Areas/MyFeature* to test precedence.</span></span>

<span data-ttu-id="717f4-276">Copiez la vue partielle *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* dans *WebApp1/Areas/MyFeature/Pages/Shared/_Message.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="717f4-276">Copy the *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* partial view to *WebApp1/Areas/MyFeature/Pages/Shared/_Message.cshtml*.</span></span> <span data-ttu-id="717f4-277">Mettez à jour le balisage pour indiquer le nouvel emplacement.</span><span class="sxs-lookup"><span data-stu-id="717f4-277">Update the markup to indicate the new location.</span></span> <span data-ttu-id="717f4-278">Générez et exécutez l’application pour vérifier que la version de la vue partielle de l’application est utilisée.</span><span class="sxs-lookup"><span data-stu-id="717f4-278">Build and run the app to verify the app's version of the partial is being used.</span></span>

### <a name="rcl-pages-layout"></a><span data-ttu-id="717f4-279">Disposition des Pages de RCL</span><span class="sxs-lookup"><span data-stu-id="717f4-279">RCL Pages layout</span></span>

<span data-ttu-id="717f4-280">Pour référence RCL de contenu comme s’il faisait partie de l’application web *Pages* dossier, créez le projet RCL avec la structure de fichier suivante :</span><span class="sxs-lookup"><span data-stu-id="717f4-280">To reference RCL content as though it is part of the web app's *Pages* folder, create the RCL project with the following file structure:</span></span>

* <span data-ttu-id="717f4-281">*RazorUIClassLib/Pages*</span><span class="sxs-lookup"><span data-stu-id="717f4-281">*RazorUIClassLib/Pages*</span></span>
* <span data-ttu-id="717f4-282">*RazorUIClassLib/Pages/Shared*</span><span class="sxs-lookup"><span data-stu-id="717f4-282">*RazorUIClassLib/Pages/Shared*</span></span>

<span data-ttu-id="717f4-283">Supposons que *RazorUIClassLib/Pages/Shared* contient deux fichiers partielles : *_Header.cshtml* et *_Footer.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="717f4-283">Suppose *RazorUIClassLib/Pages/Shared* contains two partial files: *_Header.cshtml* and *_Footer.cshtml*.</span></span> <span data-ttu-id="717f4-284">Le `<partial>` balises peut être ajoutés à *_Layout.cshtml* fichier :</span><span class="sxs-lookup"><span data-stu-id="717f4-284">The `<partial>` tags could be added to *_Layout.cshtml* file:</span></span>

```cshtml
<body>
  <partial name="_Header">
  @RenderBody()
  <partial name="_Footer">
</body>
```

::: moniker-end
