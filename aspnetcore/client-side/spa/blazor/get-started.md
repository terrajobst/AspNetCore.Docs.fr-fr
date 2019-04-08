---
title: Prise en main Blazor
author: guardrex
description: Découvrez comment bien démarrer avec Blazor en créant et modifiant un projet Blazor.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 04/07/2019
uid: spa/blazor/get-started
ms.openlocfilehash: b3928c2812be6f34cdf2f17295a1251106f651e5
ms.sourcegitcommit: 6bde1fdf686326c080a7518a6725e56e56d8886e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2019
ms.locfileid: "59068233"
---
# <a name="get-started-with-blazor"></a><span data-ttu-id="91fec-103">Prise en main Blazor</span><span class="sxs-lookup"><span data-stu-id="91fec-103">Get started with Blazor</span></span>

<span data-ttu-id="91fec-104">Par [Daniel Roth](https://github.com/danroth27) et [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="91fec-104">By [Daniel Roth](https://github.com/danroth27) and [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/razor-components-preview-notice.md)]

# [<a name="visual-studio"></a><span data-ttu-id="91fec-105">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="91fec-105">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="91fec-106">Conditions préalables :</span><span class="sxs-lookup"><span data-stu-id="91fec-106">Prerequisites:</span></span>

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.0.md)]

<span data-ttu-id="91fec-107">Pour créer votre premier projet Blazor dans Visual Studio :</span><span class="sxs-lookup"><span data-stu-id="91fec-107">To create your first Blazor project in Visual Studio:</span></span>

1. <span data-ttu-id="91fec-108">Installez la dernière version [kit SDK de .NET Core 3.0 Preview](https://dotnet.microsoft.com/download/dotnet-core/3.0) mise en production.</span><span class="sxs-lookup"><span data-stu-id="91fec-108">Install the latest [.NET Core 3.0 Preview SDK](https://dotnet.microsoft.com/download/dotnet-core/3.0) release.</span></span>
1. <span data-ttu-id="91fec-109">Permettre à Visual Studio utiliser la version préliminaire de kits de développement logiciel :</span><span class="sxs-lookup"><span data-stu-id="91fec-109">Enable Visual Studio to use preview SDKs:</span></span>
   1. <span data-ttu-id="91fec-110">Ouvrez **outils** > **Options** dans la barre de menus.</span><span class="sxs-lookup"><span data-stu-id="91fec-110">Open **Tools** > **Options** in the menu bar.</span></span>
   1. <span data-ttu-id="91fec-111">Ouvrez le **projets et Solutions** nœud.</span><span class="sxs-lookup"><span data-stu-id="91fec-111">Open the **Projects and Solutions** node.</span></span> <span data-ttu-id="91fec-112">Ouvrez le **.NET Core** onglet.</span><span class="sxs-lookup"><span data-stu-id="91fec-112">Open the **.NET Core** tab.</span></span>
   1. <span data-ttu-id="91fec-113">Cochez la case **utiliser des versions préliminaires du SDK .NET Core**.</span><span class="sxs-lookup"><span data-stu-id="91fec-113">Check the box for **Use previews of the .NET Core SDK**.</span></span> <span data-ttu-id="91fec-114">Sélectionnez **OK**.</span><span class="sxs-lookup"><span data-stu-id="91fec-114">Select **OK**.</span></span>
1. <span data-ttu-id="91fec-115">Installez la dernière version [Blazor extension](https://go.microsoft.com/fwlink/?linkid=870389) à partir de Visual Studio Marketplace.</span><span class="sxs-lookup"><span data-stu-id="91fec-115">Install the latest [Blazor extension](https://go.microsoft.com/fwlink/?linkid=870389) from the Visual Studio Marketplace.</span></span> <span data-ttu-id="91fec-116">Cette étape permet de Blazor modèles disponibles pour Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="91fec-116">This step makes Blazor templates available to Visual Studio.</span></span>
1. <span data-ttu-id="91fec-117">Rendre les modèles Blazor disponible pour une utilisation avec l’interface CLI .NET Core en exécutant la commande suivante dans une invite de commandes :</span><span class="sxs-lookup"><span data-stu-id="91fec-117">Make the Blazor templates available for use with the .NET Core CLI by running the following command in a command shell:</span></span>

   ```console
   dotnet new -i Microsoft.AspNetCore.Blazor.Templates::0.9.0-preview3-19154-02
   ```
1. <span data-ttu-id="91fec-118">Créer un nouveau projet.</span><span class="sxs-lookup"><span data-stu-id="91fec-118">Create a new project.</span></span>
1. <span data-ttu-id="91fec-119">Sélectionnez **Nouvelle application web ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="91fec-119">Select **ASP.NET Core Web Application**.</span></span> <span data-ttu-id="91fec-120">Sélectionnez **Suivant**.</span><span class="sxs-lookup"><span data-stu-id="91fec-120">Select **Next**.</span></span>
1. <span data-ttu-id="91fec-121">Fournissez un nom dans la **nom_projet** champ.</span><span class="sxs-lookup"><span data-stu-id="91fec-121">Provide a name in the **Project name** field.</span></span> <span data-ttu-id="91fec-122">Confirmer la **emplacement** entrée est correcte ou indiquez un emplacement pour le projet.</span><span class="sxs-lookup"><span data-stu-id="91fec-122">Confirm the **Location** entry is correct or provide a location for the project.</span></span> <span data-ttu-id="91fec-123">Sélectionnez **Créer**.</span><span class="sxs-lookup"><span data-stu-id="91fec-123">Select **Create**.</span></span>
1. <span data-ttu-id="91fec-124">Assurez-vous que **.NET Core** et **ASP.NET Core 3.0** sont sélectionnés en haut.</span><span class="sxs-lookup"><span data-stu-id="91fec-124">Make sure **.NET Core** and **ASP.NET Core 3.0** are selected at the top.</span></span>
1. <span data-ttu-id="91fec-125">Sélectionnez le **Blazor** modèle et sélectionnez **créer**.</span><span class="sxs-lookup"><span data-stu-id="91fec-125">Select the **Blazor** template and select **Create**.</span></span>
1. <span data-ttu-id="91fec-126">Appuyez sur **F5** pour exécuter l'application.</span><span class="sxs-lookup"><span data-stu-id="91fec-126">Press **F5** to run the app.</span></span>

<span data-ttu-id="91fec-127">Félicitations !</span><span class="sxs-lookup"><span data-stu-id="91fec-127">Congratulations!</span></span> <span data-ttu-id="91fec-128">Vous venez d’exécuter votre première application Blazor !</span><span class="sxs-lookup"><span data-stu-id="91fec-128">You just ran your first Blazor app!</span></span>

<!--

# [Visual Studio Code](#tab/visual-studio-code)

Prerequisites:

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.0.md)]

