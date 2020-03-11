---
title: Composants ASP.NET Core Blazor basés sur un modèle
author: guardrex
description: Découvrez comment les composants basés sur des modèles peuvent accepter un ou plusieurs modèles d’interface utilisateur en tant que paramètres, qui peuvent ensuite être utilisés dans le cadre de la logique de rendu du composant.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 02/12/2020
no-loc:
- Blazor
- SignalR
uid: blazor/templated-components
ms.openlocfilehash: b64d6a731e540b13c50b2c6108f75efd0ac9290c
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78657716"
---
# <a name="aspnet-core-opno-locblazor-templated-components"></a><span data-ttu-id="b36a4-103">Composants ASP.NET Core Blazor basés sur un modèle</span><span class="sxs-lookup"><span data-stu-id="b36a4-103">ASP.NET Core Blazor templated components</span></span>

<span data-ttu-id="b36a4-104">Par [Luke Latham](https://github.com/guardrex) et [Daniel Roth](https://github.com/danroth27)</span><span class="sxs-lookup"><span data-stu-id="b36a4-104">By [Luke Latham](https://github.com/guardrex) and [Daniel Roth](https://github.com/danroth27)</span></span>

<span data-ttu-id="b36a4-105">Les composants basés sur un modèle sont des composants qui acceptent un ou plusieurs modèles d’interface utilisateur en tant que paramètres, qui peuvent ensuite être utilisés dans le cadre de la logique de rendu du composant.</span><span class="sxs-lookup"><span data-stu-id="b36a4-105">Templated components are components that accept one or more UI templates as parameters, which can then be used as part of the component's rendering logic.</span></span> <span data-ttu-id="b36a4-106">Les composants basés sur un modèle vous permettent de créer des composants de niveau supérieur qui sont plus réutilisables que les composants normaux.</span><span class="sxs-lookup"><span data-stu-id="b36a4-106">Templated components allow you to author higher-level components that are more reusable than regular components.</span></span> <span data-ttu-id="b36a4-107">Voici quelques exemples :</span><span class="sxs-lookup"><span data-stu-id="b36a4-107">A couple of examples include:</span></span>

* <span data-ttu-id="b36a4-108">Composant de table qui permet à un utilisateur de spécifier des modèles pour l’en-tête, les lignes et le pied de page de la table.</span><span class="sxs-lookup"><span data-stu-id="b36a4-108">A table component that allows a user to specify templates for the table's header, rows, and footer.</span></span>
* <span data-ttu-id="b36a4-109">Composant de liste qui permet à un utilisateur de spécifier un modèle pour afficher des éléments dans une liste.</span><span class="sxs-lookup"><span data-stu-id="b36a4-109">A list component that allows a user to specify a template for rendering items in a list.</span></span>

## <a name="template-parameters"></a><span data-ttu-id="b36a4-110">Paramètres de modèle</span><span class="sxs-lookup"><span data-stu-id="b36a4-110">Template parameters</span></span>

<span data-ttu-id="b36a4-111">Un composant basé sur un modèle est défini en spécifiant un ou plusieurs paramètres de composant de type `RenderFragment` ou `RenderFragment<T>`.</span><span class="sxs-lookup"><span data-stu-id="b36a4-111">A templated component is defined by specifying one or more component parameters of type `RenderFragment` or `RenderFragment<T>`.</span></span> <span data-ttu-id="b36a4-112">Un fragment de rendu représente un segment de l’interface utilisateur à restituer.</span><span class="sxs-lookup"><span data-stu-id="b36a4-112">A render fragment represents a segment of UI to render.</span></span> <span data-ttu-id="b36a4-113">`RenderFragment<T>` prend un paramètre de type qui peut être spécifié lors de l’appel du fragment de rendu.</span><span class="sxs-lookup"><span data-stu-id="b36a4-113">`RenderFragment<T>` takes a type parameter that can be specified when the render fragment is invoked.</span></span>

<span data-ttu-id="b36a4-114">composant `TableTemplate` :</span><span class="sxs-lookup"><span data-stu-id="b36a4-114">`TableTemplate` component:</span></span>

[!code-razor[](common/samples/3.x/BlazorWebAssemblySample/Components/TableTemplate.razor)]

<span data-ttu-id="b36a4-115">Lorsque vous utilisez un composant basé sur un modèle, les paramètres de modèle peuvent être spécifiés à l’aide d’éléments enfants qui correspondent aux noms des paramètres (`TableHeader` et `RowTemplate` dans l’exemple suivant) :</span><span class="sxs-lookup"><span data-stu-id="b36a4-115">When using a templated component, the template parameters can be specified using child elements that match the names of the parameters (`TableHeader` and `RowTemplate` in the following example):</span></span>

```razor
<TableTemplate Items="pets">
    <TableHeader>
        <th>ID</th>
        <th>Name</th>
    </TableHeader>
    <RowTemplate>
        <td>@context.PetId</td>
        <td>@context.Name</td>
    </RowTemplate>
</TableTemplate>
```

## <a name="template-context-parameters"></a><span data-ttu-id="b36a4-116">Paramètres de contexte de modèle</span><span class="sxs-lookup"><span data-stu-id="b36a4-116">Template context parameters</span></span>

<span data-ttu-id="b36a4-117">Les arguments de composant de type `RenderFragment<T>` passés comme éléments ont un paramètre implicite nommé `context` (par exemple, à partir de l’exemple de code précédent, `@context.PetId`), mais vous pouvez modifier le nom de paramètre à l’aide de l’attribut `Context` sur l’élément enfant.</span><span class="sxs-lookup"><span data-stu-id="b36a4-117">Component arguments of type `RenderFragment<T>` passed as elements have an implicit parameter named `context` (for example from the preceding code sample, `@context.PetId`), but you can change the parameter name using the `Context` attribute on the child element.</span></span> <span data-ttu-id="b36a4-118">Dans l’exemple suivant, l’attribut `Context` de l’élément `RowTemplate` spécifie le paramètre `pet` :</span><span class="sxs-lookup"><span data-stu-id="b36a4-118">In the following example, the `RowTemplate` element's `Context` attribute specifies the `pet` parameter:</span></span>

```razor
<TableTemplate Items="pets">
    <TableHeader>
        <th>ID</th>
        <th>Name</th>
    </TableHeader>
    <RowTemplate Context="pet">
        <td>@pet.PetId</td>
        <td>@pet.Name</td>
    </RowTemplate>
</TableTemplate>
```

<span data-ttu-id="b36a4-119">Vous pouvez également spécifier l’attribut `Context` sur l’élément Component.</span><span class="sxs-lookup"><span data-stu-id="b36a4-119">Alternatively, you can specify the `Context` attribute on the component element.</span></span> <span data-ttu-id="b36a4-120">L’attribut `Context` spécifié s’applique à tous les paramètres de modèle spécifiés.</span><span class="sxs-lookup"><span data-stu-id="b36a4-120">The specified `Context` attribute applies to all specified template parameters.</span></span> <span data-ttu-id="b36a4-121">Cela peut être utile lorsque vous souhaitez spécifier le nom du paramètre de contenu pour le contenu enfant implicite (sans élément enfant d’encapsulation).</span><span class="sxs-lookup"><span data-stu-id="b36a4-121">This can be useful when you want to specify the content parameter name for implicit child content (without any wrapping child element).</span></span> <span data-ttu-id="b36a4-122">Dans l’exemple suivant, l’attribut `Context` apparaît sur l’élément `TableTemplate` et s’applique à tous les paramètres de modèle :</span><span class="sxs-lookup"><span data-stu-id="b36a4-122">In the following example, the `Context` attribute appears on the `TableTemplate` element and applies to all template parameters:</span></span>

```razor
<TableTemplate Items="pets" Context="pet">
    <TableHeader>
        <th>ID</th>
        <th>Name</th>
    </TableHeader>
    <RowTemplate>
        <td>@pet.PetId</td>
        <td>@pet.Name</td>
    </RowTemplate>
</TableTemplate>
```

## <a name="generic-typed-components"></a><span data-ttu-id="b36a4-123">Composants génériques</span><span class="sxs-lookup"><span data-stu-id="b36a4-123">Generic-typed components</span></span>

<span data-ttu-id="b36a4-124">Les composants basés sur un modèle sont souvent typés de façon générique.</span><span class="sxs-lookup"><span data-stu-id="b36a4-124">Templated components are often generically typed.</span></span> <span data-ttu-id="b36a4-125">Par exemple, un composant générique `ListViewTemplate` peut être utilisé pour restituer des valeurs `IEnumerable<T>`.</span><span class="sxs-lookup"><span data-stu-id="b36a4-125">For example, a generic `ListViewTemplate` component can be used to render `IEnumerable<T>` values.</span></span> <span data-ttu-id="b36a4-126">Pour définir un composant générique, utilisez la directive [`@typeparam`](xref:mvc/views/razor#typeparam) pour spécifier les paramètres de type :</span><span class="sxs-lookup"><span data-stu-id="b36a4-126">To define a generic component, use the [`@typeparam`](xref:mvc/views/razor#typeparam) directive to specify type parameters:</span></span>

[!code-razor[](common/samples/3.x/BlazorWebAssemblySample/Components/ListViewTemplate.razor)]

<span data-ttu-id="b36a4-127">Lorsque vous utilisez des composants génériques, le paramètre de type est déduit si possible :</span><span class="sxs-lookup"><span data-stu-id="b36a4-127">When using generic-typed components, the type parameter is inferred if possible:</span></span>

```razor
<ListViewTemplate Items="pets">
    <ItemTemplate Context="pet">
        <li>@pet.Name</li>
    </ItemTemplate>
</ListViewTemplate>
```

<span data-ttu-id="b36a4-128">Sinon, le paramètre de type doit être spécifié explicitement à l’aide d’un attribut qui correspond au nom du paramètre de type.</span><span class="sxs-lookup"><span data-stu-id="b36a4-128">Otherwise, the type parameter must be explicitly specified using an attribute that matches the name of the type parameter.</span></span> <span data-ttu-id="b36a4-129">Dans l’exemple suivant, `TItem="Pet"` spécifie le type :</span><span class="sxs-lookup"><span data-stu-id="b36a4-129">In the following example, `TItem="Pet"` specifies the type:</span></span>

```razor
<ListViewTemplate Items="pets" TItem="Pet">
    <ItemTemplate Context="pet">
        <li>@pet.Name</li>
    </ItemTemplate>
</ListViewTemplate>
```
