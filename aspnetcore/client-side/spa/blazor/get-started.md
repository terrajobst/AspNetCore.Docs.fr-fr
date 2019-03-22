---
title: Prise en main Blazor
author: guardrex
description: Découvrez comment bien démarrer avec Blazor en créant et modifiant un projet Blazor.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 02/12/2019
uid: spa/blazor/get-started
ms.openlocfilehash: f46bd9af0f0762e794349d4e98de5c086a690d72
ms.sourcegitcommit: a1c43150ed46aa01572399e8aede50d4668745ca
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/21/2019
ms.locfileid: "58327227"
---
# <a name="get-started-with-blazor"></a><span data-ttu-id="20dd8-103">Prise en main Blazor</span><span class="sxs-lookup"><span data-stu-id="20dd8-103">Get started with Blazor</span></span>

<span data-ttu-id="20dd8-104">Par [Daniel Roth](https://github.com/danroth27) et [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="20dd8-104">By [Daniel Roth](https://github.com/danroth27) and [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/razor-components-preview-notice.md)]

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="20dd8-105">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="20dd8-105">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="20dd8-106">Conditions préalables :</span><span class="sxs-lookup"><span data-stu-id="20dd8-106">Prerequisites:</span></span>

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.0.md)]

<span data-ttu-id="20dd8-107">Pour créer votre premier projet Blazor dans Visual Studio :</span><span class="sxs-lookup"><span data-stu-id="20dd8-107">To create your first Blazor project in Visual Studio:</span></span>