To create your first Blazor project in Visual Studio Code:

1. Execute the following command in a command shell:

   ```console
   dotnet new blazor -o WebApplication1
   ```

1. Open the *WebApplication1* folder in Visual Studio Code.

1. Visual Studio code offers to create assets to build and debug the app, which includes the *tasks.json* and *launch.json* files. Select **Yes** to add the assets.

1. Execute the app using the Visual Studio Code debugger.

1. In a browser, navigate to `https://localhost:5001`.

Congratulations! You just ran your first Blazor app!

# [Visual Studio for Mac](#tab/visual-studio-mac)

.NET Core 3.0 will be supported with Visual Studio for Mac version 8.0 or later. Visual Studio for Mac version 8.0 Preview isn't available at this time.

Use the [.NET Core CLI version of this topic](xref:razor-components/get-started?tabs=netcore-cli) on macOS.

[!INCLUDE[](~/includes/net-core-prereqs-mac-3.0.md)]

To create your first project Blazor project in Visual Studio for Mac:

1. Select **File** > **New Solution** or **New Project**.
1. In the sidebar, select **.NET Core** > **App**.
1. Select **Blazor** and select **Next**.
1. The **Target Framework** defaults to **.NET Core 3.0**. Select **Next**.
1. In the **Project Name** field, enter `WebApplication1`. Select **Create**.
1. Select **Run** > **Run Without Debugging** to run the app *without the debugger*. Running with the debugger isn't supported at this time.

Congratulations! You just ran your first Blazor app!
-->

# [<a name="net-core-cli"></a><span data-ttu-id="91fec-129">CLI .NET Core</span><span class="sxs-lookup"><span data-stu-id="91fec-129">.NET Core CLI</span></span>](#tab/netcore-cli/)

<span data-ttu-id="91fec-130">Conditions préalables :</span><span class="sxs-lookup"><span data-stu-id="91fec-130">Prerequisites:</span></span>

