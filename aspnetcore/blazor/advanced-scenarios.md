---
title: Scénarios avancés de ASP.NET Core Blazor
author: guardrex
description: En savoir plus sur les scénarios avancés dans Blazor, y compris l’intégration de la logique manuelle RenderTreeBuilder dans une application.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 02/12/2020
no-loc:
- Blazor
- SignalR
uid: blazor/advanced-scenarios
ms.openlocfilehash: 5e0618faa7b1b5e4cc15e30d9c16afaf7ccabaf0
ms.sourcegitcommit: 6645435fc8f5092fc7e923742e85592b56e37ada
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/19/2020
ms.locfileid: "77453260"
---
# <a name="aspnet-core-blazor-advanced-scenarios"></a><span data-ttu-id="30600-103">Scénarios avancés ASP.NET Core éblouissants</span><span class="sxs-lookup"><span data-stu-id="30600-103">ASP.NET Core Blazor advanced scenarios</span></span>

<span data-ttu-id="30600-104">Par [Luke Latham](https://github.com/guardrex) et [Daniel Roth](https://github.com/danroth27)</span><span class="sxs-lookup"><span data-stu-id="30600-104">By [Luke Latham](https://github.com/guardrex) and [Daniel Roth](https://github.com/danroth27)</span></span>

## <a name="manual-rendertreebuilder-logic"></a><span data-ttu-id="30600-105">Logique RenderTreeBuilder manuelle</span><span class="sxs-lookup"><span data-stu-id="30600-105">Manual RenderTreeBuilder logic</span></span>

<span data-ttu-id="30600-106">`Microsoft.AspNetCore.Components.Rendering.RenderTreeBuilder` fournit des méthodes pour la manipulation des composants et des éléments, notamment la C# génération manuelle de composants dans le code.</span><span class="sxs-lookup"><span data-stu-id="30600-106">`Microsoft.AspNetCore.Components.Rendering.RenderTreeBuilder` provides methods for manipulating components and elements, including building components manually in C# code.</span></span>

> [!NOTE]
> <span data-ttu-id="30600-107">L’utilisation de `RenderTreeBuilder` pour créer des composants est un scénario avancé.</span><span class="sxs-lookup"><span data-stu-id="30600-107">Use of `RenderTreeBuilder` to create components is an advanced scenario.</span></span> <span data-ttu-id="30600-108">Un composant mal formé (par exemple, une balise de balisage non fermée) peut entraîner un comportement indéfini.</span><span class="sxs-lookup"><span data-stu-id="30600-108">A malformed component (for example, an unclosed markup tag) can result in undefined behavior.</span></span>

<span data-ttu-id="30600-109">Prenons le composant `PetDetails` suivant, qui peut être intégré manuellement à un autre composant :</span><span class="sxs-lookup"><span data-stu-id="30600-109">Consider the following `PetDetails` component, which can be manually built into another component:</span></span>

```razor
<h2>Pet Details Component</h2>

<p>@PetDetailsQuote</p>

@code
{
    [Parameter]
    public string PetDetailsQuote { get; set; }
}
```

<span data-ttu-id="30600-110">Dans l’exemple suivant, la boucle de la méthode `CreateComponent` génère trois composants `PetDetails`.</span><span class="sxs-lookup"><span data-stu-id="30600-110">In the following example, the loop in the `CreateComponent` method generates three `PetDetails` components.</span></span> <span data-ttu-id="30600-111">Lors de l’appel de méthodes `RenderTreeBuilder` pour créer les composants (`OpenComponent` et `AddAttribute`), les numéros de séquence sont des numéros de ligne de code source.</span><span class="sxs-lookup"><span data-stu-id="30600-111">When calling `RenderTreeBuilder` methods to create the components (`OpenComponent` and `AddAttribute`), sequence numbers are source code line numbers.</span></span> <span data-ttu-id="30600-112">L’algorithme de différence éblouissant s’appuie sur les numéros de séquence correspondant à des lignes de code distinctes, et non sur des appels d’appel distincts.</span><span class="sxs-lookup"><span data-stu-id="30600-112">The Blazor difference algorithm relies on the sequence numbers corresponding to distinct lines of code, not distinct call invocations.</span></span> <span data-ttu-id="30600-113">Lors de la création d’un composant avec des méthodes `RenderTreeBuilder`, coder en dur les arguments pour les numéros de séquence.</span><span class="sxs-lookup"><span data-stu-id="30600-113">When creating a component with `RenderTreeBuilder` methods, hardcode the arguments for sequence numbers.</span></span> <span data-ttu-id="30600-114">**L’utilisation d’un calcul ou d’un compteur pour générer le numéro de séquence peut entraîner des performances médiocres.**</span><span class="sxs-lookup"><span data-stu-id="30600-114">**Using a calculation or counter to generate the sequence number can lead to poor performance.**</span></span> <span data-ttu-id="30600-115">Pour plus d’informations, consultez la section [numéros de séquence liés à des numéros de ligne de code et non à un ordre d’exécution](#sequence-numbers-relate-to-code-line-numbers-and-not-execution-order) .</span><span class="sxs-lookup"><span data-stu-id="30600-115">For more information, see the [Sequence numbers relate to code line numbers and not execution order](#sequence-numbers-relate-to-code-line-numbers-and-not-execution-order) section.</span></span>

<span data-ttu-id="30600-116">composant `BuiltContent` :</span><span class="sxs-lookup"><span data-stu-id="30600-116">`BuiltContent` component:</span></span>

```razor
@page "/BuiltContent"

<h1>Build a component</h1>

@CustomRender

<button type="button" @onclick="RenderComponent">
    Create three Pet Details components
</button>

@code {
    private RenderFragment CustomRender { get; set; }
    
    private RenderFragment CreateComponent() => builder =>
    {
        for (var i = 0; i < 3; i++) 
        {
            builder.OpenComponent(0, typeof(PetDetails));
            builder.AddAttribute(1, "PetDetailsQuote", "Someone's best friend!");
            builder.CloseComponent();
        }
    };    
    
    private void RenderComponent()
    {
        CustomRender = CreateComponent();
    }
}
```

> [!WARNING]
> <span data-ttu-id="30600-117">Les types dans `Microsoft.AspNetCore.Components.RenderTree` permettent le traitement des *résultats* des opérations de rendu.</span><span class="sxs-lookup"><span data-stu-id="30600-117">The types in `Microsoft.AspNetCore.Components.RenderTree` allow processing of the *results* of rendering operations.</span></span> <span data-ttu-id="30600-118">Il s’agit des détails internes de l’implémentation du Framework éblouissant.</span><span class="sxs-lookup"><span data-stu-id="30600-118">These are internal details of the Blazor framework implementation.</span></span> <span data-ttu-id="30600-119">Ces types doivent être considérés comme *instables* et susceptibles d’être modifiés dans les versions ultérieures.</span><span class="sxs-lookup"><span data-stu-id="30600-119">These types should be considered *unstable* and subject to change in future releases.</span></span>

### <a name="sequence-numbers-relate-to-code-line-numbers-and-not-execution-order"></a><span data-ttu-id="30600-120">Les numéros de séquence sont liés aux numéros de ligne de code et non à l’ordre d’exécution</span><span class="sxs-lookup"><span data-stu-id="30600-120">Sequence numbers relate to code line numbers and not execution order</span></span>

<span data-ttu-id="30600-121">Les fichiers de composants Razor ( *. Razor*) sont toujours compilés.</span><span class="sxs-lookup"><span data-stu-id="30600-121">Razor component files (*.razor*) are always compiled.</span></span> <span data-ttu-id="30600-122">La compilation est un avantage potentiel de l’interprétation du code, car l’étape de compilation peut être utilisée pour injecter des informations qui améliorent les performances de l’application au moment de l’exécution.</span><span class="sxs-lookup"><span data-stu-id="30600-122">Compilation is a potential advantage over interpreting code because the compile step can be used to inject information that improves app performance at runtime.</span></span>

<span data-ttu-id="30600-123">Un exemple clé de ces améliorations concerne les *numéros de séquence*.</span><span class="sxs-lookup"><span data-stu-id="30600-123">A key example of these improvements involves *sequence numbers*.</span></span> <span data-ttu-id="30600-124">Les numéros de séquence indiquent au runtime que les sorties proviennent de lignes de code distinctes et ordonnées.</span><span class="sxs-lookup"><span data-stu-id="30600-124">Sequence numbers indicate to the runtime which outputs came from which distinct and ordered lines of code.</span></span> <span data-ttu-id="30600-125">Le runtime utilise ces informations pour générer des différences d’arborescence efficaces en temps linéaire, ce qui est beaucoup plus rapide qu’un algorithme de comparaison d’arborescence générale.</span><span class="sxs-lookup"><span data-stu-id="30600-125">The runtime uses this information to generate efficient tree diffs in linear time, which is far faster than is normally possible for a general tree diff algorithm.</span></span>

<span data-ttu-id="30600-126">Considérez le fichier de composant Razor ( *. Razor*) suivant :</span><span class="sxs-lookup"><span data-stu-id="30600-126">Consider the following Razor component (*.razor*) file:</span></span>

```razor
@if (someFlag)
{
    <text>First</text>
}

Second
```

<span data-ttu-id="30600-127">Le code précédent se compile comme suit :</span><span class="sxs-lookup"><span data-stu-id="30600-127">The preceding code compiles to something like the following:</span></span>

```csharp
if (someFlag)
{
    builder.AddContent(0, "First");
}

builder.AddContent(1, "Second");
```

<span data-ttu-id="30600-128">Lorsque le code s’exécute pour la première fois, si `someFlag` est `true`, le générateur reçoit :</span><span class="sxs-lookup"><span data-stu-id="30600-128">When the code executes for the first time, if `someFlag` is `true`, the builder receives:</span></span>

| <span data-ttu-id="30600-129">Séquence</span><span class="sxs-lookup"><span data-stu-id="30600-129">Sequence</span></span> | <span data-ttu-id="30600-130">Type</span><span class="sxs-lookup"><span data-stu-id="30600-130">Type</span></span>      | <span data-ttu-id="30600-131">Données</span><span class="sxs-lookup"><span data-stu-id="30600-131">Data</span></span>   |
| :------: | --------- | :----: |
| <span data-ttu-id="30600-132">0</span><span class="sxs-lookup"><span data-stu-id="30600-132">0</span></span>        | <span data-ttu-id="30600-133">Nœud de texte</span><span class="sxs-lookup"><span data-stu-id="30600-133">Text node</span></span> | <span data-ttu-id="30600-134">Premier</span><span class="sxs-lookup"><span data-stu-id="30600-134">First</span></span>  |
| <span data-ttu-id="30600-135">1</span><span class="sxs-lookup"><span data-stu-id="30600-135">1</span></span>        | <span data-ttu-id="30600-136">Nœud de texte</span><span class="sxs-lookup"><span data-stu-id="30600-136">Text node</span></span> | <span data-ttu-id="30600-137">Seconde</span><span class="sxs-lookup"><span data-stu-id="30600-137">Second</span></span> |

<span data-ttu-id="30600-138">Imaginez que `someFlag` devient `false`et que le balisage est de nouveau restitué.</span><span class="sxs-lookup"><span data-stu-id="30600-138">Imagine that `someFlag` becomes `false`, and the markup is rendered again.</span></span> <span data-ttu-id="30600-139">Cette fois-ci, le générateur reçoit :</span><span class="sxs-lookup"><span data-stu-id="30600-139">This time, the builder receives:</span></span>

| <span data-ttu-id="30600-140">Séquence</span><span class="sxs-lookup"><span data-stu-id="30600-140">Sequence</span></span> | <span data-ttu-id="30600-141">Type</span><span class="sxs-lookup"><span data-stu-id="30600-141">Type</span></span>       | <span data-ttu-id="30600-142">Données</span><span class="sxs-lookup"><span data-stu-id="30600-142">Data</span></span>   |
| :------: | ---------- | :----: |
| <span data-ttu-id="30600-143">1</span><span class="sxs-lookup"><span data-stu-id="30600-143">1</span></span>        | <span data-ttu-id="30600-144">Nœud de texte</span><span class="sxs-lookup"><span data-stu-id="30600-144">Text node</span></span>  | <span data-ttu-id="30600-145">Seconde</span><span class="sxs-lookup"><span data-stu-id="30600-145">Second</span></span> |

<span data-ttu-id="30600-146">Quand le runtime effectue une comparaison, il constate que l’élément au `0` de la séquence a été supprimé, donc il génère le *script d’édition*trivial suivant :</span><span class="sxs-lookup"><span data-stu-id="30600-146">When the runtime performs a diff, it sees that the item at sequence `0` was removed, so it generates the following trivial *edit script*:</span></span>

* <span data-ttu-id="30600-147">Supprimez le premier nœud de texte.</span><span class="sxs-lookup"><span data-stu-id="30600-147">Remove the first text node.</span></span>

### <a name="the-problem-with-generating-sequence-numbers-programmatically"></a><span data-ttu-id="30600-148">Le problème de la génération par programmation des numéros de séquence</span><span class="sxs-lookup"><span data-stu-id="30600-148">The problem with generating sequence numbers programmatically</span></span>

<span data-ttu-id="30600-149">Imaginez plutôt que vous avez écrit la logique de générateur d’arborescence de rendu suivante :</span><span class="sxs-lookup"><span data-stu-id="30600-149">Imagine instead that you wrote the following render tree builder logic:</span></span>

```csharp
var seq = 0;

if (someFlag)
{
    builder.AddContent(seq++, "First");
}

builder.AddContent(seq++, "Second");
```

<span data-ttu-id="30600-150">La première sortie est désormais :</span><span class="sxs-lookup"><span data-stu-id="30600-150">Now, the first output is:</span></span>

| <span data-ttu-id="30600-151">Séquence</span><span class="sxs-lookup"><span data-stu-id="30600-151">Sequence</span></span> | <span data-ttu-id="30600-152">Type</span><span class="sxs-lookup"><span data-stu-id="30600-152">Type</span></span>      | <span data-ttu-id="30600-153">Données</span><span class="sxs-lookup"><span data-stu-id="30600-153">Data</span></span>   |
| :------: | --------- | :----: |
| <span data-ttu-id="30600-154">0</span><span class="sxs-lookup"><span data-stu-id="30600-154">0</span></span>        | <span data-ttu-id="30600-155">Nœud de texte</span><span class="sxs-lookup"><span data-stu-id="30600-155">Text node</span></span> | <span data-ttu-id="30600-156">Premier</span><span class="sxs-lookup"><span data-stu-id="30600-156">First</span></span>  |
| <span data-ttu-id="30600-157">1</span><span class="sxs-lookup"><span data-stu-id="30600-157">1</span></span>        | <span data-ttu-id="30600-158">Nœud de texte</span><span class="sxs-lookup"><span data-stu-id="30600-158">Text node</span></span> | <span data-ttu-id="30600-159">Seconde</span><span class="sxs-lookup"><span data-stu-id="30600-159">Second</span></span> |

<span data-ttu-id="30600-160">Ce résultat est identique au cas précédent, donc aucun problème négatif n’existe.</span><span class="sxs-lookup"><span data-stu-id="30600-160">This outcome is identical to the prior case, so no negative issues exist.</span></span> <span data-ttu-id="30600-161">`someFlag` est `false` sur le deuxième rendu et la sortie est la suivante :</span><span class="sxs-lookup"><span data-stu-id="30600-161">`someFlag` is `false` on the second rendering, and the output is:</span></span>

| <span data-ttu-id="30600-162">Séquence</span><span class="sxs-lookup"><span data-stu-id="30600-162">Sequence</span></span> | <span data-ttu-id="30600-163">Type</span><span class="sxs-lookup"><span data-stu-id="30600-163">Type</span></span>      | <span data-ttu-id="30600-164">Données</span><span class="sxs-lookup"><span data-stu-id="30600-164">Data</span></span>   |
| :------: | --------- | ------ |
| <span data-ttu-id="30600-165">0</span><span class="sxs-lookup"><span data-stu-id="30600-165">0</span></span>        | <span data-ttu-id="30600-166">Nœud de texte</span><span class="sxs-lookup"><span data-stu-id="30600-166">Text node</span></span> | <span data-ttu-id="30600-167">Seconde</span><span class="sxs-lookup"><span data-stu-id="30600-167">Second</span></span> |

<span data-ttu-id="30600-168">Cette fois-ci, l’algorithme diff constate que *deux* modifications ont été apportées, et l’algorithme génère le script Edit suivant :</span><span class="sxs-lookup"><span data-stu-id="30600-168">This time, the diff algorithm sees that *two* changes have occurred, and the algorithm generates the following edit script:</span></span>

* <span data-ttu-id="30600-169">Remplacez la valeur du premier nœud de texte par `Second`.</span><span class="sxs-lookup"><span data-stu-id="30600-169">Change the value of the first text node to `Second`.</span></span>
* <span data-ttu-id="30600-170">Supprimez le deuxième nœud de texte.</span><span class="sxs-lookup"><span data-stu-id="30600-170">Remove the second text node.</span></span>

<span data-ttu-id="30600-171">La génération des numéros de séquence a perdu toutes les informations utiles sur l’emplacement où les `if/else` branches et les boucles étaient présentes dans le code d’origine.</span><span class="sxs-lookup"><span data-stu-id="30600-171">Generating the sequence numbers has lost all the useful information about where the `if/else` branches and loops were present in the original code.</span></span> <span data-ttu-id="30600-172">Il en résulte une comparaison de **deux fois plus longtemps** qu’auparavant.</span><span class="sxs-lookup"><span data-stu-id="30600-172">This results in a diff **twice as long** as before.</span></span>

<span data-ttu-id="30600-173">Il s’agit d’un exemple trivial.</span><span class="sxs-lookup"><span data-stu-id="30600-173">This is a trivial example.</span></span> <span data-ttu-id="30600-174">Dans des cas plus réalistes avec des structures complexes et profondément imbriquées, et surtout avec des boucles, le coût des performances est généralement plus élevé.</span><span class="sxs-lookup"><span data-stu-id="30600-174">In more realistic cases with complex and deeply nested structures, and especially with loops, the performance cost is usually higher.</span></span> <span data-ttu-id="30600-175">Au lieu d’identifier immédiatement les blocs de boucle ou les branches qui ont été insérés ou supprimés, l’algorithme diff doit être recurse profondément dans les arborescences de rendu.</span><span class="sxs-lookup"><span data-stu-id="30600-175">Instead of immediately identifying which loop blocks or branches have been inserted or removed, the diff algorithm has to recurse deeply into the render trees.</span></span> <span data-ttu-id="30600-176">Cela entraîne généralement la création de scripts de modification plus longs, car l’algorithme diff est mal informé de la relation entre les anciennes et les nouvelles structures.</span><span class="sxs-lookup"><span data-stu-id="30600-176">This usually results in having to build longer edit scripts because the diff algorithm is misinformed about how the old and new structures relate to each other.</span></span>

### <a name="guidance-and-conclusions"></a><span data-ttu-id="30600-177">Conseils et conclusions</span><span class="sxs-lookup"><span data-stu-id="30600-177">Guidance and conclusions</span></span>

* <span data-ttu-id="30600-178">Les performances de l’application sont affectées si les numéros séquentiels sont générés dynamiquement.</span><span class="sxs-lookup"><span data-stu-id="30600-178">App performance suffers if sequence numbers are generated dynamically.</span></span>
* <span data-ttu-id="30600-179">L’infrastructure ne peut pas créer ses propres numéros de séquence automatiquement au moment de l’exécution, car les informations nécessaires n’existent pas, sauf si elles sont capturées au moment de la compilation.</span><span class="sxs-lookup"><span data-stu-id="30600-179">The framework can't create its own sequence numbers automatically at runtime because the necessary information doesn't exist unless it's captured at compile time.</span></span>
* <span data-ttu-id="30600-180">N’écrivez pas de longs blocs de `RenderTreeBuilder` logique implémentée manuellement.</span><span class="sxs-lookup"><span data-stu-id="30600-180">Don't write long blocks of manually-implemented `RenderTreeBuilder` logic.</span></span> <span data-ttu-id="30600-181">Préférer les fichiers *. Razor* et permettre au compilateur de gérer les numéros de séquence.</span><span class="sxs-lookup"><span data-stu-id="30600-181">Prefer *.razor* files and allow the compiler to deal with the sequence numbers.</span></span> <span data-ttu-id="30600-182">Si vous ne parvenez pas à éviter une logique de `RenderTreeBuilder` manuelle, fractionnez les blocs de code longs en éléments plus petits encapsulés dans `OpenRegion`/`CloseRegion` appels.</span><span class="sxs-lookup"><span data-stu-id="30600-182">If you're unable to avoid manual `RenderTreeBuilder` logic, split long blocks of code into smaller pieces wrapped in `OpenRegion`/`CloseRegion` calls.</span></span> <span data-ttu-id="30600-183">Chaque région a son propre espace distinct pour les numéros de séquence, ce qui vous permet de redémarrer à partir de zéro (ou tout autre nombre arbitraire) à l’intérieur de chaque région.</span><span class="sxs-lookup"><span data-stu-id="30600-183">Each region has its own separate space of sequence numbers, so you can restart from zero (or any other arbitrary number) inside each region.</span></span>
* <span data-ttu-id="30600-184">Si les numéros de séquence sont codés en dur, l’algorithme diff exige uniquement que les numéros de séquence augmentent dans la valeur.</span><span class="sxs-lookup"><span data-stu-id="30600-184">If sequence numbers are hardcoded, the diff algorithm only requires that sequence numbers increase in value.</span></span> <span data-ttu-id="30600-185">La valeur initiale et les écarts ne sont pas pertinents.</span><span class="sxs-lookup"><span data-stu-id="30600-185">The initial value and gaps are irrelevant.</span></span> <span data-ttu-id="30600-186">Une option légitime consiste à utiliser le numéro de ligne de code comme numéro de séquence, ou à commencer à partir de zéro et à augmenter par des ou des centaines (ou un intervalle de préférence).</span><span class="sxs-lookup"><span data-stu-id="30600-186">One legitimate option is to use the code line number as the sequence number, or start from zero and increase by ones or hundreds (or any preferred interval).</span></span> 
* Blazor<span data-ttu-id="30600-187"> utilise des numéros de séquence, tandis que d’autres infrastructures d’interface utilisateur de comparaison d’arborescence ne les utilisent pas.</span><span class="sxs-lookup"><span data-stu-id="30600-187"> uses sequence numbers, while other tree-diffing UI frameworks don't use them.</span></span> <span data-ttu-id="30600-188">La comparaison est beaucoup plus rapide lorsque les numéros de séquence sont utilisés et Blazor présente l’avantage d’une étape de compilation qui traite automatiquement les numéros séquentiels pour les développeurs qui créent des fichiers *. Razor* .</span><span class="sxs-lookup"><span data-stu-id="30600-188">Diffing is far faster when sequence numbers are used, and Blazor has the advantage of a compile step that deals with sequence numbers automatically for developers authoring *.razor* files.</span></span>
