---
title: Tag Helper Partial dans ASP.NET Core
author: scottaddie
description: Découvrez le Tag Helper Partial ASP.NET Core et le rôle de ses attributs dans le rendu d’une vue partielle.
monikerRange: '>= aspnetcore-2.1'
ms.author: scaddie
ms.custom: mvc
ms.date: 04/13/2018
uid: mvc/views/tag-helpers/builtin-th/partial-tag-helper
ms.openlocfilehash: fea84621f185c4113147cf0dfd173704bc7b6d81
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/20/2018
ms.locfileid: "36274399"
---
# <a name="partial-tag-helper-in-aspnet-core"></a><span data-ttu-id="e09b3-103">Tag Helper Partial dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e09b3-103">Partial Tag Helper in ASP.NET Core</span></span>

<span data-ttu-id="e09b3-104">Par [Scott Addie](https://github.com/scottaddie)</span><span class="sxs-lookup"><span data-stu-id="e09b3-104">By [Scott Addie](https://github.com/scottaddie)</span></span>

<span data-ttu-id="e09b3-105">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/tag-helpers/built-in/samples) ([procédure de téléchargement](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="e09b3-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/tag-helpers/built-in/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="overview"></a><span data-ttu-id="e09b3-106">Vue d'ensemble</span><span class="sxs-lookup"><span data-stu-id="e09b3-106">Overview</span></span>

<span data-ttu-id="e09b3-107">Le Tag Helper Partial est utilisé dans le cadre du rendu d’une [vue partielle](xref:mvc/views/partial) dans les pages Razor et les applications MVC.</span><span class="sxs-lookup"><span data-stu-id="e09b3-107">The Partial Tag Helper is used for rendering a [partial view](xref:mvc/views/partial) in Razor Pages and MVC apps.</span></span> <span data-ttu-id="e09b3-108">Tenez compte des points suivants :</span><span class="sxs-lookup"><span data-stu-id="e09b3-108">Consider that it:</span></span>

* <span data-ttu-id="e09b3-109">Il nécessite ASP.NET Core 2.1 ou ultérieur.</span><span class="sxs-lookup"><span data-stu-id="e09b3-109">Requires ASP.NET Core 2.1 or later.</span></span>
* <span data-ttu-id="e09b3-110">Il constitue une alternative à la [syntaxe HTML Helper](xref:mvc/views/partial#referencing-a-partial-view).</span><span class="sxs-lookup"><span data-stu-id="e09b3-110">Is an alternative to [HTML Helper syntax](xref:mvc/views/partial#referencing-a-partial-view).</span></span>
* <span data-ttu-id="e09b3-111">Il affiche la vue partielle de façon asynchrone.</span><span class="sxs-lookup"><span data-stu-id="e09b3-111">Renders the partial view asynchronously.</span></span>

<span data-ttu-id="e09b3-112">Parmi les options HTML Helper utilisées pour le rendu d’une vue partielle, citons les suivantes :</span><span class="sxs-lookup"><span data-stu-id="e09b3-112">The HTML Helper options for rendering a partial view include:</span></span>

* [<span data-ttu-id="e09b3-113">@await Html.PartialAsync</span><span class="sxs-lookup"><span data-stu-id="e09b3-113">@await Html.PartialAsync</span></span>](/dotnet/api/microsoft.aspnetcore.mvc.rendering.htmlhelperpartialextensions.partialasync)
* [<span data-ttu-id="e09b3-114">@await Html.RenderPartialAsync</span><span class="sxs-lookup"><span data-stu-id="e09b3-114">@await Html.RenderPartialAsync</span></span>](/dotnet/api/microsoft.aspnetcore.mvc.rendering.htmlhelperpartialextensions.renderpartialasync)
* [@Html.Partial](/dotnet/api/microsoft.aspnetcore.mvc.rendering.htmlhelperpartialextensions.partial)
* [@Html.RenderPartial](/dotnet/api/microsoft.aspnetcore.mvc.rendering.htmlhelperpartialextensions.renderpartial)

<span data-ttu-id="e09b3-115">Le modèle *Product* est utilisé dans les exemples tout au long de ce document :</span><span class="sxs-lookup"><span data-stu-id="e09b3-115">The *Product* model is used in samples throughout this document:</span></span>

[!code-csharp[](samples/TagHelpersBuiltIn/Models/Product.cs)]

<span data-ttu-id="e09b3-116">Voici l’inventaire des attributs du Tag Helper Partial.</span><span class="sxs-lookup"><span data-stu-id="e09b3-116">An inventory of the Partial Tag Helper attributes follows.</span></span>

## <a name="name"></a><span data-ttu-id="e09b3-117">name</span><span class="sxs-lookup"><span data-stu-id="e09b3-117">name</span></span>

<span data-ttu-id="e09b3-118">L'attribut `name` est obligatoire.</span><span class="sxs-lookup"><span data-stu-id="e09b3-118">The `name` attribute is required.</span></span> <span data-ttu-id="e09b3-119">Il indique le nom ou le chemin de la vue partielle à afficher.</span><span class="sxs-lookup"><span data-stu-id="e09b3-119">It indicates the name or the path of the partial view to be rendered.</span></span> <span data-ttu-id="e09b3-120">Quand un nom de vue partielle est fourni, le processus de [découverte de vue](xref:mvc/views/overview#view-discovery) est lancé.</span><span class="sxs-lookup"><span data-stu-id="e09b3-120">When a partial view name is provided, the [view discovery](xref:mvc/views/overview#view-discovery) process is initiated.</span></span> <span data-ttu-id="e09b3-121">Ce processus est ignoré quand un chemin explicite est fourni.</span><span class="sxs-lookup"><span data-stu-id="e09b3-121">That process is bypassed when an explicit path is provided.</span></span>

<span data-ttu-id="e09b3-122">Le balisage suivant utilise un chemin explicite indiquant que le fichier *_ProductPartial.cshtml* doit être chargé à partir du dossier *Shared*.</span><span class="sxs-lookup"><span data-stu-id="e09b3-122">The following markup uses an explicit path, indicating that *_ProductPartial.cshtml* is to be loaded from the *Shared* folder.</span></span> <span data-ttu-id="e09b3-123">À l’aide de l’attribut [for](#for), un modèle est passé à la vue partielle pour liaison.</span><span class="sxs-lookup"><span data-stu-id="e09b3-123">Using the [for](#for) attribute, a model is passed to the partial view for binding.</span></span>

[!code-cshtml[](samples/TagHelpersBuiltIn/Pages/Product.cshtml?name=snippet_Name)]

## <a name="for"></a><span data-ttu-id="e09b3-124">for</span><span class="sxs-lookup"><span data-stu-id="e09b3-124">for</span></span>

<span data-ttu-id="e09b3-125">L’attribut `for` affecte un [ModelExpression](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.modelexpression) à évaluer par rapport au modèle actif.</span><span class="sxs-lookup"><span data-stu-id="e09b3-125">The `for` attribute assigns a [ModelExpression](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.modelexpression) to be evaluated against the current model.</span></span> <span data-ttu-id="e09b3-126">`ModelExpression` déduit la syntaxe `@Model.`.</span><span class="sxs-lookup"><span data-stu-id="e09b3-126">A `ModelExpression` infers the `@Model.` syntax.</span></span> <span data-ttu-id="e09b3-127">Par exemple, `for="Product"` peut être utilisé à la place de `for="@Model.Product"`.</span><span class="sxs-lookup"><span data-stu-id="e09b3-127">For example, `for="Product"` can be used instead of `for="@Model.Product"`.</span></span> <span data-ttu-id="e09b3-128">Pour substituer ce comportement d’inférence par défaut, utilisez le symbole `@` pour définir une expression inline.</span><span class="sxs-lookup"><span data-stu-id="e09b3-128">This default inference behavior is overridden by using the `@` symbol to define an inline expression.</span></span> <span data-ttu-id="e09b3-129">L’attribut `for` ne peut pas être utilisé avec l’attribut [model](#model).</span><span class="sxs-lookup"><span data-stu-id="e09b3-129">The `for` attribute can't be used with the [model](#model) attribute.</span></span>

<span data-ttu-id="e09b3-130">Le balisage suivant charge *_ProductPartial.cshtml* :</span><span class="sxs-lookup"><span data-stu-id="e09b3-130">The following markup loads *_ProductPartial.cshtml*:</span></span>

[!code-cshtml[](samples/TagHelpersBuiltIn/Pages/Product.cshtml?name=snippet_For)]

<span data-ttu-id="e09b3-131">La vue partielle est liée à la propriété `Product` du modèle de page associé :</span><span class="sxs-lookup"><span data-stu-id="e09b3-131">The partial view is bound to the associated page model's `Product` property:</span></span>

[!code-csharp[](samples/TagHelpersBuiltIn/Pages/Product.cshtml.cs?highlight=8)]

## <a name="model"></a><span data-ttu-id="e09b3-132">modèle</span><span class="sxs-lookup"><span data-stu-id="e09b3-132">model</span></span>

<span data-ttu-id="e09b3-133">L’attribut `model` affecte une instance de modèle à passer à la vue partielle.</span><span class="sxs-lookup"><span data-stu-id="e09b3-133">The `model` attribute assigns a model instance to pass to the partial view.</span></span> <span data-ttu-id="e09b3-134">L’attribut `model` ne peut pas être utilisé avec l’attribut [for](#for).</span><span class="sxs-lookup"><span data-stu-id="e09b3-134">The `model` attribute can't be used with the [for](#for) attribute.</span></span>

<span data-ttu-id="e09b3-135">Dans le balisage suivant, un nouvel objet `Product` est instancié et passé à l’attribut `model` pour liaison :</span><span class="sxs-lookup"><span data-stu-id="e09b3-135">In the following markup, a new `Product` object is instantiated and passed to the `model` attribute for binding:</span></span>

[!code-cshtml[](samples/TagHelpersBuiltIn/Pages/Product.cshtml?name=snippet_Model)]

## <a name="view-data"></a><span data-ttu-id="e09b3-136">view-data</span><span class="sxs-lookup"><span data-stu-id="e09b3-136">view-data</span></span>

<span data-ttu-id="e09b3-137">L’attribut `view-data` affecte un [ViewDataDictionary](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.viewdatadictionary) à passer à la vue partielle.</span><span class="sxs-lookup"><span data-stu-id="e09b3-137">The `view-data` attribute assigns a [ViewDataDictionary](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.viewdatadictionary) to pass to the partial view.</span></span> <span data-ttu-id="e09b3-138">Le balisage suivant rend l’ensemble de la collection ViewData accessible à la vue partielle :</span><span class="sxs-lookup"><span data-stu-id="e09b3-138">The following markup makes the entire ViewData collection accessible to the partial view:</span></span>

[!code-cshtml[](samples/TagHelpersBuiltIn/Pages/Product.cshtml?name=snippet_ViewData&highlight=5-)]

<span data-ttu-id="e09b3-139">Dans le code précédent, la valeur de clé `IsNumberReadOnly` est `true` et ajoutée à la collection ViewData.</span><span class="sxs-lookup"><span data-stu-id="e09b3-139">In the preceding code, the `IsNumberReadOnly` key value is set to `true` and added to the ViewData collection.</span></span> <span data-ttu-id="e09b3-140">`ViewData["IsNumberReadOnly"]` est donc accessible au sein de la vue partielle suivante :</span><span class="sxs-lookup"><span data-stu-id="e09b3-140">Consequently, `ViewData["IsNumberReadOnly"]` is made accessible within the following partial view:</span></span>

[!code-cshtml[](samples/TagHelpersBuiltIn/Pages/Shared/_ProductViewDataPartial.cshtml?highlight=5)]

<span data-ttu-id="e09b3-141">Dans cet exemple, la valeur de `ViewData["IsNumberReadOnly"]` détermine si le champ *Number* s’affiche en lecture seule.</span><span class="sxs-lookup"><span data-stu-id="e09b3-141">In this example, the value of `ViewData["IsNumberReadOnly"]` determines whether the *Number* field is displayed as read only.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="e09b3-142">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="e09b3-142">Additional resources</span></span>

* [<span data-ttu-id="e09b3-143">Vues partielles</span><span class="sxs-lookup"><span data-stu-id="e09b3-143">Partial views</span></span>](xref:mvc/views/partial)
* [<span data-ttu-id="e09b3-144">Données faiblement typées (ViewData, attribut ViewData et ViewBag)</span><span class="sxs-lookup"><span data-stu-id="e09b3-144">Weakly typed data (ViewData, ViewData attribute, and ViewBag)</span></span>](xref:mvc/views/overview#weakly-typed-data-viewdata-viewdata-attribute-and-viewbag)
