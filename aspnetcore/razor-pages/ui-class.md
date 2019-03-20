---
title: Interface utilisateur Razor réutilisable dans les bibliothèques de classes avec ASP.NET Core
author: Rick-Anderson
description: Explique comment créer l’interface utilisateur de Razor réutilisables à l’aide de vues partielles dans une bibliothèque de classes dans ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 09/07/2018
ms.custom: seodec18
uid: razor-pages/ui-class
ms.openlocfilehash: dc7db9483f2d75fe79ed9a9806f944e4f2a05a9b
ms.sourcegitcommit: 57792e5f594db1574742588017c708350958bdf0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/20/2019
ms.locfileid: "58265344"
---
# <a name="create-reusable-ui-using-the-razor-class-library-project-in-aspnet-core"></a><span data-ttu-id="4bc2b-103">Créer l’interface utilisateur réutilisable à l’aide du projet de bibliothèque de classes Razor dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="4bc2b-103">Create reusable UI using the Razor Class Library project in ASP.NET Core</span></span>

<span data-ttu-id="4bc2b-104">Par [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="4bc2b-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="4bc2b-105">Les vues, pages, contrôleurs, modèles de page, [composants de vue](xref:mvc/views/view-components) et modèles de données Razor peuvent être intégrés à une bibliothèque de classes Razor (RCL, Razor Class Library).</span><span class="sxs-lookup"><span data-stu-id="4bc2b-105">Razor views, pages, controllers, page models, [View components](xref:mvc/views/view-components), and data models can be built into a Razor Class Library (RCL).</span></span> <span data-ttu-id="4bc2b-106">La RCL peut être empaquetée et réutilisée.</span><span class="sxs-lookup"><span data-stu-id="4bc2b-106">The RCL can be packaged and reused.</span></span> <span data-ttu-id="4bc2b-107">Les applications peuvent inclure la RCL et remplacer les vues et les pages qu’elle contient.</span><span class="sxs-lookup"><span data-stu-id="4bc2b-107">Applications can include the RCL and override the views and pages it contains.</span></span> <span data-ttu-id="4bc2b-108">Quand une vue, une vue partielle ou une page Razor est présente dans l’application web et la RCL, le balisage Razor (fichier *.cshtml*) dans l’application web est prioritaire.</span><span class="sxs-lookup"><span data-stu-id="4bc2b-108">When a view, partial view, or Razor Page is found in both the web app and the RCL, the Razor markup (*.cshtml* file) in the web app takes precedence.</span></span>

<span data-ttu-id="4bc2b-109">Cette fonctionnalité nécessite [!INCLUDE[](~/includes/2.1-SDK.md)]</span><span class="sxs-lookup"><span data-stu-id="4bc2b-109">This feature requires [!INCLUDE[](~/includes/2.1-SDK.md)]</span></span>

<span data-ttu-id="4bc2b-110">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/razor-pages/ui-class/samples) ([procédure de téléchargement](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="4bc2b-110">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/razor-pages/ui-class/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="create-a-class-library-containing-razor-ui"></a><span data-ttu-id="4bc2b-111">Créer une bibliothèque de classes contenant l’interface utilisateur Razor</span><span class="sxs-lookup"><span data-stu-id="4bc2b-111">Create a class library containing Razor UI</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="4bc2b-112">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="4bc2b-112">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="4bc2b-113">Dans Visual Studio, dans le menu **Fichier**, sélectionnez **Nouveau** > **Projet**.</span><span class="sxs-lookup"><span data-stu-id="4bc2b-113">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="4bc2b-114">Sélectionnez **Nouvelle application web ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="4bc2b-114">Select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="4bc2b-115">Nommez la bibliothèque (par exemple, « RazorClassLib ») > **OK**.</span><span class="sxs-lookup"><span data-stu-id="4bc2b-115">Name the library (for example, "RazorClassLib") > **OK**.</span></span> <span data-ttu-id="4bc2b-116">Pour éviter une collision de nom de fichier avec la bibliothèque de vues générée, vérifiez que le nom de la bibliothèque ne se termine pas par `.Views`.</span><span class="sxs-lookup"><span data-stu-id="4bc2b-116">To avoid a file name collision with the generated view library, ensure the library name doesn't end in `.Views`.</span></span>
* <span data-ttu-id="4bc2b-117">Vérifiez que **ASP.NET Core 2.1** ou ultérieur est sélectionné.</span><span class="sxs-lookup"><span data-stu-id="4bc2b-117">Verify **ASP.NET Core 2.1** or later is selected.</span></span>
* <span data-ttu-id="4bc2b-118">Sélectionnez **Razor Class Library** > **OK**.</span><span class="sxs-lookup"><span data-stu-id="4bc2b-118">Select **Razor Class Library** > **OK**.</span></span>

<span data-ttu-id="4bc2b-119">Une bibliothèque de classes Razor a le fichier projet suivant :</span><span class="sxs-lookup"><span data-stu-id="4bc2b-119">A Razor Class Library has the following project file:</span></span>

[!code-xml[Main](ui-class/samples/cli/RazorUIClassLib/RazorUIClassLib.csproj)]

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="4bc2b-120">CLI .NET Core</span><span class="sxs-lookup"><span data-stu-id="4bc2b-120">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="4bc2b-121">À partir de la ligne de commande, exécutez `dotnet new razorclasslib`.</span><span class="sxs-lookup"><span data-stu-id="4bc2b-121">From the command line, run `dotnet new razorclasslib`.</span></span> <span data-ttu-id="4bc2b-122">Exemple :</span><span class="sxs-lookup"><span data-stu-id="4bc2b-122">For example:</span></span>

```console
dotnet new razorclasslib -o RazorUIClassLib
```

<span data-ttu-id="4bc2b-123">Pour plus d’informations, consultez [dotnet new](/dotnet/core/tools/dotnet-new).</span><span class="sxs-lookup"><span data-stu-id="4bc2b-123">For more information, see [dotnet new](/dotnet/core/tools/dotnet-new).</span></span> <span data-ttu-id="4bc2b-124">Pour éviter une collision de nom de fichier avec la bibliothèque de vues générée, vérifiez que le nom de la bibliothèque ne se termine pas par `.Views`.</span><span class="sxs-lookup"><span data-stu-id="4bc2b-124">To avoid a file name collision with the generated view library, ensure the library name doesn't end in `.Views`.</span></span>

---

<span data-ttu-id="4bc2b-125">Ajoutez des fichiers Razor à la RCL.</span><span class="sxs-lookup"><span data-stu-id="4bc2b-125">Add Razor files to the RCL.</span></span>

<span data-ttu-id="4bc2b-126">Les modèles ASP.NET Core supposent que le contenu RCL se trouve dans le *zones* dossier.</span><span class="sxs-lookup"><span data-stu-id="4bc2b-126">The ASP.NET Core templates assume the RCL content is in the *Areas* folder.</span></span> <span data-ttu-id="4bc2b-127">Consultez [disposition des Pages de RCL](#afs) pour créer du contenu dans un RCL expose `~/Pages` plutôt que `~/Areas/Pages`.</span><span class="sxs-lookup"><span data-stu-id="4bc2b-127">See [RCL Pages layout](#afs) to create a RCL that exposes content in `~/Pages` rather than `~/Areas/Pages`.</span></span>

## <a name="referencing-razor-class-library-content"></a><span data-ttu-id="4bc2b-128">Référencement de contenu RCL</span><span class="sxs-lookup"><span data-stu-id="4bc2b-128">Referencing Razor Class Library content</span></span>

<span data-ttu-id="4bc2b-129">La RCL peut être référencée par :</span><span class="sxs-lookup"><span data-stu-id="4bc2b-129">The RCL can be referenced by:</span></span>

* <span data-ttu-id="4bc2b-130">Un package NuGet.</span><span class="sxs-lookup"><span data-stu-id="4bc2b-130">NuGet package.</span></span> <span data-ttu-id="4bc2b-131">Consultez [Création de packages NuGet](/nuget/create-packages/creating-a-package), [dotnet add package](/dotnet/core/tools/dotnet-add-package) et [Créer et publier un package NuGet](/nuget/quickstart/create-and-publish-a-package-using-visual-studio).</span><span class="sxs-lookup"><span data-stu-id="4bc2b-131">See [Creating NuGet packages](/nuget/create-packages/creating-a-package) and [dotnet add package](/dotnet/core/tools/dotnet-add-package) and [Create and publish a NuGet package](/nuget/quickstart/create-and-publish-a-package-using-visual-studio).</span></span>
* <span data-ttu-id="4bc2b-132">Un fichier *{NomProjet}.csproj*.</span><span class="sxs-lookup"><span data-stu-id="4bc2b-132">*{ProjectName}.csproj*.</span></span> <span data-ttu-id="4bc2b-133">Consultez [dotnet-add reference](/dotnet/core/tools/dotnet-add-reference).</span><span class="sxs-lookup"><span data-stu-id="4bc2b-133">See [dotnet-add reference](/dotnet/core/tools/dotnet-add-reference).</span></span>

## <a name="walkthrough-create-a-razor-class-library-project-and-use-from-a-razor-pages-project"></a><span data-ttu-id="4bc2b-134">Procédure pas à pas : Créez un projet de bibliothèque de classes Razor et utiliser à partir d’un projet de Pages Razor</span><span class="sxs-lookup"><span data-stu-id="4bc2b-134">Walkthrough: Create a Razor Class Library project and use from a Razor Pages project</span></span>

<span data-ttu-id="4bc2b-135">Vous pouvez télécharger et tester le [projet complet](https://github.com/aspnet/Docs/tree/master/aspnetcore/razor-pages/ui-class/samples) au lieu de le créer de toutes pièces.</span><span class="sxs-lookup"><span data-stu-id="4bc2b-135">You can download the [complete project](https://github.com/aspnet/Docs/tree/master/aspnetcore/razor-pages/ui-class/samples) and test it rather than creating it.</span></span> <span data-ttu-id="4bc2b-136">L’exemple proposé sous forme de téléchargement contient du code et des liens supplémentaires qui facilitent le test du projet.</span><span class="sxs-lookup"><span data-stu-id="4bc2b-136">The sample download contains additional code and links that make the project easy to test.</span></span> <span data-ttu-id="4bc2b-137">Si vous souhaitez commenter le mode d’obtention des exemples (téléchargement ou création au moyen d’instructions détaillées), entrez vos commentaires dans [ce problème GitHub](https://github.com/aspnet/Docs/issues/6098).</span><span class="sxs-lookup"><span data-stu-id="4bc2b-137">You can leave feedback in [this GitHub issue](https://github.com/aspnet/Docs/issues/6098) with your comments on download samples versus step-by-step instructions.</span></span>

### <a name="test-the-download-app"></a><span data-ttu-id="4bc2b-138">Tester l’application téléchargée</span><span class="sxs-lookup"><span data-stu-id="4bc2b-138">Test the download app</span></span>

<span data-ttu-id="4bc2b-139">Si vous n’avez pas téléchargé l’application complète et que vous souhaitez créer le projet pas à pas, passez à la [section suivante](#create-a-razor-class-library).</span><span class="sxs-lookup"><span data-stu-id="4bc2b-139">If you haven't downloaded the completed app and would rather create the walkthrough project, skip to the [next section](#create-a-razor-class-library).</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="4bc2b-140">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="4bc2b-140">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="4bc2b-141">Ouvrez le fichier *.sln* dans Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="4bc2b-141">Open the *.sln* file in Visual Studio.</span></span> <span data-ttu-id="4bc2b-142">Exécuter l’application.</span><span class="sxs-lookup"><span data-stu-id="4bc2b-142">Run the app.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="4bc2b-143">CLI .NET Core</span><span class="sxs-lookup"><span data-stu-id="4bc2b-143">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="4bc2b-144">À partir d’une invite de commandes dans le répertoire *cli*, générez la RCL et l’application web.</span><span class="sxs-lookup"><span data-stu-id="4bc2b-144">From a command prompt in the *cli* directory, build the RCL and web app.</span></span>

```console
dotnet build
```

<span data-ttu-id="4bc2b-145">Passez au répertoire *WebApp1* et exécutez l’application :</span><span class="sxs-lookup"><span data-stu-id="4bc2b-145">Move to the *WebApp1* directory and run the app:</span></span>

```console
dotnet run
```

---

<span data-ttu-id="4bc2b-146">Suivez les instructions contenues dans [Test WebApp1](#test)</span><span class="sxs-lookup"><span data-stu-id="4bc2b-146">Follow the instructions in [Test WebApp1](#test)</span></span>

## <a name="create-a-razor-class-library"></a><span data-ttu-id="4bc2b-147">Créer une RCL</span><span class="sxs-lookup"><span data-stu-id="4bc2b-147">Create a Razor Class Library</span></span>

<span data-ttu-id="4bc2b-148">Dans cette section, vous allez créer une RCL.</span><span class="sxs-lookup"><span data-stu-id="4bc2b-148">In this section, a Razor Class Library (RCL) is created.</span></span> <span data-ttu-id="4bc2b-149">Des fichiers Razor sont ajoutés à la RCL.</span><span class="sxs-lookup"><span data-stu-id="4bc2b-149">Razor files are added to the RCL.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="4bc2b-150">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="4bc2b-150">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="4bc2b-151">Créez le projet RCL :</span><span class="sxs-lookup"><span data-stu-id="4bc2b-151">Create the RCL project:</span></span>

* <span data-ttu-id="4bc2b-152">Dans Visual Studio, dans le menu **Fichier**, sélectionnez **Nouveau** > **Projet**.</span><span class="sxs-lookup"><span data-stu-id="4bc2b-152">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="4bc2b-153">Sélectionnez **Nouvelle application web ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="4bc2b-153">Select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="4bc2b-154">Nommez l’application **RazorUIClassLib** > **OK**.</span><span class="sxs-lookup"><span data-stu-id="4bc2b-154">Name the app **RazorUIClassLib** > **OK**.</span></span>
* <span data-ttu-id="4bc2b-155">Vérifiez que **ASP.NET Core 2.1** ou ultérieur est sélectionné.</span><span class="sxs-lookup"><span data-stu-id="4bc2b-155">Verify **ASP.NET Core 2.1** or later is selected.</span></span>
* <span data-ttu-id="4bc2b-156">Sélectionnez **Razor Class Library** > **OK**.</span><span class="sxs-lookup"><span data-stu-id="4bc2b-156">Select **Razor Class Library** > **OK**.</span></span>
* <span data-ttu-id="4bc2b-157">Ajoutez un fichier de vue partielle Razor nommé *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="4bc2b-157">Add a Razor partial view file named *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml*.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="4bc2b-158">CLI .NET Core</span><span class="sxs-lookup"><span data-stu-id="4bc2b-158">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="4bc2b-159">À partir de la ligne de commande, exécutez ce qui suit :</span><span class="sxs-lookup"><span data-stu-id="4bc2b-159">From the command line, run the following:</span></span>

```console
dotnet new razorclasslib -o RazorUIClassLib
dotnet new page -n _Message -np -o RazorUIClassLib/Areas/MyFeature/Pages/Shared
dotnet new viewstart -o RazorUIClassLib/Areas/MyFeature/Pages
```

<span data-ttu-id="4bc2b-160">Les commandes précédentes :</span><span class="sxs-lookup"><span data-stu-id="4bc2b-160">The preceding commands:</span></span>

* <span data-ttu-id="4bc2b-161">Créent la RCL `RazorUIClassLib`.</span><span class="sxs-lookup"><span data-stu-id="4bc2b-161">Creates the `RazorUIClassLib` Razor Class Library (RCL).</span></span>
* <span data-ttu-id="4bc2b-162">Créent une page _Message Razor et l’ajoutent à la RCL.</span><span class="sxs-lookup"><span data-stu-id="4bc2b-162">Creates a Razor _Message page, and adds it to the RCL.</span></span> <span data-ttu-id="4bc2b-163">Le paramètre `-np` crée la page sans `PageModel`.</span><span class="sxs-lookup"><span data-stu-id="4bc2b-163">The `-np` parameter creates the page without a `PageModel`.</span></span>
* <span data-ttu-id="4bc2b-164">Crée un [_ViewStart.cshtml](xref:mvc/views/layout#running-code-before-each-view) de fichiers et l’ajoute à la RCL.</span><span class="sxs-lookup"><span data-stu-id="4bc2b-164">Creates a [_ViewStart.cshtml](xref:mvc/views/layout#running-code-before-each-view) file and adds it to the RCL.</span></span>

<span data-ttu-id="4bc2b-165">Le *_ViewStart.cshtml* fichier est nécessaire pour utiliser la disposition du projet de Pages Razor (qui est ajoutée à la section suivante).</span><span class="sxs-lookup"><span data-stu-id="4bc2b-165">The *_ViewStart.cshtml* file is required to use the layout of the Razor Pages project (which is added in the next section).</span></span>

---

### <a name="add-razor-files-and-folders-to-the-project"></a><span data-ttu-id="4bc2b-166">Ajouter les fichiers Razor et des dossiers au projet</span><span class="sxs-lookup"><span data-stu-id="4bc2b-166">Add Razor files and folders to the project</span></span>

* <span data-ttu-id="4bc2b-167">Remplacez le balisage dans *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* par le code suivant :</span><span class="sxs-lookup"><span data-stu-id="4bc2b-167">Replace the markup in *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* with the following code:</span></span>

[!code-cshtml[Main](ui-class/samples/cli/RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml)]

* <span data-ttu-id="4bc2b-168">Remplacez le balisage dans *RazorUIClassLib/Areas/MyFeature/Pages/Page1.cshtml* par le code suivant :</span><span class="sxs-lookup"><span data-stu-id="4bc2b-168">Replace the markup in *RazorUIClassLib/Areas/MyFeature/Pages/Page1.cshtml* with the following code:</span></span>

[!code-cshtml[Main](ui-class/samples/cli/RazorUIClassLib/Areas/MyFeature/Pages/Page1.cshtml)]

<span data-ttu-id="4bc2b-169">`@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers` est nécessaire pour utiliser la vue partielle (`<partial name="_Message" />`).</span><span class="sxs-lookup"><span data-stu-id="4bc2b-169">`@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers` is required to use the partial view (`<partial name="_Message" />`).</span></span> <span data-ttu-id="4bc2b-170">Au lieu d’inclure la directive `@addTagHelper`, vous pouvez ajouter un fichier *_ViewImports.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="4bc2b-170">Rather than including the `@addTagHelper` directive, you can add a *_ViewImports.cshtml* file.</span></span> <span data-ttu-id="4bc2b-171">Exemple :</span><span class="sxs-lookup"><span data-stu-id="4bc2b-171">For example:</span></span>

```console
dotnet new viewimports -o RazorUIClassLib/Areas/MyFeature/Pages
```

<span data-ttu-id="4bc2b-172">Pour plus d’informations sur *_ViewImports.cshtml*, consultez [importation de Directives partagées](xref:mvc/views/layout#importing-shared-directives)</span><span class="sxs-lookup"><span data-stu-id="4bc2b-172">For more information on *_ViewImports.cshtml*, see [Importing Shared Directives](xref:mvc/views/layout#importing-shared-directives)</span></span>

* <span data-ttu-id="4bc2b-173">Générez la bibliothèque de classes pour vérifier l’absence d’erreurs de compilateur :</span><span class="sxs-lookup"><span data-stu-id="4bc2b-173">Build the class library to verify there are no compiler errors:</span></span>

```console
dotnet build RazorUIClassLib
```

<span data-ttu-id="4bc2b-174">La sortie de build contient *RazorUIClassLib.dll* et *RazorUIClassLib.Views.dll*.</span><span class="sxs-lookup"><span data-stu-id="4bc2b-174">The build output contains *RazorUIClassLib.dll* and *RazorUIClassLib.Views.dll*.</span></span> <span data-ttu-id="4bc2b-175">*RazorUIClassLib.Views.dll* contient le contenu Razor compilé.</span><span class="sxs-lookup"><span data-stu-id="4bc2b-175">*RazorUIClassLib.Views.dll* contains the compiled Razor content.</span></span>

### <a name="use-the-razor-ui-library-from-a-razor-pages-project"></a><span data-ttu-id="4bc2b-176">Utiliser la bibliothèque de l’interface utilisateur Razor à partir d’un projet Razor Pages</span><span class="sxs-lookup"><span data-stu-id="4bc2b-176">Use the Razor UI library from a Razor Pages project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="4bc2b-177">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="4bc2b-177">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="4bc2b-178">Créez l’application web Razor Pages :</span><span class="sxs-lookup"><span data-stu-id="4bc2b-178">Create the Razor Pages web app:</span></span>

* <span data-ttu-id="4bc2b-179">Dans **l’Explorateur de solutions**, cliquez avec le bouton droit sur la solution > **Ajouter** >  **Nouveau projet**.</span><span class="sxs-lookup"><span data-stu-id="4bc2b-179">From **Solution Explorer**, right-click the solution > **Add** >  **New Project**.</span></span>
* <span data-ttu-id="4bc2b-180">Sélectionnez **Nouvelle application web ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="4bc2b-180">Select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="4bc2b-181">Nommez l’application **WebApp1**.</span><span class="sxs-lookup"><span data-stu-id="4bc2b-181">Name the app **WebApp1**.</span></span>
* <span data-ttu-id="4bc2b-182">Vérifiez que **ASP.NET Core 2.1** ou ultérieur est sélectionné.</span><span class="sxs-lookup"><span data-stu-id="4bc2b-182">Verify **ASP.NET Core 2.1** or later is selected.</span></span>
* <span data-ttu-id="4bc2b-183">Sélectionnez **Application web** > **OK**.</span><span class="sxs-lookup"><span data-stu-id="4bc2b-183">Select **Web Application** > **OK**.</span></span>

* <span data-ttu-id="4bc2b-184">Dans **l’Explorateur de solutions**, cliquez avec le bouton droit sur **WebApp1**, puis sélectionnez **Définir comme projet de démarrage**.</span><span class="sxs-lookup"><span data-stu-id="4bc2b-184">From **Solution Explorer**, right-click on **WebApp1** and select **Set as StartUp Project**.</span></span>
* <span data-ttu-id="4bc2b-185">Dans **l’Explorateur de solutions**, cliquez avec le bouton droit sur **WebApp1**, puis sélectionnez **Dépendances de build** > **Dépendances du projet**.</span><span class="sxs-lookup"><span data-stu-id="4bc2b-185">From **Solution Explorer**, right-click on **WebApp1** and select **Build Dependencies** > **Project Dependencies**.</span></span>
* <span data-ttu-id="4bc2b-186">Cochez **RazorUIClassLib** comme dépendance de **WebApp1**.</span><span class="sxs-lookup"><span data-stu-id="4bc2b-186">Check **RazorUIClassLib** as a dependency of **WebApp1**.</span></span>
* <span data-ttu-id="4bc2b-187">Dans **l’Explorateur de solutions**, cliquez avec le bouton droit sur **WebApp1**, puis sélectionnez **Ajouter** > **Référence**.</span><span class="sxs-lookup"><span data-stu-id="4bc2b-187">From **Solution Explorer**, right-click on **WebApp1** and select **Add** > **Reference**.</span></span>
* <span data-ttu-id="4bc2b-188">Dans la boîte de dialogue **Gestionnaire de références**, cochez **RazorUIClassLib** > **OK**.</span><span class="sxs-lookup"><span data-stu-id="4bc2b-188">In the **Reference Manager** dialog, check **RazorUIClassLib** > **OK**.</span></span>

<span data-ttu-id="4bc2b-189">Exécuter l’application.</span><span class="sxs-lookup"><span data-stu-id="4bc2b-189">Run the app.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="4bc2b-190">CLI .NET Core</span><span class="sxs-lookup"><span data-stu-id="4bc2b-190">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="4bc2b-191">Créez une application web Razor Pages et un fichier de solution contenant l’application Razor Pages et la RCL :</span><span class="sxs-lookup"><span data-stu-id="4bc2b-191">Create a Razor Pages web app and a solution file containing the Razor Pages app and the Razor Class Library:</span></span>

```console
dotnet new webapp -o WebApp1
dotnet new sln
dotnet sln add WebApp1
dotnet sln add RazorUIClassLib
dotnet add WebApp1 reference RazorUIClassLib
```

<span data-ttu-id="4bc2b-192">Générez et exécutez l’application web :</span><span class="sxs-lookup"><span data-stu-id="4bc2b-192">Build and run the web app:</span></span>

```console
cd WebApp1
dotnet run
```

---

<a name="test"></a>

### <a name="test-webapp1"></a><span data-ttu-id="4bc2b-193">Tester WebApp1</span><span class="sxs-lookup"><span data-stu-id="4bc2b-193">Test WebApp1</span></span>

<span data-ttu-id="4bc2b-194">Vérifiez que la bibliothèque de classes de l’interface utilisateur Razor est utilisée.</span><span class="sxs-lookup"><span data-stu-id="4bc2b-194">Verify the Razor UI class library is being used.</span></span>

* <span data-ttu-id="4bc2b-195">Accédez à `/MyFeature/Page1`.</span><span class="sxs-lookup"><span data-stu-id="4bc2b-195">Browse to `/MyFeature/Page1`.</span></span>

## <a name="override-views-partial-views-and-pages"></a><span data-ttu-id="4bc2b-196">Substituer des vues, des vues partielles et des pages</span><span class="sxs-lookup"><span data-stu-id="4bc2b-196">Override views, partial views, and pages</span></span>

<span data-ttu-id="4bc2b-197">Quand une vue, une vue partielle ou une page Razor est présente dans l’application web et la RCL, le balisage Razor (fichier *.cshtml*) dans l’application web est prioritaire.</span><span class="sxs-lookup"><span data-stu-id="4bc2b-197">When a view, partial view, or Razor Page is found in both the web app and the Razor Class Library, the Razor markup (*.cshtml* file) in the web app takes precedence.</span></span> <span data-ttu-id="4bc2b-198">Par exemple, ajoutez *WebApp1/Areas/MyFeature/Pages/Page1.cshtml* à l’application Web 1, et Page1 dans l’application Web 1 ont priorité sur Page1 dans la bibliothèque de classes Razor.</span><span class="sxs-lookup"><span data-stu-id="4bc2b-198">For example, add *WebApp1/Areas/MyFeature/Pages/Page1.cshtml* to WebApp1, and Page1 in the WebApp1 will take precedence over Page1 in the Razor Class Library.</span></span>

<span data-ttu-id="4bc2b-199">Dans l’exemple proposé sous forme de téléchargement, renommez *WebApp1/Areas/MyFeature2* en *WebApp1/Areas/MyFeature* pour tester la priorité.</span><span class="sxs-lookup"><span data-stu-id="4bc2b-199">In the sample download, rename *WebApp1/Areas/MyFeature2* to *WebApp1/Areas/MyFeature* to test precedence.</span></span>

<span data-ttu-id="4bc2b-200">Copiez la vue partielle *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* dans *WebApp1/Areas/MyFeature/Pages/Shared/_Message.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="4bc2b-200">Copy the *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* partial view to *WebApp1/Areas/MyFeature/Pages/Shared/_Message.cshtml*.</span></span> <span data-ttu-id="4bc2b-201">Mettez à jour le balisage pour indiquer le nouvel emplacement.</span><span class="sxs-lookup"><span data-stu-id="4bc2b-201">Update the markup to indicate the new location.</span></span> <span data-ttu-id="4bc2b-202">Générez et exécutez l’application pour vérifier que la version de la vue partielle de l’application est utilisée.</span><span class="sxs-lookup"><span data-stu-id="4bc2b-202">Build and run the app to verify the app's version of the partial is being used.</span></span>

<a name="afs"></a>

### <a name="rcl-pages-layout"></a><span data-ttu-id="4bc2b-203">Disposition des Pages de RCL</span><span class="sxs-lookup"><span data-stu-id="4bc2b-203">RCL Pages layout</span></span>

<span data-ttu-id="4bc2b-204">Pour référence RCL de contenu comme s’il faisait partie de l’application web *Pages* dossier, créez le projet RCL avec la structure de fichier suivante :</span><span class="sxs-lookup"><span data-stu-id="4bc2b-204">To reference RCL content as though it is part of the web app's *Pages* folder, create the RCL project with the following file structure:</span></span>

* <span data-ttu-id="4bc2b-205">*RazorUIClassLib/Pages*</span><span class="sxs-lookup"><span data-stu-id="4bc2b-205">*RazorUIClassLib/Pages*</span></span>
* <span data-ttu-id="4bc2b-206">*RazorUIClassLib/Pages/Shared*</span><span class="sxs-lookup"><span data-stu-id="4bc2b-206">*RazorUIClassLib/Pages/Shared*</span></span>

<span data-ttu-id="4bc2b-207">Supposons que *RazorUIClassLib/Pages/Shared* contient deux fichiers partielles : *_Header.cshtml* et *_Footer.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="4bc2b-207">Suppose *RazorUIClassLib/Pages/Shared* contains two partial files: *_Header.cshtml* and *_Footer.cshtml*.</span></span> <span data-ttu-id="4bc2b-208">Le `<partial>` balises peut être ajoutés à *_Layout.cshtml* fichier :</span><span class="sxs-lookup"><span data-stu-id="4bc2b-208">The `<partial>` tags could be added to *_Layout.cshtml* file:</span></span>

```cshtml
<body>
  <partial name="_Header">
  @RenderBody()
  <partial name="_Footer">
</body>
```
