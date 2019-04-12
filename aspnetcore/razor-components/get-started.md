---
title: Prise en main des composants de Razor
author: guardrex
description: Découvrez comment bien démarrer avec les composants de Razor en créant et modifiant un projet de composants de Razor.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 04/07/2019
uid: razor-components/get-started
ms.openlocfilehash: 151e58497b0f22fa7c5a9bde1f665eeb73fd5dc3
ms.sourcegitcommit: 6bde1fdf686326c080a7518a6725e56e56d8886e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2019
ms.locfileid: "59515529"
---
# <a name="get-started-with-razor-components"></a><span data-ttu-id="16be7-103">Prise en main des composants de Razor</span><span class="sxs-lookup"><span data-stu-id="16be7-103">Get started with Razor Components</span></span>

<span data-ttu-id="16be7-104">Par [Daniel Roth](https://github.com/danroth27) et [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="16be7-104">By [Daniel Roth](https://github.com/danroth27) and [Luke Latham](https://github.com/guardrex)</span></span>

# [<a name="visual-studio"></a><span data-ttu-id="16be7-105">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="16be7-105">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="16be7-106">Conditions préalables :</span><span class="sxs-lookup"><span data-stu-id="16be7-106">Prerequisites:</span></span>

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.0.md)]

<span data-ttu-id="16be7-107">Pour créer votre premier projet de composants de Razor dans Visual Studio :</span><span class="sxs-lookup"><span data-stu-id="16be7-107">To create your first Razor Components project in Visual Studio:</span></span>

1. <span data-ttu-id="16be7-108">Installez la dernière version [kit SDK de .NET Core 3.0 Preview](https://dotnet.microsoft.com/download/dotnet-core/3.0) mise en production.</span><span class="sxs-lookup"><span data-stu-id="16be7-108">Install the latest [.NET Core 3.0 Preview SDK](https://dotnet.microsoft.com/download/dotnet-core/3.0) release.</span></span>
1. <span data-ttu-id="16be7-109">Permettre à Visual Studio utiliser la version préliminaire de kits de développement logiciel :</span><span class="sxs-lookup"><span data-stu-id="16be7-109">Enable Visual Studio to use preview SDKs:</span></span>
   1. <span data-ttu-id="16be7-110">Ouvrez **outils** > **Options** dans la barre de menus.</span><span class="sxs-lookup"><span data-stu-id="16be7-110">Open **Tools** > **Options** in the menu bar.</span></span>
   1. <span data-ttu-id="16be7-111">Ouvrez le **projets et Solutions** nœud.</span><span class="sxs-lookup"><span data-stu-id="16be7-111">Open the **Projects and Solutions** node.</span></span> <span data-ttu-id="16be7-112">Ouvrez le **.NET Core** onglet.</span><span class="sxs-lookup"><span data-stu-id="16be7-112">Open the **.NET Core** tab.</span></span>
   1. <span data-ttu-id="16be7-113">Cochez la case **utiliser des versions préliminaires du SDK .NET Core**.</span><span class="sxs-lookup"><span data-stu-id="16be7-113">Check the box for **Use previews of the .NET Core SDK**.</span></span> <span data-ttu-id="16be7-114">Sélectionnez **OK**.</span><span class="sxs-lookup"><span data-stu-id="16be7-114">Select **OK**.</span></span>
1. <span data-ttu-id="16be7-115">Créer un nouveau projet.</span><span class="sxs-lookup"><span data-stu-id="16be7-115">Create a new project.</span></span>
1. <span data-ttu-id="16be7-116">Sélectionnez **Nouvelle application web ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="16be7-116">Select **ASP.NET Core Web Application**.</span></span> <span data-ttu-id="16be7-117">Sélectionnez **Suivant**.</span><span class="sxs-lookup"><span data-stu-id="16be7-117">Select **Next**.</span></span>
1. <span data-ttu-id="16be7-118">Fournissez un nom dans la **nom_projet** champ.</span><span class="sxs-lookup"><span data-stu-id="16be7-118">Provide a name in the **Project name** field.</span></span> <span data-ttu-id="16be7-119">Confirmer la **emplacement** entrée est correcte ou indiquez un emplacement pour le projet.</span><span class="sxs-lookup"><span data-stu-id="16be7-119">Confirm the **Location** entry is correct or provide a location for the project.</span></span> <span data-ttu-id="16be7-120">Sélectionnez **Créer**.</span><span class="sxs-lookup"><span data-stu-id="16be7-120">Select **Create**.</span></span>
1. <span data-ttu-id="16be7-121">Assurez-vous que **.NET Core** et **ASP.NET Core 3.0** sont sélectionnés en haut.</span><span class="sxs-lookup"><span data-stu-id="16be7-121">Make sure **.NET Core** and **ASP.NET Core 3.0** are selected at the top.</span></span>
1. <span data-ttu-id="16be7-122">Choisissez le **Razor composants** modèle et sélectionnez **créer**.</span><span class="sxs-lookup"><span data-stu-id="16be7-122">Choose the **Razor Components** template and select **Create**.</span></span>
1. <span data-ttu-id="16be7-123">Appuyez sur **F5** pour exécuter l'application.</span><span class="sxs-lookup"><span data-stu-id="16be7-123">Press **F5** to run the app.</span></span>

<span data-ttu-id="16be7-124">Félicitations !</span><span class="sxs-lookup"><span data-stu-id="16be7-124">Congratulations!</span></span> <span data-ttu-id="16be7-125">Vous venez d’exécuter votre première application Razor composants !</span><span class="sxs-lookup"><span data-stu-id="16be7-125">You just ran your first Razor Components app!</span></span>

<!--

# [Visual Studio Code](#tab/visual-studio-code)

Prerequisites:

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.0.md)]

