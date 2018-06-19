---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/layouts
title: Présentation des Pages Web ASP.NET - création d’une disposition cohérente | Documents Microsoft
author: tfitzmac
description: Ce didacticiel vous montre comment utiliser des dispositions pour créer une apparence cohérente pour les pages sur un site qui utilise ASP.NET Web Pages. Il suppose que vous avez terminé le...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/28/2015
ms.topic: article
ms.assetid: c85ec591-f8d7-4882-b763-de6ab9f3df7a
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/layouts
msc.type: authoredcontent
ms.openlocfilehash: c2d5c4d8ed8a71979c16d484ab90d283a45de537
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30899812"
---
<a name="introducing-aspnet-web-pages---creating-a-consistent-layout"></a><span data-ttu-id="3fd78-104">Présentation des Pages Web ASP.NET - création d’une disposition cohérente</span><span class="sxs-lookup"><span data-stu-id="3fd78-104">Introducing ASP.NET Web Pages - Creating a Consistent Layout</span></span>
====================
<span data-ttu-id="3fd78-105">par [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="3fd78-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="3fd78-106">Ce didacticiel vous montre comment utiliser *dispositions* pour créer une apparence cohérente pour les pages sur un site qui utilise ASP.NET Web Pages.</span><span class="sxs-lookup"><span data-stu-id="3fd78-106">This tutorial shows you how to use *layouts* to create a consistent look for the pages on a site that uses ASP.NET Web Pages.</span></span> <span data-ttu-id="3fd78-107">Il suppose que vous avez terminé la série via [suppression des données de base de données dans les Pages Web ASP.NET](https://go.microsoft.com/fwlink/?LinkId=251584).</span><span class="sxs-lookup"><span data-stu-id="3fd78-107">It assumes you have completed the series through [Deleting Database Data in ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=251584).</span></span>
> 
> <span data-ttu-id="3fd78-108">Ce que vous allez apprendre :</span><span class="sxs-lookup"><span data-stu-id="3fd78-108">What you'll learn:</span></span>
> 
> - <span data-ttu-id="3fd78-109">Est d’une page mise en page.</span><span class="sxs-lookup"><span data-stu-id="3fd78-109">What a layout page is.</span></span>
> - <span data-ttu-id="3fd78-110">Explique comment combiner des pages de disposition avec un contenu dynamique.</span><span class="sxs-lookup"><span data-stu-id="3fd78-110">How to combine layout pages with dynamic content.</span></span>
> - <span data-ttu-id="3fd78-111">Explique comment passer des valeurs à une page de disposition.</span><span class="sxs-lookup"><span data-stu-id="3fd78-111">How to pass values to a layout page.</span></span>


## <a name="about-layouts"></a><span data-ttu-id="3fd78-112">À propos des dispositions</span><span class="sxs-lookup"><span data-stu-id="3fd78-112">About Layouts</span></span>

<span data-ttu-id="3fd78-113">Les pages que vous avez créé jusqu'à présent ont tous été terminées, les pages autonome.</span><span class="sxs-lookup"><span data-stu-id="3fd78-113">The pages you've created so far have all been complete, standalone pages.</span></span> <span data-ttu-id="3fd78-114">Ils appartiennent au même site, mais ils n’ont pas les éléments en commun ou un aspect standard.</span><span class="sxs-lookup"><span data-stu-id="3fd78-114">They all belong to the same site, but they don't have any common elements or a standard look.</span></span>

<span data-ttu-id="3fd78-115">La plupart des sites ont une apparence cohérente et la présentation.</span><span class="sxs-lookup"><span data-stu-id="3fd78-115">Most sites do have a consistent look and layout.</span></span> <span data-ttu-id="3fd78-116">Par exemple, si vous accédez à la [Web Microsoft.com à](https://www.microsoft.com/web/) de site et observer, vous voyez que les pages de tous les conformes à la disposition générale et à un thème visuel :</span><span class="sxs-lookup"><span data-stu-id="3fd78-116">For example, if you go to the [Microsoft.com/web](https://www.microsoft.com/web/) site and look around, you see that the pages all adhere to an overall layout and to a visual theme:</span></span>

![Page du site Web Microsoft.com à représentant la disposition de l’en-tête de zone de navigation, zone de contenu et un pied de page](layouts/_static/image1.png)

<span data-ttu-id="3fd78-118">Un *inefficace* pour créer cette disposition serait pour définir un en-tête, une barre de navigation et un pied de page séparément sur chaque page.</span><span class="sxs-lookup"><span data-stu-id="3fd78-118">An *inefficient* way to create this layout would be to define a header, navigation bar, and footer separately on each of your pages.</span></span> <span data-ttu-id="3fd78-119">Vous serez de dupliquer le balisage même chaque fois.</span><span class="sxs-lookup"><span data-stu-id="3fd78-119">You'd be duplicating the same markup each time.</span></span> <span data-ttu-id="3fd78-120">Si vous souhaitez modifier un objet (par exemple, mettre à jour le pied de page), vous devez modifier chaque page séparément.</span><span class="sxs-lookup"><span data-stu-id="3fd78-120">If you wanted to change something (for example, update the footer), you'd have to change each page separately.</span></span>

<span data-ttu-id="3fd78-121">C’est là *pages de disposition* sont disponibles dans.</span><span class="sxs-lookup"><span data-stu-id="3fd78-121">That's where *layout pages* come in.</span></span> <span data-ttu-id="3fd78-122">Dans ASP.NET Web Pages, vous pouvez définir une page de disposition qui fournit un conteneur global pour les pages de votre site.</span><span class="sxs-lookup"><span data-stu-id="3fd78-122">In ASP.NET Web Pages, you can define a layout page that provides an overall container for pages on your site.</span></span> <span data-ttu-id="3fd78-123">Par exemple, la page de disposition permettre contenir l’en-tête, la zone de navigation et le pied de page.</span><span class="sxs-lookup"><span data-stu-id="3fd78-123">For example, the layout page can contain the header, navigation area, and footer.</span></span> <span data-ttu-id="3fd78-124">La page de disposition inclut un espace réservé emplacement du contenu principal.</span><span class="sxs-lookup"><span data-stu-id="3fd78-124">The layout page includes a placeholder where the main content goes.</span></span>

<span data-ttu-id="3fd78-125">Vous pouvez ensuite définir des pages de contenu individuelles qui contiennent le balisage et le code pour que cette page.</span><span class="sxs-lookup"><span data-stu-id="3fd78-125">You can then define individual content pages that contain the markup and the code for only that page.</span></span> <span data-ttu-id="3fd78-126">Pages de contenu n’êtes pas obligé d’être des pages HTML complètes ; ils n’ont pas d’avoir un `<body>` élément.</span><span class="sxs-lookup"><span data-stu-id="3fd78-126">Content pages don't have to be complete HTML pages; they don't even have to have a `<body>` element.</span></span> <span data-ttu-id="3fd78-127">Ils ont également une ligne de code qui indique à ASP.NET de quelle page de disposition que vous souhaitez afficher le contenu de.</span><span class="sxs-lookup"><span data-stu-id="3fd78-127">They also have a line of code that tells ASP.NET what layout page you want to display the content in.</span></span> <span data-ttu-id="3fd78-128">Voici une image qui indique à peu près le fonctionne de cette relation :</span><span class="sxs-lookup"><span data-stu-id="3fd78-128">Here's a picture that shows roughly how this relationship works:</span></span>

![Diagramme conceptuel qui montre les deux pages de contenu et une page de disposition dans lequel elles s’adaptent](layouts/_static/image2.png)

<span data-ttu-id="3fd78-130">Cette interaction est facile à comprendre lorsque vous la voir en action.</span><span class="sxs-lookup"><span data-stu-id="3fd78-130">This interaction is easy to understand when you see it in action.</span></span> <span data-ttu-id="3fd78-131">Dans ce didacticiel, vous allez modifier vos pages de films pour utiliser une disposition.</span><span class="sxs-lookup"><span data-stu-id="3fd78-131">In this tutorial, you'll change your movies pages to use a layout.</span></span>

## <a name="adding-a-layout-page"></a><span data-ttu-id="3fd78-132">Ajout d’une Page de disposition</span><span class="sxs-lookup"><span data-stu-id="3fd78-132">Adding a Layout Page</span></span>

<span data-ttu-id="3fd78-133">Vous allez commencer par créer une page de disposition qui définit une disposition de page classique avec un en-tête, un pied de page et une zone pour le contenu principal.</span><span class="sxs-lookup"><span data-stu-id="3fd78-133">You'll start by creating a layout page that defines a typical page layout with a header, footer, and an area for the main content.</span></span> <span data-ttu-id="3fd78-134">Dans le site WebPagesMovies, ajouter une page CSHTML nommée  *\_Layout.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="3fd78-134">In the WebPagesMovies site, add a CSHTML page named *\_Layout.cshtml*.</span></span>

<span data-ttu-id="3fd78-135">Le trait de soulignement ( `_` ) caractère est significatif.</span><span class="sxs-lookup"><span data-stu-id="3fd78-135">The leading underscore ( `_` ) character is significant.</span></span> <span data-ttu-id="3fd78-136">Si un nom de la page commence par un trait de soulignement, ASP.NET ne sont pas envoyer directement cette page au navigateur.</span><span class="sxs-lookup"><span data-stu-id="3fd78-136">If a page's name starts with an underscore, ASP.NET won't directly send that page to the browser.</span></span> <span data-ttu-id="3fd78-137">Cette convention permet de définir les pages qui sont requis pour votre site, mais que les utilisateurs ne doivent pas être en mesure de demander directement.</span><span class="sxs-lookup"><span data-stu-id="3fd78-137">This convention lets you define pages that are required for your site but that users shouldn't be able to request directly.</span></span>

<span data-ttu-id="3fd78-138">Remplacez le contenu de la page avec les éléments suivants :</span><span class="sxs-lookup"><span data-stu-id="3fd78-138">Replace the content in the page with the following:</span></span>

[!code-html[Main](layouts/samples/sample1.html)]

<span data-ttu-id="3fd78-139">Comme vous pouvez le voir, ce balisage est simplement le code HTML qui utilise `<div>` éléments pour définir les trois sections de la page plus un plus `<div>` élément pour contenir les trois sections.</span><span class="sxs-lookup"><span data-stu-id="3fd78-139">As you can see, this markup is just HTML that uses `<div>` elements to define three sections in the page plus one more `<div>` element to hold the three sections.</span></span> <span data-ttu-id="3fd78-140">Le pied de page contient un peu de code Razor : `@DateTime.Now.Year`, qui restituera l’année en cours à cet emplacement dans la page.</span><span class="sxs-lookup"><span data-stu-id="3fd78-140">The footer contains a bit of Razor code: `@DateTime.Now.Year`, which will render the current year at that location in the page.</span></span>

<span data-ttu-id="3fd78-141">Notez qu’il existe un lien vers une feuille de style nommée *Movies.css*.</span><span class="sxs-lookup"><span data-stu-id="3fd78-141">Notice that there's a link to a style sheet named *Movies.css*.</span></span> <span data-ttu-id="3fd78-142">La feuille de style est où les détails de la disposition physique des éléments sont définies.</span><span class="sxs-lookup"><span data-stu-id="3fd78-142">The style sheet is where the details of the physical layout of the elements will be defined.</span></span> <span data-ttu-id="3fd78-143">Vous allez créer que dans un instant.</span><span class="sxs-lookup"><span data-stu-id="3fd78-143">You'll create that in a moment.</span></span>

<span data-ttu-id="3fd78-144">La fonctionnalité uniquement inhabituelle dans cette  *\_Layout.cshtml* page est la `@Render.Body()` ligne.</span><span class="sxs-lookup"><span data-stu-id="3fd78-144">The only unusual feature in this *\_Layout.cshtml* page is the `@Render.Body()` line.</span></span> <span data-ttu-id="3fd78-145">Qui est l’espace réservé où le contenu sortent cette disposition est fusionnée avec une autre page.</span><span class="sxs-lookup"><span data-stu-id="3fd78-145">That's the placeholder where the content will go when this layout is merged with another page.</span></span>

## <a name="adding-a-css-file"></a><span data-ttu-id="3fd78-146">Ajout d’un fichier .css</span><span class="sxs-lookup"><span data-stu-id="3fd78-146">Adding a .css File</span></span>

<span data-ttu-id="3fd78-147">La meilleure façon de définir la disposition réelle (autrement dit, apparence) des éléments de la page consiste à utiliser les règles de feuille de style en cascade.</span><span class="sxs-lookup"><span data-stu-id="3fd78-147">The preferred way to define the actual arrangement (that is, appearance) of elements on the page is to use cascading style sheet (CSS) rules.</span></span> <span data-ttu-id="3fd78-148">Afin que vous allez créer un *.css* fichier dont les règles pour votre nouvelle disposition.</span><span class="sxs-lookup"><span data-stu-id="3fd78-148">So you'll create a *.css* file that has the rules for your new layout.</span></span>

<span data-ttu-id="3fd78-149">Dans WebMatrix, sélectionnez la racine de votre site.</span><span class="sxs-lookup"><span data-stu-id="3fd78-149">In WebMatrix, select the root of your site.</span></span> <span data-ttu-id="3fd78-150">Ensuite, dans le **fichiers** onglet du ruban, cliquez sur la flèche sous le **nouveau** puis cliquez sur **nouveau dossier**.</span><span class="sxs-lookup"><span data-stu-id="3fd78-150">Then in the **Files** tab of the ribbon, click the arrow under the **New** button and then click **New Folder**.</span></span>

![L’option « Nouveau dossier » sous Nouveau dans le ruban.](layouts/_static/image3.png)

<span data-ttu-id="3fd78-152">Nommez le nouveau dossier *Styles*.</span><span class="sxs-lookup"><span data-stu-id="3fd78-152">Name the new folder *Styles*.</span></span>

![Le nouveau dossier d’affectation de noms 'Styles'](layouts/_static/image4.png)

<span data-ttu-id="3fd78-154">À l’intérieur de la nouvelle *Styles* dossier, créez un fichier nommé *Movies.css*.</span><span class="sxs-lookup"><span data-stu-id="3fd78-154">Inside the new *Styles* folder, create a file named *Movies.css*.</span></span>

![Création d’un fichier Movies.css](layouts/_static/image5.png)

<span data-ttu-id="3fd78-156">Remplacez le contenu de la nouvelle *.css* fichier avec les éléments suivants :</span><span class="sxs-lookup"><span data-stu-id="3fd78-156">Replace the contents of the new *.css* file with the following:</span></span>

[!code-css[Main](layouts/samples/sample2.css)]

<span data-ttu-id="3fd78-157">Nous ne nous dit pas grand-chose sur ces règles CSS, sauf pour noter deux choses.</span><span class="sxs-lookup"><span data-stu-id="3fd78-157">We won't say much about these CSS rules, except to note two things.</span></span> <span data-ttu-id="3fd78-158">Un est qu’en plus de définir les tailles et polices, les règles utilisent le positionnement absolu pour établir l’emplacement de l’en-tête, le pied de page et la zone de contenu principal.</span><span class="sxs-lookup"><span data-stu-id="3fd78-158">One is that in addition to setting fonts and sizes, the rules use absolute positioning to establish the location of the header, footer, and main content area.</span></span> <span data-ttu-id="3fd78-159">Si vous débutez de positionnement dans CSS, vous pouvez lire la [positionnement CSS](http://www.w3schools.com/css/css_positioning.asp) didacticiel sur le site W3Schools.</span><span class="sxs-lookup"><span data-stu-id="3fd78-159">If you're new to positioning in CSS, you can read the [CSS Positioning](http://www.w3schools.com/css/css_positioning.asp) tutorial at the W3Schools site.</span></span>

<span data-ttu-id="3fd78-160">L’autre chose à noter est bas, que nous avons copié les règles de style qui ont été initialement défini individuellement dans les *Movies.cshtml* fichier.</span><span class="sxs-lookup"><span data-stu-id="3fd78-160">The other thing to note is that at the bottom, we've copied the style rules that were originally defined individually in the *Movies.cshtml* file.</span></span> <span data-ttu-id="3fd78-161">Ces règles ont été utilisées dans le [Introduction à l’affichage des données à l’aide des Pages Web ASP.NET](https://go.microsoft.com/fwlink/?LinkId=251580) didacticiel pour rendre le `WebGrid` helper restituer un balisage qui a ajouté des bandes à la table.</span><span class="sxs-lookup"><span data-stu-id="3fd78-161">These rules were used in the [Introduction to Displaying Data by Using ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=251580) tutorial to make the `WebGrid` helper render markup that added stripes to the table.</span></span> <span data-ttu-id="3fd78-162">(Si vous prévoyez d’utiliser un *.css* fichier pour les définitions de style, vous pouvez également placer les règles de style pour l’ensemble du site.)</span><span class="sxs-lookup"><span data-stu-id="3fd78-162">(If you're going to use a *.css* file for style definitions, you might as well put the style rules for the whole site in it.)</span></span>

## <a name="updating-the-movies-file-to-use-the-layout"></a><span data-ttu-id="3fd78-163">Mise à jour le fichier de films pour utiliser la mise en page</span><span class="sxs-lookup"><span data-stu-id="3fd78-163">Updating the Movies File to Use the Layout</span></span>

<span data-ttu-id="3fd78-164">Vous pouvez maintenant mettre à jour les fichiers existants dans votre site d’utiliser la nouvelle disposition.</span><span class="sxs-lookup"><span data-stu-id="3fd78-164">Now you can update the existing files in your site to use the new layout.</span></span> <span data-ttu-id="3fd78-165">Ouvrez le *Movies.cshtml* fichier.</span><span class="sxs-lookup"><span data-stu-id="3fd78-165">Open the *Movies.cshtml* file.</span></span> <span data-ttu-id="3fd78-166">En haut, la première ligne de code, ajoutez le code suivant :</span><span class="sxs-lookup"><span data-stu-id="3fd78-166">At the top, as the first line of code, add the following:</span></span>

[!code-csharp[Main](layouts/samples/sample3.cs)]

<span data-ttu-id="3fd78-167">La page démarre désormais ainsi :</span><span class="sxs-lookup"><span data-stu-id="3fd78-167">The page now starts out this way:</span></span>

[!code-cshtml[Main](layouts/samples/sample4.cshtml?highlight=2)]

<span data-ttu-id="3fd78-168">Cette ligne de code indique à ASP.NET que quand le *films* page s’exécute, il doit être fusionné avec le  *\_Layout.cshtml* fichier.</span><span class="sxs-lookup"><span data-stu-id="3fd78-168">This one line of code tells ASP.NET that when the *Movies* page runs, it should be merged with the *\_Layout.cshtml* file.</span></span>

<span data-ttu-id="3fd78-169">Étant donné que la *Movies.cshtml* fichier utilise désormais une page de disposition, vous pouvez supprimer le balisage à partir de la *Movies.cshtml* page qui a pris en charge par le  *\_Layout.cshtml*fichier.</span><span class="sxs-lookup"><span data-stu-id="3fd78-169">Since the *Movies.cshtml* file now uses a layout page, you can remove the markup from the *Movies.cshtml* page that's taken care of by the *\_Layout.cshtml* file.</span></span> <span data-ttu-id="3fd78-170">Retirez la `<!DOCTYPE>`, `<html>`, et `<body>` balises ouvrantes et fermantes.</span><span class="sxs-lookup"><span data-stu-id="3fd78-170">Take out the `<!DOCTYPE>`, `<html>`, and `<body>` opening and closing tags.</span></span> <span data-ttu-id="3fd78-171">Retirez l’ensemble de `<head>` élément et son contenu, ce qui inclut les règles de style pour la grille, dans la mesure où vous avez maintenant ces règles un *.css* fichier.</span><span class="sxs-lookup"><span data-stu-id="3fd78-171">Take out the entire `<head>` element and its contents, which includes the style rules for the grid, since you've now got those rules in a *.css* file.</span></span> <span data-ttu-id="3fd78-172">Alors que vous êtes, les modifier existants `<h1>` élément à un `<h2>` élément ; vous avez un `<h1>` élément dans la page de disposition est déjà.</span><span class="sxs-lookup"><span data-stu-id="3fd78-172">While you're at it, change the existing `<h1>` element to an `<h2>` element; you have an `<h1>` element in the layout page already.</span></span> <span data-ttu-id="3fd78-173">Modifier la `<h2>` texte à la « Liste de films ».</span><span class="sxs-lookup"><span data-stu-id="3fd78-173">Change the `<h2>` text to "List Movies".</span></span>

<span data-ttu-id="3fd78-174">Normalement, vous n’avez pas à apporter ces types de modifications dans une page de contenu.</span><span class="sxs-lookup"><span data-stu-id="3fd78-174">Normally you wouldn't have to make these sorts of changes in a content page.</span></span> <span data-ttu-id="3fd78-175">Lorsque vous commencez votre site avec une page de disposition, vous créez des pages de contenu sans tous ces éléments pour commencer.</span><span class="sxs-lookup"><span data-stu-id="3fd78-175">When you start your site out with a layout page, you create content pages without all these elements to begin with.</span></span> <span data-ttu-id="3fd78-176">Dans ce cas, cependant, vous êtes conversion autonome d’une page qui utilise une mise en page, par conséquent, il est un peu de nettoyage.</span><span class="sxs-lookup"><span data-stu-id="3fd78-176">In this case, though, you're converting a standalone page to one that uses a layout, so there's a bit of cleanup.</span></span>

<span data-ttu-id="3fd78-177">Lorsque vous avez terminé, le *Movies.cshtml* page doit ressembler à ce qui suit :</span><span class="sxs-lookup"><span data-stu-id="3fd78-177">When you're finished, the *Movies.cshtml* page will look like the following:</span></span>

[!code-cshtml[Main](layouts/samples/sample5.cshtml)]

### <a name="testing-the-layout"></a><span data-ttu-id="3fd78-178">Test de la disposition</span><span class="sxs-lookup"><span data-stu-id="3fd78-178">Testing the Layout</span></span>

<span data-ttu-id="3fd78-179">Maintenant, vous pouvez voir l’aspect de la mise en page.</span><span class="sxs-lookup"><span data-stu-id="3fd78-179">Now you can see what the layout looks like.</span></span> <span data-ttu-id="3fd78-180">Dans WebMatrix, cliquez sur le *Movies.cshtml* page et sélectionnez **lancer dans un navigateur**.</span><span class="sxs-lookup"><span data-stu-id="3fd78-180">In WebMatrix, right-click the *Movies.cshtml* page and select **Launch in browser**.</span></span> <span data-ttu-id="3fd78-181">Lorsque le navigateur affiche la page, il semble que cette page :</span><span class="sxs-lookup"><span data-stu-id="3fd78-181">When the browser displays the page, it looks like this page:</span></span>

![Page de films restitué à l’aide d’une mise en page](layouts/_static/image6.png)

<span data-ttu-id="3fd78-183">ASP.NET a fusionné le contenu de la page Movies.cshtml dans le  *\_Layout.cshtml* page de droite où les `RenderBody` est de la méthode.</span><span class="sxs-lookup"><span data-stu-id="3fd78-183">ASP.NET has merged the content of the Movies.cshtml page into the *\_Layout.cshtml* page right where the `RenderBody` method is.</span></span> <span data-ttu-id="3fd78-184">Et bien sûr le  *\_Layout.cshtml* page références un *.css* fichier qui définit l’apparence de la page.</span><span class="sxs-lookup"><span data-stu-id="3fd78-184">And of course the *\_Layout.cshtml* page references a *.css* file that defines the look of the page.</span></span>

## <a name="updating-the-addmovie-page-to-use-the-layout"></a><span data-ttu-id="3fd78-185">Mise à jour de la Page AddMovie pour utiliser la mise en page</span><span class="sxs-lookup"><span data-stu-id="3fd78-185">Updating the AddMovie Page to Use the Layout</span></span>

<span data-ttu-id="3fd78-186">L’avantage réel de dispositions est que vous pouvez les utiliser pour toutes les pages de votre site.</span><span class="sxs-lookup"><span data-stu-id="3fd78-186">The real benefit of layouts is that you can use them for all the pages in your site.</span></span> <span data-ttu-id="3fd78-187">Ouvrez le *AddMovie.cshtml* page.</span><span class="sxs-lookup"><span data-stu-id="3fd78-187">Open the *AddMovie.cshtml* page.</span></span>

<span data-ttu-id="3fd78-188">Vous pouvez n’oubliez pas que le *AddMovie.cshtml* page contenait à l’origine des règles CSS qui définissent l’apparence des messages d’erreur de validation.</span><span class="sxs-lookup"><span data-stu-id="3fd78-188">You might remember that the *AddMovie.cshtml* page originally had some CSS rules in it to define the look of validation error messages.</span></span> <span data-ttu-id="3fd78-189">Étant donné que vous avez un *.css* fichier de votre site maintenant, vous pouvez déplacer ces règles à la *.css* fichier.</span><span class="sxs-lookup"><span data-stu-id="3fd78-189">Since you have a *.css* file for your site now, you can move those rules to the *.css* file.</span></span> <span data-ttu-id="3fd78-190">Les en retirer le *AddMovie.cshtml* de fichiers et de les ajouter au bas de la *Movies.css* fichier.</span><span class="sxs-lookup"><span data-stu-id="3fd78-190">Remove them from the *AddMovie.cshtml* file and add them to the bottom of the *Movies.css* file.</span></span> <span data-ttu-id="3fd78-191">Vous déplacez les règles suivantes :</span><span class="sxs-lookup"><span data-stu-id="3fd78-191">You are moving the following rules:</span></span>

[!code-css[Main](layouts/samples/sample6.css)]

<span data-ttu-id="3fd78-192">Maintenant apporter les mêmes types de modifications dans *AddMovie.cshtml* que vous avez effectué *Movies.cshtml* — ajouter `Layout="~/_Layout.cshtml;` et supprimez le balisage HTML qui est désormais superflu.</span><span class="sxs-lookup"><span data-stu-id="3fd78-192">Now make the same sorts of changes in *AddMovie.cshtml* that you did for *Movies.cshtml* — add `Layout="~/_Layout.cshtml;` and remove the HTML markup that's now extraneous.</span></span> <span data-ttu-id="3fd78-193">Modifier la `<h1>` élément `<h2>`.</span><span class="sxs-lookup"><span data-stu-id="3fd78-193">Change the `<h1>` element to `<h2>`.</span></span> <span data-ttu-id="3fd78-194">Lorsque vous avez terminé, la page doit ressembler à cet exemple :</span><span class="sxs-lookup"><span data-stu-id="3fd78-194">When you're done, the page will look like this example:</span></span>

[!code-cshtml[Main](layouts/samples/sample7.cshtml)]

<span data-ttu-id="3fd78-195">Exécutez la page.</span><span class="sxs-lookup"><span data-stu-id="3fd78-195">Run the page.</span></span> <span data-ttu-id="3fd78-196">Il ressemble maintenant à cette illustration :</span><span class="sxs-lookup"><span data-stu-id="3fd78-196">Now it looks like this illustration:</span></span>

![Page Ajouter des films restituée à l’aide d’une mise en page](layouts/_static/image7.png)

<span data-ttu-id="3fd78-198">Vous souhaitez apporter des modifications similaires à des pages dans le site : *EditMovie.cshtml* et *DeleteMovie.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="3fd78-198">You want to make similar changes to the pages in the site — *EditMovie.cshtml* and *DeleteMovie.cshtml*.</span></span> <span data-ttu-id="3fd78-199">Toutefois, avant de procéder, vous pouvez apporter une autre modification à la disposition qui rend un peu plus flexible.</span><span class="sxs-lookup"><span data-stu-id="3fd78-199">However, before you do, you can make another change to the layout that makes it a little more flexible.</span></span>

## <a name="passing-title-information-to-the-layout-page"></a><span data-ttu-id="3fd78-200">Passage des informations de titre pour la Page de disposition</span><span class="sxs-lookup"><span data-stu-id="3fd78-200">Passing Title Information to the Layout Page</span></span>

<span data-ttu-id="3fd78-201">Le  *\_Layout.cshtml* a de la page que vous avez créé un `<title>` élément qui est définie sur « Mon Site film ».</span><span class="sxs-lookup"><span data-stu-id="3fd78-201">The *\_Layout.cshtml* page that you created has a `<title>` element that's set to "My Movie Site".</span></span> <span data-ttu-id="3fd78-202">La plupart des navigateurs affichent le contenu de cet élément en tant que le texte d’un onglet :</span><span class="sxs-lookup"><span data-stu-id="3fd78-202">Most browsers display the content of this element as the text on a tab:</span></span>

![La page &lt;titre&gt; élément affiché dans un onglet de navigateur](layouts/_static/image8.png)

<span data-ttu-id="3fd78-204">Ces informations de titre sont génériques.</span><span class="sxs-lookup"><span data-stu-id="3fd78-204">This title information is generic.</span></span> <span data-ttu-id="3fd78-205">Vous voulez que le texte du titre plus spécifique à la page active.</span><span class="sxs-lookup"><span data-stu-id="3fd78-205">Suppose that you want the title text to be more specific to the current page.</span></span> <span data-ttu-id="3fd78-206">(Le texte du titre est également utilisé par les moteurs de recherche pour déterminer ce qui concerne votre page.) Vous pouvez passer des informations à partir d’une page de contenu comme *Movies.cshtml* ou *AddMovie.cshtml* effectue le rendu à la page de disposition, puis utiliser ces informations pour personnaliser le contenu de la page mise en page.</span><span class="sxs-lookup"><span data-stu-id="3fd78-206">(The title text is also used by search engines to determine what your page is about.) You can pass information from a content page like *Movies.cshtml* or *AddMovie.cshtml* to the layout page, and then use that information to customize what the layout page renders.</span></span>

<span data-ttu-id="3fd78-207">Ouvrez le *Movies.cshtml* page à nouveau.</span><span class="sxs-lookup"><span data-stu-id="3fd78-207">Open the *Movies.cshtml* page again.</span></span> <span data-ttu-id="3fd78-208">Dans le code en haut, ajoutez la ligne suivante :</span><span class="sxs-lookup"><span data-stu-id="3fd78-208">In the code at the top, add the following line:</span></span>

[!code-csharp[Main](layouts/samples/sample8.cs)]

<span data-ttu-id="3fd78-209">Le `Page` objet n’est disponible sur tous les *.cshtml* pages et est à cet effet, à savoir pour partager des informations entre une page et de sa disposition.</span><span class="sxs-lookup"><span data-stu-id="3fd78-209">The `Page` object is available on all *.cshtml* pages and is for this purpose, namely to share information between a page and its layout.</span></span>

<span data-ttu-id="3fd78-210">Ouvrez le<em>\_Layout.cshtml</em> page.</span><span class="sxs-lookup"><span data-stu-id="3fd78-210">Open the<em>\_Layout.cshtml</em> page.</span></span> <span data-ttu-id="3fd78-211">Modifier la `<title>` élément afin qu’il ressemble à ce balisage :</span><span class="sxs-lookup"><span data-stu-id="3fd78-211">Change the `<title>` element so that it looks like this markup:</span></span>

[!code-html[Main](layouts/samples/sample9.html)]

<span data-ttu-id="3fd78-212">Ce code affiche tout ce qui est dans le `Page.Title` propriété à cet emplacement dans la page.</span><span class="sxs-lookup"><span data-stu-id="3fd78-212">This code renders whatever is in the `Page.Title` property right at that location in the page.</span></span>

<span data-ttu-id="3fd78-213">Exécutez le *Movies.cshtml* page.</span><span class="sxs-lookup"><span data-stu-id="3fd78-213">Run the *Movies.cshtml* page.</span></span> <span data-ttu-id="3fd78-214">Cette fois l’onglet navigateur montre ce que vous avez passé en tant que la valeur de `Page.Title`:</span><span class="sxs-lookup"><span data-stu-id="3fd78-214">This time the browser tab shows what you passed as the value of `Page.Title`:</span></span>

![Un onglet de navigateur affichant le titre créé dynamiquement](layouts/_static/image9.png)

<span data-ttu-id="3fd78-216">Si vous le souhaitez, affichez la source de la page dans le navigateur.</span><span class="sxs-lookup"><span data-stu-id="3fd78-216">If you want, view the page source in the browser.</span></span> <span data-ttu-id="3fd78-217">Vous pouvez voir que la `<title>` élément est restitué sous la forme `<title>List Movies</title>`.</span><span class="sxs-lookup"><span data-stu-id="3fd78-217">You can see that the `<title>` element is rendered as `<title>List Movies</title>`.</span></span>

> [!TIP] 
> 
> <span data-ttu-id="3fd78-218">**L’objet de la Page**</span><span class="sxs-lookup"><span data-stu-id="3fd78-218">**The Page Object**</span></span>
> 
> <span data-ttu-id="3fd78-219">Une fonctionnalité de `Page` il s’agit d’un objet dynamique, le `Title` propriété n’est pas un nom fixe ou réservé.</span><span class="sxs-lookup"><span data-stu-id="3fd78-219">A useful feature of `Page` is that it's a dynamic object — the `Title` property is not a fixed or reserved name.</span></span> <span data-ttu-id="3fd78-220">Vous pouvez utiliser *tout* nom d’une valeur de la `Page` objet.</span><span class="sxs-lookup"><span data-stu-id="3fd78-220">You can use *any* name for a value of the `Page` object.</span></span> <span data-ttu-id="3fd78-221">Par exemple, vous avez peut aussi facilement ont passé le titre à l’aide d’une propriété nommée `Page.CurrentName` ou `Page.MyPage`.</span><span class="sxs-lookup"><span data-stu-id="3fd78-221">For example, you could as easily have passed the title by using a property named `Page.CurrentName` or `Page.MyPage`.</span></span> <span data-ttu-id="3fd78-222">La seule restriction est que le nom doit respecter les règles normales pour les propriétés qui peuvent être nommées.</span><span class="sxs-lookup"><span data-stu-id="3fd78-222">The only restriction is that the name has to follow the normal rules for what properties can be named.</span></span> <span data-ttu-id="3fd78-223">(Par exemple, le nom ne peut pas contenir un espace.)</span><span class="sxs-lookup"><span data-stu-id="3fd78-223">(For example, the name can't contain a space.)</span></span>
> 
> <span data-ttu-id="3fd78-224">Vous pouvez passer n’importe quel nombre de valeurs à l’aide de la `Page` objet.</span><span class="sxs-lookup"><span data-stu-id="3fd78-224">You can pass any number of values by using the `Page` object.</span></span> <span data-ttu-id="3fd78-225">Si vous souhaitez passer des informations de vidéo à la page de disposition, vous pouvez passer des valeurs à l’aide de quelque chose comme `Page.MovieTitle` et `Page.Genre` et `Page.MovieYear`.</span><span class="sxs-lookup"><span data-stu-id="3fd78-225">If you wanted to pass movie information to the layout page, you could pass values by using something like `Page.MovieTitle` and `Page.Genre` and `Page.MovieYear`.</span></span> <span data-ttu-id="3fd78-226">(Ou tous les autres noms qui vous a inventé pour stocker les informations.) La seule exigence, qui est probablement évident, est que vous devez utiliser les mêmes noms dans la page de contenu et de la page de disposition.</span><span class="sxs-lookup"><span data-stu-id="3fd78-226">(Or any other names that you invented to store the information.) The only requirement — which is probably obvious — is that you have to use the same names in the content page and the layout page.</span></span>
> 
> <span data-ttu-id="3fd78-227">Les informations que vous passez à l’aide de la `Page` objet n’est pas limité à tout le texte à afficher sur la page de disposition.</span><span class="sxs-lookup"><span data-stu-id="3fd78-227">The information you pass by using the `Page` object isn't limited to just text to display on the layout page.</span></span> <span data-ttu-id="3fd78-228">Vous pouvez passer une valeur à la page de disposition et de code dans la page de disposition permet ensuite la valeur de décider s’il faut afficher une section de la page, ce qui *.css* pour et ainsi de suite.</span><span class="sxs-lookup"><span data-stu-id="3fd78-228">You can pass a value to the layout page, and then code in the layout page can use the value to decide whether to display a section of the page, what *.css* file to use, and so on.</span></span> <span data-ttu-id="3fd78-229">Les valeurs que vous passez le `Page` objet sont comme d’autres valeurs que vous utilisez dans le code.</span><span class="sxs-lookup"><span data-stu-id="3fd78-229">The values you pass in the `Page` object are like any other values that you use in code.</span></span> <span data-ttu-id="3fd78-230">Il est simplement que les valeurs proviennent de la page de contenu et sont passés à la page de disposition.</span><span class="sxs-lookup"><span data-stu-id="3fd78-230">It's just that the values originate in the content page and are passed to the layout page.</span></span>


<span data-ttu-id="3fd78-231">Ouvrez le *AddMovie.cshtml* page et ajoutez une ligne vers le haut du code qui fournit un titre pour le *AddMovie.cshtml* page :</span><span class="sxs-lookup"><span data-stu-id="3fd78-231">Open the *AddMovie.cshtml* page and add a line to the top of the code that provides a title for the *AddMovie.cshtml* page:</span></span>

[!code-csharp[Main](layouts/samples/sample10.cs)]

<span data-ttu-id="3fd78-232">Exécutez le *AddMovie.cshtml* page.</span><span class="sxs-lookup"><span data-stu-id="3fd78-232">Run the *AddMovie.cshtml* page.</span></span> <span data-ttu-id="3fd78-233">Vous consultez le nouveau titre il :</span><span class="sxs-lookup"><span data-stu-id="3fd78-233">You see the new title there:</span></span>

![Un onglet de navigateur affichant le titre « Ajouter des films » créé dynamiquement](layouts/_static/image10.png)

## <a name="updating-the-remaining-pages-to-use-the-layout"></a><span data-ttu-id="3fd78-235">Mise à jour les Pages restantes pour utiliser la mise en page</span><span class="sxs-lookup"><span data-stu-id="3fd78-235">Updating the Remaining Pages to Use the Layout</span></span>

<span data-ttu-id="3fd78-236">Maintenant, vous pouvez terminer les pages restantes de votre site afin qu’ils utilisent la nouvelle disposition.</span><span class="sxs-lookup"><span data-stu-id="3fd78-236">Now you can finish the remaining pages in your site so that they use the new layout.</span></span> <span data-ttu-id="3fd78-237">Ouvrez *EditMovie.cshtml* et *DeleteMovie.cshtml* à son tour et apporter les mêmes modifications dans chaque.</span><span class="sxs-lookup"><span data-stu-id="3fd78-237">Open *EditMovie.cshtml* and *DeleteMovie.cshtml* in turn and make the same changes in each.</span></span>

<span data-ttu-id="3fd78-238">Ajoutez la ligne de code qui établit un lien vers la page de disposition :</span><span class="sxs-lookup"><span data-stu-id="3fd78-238">Add the line of code that links to the layout page:</span></span>

[!code-csharp[Main](layouts/samples/sample11.cs)]

<span data-ttu-id="3fd78-239">Ajoutez une ligne pour définir le titre de la page :</span><span class="sxs-lookup"><span data-stu-id="3fd78-239">Add a line to set the title of the page:</span></span>

[!code-csharp[Main](layouts/samples/sample12.cs)]

<span data-ttu-id="3fd78-240">ou :</span><span class="sxs-lookup"><span data-stu-id="3fd78-240">or:</span></span>

[!code-csharp[Main](layouts/samples/sample13.cs)]

<span data-ttu-id="3fd78-241">Supprimer tout le balisage HTML superflu : en fait, conservez uniquement les bits qui sont à l’intérieur du `<body>` élément (plus le bloc de code en haut).</span><span class="sxs-lookup"><span data-stu-id="3fd78-241">Remove all the extraneous HTML markup — basically, leave only the bits that are inside the `<body>` element (plus the code block at the top).</span></span>

<span data-ttu-id="3fd78-242">Modifier la `<h1>` élément soit un `<h2>` élément.</span><span class="sxs-lookup"><span data-stu-id="3fd78-242">Change the `<h1>` element to be an `<h2>` element.</span></span>

<span data-ttu-id="3fd78-243">Lorsque vous avez apporté ces modifications, chaque test et assurez-vous qu’elle s’affiche correctement et que le titre est correct.</span><span class="sxs-lookup"><span data-stu-id="3fd78-243">When you've made these changes, test each and make sure that it's displaying properly and that the title is correct.</span></span>

## <a name="parting-thoughts-about-layout-pages"></a><span data-ttu-id="3fd78-244">Joint des réflexions sur les Pages de disposition</span><span class="sxs-lookup"><span data-stu-id="3fd78-244">Parting Thoughts About Layout Pages</span></span>

<span data-ttu-id="3fd78-245">Dans ce didacticiel, vous avez créé un  *\_Layout.cshtml* page et utilisé la `RenderBody` méthode pour fusionner le contenu à partir d’une autre page.</span><span class="sxs-lookup"><span data-stu-id="3fd78-245">In this tutorial you created a *\_Layout.cshtml* page and used the `RenderBody` method to merge content from another page.</span></span> <span data-ttu-id="3fd78-246">C’est le modèle de base pour l’utilisation des dispositions dans les Pages Web.</span><span class="sxs-lookup"><span data-stu-id="3fd78-246">That's the basic pattern for using layouts in Web Pages.</span></span>

<span data-ttu-id="3fd78-247">Pages de disposition ont des fonctionnalités supplémentaires qui nous ne couvrons ici.</span><span class="sxs-lookup"><span data-stu-id="3fd78-247">Layout pages have additional features that we didn't cover here.</span></span> <span data-ttu-id="3fd78-248">Par exemple, vous pouvez imbriquer des pages de disposition, une page de disposition permettre référencer à son tour un autre.</span><span class="sxs-lookup"><span data-stu-id="3fd78-248">For example, you can nest layout pages — one layout page can in turn reference another.</span></span> <span data-ttu-id="3fd78-249">Dispositions imbriquées peuvent être utiles si vous travaillez avec les sous-sections d’un site qui nécessitent des dispositions différentes.</span><span class="sxs-lookup"><span data-stu-id="3fd78-249">Nested layouts can be useful if you're working with subsections of a site that require different layouts.</span></span> <span data-ttu-id="3fd78-250">Vous pouvez également utiliser des méthodes supplémentaires (par exemple, `RenderSection`) pour configurer nommé sections de la page de disposition.</span><span class="sxs-lookup"><span data-stu-id="3fd78-250">You can also use additional methods (for example, `RenderSection`) to set up named sections in the layout page.</span></span>

<span data-ttu-id="3fd78-251">La combinaison de pages de disposition et *.css* fichiers est puissant.</span><span class="sxs-lookup"><span data-stu-id="3fd78-251">The combination of layout pages and *.css* files is powerful.</span></span> <span data-ttu-id="3fd78-252">Comme vous le verrez dans la prochaine série de didacticiels, dans WebMatrix, vous pouvez créer un site basé sur un *modèle*, qui vous donne un site qui possède des fonctionnalités prégénérées qu’elle contient.</span><span class="sxs-lookup"><span data-stu-id="3fd78-252">As you'll see in the next tutorial series, in WebMatrix you can create a site based on a *template*, which gives you a site that has prebuilt functionality in it.</span></span> <span data-ttu-id="3fd78-253">Les modèles d’exploiter au mieux de CSS pour créer des sites qui superbes et qui ont des fonctionnalités telles que les menus et les pages de disposition.</span><span class="sxs-lookup"><span data-stu-id="3fd78-253">The templates make good use of layout pages and CSS to create sites that look great and that have features like menus.</span></span> <span data-ttu-id="3fd78-254">Voici une capture d’écran de la page d’accueil d’un site basé sur un modèle, en affichant les fonctionnalités qui utilisent des pages de disposition et CSS :</span><span class="sxs-lookup"><span data-stu-id="3fd78-254">Here's a screenshot of the home page from a site based on a template, showing features that use layout pages and CSS:</span></span>

![Mise en page créée par le modèle de site WebMatrix montrant les en-tête, zone de navigation, zone de contenu, section facultative et les liens de connexion](layouts/_static/image11.png)

## <a name="complete-listing-for-movie-page-updated-to-use-a-layout-page"></a><span data-ttu-id="3fd78-256">Liste complète pour la Page de film (mis à jour pour utiliser une Page de disposition)</span><span class="sxs-lookup"><span data-stu-id="3fd78-256">Complete Listing for Movie Page (Updated to Use a Layout Page)</span></span>

[!code-cshtml[Main](layouts/samples/sample14.cshtml)]

## <a name="complete-page-listing-for-add-movie-page-updated-for-layout"></a><span data-ttu-id="3fd78-257">Page fin de liste pour ajouter une Page de film (mis à jour pour la mise en page)</span><span class="sxs-lookup"><span data-stu-id="3fd78-257">Complete Page Listing for Add Movie Page (Updated for Layout)</span></span>

[!code-cshtml[Main](layouts/samples/sample15.cshtml)]

## <a name="complete-page-listing-for-delete-movie-page-updated-for-layout"></a><span data-ttu-id="3fd78-258">Terminer l’annonce de Page pour supprimer la Page film (mis à jour pour la mise en page)</span><span class="sxs-lookup"><span data-stu-id="3fd78-258">Complete Page Listing for Delete Movie Page (Updated for Layout)</span></span>

[!code-cshtml[Main](layouts/samples/sample16.cshtml)]

## <a name="complete-page-listing-for-edit-movie-page-updated-for-layout"></a><span data-ttu-id="3fd78-259">Terminer l’annonce de Page pour la Page de film de modification (mise à jour pour la mise en page)</span><span class="sxs-lookup"><span data-stu-id="3fd78-259">Complete Page Listing for Edit Movie Page (Updated for Layout)</span></span>

[!code-cshtml[Main](layouts/samples/sample17.cshtml)]

## <a name="coming-up-next"></a><span data-ttu-id="3fd78-260">Prochaine</span><span class="sxs-lookup"><span data-stu-id="3fd78-260">Coming Up Next</span></span>

<span data-ttu-id="3fd78-261">Dans l’étape suivante du didacticiel, vous allez apprendre à publier votre site sur Internet pour que tous les utilisateurs puissent le voir.</span><span class="sxs-lookup"><span data-stu-id="3fd78-261">In the next tutorial, you'll learn how to publish your site to the Internet so everyone can see it.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="3fd78-262">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="3fd78-262">Additional Resources</span></span>

- <span data-ttu-id="3fd78-263">[Création d’une zone de recherche cohérent](https://go.microsoft.com/fwlink/?LinkID=202891) , un article qui fournit certaines plus de détails sur l’utilisation des dispositions.</span><span class="sxs-lookup"><span data-stu-id="3fd78-263">[Creating a Consistent Look](https://go.microsoft.com/fwlink/?LinkID=202891) — An article that provides some more detail on working with layouts.</span></span> <span data-ttu-id="3fd78-264">Il explique également comment passer une valeur à une page de disposition qui affiche ou masque le contenu.</span><span class="sxs-lookup"><span data-stu-id="3fd78-264">It also describes how to pass a value to a layout page that shows or hides some of the content.</span></span>
- <span data-ttu-id="3fd78-265">[Pages de disposition avec Razor imbriquées](http://www.mikesdotnetting.com/Article/164/Nested-Layout-Pages-with-Razor) , Mike Brind blogs un exemple montrant comment imbriquer des pages de disposition.</span><span class="sxs-lookup"><span data-stu-id="3fd78-265">[Nested Layout Pages with Razor](http://www.mikesdotnetting.com/Article/164/Nested-Layout-Pages-with-Razor) — Mike Brind blogs an example of how to nest layout pages.</span></span> <span data-ttu-id="3fd78-266">(Y compris un téléchargement des pages.)</span><span class="sxs-lookup"><span data-stu-id="3fd78-266">(Includes a download of the pages.)</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="3fd78-267">[Précédent](deleting-data.md)
> [Suivant](publishing.md)</span><span class="sxs-lookup"><span data-stu-id="3fd78-267">[Previous](deleting-data.md)
[Next](publishing.md)</span></span>
