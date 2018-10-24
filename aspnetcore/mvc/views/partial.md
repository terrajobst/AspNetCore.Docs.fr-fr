---
title: Vues partielles dans ASP.NET Core
author: ardalis
description: Découvrez comment utiliser les vues partielles pour découper des fichiers de balisage volumineux et réduire la duplication du balisage commun entre les différentes pages web dans les applications ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 09/11/2018
uid: mvc/views/partial
ms.openlocfilehash: a836ed073dfe769fc3cc0cd0622b17937747928b
ms.sourcegitcommit: 70fb7c9d5f2ddfcf4747382a9f7159feca7a6aa7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/14/2018
ms.locfileid: "45601754"
---
# <a name="partial-views-in-aspnet-core"></a><span data-ttu-id="2e272-103">Vues partielles dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="2e272-103">Partial views in ASP.NET Core</span></span>

<span data-ttu-id="2e272-104">Par [Steve Smith](https://ardalis.com/), [Luke Latham](https://github.com/guardrex), [Maher JENDOUBI](https://twitter.com/maherjend), [Rick Anderson](https://twitter.com/RickAndMSFT) et [Scott Sauber](https://twitter.com/scottsauber)</span><span class="sxs-lookup"><span data-stu-id="2e272-104">By [Steve Smith](https://ardalis.com/), [Luke Latham](https://github.com/guardrex), [Maher JENDOUBI](https://twitter.com/maherjend), [Rick Anderson](https://twitter.com/RickAndMSFT), and [Scott Sauber](https://twitter.com/scottsauber)</span></span>

<span data-ttu-id="2e272-105">Une vue partielle est un fichier de balisage [Razor](xref:mvc/views/razor) (*.cshtml*) qui effectue le rendu d’une sortie HTML *dans* la sortie rendue d’un autre fichier de balisage.</span><span class="sxs-lookup"><span data-stu-id="2e272-105">A partial view is a [Razor](xref:mvc/views/razor) markup file (*.cshtml*) that renders HTML output *within* another markup file's rendered output.</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="2e272-106">Le terme *vue partielle* s’emploie dans le cadre du développement d’une application MVC, où les fichiers de balisage sont appelés *vues*, ou du développement d’une application Razor Pages, où les fichiers de balisage sont appelés *pages*.</span><span class="sxs-lookup"><span data-stu-id="2e272-106">The term *partial view* is used when developing either an MVC app, where markup files are called *views*, or a Razor Pages app, where markup files are called *pages*.</span></span> <span data-ttu-id="2e272-107">Cette rubrique désigne les vues MVC et les pages Razor Pages avec le terme générique *fichiers de balisage*.</span><span class="sxs-lookup"><span data-stu-id="2e272-107">This topic generically refers to MVC views and Razor Pages pages as *markup files*.</span></span>

::: moniker-end

<span data-ttu-id="2e272-108">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/partial/sample) ([procédure de téléchargement](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="2e272-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/partial/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="when-to-use-partial-views"></a><span data-ttu-id="2e272-109">Quand utiliser des vues partielles ?</span><span class="sxs-lookup"><span data-stu-id="2e272-109">When to use partial views</span></span>

<span data-ttu-id="2e272-110">Les vues partielles sont utiles pour :</span><span class="sxs-lookup"><span data-stu-id="2e272-110">Partial views are an effective way to:</span></span>

* <span data-ttu-id="2e272-111">Découper des fichiers de balisage volumineux en composants plus petits.</span><span class="sxs-lookup"><span data-stu-id="2e272-111">Break up large markup files into smaller components.</span></span>

  <span data-ttu-id="2e272-112">Dans un grand fichier de balisage complexe composé de plusieurs parties logiques, il peut s’avérer utile de travailler sur chaque partie isolée dans une vue partielle.</span><span class="sxs-lookup"><span data-stu-id="2e272-112">In a large, complex markup file composed of several logical pieces, there's an advantage to working with each piece isolated into a partial view.</span></span> <span data-ttu-id="2e272-113">Le code dans le fichier de balisage est facile à gérer, puisqu’il contient uniquement la structure de page globale et les références aux vues partielles.</span><span class="sxs-lookup"><span data-stu-id="2e272-113">The code in the markup file is manageable because the markup only contains the overall page structure and references to partial views.</span></span>
* <span data-ttu-id="2e272-114">Réduire la duplication du contenu de balisage commun entre les fichiers de balisage.</span><span class="sxs-lookup"><span data-stu-id="2e272-114">Reduce the duplication of common markup content across markup files.</span></span>

  <span data-ttu-id="2e272-115">Quand les mêmes éléments de balisage sont utilisés entre plusieurs fichiers de balisage, une vue partielle supprime la duplication du contenu de balisage dans un seul fichier de vue partielle.</span><span class="sxs-lookup"><span data-stu-id="2e272-115">When the same markup elements are used across markup files, a partial view removes the duplication of markup content into one partial view file.</span></span> <span data-ttu-id="2e272-116">Quand le balisage est modifié dans la vue partielle, il met à jour la sortie rendue des fichiers de balisage qui utilisent cette vue partielle.</span><span class="sxs-lookup"><span data-stu-id="2e272-116">When the markup is changed in the partial view, it updates the rendered output of the markup files that use the partial view.</span></span>

<span data-ttu-id="2e272-117">Les vues partielles ne doivent pas être utilisées pour tenir à jour des éléments de disposition communs.</span><span class="sxs-lookup"><span data-stu-id="2e272-117">Partial views shouldn't be used to maintain common layout elements.</span></span> <span data-ttu-id="2e272-118">Ces éléments doivent être spécifiés dans des fichiers [_Layout.cshtml](xref:mvc/views/layout).</span><span class="sxs-lookup"><span data-stu-id="2e272-118">Common layout elements should be specified in [_Layout.cshtml](xref:mvc/views/layout) files.</span></span>

<span data-ttu-id="2e272-119">N’utilisez pas une vue partielle quand du code ou une logique de rendu complexe doit être exécuté pour effectuer le rendu du balisage.</span><span class="sxs-lookup"><span data-stu-id="2e272-119">Don't use a partial view where complex rendering logic or code execution is required to render the markup.</span></span> <span data-ttu-id="2e272-120">Dans ce cas, utilisez un [composant de vue](xref:mvc/views/view-components) à la place.</span><span class="sxs-lookup"><span data-stu-id="2e272-120">Instead of a partial view, use a [view component](xref:mvc/views/view-components).</span></span>

## <a name="declare-partial-views"></a><span data-ttu-id="2e272-121">Déclarer des vues partielles</span><span class="sxs-lookup"><span data-stu-id="2e272-121">Declare partial views</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="2e272-122">Une vue partielle est un fichier de balisage *.cshtml* tenu à jour dans le dossier *Vues* (MVC) ou le dossier *Pages* (Razor Pages).</span><span class="sxs-lookup"><span data-stu-id="2e272-122">A partial view is a *.cshtml* markup file maintained within the *Views* folder (MVC) or *Pages* folder (Razor Pages).</span></span>

<span data-ttu-id="2e272-123">Dans ASP.NET Core MVC, le <xref:Microsoft.AspNetCore.Mvc.ViewResult> d’un contrôleur peut retourner une vue ou une vue partielle.</span><span class="sxs-lookup"><span data-stu-id="2e272-123">In ASP.NET Core MVC, a controller's <xref:Microsoft.AspNetCore.Mvc.ViewResult> is capable of returning either a view or a partial view.</span></span> <span data-ttu-id="2e272-124">Une fonctionnalité similaire est prévue pour Razor Pages dans ASP.NET Core 2.2.</span><span class="sxs-lookup"><span data-stu-id="2e272-124">An analogous capability is planned for Razor Pages in ASP.NET Core 2.2.</span></span> <span data-ttu-id="2e272-125">Dans Razor Pages, un <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel> peut retourner un <xref:Microsoft.AspNetCore.Mvc.PartialViewResult>.</span><span class="sxs-lookup"><span data-stu-id="2e272-125">In Razor Pages, a <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel> can return a <xref:Microsoft.AspNetCore.Mvc.PartialViewResult>.</span></span> <span data-ttu-id="2e272-126">Le référencement et le rendu des vues partielles sont décrits dans la section [Référencer une vue partielle](#reference-a-partial-view).</span><span class="sxs-lookup"><span data-stu-id="2e272-126">Referencing and rendering partial views is described in the [Reference a partial view](#reference-a-partial-view) section.</span></span>

<span data-ttu-id="2e272-127">Contrairement au rendu d’une vue MVC ou d’une page, une vue partielle n’exécute pas *_ViewStart.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="2e272-127">Unlike MVC view or page rendering, a partial view doesn't run *_ViewStart.cshtml*.</span></span> <span data-ttu-id="2e272-128">Pour plus d’informations sur *_ViewStart.cshtml*, consultez <xref:mvc/views/layout>.</span><span class="sxs-lookup"><span data-stu-id="2e272-128">For more information on *_ViewStart.cshtml*, see <xref:mvc/views/layout>.</span></span>

<span data-ttu-id="2e272-129">Les noms de fichiers des vues partielles commencent souvent par un trait de soulignement (`_`).</span><span class="sxs-lookup"><span data-stu-id="2e272-129">Partial view file names often begin with an underscore (`_`).</span></span> <span data-ttu-id="2e272-130">Cette convention de nommage n’est pas obligatoire, mais elle aide à différencier visuellement les vues partielles des autres vues et pages.</span><span class="sxs-lookup"><span data-stu-id="2e272-130">This naming convention isn't required, but it helps to visually differentiate partial views from views and pages.</span></span> <span data-ttu-id="2e272-131">Quand le nom de fichier commence par un trait de soulignement, Razor Pages ne traite pas le fichier de balisage comme une page Razor Pages, même si le balisage du fichier contient la directive `@page`.</span><span class="sxs-lookup"><span data-stu-id="2e272-131">When the file name starts with an underscore, Razor Pages doesn't process the markup file as a Razor Pages page, even when the file's markup includes the `@page` directive.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.1"

<span data-ttu-id="2e272-132">Une vue partielle est un fichier de balisage *.cshtml* tenu à jour dans le dossier *Vues*.</span><span class="sxs-lookup"><span data-stu-id="2e272-132">A partial view is a *.cshtml* markup file maintained within the *Views* folder.</span></span>

<span data-ttu-id="2e272-133">Le <xref:Microsoft.AspNetCore.Mvc.ViewResult> d’un contrôleur peut retourner une vue ou une vue partielle.</span><span class="sxs-lookup"><span data-stu-id="2e272-133">A controller's <xref:Microsoft.AspNetCore.Mvc.ViewResult> is capable of returning either a view or a partial view.</span></span>

<span data-ttu-id="2e272-134">Contrairement au rendu d’une vue MVC, une vue partielle n’exécute pas *_ViewStart.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="2e272-134">Unlike MVC view rendering, a partial view doesn't run *_ViewStart.cshtml*.</span></span> <span data-ttu-id="2e272-135">Pour plus d’informations sur *_ViewStart.cshtml*, consultez <xref:mvc/views/layout>.</span><span class="sxs-lookup"><span data-stu-id="2e272-135">For more information on *_ViewStart.cshtml*, see <xref:mvc/views/layout>.</span></span>

<span data-ttu-id="2e272-136">Les noms de fichiers des vues partielles commencent souvent par un trait de soulignement (`_`).</span><span class="sxs-lookup"><span data-stu-id="2e272-136">Partial view file names often begin with an underscore (`_`).</span></span> <span data-ttu-id="2e272-137">Cette convention de nommage n’est pas obligatoire, mais elle aide à différencier visuellement les vues partielles des autres vues.</span><span class="sxs-lookup"><span data-stu-id="2e272-137">This naming convention isn't required, but it helps to visually differentiate partial views from views.</span></span>

::: moniker-end

## <a name="reference-a-partial-view"></a><span data-ttu-id="2e272-138">Référencer une vue partielle</span><span class="sxs-lookup"><span data-stu-id="2e272-138">Reference a partial view</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="2e272-139">Il y a plusieurs façons de référencer une vue partielle au sein d’un fichier de balisage.</span><span class="sxs-lookup"><span data-stu-id="2e272-139">Within a markup file, there are several ways to reference a partial view.</span></span> <span data-ttu-id="2e272-140">Pour vos applications, nous vous recommandons de choisir l’une des approches de rendu asynchrone suivantes :</span><span class="sxs-lookup"><span data-stu-id="2e272-140">We recommend that apps use one of the following asynchronous rendering approaches:</span></span>

* [<span data-ttu-id="2e272-141">Tag Helper Partial</span><span class="sxs-lookup"><span data-stu-id="2e272-141">Partial Tag Helper</span></span>](#partial-tag-helper)
* [<span data-ttu-id="2e272-142">Helper HTML asynchrone</span><span class="sxs-lookup"><span data-stu-id="2e272-142">Asynchronous HTML Helper</span></span>](#asynchronous-html-helper)

::: moniker-end

::: moniker range="< aspnetcore-2.1"

<span data-ttu-id="2e272-143">Il y a deux façons de référencer une vue partielle au sein d’un fichier de balisage :</span><span class="sxs-lookup"><span data-stu-id="2e272-143">Within a markup file, there are two ways to reference a partial view:</span></span>

* [<span data-ttu-id="2e272-144">Helper HTML asynchrone</span><span class="sxs-lookup"><span data-stu-id="2e272-144">Asynchronous HTML Helper</span></span>](#asynchronous-html-helper)
* [<span data-ttu-id="2e272-145">Helper HTML synchrone</span><span class="sxs-lookup"><span data-stu-id="2e272-145">Synchronous HTML Helper</span></span>](#synchronous-html-helper)

<span data-ttu-id="2e272-146">Pour vos applications, nous vous recommandons d’utiliser le [Helper HTML asynchrone](#asynchronous-html-helper).</span><span class="sxs-lookup"><span data-stu-id="2e272-146">We recommend that apps use the [Asynchronous HTML Helper](#asynchronous-html-helper).</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

### <a name="partial-tag-helper"></a><span data-ttu-id="2e272-147">Tag Helper Partial</span><span class="sxs-lookup"><span data-stu-id="2e272-147">Partial Tag Helper</span></span>

<span data-ttu-id="2e272-148">Le [Tag Helper Partial](xref:mvc/views/tag-helpers/builtin-th/partial-tag-helper) nécessite ASP.NET Core 2.1 ou version ultérieure.</span><span class="sxs-lookup"><span data-stu-id="2e272-148">The [Partial Tag Helper](xref:mvc/views/tag-helpers/builtin-th/partial-tag-helper) requires ASP.NET Core 2.1 or later.</span></span>

<span data-ttu-id="2e272-149">Le Tag Helper Partial effectue un rendu asynchrone et utilise une syntaxe de type HTML :</span><span class="sxs-lookup"><span data-stu-id="2e272-149">The Partial Tag Helper renders content asynchronously and uses an HTML-like syntax:</span></span>

```cshtml
<partial name="_PartialName" />
```

<span data-ttu-id="2e272-150">Si une extension de fichier est spécifiée, la vue partielle référencée par le Tag Helper doit se trouver dans le même dossier que le fichier de balisage qui appelle la vue partielle :</span><span class="sxs-lookup"><span data-stu-id="2e272-150">When a file extension is present, the Tag Helper references a partial view that must be in the same folder as the markup file calling the partial view:</span></span>

```cshtml
<partial name="_PartialName.cshtml" />
```

<span data-ttu-id="2e272-151">L’exemple suivant référence une vue partielle à la racine de l’application.</span><span class="sxs-lookup"><span data-stu-id="2e272-151">The following example references a partial view from the app root.</span></span> <span data-ttu-id="2e272-152">Les chemins qui commencent par un tilde et une barre oblique (`~/`) ou par une barre oblique seule (`/`) renvoient à la racine de l’application :</span><span class="sxs-lookup"><span data-stu-id="2e272-152">Paths that start with a tilde-slash (`~/`) or a slash (`/`) refer to the app root:</span></span>

<span data-ttu-id="2e272-153">**Razor Pages**</span><span class="sxs-lookup"><span data-stu-id="2e272-153">**Razor Pages**</span></span>

```cshtml
<partial name="~/Pages/Folder/_PartialName.cshtml" />
<partial name="/Pages/Folder/_PartialName.cshtml" />
```

<span data-ttu-id="2e272-154">**MVC**</span><span class="sxs-lookup"><span data-stu-id="2e272-154">**MVC**</span></span>

```cshtml
<partial name="~/Views/Folder/_PartialName.cshtml" />
<partial name="/Views/Folder/_PartialName.cshtml" />
```

<span data-ttu-id="2e272-155">L’exemple suivant référence une vue partielle avec un chemin relatif :</span><span class="sxs-lookup"><span data-stu-id="2e272-155">The following example references a partial view with a relative path:</span></span>

```cshtml
<partial name="../Account/_PartialName.cshtml" />
```

<span data-ttu-id="2e272-156">Pour plus d'informations, consultez <xref:mvc/views/tag-helpers/builtin-th/partial-tag-helper>.</span><span class="sxs-lookup"><span data-stu-id="2e272-156">For more information, see <xref:mvc/views/tag-helpers/builtin-th/partial-tag-helper>.</span></span>

::: moniker-end

### <a name="asynchronous-html-helper"></a><span data-ttu-id="2e272-157">Helper HTML asynchrone</span><span class="sxs-lookup"><span data-stu-id="2e272-157">Asynchronous HTML Helper</span></span>

<span data-ttu-id="2e272-158">Avec un Helper HTML, la bonne pratique est d’utiliser <xref:Microsoft.AspNetCore.Mvc.Rendering.HtmlHelperPartialExtensions.PartialAsync*>.</span><span class="sxs-lookup"><span data-stu-id="2e272-158">When using an HTML Helper, the best practice is to use <xref:Microsoft.AspNetCore.Mvc.Rendering.HtmlHelperPartialExtensions.PartialAsync*>.</span></span> <span data-ttu-id="2e272-159">`PartialAsync` retourne un type <xref:Microsoft.AspNetCore.Html.IHtmlContent> wrappé dans un <xref:System.Threading.Tasks.Task`1>.</span><span class="sxs-lookup"><span data-stu-id="2e272-159">`PartialAsync` returns an <xref:Microsoft.AspNetCore.Html.IHtmlContent> type wrapped in a <xref:System.Threading.Tasks.Task`1>.</span></span> <span data-ttu-id="2e272-160">La méthode est référencée par l’ajout du préfixe `@` à l’appel attendu :</span><span class="sxs-lookup"><span data-stu-id="2e272-160">The method is referenced by prefixing the awaited call with an `@` character:</span></span>

```cshtml
@await Html.PartialAsync("_PartialName")
```

<span data-ttu-id="2e272-161">Si l’extension de fichier est spécifiée, la vue partielle référencée par le Helper HTML doit se trouver dans le même dossier que le fichier de balisage qui appelle la vue partielle :</span><span class="sxs-lookup"><span data-stu-id="2e272-161">When the file extension is present, the HTML Helper references a partial view that must be in the same folder as the markup file calling the partial view:</span></span>

```cshtml
@await Html.PartialAsync("_PartialName.cshtml")
```

<span data-ttu-id="2e272-162">L’exemple suivant référence une vue partielle à la racine de l’application.</span><span class="sxs-lookup"><span data-stu-id="2e272-162">The following example references a partial view from the app root.</span></span> <span data-ttu-id="2e272-163">Les chemins qui commencent par un tilde et une barre oblique (`~/`) ou par une barre oblique seule (`/`) renvoient à la racine de l’application :</span><span class="sxs-lookup"><span data-stu-id="2e272-163">Paths that start with a tilde-slash (`~/`) or a slash (`/`) refer to the app root:</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="2e272-164">**Pages Razor**</span><span class="sxs-lookup"><span data-stu-id="2e272-164">**Razor Pages**</span></span>

```cshtml
@await Html.PartialAsync("~/Pages/Folder/_PartialName.cshtml")
@await Html.PartialAsync("/Pages/Folder/_PartialName.cshtml")
```

<span data-ttu-id="2e272-165">**MVC**</span><span class="sxs-lookup"><span data-stu-id="2e272-165">**MVC**</span></span>

::: moniker-end

```cshtml
@await Html.PartialAsync("~/Views/Folder/_PartialName.cshtml")
@await Html.PartialAsync("/Views/Folder/_PartialName.cshtml")
```

<span data-ttu-id="2e272-166">L’exemple suivant référence une vue partielle avec un chemin relatif :</span><span class="sxs-lookup"><span data-stu-id="2e272-166">The following example references a partial view with a relative path:</span></span>

```cshtml
@await Html.PartialAsync("../Account/_LoginPartial.cshtml")
```

<span data-ttu-id="2e272-167">Vous pouvez aussi effectuer le rendu d’une vue partielle avec <xref:Microsoft.AspNetCore.Mvc.Rendering.HtmlHelperPartialExtensions.RenderPartialAsync*>.</span><span class="sxs-lookup"><span data-stu-id="2e272-167">Alternatively, you can render a partial view with <xref:Microsoft.AspNetCore.Mvc.Rendering.HtmlHelperPartialExtensions.RenderPartialAsync*>.</span></span> <span data-ttu-id="2e272-168">Cette méthode ne retourne aucun <xref:Microsoft.AspNetCore.Html.IHtmlContent>.</span><span class="sxs-lookup"><span data-stu-id="2e272-168">This method doesn't return an <xref:Microsoft.AspNetCore.Html.IHtmlContent>.</span></span> <span data-ttu-id="2e272-169">Elle envoie la sortie rendue directement à la réponse.</span><span class="sxs-lookup"><span data-stu-id="2e272-169">It streams the rendered output directly to the response.</span></span> <span data-ttu-id="2e272-170">Comme elle ne retourne aucun résultat, cette méthode doit être appelée dans un bloc de code Razor :</span><span class="sxs-lookup"><span data-stu-id="2e272-170">Because the method doesn't return a result, it must be called within a Razor code block:</span></span>

[!code-cshtml[](partial/sample/PartialViewsSample/Views/Home/Discovery.cshtml?name=snippet_RenderPartialAsync)]

<span data-ttu-id="2e272-171">Dans la mesure où elle envoie directement le contenu rendu, la méthode `RenderPartialAsync` est plus efficace dans certains scénarios.</span><span class="sxs-lookup"><span data-stu-id="2e272-171">Since `RenderPartialAsync` streams rendered content, it provides better performance in some scenarios.</span></span> <span data-ttu-id="2e272-172">Dans les scénarios nécessitant de hautes performances, effectuez un test d’évaluation de la page à l’aide de ces deux approches et choisissez celle qui génère une réponse plus rapidement.</span><span class="sxs-lookup"><span data-stu-id="2e272-172">In performance-critical situations, benchmark the page using both approaches and use the approach that generates a faster response.</span></span>

### <a name="synchronous-html-helper"></a><span data-ttu-id="2e272-173">Helper HTML synchrone</span><span class="sxs-lookup"><span data-stu-id="2e272-173">Synchronous HTML Helper</span></span>

<span data-ttu-id="2e272-174"><xref:Microsoft.AspNetCore.Mvc.Rendering.HtmlHelperPartialExtensions.Partial*> et <xref:Microsoft.AspNetCore.Mvc.Rendering.HtmlHelperPartialExtensions.RenderPartial*> sont les équivalents synchrones de `PartialAsync` et `RenderPartialAsync`, respectivement.</span><span class="sxs-lookup"><span data-stu-id="2e272-174"><xref:Microsoft.AspNetCore.Mvc.Rendering.HtmlHelperPartialExtensions.Partial*> and <xref:Microsoft.AspNetCore.Mvc.Rendering.HtmlHelperPartialExtensions.RenderPartial*> are the synchronous equivalents of `PartialAsync` and `RenderPartialAsync`, respectively.</span></span> <span data-ttu-id="2e272-175">Les équivalents synchrones ne sont pas recommandés en raison du risque d’interblocage dans certains scénarios.</span><span class="sxs-lookup"><span data-stu-id="2e272-175">The synchronous equivalents aren't recommended because there are scenarios in which they deadlock.</span></span> <span data-ttu-id="2e272-176">Les méthodes synchrones sont prévues d’être supprimées dans une version ultérieure.</span><span class="sxs-lookup"><span data-stu-id="2e272-176">The synchronous methods are targeted for removal in a future release.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="2e272-177">Si vous devez exécuter du code, utilisez un [composant de vue](xref:mvc/views/view-components) au lieu d’une vue partielle.</span><span class="sxs-lookup"><span data-stu-id="2e272-177">If you need to execute code, use a [view component](xref:mvc/views/view-components) instead of a partial view.</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="2e272-178">L’appel de `Partial` ou `RenderPartial` génère un avertissement de l’analyseur Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="2e272-178">Calling `Partial` or `RenderPartial` results in a Visual Studio analyzer warning.</span></span> <span data-ttu-id="2e272-179">Par exemple, la présence de `Partial` génère le message d’avertissement suivant :</span><span class="sxs-lookup"><span data-stu-id="2e272-179">For example, the presence of `Partial` yields the following warning message:</span></span>

> <span data-ttu-id="2e272-180">L’utilisation de IHtmlHelper.Partial peut entraîner des interblocages d’application.</span><span class="sxs-lookup"><span data-stu-id="2e272-180">Use of IHtmlHelper.Partial may result in application deadlocks.</span></span> <span data-ttu-id="2e272-181">Utilisez plutôt un Tag Helper &lt;Partial&gt; ou IHtmlHelper.PartialAsync.</span><span class="sxs-lookup"><span data-stu-id="2e272-181">Consider using &lt;partial&gt; Tag Helper or IHtmlHelper.PartialAsync.</span></span>

<span data-ttu-id="2e272-182">Remplacez les appels à `@Html.Partial` par `@await Html.PartialAsync` ou le [Tag Helper Partial](xref:mvc/views/tag-helpers/builtin-th/partial-tag-helper).</span><span class="sxs-lookup"><span data-stu-id="2e272-182">Replace calls to `@Html.Partial` with `@await Html.PartialAsync` or the [Partial Tag Helper](xref:mvc/views/tag-helpers/builtin-th/partial-tag-helper).</span></span> <span data-ttu-id="2e272-183">Pour plus d’informations sur la migration du Tag Helper Partial, consultez [Migrer à partir d’une assistance HTML](xref:mvc/views/tag-helpers/builtin-th/partial-tag-helper#migrate-from-an-html-helper).</span><span class="sxs-lookup"><span data-stu-id="2e272-183">For more information on Partial Tag Helper migration, see [Migrate from an HTML Helper](xref:mvc/views/tag-helpers/builtin-th/partial-tag-helper#migrate-from-an-html-helper).</span></span>

::: moniker-end

## <a name="partial-view-discovery"></a><span data-ttu-id="2e272-184">Découverte des vues partielles</span><span class="sxs-lookup"><span data-stu-id="2e272-184">Partial view discovery</span></span>

<span data-ttu-id="2e272-185">Quand une vue partielle est référencée par son nom sans extension de fichier, les emplacements suivants sont recherchés dans l’ordre indiqué :</span><span class="sxs-lookup"><span data-stu-id="2e272-185">When a partial view is referenced by name without a file extension, the following locations are searched in the stated order:</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="2e272-186">**Razor Pages**</span><span class="sxs-lookup"><span data-stu-id="2e272-186">**Razor Pages**</span></span>

1. <span data-ttu-id="2e272-187">Dossier de la page en cours d’exécution</span><span class="sxs-lookup"><span data-stu-id="2e272-187">Currently executing page's folder</span></span>
1. <span data-ttu-id="2e272-188">Directory Graph au-dessus du dossier de la page</span><span class="sxs-lookup"><span data-stu-id="2e272-188">Directory graph above the page's folder</span></span>
1. `/Shared`
1. `/Pages/Shared`
1. `/Views/Shared`

<span data-ttu-id="2e272-189">**MVC**</span><span class="sxs-lookup"><span data-stu-id="2e272-189">**MVC**</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

1. `/Areas/<Area-Name>/Views/<Controller-Name>`
1. `/Areas/<Area-Name>/Views/Shared`
1. `/Views/Shared`
1. `/Pages/Shared`

::: moniker-end

::: moniker range="< aspnetcore-2.0"

1. `/Areas/<Area-Name>/Views/<Controller-Name>`
1. `/Areas/<Area-Name>/Views/Shared`
1. `/Views/Shared`

::: moniker-end

<span data-ttu-id="2e272-190">Les conventions suivantes s’appliquent à la détection des vues partielles :</span><span class="sxs-lookup"><span data-stu-id="2e272-190">The following conventions apply to partial view discovery:</span></span>

* <span data-ttu-id="2e272-191">Vous pouvez avoir plusieurs vues partielles avec le même nom de fichier à condition que les vues partielles se trouvent dans des dossiers distincts.</span><span class="sxs-lookup"><span data-stu-id="2e272-191">Different partial views with the same file name are allowed when the partial views are in different folders.</span></span>
* <span data-ttu-id="2e272-192">Quand vous référencez une vue partielle par son nom (sans extension de fichier) et que la vue partielle est présente à la fois dans le dossier de l’appelant et le dossier *Partagé*, la vue partielle fournie est celle du dossier de l’appelant.</span><span class="sxs-lookup"><span data-stu-id="2e272-192">When referencing a partial view by name without a file extension and the partial view is present in both the caller's folder and the *Shared* folder, the partial view in the caller's folder supplies the partial view.</span></span> <span data-ttu-id="2e272-193">Si la vue partielle n’est pas présente dans le dossier de l’appelant, la vue partielle fournie est celle du dossier *Partagé*.</span><span class="sxs-lookup"><span data-stu-id="2e272-193">If the partial view isn't present in the caller's folder, the partial view is provided from the *Shared* folder.</span></span> <span data-ttu-id="2e272-194">Les vues partielles dans le dossier *Partagé* sont appelées *vues partielles partagées* ou *vues partielles par défaut*.</span><span class="sxs-lookup"><span data-stu-id="2e272-194">Partial views in the *Shared* folder are called *shared partial views* or *default partial views*.</span></span>
* <span data-ttu-id="2e272-195">Les vues partielles peuvent être *chaînées*, c’est-à-dire qu’une vue partielle peut appeler une autre vue partielle si une référence circulaire n’est pas formée par les appels.</span><span class="sxs-lookup"><span data-stu-id="2e272-195">Partial views can be *chained*&mdash;a partial view can call another partial view if a circular reference isn't formed by the calls.</span></span> <span data-ttu-id="2e272-196">Les chemins relatifs sont toujours relatifs au fichier actuel, et non au fichier racine ou parent associé.</span><span class="sxs-lookup"><span data-stu-id="2e272-196">Relative paths are always relative to the current file, not to the root or parent of the file.</span></span>

> [!NOTE]
> <span data-ttu-id="2e272-197">Une `section` [Razor](xref:mvc/views/razor) définie dans une vue partielle n’est pas visible par les fichiers de balisage parents.</span><span class="sxs-lookup"><span data-stu-id="2e272-197">A [Razor](xref:mvc/views/razor) `section` defined in a partial view is invisible to parent markup files.</span></span> <span data-ttu-id="2e272-198">La `section` est visible uniquement par la vue partielle dans laquelle elle est définie.</span><span class="sxs-lookup"><span data-stu-id="2e272-198">The `section` is only visible to the partial view in which it's defined.</span></span>

## <a name="access-data-from-partial-views"></a><span data-ttu-id="2e272-199">Accéder à des données à partir de vues partielles</span><span class="sxs-lookup"><span data-stu-id="2e272-199">Access data from partial views</span></span>

<span data-ttu-id="2e272-200">Quand une vue partielle est instanciée, elle reçoit une *copie* du dictionnaire `ViewData` du parent.</span><span class="sxs-lookup"><span data-stu-id="2e272-200">When a partial view is instantiated, it receives a *copy* of the parent's `ViewData` dictionary.</span></span> <span data-ttu-id="2e272-201">Les mises à jour apportées aux données de la vue partielle ne sont pas conservées dans la vue parente.</span><span class="sxs-lookup"><span data-stu-id="2e272-201">Updates made to the data within the partial view aren't persisted to the parent view.</span></span> <span data-ttu-id="2e272-202">Tout changement apporté à `ViewData` dans une vue partielle est perdu quand la vue partielle est retournée.</span><span class="sxs-lookup"><span data-stu-id="2e272-202">`ViewData` changes in a partial view are lost when the partial view returns.</span></span>

<span data-ttu-id="2e272-203">L’exemple suivant montre comment passer une instance de [ViewDataDictionary](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.viewdatadictionary) à une vue partielle :</span><span class="sxs-lookup"><span data-stu-id="2e272-203">The following example demonstrates how to pass an instance of [ViewDataDictionary](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.viewdatadictionary) to a partial view:</span></span>

```cshtml
@await Html.PartialAsync("_PartialName", customViewData)
```

<span data-ttu-id="2e272-204">Vous pouvez passer un modèle dans une vue partielle.</span><span class="sxs-lookup"><span data-stu-id="2e272-204">You can pass a model into a partial view.</span></span> <span data-ttu-id="2e272-205">Le modèle peut être un objet personnalisé.</span><span class="sxs-lookup"><span data-stu-id="2e272-205">The model can be a custom object.</span></span> <span data-ttu-id="2e272-206">Vous pouvez passer un modèle avec `PartialAsync` (envoie le rendu d’un bloc de contenu à l’appelant) ou avec `RenderPartialAsync` (transmet le contenu à la sortie) :</span><span class="sxs-lookup"><span data-stu-id="2e272-206">You can pass a model with `PartialAsync` (renders a block of content to the caller) or `RenderPartialAsync` (streams the content to the output):</span></span>

```cshtml
@await Html.PartialAsync("_PartialName", model)
```

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="2e272-207">**Razor Pages**</span><span class="sxs-lookup"><span data-stu-id="2e272-207">**Razor Pages**</span></span>

<span data-ttu-id="2e272-208">Le balisage suivant dans l’exemple d’application est extrait de la page *Pages/ArticlesRP/ReadRP.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="2e272-208">The following markup in the sample app is from the *Pages/ArticlesRP/ReadRP.cshtml* page.</span></span> <span data-ttu-id="2e272-209">La page contient deux vues partielles.</span><span class="sxs-lookup"><span data-stu-id="2e272-209">The page contains two partial views.</span></span> <span data-ttu-id="2e272-210">La seconde vue partielle passe un modèle et `ViewData` à la vue partielle.</span><span class="sxs-lookup"><span data-stu-id="2e272-210">The second partial view passes in a model and `ViewData` to the partial view.</span></span> <span data-ttu-id="2e272-211">La surcharge de constructeur `ViewDataDictionary` passe un nouveau dictionnaire `ViewData` tout en conservant le dictionnaire `ViewData` existant.</span><span class="sxs-lookup"><span data-stu-id="2e272-211">The `ViewDataDictionary` constructor overload is used to pass a new `ViewData` dictionary while retaining the existing `ViewData` dictionary.</span></span>

[!code-cshtml[](partial/sample/PartialViewsSample/Pages/ArticlesRP/ReadRP.cshtml?name=snippet_ReadPartialViewRP&highlight=5,15-19)]

<span data-ttu-id="2e272-212">*Pages/Shared/_AuthorPartialRP.cshtml* est la première vue partielle référencée par le fichier de balisage *ReadRP.cshtml* :</span><span class="sxs-lookup"><span data-stu-id="2e272-212">*Pages/Shared/_AuthorPartialRP.cshtml* is the first partial view referenced by the *ReadRP.cshtml* markup file:</span></span>

[!code-cshtml[](partial/sample/PartialViewsSample/Pages/Shared/_AuthorPartialRP.cshtml)]

<span data-ttu-id="2e272-213">*Pages/ArticlesRP/_ArticleSectionRP.cshtml* est la seconde vue partielle référencée par le fichier de balisage *ReadRP.cshtml* :</span><span class="sxs-lookup"><span data-stu-id="2e272-213">*Pages/ArticlesRP/_ArticleSectionRP.cshtml* is the second partial view referenced by the *ReadRP.cshtml* markup file:</span></span>

[!code-cshtml[](partial/sample/PartialViewsSample/Pages/ArticlesRP/_ArticleSectionRP.cshtml)]

<span data-ttu-id="2e272-214">**MVC**</span><span class="sxs-lookup"><span data-stu-id="2e272-214">**MVC**</span></span>

::: moniker-end

<span data-ttu-id="2e272-215">Le balisage suivant dans l’exemple d’application illustre la vue *Views/Articles/Read.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="2e272-215">The following markup in the sample app shows the *Views/Articles/Read.cshtml* view.</span></span> <span data-ttu-id="2e272-216">La vue contient deux vues partielles.</span><span class="sxs-lookup"><span data-stu-id="2e272-216">The view contains two partial views.</span></span> <span data-ttu-id="2e272-217">La seconde vue partielle passe un modèle et `ViewData` à la vue partielle.</span><span class="sxs-lookup"><span data-stu-id="2e272-217">The second partial view passes in a model and `ViewData` to the partial view.</span></span> <span data-ttu-id="2e272-218">La surcharge de constructeur `ViewDataDictionary` passe un nouveau dictionnaire `ViewData` tout en conservant le dictionnaire `ViewData` existant.</span><span class="sxs-lookup"><span data-stu-id="2e272-218">The `ViewDataDictionary` constructor overload is used to pass a new `ViewData` dictionary while retaining the existing `ViewData` dictionary.</span></span>

[!code-cshtml[](partial/sample/PartialViewsSample/Views/Articles/Read.cshtml?name=snippet_ReadPartialView&highlight=5,15-19)]

<span data-ttu-id="2e272-219">*Views/Shared/_AuthorPartial.cshtml* est la première vue partielle référencée par le fichier de balisage *ReadRP.cshtml* :</span><span class="sxs-lookup"><span data-stu-id="2e272-219">*Views/Shared/_AuthorPartial.cshtml* is the first partial view referenced by the *ReadRP.cshtml* markup file:</span></span>

[!code-cshtml[](partial/sample/PartialViewsSample/Views/Shared/_AuthorPartial.cshtml)]

<span data-ttu-id="2e272-220">*Views/Articles/_ArticleSection.cshtml* est la seconde vue partielle référencée par le fichier de balisage *Read.cshtml* :</span><span class="sxs-lookup"><span data-stu-id="2e272-220">*Views/Articles/_ArticleSection.cshtml* is the second partial view referenced by the *Read.cshtml* markup file:</span></span>

[!code-cshtml[](partial/sample/PartialViewsSample/Views/Articles/_ArticleSection.cshtml)]

<span data-ttu-id="2e272-221">Au moment de l’exécution, le rendu des vues partielles est effectué dans la sortie rendue du fichier de balisage parent, qui est elle-même rendue dans le fichier *_Layout.cshtml* partagé.</span><span class="sxs-lookup"><span data-stu-id="2e272-221">At runtime, the partials are rendered into the parent markup file's rendered output, which itself is rendered within the shared *_Layout.cshtml*.</span></span> <span data-ttu-id="2e272-222">La première vue partielle affiche le nom de l’auteur et la date de publication de l’article :</span><span class="sxs-lookup"><span data-stu-id="2e272-222">The first partial view renders the article author's name and publication date:</span></span>

> <span data-ttu-id="2e272-223">Abraham Lincoln</span><span class="sxs-lookup"><span data-stu-id="2e272-223">Abraham Lincoln</span></span>
>
> <span data-ttu-id="2e272-224">This partial view from &lt;shared partial view file path&gt;.</span><span class="sxs-lookup"><span data-stu-id="2e272-224">This partial view from &lt;shared partial view file path&gt;.</span></span>
> <span data-ttu-id="2e272-225">11/19/1863 12:00:00 AM</span><span class="sxs-lookup"><span data-stu-id="2e272-225">11/19/1863 12:00:00 AM</span></span>

<span data-ttu-id="2e272-226">La seconde vue partielle affiche les sections de l’article :</span><span class="sxs-lookup"><span data-stu-id="2e272-226">The second partial view renders the article's sections:</span></span>

> <span data-ttu-id="2e272-227">Section One Index: 0</span><span class="sxs-lookup"><span data-stu-id="2e272-227">Section One Index: 0</span></span>
>
> <span data-ttu-id="2e272-228">Four score and seven years ago ...</span><span class="sxs-lookup"><span data-stu-id="2e272-228">Four score and seven years ago ...</span></span>
>
> <span data-ttu-id="2e272-229">Section Two Index: 1</span><span class="sxs-lookup"><span data-stu-id="2e272-229">Section Two Index: 1</span></span>
>
> <span data-ttu-id="2e272-230">Now we are engaged in a great civil war, testing ...</span><span class="sxs-lookup"><span data-stu-id="2e272-230">Now we are engaged in a great civil war, testing ...</span></span>
>
> <span data-ttu-id="2e272-231">Section Three Index: 2</span><span class="sxs-lookup"><span data-stu-id="2e272-231">Section Three Index: 2</span></span>
>
> <span data-ttu-id="2e272-232">But, in a larger sense, we can not dedicate ...</span><span class="sxs-lookup"><span data-stu-id="2e272-232">But, in a larger sense, we can not dedicate ...</span></span>

## <a name="additional-resources"></a><span data-ttu-id="2e272-233">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="2e272-233">Additional resources</span></span>

::: moniker range=">= aspnetcore-2.1"

* <xref:mvc/views/razor>
* <xref:mvc/views/tag-helpers/intro>
* <xref:mvc/views/tag-helpers/builtin-th/partial-tag-helper>
* <xref:mvc/views/view-components>
* <xref:mvc/controllers/areas>

::: moniker-end

::: moniker range="< aspnetcore-2.1"

* <xref:mvc/views/razor>
* <xref:mvc/views/view-components>
* <xref:mvc/controllers/areas>

::: moniker-end
