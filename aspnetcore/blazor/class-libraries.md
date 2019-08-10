---
title: Bibliothèques de classes des composants Razor ASP.NET Core
author: guardrex
description: Découvrez comment les composants peuvent être inclus dans des applications éblouissantes à partir d’une bibliothèque de composants externes.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 07/02/2019
uid: blazor/class-libraries
ms.openlocfilehash: 402b7b072554f63f85e7cf5e55336104d235a071
ms.sourcegitcommit: 776367717e990bdd600cb3c9148ffb905d56862d
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/09/2019
ms.locfileid: "68948439"
---
# <a name="aspnet-core-razor-components-class-libraries"></a><span data-ttu-id="6ab07-103">Bibliothèques de classes des composants Razor ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="6ab07-103">ASP.NET Core Razor components class libraries</span></span>

<span data-ttu-id="6ab07-104">Par [Simon Timms](https://github.com/stimms)</span><span class="sxs-lookup"><span data-stu-id="6ab07-104">By [Simon Timms](https://github.com/stimms)</span></span>

<span data-ttu-id="6ab07-105">Les composants peuvent être partagés dans une [bibliothèque de classes Razor (RCL)](xref:razor-pages/ui-class) entre les projets.</span><span class="sxs-lookup"><span data-stu-id="6ab07-105">Components can be shared in a [Razor class library (RCL)](xref:razor-pages/ui-class) across projects.</span></span> <span data-ttu-id="6ab07-106">Une *bibliothèque de classes de composants Razor* peut être incluse à partir de:</span><span class="sxs-lookup"><span data-stu-id="6ab07-106">A *Razor components class library* can be included from:</span></span>

* <span data-ttu-id="6ab07-107">Un autre projet dans la solution.</span><span class="sxs-lookup"><span data-stu-id="6ab07-107">Another project in the solution.</span></span>
* <span data-ttu-id="6ab07-108">Package NuGet.</span><span class="sxs-lookup"><span data-stu-id="6ab07-108">A NuGet package.</span></span>
* <span data-ttu-id="6ab07-109">Bibliothèque .NET référencée.</span><span class="sxs-lookup"><span data-stu-id="6ab07-109">A referenced .NET library.</span></span>

<span data-ttu-id="6ab07-110">Tout comme les composants sont des types .NET standard, les composants fournis par un RCL sont des assemblys .NET normaux.</span><span class="sxs-lookup"><span data-stu-id="6ab07-110">Just as components are regular .NET types, components provided by an RCL are normal .NET assemblies.</span></span>

## <a name="create-an-rcl"></a><span data-ttu-id="6ab07-111">Créer un RCL</span><span class="sxs-lookup"><span data-stu-id="6ab07-111">Create an RCL</span></span>

<span data-ttu-id="6ab07-112">Suivez les instructions de l' <xref:blazor/get-started> article pour configurer votre environnement pour éblouissant.</span><span class="sxs-lookup"><span data-stu-id="6ab07-112">Follow the guidance in the <xref:blazor/get-started> article to configure your environment for Blazor.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="6ab07-113">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6ab07-113">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="6ab07-114">Créer un nouveau projet.</span><span class="sxs-lookup"><span data-stu-id="6ab07-114">Create a new project.</span></span>
1. <span data-ttu-id="6ab07-115">Sélectionnez **Nouvelle application web ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="6ab07-115">Select **ASP.NET Core Web Application**.</span></span> <span data-ttu-id="6ab07-116">Sélectionnez **Suivant**.</span><span class="sxs-lookup"><span data-stu-id="6ab07-116">Select **Next**.</span></span>
1. <span data-ttu-id="6ab07-117">Indiquez un nom de projet dans le champ **Nom du projet**, ou acceptez le nom de projet par défaut.</span><span class="sxs-lookup"><span data-stu-id="6ab07-117">Provide a project name in the **Project name** field or accept the default project name.</span></span> <span data-ttu-id="6ab07-118">Les exemples de cette rubrique utilisent le nom `MyComponentLib1`du projet.</span><span class="sxs-lookup"><span data-stu-id="6ab07-118">The examples in this topic use the project name `MyComponentLib1`.</span></span> <span data-ttu-id="6ab07-119">Sélectionnez **Créer**.</span><span class="sxs-lookup"><span data-stu-id="6ab07-119">Select **Create**.</span></span>
1. <span data-ttu-id="6ab07-120">Dans la boîte de dialogue **Créer une application web ASP.NET Core**, vérifiez que **.NET Core** et **ASP.NET Core 3.0** sont sélectionnés.</span><span class="sxs-lookup"><span data-stu-id="6ab07-120">In the **Create a new ASP.NET Core Web Application** dialog, confirm that **.NET Core** and **ASP.NET Core 3.0** are selected.</span></span>
1. <span data-ttu-id="6ab07-121">Sélectionnez le modèle **bibliothèque de classes Razor** .</span><span class="sxs-lookup"><span data-stu-id="6ab07-121">Select the **Razor Class Library** template.</span></span> <span data-ttu-id="6ab07-122">Sélectionnez **Créer**.</span><span class="sxs-lookup"><span data-stu-id="6ab07-122">Select **Create**.</span></span>
1. <span data-ttu-id="6ab07-123">Ajouter RCL à une solution:</span><span class="sxs-lookup"><span data-stu-id="6ab07-123">Add the RCL to a solution:</span></span>
   1. <span data-ttu-id="6ab07-124">Cliquez avec le bouton droit sur la solution.</span><span class="sxs-lookup"><span data-stu-id="6ab07-124">Right-click the solution.</span></span> <span data-ttu-id="6ab07-125">Sélectionnez **Ajouter** > un**projet existant**.</span><span class="sxs-lookup"><span data-stu-id="6ab07-125">Select **Add** > **Existing Project**.</span></span>
   1. <span data-ttu-id="6ab07-126">Accédez au fichier projet de RCL.</span><span class="sxs-lookup"><span data-stu-id="6ab07-126">Navigate to the RCL's project file.</span></span>
   1. <span data-ttu-id="6ab07-127">Sélectionnez le fichier projet de RCL ( *. csproj*).</span><span class="sxs-lookup"><span data-stu-id="6ab07-127">Select the RCL's project file (*.csproj*).</span></span>
1. <span data-ttu-id="6ab07-128">Ajoutez une référence à l’RCL à partir de l’application:</span><span class="sxs-lookup"><span data-stu-id="6ab07-128">Add a reference the RCL from the app:</span></span>
   1. <span data-ttu-id="6ab07-129">Cliquez avec le bouton droit sur le projet d’application.</span><span class="sxs-lookup"><span data-stu-id="6ab07-129">Right-click the app project.</span></span> <span data-ttu-id="6ab07-130">Sélectionnez **Ajouter** > une**référence**.</span><span class="sxs-lookup"><span data-stu-id="6ab07-130">Select **Add** > **Reference**.</span></span>
   1. <span data-ttu-id="6ab07-131">Sélectionnez le projet RCL.</span><span class="sxs-lookup"><span data-stu-id="6ab07-131">Select the RCL project.</span></span> <span data-ttu-id="6ab07-132">Sélectionnez **OK**.</span><span class="sxs-lookup"><span data-stu-id="6ab07-132">Select **OK**.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="6ab07-133">CLI .NET Core</span><span class="sxs-lookup"><span data-stu-id="6ab07-133">.NET Core CLI</span></span>](#tab/netcore-cli)

1. <span data-ttu-id="6ab07-134">Utilisez le modèle de **bibliothèque de classes Razor** (`razorclasslib`) avec la commande [dotnet New](/dotnet/core/tools/dotnet-new) dans un interpréteur de commandes.</span><span class="sxs-lookup"><span data-stu-id="6ab07-134">Use the **Razor Class Library** template (`razorclasslib`) with the [dotnet new](/dotnet/core/tools/dotnet-new) command in a command shell.</span></span> <span data-ttu-id="6ab07-135">Dans l’exemple suivant, un RCL est créé nommé `MyComponentLib1`.</span><span class="sxs-lookup"><span data-stu-id="6ab07-135">In the following example, an RCL is created named `MyComponentLib1`.</span></span> <span data-ttu-id="6ab07-136">Le dossier qui contient `MyComponentLib1` est créé automatiquement lors de l’exécution de la commande:</span><span class="sxs-lookup"><span data-stu-id="6ab07-136">The folder that holds `MyComponentLib1` is created automatically when the command is executed:</span></span>

   ```console
   dotnet new razorclasslib -o MyComponentLib1
   ```

1. <span data-ttu-id="6ab07-137">Pour ajouter la bibliothèque à un projet existant, utilisez la commande [dotnet Add Reference](/dotnet/core/tools/dotnet-add-reference) dans un interpréteur de commandes.</span><span class="sxs-lookup"><span data-stu-id="6ab07-137">To add the library to an existing project, use the [dotnet add reference](/dotnet/core/tools/dotnet-add-reference) command in a command shell.</span></span> <span data-ttu-id="6ab07-138">Dans l’exemple suivant, le RCL est ajouté à l’application.</span><span class="sxs-lookup"><span data-stu-id="6ab07-138">In the following example, the RCL is added to the app.</span></span> <span data-ttu-id="6ab07-139">Exécutez la commande suivante à partir du dossier du projet de l’application avec le chemin d’accès à la bibliothèque:</span><span class="sxs-lookup"><span data-stu-id="6ab07-139">Execute the following command from the app's project folder with the path to the library:</span></span>

   ```console
   dotnet add reference {PATH TO LIBRARY}
   ```

---

## <a name="rcls-not-supported-for-client-side-apps"></a><span data-ttu-id="6ab07-140">RCLs non pris en charge pour les applications côté client</span><span class="sxs-lookup"><span data-stu-id="6ab07-140">RCLs not supported for client-side apps</span></span>

<span data-ttu-id="6ab07-141">Dans la version préliminaire actuelle ASP.NET Core 3,0, les bibliothèques de classes Razor ne sont pas compatibles avec les applications côté client éblouissantes.</span><span class="sxs-lookup"><span data-stu-id="6ab07-141">In the current ASP.NET Core 3.0 Preview, Razor class libraries aren't compatible with Blazor client-side apps.</span></span> <span data-ttu-id="6ab07-142">Pour les applications côté client éblouissantes, utilisez une bibliothèque de composants éblouissante créée par `blazorlib` le modèle dans une interface de commande:</span><span class="sxs-lookup"><span data-stu-id="6ab07-142">For Blazor client-side apps, use a Blazor component library created by the `blazorlib` template in a command shell:</span></span>

```console
dotnet new blazorlib -o MyComponentLib1
```

<span data-ttu-id="6ab07-143">Les bibliothèques de composants `blazorlib` qui utilisent le modèle peuvent inclure des fichiers statiques, tels que des images, du JavaScript et des feuilles de style.</span><span class="sxs-lookup"><span data-stu-id="6ab07-143">Component libraries using the `blazorlib` template can include static files, such as images, JavaScript, and stylesheets.</span></span> <span data-ttu-id="6ab07-144">Au moment de la génération, les fichiers statiques sont incorporés dans le fichier d’assembly ( *. dll*) généré, ce qui permet la consommation des composants sans avoir à se soucier de l’inclusion de leurs ressources.</span><span class="sxs-lookup"><span data-stu-id="6ab07-144">At build time, static files are embedded into the built assembly file (*.dll*), which allows consumption of the components without having to worry about how to include their resources.</span></span> <span data-ttu-id="6ab07-145">Tous les fichiers inclus dans `content` le répertoire sont marqués comme ressource incorporée.</span><span class="sxs-lookup"><span data-stu-id="6ab07-145">Any files included in the `content` directory are marked as an embedded resource.</span></span>

## <a name="consume-a-library-component"></a><span data-ttu-id="6ab07-146">Consommer un composant de bibliothèque</span><span class="sxs-lookup"><span data-stu-id="6ab07-146">Consume a library component</span></span>

<span data-ttu-id="6ab07-147">Pour utiliser des composants définis dans une bibliothèque dans un autre projet, utilisez l’une des approches suivantes:</span><span class="sxs-lookup"><span data-stu-id="6ab07-147">In order to consume components defined in a library in another project, use either of the following approaches:</span></span>

* <span data-ttu-id="6ab07-148">Utilisez le nom de type complet avec l’espace de noms.</span><span class="sxs-lookup"><span data-stu-id="6ab07-148">Use the full type name with the namespace.</span></span>
* <span data-ttu-id="6ab07-149">Utilisez la directive [ \@using](xref:mvc/views/razor#using) de Razor.</span><span class="sxs-lookup"><span data-stu-id="6ab07-149">Use Razor's [\@using](xref:mvc/views/razor#using) directive.</span></span> <span data-ttu-id="6ab07-150">Des composants individuels peuvent être ajoutés par nom.</span><span class="sxs-lookup"><span data-stu-id="6ab07-150">Individual components can be added by name.</span></span>

<span data-ttu-id="6ab07-151">Dans les exemples suivants, `MyComponentLib1` est une bibliothèque de composants contenant `SalesReport` un composant.</span><span class="sxs-lookup"><span data-stu-id="6ab07-151">In the following examples, `MyComponentLib1` is a component library containing a `SalesReport` component.</span></span>

<span data-ttu-id="6ab07-152">Le `SalesReport` composant peut être référencé à l’aide de son nom de type complet avec l’espace de noms:</span><span class="sxs-lookup"><span data-stu-id="6ab07-152">The `SalesReport` component can be referenced using its full type name with namespace:</span></span>

```cshtml
<h1>Hello, world!</h1>

Welcome to your new app.

<MyComponentLib1.SalesReport />
```

<span data-ttu-id="6ab07-153">Le composant peut également être référencé si la bibliothèque est placée dans la portée avec une `@using` directive:</span><span class="sxs-lookup"><span data-stu-id="6ab07-153">The component can also be referenced if the library is brought into scope with an `@using` directive:</span></span>

```cshtml
@using MyComponentLib1

<h1>Hello, world!</h1>

Welcome to your new app.

<SalesReport />
```

<span data-ttu-id="6ab07-154">Incluez `@using MyComponentLib1` la directive dans le fichier *_Import. Razor* de niveau supérieur pour mettre les composants de la bibliothèque à la disposition d’un projet entier.</span><span class="sxs-lookup"><span data-stu-id="6ab07-154">Include the `@using MyComponentLib1` directive in the top-level *_Import.razor* file to make the library's components available to an entire project.</span></span> <span data-ttu-id="6ab07-155">Ajoutez la directive à un fichier *_Import. Razor* à n’importe quel niveau pour appliquer l’espace de noms à une seule page ou à un ensemble de pages dans un dossier.</span><span class="sxs-lookup"><span data-stu-id="6ab07-155">Add the directive to an *_Import.razor* file at any level to apply the namespace to a single page or set of pages within a folder.</span></span>

## <a name="build-pack-and-ship-to-nuget"></a><span data-ttu-id="6ab07-156">Générer, empaqueter et envoyer à NuGet</span><span class="sxs-lookup"><span data-stu-id="6ab07-156">Build, pack, and ship to NuGet</span></span>

<span data-ttu-id="6ab07-157">Étant donné que les bibliothèques de composants sont des bibliothèques .NET standard, leur empaquetage et leur envoi à NuGet ne sont pas différents de l’empaquetage et de l’expédition d’une bibliothèque à NuGet.</span><span class="sxs-lookup"><span data-stu-id="6ab07-157">Because component libraries are standard .NET libraries, packaging and shipping them to NuGet is no different from packaging and shipping any library to NuGet.</span></span> <span data-ttu-id="6ab07-158">L’empaquetage est effectué à l’aide de la commande [dotnet Pack](/dotnet/core/tools/dotnet-pack) dans une interface de commande:</span><span class="sxs-lookup"><span data-stu-id="6ab07-158">Packaging is performed using the [dotnet pack](/dotnet/core/tools/dotnet-pack) command in a command shell:</span></span>

```console
dotnet pack
```

<span data-ttu-id="6ab07-159">Téléchargez le package dans NuGet à l’aide de la commande [dotnet NuGet Publish](/dotnet/core/tools/dotnet-nuget-push) dans une interface de commande:</span><span class="sxs-lookup"><span data-stu-id="6ab07-159">Upload the package to NuGet using the [dotnet nuget publish](/dotnet/core/tools/dotnet-nuget-push) command in a command shell:</span></span>

```console
dotnet nuget publish
```

<span data-ttu-id="6ab07-160">Lorsque vous utilisez `blazorlib` le modèle, les ressources statiques sont incluses dans le package NuGet.</span><span class="sxs-lookup"><span data-stu-id="6ab07-160">When using the `blazorlib` template, static resources are included in the NuGet package.</span></span> <span data-ttu-id="6ab07-161">Les consommateurs de bibliothèques reçoivent automatiquement des scripts et des feuilles de style, si bien que les consommateurs ne sont pas tenus d’installer manuellement les ressources.</span><span class="sxs-lookup"><span data-stu-id="6ab07-161">Library consumers automatically receive scripts and stylesheets, so consumers aren't required to manually install the resources.</span></span>

## <a name="create-a-razor-components-class-library-with-static-assets"></a><span data-ttu-id="6ab07-162">Créer une bibliothèque de classes de composants Razor avec des ressources statiques</span><span class="sxs-lookup"><span data-stu-id="6ab07-162">Create a Razor components class library with static assets</span></span>

<span data-ttu-id="6ab07-163">Un RCL peut inclure des ressources statiques.</span><span class="sxs-lookup"><span data-stu-id="6ab07-163">An RCL can include static assets.</span></span> <span data-ttu-id="6ab07-164">Les ressources statiques sont disponibles pour toutes les applications qui consomment la bibliothèque.</span><span class="sxs-lookup"><span data-stu-id="6ab07-164">The static assets are available to any app that consumes the library.</span></span> <span data-ttu-id="6ab07-165">Pour plus d'informations, consultez <xref:razor-pages/ui-class#create-an-rcl-with-static-assets>.</span><span class="sxs-lookup"><span data-stu-id="6ab07-165">For more information, see <xref:razor-pages/ui-class#create-an-rcl-with-static-assets>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="6ab07-166">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="6ab07-166">Additional resources</span></span>

* <xref:razor-pages/ui-class>