To create your first Razor Components project in Visual Studio Code:

1. Execute the following command from a command shell:

   ```console
   dotnet new razorcomponents -o WebApplication1
   ```

1. Open the *WebApplication1* folder in Visual Studio Code.

1. Add a *.vscode* folder.

1. Add a *tasks.json* file to the *.vscode* folder with the following content:

   [!code-json[](get-started/samples_snapshot/3.x/tasks.json)]

1. Add a *launch.json* file to the *.vscode* folder with the following content:

   [!code-json[](get-started/samples_snapshot/3.x/launch.json)]

1. Execute the app using the Visual Studio Code debugger.

1. In a browser, navigate to `https://localhost:5001`.

Congratulations! You just ran your first Razor Components app!

# [Visual Studio for Mac](#tab/visual-studio-mac)

.NET Core 3.0 will be supported with Visual Studio for Mac version 8.0 or later. Visual Studio for Mac version 8.0 Preview isn't available at this time.

Use the [.NET Core CLI version of this topic](xref:razor-components/get-started?tabs=netcore-cli) on macOS.

[!INCLUDE[](~/includes/net-core-prereqs-mac-3.0.md)]

To create your first project Razor Components project in Visual Studio for Mac:

1. Select **File** > **New Solution** or **New Project**.
1. In the sidebar, select **.NET Core** > **App**.
1. Select **ASP.NET Core Razor Components** and select **Next**.
1. The **Target Framework** defaults to **.NET Core 3.0**. Select **Next**.
1. In the **Project Name** field, enter `WebApplication1`. Select **Create**.
1. Select **Run** > **Run Without Debugging** to run the app *without the debugger*. Running with the debugger isn't supported at this time.

Congratulations! You just ran your first Razor Components app!
-->

# [<a name="net-core-cli"></a><span data-ttu-id="16be7-126">CLI .NET Core</span><span class="sxs-lookup"><span data-stu-id="16be7-126">.NET Core CLI</span></span>](#tab/netcore-cli/)

