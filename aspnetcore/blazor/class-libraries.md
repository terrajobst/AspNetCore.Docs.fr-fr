---
title: Bibliothèques de classes des composants Razor ASP.NET Core
author: guardrex
description: Découvrez comment les composants peuvent être inclus dans des applications éblouissantes à partir d’une bibliothèque de composants externes.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 08/13/2019
uid: blazor/class-libraries
ms.openlocfilehash: 91c4ffb29649c162fc15e5cb70dc60144b53afea
ms.sourcegitcommit: 215954a638d24124f791024c66fd4fb9109fd380
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/18/2019
ms.locfileid: "71080662"
---
# <a name="aspnet-core-razor-components-class-libraries"></a><span data-ttu-id="2515b-103">Bibliothèques de classes des composants Razor ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="2515b-103">ASP.NET Core Razor components class libraries</span></span>

<span data-ttu-id="2515b-104">Par [Simon Timms](https://github.com/stimms)</span><span class="sxs-lookup"><span data-stu-id="2515b-104">By [Simon Timms](https://github.com/stimms)</span></span>

<span data-ttu-id="2515b-105">Les composants peuvent être partagés dans une [bibliothèque de classes Razor (RCL)](xref:razor-pages/ui-class) entre les projets.</span><span class="sxs-lookup"><span data-stu-id="2515b-105">Components can be shared in a [Razor class library (RCL)](xref:razor-pages/ui-class) across projects.</span></span> <span data-ttu-id="2515b-106">Une *bibliothèque de classes de composants Razor* peut être incluse à partir de :</span><span class="sxs-lookup"><span data-stu-id="2515b-106">A *Razor components class library* can be included from:</span></span>

* <span data-ttu-id="2515b-107">Un autre projet dans la solution.</span><span class="sxs-lookup"><span data-stu-id="2515b-107">Another project in the solution.</span></span>
* <span data-ttu-id="2515b-108">Package NuGet.</span><span class="sxs-lookup"><span data-stu-id="2515b-108">A NuGet package.</span></span>
* <span data-ttu-id="2515b-109">Bibliothèque .NET référencée.</span><span class="sxs-lookup"><span data-stu-id="2515b-109">A referenced .NET library.</span></span>

<span data-ttu-id="2515b-110">Tout comme les composants sont des types .NET standard, les composants fournis par un RCL sont des assemblys .NET normaux.</span><span class="sxs-lookup"><span data-stu-id="2515b-110">Just as components are regular .NET types, components provided by an RCL are normal .NET assemblies.</span></span>

## <a name="create-an-rcl"></a><span data-ttu-id="2515b-111">Créer un RCL</span><span class="sxs-lookup"><span data-stu-id="2515b-111">Create an RCL</span></span>

<span data-ttu-id="2515b-112">Suivez les instructions de l' <xref:blazor/get-started> article pour configurer votre environnement pour éblouissant.</span><span class="sxs-lookup"><span data-stu-id="2515b-112">Follow the guidance in the <xref:blazor/get-started> article to configure your environment for Blazor.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="2515b-113">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="2515b-113">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="2515b-114">Créer un nouveau projet.</span><span class="sxs-lookup"><span data-stu-id="2515b-114">Create a new project.</span></span>
1. <span data-ttu-id="2515b-115">Sélectionnez **bibliothèque de classes Razor**.</span><span class="sxs-lookup"><span data-stu-id="2515b-115">Select **Razor Class Library**.</span></span> <span data-ttu-id="2515b-116">Sélectionnez **Suivant**.</span><span class="sxs-lookup"><span data-stu-id="2515b-116">Select **Next**.</span></span>
1. <span data-ttu-id="2515b-117">Dans la boîte de dialogue **créer une nouvelle bibliothèque de classes Razor** , sélectionnez **créer**.</span><span class="sxs-lookup"><span data-stu-id="2515b-117">In the **Create a new Razor class library** dialog, select **Create**.</span></span>
1. <span data-ttu-id="2515b-118">Indiquez un nom de projet dans le champ **Nom du projet**, ou acceptez le nom de projet par défaut.</span><span class="sxs-lookup"><span data-stu-id="2515b-118">Provide a project name in the **Project name** field or accept the default project name.</span></span> <span data-ttu-id="2515b-119">Les exemples de cette rubrique utilisent le nom `MyComponentLib1`du projet.</span><span class="sxs-lookup"><span data-stu-id="2515b-119">The examples in this topic use the project name `MyComponentLib1`.</span></span> <span data-ttu-id="2515b-120">Sélectionnez **Créer**.</span><span class="sxs-lookup"><span data-stu-id="2515b-120">Select **Create**.</span></span>
1. <span data-ttu-id="2515b-121">Ajouter RCL à une solution :</span><span class="sxs-lookup"><span data-stu-id="2515b-121">Add the RCL to a solution:</span></span>
   1. <span data-ttu-id="2515b-122">Cliquez avec le bouton droit sur la solution.</span><span class="sxs-lookup"><span data-stu-id="2515b-122">Right-click the solution.</span></span> <span data-ttu-id="2515b-123">Sélectionnez **Ajouter** > un**projet existant**.</span><span class="sxs-lookup"><span data-stu-id="2515b-123">Select **Add** > **Existing Project**.</span></span>
   1. <span data-ttu-id="2515b-124">Accédez au fichier projet de RCL.</span><span class="sxs-lookup"><span data-stu-id="2515b-124">Navigate to the RCL's project file.</span></span>
   1. <span data-ttu-id="2515b-125">Sélectionnez le fichier projet de RCL ( *. csproj*).</span><span class="sxs-lookup"><span data-stu-id="2515b-125">Select the RCL's project file (*.csproj*).</span></span>
1. <span data-ttu-id="2515b-126">Ajoutez une référence à l’RCL à partir de l’application :</span><span class="sxs-lookup"><span data-stu-id="2515b-126">Add a reference the RCL from the app:</span></span>
   1. <span data-ttu-id="2515b-127">Cliquez avec le bouton droit sur le projet d’application.</span><span class="sxs-lookup"><span data-stu-id="2515b-127">Right-click the app project.</span></span> <span data-ttu-id="2515b-128">Sélectionnez **Ajouter** > une**référence**.</span><span class="sxs-lookup"><span data-stu-id="2515b-128">Select **Add** > **Reference**.</span></span>
   1. <span data-ttu-id="2515b-129">Sélectionnez le projet RCL.</span><span class="sxs-lookup"><span data-stu-id="2515b-129">Select the RCL project.</span></span> <span data-ttu-id="2515b-130">Sélectionnez **OK**.</span><span class="sxs-lookup"><span data-stu-id="2515b-130">Select **OK**.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="2515b-131">CLI .NET Core</span><span class="sxs-lookup"><span data-stu-id="2515b-131">.NET Core CLI</span></span>](#tab/netcore-cli)

1. <span data-ttu-id="2515b-132">Utilisez le modèle de **bibliothèque de classes Razor** (`razorclasslib`) avec la commande [dotnet New](/dotnet/core/tools/dotnet-new) dans un interpréteur de commandes.</span><span class="sxs-lookup"><span data-stu-id="2515b-132">Use the **Razor Class Library** template (`razorclasslib`) with the [dotnet new](/dotnet/core/tools/dotnet-new) command in a command shell.</span></span> <span data-ttu-id="2515b-133">Dans l’exemple suivant, un RCL est créé nommé `MyComponentLib1`.</span><span class="sxs-lookup"><span data-stu-id="2515b-133">In the following example, an RCL is created named `MyComponentLib1`.</span></span> <span data-ttu-id="2515b-134">Le dossier qui contient `MyComponentLib1` est créé automatiquement lors de l’exécution de la commande :</span><span class="sxs-lookup"><span data-stu-id="2515b-134">The folder that holds `MyComponentLib1` is created automatically when the command is executed:</span></span>

   ```dotnetcli
   dotnet new razorclasslib -o MyComponentLib1
   ```

1. <span data-ttu-id="2515b-135">Pour ajouter la bibliothèque à un projet existant, utilisez la commande [dotnet Add Reference](/dotnet/core/tools/dotnet-add-reference) dans un interpréteur de commandes.</span><span class="sxs-lookup"><span data-stu-id="2515b-135">To add the library to an existing project, use the [dotnet add reference](/dotnet/core/tools/dotnet-add-reference) command in a command shell.</span></span> <span data-ttu-id="2515b-136">Dans l’exemple suivant, le RCL est ajouté à l’application.</span><span class="sxs-lookup"><span data-stu-id="2515b-136">In the following example, the RCL is added to the app.</span></span> <span data-ttu-id="2515b-137">Exécutez la commande suivante à partir du dossier du projet de l’application avec le chemin d’accès à la bibliothèque :</span><span class="sxs-lookup"><span data-stu-id="2515b-137">Execute the following command from the app's project folder with the path to the library:</span></span>

   ```dotnetcli
   dotnet add reference {PATH TO LIBRARY}
   ```

---

## <a name="consume-a-library-component"></a><span data-ttu-id="2515b-138">Consommer un composant de bibliothèque</span><span class="sxs-lookup"><span data-stu-id="2515b-138">Consume a library component</span></span>

<span data-ttu-id="2515b-139">Pour utiliser des composants définis dans une bibliothèque dans un autre projet, utilisez l’une des approches suivantes :</span><span class="sxs-lookup"><span data-stu-id="2515b-139">In order to consume components defined in a library in another project, use either of the following approaches:</span></span>

* <span data-ttu-id="2515b-140">Utilisez le nom de type complet avec l’espace de noms.</span><span class="sxs-lookup"><span data-stu-id="2515b-140">Use the full type name with the namespace.</span></span>
* <span data-ttu-id="2515b-141">Utilisez la directive [ \@using](xref:mvc/views/razor#using) de Razor.</span><span class="sxs-lookup"><span data-stu-id="2515b-141">Use Razor's [\@using](xref:mvc/views/razor#using) directive.</span></span> <span data-ttu-id="2515b-142">Des composants individuels peuvent être ajoutés par nom.</span><span class="sxs-lookup"><span data-stu-id="2515b-142">Individual components can be added by name.</span></span>

<span data-ttu-id="2515b-143">Dans les exemples suivants, `MyComponentLib1` est une bibliothèque de composants contenant `SalesReport` un composant.</span><span class="sxs-lookup"><span data-stu-id="2515b-143">In the following examples, `MyComponentLib1` is a component library containing a `SalesReport` component.</span></span>

<span data-ttu-id="2515b-144">Le `SalesReport` composant peut être référencé à l’aide de son nom de type complet avec l’espace de noms :</span><span class="sxs-lookup"><span data-stu-id="2515b-144">The `SalesReport` component can be referenced using its full type name with namespace:</span></span>

```cshtml
<h1>Hello, world!</h1>

Welcome to your new app.

<MyComponentLib1.SalesReport />
```

<span data-ttu-id="2515b-145">Le composant peut également être référencé si la bibliothèque est placée dans la portée avec une `@using` directive :</span><span class="sxs-lookup"><span data-stu-id="2515b-145">The component can also be referenced if the library is brought into scope with an `@using` directive:</span></span>

```cshtml
@using MyComponentLib1

<h1>Hello, world!</h1>

Welcome to your new app.

<SalesReport />
```

<span data-ttu-id="2515b-146">Incluez `@using MyComponentLib1` la directive dans le fichier *_Import. Razor* de niveau supérieur pour mettre les composants de la bibliothèque à la disposition d’un projet entier.</span><span class="sxs-lookup"><span data-stu-id="2515b-146">Include the `@using MyComponentLib1` directive in the top-level *_Import.razor* file to make the library's components available to an entire project.</span></span> <span data-ttu-id="2515b-147">Ajoutez la directive à un fichier *_Import. Razor* à n’importe quel niveau pour appliquer l’espace de noms à une seule page ou à un ensemble de pages dans un dossier.</span><span class="sxs-lookup"><span data-stu-id="2515b-147">Add the directive to an *_Import.razor* file at any level to apply the namespace to a single page or set of pages within a folder.</span></span>

## <a name="build-pack-and-ship-to-nuget"></a><span data-ttu-id="2515b-148">Générer, empaqueter et envoyer à NuGet</span><span class="sxs-lookup"><span data-stu-id="2515b-148">Build, pack, and ship to NuGet</span></span>

<span data-ttu-id="2515b-149">Étant donné que les bibliothèques de composants sont des bibliothèques .NET standard, leur empaquetage et leur envoi à NuGet ne sont pas différents de l’empaquetage et de l’expédition d’une bibliothèque à NuGet.</span><span class="sxs-lookup"><span data-stu-id="2515b-149">Because component libraries are standard .NET libraries, packaging and shipping them to NuGet is no different from packaging and shipping any library to NuGet.</span></span> <span data-ttu-id="2515b-150">L’empaquetage est effectué à l’aide de la commande [dotnet Pack](/dotnet/core/tools/dotnet-pack) dans une interface de commande :</span><span class="sxs-lookup"><span data-stu-id="2515b-150">Packaging is performed using the [dotnet pack](/dotnet/core/tools/dotnet-pack) command in a command shell:</span></span>

```dotnetcli
dotnet pack
```

<span data-ttu-id="2515b-151">Téléchargez le package dans NuGet à l’aide de la commande [dotnet NuGet Publish](/dotnet/core/tools/dotnet-nuget-push) dans une interface de commande :</span><span class="sxs-lookup"><span data-stu-id="2515b-151">Upload the package to NuGet using the [dotnet nuget publish](/dotnet/core/tools/dotnet-nuget-push) command in a command shell:</span></span>

```dotnetcli
dotnet nuget publish
```

## <a name="create-a-razor-components-class-library-with-static-assets"></a><span data-ttu-id="2515b-152">Créer une bibliothèque de classes de composants Razor avec des ressources statiques</span><span class="sxs-lookup"><span data-stu-id="2515b-152">Create a Razor components class library with static assets</span></span>

<span data-ttu-id="2515b-153">Un RCL peut inclure des ressources statiques.</span><span class="sxs-lookup"><span data-stu-id="2515b-153">An RCL can include static assets.</span></span> <span data-ttu-id="2515b-154">Les ressources statiques sont disponibles pour toutes les applications qui consomment la bibliothèque.</span><span class="sxs-lookup"><span data-stu-id="2515b-154">The static assets are available to any app that consumes the library.</span></span> <span data-ttu-id="2515b-155">Pour plus d'informations, consultez <xref:razor-pages/ui-class#create-an-rcl-with-static-assets>.</span><span class="sxs-lookup"><span data-stu-id="2515b-155">For more information, see <xref:razor-pages/ui-class#create-an-rcl-with-static-assets>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="2515b-156">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="2515b-156">Additional resources</span></span>

* <xref:razor-pages/ui-class>