* [<span data-ttu-id="91fec-131">Afficher un aperçu de .NET core SDK 3.0</span><span class="sxs-lookup"><span data-stu-id="91fec-131">.NET Core SDK 3.0 Preview</span></span>](https://dotnet.microsoft.com/download/dotnet-core/3.0)

1. <span data-ttu-id="91fec-132">Ajouter les modèles de Blazor en exécutant la commande suivante dans une invite de commandes :</span><span class="sxs-lookup"><span data-stu-id="91fec-132">Add the Blazor templates by running the following command in a command shell:</span></span>

   ```console
   dotnet new -i Microsoft.AspNetCore.Blazor.Templates::0.9.0-preview3-19154-02
   ```

1. <span data-ttu-id="91fec-133">Créer votre premier projet Blazor dans une invite de commandes :</span><span class="sxs-lookup"><span data-stu-id="91fec-133">Create your first Blazor project in a command shell:</span></span>

   ```console
   dotnet new blazor -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

1. <span data-ttu-id="91fec-134">Dans un navigateur, accédez à `https://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="91fec-134">In a browser, navigate to `https://localhost:5001`.</span></span>

<span data-ttu-id="91fec-135">Félicitations !</span><span class="sxs-lookup"><span data-stu-id="91fec-135">Congratulations!</span></span> <span data-ttu-id="91fec-136">Vous venez d’exécuter votre première application Blazor !</span><span class="sxs-lookup"><span data-stu-id="91fec-136">You just ran your first Blazor app!</span></span>

---

## <a name="blazor-project"></a><span data-ttu-id="91fec-137">Projet de Blazor</span><span class="sxs-lookup"><span data-stu-id="91fec-137">Blazor project</span></span>

<span data-ttu-id="91fec-138">Quand l’application est exécutée, plusieurs pages sont disponibles à partir des onglets dans la barre latérale :</span><span class="sxs-lookup"><span data-stu-id="91fec-138">When the app is run, multiple pages are available from tabs in the sidebar:</span></span>

* <span data-ttu-id="91fec-139">Accueil</span><span class="sxs-lookup"><span data-stu-id="91fec-139">Home</span></span>
* <span data-ttu-id="91fec-140">Counter</span><span class="sxs-lookup"><span data-stu-id="91fec-140">Counter</span></span>
* <span data-ttu-id="91fec-141">Récupérer des données</span><span class="sxs-lookup"><span data-stu-id="91fec-141">Fetch data</span></span>

<span data-ttu-id="91fec-142">Sur la page Counter, sélectionnez le bouton **Click me** pour incrémenter le compteur sans actualisation de la page.</span><span class="sxs-lookup"><span data-stu-id="91fec-142">On the Counter page, select the **Click me** button to increment the counter without a page refresh.</span></span> <span data-ttu-id="91fec-143">Incrémenter un compteur dans une page Web normalement requiert l’écriture de JavaScript, mais Blazor fournit une meilleure approche à l’aide C#.</span><span class="sxs-lookup"><span data-stu-id="91fec-143">Incrementing a counter in a webpage normally requires writing JavaScript, but Blazor provides a better approach using C#.</span></span>

<span data-ttu-id="91fec-144">*Pages/Counter.cshtml* :</span><span class="sxs-lookup"><span data-stu-id="91fec-144">*Pages/Counter.cshtml*:</span></span>

[!code-cshtml[](get-started/samples_snapshot/3.x/Counter1.cshtml)]

<span data-ttu-id="91fec-145">Une demande de `/counter` dans le navigateur, comme spécifié par le `@page` directive en haut, entraîne le composant de compteur afficher son contenu.</span><span class="sxs-lookup"><span data-stu-id="91fec-145">A request for `/counter` in the browser, as specified by the `@page` directive at the top, causes the Counter component to render its content.</span></span> <span data-ttu-id="91fec-146">Composants restituent dans une représentation en mémoire de l’arborescence de rendu qui peut ensuite être utilisé pour mettre à jour l’interface utilisateur de manière flexible et efficace.</span><span class="sxs-lookup"><span data-stu-id="91fec-146">Components render into an in-memory representation of the render tree that can then be used to update the UI in a flexible and efficient way.</span></span>

<span data-ttu-id="91fec-147">Chaque fois que le **Click me** bouton est sélectionné :</span><span class="sxs-lookup"><span data-stu-id="91fec-147">Each time the **Click me** button is selected:</span></span>

* <span data-ttu-id="91fec-148">Le `onclick` événement est déclenché.</span><span class="sxs-lookup"><span data-stu-id="91fec-148">The `onclick` event is fired.</span></span>
* <span data-ttu-id="91fec-149">La méthode `IncrementCount` est appelée.</span><span class="sxs-lookup"><span data-stu-id="91fec-149">The `IncrementCount` method is called.</span></span>
* <span data-ttu-id="91fec-150">Le `currentCount` est incrémenté.</span><span class="sxs-lookup"><span data-stu-id="91fec-150">The `currentCount` is incremented.</span></span>
* <span data-ttu-id="91fec-151">Le composant est une nouvelle fois restitué.</span><span class="sxs-lookup"><span data-stu-id="91fec-151">The component is rendered again.</span></span>

<span data-ttu-id="91fec-152">Le runtime compare le nouveau contenu au contenu précédent et s’applique uniquement le contenu est modifié pour le modèle DOM (Document Object).</span><span class="sxs-lookup"><span data-stu-id="91fec-152">The runtime compares the new content to the previous content and only applies the changed content to the Document Object Model (DOM).</span></span>

<span data-ttu-id="91fec-153">Ajouter un composant à un autre composant à l’aide d’une syntaxe de type HTML.</span><span class="sxs-lookup"><span data-stu-id="91fec-153">Add a component to another component using an HTML-like syntax.</span></span> <span data-ttu-id="91fec-154">Paramètres de composant sont spécifiés à l’aide d’attributs ou contenu enfant.</span><span class="sxs-lookup"><span data-stu-id="91fec-154">Component parameters are specified using attributes or child content.</span></span> <span data-ttu-id="91fec-155">Par exemple, un composant de compteur peut être ajouté à la page d’accueil de l’application en ajoutant un `<Counter />` élément pour le composant d’Index.</span><span class="sxs-lookup"><span data-stu-id="91fec-155">For example, a Counter component can be added to the app's homepage by adding a `<Counter />` element to the Index component.</span></span>

<span data-ttu-id="91fec-156">Dans *pages/index.cshtml*, remplacez le composant enquête invite avec un composant de compteur :</span><span class="sxs-lookup"><span data-stu-id="91fec-156">In *Pages/Index.cshtml*, replace the Survey Prompt component with a Counter component:</span></span>

[!code-cshtml[](get-started/samples_snapshot/3.x/Index1.cshtml?highlight=7)]

<span data-ttu-id="91fec-157">Exécuter l’application.</span><span class="sxs-lookup"><span data-stu-id="91fec-157">Run the app.</span></span> <span data-ttu-id="91fec-158">La page d’accueil a son propre compteur.</span><span class="sxs-lookup"><span data-stu-id="91fec-158">The homepage has its own counter.</span></span>

<span data-ttu-id="91fec-159">Pour ajouter un paramètre au composant de compteur, mettre à jour du composant `@functions` bloc :</span><span class="sxs-lookup"><span data-stu-id="91fec-159">To add a parameter to the Counter component, update the component's `@functions` block:</span></span>

* <span data-ttu-id="91fec-160">Ajouter une propriété pour `IncrementAmount` décorée avec le `[Parameter]` attribut.</span><span class="sxs-lookup"><span data-stu-id="91fec-160">Add a property for `IncrementAmount` decorated with the `[Parameter]` attribute.</span></span>
* <span data-ttu-id="91fec-161">Modifiez la méthode `IncrementCount` pour utiliser `IncrementAmount` lorsque vous augmentez la valeur de `currentCount`.</span><span class="sxs-lookup"><span data-stu-id="91fec-161">Change the `IncrementCount` method to use the `IncrementAmount` when increasing the value of `currentCount`.</span></span>

<span data-ttu-id="91fec-162">*Pages/Counter.cshtml* :</span><span class="sxs-lookup"><span data-stu-id="91fec-162">*Pages/Counter.cshtml*:</span></span>

[!code-cshtml[](get-started/samples_snapshot/3.x/Counter2.cshtml?highlight=4,8)]

<span data-ttu-id="91fec-163">Spécifiez un paramètre `IncrementAmount` dans l’élément `<Counter>` du composant Home à l’aide d’un attribut.</span><span class="sxs-lookup"><span data-stu-id="91fec-163">Specify an `IncrementAmount` parameter in the Home component's `<Counter>` element using an attribute.</span></span>

<span data-ttu-id="91fec-164">*Pages/Index.cshtml* :</span><span class="sxs-lookup"><span data-stu-id="91fec-164">*Pages/Index.cshtml*:</span></span>

[!code-cshtml[](get-started/samples_snapshot/3.x/Index2.cshtml)]

<span data-ttu-id="91fec-165">Exécuter l’application.</span><span class="sxs-lookup"><span data-stu-id="91fec-165">Run the app.</span></span> <span data-ttu-id="91fec-166">La page d’accueil a son propre compteur incrémente par dix chaque fois que le **Click me** bouton est sélectionné.</span><span class="sxs-lookup"><span data-stu-id="91fec-166">The homepage has its own counter that increments by ten each time the **Click me** button is selected.</span></span>

## <a name="next-steps"></a><span data-ttu-id="91fec-167">Étapes suivantes</span><span class="sxs-lookup"><span data-stu-id="91fec-167">Next steps</span></span>

<xref:tutorials/first-razor-components-app>