<span data-ttu-id="16be7-127">Conditions préalables :</span><span class="sxs-lookup"><span data-stu-id="16be7-127">Prerequisites:</span></span>

* [<span data-ttu-id="16be7-128">Afficher un aperçu de .NET core SDK 3.0</span><span class="sxs-lookup"><span data-stu-id="16be7-128">.NET Core SDK 3.0 Preview</span></span>](https://dotnet.microsoft.com/download/dotnet-core/3.0)

1. <span data-ttu-id="16be7-129">Pour créer votre premier projet Razor composants à partir d’une invite de commandes :</span><span class="sxs-lookup"><span data-stu-id="16be7-129">To create your first Razor Components project from a command shell:</span></span>

   ```console
   dotnet new razorcomponents -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

1. <span data-ttu-id="16be7-130">Dans un navigateur, accédez à `https://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="16be7-130">In a browser, navigate to `https://localhost:5001`.</span></span>

<span data-ttu-id="16be7-131">Félicitations !</span><span class="sxs-lookup"><span data-stu-id="16be7-131">Congratulations!</span></span> <span data-ttu-id="16be7-132">Vous venez d’exécuter votre première application Razor composants !</span><span class="sxs-lookup"><span data-stu-id="16be7-132">You just ran your first Razor Components app!</span></span>

---

## <a name="razor-components-project"></a><span data-ttu-id="16be7-133">Projet de composants de Razor</span><span class="sxs-lookup"><span data-stu-id="16be7-133">Razor Components project</span></span>

<span data-ttu-id="16be7-134">Composants de Razor sont créés à l’aide de la syntaxe Razor, mais sont compilées différemment des vues de Pages Razor et MVC.</span><span class="sxs-lookup"><span data-stu-id="16be7-134">Razor Components are authored using Razor syntax but are compiled differently than Razor Pages and MVC views.</span></span> <span data-ttu-id="16be7-135">Le *.razor* extension de fichier est utilisée pour spécifier un composant de Razor.</span><span class="sxs-lookup"><span data-stu-id="16be7-135">The *.razor* file extension is used to specify a Razor Component.</span></span> <span data-ttu-id="16be7-136">Les Pages Razor et MVC vues continuent à utiliser le *.cshtml* extension de fichier.</span><span class="sxs-lookup"><span data-stu-id="16be7-136">Razor Pages and MVC views continue to use the *.cshtml* file extension.</span></span>

> [!NOTE]
> <span data-ttu-id="16be7-137">Razor composants peuvent être créés à l’aide de la *.cshtml* extension de fichier tant que ces fichiers sont identifiés en tant que fichiers de composant de Razor à l’aide de la `_RazorComponentInclude` propriété MSBuild.</span><span class="sxs-lookup"><span data-stu-id="16be7-137">Razor Components can be authored using the *.cshtml* file extension as long as those files are identified as Razor Component files using the `_RazorComponentInclude` MSBuild property.</span></span> <span data-ttu-id="16be7-138">Par exemple, une application créée à l’aide du modèle de composant de Razor Spécifie que tous les *.cshtml* fichiers sous le *composants* dossier doit être traité en tant que composants de Razor :</span><span class="sxs-lookup"><span data-stu-id="16be7-138">For example, an app created using the Razor Component template specifies that all *.cshtml* files under the *Components* folder should be treated as Razor Components:</span></span>
>
> ```xml
> <_RazorComponentInclude>Components\**\*.cshtml</_RazorComponentInclude>
> ```

<span data-ttu-id="16be7-139">Quand l’application est exécutée, plusieurs pages sont disponibles à partir des onglets dans la barre latérale :</span><span class="sxs-lookup"><span data-stu-id="16be7-139">When the app is run, multiple pages are available from tabs in the sidebar:</span></span>

* <span data-ttu-id="16be7-140">Accueil</span><span class="sxs-lookup"><span data-stu-id="16be7-140">Home</span></span>
* <span data-ttu-id="16be7-141">Counter</span><span class="sxs-lookup"><span data-stu-id="16be7-141">Counter</span></span>
* <span data-ttu-id="16be7-142">Récupérer des données</span><span class="sxs-lookup"><span data-stu-id="16be7-142">Fetch data</span></span>

<span data-ttu-id="16be7-143">Sur la page Counter, sélectionnez le bouton **Click me** pour incrémenter le compteur sans actualisation de la page.</span><span class="sxs-lookup"><span data-stu-id="16be7-143">On the Counter page, select the **Click me** button to increment the counter without a page refresh.</span></span> <span data-ttu-id="16be7-144">L’incrémentation d’un compteur dans une page web requiert normalement l’écriture JavaScript, mais le projet Composants Razor fournit une meilleure approche à l’aide C#.</span><span class="sxs-lookup"><span data-stu-id="16be7-144">Incrementing a counter in a webpage normally requires writing JavaScript, but Razor Components provides a better approach using C#.</span></span>

<span data-ttu-id="16be7-145">*WebApplication1/Components/Pages/Counter.razor*:</span><span class="sxs-lookup"><span data-stu-id="16be7-145">*WebApplication1/Components/Pages/Counter.razor*:</span></span>

[!code-cshtml[](get-started/samples_snapshot/3.x/Counter1.razor)]

<span data-ttu-id="16be7-146">Une demande de `/counter` dans le navigateur, comme spécifié par le `@page` directive en haut, entraîne le composant de compteur afficher son contenu.</span><span class="sxs-lookup"><span data-stu-id="16be7-146">A request for `/counter` in the browser, as specified by the `@page` directive at the top, causes the Counter component to render its content.</span></span> <span data-ttu-id="16be7-147">Composants restituent dans une représentation en mémoire de l’arborescence de rendu qui peut ensuite être utilisé pour mettre à jour l’interface utilisateur de manière flexible et efficace.</span><span class="sxs-lookup"><span data-stu-id="16be7-147">Components render into an in-memory representation of the render tree that can then be used to update the UI in a flexible and efficient way.</span></span>

<span data-ttu-id="16be7-148">Chaque fois que le **Click me** bouton est sélectionné :</span><span class="sxs-lookup"><span data-stu-id="16be7-148">Each time the **Click me** button is selected:</span></span>

* <span data-ttu-id="16be7-149">Le `onclick` événement est déclenché.</span><span class="sxs-lookup"><span data-stu-id="16be7-149">The `onclick` event is fired.</span></span>
* <span data-ttu-id="16be7-150">La méthode `IncrementCount` est appelée.</span><span class="sxs-lookup"><span data-stu-id="16be7-150">The `IncrementCount` method is called.</span></span>
* <span data-ttu-id="16be7-151">Le `currentCount` est incrémenté.</span><span class="sxs-lookup"><span data-stu-id="16be7-151">The `currentCount` is incremented.</span></span>
* <span data-ttu-id="16be7-152">Le composant est une nouvelle fois restitué.</span><span class="sxs-lookup"><span data-stu-id="16be7-152">The component is rendered again.</span></span>

<span data-ttu-id="16be7-153">Le runtime compare le nouveau contenu au contenu précédent et s’applique uniquement le contenu est modifié pour le modèle DOM (Document Object).</span><span class="sxs-lookup"><span data-stu-id="16be7-153">The runtime compares the new content to the previous content and only applies the changed content to the Document Object Model (DOM).</span></span>

<span data-ttu-id="16be7-154">Ajouter un composant à un autre composant à l’aide d’une syntaxe de type HTML.</span><span class="sxs-lookup"><span data-stu-id="16be7-154">Add a component to another component using an HTML-like syntax.</span></span> <span data-ttu-id="16be7-155">Paramètres de composant sont spécifiés à l’aide d’attributs ou contenu enfant.</span><span class="sxs-lookup"><span data-stu-id="16be7-155">Component parameters are specified using attributes or child content.</span></span> <span data-ttu-id="16be7-156">Par exemple, un composant de compteur peut être ajouté à la page d’accueil de l’application en ajoutant un `<Counter />` élément pour le composant d’Index.</span><span class="sxs-lookup"><span data-stu-id="16be7-156">For example, a Counter component can be added to the app's homepage by adding a `<Counter />` element to the Index component.</span></span>

<span data-ttu-id="16be7-157">*WebApplication1/Components/Pages/Index.razor*:</span><span class="sxs-lookup"><span data-stu-id="16be7-157">*WebApplication1/Components/Pages/Index.razor*:</span></span>

[!code-cshtml[](get-started/samples_snapshot/3.x/Index1.razor?highlight=7)]

<span data-ttu-id="16be7-158">Exécuter l’application.</span><span class="sxs-lookup"><span data-stu-id="16be7-158">Run the app.</span></span> <span data-ttu-id="16be7-159">La page d’accueil a son propre compteur.</span><span class="sxs-lookup"><span data-stu-id="16be7-159">The homepage has its own counter.</span></span>

<span data-ttu-id="16be7-160">Pour ajouter un paramètre au composant de compteur, mettre à jour du composant `@functions` bloc :</span><span class="sxs-lookup"><span data-stu-id="16be7-160">To add a parameter to the Counter component, update the component's `@functions` block:</span></span>

* <span data-ttu-id="16be7-161">Ajouter une propriété pour `IncrementAmount` décorée avec le `[Parameter]` attribut.</span><span class="sxs-lookup"><span data-stu-id="16be7-161">Add a property for `IncrementAmount` decorated with the `[Parameter]` attribute.</span></span>
* <span data-ttu-id="16be7-162">Modifiez la méthode `IncrementCount` pour utiliser `IncrementAmount` lorsque vous augmentez la valeur de `currentCount`.</span><span class="sxs-lookup"><span data-stu-id="16be7-162">Change the `IncrementCount` method to use the `IncrementAmount` when increasing the value of `currentCount`.</span></span>

<span data-ttu-id="16be7-163">*WebApplication1/Components/Pages/Counter.razor*:</span><span class="sxs-lookup"><span data-stu-id="16be7-163">*WebApplication1/Components/Pages/Counter.razor*:</span></span>

[!code-cshtml[](get-started/samples_snapshot/3.x/Counter2.razor?highlight=4,8)]

<span data-ttu-id="16be7-164">Spécifiez un paramètre `IncrementAmount` dans l’élément `<Counter>` du composant Home à l’aide d’un attribut.</span><span class="sxs-lookup"><span data-stu-id="16be7-164">Specify an `IncrementAmount` parameter in the Home component's `<Counter>` element using an attribute.</span></span>

<span data-ttu-id="16be7-165">*WebApplication1/Components/Pages/Index.razor*:</span><span class="sxs-lookup"><span data-stu-id="16be7-165">*WebApplication1/Components/Pages/Index.razor*:</span></span>

[!code-cshtml[](get-started/samples_snapshot/3.x/Index2.razor)]

<span data-ttu-id="16be7-166">Exécuter l’application.</span><span class="sxs-lookup"><span data-stu-id="16be7-166">Run the app.</span></span> <span data-ttu-id="16be7-167">La page d’accueil a son propre compteur incrémente par dix chaque fois que le **Click me** bouton est sélectionné.</span><span class="sxs-lookup"><span data-stu-id="16be7-167">The homepage has its own counter that increments by ten each time the **Click me** button is selected.</span></span>

## <a name="next-steps"></a><span data-ttu-id="16be7-168">Étapes suivantes</span><span class="sxs-lookup"><span data-stu-id="16be7-168">Next steps</span></span>

<xref:tutorials/first-razor-components-app>
