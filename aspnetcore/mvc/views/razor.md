---
title: Informations de référence sur la syntaxe Razor pour ASP.NET Core
author: rick-anderson
description: Apprenez à utiliser la syntaxe de balisage Razor pour incorporer du code serveur dans des pages web.
manager: wpickett
ms.author: riande
ms.date: 10/18/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: mvc/views/razor
ms.openlocfilehash: 9c96ea34071bf3009f1ec53ed9af9206439aa229
ms.sourcegitcommit: 2ab550f8c46e1a8a5d45e58be44d151c676af256
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/28/2018
---
# <a name="razor-syntax-reference-for-aspnet-core"></a><span data-ttu-id="2b1b7-103">Informations de référence sur la syntaxe Razor pour ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="2b1b7-103">Razor syntax reference for ASP.NET Core</span></span>

<span data-ttu-id="2b1b7-104">Par [Rick Anderson](https://twitter.com/RickAndMSFT), [Luke Latham](https://github.com/guardrex), [Taylor Mullen](https://twitter.com/ntaylormullen) et [Dan Vicarel](https://github.com/Rabadash8820)</span><span class="sxs-lookup"><span data-stu-id="2b1b7-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Luke Latham](https://github.com/guardrex), [Taylor Mullen](https://twitter.com/ntaylormullen), and [Dan Vicarel](https://github.com/Rabadash8820)</span></span>

<span data-ttu-id="2b1b7-105">Razor est une syntaxe de balisage qui permet d’incorporer du code serveur dans des pages web.</span><span class="sxs-lookup"><span data-stu-id="2b1b7-105">Razor is a markup syntax for embedding server-based code into webpages.</span></span> <span data-ttu-id="2b1b7-106">La syntaxe Razor est constituée de balises Razor, ainsi que de code C# et HTML.</span><span class="sxs-lookup"><span data-stu-id="2b1b7-106">The Razor syntax consists of Razor markup, C#, and HTML.</span></span> <span data-ttu-id="2b1b7-107">Les fichiers contenant de la syntaxe Razor ont généralement l’extension de fichier *.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="2b1b7-107">Files containing Razor generally have a *.cshtml* file extension.</span></span>

## <a name="rendering-html"></a><span data-ttu-id="2b1b7-108">Rendu HTML</span><span class="sxs-lookup"><span data-stu-id="2b1b7-108">Rendering HTML</span></span>

<span data-ttu-id="2b1b7-109">Razor utilise par défaut le langage HTML.</span><span class="sxs-lookup"><span data-stu-id="2b1b7-109">The default Razor language is HTML.</span></span> <span data-ttu-id="2b1b7-110">Le rendu HTML d’un balisage Razor n’est pas différent du rendu HTML d’un fichier en HTML.</span><span class="sxs-lookup"><span data-stu-id="2b1b7-110">Rendering HTML from Razor markup is no different than rendering HTML from an HTML file.</span></span> <span data-ttu-id="2b1b7-111">Le balisage HTML dans les fichiers Razor *.cshtml* est affiché tel quel par le serveur.</span><span class="sxs-lookup"><span data-stu-id="2b1b7-111">HTML markup in *.cshtml* Razor files is rendered by the server unchanged.</span></span>

## <a name="razor-syntax"></a><span data-ttu-id="2b1b7-112">Syntaxe Razor</span><span class="sxs-lookup"><span data-stu-id="2b1b7-112">Razor syntax</span></span>

<span data-ttu-id="2b1b7-113">Razor prend en charge le langage C# et utilise le symbole `@` pour convertir du code HTML en C#.</span><span class="sxs-lookup"><span data-stu-id="2b1b7-113">Razor supports C# and uses the `@` symbol to transition from HTML to C#.</span></span> <span data-ttu-id="2b1b7-114">Razor évalue les expressions C# et les affiche dans la sortie HTML.</span><span class="sxs-lookup"><span data-stu-id="2b1b7-114">Razor evaluates C# expressions and renders them in the HTML output.</span></span>

<span data-ttu-id="2b1b7-115">Quand un symbole `@` est suivi d’un [mot clé réservé Razor](#razor-reserved-keywords), il est converti en balise Razor.</span><span class="sxs-lookup"><span data-stu-id="2b1b7-115">When an `@` symbol is followed by a [Razor reserved keyword](#razor-reserved-keywords), it transitions into Razor-specific markup.</span></span> <span data-ttu-id="2b1b7-116">Sinon, il est converti en code C# brut.</span><span class="sxs-lookup"><span data-stu-id="2b1b7-116">Otherwise, it transitions into plain C#.</span></span>

<span data-ttu-id="2b1b7-117">Pour mettre un symbole `@` en échappement dans le balisage Razor, ajoutez un deuxième symbole `@` :</span><span class="sxs-lookup"><span data-stu-id="2b1b7-117">To escape an `@` symbol in Razor markup, use a second `@` symbol:</span></span>

```cshtml
<p>@@Username</p>
```

<span data-ttu-id="2b1b7-118">Le code est affiché en HTML avec un seul symbole `@` :</span><span class="sxs-lookup"><span data-stu-id="2b1b7-118">The code is rendered in HTML with a single `@` symbol:</span></span>

```html
<p>@Username</p>
```

<span data-ttu-id="2b1b7-119">Les attributs et le code HTML contenant des adresses e-mail ne traitent pas le symbole `@` comme un caractère de conversion.</span><span class="sxs-lookup"><span data-stu-id="2b1b7-119">HTML attributes and content containing email addresses don't treat the `@` symbol as a transition character.</span></span> <span data-ttu-id="2b1b7-120">Dans l’exemple suivant, les adresses e-mail ne sont pas modifiées par l’analyse Razor :</span><span class="sxs-lookup"><span data-stu-id="2b1b7-120">The email addresses in the following example are untouched by Razor parsing:</span></span>

```cshtml
<a href="mailto:Support@contoso.com">Support@contoso.com</a>
```

## <a name="implicit-razor-expressions"></a><span data-ttu-id="2b1b7-121">Expressions Razor implicites</span><span class="sxs-lookup"><span data-stu-id="2b1b7-121">Implicit Razor expressions</span></span>

<span data-ttu-id="2b1b7-122">Les expressions Razor implicites commencent par `@` suivi de code C# :</span><span class="sxs-lookup"><span data-stu-id="2b1b7-122">Implicit Razor expressions start with `@` followed by C# code:</span></span>

```cshtml
<p>@DateTime.Now</p>
<p>@DateTime.IsLeapYear(2016)</p>
```

<span data-ttu-id="2b1b7-123">À l’exception du mot clé `await` C#, les expressions implicites ne doivent pas contenir d’espaces.</span><span class="sxs-lookup"><span data-stu-id="2b1b7-123">With the exception of the C# `await` keyword, implicit expressions must not contain spaces.</span></span> <span data-ttu-id="2b1b7-124">Si l’instruction C# se termine de façon non ambigüe, il est possible d’insérer des espaces n’importe où dans la chaîne :</span><span class="sxs-lookup"><span data-stu-id="2b1b7-124">If the C# statement has a clear ending, spaces can be intermingled:</span></span>

```cshtml
<p>@await DoSomething("hello", "world")</p>
```

<span data-ttu-id="2b1b7-125">Les expressions implicites **ne doivent pas** contenir de caractères génériques C#, car les caractères entre crochets (`<>`) sont interprétés comme une balise HTML.</span><span class="sxs-lookup"><span data-stu-id="2b1b7-125">Implicit expressions **cannot** contain C# generics, as the characters inside the brackets (`<>`) are interpreted as an HTML tag.</span></span> <span data-ttu-id="2b1b7-126">Le code suivant n’est **pas** valide :</span><span class="sxs-lookup"><span data-stu-id="2b1b7-126">The following code is **not** valid:</span></span>

```cshtml
<p>@GenericMethod<int>()</p>
```

<span data-ttu-id="2b1b7-127">Le code précédent génère l’un des types d’erreur de compilateur suivants :</span><span class="sxs-lookup"><span data-stu-id="2b1b7-127">The preceding code generates a compiler error similar to one of the following:</span></span>

 * <span data-ttu-id="2b1b7-128">L’élément « int » n’a pas été fermé.</span><span class="sxs-lookup"><span data-stu-id="2b1b7-128">The "int" element wasn't closed.</span></span> <span data-ttu-id="2b1b7-129">Tous les éléments doivent se fermer automatiquement ou contenir une balise de fin correspondante.</span><span class="sxs-lookup"><span data-stu-id="2b1b7-129">All elements must be either self-closing or have a matching end tag.</span></span>
 *  <span data-ttu-id="2b1b7-130">Impossible de convertir le groupe de méthodes 'GenericMethod' en type non-délégué 'object'.</span><span class="sxs-lookup"><span data-stu-id="2b1b7-130">Cannot convert method group 'GenericMethod' to non-delegate type 'object'.</span></span> <span data-ttu-id="2b1b7-131">Souhaitiez-vous appeler la méthode ?</span><span class="sxs-lookup"><span data-stu-id="2b1b7-131">Did you intend to invoke the method?\`</span></span> 
 
<span data-ttu-id="2b1b7-132">Les appels de méthode générique doivent être inclus dans un wrapper dans une [expression Razor explicite](#explicit-razor-expressions) ou dans un [bloc de code Razor](#razor-code-blocks).</span><span class="sxs-lookup"><span data-stu-id="2b1b7-132">Generic method calls must be wrapped in an [explicit Razor expression](#explicit-razor-expressions) or a [Razor code block](#razor-code-blocks).</span></span>

## <a name="explicit-razor-expressions"></a><span data-ttu-id="2b1b7-133">Expressions Razor explicites</span><span class="sxs-lookup"><span data-stu-id="2b1b7-133">Explicit Razor expressions</span></span>

<span data-ttu-id="2b1b7-134">Les expressions Razor explicites se composent d’un symbole `@` suivi d’un contenu entre parenthèses.</span><span class="sxs-lookup"><span data-stu-id="2b1b7-134">Explicit Razor expressions consist of an `@` symbol with balanced parenthesis.</span></span> <span data-ttu-id="2b1b7-135">Pour afficher l’heure de la semaine précédente, utilisez le balisage Razor suivant :</span><span class="sxs-lookup"><span data-stu-id="2b1b7-135">To render last week's time, the following Razor markup is used:</span></span>

```cshtml
<p>Last week this time: @(DateTime.Now - TimeSpan.FromDays(7))</p>
```

<span data-ttu-id="2b1b7-136">Le contenu situé entre les parenthèses `@()` est évalué et affiché dans la sortie.</span><span class="sxs-lookup"><span data-stu-id="2b1b7-136">Any content within the `@()` parenthesis is evaluated and rendered to the output.</span></span>

<span data-ttu-id="2b1b7-137">Les expressions implicites, décrites dans la section précédente, ne doivent généralement pas contenir d’espaces.</span><span class="sxs-lookup"><span data-stu-id="2b1b7-137">Implicit expressions, described in the previous section, generally can't contain spaces.</span></span> <span data-ttu-id="2b1b7-138">Dans le code suivant, une semaine n’est pas déduite de l’heure actuelle :</span><span class="sxs-lookup"><span data-stu-id="2b1b7-138">In the following code, one week isn't subtracted from the current time:</span></span>

[!code-cshtml[](razor/sample/Views/Home/Contact.cshtml?range=17)]

<span data-ttu-id="2b1b7-139">Le code s’affiche en HTML de la façon suivante :</span><span class="sxs-lookup"><span data-stu-id="2b1b7-139">The code renders the following HTML:</span></span>

```html
<p>Last week: 7/7/2016 4:39:52 PM - TimeSpan.FromDays(7)</p>
```

<span data-ttu-id="2b1b7-140">Les expressions explicites peuvent servir à concaténer du texte avec un résultat d’expression :</span><span class="sxs-lookup"><span data-stu-id="2b1b7-140">Explicit expressions can be used to concatenate text with an expression result:</span></span>

```cshtml
@{
    var joe = new Person("Joe", 33);
}

<p>Age@(joe.Age)</p>
```

<span data-ttu-id="2b1b7-141">Sans l’expression explicite, `<p>Age@joe.Age</p>` est traité comme une adresse e-mail, et `<p>Age@joe.Age</p>` est affiché.</span><span class="sxs-lookup"><span data-stu-id="2b1b7-141">Without the explicit expression, `<p>Age@joe.Age</p>` is treated as an email address, and `<p>Age@joe.Age</p>` is rendered.</span></span> <span data-ttu-id="2b1b7-142">Avec une expression explicite, `<p>Age33</p>` est affiché.</span><span class="sxs-lookup"><span data-stu-id="2b1b7-142">When written as an explicit expression, `<p>Age33</p>` is rendered.</span></span>

<span data-ttu-id="2b1b7-143">Les expressions explicites peuvent être utilisées pour afficher la sortie de méthodes génériques dans les fichiers *.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="2b1b7-143">Explicit expressions can be used to render output from generic methods in *.cshtml* files.</span></span> <span data-ttu-id="2b1b7-144">Le balisage suivant montre comment corriger l’erreur affichée précédemment provoquée par les crochets d’un générique C#.</span><span class="sxs-lookup"><span data-stu-id="2b1b7-144">The following markup shows how to correct the error shown earlier caused by the brackets of a C# generic.</span></span> <span data-ttu-id="2b1b7-145">Le code est écrit sous forme d’expression explicite :</span><span class="sxs-lookup"><span data-stu-id="2b1b7-145">The code is written as an explicit expression:</span></span>

```cshtml
<p>@(GenericMethod<int>())</p>
```

## <a name="expression-encoding"></a><span data-ttu-id="2b1b7-146">Encodage des expressions</span><span class="sxs-lookup"><span data-stu-id="2b1b7-146">Expression encoding</span></span>

<span data-ttu-id="2b1b7-147">Les expressions C# évaluées qui correspondent à une chaîne sont encodées en HTML.</span><span class="sxs-lookup"><span data-stu-id="2b1b7-147">C# expressions that evaluate to a string are HTML encoded.</span></span> <span data-ttu-id="2b1b7-148">Les expressions C# évaluées qui correspondent à `IHtmlContent` sont affichées directement par `IHtmlContent.WriteTo`.</span><span class="sxs-lookup"><span data-stu-id="2b1b7-148">C# expressions that evaluate to `IHtmlContent` are rendered directly through `IHtmlContent.WriteTo`.</span></span> <span data-ttu-id="2b1b7-149">Les expressions C# évaluées qui ne correspondent pas à `IHtmlContent` sont converties en chaîne par `ToString` et sont encodées avant d’être affichées.</span><span class="sxs-lookup"><span data-stu-id="2b1b7-149">C# expressions that don't evaluate to `IHtmlContent` are converted to a string by `ToString` and encoded before they're rendered.</span></span>

```cshtml
@("<span>Hello World</span>")
```

<span data-ttu-id="2b1b7-150">Le code s’affiche en HTML de la façon suivante :</span><span class="sxs-lookup"><span data-stu-id="2b1b7-150">The code renders the following HTML:</span></span>

```html
&lt;span&gt;Hello World&lt;/span&gt;
```

<span data-ttu-id="2b1b7-151">Le code HTML s’affiche dans le navigateur de cette façon :</span><span class="sxs-lookup"><span data-stu-id="2b1b7-151">The HTML is shown in the browser as:</span></span>

```
<span>Hello World</span>
```

<span data-ttu-id="2b1b7-152">La sortie `HtmlHelper.Raw` n’est pas encodée, mais elle est affichée sous forme de balisage HTML.</span><span class="sxs-lookup"><span data-stu-id="2b1b7-152">`HtmlHelper.Raw` output isn't encoded but rendered as HTML markup.</span></span>

> [!WARNING]
> <span data-ttu-id="2b1b7-153">Utiliser `HtmlHelper.Raw` sur des entrées utilisateur non nettoyées présente un risque pour la sécurité.</span><span class="sxs-lookup"><span data-stu-id="2b1b7-153">Using `HtmlHelper.Raw` on unsanitized user input is a security risk.</span></span> <span data-ttu-id="2b1b7-154">Les entrées utilisateur peuvent contenir du code malveillant JavaScript ou d’un autre type.</span><span class="sxs-lookup"><span data-stu-id="2b1b7-154">User input might contain malicious JavaScript or other exploits.</span></span> <span data-ttu-id="2b1b7-155">Le nettoyage des entrées utilisateur est difficile.</span><span class="sxs-lookup"><span data-stu-id="2b1b7-155">Sanitizing user input is difficult.</span></span> <span data-ttu-id="2b1b7-156">C’est pourquoi il est préférable de ne pas utiliser `HtmlHelper.Raw` sur des entrées utilisateur.</span><span class="sxs-lookup"><span data-stu-id="2b1b7-156">Avoid using `HtmlHelper.Raw` with user input.</span></span>

```cshtml
@Html.Raw("<span>Hello World</span>")
```

<span data-ttu-id="2b1b7-157">Le code s’affiche en HTML de la façon suivante :</span><span class="sxs-lookup"><span data-stu-id="2b1b7-157">The code renders the following HTML:</span></span>

```html
<span>Hello World</span>
```

## <a name="razor-code-blocks"></a><span data-ttu-id="2b1b7-158">Blocs de code Razor</span><span class="sxs-lookup"><span data-stu-id="2b1b7-158">Razor code blocks</span></span>

<span data-ttu-id="2b1b7-159">Les blocs de code Razor commencent par `@` et sont délimités par deux `{}`.</span><span class="sxs-lookup"><span data-stu-id="2b1b7-159">Razor code blocks start with `@` and are enclosed by `{}`.</span></span> <span data-ttu-id="2b1b7-160">Contrairement aux expressions, le code C# figurant dans des blocs de code n’est pas affiché.</span><span class="sxs-lookup"><span data-stu-id="2b1b7-160">Unlike expressions, C# code inside code blocks isn't rendered.</span></span> <span data-ttu-id="2b1b7-161">Les blocs de code et les expressions dans une vue ont la même étendue et sont définis dans l’ordre :</span><span class="sxs-lookup"><span data-stu-id="2b1b7-161">Code blocks and expressions in a view share the same scope and are defined in order:</span></span>

```cshtml
@{
    var quote = "The future depends on what you do today. - Mahatma Gandhi";
}

<p>@quote</p>

@{
    quote = "Hate cannot drive out hate, only love can do that. - Martin Luther King, Jr.";
}

<p>@quote</p>
```

<span data-ttu-id="2b1b7-162">Le code s’affiche en HTML de la façon suivante :</span><span class="sxs-lookup"><span data-stu-id="2b1b7-162">The code renders the following HTML:</span></span>

```html
<p>The future depends on what you do today. - Mahatma Gandhi</p>
<p>Hate cannot drive out hate, only love can do that. - Martin Luther King, Jr.</p>
```

### <a name="implicit-transitions"></a><span data-ttu-id="2b1b7-163">Transitions implicites</span><span class="sxs-lookup"><span data-stu-id="2b1b7-163">Implicit transitions</span></span>

<span data-ttu-id="2b1b7-164">Un bloc de code utilise le langage C# par défaut, mais la page Razor peut le reconvertir en HTML :</span><span class="sxs-lookup"><span data-stu-id="2b1b7-164">The default language in a code block is C#, but the Razor Page can transition back to HTML:</span></span>

```cshtml
@{
    var inCSharp = true;
    <p>Now in HTML, was in C# @inCSharp</p>
}
```

### <a name="explicit-delimited-transition"></a><span data-ttu-id="2b1b7-165">Conversion délimitée explicite</span><span class="sxs-lookup"><span data-stu-id="2b1b7-165">Explicit delimited transition</span></span>

<span data-ttu-id="2b1b7-166">Pour définir une sous-section d’un bloc de code qui doit s’afficher en HTML, placez les caractères à afficher dans la balise **\<text>** Razor suivante :</span><span class="sxs-lookup"><span data-stu-id="2b1b7-166">To define a subsection of a code block that should render HTML, surround the characters for rendering with the Razor **\<text>** tag:</span></span>

```cshtml
@for (var i = 0; i < people.Length; i++)
{
    var person = people[i];
    <text>Name: @person.Name</text>
}
```

<span data-ttu-id="2b1b7-167">Utilisez cette approche pour afficher du code HTML qui n’est pas entouré d’une balise HTML.</span><span class="sxs-lookup"><span data-stu-id="2b1b7-167">Use this approach to render HTML that isn't surrounded by an HTML tag.</span></span> <span data-ttu-id="2b1b7-168">Sans la balise HTML ou Razor, une erreur d’exécution Razor se produit.</span><span class="sxs-lookup"><span data-stu-id="2b1b7-168">Without an HTML or Razor tag, a Razor runtime error occurs.</span></span>

<span data-ttu-id="2b1b7-169">La balise **\<text>** est utile pour contrôler les espaces blancs dans le contenu affiché :</span><span class="sxs-lookup"><span data-stu-id="2b1b7-169">The **\<text>** tag is useful to control whitespace when rendering content:</span></span>

* <span data-ttu-id="2b1b7-170">Seul le contenu situé dans la balise **\<text>** est affiché.</span><span class="sxs-lookup"><span data-stu-id="2b1b7-170">Only the content between the **\<text>** tag is rendered.</span></span> 
* <span data-ttu-id="2b1b7-171">Aucun espace blanc avant ou après la balise **\<text>** ne s’affiche dans la sortie HTML.</span><span class="sxs-lookup"><span data-stu-id="2b1b7-171">No whitespace before or after the **\<text>** tag appears in the HTML output.</span></span>

### <a name="explicit-line-transition-with-"></a><span data-ttu-id="2b1b7-172">Conversion de ligne explicite avec @ :</span><span class="sxs-lookup"><span data-stu-id="2b1b7-172">Explicit Line Transition with @:</span></span>

<span data-ttu-id="2b1b7-173">Pour afficher le reste d’une ligne entière en HTML à l’intérieur d’un bloc de code, utilisez la syntaxe `@:` :</span><span class="sxs-lookup"><span data-stu-id="2b1b7-173">To render the rest of an entire line as HTML inside a code block, use the `@:` syntax:</span></span>

```cshtml
@for (var i = 0; i < people.Length; i++)
{
    var person = people[i];
    @:Name: @person.Name
}
```

<span data-ttu-id="2b1b7-174">Sans la balise `@:` dans le code, une erreur d’exécution Razor se produit.</span><span class="sxs-lookup"><span data-stu-id="2b1b7-174">Without the `@:` in the code, a Razor runtime error is generated.</span></span>

<span data-ttu-id="2b1b7-175">Avertissement : La présence de caractères `@` en trop dans un fichier Razor risque de provoquer des erreurs du compilateur au niveau des instructions suivantes dans le bloc.</span><span class="sxs-lookup"><span data-stu-id="2b1b7-175">Warning: Extra `@` characters in a Razor file can cause compiler errors at statements later in the block.</span></span> <span data-ttu-id="2b1b7-176">Ces erreurs du compilateur peuvent être difficiles à comprendre, car elles se produisent en réalité avant les erreurs signalées.</span><span class="sxs-lookup"><span data-stu-id="2b1b7-176">These compiler errors can be difficult to understand because the actual error occurs before the reported error.</span></span> <span data-ttu-id="2b1b7-177">Ce type d’erreur est courant après la combinaison de plusieurs expressions implicites ou explicites dans un même bloc de code.</span><span class="sxs-lookup"><span data-stu-id="2b1b7-177">This error is common after combining multiple implicit/explicit expressions into a single code block.</span></span>

## <a name="control-structures"></a><span data-ttu-id="2b1b7-178">Structures de contrôle</span><span class="sxs-lookup"><span data-stu-id="2b1b7-178">Control structures</span></span>

<span data-ttu-id="2b1b7-179">Les structures de contrôle sont une extension des blocs de code.</span><span class="sxs-lookup"><span data-stu-id="2b1b7-179">Control structures are an extension of code blocks.</span></span> <span data-ttu-id="2b1b7-180">Toutes les caractéristiques des blocs de code (conversion de balisage, Inline C#) valent aussi pour les structures suivantes :</span><span class="sxs-lookup"><span data-stu-id="2b1b7-180">All aspects of code blocks (transitioning to markup, inline C#) also apply to the following structures:</span></span>

### <a name="conditionals-if-else-if-else-and-switch"></a><span data-ttu-id="2b1b7-181">Conditions @if, else if, else et @switch</span><span class="sxs-lookup"><span data-stu-id="2b1b7-181">Conditionals @if, else if, else, and @switch</span></span>

<span data-ttu-id="2b1b7-182">`@if` contrôle l’exécution du code :</span><span class="sxs-lookup"><span data-stu-id="2b1b7-182">`@if` controls when code runs:</span></span>

```cshtml
@if (value % 2 == 0)
{
    <p>The value was even.</p>
}
```

<span data-ttu-id="2b1b7-183">`else` et `else if` ne nécessitent pas le symbole `@` :</span><span class="sxs-lookup"><span data-stu-id="2b1b7-183">`else` and `else if` don't require the `@` symbol:</span></span>

```cshtml
@if (value % 2 == 0)
{
    <p>The value was even.</p>
}
else if (value >= 1337)
{
    <p>The value is large.</p>
}
else
{
    <p>The value is odd and small.</p>
}
```

<span data-ttu-id="2b1b7-184">Le balisage suivant montre comment utiliser une instruction switch :</span><span class="sxs-lookup"><span data-stu-id="2b1b7-184">The following markup shows how to use a switch statement:</span></span>

```cshtml
@switch (value)
{
    case 1:
        <p>The value is 1!</p>
        break;
    case 1337:
        <p>Your number is 1337!</p>
        break;
    default:
        <p>Your number wasn't 1 or 1337.</p>
        break;
}
```

### <a name="looping-for-foreach-while-and-do-while"></a><span data-ttu-id="2b1b7-185">Boucles @for, @foreach, @while et @do while</span><span class="sxs-lookup"><span data-stu-id="2b1b7-185">Looping @for, @foreach, @while, and @do while</span></span>

<span data-ttu-id="2b1b7-186">Le rendu HTML peut être réalisé à partir d'instructions de contrôle de boucle.</span><span class="sxs-lookup"><span data-stu-id="2b1b7-186">Templated HTML can be rendered with looping control statements.</span></span> <span data-ttu-id="2b1b7-187">Pour afficher une liste de personnes :</span><span class="sxs-lookup"><span data-stu-id="2b1b7-187">To render a list of people:</span></span>

```cshtml
@{
    var people = new Person[]
    {
          new Person("Weston", 33),
          new Person("Johnathon", 41),
          ...
    };
}
```

<span data-ttu-id="2b1b7-188">Les instructions de boucle suivantes sont prises en charge :</span><span class="sxs-lookup"><span data-stu-id="2b1b7-188">The following looping statements are supported:</span></span>

`@for`

```cshtml
@for (var i = 0; i < people.Length; i++)
{
    var person = people[i];
    <p>Name: @person.Name</p>
    <p>Age: @person.Age</p>
}
```

`@foreach`

```cshtml
@foreach (var person in people)
{
    <p>Name: @person.Name</p>
    <p>Age: @person.Age</p>
}
```

`@while`

```cshtml
@{ var i = 0; }
@while (i < people.Length)
{
    var person = people[i];
    <p>Name: @person.Name</p>
    <p>Age: @person.Age</p>

    i++;
}
```

`@do while`

```cshtml
@{ var i = 0; }
@do
{
    var person = people[i];
    <p>Name: @person.Name</p>
    <p>Age: @person.Age</p>

    i++;
} while (i < people.Length);
```

### <a name="compound-using"></a><span data-ttu-id="2b1b7-189">Instruction composée @using</span><span class="sxs-lookup"><span data-stu-id="2b1b7-189">Compound @using</span></span>

<span data-ttu-id="2b1b7-190">En C#, une instruction `using` est utilisée pour garantir la dispose d’un objet.</span><span class="sxs-lookup"><span data-stu-id="2b1b7-190">In C#, a `using` statement is used to ensure an object is disposed.</span></span> <span data-ttu-id="2b1b7-191">Dans Razor, le même mécanisme permet de créer des HTML Helpers avec du contenu supplémentaire.</span><span class="sxs-lookup"><span data-stu-id="2b1b7-191">In Razor, the same mechanism is used to create HTML Helpers that contain additional content.</span></span> <span data-ttu-id="2b1b7-192">Dans le code suivant, les HTML Helpers affichent une balise Form à l’aide de l’instruction `@using` :</span><span class="sxs-lookup"><span data-stu-id="2b1b7-192">In the following code, HTML Helpers render a form tag with the `@using` statement:</span></span>


```cshtml
@using (Html.BeginForm())
{
    <div>
        email:
        <input type="email" id="Email" value="">
        <button>Register</button>
    </div>
}
```

<span data-ttu-id="2b1b7-193">Des actions au niveau de l’étendue peuvent être effectuées avec des [Tag Helpers](xref:mvc/views/tag-helpers/intro).</span><span class="sxs-lookup"><span data-stu-id="2b1b7-193">Scope-level actions can be performed with [Tag Helpers](xref:mvc/views/tag-helpers/intro).</span></span>

### <a name="try-catch-finally"></a><span data-ttu-id="2b1b7-194">@try, catch, finally</span><span class="sxs-lookup"><span data-stu-id="2b1b7-194">@try, catch, finally</span></span>

<span data-ttu-id="2b1b7-195">La gestion des exceptions est similaire à C# :</span><span class="sxs-lookup"><span data-stu-id="2b1b7-195">Exception handling is similar to C#:</span></span>

[!code-cshtml[](razor/sample/Views/Home/Contact7.cshtml)]

### <a name="lock"></a>@lock

<span data-ttu-id="2b1b7-196">Razor permet de verrouiller les sections critiques par des instructions lock :</span><span class="sxs-lookup"><span data-stu-id="2b1b7-196">Razor has the capability to protect critical sections with lock statements:</span></span>

```cshtml
@lock (SomeLock)
{
    // Do critical section work
}
```

### <a name="comments"></a><span data-ttu-id="2b1b7-197">Commentaires</span><span class="sxs-lookup"><span data-stu-id="2b1b7-197">Comments</span></span>

<span data-ttu-id="2b1b7-198">Razor prend en charge les commentaires HTML et C# :</span><span class="sxs-lookup"><span data-stu-id="2b1b7-198">Razor supports C# and HTML comments:</span></span>

```cshtml
@{
    /* C# comment */
    // Another C# comment
}
<!-- HTML comment -->
```

<span data-ttu-id="2b1b7-199">Le code s’affiche en HTML de la façon suivante :</span><span class="sxs-lookup"><span data-stu-id="2b1b7-199">The code renders the following HTML:</span></span>

```html
<!-- HTML comment -->
```

<span data-ttu-id="2b1b7-200">Le serveur supprime les commentaires Razor avant d’afficher la page web.</span><span class="sxs-lookup"><span data-stu-id="2b1b7-200">Razor comments are removed by the server before the webpage is rendered.</span></span> <span data-ttu-id="2b1b7-201">Razor délimite les commentaires avec `@*  *@`.</span><span class="sxs-lookup"><span data-stu-id="2b1b7-201">Razor uses `@*  *@` to delimit comments.</span></span> <span data-ttu-id="2b1b7-202">Le code suivant est commenté pour indiquer au serveur de ne pas afficher le balisage :</span><span class="sxs-lookup"><span data-stu-id="2b1b7-202">The following code is commented out, so the server doesn't render any markup:</span></span>

```cshtml
@*
    @{
        /* C# comment */
        // Another C# comment
    }
    <!-- HTML comment -->
*@
```

## <a name="directives"></a><span data-ttu-id="2b1b7-203">Directives</span><span class="sxs-lookup"><span data-stu-id="2b1b7-203">Directives</span></span>

<span data-ttu-id="2b1b7-204">Les directives Razor sont représentées par des expressions implicites constituées du symbole `@` suivi de mots clés réservés.</span><span class="sxs-lookup"><span data-stu-id="2b1b7-204">Razor directives are represented by implicit expressions with reserved keywords following the `@` symbol.</span></span> <span data-ttu-id="2b1b7-205">En règle générale, une directive change la manière dont une vue est analysée ou active une fonctionnalité différente.</span><span class="sxs-lookup"><span data-stu-id="2b1b7-205">A directive typically changes the way a view is parsed or enables different functionality.</span></span>

<span data-ttu-id="2b1b7-206">Pour mieux comprendre le fonctionnement des directives, vous devez bien comprendre comment Razor génère le code pour une vue.</span><span class="sxs-lookup"><span data-stu-id="2b1b7-206">Understanding how Razor generates code for a view makes it easier to understand how directives work.</span></span>

[!code-html[](razor/sample/Views/Home/Contact8.cshtml)]

<span data-ttu-id="2b1b7-207">Le code génère une classe semblable à celle-ci :</span><span class="sxs-lookup"><span data-stu-id="2b1b7-207">The code generates a class similar to the following:</span></span>

```csharp
public class _Views_Something_cshtml : RazorPage<dynamic>
{
    public override async Task ExecuteAsync()
    {
        var output = "Getting old ain't for wimps! - Anonymous";

        WriteLiteral("/r/n<div>Quote of the Day: ");
        Write(output);
        WriteLiteral("</div>");
    }
}
```

<span data-ttu-id="2b1b7-208">La section [Affichage de la classe C# Razor générée pour une vue](#viewing-the-razor-c-class-generated-for-a-view), plus loin dans cet article, explique comment afficher cette classe générée.</span><span class="sxs-lookup"><span data-stu-id="2b1b7-208">Later in this article, the section [Viewing the Razor C# class generated for a view](#viewing-the-razor-c-class-generated-for-a-view) explains how to view this generated class.</span></span>

<a name="using"></a>
### <a name="using"></a>@using

<span data-ttu-id="2b1b7-209">La directive `@using` ajoute la directive `using` C# à la vue générée :</span><span class="sxs-lookup"><span data-stu-id="2b1b7-209">The `@using` directive adds the C# `using` directive to the generated view:</span></span>

[!code-cshtml[](razor/sample/Views/Home/Contact9.cshtml)]

### <a name="model"></a>@model

<span data-ttu-id="2b1b7-210">La directive `@model` spécifie le type du modèle passé à une vue :</span><span class="sxs-lookup"><span data-stu-id="2b1b7-210">The `@model` directive specifies the type of the model passed to a view:</span></span>

```cshtml
@model TypeNameOfModel
```

<span data-ttu-id="2b1b7-211">Dans une application ASP.NET Core MVC créée avec des comptes d’utilisateur individuels, la vue *Views/Account/Login.cshtml* contient la déclaration de modèle suivante :</span><span class="sxs-lookup"><span data-stu-id="2b1b7-211">In an ASP.NET Core MVC app created with individual user accounts, the *Views/Account/Login.cshtml* view contains the following model declaration:</span></span>

```cshtml
@model LoginViewModel
```

<span data-ttu-id="2b1b7-212">La classe générée hérite de `RazorPage<dynamic>` :</span><span class="sxs-lookup"><span data-stu-id="2b1b7-212">The class generated inherits from `RazorPage<dynamic>`:</span></span>

```csharp
public class _Views_Account_Login_cshtml : RazorPage<LoginViewModel>
```

<span data-ttu-id="2b1b7-213">Razor expose une propriété `Model` pour accéder au modèle passé à la vue :</span><span class="sxs-lookup"><span data-stu-id="2b1b7-213">Razor exposes a `Model` property for accessing the model passed to the view:</span></span>

```cshtml
<div>The Login Email: @Model.Email</div>
```

<span data-ttu-id="2b1b7-214">La directive `@model` spécifie le type de cette propriété.</span><span class="sxs-lookup"><span data-stu-id="2b1b7-214">The `@model` directive specifies the type of this property.</span></span> <span data-ttu-id="2b1b7-215">La directive spécifie le type `T` dans `RazorPage<T>` pour la classe générée dont dérive la vue.</span><span class="sxs-lookup"><span data-stu-id="2b1b7-215">The directive specifies the `T` in `RazorPage<T>` that the generated class that the view derives from.</span></span> <span data-ttu-id="2b1b7-216">Si la directive `@model` n’est pas spécifiée, la propriété `Model` est de type `dynamic`.</span><span class="sxs-lookup"><span data-stu-id="2b1b7-216">If the `@model` directive isn't specified, the `Model` property is of type `dynamic`.</span></span> <span data-ttu-id="2b1b7-217">La valeur du modèle est passée à la vue par le contrôleur.</span><span class="sxs-lookup"><span data-stu-id="2b1b7-217">The value of the model is passed from the controller to the view.</span></span> <span data-ttu-id="2b1b7-218">Pour plus d’informations, consultez [Modèles fortement typés et le mot clé &commat;model](xref:tutorials/first-mvc-app/adding-model#strongly-typed-models-and-the--keyword).</span><span class="sxs-lookup"><span data-stu-id="2b1b7-218">For more information, see [Strongly typed models and the &commat;model keyword](xref:tutorials/first-mvc-app/adding-model#strongly-typed-models-and-the--keyword).</span></span>

### <a name="inherits"></a>@inherits

<span data-ttu-id="2b1b7-219">La directive `@inherits` fournit un contrôle complet de la classe héritée par la vue :</span><span class="sxs-lookup"><span data-stu-id="2b1b7-219">The `@inherits` directive provides full control of the class the view inherits:</span></span>

```cshtml
@inherits TypeNameOfClassToInheritFrom
```

<span data-ttu-id="2b1b7-220">Le code suivant est un type de page Razor personnalisé :</span><span class="sxs-lookup"><span data-stu-id="2b1b7-220">The following code is a custom Razor page type:</span></span>

[!code-csharp[](razor/sample/Classes/CustomRazorPage.cs)]

<span data-ttu-id="2b1b7-221">Le `CustomText` s’affiche dans une vue :</span><span class="sxs-lookup"><span data-stu-id="2b1b7-221">The `CustomText` is displayed in a view:</span></span>

[!code-cshtml[](razor/sample/Views/Home/Contact10.cshtml)]

<span data-ttu-id="2b1b7-222">Le code s’affiche en HTML de la façon suivante :</span><span class="sxs-lookup"><span data-stu-id="2b1b7-222">The code renders the following HTML:</span></span>

```html
<div>Custom text: Gardyloo! - A Scottish warning yelled from a window before dumping a slop bucket on the street below.</div>
```

 <span data-ttu-id="2b1b7-223">`@model` et `@inherits` peuvent s’utiliser dans la même vue.</span><span class="sxs-lookup"><span data-stu-id="2b1b7-223">`@model` and `@inherits` can be used in the same view.</span></span> <span data-ttu-id="2b1b7-224">`@inherits` peut être dans un fichier *_ViewImports.cshtml* importé par la vue :</span><span class="sxs-lookup"><span data-stu-id="2b1b7-224">`@inherits` can be in a *_ViewImports.cshtml* file that the view imports:</span></span>

[!code-cshtml[](razor/sample/Views/_ViewImportsModel.cshtml)]

<span data-ttu-id="2b1b7-225">Le code suivant est un exemple de vue fortement typée :</span><span class="sxs-lookup"><span data-stu-id="2b1b7-225">The following code is an example of a strongly-typed view:</span></span>

[!code-cshtml[](razor/sample/Views/Home/Login1.cshtml)]

<span data-ttu-id="2b1b7-226">Si « rick@contoso.com » est passé au modèle, la vue génère le balisage HTML suivant :</span><span class="sxs-lookup"><span data-stu-id="2b1b7-226">If "rick@contoso.com" is passed in the model, the view generates the following HTML markup:</span></span>

```html
<div>The Login Email: rick@contoso.com</div>
<div>Custom text: Gardyloo! - A Scottish warning yelled from a window before dumping a slop bucket on the street below.</div>
```

### <a name="inject"></a>@inject


<span data-ttu-id="2b1b7-227">La directive `@inject` permet à la page Razor d’injecter un service dans une vue à partir du [conteneur de services](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="2b1b7-227">The `@inject` directive enables the Razor Page to inject a service from the [service container](xref:fundamentals/dependency-injection) into a view.</span></span> <span data-ttu-id="2b1b7-228">Pour plus d’informations, consultez [Injection de dépendances dans les vues](xref:mvc/views/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="2b1b7-228">For more information, see [Dependency injection into views](xref:mvc/views/dependency-injection).</span></span>

### <a name="functions"></a>@functions

<span data-ttu-id="2b1b7-229">La directive `@functions` permet à une page Razor d’ajouter un bloc de code C# à une vue :</span><span class="sxs-lookup"><span data-stu-id="2b1b7-229">The `@functions` directive enables a Razor Page to add a C# code block to a view:</span></span>

```cshtml
@functions { // C# Code }
```

<span data-ttu-id="2b1b7-230">Exemple :</span><span class="sxs-lookup"><span data-stu-id="2b1b7-230">For example:</span></span>

[!code-cshtml[](razor/sample/Views/Home/Contact6.cshtml)]

<span data-ttu-id="2b1b7-231">Le code génère le balisage HTML suivant :</span><span class="sxs-lookup"><span data-stu-id="2b1b7-231">The code generates the following HTML markup:</span></span>

```html
<div>From method: Hello</div>
```

<span data-ttu-id="2b1b7-232">Le code suivant correspond à la classe C# Razor générée :</span><span class="sxs-lookup"><span data-stu-id="2b1b7-232">The following code is the generated Razor C# class:</span></span>

[!code-csharp[](razor/sample/Classes/Views_Home_Test_cshtml.cs?range=1-19)]

### <a name="section"></a>@section

<span data-ttu-id="2b1b7-233">La directive `@section` est utilisée conjointement avec une [disposition](xref:mvc/views/layout) pour permettre aux vues d’afficher le contenu dans différentes parties de la page HTML.</span><span class="sxs-lookup"><span data-stu-id="2b1b7-233">The `@section` directive is used in conjunction with the [layout](xref:mvc/views/layout) to enable views to render content in different parts of the HTML page.</span></span> <span data-ttu-id="2b1b7-234">Pour plus d’informations, consultez [Sections](xref:mvc/views/layout#layout-sections-label).</span><span class="sxs-lookup"><span data-stu-id="2b1b7-234">For more information, see [Sections](xref:mvc/views/layout#layout-sections-label).</span></span>

## <a name="tag-helpers"></a><span data-ttu-id="2b1b7-235">Tag Helpers</span><span class="sxs-lookup"><span data-stu-id="2b1b7-235">Tag Helpers</span></span>

<span data-ttu-id="2b1b7-236">Il existe trois directives spécifiques aux [Tag Helpers](xref:mvc/views/tag-helpers/intro).</span><span class="sxs-lookup"><span data-stu-id="2b1b7-236">There are three directives that pertain to [Tag Helpers](xref:mvc/views/tag-helpers/intro).</span></span>

| <span data-ttu-id="2b1b7-237">Directive</span><span class="sxs-lookup"><span data-stu-id="2b1b7-237">Directive</span></span> | <span data-ttu-id="2b1b7-238">Fonction</span><span class="sxs-lookup"><span data-stu-id="2b1b7-238">Function</span></span> |
| --------- | -------- |
| [<span data-ttu-id="2b1b7-239">&commat;addTagHelper</span><span class="sxs-lookup"><span data-stu-id="2b1b7-239">&commat;addTagHelper</span></span>](xref:mvc/views/tag-helpers/intro#add-helper-label) | <span data-ttu-id="2b1b7-240">Rend les Tag Helpers disponibles dans une vue.</span><span class="sxs-lookup"><span data-stu-id="2b1b7-240">Makes Tag Helpers available to a view.</span></span> |
| [<span data-ttu-id="2b1b7-241">&commat;removeTagHelper</span><span class="sxs-lookup"><span data-stu-id="2b1b7-241">&commat;removeTagHelper</span></span>](xref:mvc/views/tag-helpers/intro#remove-razor-directives-label) | <span data-ttu-id="2b1b7-242">Supprime les Tag Helpers précédemment ajoutés à une vue.</span><span class="sxs-lookup"><span data-stu-id="2b1b7-242">Removes Tag Helpers previously added from a view.</span></span> |
| [<span data-ttu-id="2b1b7-243">&commat;tagHelperPrefix</span><span class="sxs-lookup"><span data-stu-id="2b1b7-243">&commat;tagHelperPrefix</span></span>](xref:mvc/views/tag-helpers/intro#prefix-razor-directives-label) | <span data-ttu-id="2b1b7-244">Spécifie un préfixe de balise pour activer la prise en charge des Tag Helpers et rendre leur usage explicite.</span><span class="sxs-lookup"><span data-stu-id="2b1b7-244">Specifies a tag prefix to enable Tag Helper support and to make Tag Helper usage explicit.</span></span> |

## <a name="razor-reserved-keywords"></a><span data-ttu-id="2b1b7-245">Mots clés réservés Razor</span><span class="sxs-lookup"><span data-stu-id="2b1b7-245">Razor reserved keywords</span></span>

### <a name="razor-keywords"></a><span data-ttu-id="2b1b7-246">Mots clés Razor</span><span class="sxs-lookup"><span data-stu-id="2b1b7-246">Razor keywords</span></span>

* <span data-ttu-id="2b1b7-247">page (nécessite ASP.NET Core 2.0 ou version ultérieure)</span><span class="sxs-lookup"><span data-stu-id="2b1b7-247">page (Requires ASP.NET Core 2.0 and later)</span></span>
* <span data-ttu-id="2b1b7-248">fonctions</span><span class="sxs-lookup"><span data-stu-id="2b1b7-248">functions</span></span>
* <span data-ttu-id="2b1b7-249">hérite</span><span class="sxs-lookup"><span data-stu-id="2b1b7-249">inherits</span></span>
* <span data-ttu-id="2b1b7-250">modèle</span><span class="sxs-lookup"><span data-stu-id="2b1b7-250">model</span></span>
* <span data-ttu-id="2b1b7-251">section</span><span class="sxs-lookup"><span data-stu-id="2b1b7-251">section</span></span>
* <span data-ttu-id="2b1b7-252">helper (non pris en charge par ASP.NET Core)</span><span class="sxs-lookup"><span data-stu-id="2b1b7-252">helper (Not currently supported by ASP.NET Core)</span></span>

<span data-ttu-id="2b1b7-253">Les mots clés Razor sont précédés d’une séquence d’échappement `@(Razor Keyword)` (par exemple, `@(functions)`).</span><span class="sxs-lookup"><span data-stu-id="2b1b7-253">Razor keywords are escaped with `@(Razor Keyword)` (for example, `@(functions)`).</span></span>

### <a name="c-razor-keywords"></a><span data-ttu-id="2b1b7-254">Mots clés Razor C#</span><span class="sxs-lookup"><span data-stu-id="2b1b7-254">C# Razor keywords</span></span>

* <span data-ttu-id="2b1b7-255">casse</span><span class="sxs-lookup"><span data-stu-id="2b1b7-255">case</span></span>
* <span data-ttu-id="2b1b7-256">do</span><span class="sxs-lookup"><span data-stu-id="2b1b7-256">do</span></span>
* <span data-ttu-id="2b1b7-257">default</span><span class="sxs-lookup"><span data-stu-id="2b1b7-257">default</span></span>
* <span data-ttu-id="2b1b7-258">for</span><span class="sxs-lookup"><span data-stu-id="2b1b7-258">for</span></span>
* <span data-ttu-id="2b1b7-259">foreach</span><span class="sxs-lookup"><span data-stu-id="2b1b7-259">foreach</span></span>
* <span data-ttu-id="2b1b7-260">if</span><span class="sxs-lookup"><span data-stu-id="2b1b7-260">if</span></span>
* <span data-ttu-id="2b1b7-261">else</span><span class="sxs-lookup"><span data-stu-id="2b1b7-261">else</span></span>
* <span data-ttu-id="2b1b7-262">lock</span><span class="sxs-lookup"><span data-stu-id="2b1b7-262">lock</span></span>
* <span data-ttu-id="2b1b7-263">switch</span><span class="sxs-lookup"><span data-stu-id="2b1b7-263">switch</span></span>
* <span data-ttu-id="2b1b7-264">try</span><span class="sxs-lookup"><span data-stu-id="2b1b7-264">try</span></span>
* <span data-ttu-id="2b1b7-265">catch</span><span class="sxs-lookup"><span data-stu-id="2b1b7-265">catch</span></span>
* <span data-ttu-id="2b1b7-266">finally</span><span class="sxs-lookup"><span data-stu-id="2b1b7-266">finally</span></span>
* <span data-ttu-id="2b1b7-267">using</span><span class="sxs-lookup"><span data-stu-id="2b1b7-267">using</span></span>
* <span data-ttu-id="2b1b7-268">while</span><span class="sxs-lookup"><span data-stu-id="2b1b7-268">while</span></span>

<span data-ttu-id="2b1b7-269">Les mots clés Razor C# doivent être précédés d’une double séquence d’échappement `@(@C# Razor Keyword)` (par exemple, `@(@case)`).</span><span class="sxs-lookup"><span data-stu-id="2b1b7-269">C# Razor keywords must be double-escaped with `@(@C# Razor Keyword)` (for example, `@(@case)`).</span></span> <span data-ttu-id="2b1b7-270">La première séquence d’échappement `@` est pour l’analyseur Razor.</span><span class="sxs-lookup"><span data-stu-id="2b1b7-270">The first `@` escapes the Razor parser.</span></span> <span data-ttu-id="2b1b7-271">La seconde séquence d’échappement `@` est pour l’analyseur C#.</span><span class="sxs-lookup"><span data-stu-id="2b1b7-271">The second `@` escapes the C# parser.</span></span>

### <a name="reserved-keywords-not-used-by-razor"></a><span data-ttu-id="2b1b7-272">Mots clés réservés non utilisés par Razor</span><span class="sxs-lookup"><span data-stu-id="2b1b7-272">Reserved keywords not used by Razor</span></span>

* <span data-ttu-id="2b1b7-273">namespace</span><span class="sxs-lookup"><span data-stu-id="2b1b7-273">namespace</span></span>
* <span data-ttu-id="2b1b7-274">class</span><span class="sxs-lookup"><span data-stu-id="2b1b7-274">class</span></span>

## <a name="viewing-the-razor-c-class-generated-for-a-view"></a><span data-ttu-id="2b1b7-275">Affichage de la classe C# Razor générée pour une vue</span><span class="sxs-lookup"><span data-stu-id="2b1b7-275">Viewing the Razor C# class generated for a view</span></span>

<span data-ttu-id="2b1b7-276">Ajoutez la classe suivante au projet ASP.NET Core MVC :</span><span class="sxs-lookup"><span data-stu-id="2b1b7-276">Add the following class to the ASP.NET Core MVC project:</span></span>

[!code-csharp[](razor/sample/Utilities/CustomTemplateEngine.cs)]

<span data-ttu-id="2b1b7-277">Remplacez la classe `RazorTemplateEngine` ajoutée par MVC par la classe `CustomTemplateEngine` :</span><span class="sxs-lookup"><span data-stu-id="2b1b7-277">Override the `RazorTemplateEngine` added by MVC with the `CustomTemplateEngine` class:</span></span>

[!code-csharp[](razor/sample/Startup.cs?highlight=4&range=10-14)]

<span data-ttu-id="2b1b7-278">Définissez un point d’arrêt sur l’instruction `return csharpDocument` de `CustomTemplateEngine`.</span><span class="sxs-lookup"><span data-stu-id="2b1b7-278">Set a break point on the `return csharpDocument` statement of `CustomTemplateEngine`.</span></span> <span data-ttu-id="2b1b7-279">Quand l’exécution du programme s’arrête au point d’arrêt, affichez la valeur de `generatedCode`.</span><span class="sxs-lookup"><span data-stu-id="2b1b7-279">When program execution stops at the break point, view the value of `generatedCode`.</span></span>

![Vue Visualiseur de texte du code généré](razor/_static/tvr.png)

## <a name="view-lookups-and-case-sensitivity"></a><span data-ttu-id="2b1b7-281">Recherches de vues et respect de la casse</span><span class="sxs-lookup"><span data-stu-id="2b1b7-281">View lookups and case sensitivity</span></span>

<span data-ttu-id="2b1b7-282">Le moteur de vue Razor effectue des recherches de vues en respectant la casse.</span><span class="sxs-lookup"><span data-stu-id="2b1b7-282">The Razor view engine performs case-sensitive lookups for views.</span></span> <span data-ttu-id="2b1b7-283">Toutefois, la recherche réellement effectuée est déterminée par le système de fichiers sous-jacent :</span><span class="sxs-lookup"><span data-stu-id="2b1b7-283">However, the actual lookup is determined by the underlying file system:</span></span>

* <span data-ttu-id="2b1b7-284">Source basé sur un fichier :</span><span class="sxs-lookup"><span data-stu-id="2b1b7-284">File based source:</span></span> 
  * <span data-ttu-id="2b1b7-285">Sur les systèmes d’exploitation avec des systèmes de fichiers qui ne respectent pas la casse (par exemple, Windows), les recherches de fournisseurs de fichiers physiques ne respectent pas la casse.</span><span class="sxs-lookup"><span data-stu-id="2b1b7-285">On operating systems with case insensitive file systems (for example, Windows), physical file provider lookups are case insensitive.</span></span> <span data-ttu-id="2b1b7-286">Par exemple, `return View("Test")` trouve les correspondances */Views/Home/Test.cshtml*, */Views/home/test.cshtml* et toutes les autres variantes de casse.</span><span class="sxs-lookup"><span data-stu-id="2b1b7-286">For example, `return View("Test")` results in matches for */Views/Home/Test.cshtml*, */Views/home/test.cshtml*, and any other casing variant.</span></span>
  * <span data-ttu-id="2b1b7-287">Sur des systèmes de fichiers respectant la casse (par exemple, Linux, OSX, et avec `EmbeddedFileProvider`), les recherches respectent la casse.</span><span class="sxs-lookup"><span data-stu-id="2b1b7-287">On case-sensitive file systems (for example, Linux, OSX, and with `EmbeddedFileProvider`), lookups are case-sensitive.</span></span> <span data-ttu-id="2b1b7-288">Par exemple, `return View("Test")` trouve uniquement la correspondance */Views/Home/Test.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="2b1b7-288">For example, `return View("Test")` specifically matches */Views/Home/Test.cshtml*.</span></span>
* <span data-ttu-id="2b1b7-289">Vues précompilées : Avec ASP.NET Core 2.0 et les versions ultérieures, les recherches de vues précompilées ne respectent pas la casse, quels que soient les systèmes d’exploitation.</span><span class="sxs-lookup"><span data-stu-id="2b1b7-289">Precompiled views: With ASP.NET Core 2.0 and later, looking up precompiled views is case insensitive on all operating systems.</span></span> <span data-ttu-id="2b1b7-290">Le comportement est le même que celui du fournisseur de fichiers physiques sur Windows.</span><span class="sxs-lookup"><span data-stu-id="2b1b7-290">The behavior is identical to physical file provider's behavior on Windows.</span></span> <span data-ttu-id="2b1b7-291">Si deux vues précompilées diffèrent seulement par leur casse, le résultat de la recherche est non déterministe.</span><span class="sxs-lookup"><span data-stu-id="2b1b7-291">If two precompiled views differ only in case, the result of lookup is non-deterministic.</span></span>

<span data-ttu-id="2b1b7-292">Les développeurs doivent s’efforcer d’utiliser la même casse pour les noms de fichiers et de répertoires que pour les noms des éléments suivants :</span><span class="sxs-lookup"><span data-stu-id="2b1b7-292">Developers are encouraged to match the casing of file and directory names to the casing of:</span></span>

    * <span data-ttu-id="2b1b7-293">Zone, contrôleur et action</span><span class="sxs-lookup"><span data-stu-id="2b1b7-293">Area, controller, and action names.</span></span> 
    * <span data-ttu-id="2b1b7-294">Pages Razor</span><span class="sxs-lookup"><span data-stu-id="2b1b7-294">Razor Pages.</span></span>
    
<span data-ttu-id="2b1b7-295">L’utilisation d’une casse identique garantit que les déploiements trouvent toujours les vues associées, indépendamment du système de fichiers sous-jacent.</span><span class="sxs-lookup"><span data-stu-id="2b1b7-295">Matching case ensures the deployments find their views regardless of the underlying file system.</span></span>
