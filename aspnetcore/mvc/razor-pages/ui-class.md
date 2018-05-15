---
title: Interface utilisateur Razor réutilisable dans les bibliothèques de classes avec ASP.NET Core
author: Rick-Anderson
description: Explique comment créer une interface utilisateur Razor réutilisable dans une bibliothèque de classes.
manager: wpickett
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 4/31/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: advanced
uid: mvc/razor-pages/ui-class
ms.openlocfilehash: 731d37a8f4983b18ded114f05470f8a408deb7cd
ms.sourcegitcommit: 5130b3034165f5cf49d829fe7475a84aa33d2693
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="create-reusable-ui-using-the-razor-class-library-project-in-aspnet-core"></a><span data-ttu-id="a6e23-103">Créez une interface utilisateur réutilisable à l’aide du projet Razor Class Library dans ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="a6e23-103">Create reusable UI using the Razor Class Library project in ASP.NET Core.</span></span>

<span data-ttu-id="a6e23-104">Par [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="a6e23-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="a6e23-105">Les vues, pages, contrôleurs, modèles de page et modèles de données Razor peuvent être intégrés à une bibliothèque de classes Razor (RCL, Razor Class Library).</span><span class="sxs-lookup"><span data-stu-id="a6e23-105">Razor views, pages, controllers, page models, and data models can be built into a Razor Class Library(RCL).</span></span> <span data-ttu-id="a6e23-106">La RCL peut être empaquetée et réutilisée.</span><span class="sxs-lookup"><span data-stu-id="a6e23-106">The RCL can be and packaged and reused.</span></span> <span data-ttu-id="a6e23-107">Les applications peuvent inclure la RCL et remplacer les vues et les pages qu’elle contient.</span><span class="sxs-lookup"><span data-stu-id="a6e23-107">Applications can include the RCL and override the views and pages it contains.</span></span> <span data-ttu-id="a6e23-108">Quand une vue, une vue partielle ou une page Razor est présente dans l’application web et la RCL, le balisage Razor (fichier *.cshtml*) dans l’application web est prioritaire.</span><span class="sxs-lookup"><span data-stu-id="a6e23-108">When a view, partial view, or Razor Page is found in both the web app and the RCL, the Razor markup (*.cshtml* file) in the web app takes precedence.</span></span>

<span data-ttu-id="a6e23-109">Cette fonctionnalité nécessite [!INCLUDE[](~/includes/2.1-SDK.md)].</span><span class="sxs-lookup"><span data-stu-id="a6e23-109">This feature requires [!INCLUDE[](~/includes/2.1-SDK.md)]</span></span>

[!INCLUDE[](~/includes/2.1.md)]

<span data-ttu-id="a6e23-110">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/razor-pages/ui-class/samples) ([procédure de téléchargement](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="a6e23-110">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/razor-pages/ui-class/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="create-a-class-library-containing-razor-ui"></a><span data-ttu-id="a6e23-111">Créer une bibliothèque de classes contenant l’interface utilisateur Razor</span><span class="sxs-lookup"><span data-stu-id="a6e23-111">Create a class library containing Razor UI</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="a6e23-112">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a6e23-112">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="a6e23-113">Dans Visual Studio, dans le menu **Fichier**, sélectionnez **Nouveau** > **Projet**.</span><span class="sxs-lookup"><span data-stu-id="a6e23-113">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="a6e23-114">Sélectionnez **Nouvelle application web ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="a6e23-114">Select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="a6e23-115">Vérifiez que **ASP.NET Core 2.1** ou ultérieur est sélectionné.</span><span class="sxs-lookup"><span data-stu-id="a6e23-115">Verify **ASP.NET Core 2.1** or later is selected.</span></span>
* <span data-ttu-id="a6e23-116">Sélectionnez **Razor Class Library** > **OK**.</span><span class="sxs-lookup"><span data-stu-id="a6e23-116">Select **Razor Class Library** > **OK**.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="a6e23-117">CLI .NET Core</span><span class="sxs-lookup"><span data-stu-id="a6e23-117">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="a6e23-118">À partir de la ligne de commande, exécutez `dotnet new razorclasslib`.</span><span class="sxs-lookup"><span data-stu-id="a6e23-118">From the commandline, run `dotnet new razorclasslib`.</span></span> <span data-ttu-id="a6e23-119">Exemple :</span><span class="sxs-lookup"><span data-stu-id="a6e23-119">For example:</span></span>

``` CLI
dotnet new razorclasslib -o RazorUIClassLib
```

<span data-ttu-id="a6e23-120">Pour plus d’informations, consultez [dotnet new](/dotnet/core/tools/dotnet-new).</span><span class="sxs-lookup"><span data-stu-id="a6e23-120">For more information, see [dotnet new](/dotnet/core/tools/dotnet-new).</span></span>

------
<span data-ttu-id="a6e23-121">Ajoutez des fichiers Razor à la RCL.</span><span class="sxs-lookup"><span data-stu-id="a6e23-121">Add Razor files to the RCL.</span></span>

<span data-ttu-id="a6e23-122">Nous vous recommandons de placer le contenu RCL dans le dossier *Areas*.</span><span class="sxs-lookup"><span data-stu-id="a6e23-122">We recommend RCL content go in the *Areas* folder.</span></span> 


## <a name="referencing-razor-class-library-content"></a><span data-ttu-id="a6e23-123">Référencement de contenu RCL</span><span class="sxs-lookup"><span data-stu-id="a6e23-123">Referencing Razor Class Library content</span></span>

<span data-ttu-id="a6e23-124">La RCL peut être référencée par :</span><span class="sxs-lookup"><span data-stu-id="a6e23-124">The RCL can be referenced by:</span></span>

* <span data-ttu-id="a6e23-125">Un package NuGet.</span><span class="sxs-lookup"><span data-stu-id="a6e23-125">NuGet package.</span></span> <span data-ttu-id="a6e23-126">Consultez [Création de packages NuGet](/nuget/create-packages/creating-a-package), [dotnet add package](/dotnet/core/tools/dotnet-add-package) et [Créer et publier un package NuGet](/nuget/quickstart/create-and-publish-a-package-using-visual-studio).</span><span class="sxs-lookup"><span data-stu-id="a6e23-126">See [Creating NuGet packages](/nuget/create-packages/creating-a-package) and [dotnet add package](/dotnet/core/tools/dotnet-add-package) and [Create and publish a NuGet package](/nuget/quickstart/create-and-publish-a-package-using-visual-studio).</span></span>
* <span data-ttu-id="a6e23-127">Un fichier *{NomProjet}.csproj*.</span><span class="sxs-lookup"><span data-stu-id="a6e23-127">*{ProjectName}.csproj*.</span></span> <span data-ttu-id="a6e23-128">Consultez [dotnet-add reference](/dotnet/core/tools/dotnet-add-reference).</span><span class="sxs-lookup"><span data-stu-id="a6e23-128">See [dotnet-add reference](/dotnet/core/tools/dotnet-add-reference).</span></span>

### <a name="partial-files-access-in-the-rcl"></a><span data-ttu-id="a6e23-129">Accès aux fichiers partiels dans la RCL</span><span class="sxs-lookup"><span data-stu-id="a6e23-129">Partial files access in the RCL</span></span>

<span data-ttu-id="a6e23-130">Pour le contenu situé hors de la RCL, le runtime ASP.NET Core ne recherche pas de fichiers partiels dans la RCL.</span><span class="sxs-lookup"><span data-stu-id="a6e23-130">For content outside the RCL, the ASP.NET Core runtime does not search for partial files in the RCL.</span></span>

<span data-ttu-id="a6e23-131">Par exemple, dans l’exemple de téléchargement, la vue partielle *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* **ne peut pas** être référencée dans *WebApp1\Pages\About.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="a6e23-131">For example, in the sample download, the *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* partial view can **not** be referenced in *WebApp1\Pages\About.cshtml*.</span></span> <span data-ttu-id="a6e23-132">Toutefois, les pages dans la RCL *RazorUIClassLib/* **peuvent** accéder à *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="a6e23-132">However, pages in the RCL ( *RazorUIClassLib/* **can** access *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml*.</span></span>

## <a name="walkthrough-create-a-razor-class-library-project-and-use-from-a-razor-pages-project"></a><span data-ttu-id="a6e23-133">Procédure pas à pas : Créer un projet RCL et l’utiliser à partir d’un projet de pages Razor</span><span class="sxs-lookup"><span data-stu-id="a6e23-133">Walkthrough: Create a Razor Class Library project and use from a Razor Pages project</span></span>

<span data-ttu-id="a6e23-134">Vous pouvez télécharger et tester le [projet complet](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/razor-pages/ui-class/samples) au lieu de le créer de toutes pièces.</span><span class="sxs-lookup"><span data-stu-id="a6e23-134">You can download the [complete project](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/razor-pages/ui-class/samples) and test it rather than creating it.</span></span> <span data-ttu-id="a6e23-135">L’exemple proposé sous forme de téléchargement contient du code et des liens supplémentaires qui facilitent le test du projet.</span><span class="sxs-lookup"><span data-stu-id="a6e23-135">The sample download contains additional code and links that make the project easy to test.</span></span> <span data-ttu-id="a6e23-136">Si vous souhaitez commenter le mode d’obtention des exemples (téléchargement ou création au moyen d’instructions détaillées), entrez vos commentaires dans [ce problème GitHub](https://github.com/aspnet/Docs/issues/6098).</span><span class="sxs-lookup"><span data-stu-id="a6e23-136">You can leave feedback in [this GitHub issue](https://github.com/aspnet/Docs/issues/6098) with your comments on download samples versus step-by-step instructions.</span></span>

### <a name="test-the-download-app"></a><span data-ttu-id="a6e23-137">Tester l’application téléchargée</span><span class="sxs-lookup"><span data-stu-id="a6e23-137">Test the download app</span></span>

<span data-ttu-id="a6e23-138">Si vous n’avez pas téléchargé l’application complète et que vous souhaitez créer le projet pas à pas, passez à la [section suivante](#create-a-razor-class-library).</span><span class="sxs-lookup"><span data-stu-id="a6e23-138">If you haven't downloaded the completed app and would rather create the walkthrough project, skip to the [next section](#create-a-razor-class-library).</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="a6e23-139">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a6e23-139">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="a6e23-140">Ouvrez le fichier *.sln* dans Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="a6e23-140">Open the *.sln* file in Visual Studio.</span></span> <span data-ttu-id="a6e23-141">Exécutez l’application.</span><span class="sxs-lookup"><span data-stu-id="a6e23-141">Run the app.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="a6e23-142">CLI .NET Core</span><span class="sxs-lookup"><span data-stu-id="a6e23-142">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="a6e23-143">À partir d’une invite de commandes dans le répertoire *cli*, générez la RCL et l’application web.</span><span class="sxs-lookup"><span data-stu-id="a6e23-143">From a command prompt in the *cli* directory, build the RCL and web app.</span></span>

``` CLI
dotnet build
```

<span data-ttu-id="a6e23-144">Passez au répertoire *WebApp1* et exécutez l’application :</span><span class="sxs-lookup"><span data-stu-id="a6e23-144">Move to the *WebApp1* directory and run the app:</span></span>

``` CLI
dotnet run
```
------

<span data-ttu-id="a6e23-145">Suivez les instructions contenues dans [Test WebApp1](#test)</span><span class="sxs-lookup"><span data-stu-id="a6e23-145">Follow the instructions in [Test WebApp1](#test)</span></span>

## <a name="create-a-razor-class-library"></a><span data-ttu-id="a6e23-146">Créer une RCL</span><span class="sxs-lookup"><span data-stu-id="a6e23-146">Create a Razor Class Library</span></span>

<span data-ttu-id="a6e23-147">Dans cette section, vous allez créer une RCL.</span><span class="sxs-lookup"><span data-stu-id="a6e23-147">In this section, a Razor Class Library (RCL) is created.</span></span> <span data-ttu-id="a6e23-148">Des fichiers Razor sont ajoutés à la RCL.</span><span class="sxs-lookup"><span data-stu-id="a6e23-148">Razor files are added to the RCL.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="a6e23-149">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a6e23-149">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="a6e23-150">Créez le projet RCL :</span><span class="sxs-lookup"><span data-stu-id="a6e23-150">Create the RCL project:</span></span>

* <span data-ttu-id="a6e23-151">Dans Visual Studio, dans le menu **Fichier**, sélectionnez **Nouveau** > **Projet**.</span><span class="sxs-lookup"><span data-stu-id="a6e23-151">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="a6e23-152">Sélectionnez **Nouvelle application web ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="a6e23-152">Select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="a6e23-153">Nommez l’application **RazorUIClassLib**.</span><span class="sxs-lookup"><span data-stu-id="a6e23-153">Name the app **RazorUIClassLib**.</span></span>
* <span data-ttu-id="a6e23-154">Vérifiez que **ASP.NET Core 2.1** ou ultérieur est sélectionné.</span><span class="sxs-lookup"><span data-stu-id="a6e23-154">Verify **ASP.NET Core 2.1** or later is selected.</span></span>
* <span data-ttu-id="a6e23-155">Sélectionnez **Razor Class Library** > **OK**.</span><span class="sxs-lookup"><span data-stu-id="a6e23-155">Select **Razor Class Library** > **OK**.</span></span>

<span data-ttu-id="a6e23-156">Créez l’application web Razor Pages :</span><span class="sxs-lookup"><span data-stu-id="a6e23-156">Create the Razor Pages web app:</span></span>

* <span data-ttu-id="a6e23-157">Dans **l’Explorateur de solutions**, cliquez avec le bouton droit sur la solution > **Ajouter** >  **Nouveau projet**.</span><span class="sxs-lookup"><span data-stu-id="a6e23-157">From **Solution Explorer**, right-click the solution > **Add** >  **New Project**.</span></span>
* <span data-ttu-id="a6e23-158">Sélectionnez **Nouvelle application web ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="a6e23-158">Select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="a6e23-159">Nommez l’application **WebApp1**.</span><span class="sxs-lookup"><span data-stu-id="a6e23-159">Name the app **WebApp1**.</span></span>
* <span data-ttu-id="a6e23-160">Vérifiez que **ASP.NET Core 2.1** ou ultérieur est sélectionné.</span><span class="sxs-lookup"><span data-stu-id="a6e23-160">Verify **ASP.NET Core 2.1** or later is selected.</span></span>
* <span data-ttu-id="a6e23-161">Sélectionnez **Application web** > **OK**.</span><span class="sxs-lookup"><span data-stu-id="a6e23-161">Select **Web Application** > **OK**.</span></span>

### <a name="add-razor-files-and-folders-to-the-project"></a><span data-ttu-id="a6e23-162">Ajouter des fichiers et des dossiers Razor au projet.</span><span class="sxs-lookup"><span data-stu-id="a6e23-162">Add Razor files and folders to the project.</span></span>

* <span data-ttu-id="a6e23-163">Ajoutez un fichier de vue partielle Razor nommé *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="a6e23-163">Add a Razor partial view file named *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml*.</span></span>
* <span data-ttu-id="a6e23-164">Remplacez le balisage dans *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* par le code suivant :</span><span class="sxs-lookup"><span data-stu-id="a6e23-164">Replace the markup in *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* with the following code:</span></span>

[!code-html[Main](ui-class/samples/cli/RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml)]

* <span data-ttu-id="a6e23-165">Copiez le fichier *_ViewStart.cshtml* du projet WebApp1 dans *RazorUIClassLib/Areas/MyFeature/Pages/_ViewStart.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="a6e23-165">Copy the *_ViewStart.cshtml* file from the WebApp1 project to  *RazorUIClassLib/Areas/MyFeature/Pages/_ViewStart.cshtml*.</span></span>

  <span data-ttu-id="a6e23-166">Le fichier [viewstart](xref:mvc/views/layout#running-code-before-each-view) est nécessaire pour utiliser la disposition du projet Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="a6e23-166">The [viewstart](xref:mvc/views/layout#running-code-before-each-view) file is required to use the layout of the Razor Pages project.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="a6e23-167">CLI .NET Core</span><span class="sxs-lookup"><span data-stu-id="a6e23-167">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="a6e23-168">À partir de la ligne de commande, exécutez ce qui suit :</span><span class="sxs-lookup"><span data-stu-id="a6e23-168">From the command line, run the following:</span></span>

``` CLI
dotnet new razorclasslib -o RazorUIClassLib
dotnet new page -n _Message -np -o RazorUIClassLib/Areas/MyFeature/Pages/Shared
dotnet new viewstart -o RazorUIClassLib/Areas/MyFeature/Pages
```

<span data-ttu-id="a6e23-169">Les commandes précédentes :</span><span class="sxs-lookup"><span data-stu-id="a6e23-169">The preceding commands:</span></span>

* <span data-ttu-id="a6e23-170">Créent la RCL `RazorUIClassLib`.</span><span class="sxs-lookup"><span data-stu-id="a6e23-170">Creates the `RazorUIClassLib` Razor Class Library (RCL).</span></span>
* <span data-ttu-id="a6e23-171">Créent une page _Message Razor et l’ajoutent à la RCL.</span><span class="sxs-lookup"><span data-stu-id="a6e23-171">Creates a Razor _Message page, and adds it to the RCL.</span></span> <span data-ttu-id="a6e23-172">Le paramètre `-np` crée la page sans `PageModel`.</span><span class="sxs-lookup"><span data-stu-id="a6e23-172">The `-np` parameter creates the page without a `PageModel`.</span></span>
* <span data-ttu-id="a6e23-173">Créent un fichier [viewstart](xref:mvc/views/layout#running-code-before-each-view) et l’ajoutent à la RCL.</span><span class="sxs-lookup"><span data-stu-id="a6e23-173">Creates a [viewstart](xref:mvc/views/layout#running-code-before-each-view) file and adds it to the RCL.</span></span>

<span data-ttu-id="a6e23-174">Le fichier viewstart est nécessaire pour utiliser la disposition du projet de pages Razor (lequel est ajouté dans la section suivante).</span><span class="sxs-lookup"><span data-stu-id="a6e23-174">The viewstart file is required to use the layout of the Razor Pages project (which is added in the next section).</span></span>

<span data-ttu-id="a6e23-175">Mettez à jour les pages Razor :</span><span class="sxs-lookup"><span data-stu-id="a6e23-175">Update the Razor Pages:</span></span>

* <span data-ttu-id="a6e23-176">Remplacez le balisage dans *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* par le code suivant :</span><span class="sxs-lookup"><span data-stu-id="a6e23-176">Replace the markup in *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* with the following code:</span></span>

[!code-html[Main](ui-class/samples/cli/RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml)]

* <span data-ttu-id="a6e23-177">Remplacez le balisage dans *RazorUIClassLib/Areas/MyFeature/Pages/Page1.cshtml* par le code suivant :</span><span class="sxs-lookup"><span data-stu-id="a6e23-177">Replace the markup in *RazorUIClassLib/Areas/MyFeature/Pages/Page1.cshtml* with the following code:</span></span>

[!code-html[Main](ui-class/samples/cli/RazorUIClassLib/Areas/MyFeature/Pages/Page1.cshtml)]

<span data-ttu-id="a6e23-178">`@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers` est nécessaire pour utiliser la vue partielle (`<partial name="_Message" />`).</span><span class="sxs-lookup"><span data-stu-id="a6e23-178">`@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers` is required to use the partial view (`<partial name="_Message" />`).</span></span> <span data-ttu-id="a6e23-179">Au lieu d’inclure la directive `@addTagHelper`, vous pouvez ajouter un fichier *_ViewImports.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="a6e23-179">Rather than including the `@addTagHelper` directive, you can add a *_ViewImports.cshtml* file.</span></span> <span data-ttu-id="a6e23-180">Exemple :</span><span class="sxs-lookup"><span data-stu-id="a6e23-180">For example:</span></span>

``` CLI
dotnet new viewimports -o RazorUIClassLib/Areas/MyFeature/Pages
```

<span data-ttu-id="a6e23-181">Pour plus d’informations sur viewimports, consultez [Importation de directives partagées](xref:mvc/views/layout#importing-shared-directives).</span><span class="sxs-lookup"><span data-stu-id="a6e23-181">For more information on viewimports, see [Importing Shared Directives](xref:mvc/views/layout#importing-shared-directives)</span></span>

* <span data-ttu-id="a6e23-182">Générez la bibliothèque de classes pour vérifier l’absence d’erreurs de compilateur :</span><span class="sxs-lookup"><span data-stu-id="a6e23-182">Build the class library to verify there are no compiler errors:</span></span>

``` CLI
dotnet build RazorUIClassLib
```

<span data-ttu-id="a6e23-183">La sortie de build contient *RazorUIClassLib.dll* et *RazorUIClassLib.Views.dll*.</span><span class="sxs-lookup"><span data-stu-id="a6e23-183">The build output contains *RazorUIClassLib.dll* and *RazorUIClassLib.Views.dll*.</span></span> <span data-ttu-id="a6e23-184">*RazorUIClassLib.Views.dll* contient le contenu Razor compilé.</span><span class="sxs-lookup"><span data-stu-id="a6e23-184">*RazorUIClassLib.Views.dll* contains the compiled Razor content.</span></span>

------

### <a name="use-the-razor-ui-library-from-a-razor-pages-project"></a><span data-ttu-id="a6e23-185">Utiliser la bibliothèque de l’interface utilisateur Razor à partir d’un projet Razor Pages</span><span class="sxs-lookup"><span data-stu-id="a6e23-185">Use the Razor UI library from a Razor Pages project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="a6e23-186">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a6e23-186">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="a6e23-187">Dans **l’Explorateur de solutions**, cliquez avec le bouton droit sur **WebApp1**, puis sélectionnez **Définir comme projet de démarrage**.</span><span class="sxs-lookup"><span data-stu-id="a6e23-187">From **Solution Explorer**, right-click on **WebApp1** and select **Set as StartUp Project**.</span></span>
* <span data-ttu-id="a6e23-188">Dans **l’Explorateur de solutions**, cliquez avec le bouton droit sur **WebApp1**, puis sélectionnez **Dépendances de build** > **Dépendances du projet**.</span><span class="sxs-lookup"><span data-stu-id="a6e23-188">From **Solution Explorer**, right-click on **WebApp1** and select **Build Dependencies** > **Project Dependencies**.</span></span>
* <span data-ttu-id="a6e23-189">Cochez **RazorUIClassLib** comme dépendance de **WebApp1**.</span><span class="sxs-lookup"><span data-stu-id="a6e23-189">Check **RazorUIClassLib** as a dependency of **WebApp1**.</span></span>
* <span data-ttu-id="a6e23-190">Dans **l’Explorateur de solutions**, cliquez avec le bouton droit sur **WebApp1**, puis sélectionnez **Ajouter** > **Référence**.</span><span class="sxs-lookup"><span data-stu-id="a6e23-190">From **Solution Explorer**, right-click on **WebApp1** and select **Add** > **Reference**.</span></span>
* <span data-ttu-id="a6e23-191">Dans la boîte de dialogue **Gestionnaire de références**, cochez **RazorUIClassLib** > **OK**.</span><span class="sxs-lookup"><span data-stu-id="a6e23-191">In the **Reference Manager** dialog, check **RazorUIClassLib** > **OK**.</span></span>

<span data-ttu-id="a6e23-192">Exécutez l’application.</span><span class="sxs-lookup"><span data-stu-id="a6e23-192">Run the app.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="a6e23-193">CLI .NET Core</span><span class="sxs-lookup"><span data-stu-id="a6e23-193">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="a6e23-194">Créez une application web Razor Pages et un fichier de solution contenant l’application Razor Pages et la RCL :</span><span class="sxs-lookup"><span data-stu-id="a6e23-194">Create a Razor Pages web app and a solution file containing the Razor Pages app and the Razor Class Library:</span></span>

``` CLI
dotnet new razor -o WebApp1
dotnet new sln
dotnet sln add WebApp1
dotnet sln add RazorUIClassLib
dotnet add WebApp1 reference RazorUIClassLib
```

<span data-ttu-id="a6e23-195">Générez et exécutez l’application web :</span><span class="sxs-lookup"><span data-stu-id="a6e23-195">Build and run the web app:</span></span>

``` CLI
cd WebApp1
dotnet run
```

------

<a name="test"></a>

### <a name="test-webapp1"></a><span data-ttu-id="a6e23-196">Tester WebApp1</span><span class="sxs-lookup"><span data-stu-id="a6e23-196">Test WebApp1</span></span>

<span data-ttu-id="a6e23-197">Vérifiez que la bibliothèque de classes de l’interface utilisateur Razor est utilisée.</span><span class="sxs-lookup"><span data-stu-id="a6e23-197">Verify the Razor UI class library is being used.</span></span>

* <span data-ttu-id="a6e23-198">Accédez à `/MyFeature/Page1`.</span><span class="sxs-lookup"><span data-stu-id="a6e23-198">Browse to `/MyFeature/Page1`.</span></span>

## <a name="override-views-partial-views-and-pages"></a><span data-ttu-id="a6e23-199">Substituer des vues, des vues partielles et des pages</span><span class="sxs-lookup"><span data-stu-id="a6e23-199">Override views, partial views, and pages</span></span>

<span data-ttu-id="a6e23-200">Quand une vue, une vue partielle ou une page Razor est présente dans l’application web et la RCL, le balisage Razor (fichier *.cshtml*) dans l’application web est prioritaire.</span><span class="sxs-lookup"><span data-stu-id="a6e23-200">When a view, partial view, or Razor Page is found in both the web app and the Razor Class Library, the Razor markup (*.cshtml* file) in the web app takes precedence.</span></span> <span data-ttu-id="a6e23-201">Par exemple, si vous ajoutez *WebApp1/Areas/MyFeature/Pages/Page1.cshtml* à WebApp1, Page1 dans WebApp1 l’emporte sur Page1 dans la RCL.</span><span class="sxs-lookup"><span data-stu-id="a6e23-201">For example, add *WebApp1/Areas/MyFeature/Pages/Page1.cshtml* to WebApp1, and Page1 in the WebApp1 will take precedence over Page1in the Razor Class Library.</span></span>

<span data-ttu-id="a6e23-202">Dans l’exemple proposé sous forme de téléchargement, renommez *WebApp1/Areas/MyFeature2* en *WebApp1/Areas/MyFeature* pour tester la priorité.</span><span class="sxs-lookup"><span data-stu-id="a6e23-202">In the sample download, rename *WebApp1/Areas/MyFeature2* to *WebApp1/Areas/MyFeature* to test precedence.</span></span>

<span data-ttu-id="a6e23-203">Copiez la vue partielle *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* dans *WebApp1/Areas/MyFeature/Pages/Shared/_Message.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="a6e23-203">Copy the *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* partial view to *WebApp1/Areas/MyFeature/Pages/Shared/_Message.cshtml*.</span></span> <span data-ttu-id="a6e23-204">Mettez à jour le balisage pour indiquer le nouvel emplacement.</span><span class="sxs-lookup"><span data-stu-id="a6e23-204">Update the markup to indicate the new location.</span></span> <span data-ttu-id="a6e23-205">Générez et exécutez l’application pour vérifier que la version de la vue partielle de l’application est utilisée.</span><span class="sxs-lookup"><span data-stu-id="a6e23-205">Build and run the app to verify the app's version of the partial is being used.</span></span>
