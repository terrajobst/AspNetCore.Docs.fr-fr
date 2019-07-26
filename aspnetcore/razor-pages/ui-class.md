---
title: Interface utilisateur Razor réutilisable dans les bibliothèques de classes avec ASP.NET Core
author: Rick-Anderson
description: Explique comment créer l’interface utilisateur de Razor réutilisables à l’aide de vues partielles dans une bibliothèque de classes dans ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 06/28/2019
ms.custom: mvc, seodec18
uid: razor-pages/ui-class
ms.openlocfilehash: 77c7d4a318610fcd424da0485abd41d11e3fad6a
ms.sourcegitcommit: fbc66827e319d28bebed678ea5fd42f582fe3c34
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/25/2019
ms.locfileid: "68493564"
---
# <a name="create-reusable-ui-using-the-razor-class-library-project-in-aspnet-core"></a><span data-ttu-id="80efb-103">Créer une interface utilisateur réutilisable à l’aide du projet de bibliothèque de classes Razor dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="80efb-103">Create reusable UI using the Razor class library project in ASP.NET Core</span></span>

<span data-ttu-id="80efb-104">Par [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="80efb-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="80efb-105">Les vues Razor, les pages, les contrôleurs, les modèles de page, les [composants Razor](xref:blazor/class-libraries), les [composants de vue](xref:mvc/views/view-components)et les modèles de données peuvent être intégrés dans une bibliothèque de classes Razor (RCL).</span><span class="sxs-lookup"><span data-stu-id="80efb-105">Razor views, pages, controllers, page models, [Razor components](xref:blazor/class-libraries), [View components](xref:mvc/views/view-components), and data models can be built into a Razor class library (RCL).</span></span> <span data-ttu-id="80efb-106">La RCL peut être empaquetée et réutilisée.</span><span class="sxs-lookup"><span data-stu-id="80efb-106">The RCL can be packaged and reused.</span></span> <span data-ttu-id="80efb-107">Les applications peuvent inclure la RCL et remplacer les vues et les pages qu’elle contient.</span><span class="sxs-lookup"><span data-stu-id="80efb-107">Applications can include the RCL and override the views and pages it contains.</span></span> <span data-ttu-id="80efb-108">Quand une vue, une vue partielle ou une page Razor est présente dans l’application web et la RCL, le balisage Razor (fichier *.cshtml*) dans l’application web est prioritaire.</span><span class="sxs-lookup"><span data-stu-id="80efb-108">When a view, partial view, or Razor Page is found in both the web app and the RCL, the Razor markup (*.cshtml* file) in the web app takes precedence.</span></span>

<span data-ttu-id="80efb-109">Cette fonctionnalité nécessite [!INCLUDE[](~/includes/2.1-SDK.md)]</span><span class="sxs-lookup"><span data-stu-id="80efb-109">This feature requires [!INCLUDE[](~/includes/2.1-SDK.md)]</span></span>

<span data-ttu-id="80efb-110">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/ui-class/samples) ([procédure de téléchargement](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="80efb-110">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/ui-class/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="create-a-class-library-containing-razor-ui"></a><span data-ttu-id="80efb-111">Créer une bibliothèque de classes contenant l’interface utilisateur Razor</span><span class="sxs-lookup"><span data-stu-id="80efb-111">Create a class library containing Razor UI</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="80efb-112">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="80efb-112">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="80efb-113">Dans Visual Studio, dans le menu **Fichier**, sélectionnez **Nouveau** > **Projet**.</span><span class="sxs-lookup"><span data-stu-id="80efb-113">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="80efb-114">Sélectionnez **Nouvelle application web ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="80efb-114">Select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="80efb-115">Nommez la bibliothèque (par exemple, « RazorClassLib ») > **OK**.</span><span class="sxs-lookup"><span data-stu-id="80efb-115">Name the library (for example, "RazorClassLib") > **OK**.</span></span> <span data-ttu-id="80efb-116">Pour éviter une collision de nom de fichier avec la bibliothèque de vues générée, vérifiez que le nom de la bibliothèque ne se termine pas par `.Views`.</span><span class="sxs-lookup"><span data-stu-id="80efb-116">To avoid a file name collision with the generated view library, ensure the library name doesn't end in `.Views`.</span></span>
* <span data-ttu-id="80efb-117">Vérifiez que **ASP.NET Core 2.1** ou ultérieur est sélectionné.</span><span class="sxs-lookup"><span data-stu-id="80efb-117">Verify **ASP.NET Core 2.1** or later is selected.</span></span>
* <span data-ttu-id="80efb-118">Sélectionnez **Razor Class Library** > **OK**.</span><span class="sxs-lookup"><span data-stu-id="80efb-118">Select **Razor Class Library** > **OK**.</span></span>

<span data-ttu-id="80efb-119">Un RCL a le fichier projet suivant:</span><span class="sxs-lookup"><span data-stu-id="80efb-119">An RCL has the following project file:</span></span>

[!code-xml[Main](ui-class/samples/cli/RazorUIClassLib/RazorUIClassLib.csproj)]

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="80efb-120">CLI .NET Core</span><span class="sxs-lookup"><span data-stu-id="80efb-120">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="80efb-121">À partir de la ligne de commande, exécutez `dotnet new razorclasslib`.</span><span class="sxs-lookup"><span data-stu-id="80efb-121">From the command line, run `dotnet new razorclasslib`.</span></span> <span data-ttu-id="80efb-122">Exemple :</span><span class="sxs-lookup"><span data-stu-id="80efb-122">For example:</span></span>

```console
dotnet new razorclasslib -o RazorUIClassLib
```

<span data-ttu-id="80efb-123">Pour plus d’informations, consultez [dotnet new](/dotnet/core/tools/dotnet-new).</span><span class="sxs-lookup"><span data-stu-id="80efb-123">For more information, see [dotnet new](/dotnet/core/tools/dotnet-new).</span></span> <span data-ttu-id="80efb-124">Pour éviter une collision de nom de fichier avec la bibliothèque de vues générée, vérifiez que le nom de la bibliothèque ne se termine pas par `.Views`.</span><span class="sxs-lookup"><span data-stu-id="80efb-124">To avoid a file name collision with the generated view library, ensure the library name doesn't end in `.Views`.</span></span>

---

<span data-ttu-id="80efb-125">Ajoutez des fichiers Razor à la RCL.</span><span class="sxs-lookup"><span data-stu-id="80efb-125">Add Razor files to the RCL.</span></span>

<span data-ttu-id="80efb-126">Les modèles ASP.NET Core supposent que le contenu RCL se trouve dans le *zones* dossier.</span><span class="sxs-lookup"><span data-stu-id="80efb-126">The ASP.NET Core templates assume the RCL content is in the *Areas* folder.</span></span> <span data-ttu-id="80efb-127">Consultez [RCL pages layout](#afs) pour créer un RCL qui expose du contenu `~/Pages` dans plutôt `~/Areas/Pages`que.</span><span class="sxs-lookup"><span data-stu-id="80efb-127">See [RCL Pages layout](#afs) to create an RCL that exposes content in `~/Pages` rather than `~/Areas/Pages`.</span></span>

## <a name="referencing-rcl-content"></a><span data-ttu-id="80efb-128">Référencement du contenu RCL</span><span class="sxs-lookup"><span data-stu-id="80efb-128">Referencing RCL content</span></span>

<span data-ttu-id="80efb-129">La RCL peut être référencée par :</span><span class="sxs-lookup"><span data-stu-id="80efb-129">The RCL can be referenced by:</span></span>

* <span data-ttu-id="80efb-130">Un package NuGet.</span><span class="sxs-lookup"><span data-stu-id="80efb-130">NuGet package.</span></span> <span data-ttu-id="80efb-131">Consultez [Création de packages NuGet](/nuget/create-packages/creating-a-package), [dotnet add package](/dotnet/core/tools/dotnet-add-package) et [Créer et publier un package NuGet](/nuget/quickstart/create-and-publish-a-package-using-visual-studio).</span><span class="sxs-lookup"><span data-stu-id="80efb-131">See [Creating NuGet packages](/nuget/create-packages/creating-a-package) and [dotnet add package](/dotnet/core/tools/dotnet-add-package) and [Create and publish a NuGet package](/nuget/quickstart/create-and-publish-a-package-using-visual-studio).</span></span>
* <span data-ttu-id="80efb-132">Un fichier *{NomProjet}.csproj*.</span><span class="sxs-lookup"><span data-stu-id="80efb-132">*{ProjectName}.csproj*.</span></span> <span data-ttu-id="80efb-133">Consultez [dotnet-add reference](/dotnet/core/tools/dotnet-add-reference).</span><span class="sxs-lookup"><span data-stu-id="80efb-133">See [dotnet-add reference](/dotnet/core/tools/dotnet-add-reference).</span></span>

## <a name="walkthrough-create-an-rcl-project-and-use-from-a-razor-pages-project"></a><span data-ttu-id="80efb-134">Procédure pas à pas : Créer un projet RCL et l’utiliser à partir d’un projet Razor Pages</span><span class="sxs-lookup"><span data-stu-id="80efb-134">Walkthrough: Create an RCL project and use from a Razor Pages project</span></span>

<span data-ttu-id="80efb-135">Vous pouvez télécharger et tester le [projet complet](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/ui-class/samples) au lieu de le créer de toutes pièces.</span><span class="sxs-lookup"><span data-stu-id="80efb-135">You can download the [complete project](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/ui-class/samples) and test it rather than creating it.</span></span> <span data-ttu-id="80efb-136">L’exemple proposé sous forme de téléchargement contient du code et des liens supplémentaires qui facilitent le test du projet.</span><span class="sxs-lookup"><span data-stu-id="80efb-136">The sample download contains additional code and links that make the project easy to test.</span></span> <span data-ttu-id="80efb-137">Si vous souhaitez commenter le mode d’obtention des exemples (téléchargement ou création au moyen d’instructions détaillées), entrez vos commentaires dans [ce problème GitHub](https://github.com/aspnet/AspNetCore.Docs/issues/6098).</span><span class="sxs-lookup"><span data-stu-id="80efb-137">You can leave feedback in [this GitHub issue](https://github.com/aspnet/AspNetCore.Docs/issues/6098) with your comments on download samples versus step-by-step instructions.</span></span>

### <a name="test-the-download-app"></a><span data-ttu-id="80efb-138">Tester l’application téléchargée</span><span class="sxs-lookup"><span data-stu-id="80efb-138">Test the download app</span></span>

<span data-ttu-id="80efb-139">Si vous n’avez pas téléchargé l’application complète et que vous souhaitez créer le projet pas à pas, passez à la [section suivante](#create-an-rcl).</span><span class="sxs-lookup"><span data-stu-id="80efb-139">If you haven't downloaded the completed app and would rather create the walkthrough project, skip to the [next section](#create-an-rcl).</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="80efb-140">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="80efb-140">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="80efb-141">Ouvrez le fichier *.sln* dans Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="80efb-141">Open the *.sln* file in Visual Studio.</span></span> <span data-ttu-id="80efb-142">Exécuter l’application.</span><span class="sxs-lookup"><span data-stu-id="80efb-142">Run the app.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="80efb-143">CLI .NET Core</span><span class="sxs-lookup"><span data-stu-id="80efb-143">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="80efb-144">À partir d’une invite de commandes dans le répertoire *cli*, générez la RCL et l’application web.</span><span class="sxs-lookup"><span data-stu-id="80efb-144">From a command prompt in the *cli* directory, build the RCL and web app.</span></span>

```console
dotnet build
```

<span data-ttu-id="80efb-145">Passez au répertoire *WebApp1* et exécutez l’application :</span><span class="sxs-lookup"><span data-stu-id="80efb-145">Move to the *WebApp1* directory and run the app:</span></span>

```console
dotnet run
```

---

<span data-ttu-id="80efb-146">Suivez les instructions contenues dans [Test WebApp1](#test)</span><span class="sxs-lookup"><span data-stu-id="80efb-146">Follow the instructions in [Test WebApp1](#test)</span></span>

## <a name="create-an-rcl"></a><span data-ttu-id="80efb-147">Créer un RCL</span><span class="sxs-lookup"><span data-stu-id="80efb-147">Create an RCL</span></span>

<span data-ttu-id="80efb-148">Dans cette section, un RCL est créé.</span><span class="sxs-lookup"><span data-stu-id="80efb-148">In this section, an RCL is created.</span></span> <span data-ttu-id="80efb-149">Des fichiers Razor sont ajoutés à la RCL.</span><span class="sxs-lookup"><span data-stu-id="80efb-149">Razor files are added to the RCL.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="80efb-150">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="80efb-150">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="80efb-151">Créez le projet RCL :</span><span class="sxs-lookup"><span data-stu-id="80efb-151">Create the RCL project:</span></span>

* <span data-ttu-id="80efb-152">Dans Visual Studio, dans le menu **Fichier**, sélectionnez **Nouveau** > **Projet**.</span><span class="sxs-lookup"><span data-stu-id="80efb-152">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="80efb-153">Sélectionnez **Nouvelle application web ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="80efb-153">Select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="80efb-154">Nommez l’application **RazorUIClassLib** > **OK**.</span><span class="sxs-lookup"><span data-stu-id="80efb-154">Name the app **RazorUIClassLib** > **OK**.</span></span>
* <span data-ttu-id="80efb-155">Vérifiez que **ASP.NET Core 2.1** ou ultérieur est sélectionné.</span><span class="sxs-lookup"><span data-stu-id="80efb-155">Verify **ASP.NET Core 2.1** or later is selected.</span></span>
* <span data-ttu-id="80efb-156">Sélectionnez **Razor Class Library** > **OK**.</span><span class="sxs-lookup"><span data-stu-id="80efb-156">Select **Razor Class Library** > **OK**.</span></span>
* <span data-ttu-id="80efb-157">Ajoutez un fichier de vue partielle Razor nommé *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="80efb-157">Add a Razor partial view file named *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml*.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="80efb-158">CLI .NET Core</span><span class="sxs-lookup"><span data-stu-id="80efb-158">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="80efb-159">À partir de la ligne de commande, exécutez ce qui suit :</span><span class="sxs-lookup"><span data-stu-id="80efb-159">From the command line, run the following:</span></span>

```console
dotnet new razorclasslib -o RazorUIClassLib
dotnet new page -n _Message -np -o RazorUIClassLib/Areas/MyFeature/Pages/Shared
dotnet new viewstart -o RazorUIClassLib/Areas/MyFeature/Pages
```

<span data-ttu-id="80efb-160">Les commandes précédentes :</span><span class="sxs-lookup"><span data-stu-id="80efb-160">The preceding commands:</span></span>

* <span data-ttu-id="80efb-161">Crée le `RazorUIClassLib` RCL.</span><span class="sxs-lookup"><span data-stu-id="80efb-161">Creates the `RazorUIClassLib` RCL.</span></span>
* <span data-ttu-id="80efb-162">Créent une page _Message Razor et l’ajoutent à la RCL.</span><span class="sxs-lookup"><span data-stu-id="80efb-162">Creates a Razor _Message page, and adds it to the RCL.</span></span> <span data-ttu-id="80efb-163">Le paramètre `-np` crée la page sans `PageModel`.</span><span class="sxs-lookup"><span data-stu-id="80efb-163">The `-np` parameter creates the page without a `PageModel`.</span></span>
* <span data-ttu-id="80efb-164">Crée un [_ViewStart.cshtml](xref:mvc/views/layout#running-code-before-each-view) de fichiers et l’ajoute à la RCL.</span><span class="sxs-lookup"><span data-stu-id="80efb-164">Creates a [_ViewStart.cshtml](xref:mvc/views/layout#running-code-before-each-view) file and adds it to the RCL.</span></span>

<span data-ttu-id="80efb-165">Le *_ViewStart.cshtml* fichier est nécessaire pour utiliser la disposition du projet de Pages Razor (qui est ajoutée à la section suivante).</span><span class="sxs-lookup"><span data-stu-id="80efb-165">The *_ViewStart.cshtml* file is required to use the layout of the Razor Pages project (which is added in the next section).</span></span>

---

### <a name="add-razor-files-and-folders-to-the-project"></a><span data-ttu-id="80efb-166">Ajouter les fichiers Razor et des dossiers au projet</span><span class="sxs-lookup"><span data-stu-id="80efb-166">Add Razor files and folders to the project</span></span>

* <span data-ttu-id="80efb-167">Remplacez le balisage dans *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* par le code suivant :</span><span class="sxs-lookup"><span data-stu-id="80efb-167">Replace the markup in *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* with the following code:</span></span>

[!code-cshtml[Main](ui-class/samples/cli/RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml)]

* <span data-ttu-id="80efb-168">Remplacez le balisage dans *RazorUIClassLib/Areas/MyFeature/Pages/Page1.cshtml* par le code suivant :</span><span class="sxs-lookup"><span data-stu-id="80efb-168">Replace the markup in *RazorUIClassLib/Areas/MyFeature/Pages/Page1.cshtml* with the following code:</span></span>

[!code-cshtml[Main](ui-class/samples/cli/RazorUIClassLib/Areas/MyFeature/Pages/Page1.cshtml)]

<span data-ttu-id="80efb-169">`@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers` est nécessaire pour utiliser la vue partielle (`<partial name="_Message" />`).</span><span class="sxs-lookup"><span data-stu-id="80efb-169">`@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers` is required to use the partial view (`<partial name="_Message" />`).</span></span> <span data-ttu-id="80efb-170">Au lieu d’inclure la directive `@addTagHelper`, vous pouvez ajouter un fichier *_ViewImports.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="80efb-170">Rather than including the `@addTagHelper` directive, you can add a *_ViewImports.cshtml* file.</span></span> <span data-ttu-id="80efb-171">Exemple :</span><span class="sxs-lookup"><span data-stu-id="80efb-171">For example:</span></span>

```console
dotnet new viewimports -o RazorUIClassLib/Areas/MyFeature/Pages
```

<span data-ttu-id="80efb-172">Pour plus d’informations sur *_ViewImports.cshtml*, consultez [importation de Directives partagées](xref:mvc/views/layout#importing-shared-directives)</span><span class="sxs-lookup"><span data-stu-id="80efb-172">For more information on *_ViewImports.cshtml*, see [Importing Shared Directives](xref:mvc/views/layout#importing-shared-directives)</span></span>

* <span data-ttu-id="80efb-173">Générez la bibliothèque de classes pour vérifier l’absence d’erreurs de compilateur :</span><span class="sxs-lookup"><span data-stu-id="80efb-173">Build the class library to verify there are no compiler errors:</span></span>

```console
dotnet build RazorUIClassLib
```

<span data-ttu-id="80efb-174">La sortie de build contient *RazorUIClassLib.dll* et *RazorUIClassLib.Views.dll*.</span><span class="sxs-lookup"><span data-stu-id="80efb-174">The build output contains *RazorUIClassLib.dll* and *RazorUIClassLib.Views.dll*.</span></span> <span data-ttu-id="80efb-175">*RazorUIClassLib.Views.dll* contient le contenu Razor compilé.</span><span class="sxs-lookup"><span data-stu-id="80efb-175">*RazorUIClassLib.Views.dll* contains the compiled Razor content.</span></span>

### <a name="use-the-razor-ui-library-from-a-razor-pages-project"></a><span data-ttu-id="80efb-176">Utiliser la bibliothèque de l’interface utilisateur Razor à partir d’un projet Razor Pages</span><span class="sxs-lookup"><span data-stu-id="80efb-176">Use the Razor UI library from a Razor Pages project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="80efb-177">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="80efb-177">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="80efb-178">Créez l’application web Razor Pages :</span><span class="sxs-lookup"><span data-stu-id="80efb-178">Create the Razor Pages web app:</span></span>

* <span data-ttu-id="80efb-179">Dans **l’Explorateur de solutions**, cliquez avec le bouton droit sur la solution > **Ajouter** >  **Nouveau projet**.</span><span class="sxs-lookup"><span data-stu-id="80efb-179">From **Solution Explorer**, right-click the solution > **Add** >  **New Project**.</span></span>
* <span data-ttu-id="80efb-180">Sélectionnez **Nouvelle application web ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="80efb-180">Select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="80efb-181">Nommez l’application **WebApp1**.</span><span class="sxs-lookup"><span data-stu-id="80efb-181">Name the app **WebApp1**.</span></span>
* <span data-ttu-id="80efb-182">Vérifiez que **ASP.NET Core 2.1** ou ultérieur est sélectionné.</span><span class="sxs-lookup"><span data-stu-id="80efb-182">Verify **ASP.NET Core 2.1** or later is selected.</span></span>
* <span data-ttu-id="80efb-183">Sélectionnez **Application web** > **OK**.</span><span class="sxs-lookup"><span data-stu-id="80efb-183">Select **Web Application** > **OK**.</span></span>

* <span data-ttu-id="80efb-184">Dans **l’Explorateur de solutions**, cliquez avec le bouton droit sur **WebApp1**, puis sélectionnez **Définir comme projet de démarrage**.</span><span class="sxs-lookup"><span data-stu-id="80efb-184">From **Solution Explorer**, right-click on **WebApp1** and select **Set as StartUp Project**.</span></span>
* <span data-ttu-id="80efb-185">Dans **l’Explorateur de solutions**, cliquez avec le bouton droit sur **WebApp1**, puis sélectionnez **Dépendances de build** > **Dépendances du projet**.</span><span class="sxs-lookup"><span data-stu-id="80efb-185">From **Solution Explorer**, right-click on **WebApp1** and select **Build Dependencies** > **Project Dependencies**.</span></span>
* <span data-ttu-id="80efb-186">Cochez **RazorUIClassLib** comme dépendance de **WebApp1**.</span><span class="sxs-lookup"><span data-stu-id="80efb-186">Check **RazorUIClassLib** as a dependency of **WebApp1**.</span></span>
* <span data-ttu-id="80efb-187">Dans **l’Explorateur de solutions**, cliquez avec le bouton droit sur **WebApp1**, puis sélectionnez **Ajouter** > **Référence**.</span><span class="sxs-lookup"><span data-stu-id="80efb-187">From **Solution Explorer**, right-click on **WebApp1** and select **Add** > **Reference**.</span></span>
* <span data-ttu-id="80efb-188">Dans la boîte de dialogue **Gestionnaire de références**, cochez **RazorUIClassLib** > **OK**.</span><span class="sxs-lookup"><span data-stu-id="80efb-188">In the **Reference Manager** dialog, check **RazorUIClassLib** > **OK**.</span></span>

<span data-ttu-id="80efb-189">Exécuter l’application.</span><span class="sxs-lookup"><span data-stu-id="80efb-189">Run the app.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="80efb-190">CLI .NET Core</span><span class="sxs-lookup"><span data-stu-id="80efb-190">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="80efb-191">Créez un Razor Pages application Web et un fichier solution contenant l’application Razor Pages et le RCL:</span><span class="sxs-lookup"><span data-stu-id="80efb-191">Create a Razor Pages web app and a solution file containing the Razor Pages app and the RCL:</span></span>

```console
dotnet new webapp -o WebApp1
dotnet new sln
dotnet sln add WebApp1
dotnet sln add RazorUIClassLib
dotnet add WebApp1 reference RazorUIClassLib
```

<span data-ttu-id="80efb-192">Générez et exécutez l’application web :</span><span class="sxs-lookup"><span data-stu-id="80efb-192">Build and run the web app:</span></span>

```console
cd WebApp1
dotnet run
```

---

<a name="test"></a>

### <a name="test-webapp1"></a><span data-ttu-id="80efb-193">Tester WebApp1</span><span class="sxs-lookup"><span data-stu-id="80efb-193">Test WebApp1</span></span>

<span data-ttu-id="80efb-194">Vérifiez que la bibliothèque de classes de l’interface utilisateur Razor est en cours d’utilisation:</span><span class="sxs-lookup"><span data-stu-id="80efb-194">Verify the Razor UI class library is in use:</span></span>

* <span data-ttu-id="80efb-195">Accédez à `/MyFeature/Page1`.</span><span class="sxs-lookup"><span data-stu-id="80efb-195">Browse to `/MyFeature/Page1`.</span></span>

## <a name="override-views-partial-views-and-pages"></a><span data-ttu-id="80efb-196">Substituer des vues, des vues partielles et des pages</span><span class="sxs-lookup"><span data-stu-id="80efb-196">Override views, partial views, and pages</span></span>

<span data-ttu-id="80efb-197">Quand une vue, une vue partielle ou une page Razor est présente dans l’application web et la RCL, le balisage Razor (fichier *.cshtml*) dans l’application web est prioritaire.</span><span class="sxs-lookup"><span data-stu-id="80efb-197">When a view, partial view, or Razor Page is found in both the web app and the RCL, the Razor markup (*.cshtml* file) in the web app takes precedence.</span></span> <span data-ttu-id="80efb-198">Par exemple, ajoutez *application Web 1/Areas/MyFeature/pages/Page1. cshtml* à application Web 1, et Page1 dans le application Web 1 aura priorité sur Page1 dans le RCL.</span><span class="sxs-lookup"><span data-stu-id="80efb-198">For example, add *WebApp1/Areas/MyFeature/Pages/Page1.cshtml* to WebApp1, and Page1 in the WebApp1 will take precedence over Page1 in the RCL.</span></span>

<span data-ttu-id="80efb-199">Dans l’exemple proposé sous forme de téléchargement, renommez *WebApp1/Areas/MyFeature2* en *WebApp1/Areas/MyFeature* pour tester la priorité.</span><span class="sxs-lookup"><span data-stu-id="80efb-199">In the sample download, rename *WebApp1/Areas/MyFeature2* to *WebApp1/Areas/MyFeature* to test precedence.</span></span>

<span data-ttu-id="80efb-200">Copiez la vue partielle *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* dans *WebApp1/Areas/MyFeature/Pages/Shared/_Message.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="80efb-200">Copy the *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* partial view to *WebApp1/Areas/MyFeature/Pages/Shared/_Message.cshtml*.</span></span> <span data-ttu-id="80efb-201">Mettez à jour le balisage pour indiquer le nouvel emplacement.</span><span class="sxs-lookup"><span data-stu-id="80efb-201">Update the markup to indicate the new location.</span></span> <span data-ttu-id="80efb-202">Générez et exécutez l’application pour vérifier que la version de la vue partielle de l’application est utilisée.</span><span class="sxs-lookup"><span data-stu-id="80efb-202">Build and run the app to verify the app's version of the partial is being used.</span></span>

<a name="afs"></a>

### <a name="rcl-pages-layout"></a><span data-ttu-id="80efb-203">Disposition des Pages de RCL</span><span class="sxs-lookup"><span data-stu-id="80efb-203">RCL Pages layout</span></span>

<span data-ttu-id="80efb-204">Pour référence RCL de contenu comme s’il faisait partie de l’application web *Pages* dossier, créez le projet RCL avec la structure de fichier suivante :</span><span class="sxs-lookup"><span data-stu-id="80efb-204">To reference RCL content as though it is part of the web app's *Pages* folder, create the RCL project with the following file structure:</span></span>

* <span data-ttu-id="80efb-205">*RazorUIClassLib/Pages*</span><span class="sxs-lookup"><span data-stu-id="80efb-205">*RazorUIClassLib/Pages*</span></span>
* <span data-ttu-id="80efb-206">*RazorUIClassLib/Pages/Shared*</span><span class="sxs-lookup"><span data-stu-id="80efb-206">*RazorUIClassLib/Pages/Shared*</span></span>

<span data-ttu-id="80efb-207">Supposons que *RazorUIClassLib/Pages/Shared* contient deux fichiers partielles : *_Header.cshtml* et *_Footer.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="80efb-207">Suppose *RazorUIClassLib/Pages/Shared* contains two partial files: *_Header.cshtml* and *_Footer.cshtml*.</span></span> <span data-ttu-id="80efb-208">Le `<partial>` balises peut être ajoutés à *_Layout.cshtml* fichier :</span><span class="sxs-lookup"><span data-stu-id="80efb-208">The `<partial>` tags could be added to *_Layout.cshtml* file:</span></span>

```cshtml
<body>
  <partial name="_Header">
  @RenderBody()
  <partial name="_Footer">
</body>
```

::: moniker range=">= aspnetcore-3.0"

## <a name="create-an-rcl-with-static-assets"></a><span data-ttu-id="80efb-209">Créer un RCL avec des ressources statiques</span><span class="sxs-lookup"><span data-stu-id="80efb-209">Create an RCL with static assets</span></span>

<span data-ttu-id="80efb-210">Un RCL peut nécessiter des ressources statiques auxiliaires qui peuvent être référencées par l’application consommatrice de RCL.</span><span class="sxs-lookup"><span data-stu-id="80efb-210">An RCL may require companion static assets that can be referenced by the consuming app of the RCL.</span></span> <span data-ttu-id="80efb-211">ASP.NET Core permet de créer des RCLs qui incluent des ressources statiques disponibles pour une application consommatrice.</span><span class="sxs-lookup"><span data-stu-id="80efb-211">ASP.NET Core allows creating RCLs that include static assets that are available to a consuming app.</span></span>

<span data-ttu-id="80efb-212">Pour inclure des ressources d’accompagnement dans le cadre d’un RCL, créez un dossier *wwwroot* dans la bibliothèque de classes et incluez tous les fichiers nécessaires dans ce dossier.</span><span class="sxs-lookup"><span data-stu-id="80efb-212">To include companion assets as part of an RCL, create a *wwwroot* folder in the class library and include any required files in that folder.</span></span>

<span data-ttu-id="80efb-213">Lors de la compression d’un RCL, toutes les ressources associées dans le dossier *wwwroot* sont automatiquement incluses dans le package et mises à la disposition des applications faisant référence au package.</span><span class="sxs-lookup"><span data-stu-id="80efb-213">When packing an RCL, all companion assets in the *wwwroot* folder are included in the package automatically and are made available to apps referencing the package.</span></span>

### <a name="consume-content-from-a-referenced-rcl"></a><span data-ttu-id="80efb-214">Consommer du contenu à partir d’un RCL référencé</span><span class="sxs-lookup"><span data-stu-id="80efb-214">Consume content from a referenced RCL</span></span>

<span data-ttu-id="80efb-215">Les fichiers inclus dans le dossier *wwwroot* du RCL sont exposés à l’application consommatrice sous le préfixe `_content/{LIBRARY NAME}/`.</span><span class="sxs-lookup"><span data-stu-id="80efb-215">The files included in the *wwwroot* folder of the RCL are exposed to the consuming app under the prefix `_content/{LIBRARY NAME}/`.</span></span> <span data-ttu-id="80efb-216">Par exemple, une bibliothèque nommée *Razor. class. lib* génère un chemin d’accès au contenu statique `_content/Razor.Class.Lib/`à l’emplacement.</span><span class="sxs-lookup"><span data-stu-id="80efb-216">For example, a library named *Razor.Class.Lib* results in a path to static content at `_content/Razor.Class.Lib/`.</span></span>

<span data-ttu-id="80efb-217">L’application consommatrice référence les ressources statiques fournies par la bibliothèque `<script>`avec `<style>` `<img>`,, et d’autres balises html.</span><span class="sxs-lookup"><span data-stu-id="80efb-217">The consuming app references static assets provided by the library with `<script>`, `<style>`, `<img>`, and other HTML tags.</span></span> <span data-ttu-id="80efb-218">La [prise en charge des fichiers statiques](xref:fundamentals/static-files) doit être activée pour l’application consommatrice.</span><span class="sxs-lookup"><span data-stu-id="80efb-218">The consuming app must have [static file support](xref:fundamentals/static-files) enabled.</span></span>

### <a name="multi-project-development-flow"></a><span data-ttu-id="80efb-219">Déroulement du développement de projets multiples</span><span class="sxs-lookup"><span data-stu-id="80efb-219">Multi-project development flow</span></span>

<span data-ttu-id="80efb-220">Lorsque l’application consommatrice s’exécute:</span><span class="sxs-lookup"><span data-stu-id="80efb-220">When the consuming app runs:</span></span>

* <span data-ttu-id="80efb-221">Les ressources du RCL restent dans leurs dossiers d’origine.</span><span class="sxs-lookup"><span data-stu-id="80efb-221">The assets in the RCL stay in their original folders.</span></span> <span data-ttu-id="80efb-222">Les ressources ne sont pas déplacées vers l’application consommatrice.</span><span class="sxs-lookup"><span data-stu-id="80efb-222">The assets aren't moved to the consuming app.</span></span>
* <span data-ttu-id="80efb-223">Toute modification dans le dossier *wwwroot* de RCL est reflétée dans l’application consommatrice une fois que le RCL est reconstruit et sans régénérer l’application consommateur.</span><span class="sxs-lookup"><span data-stu-id="80efb-223">Any change within the RCL's *wwwroot* folder is reflected in the consuming app after the RCL is rebuilt and without rebuilding the consuming app.</span></span>

<span data-ttu-id="80efb-224">Lorsque le RCL est généré, un manifeste qui décrit les emplacements des ressources Web statiques est généré.</span><span class="sxs-lookup"><span data-stu-id="80efb-224">When the RCL is built, a manifest is produced that describes the static web asset locations.</span></span> <span data-ttu-id="80efb-225">L’application consommatrice lit le manifeste au moment de l’exécution pour consommer les ressources des packages et des projets référencés.</span><span class="sxs-lookup"><span data-stu-id="80efb-225">The consuming app reads the manifest at runtime to consume the assets from referenced projects and packages.</span></span> <span data-ttu-id="80efb-226">Lorsqu’un nouvel élément multimédia est ajouté à un RCL, le RCL doit être régénéré pour mettre à jour son manifeste avant qu’une application consommatrice puisse accéder au nouvel élément multimédia.</span><span class="sxs-lookup"><span data-stu-id="80efb-226">When a new asset is added to an RCL, the RCL must be rebuilt to update its manifest before a consuming app can access the new asset.</span></span>

### <a name="publish"></a><span data-ttu-id="80efb-227">Publish</span><span class="sxs-lookup"><span data-stu-id="80efb-227">Publish</span></span>

<span data-ttu-id="80efb-228">Lorsque l’application est publiée, les ressources complémentaires de tous les packages et projets référencés sont copiées dans le dossier *wwwroot* de l’application `_content/{LIBRARY NAME}/`publiée sous.</span><span class="sxs-lookup"><span data-stu-id="80efb-228">When the app is published, the companion assets from all referenced projects and packages are copied into the *wwwroot* folder of the published app under `_content/{LIBRARY NAME}/`.</span></span>

::: moniker-end
