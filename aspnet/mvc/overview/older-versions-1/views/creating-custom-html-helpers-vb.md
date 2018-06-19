---
uid: mvc/overview/older-versions-1/views/creating-custom-html-helpers-vb
title: Création de programmes d’assistance HTML personnalisé (VB) | Documents Microsoft
author: microsoft
description: L’objectif de ce didacticiel est de montrer comment vous pouvez créer des applications auxiliaires HTML personnalisé que vous pouvez utiliser dans vos vues MVC. En tirant parti du programme d’assistance HTML...
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/07/2008
ms.topic: article
ms.assetid: f96f4800-19ef-44c0-b457-55e777eb5de8
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/views/creating-custom-html-helpers-vb
msc.type: authoredcontent
ms.openlocfilehash: 6980026e2653eacb71697f9b34def9bc38638726
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30871504"
---
<a name="creating-custom-html-helpers-vb"></a><span data-ttu-id="6b6af-104">Création de programmes d’assistance HTML personnalisé (VB)</span><span class="sxs-lookup"><span data-stu-id="6b6af-104">Creating Custom HTML Helpers (VB)</span></span>
====================
<span data-ttu-id="6b6af-105">par [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="6b6af-105">by [Microsoft](https://github.com/microsoft)</span></span>

[<span data-ttu-id="6b6af-106">Télécharger le PDF</span><span class="sxs-lookup"><span data-stu-id="6b6af-106">Download PDF</span></span>](http://download.microsoft.com/download/1/1/f/11f721aa-d749-4ed7-bb89-a681b68894e6/ASPNET_MVC_Tutorial_9_VB.pdf)

> <span data-ttu-id="6b6af-107">L’objectif de ce didacticiel est de montrer comment vous pouvez créer des applications auxiliaires HTML personnalisé que vous pouvez utiliser dans vos vues MVC.</span><span class="sxs-lookup"><span data-stu-id="6b6af-107">The goal of this tutorial is to demonstrate how you can create custom HTML Helpers that you can use within your MVC views.</span></span> <span data-ttu-id="6b6af-108">En tirant parti des programmes d’assistance HTML, vous pouvez réduire la quantité de saisie fastidieux de balises HTML que vous devez effectuer pour créer une page HTML standard.</span><span class="sxs-lookup"><span data-stu-id="6b6af-108">By taking advantage of HTML Helpers, you can reduce the amount of tedious typing of HTML tags that you must perform to create a standard HTML page.</span></span>


<span data-ttu-id="6b6af-109">L’objectif de ce didacticiel est de montrer comment vous pouvez créer des applications auxiliaires HTML personnalisé que vous pouvez utiliser dans vos vues MVC.</span><span class="sxs-lookup"><span data-stu-id="6b6af-109">The goal of this tutorial is to demonstrate how you can create custom HTML Helpers that you can use within your MVC views.</span></span> <span data-ttu-id="6b6af-110">En tirant parti des programmes d’assistance HTML, vous pouvez réduire la quantité de saisie fastidieux de balises HTML que vous devez effectuer pour créer une page HTML standard.</span><span class="sxs-lookup"><span data-stu-id="6b6af-110">By taking advantage of HTML Helpers, you can reduce the amount of tedious typing of HTML tags that you must perform to create a standard HTML page.</span></span>

<span data-ttu-id="6b6af-111">Dans la première partie de ce didacticiel, je décris parmi les programmes d’assistance HTML existant, inclus avec l’infrastructure ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="6b6af-111">In the first part of this tutorial, I describe some of the existing HTML Helpers included with the ASP.NET MVC framework.</span></span> <span data-ttu-id="6b6af-112">Ensuite, je décris deux méthodes de création des programmes d’assistance HTML personnalisé : vous expliquent comment créer des programmes d’assistance HTML personnalisé en créant une méthode partagée et en créant une méthode d’extension.</span><span class="sxs-lookup"><span data-stu-id="6b6af-112">Next, I describe two methods of creating custom HTML Helpers: I explain how to create custom HTML Helpers by creating a shared method and by creating an extension method.</span></span>

## <a name="understanding-html-helpers"></a><span data-ttu-id="6b6af-113">Présentation des programmes d’assistance HTML</span><span class="sxs-lookup"><span data-stu-id="6b6af-113">Understanding HTML Helpers</span></span>

<span data-ttu-id="6b6af-114">Une application d’assistance HTML est simplement une méthode qui retourne une chaîne.</span><span class="sxs-lookup"><span data-stu-id="6b6af-114">An HTML Helper is just a method that returns a string.</span></span> <span data-ttu-id="6b6af-115">La chaîne peut représenter n’importe quel type de contenu que vous souhaitez.</span><span class="sxs-lookup"><span data-stu-id="6b6af-115">The string can represent any type of content that you want.</span></span> <span data-ttu-id="6b6af-116">Par exemple, vous pouvez utiliser des programmes d’assistance HTML pour restituer des balises HTML standard tels que HTML `<input>` et `<img>` balises.</span><span class="sxs-lookup"><span data-stu-id="6b6af-116">For example, you can use HTML Helpers to render standard HTML tags like HTML `<input>` and `<img>` tags.</span></span> <span data-ttu-id="6b6af-117">Vous pouvez également utiliser des programmes d’assistance HTML pour restituer le contenu plus complexe telles que la bande d’onglets ou d’une table HTML de base de données.</span><span class="sxs-lookup"><span data-stu-id="6b6af-117">You also can use HTML Helpers to render more complex content such as a tab strip or an HTML table of database data.</span></span>

<span data-ttu-id="6b6af-118">L’infrastructure ASP.NET MVC inclut l’ensemble des programmes d’assistance de HTML standard (il ne s’agit pas d’une liste complète) suivantes :</span><span class="sxs-lookup"><span data-stu-id="6b6af-118">The ASP.NET MVC framework includes the following set of standard HTML Helpers (this is not a complete list):</span></span>

- <span data-ttu-id="6b6af-119">Html.ActionLink()</span><span class="sxs-lookup"><span data-stu-id="6b6af-119">Html.ActionLink()</span></span>
- <span data-ttu-id="6b6af-120">Html.BeginForm()</span><span class="sxs-lookup"><span data-stu-id="6b6af-120">Html.BeginForm()</span></span>
- <span data-ttu-id="6b6af-121">Html.CheckBox()</span><span class="sxs-lookup"><span data-stu-id="6b6af-121">Html.CheckBox()</span></span>
- <span data-ttu-id="6b6af-122">Html.DropDownList()</span><span class="sxs-lookup"><span data-stu-id="6b6af-122">Html.DropDownList()</span></span>
- <span data-ttu-id="6b6af-123">Html.EndForm()</span><span class="sxs-lookup"><span data-stu-id="6b6af-123">Html.EndForm()</span></span>
- <span data-ttu-id="6b6af-124">Html.Hidden()</span><span class="sxs-lookup"><span data-stu-id="6b6af-124">Html.Hidden()</span></span>
- <span data-ttu-id="6b6af-125">Html.ListBox()</span><span class="sxs-lookup"><span data-stu-id="6b6af-125">Html.ListBox()</span></span>
- <span data-ttu-id="6b6af-126">Html.Password()</span><span class="sxs-lookup"><span data-stu-id="6b6af-126">Html.Password()</span></span>
- <span data-ttu-id="6b6af-127">Html.RadioButton()</span><span class="sxs-lookup"><span data-stu-id="6b6af-127">Html.RadioButton()</span></span>
- <span data-ttu-id="6b6af-128">Html.TextArea()</span><span class="sxs-lookup"><span data-stu-id="6b6af-128">Html.TextArea()</span></span>
- <span data-ttu-id="6b6af-129">Html.TextBox()</span><span class="sxs-lookup"><span data-stu-id="6b6af-129">Html.TextBox()</span></span>

<span data-ttu-id="6b6af-130">Par exemple, considérez le formulaire dans la liste 1.</span><span class="sxs-lookup"><span data-stu-id="6b6af-130">For example, consider the form in Listing 1.</span></span> <span data-ttu-id="6b6af-131">Ce formulaire est affiché à l’aide de deux des programmes d’assistance HTML standard (voir Figure 1).</span><span class="sxs-lookup"><span data-stu-id="6b6af-131">This form is rendered with the help of two of the standard HTML Helpers (see Figure 1).</span></span> <span data-ttu-id="6b6af-132">Ce formulaire utilise la `Html.BeginForm()` et `Html.TextBox()` méthodes d’assistance.</span><span class="sxs-lookup"><span data-stu-id="6b6af-132">This form uses the `Html.BeginForm()` and `Html.TextBox()` Helper methods.</span></span>


<span data-ttu-id="6b6af-133">[![Page rendue avec des programmes d’assistance HTML](creating-custom-html-helpers-vb/_static/image2.png)](creating-custom-html-helpers-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="6b6af-133">[![Page rendered with HTML Helpers](creating-custom-html-helpers-vb/_static/image2.png)](creating-custom-html-helpers-vb/_static/image1.png)</span></span>

<span data-ttu-id="6b6af-134">**Figure 01**: Page rendue avec des programmes d’assistance HTML ([cliquez pour afficher l’image en taille réelle](creating-custom-html-helpers-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="6b6af-134">**Figure 01**: Page rendered with HTML Helpers ([Click to view full-size image](creating-custom-html-helpers-vb/_static/image3.png))</span></span>


<span data-ttu-id="6b6af-135">**La liste 1 : `Views\Home\Index.aspx`**</span><span class="sxs-lookup"><span data-stu-id="6b6af-135">**Listing 1 – `Views\Home\Index.aspx`**</span></span>

[!code-aspx[Main](creating-custom-html-helpers-vb/samples/sample1.aspx)]

<span data-ttu-id="6b6af-136">Le `Html.BeginForm()` méthode d’assistance est utilisé pour créer l’ouverture et de fermeture HTML `<form>` balises.</span><span class="sxs-lookup"><span data-stu-id="6b6af-136">The `Html.BeginForm()` Helper method is used to create the opening and closing HTML `<form>` tags.</span></span> <span data-ttu-id="6b6af-137">Notez que le `Html.BeginForm()` méthode est appelée au sein d’un à l’aide de déclaration.</span><span class="sxs-lookup"><span data-stu-id="6b6af-137">Notice that the `Html.BeginForm()` method is called within a using statement.</span></span> <span data-ttu-id="6b6af-138">L’à l’aide d’instruction garantit que le `<form>` balise est fermé à la fin de l’utilisation bloc.</span><span class="sxs-lookup"><span data-stu-id="6b6af-138">The using statement ensures that the `<form>` tag gets closed at the end of the using block.</span></span>

<span data-ttu-id="6b6af-139">Si vous préférez, au lieu de créer un à l’aide de bloc, vous pouvez appeler la méthode d’assistance de Html.EndForm() pour fermer la `<form>` balise.</span><span class="sxs-lookup"><span data-stu-id="6b6af-139">If you prefer, instead of creating a using block, you can call the Html.EndForm() Helper method to close the `<form>` tag.</span></span> <span data-ttu-id="6b6af-140">Utiliser l’approche de création d’ouverture et fermeture `<form>` balise semble plus intuitif.</span><span class="sxs-lookup"><span data-stu-id="6b6af-140">Use whichever approach to creating an opening and closing `<form>` tag that seems most intuitive to you.</span></span>

<span data-ttu-id="6b6af-141">Le `Html.TextBox()` méthodes d’assistance sont utilisés dans la liste 1 pour le rendu HTML `<input>` balises.</span><span class="sxs-lookup"><span data-stu-id="6b6af-141">The `Html.TextBox()` Helper methods are used in Listing 1 to render HTML `<input>` tags.</span></span> <span data-ttu-id="6b6af-142">Si vous sélectionnez Afficher la source dans votre navigateur affiche la source HTML dans le Listing 2.</span><span class="sxs-lookup"><span data-stu-id="6b6af-142">If you select view source in your browser then you see the HTML source in Listing 2.</span></span> <span data-ttu-id="6b6af-143">Notez que la source contient des balises HTML standard.</span><span class="sxs-lookup"><span data-stu-id="6b6af-143">Notice that the source contains standard HTML tags.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="6b6af-144">Notez que la `Html.TextBox()`-HTML rendue avec application d’assistance `<%= %>` balises au lieu de `<% %>` balises.</span><span class="sxs-lookup"><span data-stu-id="6b6af-144">notice that the `Html.TextBox()`-HTML Helper is rendered with `<%= %>` tags instead of `<% %>` tags.</span></span> <span data-ttu-id="6b6af-145">Si vous n’incluez pas le signe égal, puis rien n’est restitué dans le navigateur.</span><span class="sxs-lookup"><span data-stu-id="6b6af-145">If you don't include the equal sign, then nothing gets rendered to the browser.</span></span>

<span data-ttu-id="6b6af-146">L’infrastructure ASP.NET MVC contient un petit ensemble de programmes d’assistance.</span><span class="sxs-lookup"><span data-stu-id="6b6af-146">The ASP.NET MVC framework contains a small set of helpers.</span></span> <span data-ttu-id="6b6af-147">Très probablement, vous devez étendre l’infrastructure MVC avec des programmes d’assistance HTML personnalisé.</span><span class="sxs-lookup"><span data-stu-id="6b6af-147">Most likely, you will need to extend the MVC framework with custom HTML Helpers.</span></span> <span data-ttu-id="6b6af-148">Dans le reste de ce didacticiel, vous découvrez des deux méthodes de création des programmes d’assistance HTML personnalisé.</span><span class="sxs-lookup"><span data-stu-id="6b6af-148">In the remainder of this tutorial, you learn two methods of creating custom HTML Helpers.</span></span>

<span data-ttu-id="6b6af-149">**Liste 2 : `Index.aspx Source`**</span><span class="sxs-lookup"><span data-stu-id="6b6af-149">**Listing 2 – `Index.aspx Source`**</span></span>

[!code-aspx[Main](creating-custom-html-helpers-vb/samples/sample2.aspx)]

### <a name="creating-html-helpers-with-shared-methods"></a><span data-ttu-id="6b6af-150">Création de programmes d’assistance HTML avec des méthodes partagées</span><span class="sxs-lookup"><span data-stu-id="6b6af-150">Creating HTML Helpers with Shared Methods</span></span>

<span data-ttu-id="6b6af-151">Le moyen le plus simple pour créer un programme d’assistance HTML nouvelle est pour créer une méthode partagée qui retourne une chaîne.</span><span class="sxs-lookup"><span data-stu-id="6b6af-151">The easiest way to create a new HTML Helper is to create a shared method that returns a string.</span></span> <span data-ttu-id="6b6af-152">Par exemple, imaginez que vous décidez de créer un nouveau programme d’assistance HTML qui effectue le rendu HTML `<label>` balise.</span><span class="sxs-lookup"><span data-stu-id="6b6af-152">Imagine, for example, that you decide to create a new HTML Helper that renders an HTML `<label>` tag.</span></span> <span data-ttu-id="6b6af-153">Vous pouvez utiliser la classe dans la liste 2 pour restituer un `<label>`.</span><span class="sxs-lookup"><span data-stu-id="6b6af-153">You can use the class in Listing 2 to render a `<label>`.</span></span>

<span data-ttu-id="6b6af-154">**Liste 2 : `Helpers\LabelHelper.vb`**</span><span class="sxs-lookup"><span data-stu-id="6b6af-154">**Listing 2 – `Helpers\LabelHelper.vb`**</span></span>

[!code-vb[Main](creating-custom-html-helpers-vb/samples/sample3.vb)]

<span data-ttu-id="6b6af-155">Il n’a rien de spécial sur la classe dans la liste 2.</span><span class="sxs-lookup"><span data-stu-id="6b6af-155">There is nothing special about the class in Listing 2.</span></span> <span data-ttu-id="6b6af-156">Le `Label()` méthode retourne simplement une chaîne.</span><span class="sxs-lookup"><span data-stu-id="6b6af-156">The `Label()` method simply returns a string.</span></span>

<span data-ttu-id="6b6af-157">La vue Index modifiée dans la liste 3 utilise le `LabelHelper` pour le rendu HTML `<label>` balises.</span><span class="sxs-lookup"><span data-stu-id="6b6af-157">The modified Index view in Listing 3 uses the `LabelHelper` to render HTML `<label>` tags.</span></span> <span data-ttu-id="6b6af-158">Notez que la vue inclut une `<%@ imports %>` directive qui importe l’espace de noms Application1.Helpers.</span><span class="sxs-lookup"><span data-stu-id="6b6af-158">Notice that the view includes an `<%@ imports %>` directive that imports the Application1.Helpers namespace.</span></span>

<span data-ttu-id="6b6af-159">**Liste 2 : `Views\Home\Index2.aspx`**</span><span class="sxs-lookup"><span data-stu-id="6b6af-159">**Listing 2 – `Views\Home\Index2.aspx`**</span></span>

[!code-aspx[Main](creating-custom-html-helpers-vb/samples/sample4.aspx)]

### <a name="creating-html-helpers-with-extension-methods"></a><span data-ttu-id="6b6af-160">Création de programmes d’assistance HTML avec les méthodes d’Extension</span><span class="sxs-lookup"><span data-stu-id="6b6af-160">Creating HTML Helpers with Extension Methods</span></span>

<span data-ttu-id="6b6af-161">Si vous souhaitez créer des programmes d’assistance HTML qui fonctionnent comme les programmes d’assistance HTML standard inclus dans l’infrastructure ASP.NET MVC, vous devez créer des méthodes d’extension.</span><span class="sxs-lookup"><span data-stu-id="6b6af-161">If you want to create HTML Helpers that work just like the standard HTML Helpers included in the ASP.NET MVC framework then you need to create extension methods.</span></span> <span data-ttu-id="6b6af-162">Méthodes d’extension permettent d’ajouter de nouvelles méthodes à une classe existante.</span><span class="sxs-lookup"><span data-stu-id="6b6af-162">Extension methods enable you to add new methods to an existing class.</span></span> <span data-ttu-id="6b6af-163">Lorsque vous créez une méthode de programme d’assistance HTML, vous ajoutez de nouvelles méthodes pour la `HtmlHelper` classe représentée par la propriété de Html d’un affichage.</span><span class="sxs-lookup"><span data-stu-id="6b6af-163">When creating an HTML Helper method, you add new methods to the `HtmlHelper` class represented by a view's Html property.</span></span>

<span data-ttu-id="6b6af-164">Le module Visual Basic dans la liste 3 ajoute une méthode d’extension nommée `Label()` à la `HtmlHelper` classe.</span><span class="sxs-lookup"><span data-stu-id="6b6af-164">The Visual Basic module in Listing 3 adds an extension method named `Label()` to the `HtmlHelper` class.</span></span> <span data-ttu-id="6b6af-165">Il existe plusieurs choses que vous devez remarquer sur ce module.</span><span class="sxs-lookup"><span data-stu-id="6b6af-165">There are a couple of things that you should notice about this module.</span></span> <span data-ttu-id="6b6af-166">Tout d’abord, notez que le module est décoré avec le `<Extension()>` attribut.</span><span class="sxs-lookup"><span data-stu-id="6b6af-166">First, notice that the module is decorated with the `<Extension()>` attribute.</span></span> <span data-ttu-id="6b6af-167">Pour utiliser cet attribut, vous devez importer le `System.Runtime.CompilerServices` espace de noms</span><span class="sxs-lookup"><span data-stu-id="6b6af-167">In order to use this attribute, you must import the `System.Runtime.CompilerServices` namespace</span></span>

<span data-ttu-id="6b6af-168">En second lieu, notez que le premier paramètre de la `Label()` méthode représente la `HtmlHelper` classe.</span><span class="sxs-lookup"><span data-stu-id="6b6af-168">Second, notice that the first parameter of the `Label()` method represents the `HtmlHelper` class.</span></span> <span data-ttu-id="6b6af-169">Le premier paramètre d’une méthode d’extension indique la classe qui étend de la méthode d’extension.</span><span class="sxs-lookup"><span data-stu-id="6b6af-169">The first parameter of an extension method indicates the class that the extension method extends.</span></span>

<span data-ttu-id="6b6af-170">**La liste 3 : `Helpers\LabelExtensions.vb`**</span><span class="sxs-lookup"><span data-stu-id="6b6af-170">**Listing 3 – `Helpers\LabelExtensions.vb`**</span></span>

[!code-vb[Main](creating-custom-html-helpers-vb/samples/sample5.vb)]

<span data-ttu-id="6b6af-171">Après avoir créé une méthode d’extension et que vous générez votre application avec succès, la méthode d’extension s’affiche dans Visual Studio Intellisense, telles que toutes les autres méthodes d’une classe (voir Figure 2).</span><span class="sxs-lookup"><span data-stu-id="6b6af-171">After you create an extension method, and build your application successfully, the extension method appears in Visual Studio Intellisense like all of the other methods of a class (see Figure 2).</span></span> <span data-ttu-id="6b6af-172">La seule différence est qu’extension méthodes apparaissent avec un symbole spécial en regard des (il s’agit d’une icône de flèche vers le bas).</span><span class="sxs-lookup"><span data-stu-id="6b6af-172">The only difference is that extension methods appear with a special symbol next to them (an icon of a downward arrow).</span></span>


<span data-ttu-id="6b6af-173">[![À l’aide de la méthode d’extension Html.Label()](creating-custom-html-helpers-vb/_static/image5.png)](creating-custom-html-helpers-vb/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="6b6af-173">[![Using the Html.Label() extension method](creating-custom-html-helpers-vb/_static/image5.png)](creating-custom-html-helpers-vb/_static/image4.png)</span></span>

<span data-ttu-id="6b6af-174">**Figure 02**: à l’aide de la méthode d’extension Html.Label() ([cliquez pour afficher l’image en taille réelle](creating-custom-html-helpers-vb/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="6b6af-174">**Figure 02**: Using the Html.Label() extension method  ([Click to view full-size image](creating-custom-html-helpers-vb/_static/image6.png))</span></span>


<span data-ttu-id="6b6af-175">La vue Index modifiée sur la liste 4 utilise la méthode d’extension Html.Label() à restituer tous ses &lt;étiquette&gt; balises.</span><span class="sxs-lookup"><span data-stu-id="6b6af-175">The modified Index view in Listing 4 uses the Html.Label() extension method to render all of its &lt;label&gt; tags.</span></span>

<span data-ttu-id="6b6af-176">**La liste 4 – `Views\Home\Index3.aspx`**</span><span class="sxs-lookup"><span data-stu-id="6b6af-176">**Listing 4 – `Views\Home\Index3.aspx`**</span></span>

[!code-aspx[Main](creating-custom-html-helpers-vb/samples/sample6.aspx)]

## <a name="summary"></a><span data-ttu-id="6b6af-177">Récapitulatif</span><span class="sxs-lookup"><span data-stu-id="6b6af-177">Summary</span></span>

<span data-ttu-id="6b6af-178">Dans ce didacticiel, vous avez appris à deux méthodes de création des programmes d’assistance HTML personnalisé.</span><span class="sxs-lookup"><span data-stu-id="6b6af-178">In this tutorial, you learned two methods of creating custom HTML Helpers.</span></span> <span data-ttu-id="6b6af-179">Tout d’abord, vous avez appris à créer un personnalisé `Label()` programme d’assistance HTML en créant une méthode partagée qui retourne une chaîne.</span><span class="sxs-lookup"><span data-stu-id="6b6af-179">First, you learned how to create a custom `Label()` HTML Helper by creating a shared method that returns a string.</span></span> <span data-ttu-id="6b6af-180">Ensuite, vous avez appris à créer un personnalisé `Label()` méthode du programme d’assistance HTML en créant une méthode d’extension sur la `HtmlHelper` classe.</span><span class="sxs-lookup"><span data-stu-id="6b6af-180">Next, you learned how to create a custom `Label()` HTML Helper method by creating an extension method on the `HtmlHelper` class.</span></span>

<span data-ttu-id="6b6af-181">Dans ce didacticiel, I se concentre sur la création d’une méthode de programme d’assistance HTML extrêmement simple.</span><span class="sxs-lookup"><span data-stu-id="6b6af-181">In this tutorial, I focused on building an extremely simple HTML Helper method.</span></span> <span data-ttu-id="6b6af-182">Notez qu’une application d’assistance HTML peuvent être aussi complexe que vous le souhaitez.</span><span class="sxs-lookup"><span data-stu-id="6b6af-182">Realize that an HTML Helper can be as complicated as you want.</span></span> <span data-ttu-id="6b6af-183">Vous pouvez générer des programmes d’assistance HTML qui restituent contenu riche, tels que les vues de l’arborescence, les menus ou les tables de base de données.</span><span class="sxs-lookup"><span data-stu-id="6b6af-183">You can build HTML Helpers that render rich content such as tree views, menus, or tables of database data.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="6b6af-184">[Précédent](asp-net-mvc-views-overview-vb.md)
> [Suivant](using-the-tagbuilder-class-to-build-html-helpers-vb.md)</span><span class="sxs-lookup"><span data-stu-id="6b6af-184">[Previous](asp-net-mvc-views-overview-vb.md)
[Next](using-the-tagbuilder-class-to-build-html-helpers-vb.md)</span></span>