1. <span data-ttu-id="20dd8-108">Installez la dernière version [Blazor extension](https://go.microsoft.com/fwlink/?linkid=870389) à partir de Visual Studio Marketplace.</span><span class="sxs-lookup"><span data-stu-id="20dd8-108">Install the latest [Blazor extension](https://go.microsoft.com/fwlink/?linkid=870389) from the Visual Studio Marketplace.</span></span> <span data-ttu-id="20dd8-109">Cette étape permet de Blazor modèles disponibles pour Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="20dd8-109">This step makes Blazor templates available to Visual Studio.</span></span>
1. <span data-ttu-id="20dd8-110">Rendre les modèles Blazor disponible pour une utilisation avec l’interface CLI .NET Core en exécutant la commande suivante dans une invite de commandes :</span><span class="sxs-lookup"><span data-stu-id="20dd8-110">Make the Blazor templates available for use with the .NET Core CLI by running the following command in a command shell:</span></span>

   ```console
   dotnet new -i Microsoft.AspNetCore.Blazor.Templates::0.9.0-preview3-19154-02
   ```

1. <span data-ttu-id="20dd8-111">Sélectionnez **fichier** > **nouveau projet** > **Web** > **Application Web ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="20dd8-111">Select **File** > **New Project** > **Web** > **ASP.NET Core Web Application**.</span></span>
1. <span data-ttu-id="20dd8-112">Assurez-vous que **.NET Core** et **ASP.NET Core 3.0** sont sélectionnés en haut.</span><span class="sxs-lookup"><span data-stu-id="20dd8-112">Make sure **.NET Core** and **ASP.NET Core 3.0** are selected at the top.</span></span>
1. <span data-ttu-id="20dd8-113">Choisissez le modèle **Blazor** et sélectionnez **OK**.</span><span class="sxs-lookup"><span data-stu-id="20dd8-113">Choose the **Blazor** template and select **OK**.</span></span>
1. <span data-ttu-id="20dd8-114">Appuyez sur **F5** pour exécuter l'application.</span><span class="sxs-lookup"><span data-stu-id="20dd8-114">Press **F5** to run the app.</span></span>

<span data-ttu-id="20dd8-115">Félicitations !</span><span class="sxs-lookup"><span data-stu-id="20dd8-115">Congratulations!</span></span> <span data-ttu-id="20dd8-116">Vous venez d’exécuter votre première application Blazor !</span><span class="sxs-lookup"><span data-stu-id="20dd8-116">You just ran your first Blazor app!</span></span>

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

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="20dd8-117">CLI .NET Core</span><span class="sxs-lookup"><span data-stu-id="20dd8-117">.NET Core CLI</span></span>](#tab/netcore-cli/)

<span data-ttu-id="20dd8-118">Conditions préalables :</span><span class="sxs-lookup"><span data-stu-id="20dd8-118">Prerequisites:</span></span>

* [<span data-ttu-id="20dd8-119">Afficher un aperçu de .NET core SDK 3.0</span><span class="sxs-lookup"><span data-stu-id="20dd8-119">.NET Core SDK 3.0 Preview</span></span>](https://dotnet.microsoft.com/download/dotnet-core/3.0)

1. <span data-ttu-id="20dd8-120">Ajouter les modèles de Blazor en exécutant la commande suivante dans une invite de commandes :</span><span class="sxs-lookup"><span data-stu-id="20dd8-120">Add the Blazor templates by running the following command in a command shell:</span></span>

   ```console
   dotnet new -i Microsoft.AspNetCore.Blazor.Templates::0.9.0-preview3-19154-02
   ```

1. <span data-ttu-id="20dd8-121">Créer votre premier projet Blazor dans une invite de commandes :</span><span class="sxs-lookup"><span data-stu-id="20dd8-121">Create your first Blazor project in a command shell:</span></span>

   ```console
   dotnet new blazor -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

1. <span data-ttu-id="20dd8-122">Dans un navigateur, accédez à `https://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="20dd8-122">In a browser, navigate to `https://localhost:5001`.</span></span>

<span data-ttu-id="20dd8-123">Félicitations !</span><span class="sxs-lookup"><span data-stu-id="20dd8-123">Congratulations!</span></span> <span data-ttu-id="20dd8-124">Vous venez d’exécuter votre première application Blazor !</span><span class="sxs-lookup"><span data-stu-id="20dd8-124">You just ran your first Blazor app!</span></span>

---

## <a name="blazor-project"></a><span data-ttu-id="20dd8-125">Projet de Blazor</span><span class="sxs-lookup"><span data-stu-id="20dd8-125">Blazor project</span></span>

<span data-ttu-id="20dd8-126">Quand l’application est exécutée, plusieurs pages sont disponibles à partir des onglets dans la barre latérale :</span><span class="sxs-lookup"><span data-stu-id="20dd8-126">When the app is run, multiple pages are available from tabs in the sidebar:</span></span>

* <span data-ttu-id="20dd8-127">Accueil</span><span class="sxs-lookup"><span data-stu-id="20dd8-127">Home</span></span>
* <span data-ttu-id="20dd8-128">Counter</span><span class="sxs-lookup"><span data-stu-id="20dd8-128">Counter</span></span>
* <span data-ttu-id="20dd8-129">Récupérer des données</span><span class="sxs-lookup"><span data-stu-id="20dd8-129">Fetch data</span></span>

<span data-ttu-id="20dd8-130">Sur la page Counter, sélectionnez le bouton **Click me** pour incrémenter le compteur sans actualisation de la page.</span><span class="sxs-lookup"><span data-stu-id="20dd8-130">On the Counter page, select the **Click me** button to increment the counter without a page refresh.</span></span> <span data-ttu-id="20dd8-131">Incrémenter un compteur dans une page Web normalement requiert l’écriture de JavaScript, mais Blazor fournit une meilleure approche à l’aide C#.</span><span class="sxs-lookup"><span data-stu-id="20dd8-131">Incrementing a counter in a webpage normally requires writing JavaScript, but Blazor provides a better approach using C#.</span></span>

<span data-ttu-id="20dd8-132">*Pages/Counter.cshtml* :</span><span class="sxs-lookup"><span data-stu-id="20dd8-132">*Pages/Counter.cshtml*:</span></span>

[!code-cshtml[](get-started/samples_snapshot/3.x/Counter1.cshtml)]

<span data-ttu-id="20dd8-133">Une demande de `/counter` dans le navigateur, comme spécifié par le `@page` directive en haut, entraîne le composant de compteur afficher son contenu.</span><span class="sxs-lookup"><span data-stu-id="20dd8-133">A request for `/counter` in the browser, as specified by the `@page` directive at the top, causes the Counter component to render its content.</span></span> <span data-ttu-id="20dd8-134">Composants restituent dans une représentation en mémoire de l’arborescence de rendu qui peut ensuite être utilisé pour mettre à jour l’interface utilisateur de manière flexible et efficace.</span><span class="sxs-lookup"><span data-stu-id="20dd8-134">Components render into an in-memory representation of the render tree that can then be used to update the UI in a flexible and efficient way.</span></span>

<span data-ttu-id="20dd8-135">Chaque fois que le **Click me** bouton est sélectionné :</span><span class="sxs-lookup"><span data-stu-id="20dd8-135">Each time the **Click me** button is selected:</span></span>

* <span data-ttu-id="20dd8-136">Le `onclick` événement est déclenché.</span><span class="sxs-lookup"><span data-stu-id="20dd8-136">The `onclick` event is fired.</span></span>
* <span data-ttu-id="20dd8-137">La méthode `IncrementCount` est appelée.</span><span class="sxs-lookup"><span data-stu-id="20dd8-137">The `IncrementCount` method is called.</span></span>
* <span data-ttu-id="20dd8-138">Le `currentCount` est incrémenté.</span><span class="sxs-lookup"><span data-stu-id="20dd8-138">The `currentCount` is incremented.</span></span>
* <span data-ttu-id="20dd8-139">Le composant est une nouvelle fois restitué.</span><span class="sxs-lookup"><span data-stu-id="20dd8-139">The component is rendered again.</span></span>

<span data-ttu-id="20dd8-140">Le runtime compare le nouveau contenu au contenu précédent et s’applique uniquement le contenu est modifié pour le modèle DOM (Document Object).</span><span class="sxs-lookup"><span data-stu-id="20dd8-140">The runtime compares the new content to the previous content and only applies the changed content to the Document Object Model (DOM).</span></span>

<span data-ttu-id="20dd8-141">Ajouter un composant à un autre composant à l’aide d’une syntaxe de type HTML.</span><span class="sxs-lookup"><span data-stu-id="20dd8-141">Add a component to another component using an HTML-like syntax.</span></span> <span data-ttu-id="20dd8-142">Paramètres de composant sont spécifiés à l’aide d’attributs ou contenu enfant.</span><span class="sxs-lookup"><span data-stu-id="20dd8-142">Component parameters are specified using attributes or child content.</span></span> <span data-ttu-id="20dd8-143">Par exemple, un composant de compteur peut être ajouté à la page d’accueil de l’application en ajoutant un `<Counter />` élément pour le composant d’Index.</span><span class="sxs-lookup"><span data-stu-id="20dd8-143">For example, a Counter component can be added to the app's homepage by adding a `<Counter />` element to the Index component.</span></span>

<span data-ttu-id="20dd8-144">Dans *pages/index.cshtml*, remplacez le composant enquête invite avec un composant de compteur :</span><span class="sxs-lookup"><span data-stu-id="20dd8-144">In *Pages/Index.cshtml*, replace the Survey Prompt component with a Counter component:</span></span>

[!code-cshtml[](get-started/samples_snapshot/3.x/Index1.cshtml?highlight=7)]

<span data-ttu-id="20dd8-145">Exécuter l’application.</span><span class="sxs-lookup"><span data-stu-id="20dd8-145">Run the app.</span></span> <span data-ttu-id="20dd8-146">La page d’accueil a son propre compteur.</span><span class="sxs-lookup"><span data-stu-id="20dd8-146">The homepage has its own counter.</span></span>

<span data-ttu-id="20dd8-147">Pour ajouter un paramètre au composant de compteur, mettre à jour du composant `@functions` bloc :</span><span class="sxs-lookup"><span data-stu-id="20dd8-147">To add a parameter to the Counter component, update the component's `@functions` block:</span></span>

* <span data-ttu-id="20dd8-148">Ajouter une propriété pour `IncrementAmount` décorée avec le `[Parameter]` attribut.</span><span class="sxs-lookup"><span data-stu-id="20dd8-148">Add a property for `IncrementAmount` decorated with the `[Parameter]` attribute.</span></span>
* <span data-ttu-id="20dd8-149">Modifiez la méthode `IncrementCount` pour utiliser `IncrementAmount` lorsque vous augmentez la valeur de `currentCount`.</span><span class="sxs-lookup"><span data-stu-id="20dd8-149">Change the `IncrementCount` method to use the `IncrementAmount` when increasing the value of `currentCount`.</span></span>

<span data-ttu-id="20dd8-150">*Pages/Counter.cshtml* :</span><span class="sxs-lookup"><span data-stu-id="20dd8-150">*Pages/Counter.cshtml*:</span></span>

[!code-cshtml[](get-started/samples_snapshot/3.x/Counter2.cshtml?highlight=4,8)]

<span data-ttu-id="20dd8-151">Spécifiez un paramètre `IncrementAmount` dans l’élément `<Counter>` du composant Home à l’aide d’un attribut.</span><span class="sxs-lookup"><span data-stu-id="20dd8-151">Specify an `IncrementAmount` parameter in the Home component's `<Counter>` element using an attribute.</span></span>

<span data-ttu-id="20dd8-152">*Pages/Index.cshtml* :</span><span class="sxs-lookup"><span data-stu-id="20dd8-152">*Pages/Index.cshtml*:</span></span>

[!code-cshtml[](get-started/samples_snapshot/3.x/Index2.cshtml)]

<span data-ttu-id="20dd8-153">Exécuter l’application.</span><span class="sxs-lookup"><span data-stu-id="20dd8-153">Run the app.</span></span> <span data-ttu-id="20dd8-154">La page d’accueil a son propre compteur incrémente par dix chaque fois que le **Click me** bouton est sélectionné.</span><span class="sxs-lookup"><span data-stu-id="20dd8-154">The homepage has its own counter that increments by ten each time the **Click me** button is selected.</span></span>

## <a name="next-steps"></a><span data-ttu-id="20dd8-155">Étapes suivantes</span><span class="sxs-lookup"><span data-stu-id="20dd8-155">Next steps</span></span>

<xref:tutorials/first-razor-components-app>
