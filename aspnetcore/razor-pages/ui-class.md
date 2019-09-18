---
title: Interface utilisateur Razor réutilisable dans les bibliothèques de classes avec ASP.NET Core
author: Rick-Anderson
description: Explique comment créer l’interface utilisateur de Razor réutilisables à l’aide de vues partielles dans une bibliothèque de classes dans ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 08/22/2019
ms.custom: mvc, seodec18
uid: razor-pages/ui-class
ms.openlocfilehash: 92c04c1ac4c70c6245accf272753bc914aaab860
ms.sourcegitcommit: 215954a638d24124f791024c66fd4fb9109fd380
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/18/2019
ms.locfileid: "71081871"
---
# <a name="create-reusable-ui-using-the-razor-class-library-project-in-aspnet-core"></a><span data-ttu-id="f81d2-103">Créer une interface utilisateur réutilisable à l’aide du projet de bibliothèque de classes Razor dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="f81d2-103">Create reusable UI using the Razor class library project in ASP.NET Core</span></span>

<span data-ttu-id="f81d2-104">Par [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="f81d2-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="f81d2-105">Les vues Razor, les pages, les contrôleurs, les modèles de page, les [composants Razor](xref:blazor/class-libraries), les [composants de vue](xref:mvc/views/view-components)et les modèles de données peuvent être intégrés dans une bibliothèque de classes Razor (RCL).</span><span class="sxs-lookup"><span data-stu-id="f81d2-105">Razor views, pages, controllers, page models, [Razor components](xref:blazor/class-libraries), [View components](xref:mvc/views/view-components), and data models can be built into a Razor class library (RCL).</span></span> <span data-ttu-id="f81d2-106">La RCL peut être empaquetée et réutilisée.</span><span class="sxs-lookup"><span data-stu-id="f81d2-106">The RCL can be packaged and reused.</span></span> <span data-ttu-id="f81d2-107">Les applications peuvent inclure la RCL et remplacer les vues et les pages qu’elle contient.</span><span class="sxs-lookup"><span data-stu-id="f81d2-107">Applications can include the RCL and override the views and pages it contains.</span></span> <span data-ttu-id="f81d2-108">Quand une vue, une vue partielle ou une page Razor est présente dans l’application web et la RCL, le balisage Razor (fichier *.cshtml*) dans l’application web est prioritaire.</span><span class="sxs-lookup"><span data-stu-id="f81d2-108">When a view, partial view, or Razor Page is found in both the web app and the RCL, the Razor markup (*.cshtml* file) in the web app takes precedence.</span></span>

<span data-ttu-id="f81d2-109">Cette fonctionnalité nécessite [!INCLUDE[](~/includes/2.1-SDK.md)]</span><span class="sxs-lookup"><span data-stu-id="f81d2-109">This feature requires [!INCLUDE[](~/includes/2.1-SDK.md)]</span></span>

<span data-ttu-id="f81d2-110">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/ui-class/samples) ([procédure de téléchargement](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="f81d2-110">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/ui-class/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="create-a-class-library-containing-razor-ui"></a><span data-ttu-id="f81d2-111">Créer une bibliothèque de classes contenant l’interface utilisateur Razor</span><span class="sxs-lookup"><span data-stu-id="f81d2-111">Create a class library containing Razor UI</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="f81d2-112">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f81d2-112">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="f81d2-113">Dans Visual Studio, dans le menu **Fichier**, sélectionnez **Nouveau** > **Projet**.</span><span class="sxs-lookup"><span data-stu-id="f81d2-113">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="f81d2-114">Sélectionnez **Nouvelle application web ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="f81d2-114">Select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="f81d2-115">Nommez la bibliothèque (par exemple, « RazorClassLib ») > **OK**.</span><span class="sxs-lookup"><span data-stu-id="f81d2-115">Name the library (for example, "RazorClassLib") > **OK**.</span></span> <span data-ttu-id="f81d2-116">Pour éviter une collision de nom de fichier avec la bibliothèque de vues générée, vérifiez que le nom de la bibliothèque ne se termine pas par `.Views`.</span><span class="sxs-lookup"><span data-stu-id="f81d2-116">To avoid a file name collision with the generated view library, ensure the library name doesn't end in `.Views`.</span></span>
* <span data-ttu-id="f81d2-117">Vérifiez que **ASP.NET Core 2.1** ou ultérieur est sélectionné.</span><span class="sxs-lookup"><span data-stu-id="f81d2-117">Verify **ASP.NET Core 2.1** or later is selected.</span></span>
* <span data-ttu-id="f81d2-118">Sélectionnez **Razor Class Library** > **OK**.</span><span class="sxs-lookup"><span data-stu-id="f81d2-118">Select **Razor Class Library** > **OK**.</span></span>

<span data-ttu-id="f81d2-119">Un RCL a le fichier projet suivant :</span><span class="sxs-lookup"><span data-stu-id="f81d2-119">An RCL has the following project file:</span></span>

[!code-xml[Main](ui-class/samples/cli/RazorUIClassLib/RazorUIClassLib.csproj)]

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="f81d2-120">CLI .NET Core</span><span class="sxs-lookup"><span data-stu-id="f81d2-120">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="f81d2-121">À partir de la ligne de commande, exécutez `dotnet new razorclasslib`.</span><span class="sxs-lookup"><span data-stu-id="f81d2-121">From the command line, run `dotnet new razorclasslib`.</span></span> <span data-ttu-id="f81d2-122">Exemple :</span><span class="sxs-lookup"><span data-stu-id="f81d2-122">For example:</span></span>

```dotnetcli
dotnet new razorclasslib -o RazorUIClassLib
```

<span data-ttu-id="f81d2-123">Pour plus d’informations, consultez [dotnet new](/dotnet/core/tools/dotnet-new).</span><span class="sxs-lookup"><span data-stu-id="f81d2-123">For more information, see [dotnet new](/dotnet/core/tools/dotnet-new).</span></span> <span data-ttu-id="f81d2-124">Pour éviter une collision de nom de fichier avec la bibliothèque de vues générée, vérifiez que le nom de la bibliothèque ne se termine pas par `.Views`.</span><span class="sxs-lookup"><span data-stu-id="f81d2-124">To avoid a file name collision with the generated view library, ensure the library name doesn't end in `.Views`.</span></span>

---

<span data-ttu-id="f81d2-125">Ajoutez des fichiers Razor à la RCL.</span><span class="sxs-lookup"><span data-stu-id="f81d2-125">Add Razor files to the RCL.</span></span>

<span data-ttu-id="f81d2-126">Les modèles ASP.NET Core supposent que le contenu RCL se trouve dans le *zones* dossier.</span><span class="sxs-lookup"><span data-stu-id="f81d2-126">The ASP.NET Core templates assume the RCL content is in the *Areas* folder.</span></span> <span data-ttu-id="f81d2-127">Consultez [RCL pages layout](#afs) pour créer un RCL qui expose du contenu `~/Pages` dans plutôt `~/Areas/Pages`que.</span><span class="sxs-lookup"><span data-stu-id="f81d2-127">See [RCL Pages layout](#afs) to create an RCL that exposes content in `~/Pages` rather than `~/Areas/Pages`.</span></span>

## <a name="referencing-rcl-content"></a><span data-ttu-id="f81d2-128">Référencement du contenu RCL</span><span class="sxs-lookup"><span data-stu-id="f81d2-128">Referencing RCL content</span></span>

<span data-ttu-id="f81d2-129">La RCL peut être référencée par :</span><span class="sxs-lookup"><span data-stu-id="f81d2-129">The RCL can be referenced by:</span></span>

* <span data-ttu-id="f81d2-130">Un package NuGet.</span><span class="sxs-lookup"><span data-stu-id="f81d2-130">NuGet package.</span></span> <span data-ttu-id="f81d2-131">Consultez [Création de packages NuGet](/nuget/create-packages/creating-a-package), [dotnet add package](/dotnet/core/tools/dotnet-add-package) et [Créer et publier un package NuGet](/nuget/quickstart/create-and-publish-a-package-using-visual-studio).</span><span class="sxs-lookup"><span data-stu-id="f81d2-131">See [Creating NuGet packages](/nuget/create-packages/creating-a-package) and [dotnet add package](/dotnet/core/tools/dotnet-add-package) and [Create and publish a NuGet package](/nuget/quickstart/create-and-publish-a-package-using-visual-studio).</span></span>
* <span data-ttu-id="f81d2-132">Un fichier *{NomProjet}.csproj*.</span><span class="sxs-lookup"><span data-stu-id="f81d2-132">*{ProjectName}.csproj*.</span></span> <span data-ttu-id="f81d2-133">Consultez [dotnet-add reference](/dotnet/core/tools/dotnet-add-reference).</span><span class="sxs-lookup"><span data-stu-id="f81d2-133">See [dotnet-add reference](/dotnet/core/tools/dotnet-add-reference).</span></span>

## <a name="walkthrough-create-an-rcl-project-and-use-from-a-razor-pages-project"></a><span data-ttu-id="f81d2-134">Procédure pas à pas : Créer un projet RCL et l’utiliser à partir d’un projet Razor Pages</span><span class="sxs-lookup"><span data-stu-id="f81d2-134">Walkthrough: Create an RCL project and use from a Razor Pages project</span></span>

<span data-ttu-id="f81d2-135">Vous pouvez télécharger et tester le [projet complet](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/ui-class/samples) au lieu de le créer de toutes pièces.</span><span class="sxs-lookup"><span data-stu-id="f81d2-135">You can download the [complete project](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/ui-class/samples) and test it rather than creating it.</span></span> <span data-ttu-id="f81d2-136">L’exemple proposé sous forme de téléchargement contient du code et des liens supplémentaires qui facilitent le test du projet.</span><span class="sxs-lookup"><span data-stu-id="f81d2-136">The sample download contains additional code and links that make the project easy to test.</span></span> <span data-ttu-id="f81d2-137">Si vous souhaitez commenter le mode d’obtention des exemples (téléchargement ou création au moyen d’instructions détaillées), entrez vos commentaires dans [ce problème GitHub](https://github.com/aspnet/AspNetCore.Docs/issues/6098).</span><span class="sxs-lookup"><span data-stu-id="f81d2-137">You can leave feedback in [this GitHub issue](https://github.com/aspnet/AspNetCore.Docs/issues/6098) with your comments on download samples versus step-by-step instructions.</span></span>

### <a name="test-the-download-app"></a><span data-ttu-id="f81d2-138">Tester l’application téléchargée</span><span class="sxs-lookup"><span data-stu-id="f81d2-138">Test the download app</span></span>

<span data-ttu-id="f81d2-139">Si vous n’avez pas téléchargé l’application complète et que vous souhaitez créer le projet pas à pas, passez à la [section suivante](#create-an-rcl).</span><span class="sxs-lookup"><span data-stu-id="f81d2-139">If you haven't downloaded the completed app and would rather create the walkthrough project, skip to the [next section](#create-an-rcl).</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="f81d2-140">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f81d2-140">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="f81d2-141">Ouvrez le fichier *.sln* dans Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="f81d2-141">Open the *.sln* file in Visual Studio.</span></span> <span data-ttu-id="f81d2-142">Exécuter l’application.</span><span class="sxs-lookup"><span data-stu-id="f81d2-142">Run the app.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="f81d2-143">CLI .NET Core</span><span class="sxs-lookup"><span data-stu-id="f81d2-143">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="f81d2-144">À partir d’une invite de commandes dans le répertoire *cli*, générez la RCL et l’application web.</span><span class="sxs-lookup"><span data-stu-id="f81d2-144">From a command prompt in the *cli* directory, build the RCL and web app.</span></span>

```dotnetcli
dotnet build
```

<span data-ttu-id="f81d2-145">Passez au répertoire *WebApp1* et exécutez l’application :</span><span class="sxs-lookup"><span data-stu-id="f81d2-145">Move to the *WebApp1* directory and run the app:</span></span>

```dotnetcli
dotnet run
```

---

<span data-ttu-id="f81d2-146">Suivez les instructions contenues dans [Test WebApp1](#test)</span><span class="sxs-lookup"><span data-stu-id="f81d2-146">Follow the instructions in [Test WebApp1](#test)</span></span>

## <a name="create-an-rcl"></a><span data-ttu-id="f81d2-147">Créer un RCL</span><span class="sxs-lookup"><span data-stu-id="f81d2-147">Create an RCL</span></span>

<span data-ttu-id="f81d2-148">Dans cette section, un RCL est créé.</span><span class="sxs-lookup"><span data-stu-id="f81d2-148">In this section, an RCL is created.</span></span> <span data-ttu-id="f81d2-149">Des fichiers Razor sont ajoutés à la RCL.</span><span class="sxs-lookup"><span data-stu-id="f81d2-149">Razor files are added to the RCL.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="f81d2-150">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f81d2-150">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="f81d2-151">Créez le projet RCL :</span><span class="sxs-lookup"><span data-stu-id="f81d2-151">Create the RCL project:</span></span>

* <span data-ttu-id="f81d2-152">Dans Visual Studio, dans le menu **Fichier**, sélectionnez **Nouveau** > **Projet**.</span><span class="sxs-lookup"><span data-stu-id="f81d2-152">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="f81d2-153">Sélectionnez **Nouvelle application web ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="f81d2-153">Select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="f81d2-154">Nommez l’application **RazorUIClassLib** > **OK**.</span><span class="sxs-lookup"><span data-stu-id="f81d2-154">Name the app **RazorUIClassLib** > **OK**.</span></span>
* <span data-ttu-id="f81d2-155">Vérifiez que **ASP.NET Core 2.1** ou ultérieur est sélectionné.</span><span class="sxs-lookup"><span data-stu-id="f81d2-155">Verify **ASP.NET Core 2.1** or later is selected.</span></span>
* <span data-ttu-id="f81d2-156">Sélectionnez **Razor Class Library** > **OK**.</span><span class="sxs-lookup"><span data-stu-id="f81d2-156">Select **Razor Class Library** > **OK**.</span></span>
* <span data-ttu-id="f81d2-157">Ajoutez un fichier de vue partielle Razor nommé *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="f81d2-157">Add a Razor partial view file named *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml*.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="f81d2-158">CLI .NET Core</span><span class="sxs-lookup"><span data-stu-id="f81d2-158">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="f81d2-159">À partir de la ligne de commande, exécutez ce qui suit :</span><span class="sxs-lookup"><span data-stu-id="f81d2-159">From the command line, run the following:</span></span>

```dotnetcli
dotnet new razorclasslib -o RazorUIClassLib
dotnet new page -n _Message -np -o RazorUIClassLib/Areas/MyFeature/Pages/Shared
dotnet new viewstart -o RazorUIClassLib/Areas/MyFeature/Pages
```

<span data-ttu-id="f81d2-160">Les commandes précédentes :</span><span class="sxs-lookup"><span data-stu-id="f81d2-160">The preceding commands:</span></span>

* <span data-ttu-id="f81d2-161">Crée le `RazorUIClassLib` RCL.</span><span class="sxs-lookup"><span data-stu-id="f81d2-161">Creates the `RazorUIClassLib` RCL.</span></span>
* <span data-ttu-id="f81d2-162">Créent une page _Message Razor et l’ajoutent à la RCL.</span><span class="sxs-lookup"><span data-stu-id="f81d2-162">Creates a Razor _Message page, and adds it to the RCL.</span></span> <span data-ttu-id="f81d2-163">Le paramètre `-np` crée la page sans `PageModel`.</span><span class="sxs-lookup"><span data-stu-id="f81d2-163">The `-np` parameter creates the page without a `PageModel`.</span></span>
* <span data-ttu-id="f81d2-164">Crée un [_ViewStart.cshtml](xref:mvc/views/layout#running-code-before-each-view) de fichiers et l’ajoute à la RCL.</span><span class="sxs-lookup"><span data-stu-id="f81d2-164">Creates a [_ViewStart.cshtml](xref:mvc/views/layout#running-code-before-each-view) file and adds it to the RCL.</span></span>

<span data-ttu-id="f81d2-165">Le *_ViewStart.cshtml* fichier est nécessaire pour utiliser la disposition du projet de Pages Razor (qui est ajoutée à la section suivante).</span><span class="sxs-lookup"><span data-stu-id="f81d2-165">The *_ViewStart.cshtml* file is required to use the layout of the Razor Pages project (which is added in the next section).</span></span>

---

### <a name="add-razor-files-and-folders-to-the-project"></a><span data-ttu-id="f81d2-166">Ajouter les fichiers Razor et des dossiers au projet</span><span class="sxs-lookup"><span data-stu-id="f81d2-166">Add Razor files and folders to the project</span></span>

* <span data-ttu-id="f81d2-167">Remplacez le balisage dans *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* par le code suivant :</span><span class="sxs-lookup"><span data-stu-id="f81d2-167">Replace the markup in *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* with the following code:</span></span>

[!code-cshtml[Main](ui-class/samples/cli/RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml)]

* <span data-ttu-id="f81d2-168">Remplacez le balisage dans *RazorUIClassLib/Areas/MyFeature/Pages/Page1.cshtml* par le code suivant :</span><span class="sxs-lookup"><span data-stu-id="f81d2-168">Replace the markup in *RazorUIClassLib/Areas/MyFeature/Pages/Page1.cshtml* with the following code:</span></span>

[!code-cshtml[Main](ui-class/samples/cli/RazorUIClassLib/Areas/MyFeature/Pages/Page1.cshtml)]

<span data-ttu-id="f81d2-169">`@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers` est nécessaire pour utiliser la vue partielle (`<partial name="_Message" />`).</span><span class="sxs-lookup"><span data-stu-id="f81d2-169">`@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers` is required to use the partial view (`<partial name="_Message" />`).</span></span> <span data-ttu-id="f81d2-170">Au lieu d’inclure la directive `@addTagHelper`, vous pouvez ajouter un fichier *_ViewImports.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="f81d2-170">Rather than including the `@addTagHelper` directive, you can add a *_ViewImports.cshtml* file.</span></span> <span data-ttu-id="f81d2-171">Exemple :</span><span class="sxs-lookup"><span data-stu-id="f81d2-171">For example:</span></span>

```dotnetcli
dotnet new viewimports -o RazorUIClassLib/Areas/MyFeature/Pages
```

<span data-ttu-id="f81d2-172">Pour plus d’informations sur *_ViewImports.cshtml*, consultez [importation de Directives partagées](xref:mvc/views/layout#importing-shared-directives)</span><span class="sxs-lookup"><span data-stu-id="f81d2-172">For more information on *_ViewImports.cshtml*, see [Importing Shared Directives](xref:mvc/views/layout#importing-shared-directives)</span></span>

* <span data-ttu-id="f81d2-173">Générez la bibliothèque de classes pour vérifier l’absence d’erreurs de compilateur :</span><span class="sxs-lookup"><span data-stu-id="f81d2-173">Build the class library to verify there are no compiler errors:</span></span>

```dotnetcli
dotnet build RazorUIClassLib
```

<span data-ttu-id="f81d2-174">La sortie de build contient *RazorUIClassLib.dll* et *RazorUIClassLib.Views.dll*.</span><span class="sxs-lookup"><span data-stu-id="f81d2-174">The build output contains *RazorUIClassLib.dll* and *RazorUIClassLib.Views.dll*.</span></span> <span data-ttu-id="f81d2-175">*RazorUIClassLib.Views.dll* contient le contenu Razor compilé.</span><span class="sxs-lookup"><span data-stu-id="f81d2-175">*RazorUIClassLib.Views.dll* contains the compiled Razor content.</span></span>

### <a name="use-the-razor-ui-library-from-a-razor-pages-project"></a><span data-ttu-id="f81d2-176">Utiliser la bibliothèque de l’interface utilisateur Razor à partir d’un projet Razor Pages</span><span class="sxs-lookup"><span data-stu-id="f81d2-176">Use the Razor UI library from a Razor Pages project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="f81d2-177">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f81d2-177">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="f81d2-178">Créez l’application web Razor Pages :</span><span class="sxs-lookup"><span data-stu-id="f81d2-178">Create the Razor Pages web app:</span></span>

* <span data-ttu-id="f81d2-179">Dans **l’Explorateur de solutions**, cliquez avec le bouton droit sur la solution > **Ajouter** >  **Nouveau projet**.</span><span class="sxs-lookup"><span data-stu-id="f81d2-179">From **Solution Explorer**, right-click the solution > **Add** >  **New Project**.</span></span>
* <span data-ttu-id="f81d2-180">Sélectionnez **Nouvelle application web ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="f81d2-180">Select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="f81d2-181">Nommez l’application **WebApp1**.</span><span class="sxs-lookup"><span data-stu-id="f81d2-181">Name the app **WebApp1**.</span></span>
* <span data-ttu-id="f81d2-182">Vérifiez que **ASP.NET Core 2.1** ou ultérieur est sélectionné.</span><span class="sxs-lookup"><span data-stu-id="f81d2-182">Verify **ASP.NET Core 2.1** or later is selected.</span></span>
* <span data-ttu-id="f81d2-183">Sélectionnez **Application web** > **OK**.</span><span class="sxs-lookup"><span data-stu-id="f81d2-183">Select **Web Application** > **OK**.</span></span>

* <span data-ttu-id="f81d2-184">Dans **l’Explorateur de solutions**, cliquez avec le bouton droit sur **WebApp1**, puis sélectionnez **Définir comme projet de démarrage**.</span><span class="sxs-lookup"><span data-stu-id="f81d2-184">From **Solution Explorer**, right-click on **WebApp1** and select **Set as StartUp Project**.</span></span>
* <span data-ttu-id="f81d2-185">Dans **l’Explorateur de solutions**, cliquez avec le bouton droit sur **WebApp1**, puis sélectionnez **Dépendances de build** > **Dépendances du projet**.</span><span class="sxs-lookup"><span data-stu-id="f81d2-185">From **Solution Explorer**, right-click on **WebApp1** and select **Build Dependencies** > **Project Dependencies**.</span></span>
* <span data-ttu-id="f81d2-186">Cochez **RazorUIClassLib** comme dépendance de **WebApp1**.</span><span class="sxs-lookup"><span data-stu-id="f81d2-186">Check **RazorUIClassLib** as a dependency of **WebApp1**.</span></span>
* <span data-ttu-id="f81d2-187">Dans **l’Explorateur de solutions**, cliquez avec le bouton droit sur **WebApp1**, puis sélectionnez **Ajouter** > **Référence**.</span><span class="sxs-lookup"><span data-stu-id="f81d2-187">From **Solution Explorer**, right-click on **WebApp1** and select **Add** > **Reference**.</span></span>
* <span data-ttu-id="f81d2-188">Dans la boîte de dialogue **Gestionnaire de références**, cochez **RazorUIClassLib** > **OK**.</span><span class="sxs-lookup"><span data-stu-id="f81d2-188">In the **Reference Manager** dialog, check **RazorUIClassLib** > **OK**.</span></span>

<span data-ttu-id="f81d2-189">Exécuter l’application.</span><span class="sxs-lookup"><span data-stu-id="f81d2-189">Run the app.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="f81d2-190">CLI .NET Core</span><span class="sxs-lookup"><span data-stu-id="f81d2-190">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="f81d2-191">Créez un Razor Pages application Web et un fichier solution contenant l’application Razor Pages et le RCL :</span><span class="sxs-lookup"><span data-stu-id="f81d2-191">Create a Razor Pages web app and a solution file containing the Razor Pages app and the RCL:</span></span>

```dotnetcli
dotnet new webapp -o WebApp1
dotnet new sln
dotnet sln add WebApp1
dotnet sln add RazorUIClassLib
dotnet add WebApp1 reference RazorUIClassLib
```

<span data-ttu-id="f81d2-192">Générez et exécutez l’application web :</span><span class="sxs-lookup"><span data-stu-id="f81d2-192">Build and run the web app:</span></span>

```dotnetcli
cd WebApp1
dotnet run
```

---

<a name="test"></a>

### <a name="test-webapp1"></a><span data-ttu-id="f81d2-193">Tester WebApp1</span><span class="sxs-lookup"><span data-stu-id="f81d2-193">Test WebApp1</span></span>

<span data-ttu-id="f81d2-194">Vérifiez que la bibliothèque de classes de l’interface utilisateur Razor est en cours d’utilisation :</span><span class="sxs-lookup"><span data-stu-id="f81d2-194">Verify the Razor UI class library is in use:</span></span>

* <span data-ttu-id="f81d2-195">Accédez à `/MyFeature/Page1`.</span><span class="sxs-lookup"><span data-stu-id="f81d2-195">Browse to `/MyFeature/Page1`.</span></span>

## <a name="override-views-partial-views-and-pages"></a><span data-ttu-id="f81d2-196">Substituer des vues, des vues partielles et des pages</span><span class="sxs-lookup"><span data-stu-id="f81d2-196">Override views, partial views, and pages</span></span>

<span data-ttu-id="f81d2-197">Quand une vue, une vue partielle ou une page Razor est présente dans l’application web et la RCL, le balisage Razor (fichier *.cshtml*) dans l’application web est prioritaire.</span><span class="sxs-lookup"><span data-stu-id="f81d2-197">When a view, partial view, or Razor Page is found in both the web app and the RCL, the Razor markup (*.cshtml* file) in the web app takes precedence.</span></span> <span data-ttu-id="f81d2-198">Par exemple, ajoutez *application Web 1/Areas/MyFeature/pages/Page1. cshtml* à application Web 1, et Page1 dans le application Web 1 aura priorité sur Page1 dans le RCL.</span><span class="sxs-lookup"><span data-stu-id="f81d2-198">For example, add *WebApp1/Areas/MyFeature/Pages/Page1.cshtml* to WebApp1, and Page1 in the WebApp1 will take precedence over Page1 in the RCL.</span></span>

<span data-ttu-id="f81d2-199">Dans l’exemple proposé sous forme de téléchargement, renommez *WebApp1/Areas/MyFeature2* en *WebApp1/Areas/MyFeature* pour tester la priorité.</span><span class="sxs-lookup"><span data-stu-id="f81d2-199">In the sample download, rename *WebApp1/Areas/MyFeature2* to *WebApp1/Areas/MyFeature* to test precedence.</span></span>

<span data-ttu-id="f81d2-200">Copiez la vue partielle *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* dans *WebApp1/Areas/MyFeature/Pages/Shared/_Message.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="f81d2-200">Copy the *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* partial view to *WebApp1/Areas/MyFeature/Pages/Shared/_Message.cshtml*.</span></span> <span data-ttu-id="f81d2-201">Mettez à jour le balisage pour indiquer le nouvel emplacement.</span><span class="sxs-lookup"><span data-stu-id="f81d2-201">Update the markup to indicate the new location.</span></span> <span data-ttu-id="f81d2-202">Générez et exécutez l’application pour vérifier que la version de la vue partielle de l’application est utilisée.</span><span class="sxs-lookup"><span data-stu-id="f81d2-202">Build and run the app to verify the app's version of the partial is being used.</span></span>

<a name="afs"></a>

### <a name="rcl-pages-layout"></a><span data-ttu-id="f81d2-203">Disposition des Pages de RCL</span><span class="sxs-lookup"><span data-stu-id="f81d2-203">RCL Pages layout</span></span>

<span data-ttu-id="f81d2-204">Pour référence RCL de contenu comme s’il faisait partie de l’application web *Pages* dossier, créez le projet RCL avec la structure de fichier suivante :</span><span class="sxs-lookup"><span data-stu-id="f81d2-204">To reference RCL content as though it is part of the web app's *Pages* folder, create the RCL project with the following file structure:</span></span>

* <span data-ttu-id="f81d2-205">*RazorUIClassLib/Pages*</span><span class="sxs-lookup"><span data-stu-id="f81d2-205">*RazorUIClassLib/Pages*</span></span>
* <span data-ttu-id="f81d2-206">*RazorUIClassLib/Pages/Shared*</span><span class="sxs-lookup"><span data-stu-id="f81d2-206">*RazorUIClassLib/Pages/Shared*</span></span>

<span data-ttu-id="f81d2-207">Supposons que *RazorUIClassLib/Pages/Shared* contient deux fichiers partielles : *_Header.cshtml* et *_Footer.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="f81d2-207">Suppose *RazorUIClassLib/Pages/Shared* contains two partial files: *_Header.cshtml* and *_Footer.cshtml*.</span></span> <span data-ttu-id="f81d2-208">Le `<partial>` balises peut être ajoutés à *_Layout.cshtml* fichier :</span><span class="sxs-lookup"><span data-stu-id="f81d2-208">The `<partial>` tags could be added to *_Layout.cshtml* file:</span></span>

```cshtml
<body>
  <partial name="_Header">
  @RenderBody()
  <partial name="_Footer">
</body>
```

::: moniker range=">= aspnetcore-3.0"

## <a name="create-an-rcl-with-static-assets"></a><span data-ttu-id="f81d2-209">Créer un RCL avec des ressources statiques</span><span class="sxs-lookup"><span data-stu-id="f81d2-209">Create an RCL with static assets</span></span>

<span data-ttu-id="f81d2-210">Un RCL peut nécessiter des ressources statiques auxiliaires qui peuvent être référencées par l’application consommatrice de RCL.</span><span class="sxs-lookup"><span data-stu-id="f81d2-210">An RCL may require companion static assets that can be referenced by the consuming app of the RCL.</span></span> <span data-ttu-id="f81d2-211">ASP.NET Core permet de créer des RCLs qui incluent des ressources statiques disponibles pour une application consommatrice.</span><span class="sxs-lookup"><span data-stu-id="f81d2-211">ASP.NET Core allows creating RCLs that include static assets that are available to a consuming app.</span></span>

<span data-ttu-id="f81d2-212">Pour inclure des ressources d’accompagnement dans le cadre d’un RCL, créez un dossier *wwwroot* dans la bibliothèque de classes et incluez tous les fichiers nécessaires dans ce dossier.</span><span class="sxs-lookup"><span data-stu-id="f81d2-212">To include companion assets as part of an RCL, create a *wwwroot* folder in the class library and include any required files in that folder.</span></span>

<span data-ttu-id="f81d2-213">Lors de la compression d’un RCL, toutes les ressources associées dans le dossier *wwwroot* sont automatiquement incluses dans le package.</span><span class="sxs-lookup"><span data-stu-id="f81d2-213">When packing an RCL, all companion assets in the *wwwroot* folder are automatically included in the package.</span></span>

### <a name="exclude-static-assets"></a><span data-ttu-id="f81d2-214">Exclure des ressources statiques</span><span class="sxs-lookup"><span data-stu-id="f81d2-214">Exclude static assets</span></span>

<span data-ttu-id="f81d2-215">Pour exclure des ressources statiques, ajoutez le chemin d’exclusion `$(DefaultItemExcludes)` souhaité au groupe de propriétés dans le fichier projet.</span><span class="sxs-lookup"><span data-stu-id="f81d2-215">To exclude static assets, add the desired exclusion path to the `$(DefaultItemExcludes)` property group in the project file.</span></span> <span data-ttu-id="f81d2-216">Séparez les entrées par un`;`point-virgule ().</span><span class="sxs-lookup"><span data-stu-id="f81d2-216">Separate entries with a semicolon (`;`).</span></span>

<span data-ttu-id="f81d2-217">Dans l’exemple suivant, la feuille de style *lib. CSS* du dossier *wwwroot* n’est pas considérée comme une ressource statique et n’est pas incluse dans le RCL publié :</span><span class="sxs-lookup"><span data-stu-id="f81d2-217">In the following example, the *lib.css* stylesheet in the *wwwroot* folder isn't considered a static asset and isn't included in the published RCL:</span></span>

```xml
<PropertyGroup>
  <DefaultItemExcludes>$(DefaultItemExcludes);wwwroot\lib.css</DefaultItemExcludes>
</PropertyGroup>
```

### <a name="typescript-integration"></a><span data-ttu-id="f81d2-218">Intégration de la machine à écrire</span><span class="sxs-lookup"><span data-stu-id="f81d2-218">Typescript integration</span></span>

<span data-ttu-id="f81d2-219">Pour inclure des fichiers de machine à écrire dans un RCL :</span><span class="sxs-lookup"><span data-stu-id="f81d2-219">To include TypeScript files in an RCL:</span></span>

1. <span data-ttu-id="f81d2-220">Placez les fichiers de machine à écrire ( *. TS*) en dehors du dossier *wwwroot* .</span><span class="sxs-lookup"><span data-stu-id="f81d2-220">Place the TypeScript files (*.ts*) outside of the *wwwroot* folder.</span></span> <span data-ttu-id="f81d2-221">Par exemple, placez les fichiers dans un dossier *client* .</span><span class="sxs-lookup"><span data-stu-id="f81d2-221">For example, place the files in a *Client* folder.</span></span>

1. <span data-ttu-id="f81d2-222">Configurez la sortie de génération de machine à écrire pour le dossier *wwwroot* .</span><span class="sxs-lookup"><span data-stu-id="f81d2-222">Configure the TypeScript build output for the *wwwroot* folder.</span></span> <span data-ttu-id="f81d2-223">Définissez la `TypescriptOutDir` propriété à l’intérieur `PropertyGroup` d’un dans le fichier projet :</span><span class="sxs-lookup"><span data-stu-id="f81d2-223">Set the `TypescriptOutDir` property inside of a `PropertyGroup` in the project file:</span></span>

   ```xml
   <TypescriptOutDir>wwwroot</TypescriptOutDir>
   ```

1. <span data-ttu-id="f81d2-224">Incluez la cible de la machine à écrire `ResolveCurrentProjectStaticWebAssets` comme dépendance de la cible en ajoutant la cible `PropertyGroup` suivante à l’intérieur d’un dans le fichier projet :</span><span class="sxs-lookup"><span data-stu-id="f81d2-224">Include the TypeScript target as a dependency of the `ResolveCurrentProjectStaticWebAssets` target by adding the following target inside of a `PropertyGroup` in the project file:</span></span>

   ```xml
   <ResolveCurrentProjectStaticWebAssetsInputsDependsOn>
     TypeScriptCompile;
     $(ResolveCurrentProjectStaticWebAssetsInputs)
   </ResolveCurrentProjectStaticWebAssetsInputsDependsOn>
   ```

### <a name="consume-content-from-a-referenced-rcl"></a><span data-ttu-id="f81d2-225">Consommer du contenu à partir d’un RCL référencé</span><span class="sxs-lookup"><span data-stu-id="f81d2-225">Consume content from a referenced RCL</span></span>

<span data-ttu-id="f81d2-226">Les fichiers inclus dans le dossier *wwwroot* du RCL sont exposés à l’application consommatrice sous le préfixe `_content/{LIBRARY NAME}/`.</span><span class="sxs-lookup"><span data-stu-id="f81d2-226">The files included in the *wwwroot* folder of the RCL are exposed to the consuming app under the prefix `_content/{LIBRARY NAME}/`.</span></span> <span data-ttu-id="f81d2-227">Par exemple, une bibliothèque nommée *Razor. class. lib* génère un chemin d’accès au contenu statique `_content/Razor.Class.Lib/`à l’emplacement.</span><span class="sxs-lookup"><span data-stu-id="f81d2-227">For example, a library named *Razor.Class.Lib* results in a path to static content at `_content/Razor.Class.Lib/`.</span></span>

<span data-ttu-id="f81d2-228">L’application consommatrice référence les ressources statiques fournies par la bibliothèque `<script>`avec `<style>` `<img>`,, et d’autres balises html.</span><span class="sxs-lookup"><span data-stu-id="f81d2-228">The consuming app references static assets provided by the library with `<script>`, `<style>`, `<img>`, and other HTML tags.</span></span> <span data-ttu-id="f81d2-229">L’application consommatrice doit avoir la [prise en charge des fichiers statiques](xref:fundamentals/static-files) activée dans `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="f81d2-229">The consuming app must have [static file support](xref:fundamentals/static-files) enabled in `Startup.Configure`:</span></span>

```csharp
public void Configure(IApplicationBuilder app, IWebHostEnvironment env)
{
    ...

    app.UseStaticFiles();

    ...
}
```

<span data-ttu-id="f81d2-230">Lors de l’exécution de l’application consommatrice à partir`dotnet run`de la sortie de génération (), les ressources Web statiques sont activées par défaut dans l’environnement de développement.</span><span class="sxs-lookup"><span data-stu-id="f81d2-230">When running the consuming app from build output (`dotnet run`), static web assets are enabled by default in the Development environment.</span></span> <span data-ttu-id="f81d2-231">Pour prendre en charge les ressources dans d’autres environnements lors de l' `UseStaticWebAssets` exécution à partir de la sortie de génération, appelez sur le générateur d’hôte dans *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="f81d2-231">To support assets in other environments when running from build output, call `UseStaticWebAssets` on the host builder in *Program.cs*:</span></span>

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

<span data-ttu-id="f81d2-232">L' `UseStaticWebAssets` appel de n’est pas requis lors de l’exécution`dotnet publish`d’une application à partir de la sortie publiée ().</span><span class="sxs-lookup"><span data-stu-id="f81d2-232">Calling `UseStaticWebAssets` isn't required when running an app from published output (`dotnet publish`).</span></span>

### <a name="multi-project-development-flow"></a><span data-ttu-id="f81d2-233">Déroulement du développement de projets multiples</span><span class="sxs-lookup"><span data-stu-id="f81d2-233">Multi-project development flow</span></span>

<span data-ttu-id="f81d2-234">Lorsque l’application consommatrice s’exécute :</span><span class="sxs-lookup"><span data-stu-id="f81d2-234">When the consuming app runs:</span></span>

* <span data-ttu-id="f81d2-235">Les ressources du RCL restent dans leurs dossiers d’origine.</span><span class="sxs-lookup"><span data-stu-id="f81d2-235">The assets in the RCL stay in their original folders.</span></span> <span data-ttu-id="f81d2-236">Les ressources ne sont pas déplacées vers l’application consommatrice.</span><span class="sxs-lookup"><span data-stu-id="f81d2-236">The assets aren't moved to the consuming app.</span></span>
* <span data-ttu-id="f81d2-237">Toute modification dans le dossier *wwwroot* de RCL est reflétée dans l’application consommatrice une fois que le RCL est reconstruit et sans régénérer l’application consommateur.</span><span class="sxs-lookup"><span data-stu-id="f81d2-237">Any change within the RCL's *wwwroot* folder is reflected in the consuming app after the RCL is rebuilt and without rebuilding the consuming app.</span></span>

<span data-ttu-id="f81d2-238">Lorsque le RCL est généré, un manifeste qui décrit les emplacements des ressources Web statiques est généré.</span><span class="sxs-lookup"><span data-stu-id="f81d2-238">When the RCL is built, a manifest is produced that describes the static web asset locations.</span></span> <span data-ttu-id="f81d2-239">L’application consommatrice lit le manifeste au moment de l’exécution pour consommer les ressources des packages et des projets référencés.</span><span class="sxs-lookup"><span data-stu-id="f81d2-239">The consuming app reads the manifest at runtime to consume the assets from referenced projects and packages.</span></span> <span data-ttu-id="f81d2-240">Lorsqu’un nouvel élément multimédia est ajouté à un RCL, le RCL doit être régénéré pour mettre à jour son manifeste avant qu’une application consommatrice puisse accéder au nouvel élément multimédia.</span><span class="sxs-lookup"><span data-stu-id="f81d2-240">When a new asset is added to an RCL, the RCL must be rebuilt to update its manifest before a consuming app can access the new asset.</span></span>

### <a name="publish"></a><span data-ttu-id="f81d2-241">Publish</span><span class="sxs-lookup"><span data-stu-id="f81d2-241">Publish</span></span>

<span data-ttu-id="f81d2-242">Lorsque l’application est publiée, les ressources complémentaires de tous les packages et projets référencés sont copiées dans le dossier *wwwroot* de l’application `_content/{LIBRARY NAME}/`publiée sous.</span><span class="sxs-lookup"><span data-stu-id="f81d2-242">When the app is published, the companion assets from all referenced projects and packages are copied into the *wwwroot* folder of the published app under `_content/{LIBRARY NAME}/`.</span></span>

::: moniker-end
