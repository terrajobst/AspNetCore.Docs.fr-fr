---
title: Bibliothèques de classes de composants de Razor
author: guardrex
description: Découvrez comment les composants peuvent être inclus dans les composants de Razor des applications à partir d’une bibliothèque de composants externes.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 03/14/2019
uid: razor-components/class-libraries
ms.openlocfilehash: 1064ad60d90af15af483ba9bca5ed85fb63c2924
ms.sourcegitcommit: 10e14b85490f064395e9b2f423d21e3c2d39ed8b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/19/2019
ms.locfileid: "59515392"
---
# <a name="razor-components-class-libraries"></a><span data-ttu-id="f34a1-103">Bibliothèques de classes de composants de Razor</span><span class="sxs-lookup"><span data-stu-id="f34a1-103">Razor Components Class Libraries</span></span>

<span data-ttu-id="f34a1-104">Par [Simon Timms](https://github.com/stimms)</span><span class="sxs-lookup"><span data-stu-id="f34a1-104">By [Simon Timms](https://github.com/stimms)</span></span>

<span data-ttu-id="f34a1-105">Les composants peuvent être partagées dans les bibliothèques de classes Razor entre projets.</span><span class="sxs-lookup"><span data-stu-id="f34a1-105">Components can be shared in Razor class libraries across projects.</span></span> <span data-ttu-id="f34a1-106">Les composants peuvent être inclus dans :</span><span class="sxs-lookup"><span data-stu-id="f34a1-106">Components can be included from:</span></span>

* <span data-ttu-id="f34a1-107">Un autre projet dans la solution.</span><span class="sxs-lookup"><span data-stu-id="f34a1-107">Another project in the solution.</span></span>
* <span data-ttu-id="f34a1-108">Un package NuGet.</span><span class="sxs-lookup"><span data-stu-id="f34a1-108">A NuGet package.</span></span>
* <span data-ttu-id="f34a1-109">Une bibliothèque .NET référencée.</span><span class="sxs-lookup"><span data-stu-id="f34a1-109">A referenced .NET library.</span></span>

<span data-ttu-id="f34a1-110">Tout comme les composants sont des types .NET standards, les composants fournis par les bibliothèques de classes Razor sont des assemblys .NET normales.</span><span class="sxs-lookup"><span data-stu-id="f34a1-110">Just as components are regular .NET types, components provided by Razor class libraries are normal .NET assemblies.</span></span>

<span data-ttu-id="f34a1-111">Utilisez le `razorclasslib` modèle (bibliothèque de classes Razor) avec le [dotnet nouvelle](/dotnet/core/tools/dotnet-new) commande :</span><span class="sxs-lookup"><span data-stu-id="f34a1-111">Use the `razorclasslib` (Razor class library) template with the [dotnet new](/dotnet/core/tools/dotnet-new) command:</span></span>

```console
dotnet new razorclasslib -o MyComponentLib1
```

<span data-ttu-id="f34a1-112">Ajouter des fichiers de composant de Razor (*.razor*) à la bibliothèque de classes Razor.</span><span class="sxs-lookup"><span data-stu-id="f34a1-112">Add Razor Component files (*.razor*) to the Razor class library.</span></span>

<span data-ttu-id="f34a1-113">Pour ajouter la bibliothèque à un projet existant, utilisez la [dotnet sln](/dotnet/core/tools/dotnet-sln) commande :</span><span class="sxs-lookup"><span data-stu-id="f34a1-113">To add the library to an existing project, use the [dotnet sln](/dotnet/core/tools/dotnet-sln) command:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="f34a1-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f34a1-114">Visual Studio</span></span>](#tab/visual-studio)

```console
dotnet sln add .\MyComponentLib1
```

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="f34a1-115">CLI .NET Core</span><span class="sxs-lookup"><span data-stu-id="f34a1-115">.NET Core CLI</span></span>](#tab/netcore-cli)

```console
dotnet add WebApplication1 reference MyComponentLib1
```

---

> [!NOTE]
> <span data-ttu-id="f34a1-116">Bibliothèques de classes Razor ne sont pas compatibles avec les applications Blazor dans ASP.NET Core Preview 3.</span><span class="sxs-lookup"><span data-stu-id="f34a1-116">Razor class libraries aren't compatible with Blazor apps in ASP.NET Core Preview 3.</span></span>
>
> <span data-ttu-id="f34a1-117">Pour créer des composants dans une bibliothèque qui peut être partagée avec les applications Blazor et composants de Razor, utilisez une bibliothèque de classes Blazor créée par la `blazorlib` modèle.</span><span class="sxs-lookup"><span data-stu-id="f34a1-117">To create components in a library that can be shared with Blazor and Razor Components apps, use a Blazor class library created by the `blazorlib` template.</span></span>
>
> <span data-ttu-id="f34a1-118">Bibliothèques de classes Razor ne prennent en charge les ressources statiques dans ASP.NET Core Preview 3.</span><span class="sxs-lookup"><span data-stu-id="f34a1-118">Razor class libraries don't support static assets in ASP.NET Core Preview 3.</span></span> <span data-ttu-id="f34a1-119">Les bibliothèques de composants à l’aide de la `blazorlib` modèle peut inclure des fichiers statiques, tels que des images, de JavaScript et de feuilles de style.</span><span class="sxs-lookup"><span data-stu-id="f34a1-119">Component libraries using the `blazorlib` template can include static files, such as images, JavaScript, and stylesheets.</span></span> <span data-ttu-id="f34a1-120">Au moment de la génération, les fichiers statiques sont incorporés dans le fichier d’assembly généré (*.dll*), ce qui permet la consommation des composants sans avoir à vous soucier de l’inclusion de leurs ressources.</span><span class="sxs-lookup"><span data-stu-id="f34a1-120">At build time, static files are embedded into the built assembly file (*.dll*), which allows consumption of the components without having to worry about how to include their resources.</span></span> <span data-ttu-id="f34a1-121">Les fichiers inclus dans le `content` répertoire sont marquées comme ressource incorporée.</span><span class="sxs-lookup"><span data-stu-id="f34a1-121">Any files included in the `content` directory are marked as an embedded resource.</span></span>

## <a name="consume-a-library-component"></a><span data-ttu-id="f34a1-122">Utiliser un composant de bibliothèque</span><span class="sxs-lookup"><span data-stu-id="f34a1-122">Consume a library component</span></span>

<span data-ttu-id="f34a1-123">Pour consommer des composants définis dans une bibliothèque dans un autre projet, le [ @addTagHelper ](xref:mvc/views/tag-helpers/intro#add-helper-label) directive doit être utilisée.</span><span class="sxs-lookup"><span data-stu-id="f34a1-123">In order to consume components defined in a library in another project, the [@addTagHelper](xref:mvc/views/tag-helpers/intro#add-helper-label) directive must be used.</span></span> <span data-ttu-id="f34a1-124">Les composants individuels peuvent être ajoutés par nom.</span><span class="sxs-lookup"><span data-stu-id="f34a1-124">Individual components may be added by name.</span></span>

<span data-ttu-id="f34a1-125">Le format général de la directive est :</span><span class="sxs-lookup"><span data-stu-id="f34a1-125">The general format of the directive is:</span></span>

```cshtml
@addTagHelper MyComponentLib1.Component1, MyComponentLib1

<h1>Hello, world!</h1>

Welcome to your new app.

<Component1 />
```

<span data-ttu-id="f34a1-126">Par exemple, la directive suivante ajoute `Component1` de `MyComponentLib1`:</span><span class="sxs-lookup"><span data-stu-id="f34a1-126">For example, the following directive adds `Component1` of `MyComponentLib1`:</span></span>

```cshtml
@addTagHelper MyComponentLib1.Component1, MyComponentLib1
```

<span data-ttu-id="f34a1-127">Toutefois, il est courant d’inclure tous les composants à partir d’un assembly à l’aide d’un caractère générique (`*`) :</span><span class="sxs-lookup"><span data-stu-id="f34a1-127">However, it's common to include all of the components from an assembly using a wildcard (`*`):</span></span>

```cshtml
@addTagHelper *, MyComponentLib1
```

<span data-ttu-id="f34a1-128">Le `@addTagHelper` directive peut être incluse dans *_ViewImport.cshtml* pour rendre les composants disponibles pour un projet entier ou appliqués à une seule page ou un ensemble de pages au sein d’un dossier.</span><span class="sxs-lookup"><span data-stu-id="f34a1-128">The `@addTagHelper` directive can be included in *_ViewImport.cshtml* to make the components available for an entire project or applied to a single page or set of pages within a folder.</span></span> <span data-ttu-id="f34a1-129">Avec la `@addTagHelper` directive en place, les composants de la bibliothèque de composant peuvent être consommés comme s’ils étaient dans le même assembly que l’application.</span><span class="sxs-lookup"><span data-stu-id="f34a1-129">With the `@addTagHelper` directive in place, the components of the component library can be consumed as if they were in the same assembly as the app.</span></span>

## <a name="build-pack-and-ship-to-nuget"></a><span data-ttu-id="f34a1-130">Build, les packs et les expédier à NuGet</span><span class="sxs-lookup"><span data-stu-id="f34a1-130">Build, pack, and ship to NuGet</span></span>

<span data-ttu-id="f34a1-131">Étant donné que les bibliothèques de composants sont des bibliothèques .NET standard, empaquetage et leur transfert vers NuGet ne diffère pas d’emballage et d’expédition n’importe quelle bibliothèque à NuGet.</span><span class="sxs-lookup"><span data-stu-id="f34a1-131">Because component libraries are standard .NET libraries, packaging and shipping them to NuGet is no different from packaging and shipping any library to NuGet.</span></span> <span data-ttu-id="f34a1-132">Empaquetage est effectué à l’aide de la [dotnet pack](/dotnet/core/tools/dotnet-pack) commande :</span><span class="sxs-lookup"><span data-stu-id="f34a1-132">Packaging is performed using the [dotnet pack](/dotnet/core/tools/dotnet-pack) command:</span></span>

```console
dotnet pack
```

<span data-ttu-id="f34a1-133">Télécharger le package à l’aide de NuGet le [dotnet nuget publier](/dotnet/core/tools/dotnet-nuget-push) commande :</span><span class="sxs-lookup"><span data-stu-id="f34a1-133">Upload the package to NuGet using the [dotnet nuget publish](/dotnet/core/tools/dotnet-nuget-push) command:</span></span>

```console
dotnet nuget publish
```

<span data-ttu-id="f34a1-134">Lorsque vous utilisez la `blazorlib` modèle, les ressources statiques sont inclus dans le package NuGet.</span><span class="sxs-lookup"><span data-stu-id="f34a1-134">When using the `blazorlib` template, static resources are included in the NuGet package.</span></span> <span data-ttu-id="f34a1-135">Consommateurs de la bibliothèque reçoivent automatiquement des scripts et feuilles de style, les consommateurs ne sont pas nécessaires pour installer manuellement les ressources.</span><span class="sxs-lookup"><span data-stu-id="f34a1-135">Library consumers automatically receive scripts and stylesheets, so consumers aren't required to manually install the resources.</span></span>
