---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/deleting-data
title: Présentation des Pages Web ASP.NET - suppression de la base de données | Microsoft Docs
author: Rick-Anderson
description: Ce didacticiel vous montre comment supprimer une entrée de la base de données individuelle. Il part du principe que vous avez terminé la série grâce à la mise à jour des données de base de données dans ASP.NET Web PA...
ms.author: riande
ms.date: 01/02/2018
ms.assetid: 75b5c1cf-84bd-434f-8a86-85c568eb5b09
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/deleting-data
msc.type: authoredcontent
ms.openlocfilehash: b2ef8fcc8cc534bd31fea83bf0b085b85995f417
ms.sourcegitcommit: 2d3e5422d530203efdaf2014d1d7df31f88d08d0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/05/2018
ms.locfileid: "51020440"
---
<a name="introducing-aspnet-web-pages---deleting-database-data"></a><span data-ttu-id="d22b0-104">Présentation des Pages Web ASP.NET - suppression de la base de données</span><span class="sxs-lookup"><span data-stu-id="d22b0-104">Introducing ASP.NET Web Pages - Deleting Database Data</span></span>
====================
<span data-ttu-id="d22b0-105">par [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="d22b0-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="d22b0-106">Ce didacticiel vous montre comment supprimer une entrée de la base de données individuelle.</span><span class="sxs-lookup"><span data-stu-id="d22b0-106">This tutorial shows you how to delete an individual database entry.</span></span> <span data-ttu-id="d22b0-107">Il part du principe que vous avez terminé la série via [la mise à jour la base de données dans ASP.NET Web Pages](updating-data.md).</span><span class="sxs-lookup"><span data-stu-id="d22b0-107">It assumes you have completed the series through [Updating Database Data in ASP.NET Web Pages](updating-data.md).</span></span>
> 
> <span data-ttu-id="d22b0-108">Ce que vous allez apprendre :</span><span class="sxs-lookup"><span data-stu-id="d22b0-108">What you'll learn:</span></span>
> 
> - <span data-ttu-id="d22b0-109">La sélection d’un enregistrement individuel à partir d’une liste d’enregistrements.</span><span class="sxs-lookup"><span data-stu-id="d22b0-109">How to select an individual record from a listing of records.</span></span>
> - <span data-ttu-id="d22b0-110">Comment supprimer un enregistrement unique d’une base de données.</span><span class="sxs-lookup"><span data-stu-id="d22b0-110">How to delete a single record from a database.</span></span>
> - <span data-ttu-id="d22b0-111">Comment vérifier que l’utilisateur a cliqué sur un bouton spécifique dans un formulaire.</span><span class="sxs-lookup"><span data-stu-id="d22b0-111">How to check that a specific button was clicked in a form.</span></span>
>   
> 
> <span data-ttu-id="d22b0-112">Fonctionnalités technologies abordées :</span><span class="sxs-lookup"><span data-stu-id="d22b0-112">Features/technologies discussed:</span></span>
> 
> - <span data-ttu-id="d22b0-113">Le `WebGrid` helper.</span><span class="sxs-lookup"><span data-stu-id="d22b0-113">The `WebGrid` helper.</span></span>
> - <span data-ttu-id="d22b0-114">Le code SQL `Delete` commande.</span><span class="sxs-lookup"><span data-stu-id="d22b0-114">The SQL `Delete` command.</span></span>
> - <span data-ttu-id="d22b0-115">Le `Database.Execute` méthode à exécuter une instance SQL `Delete` commande.</span><span class="sxs-lookup"><span data-stu-id="d22b0-115">The `Database.Execute` method to run a SQL `Delete` command.</span></span>


## <a name="what-youll-build"></a><span data-ttu-id="d22b0-116">Ce que vous allez générer</span><span class="sxs-lookup"><span data-stu-id="d22b0-116">What You'll Build</span></span>

<span data-ttu-id="d22b0-117">Dans le didacticiel précédent, vous avez appris à mettre à jour un enregistrement de base de données existant.</span><span class="sxs-lookup"><span data-stu-id="d22b0-117">In the previous tutorial, you learned how to update an existing database record.</span></span> <span data-ttu-id="d22b0-118">Ce didacticiel est similaire, mais au lieu de la mise à jour l’enregistrement, vous allez le supprimer.</span><span class="sxs-lookup"><span data-stu-id="d22b0-118">This tutorial is similar, except that instead of updating the record, you'll delete it.</span></span> <span data-ttu-id="d22b0-119">Les processus sont similaires, sauf que la suppression est plus simple, ce didacticiel est donc court.</span><span class="sxs-lookup"><span data-stu-id="d22b0-119">The processes are much the same, except that deleting is simpler, so this tutorial will be short.</span></span>

<span data-ttu-id="d22b0-120">Dans le *films* page, vous allez mettre à jour le `WebGrid` helper afin qu’elle affiche un **supprimer** lien en regard de chaque film pour accompagner le **modifier** lien que vous avez ajouté précédemment.</span><span class="sxs-lookup"><span data-stu-id="d22b0-120">In the *Movies* page, you'll update the `WebGrid` helper so that it displays a **Delete** link next to each movie to accompany the **Edit** link you added earlier.</span></span>

![Page de films montrant un lien Supprimer pour chaque film](deleting-data/_static/image1.png)

<span data-ttu-id="d22b0-122">Comme avec la modification, lorsque vous cliquez sur le **supprimer** lien, il permet d’accéder à une autre page, où les informations de film sont déjà dans un formulaire :</span><span class="sxs-lookup"><span data-stu-id="d22b0-122">As with editing, when you click the **Delete** link, it takes you to a different page, where the movie information is already in a form:</span></span>

![Supprimer la page de film avec un film affiché](deleting-data/_static/image2.png)

<span data-ttu-id="d22b0-124">Vous pouvez ensuite cliquer sur le bouton pour supprimer définitivement un enregistrement.</span><span class="sxs-lookup"><span data-stu-id="d22b0-124">You can then click the button to delete the record permanently.</span></span>

## <a name="adding-a-delete-link-to-the-movie-listing"></a><span data-ttu-id="d22b0-125">Ajout d’un lien de suppression à la liste de films</span><span class="sxs-lookup"><span data-stu-id="d22b0-125">Adding a Delete Link to the Movie Listing</span></span>

<span data-ttu-id="d22b0-126">Vous commencerez en ajoutant un **supprimer** lier à la `WebGrid` helper.</span><span class="sxs-lookup"><span data-stu-id="d22b0-126">You'll start by adding a **Delete** link to the `WebGrid` helper.</span></span> <span data-ttu-id="d22b0-127">Ce lien est similaire à la **modifier** lien que vous avez ajouté dans le didacticiel précédent.</span><span class="sxs-lookup"><span data-stu-id="d22b0-127">This link is similar to the **Edit** link you added in a previous tutorial.</span></span>

<span data-ttu-id="d22b0-128">Ouvrez le *Movies.cshtml* fichier.</span><span class="sxs-lookup"><span data-stu-id="d22b0-128">Open the *Movies.cshtml* file.</span></span>

<span data-ttu-id="d22b0-129">Modifier le `WebGrid` balisage dans le corps de la page en ajoutant une colonne.</span><span class="sxs-lookup"><span data-stu-id="d22b0-129">Change the `WebGrid` markup in the body of the page by adding a column.</span></span> <span data-ttu-id="d22b0-130">Voici le balisage modifié :</span><span class="sxs-lookup"><span data-stu-id="d22b0-130">Here's the modified markup:</span></span>

[!code-html[Main](deleting-data/samples/sample1.html?highlight=9-10)]

<span data-ttu-id="d22b0-131">La nouvelle colonne est celle-ci :</span><span class="sxs-lookup"><span data-stu-id="d22b0-131">The new column is this one:</span></span>

[!code-html[Main](deleting-data/samples/sample2.html)]

<span data-ttu-id="d22b0-132">La manière de configurer la grille, le **modifier** colonne est plus à gauche dans la grille et le **supprimer** colonne est plus à droite.</span><span class="sxs-lookup"><span data-stu-id="d22b0-132">The way the grid is configured, the **Edit** column is leftmost in the grid and the **Delete** column is rightmost.</span></span> <span data-ttu-id="d22b0-133">(Il existe une virgule après le `Year` colonne maintenant, au cas où vous n’avez pas remarquer que.) Il n’a rien de spécial sur où ces colonnes lien go, et vous pouvez aussi facilement les placer en regard de chacun des autres.</span><span class="sxs-lookup"><span data-stu-id="d22b0-133">(There's a comma after the `Year` column now, in case you didn't notice that.) There's nothing special about where these link columns go, and you could as easily put them next to each other.</span></span> <span data-ttu-id="d22b0-134">Dans ce cas, ils sont distincts pour les rendre plus difficile à se mélanger.</span><span class="sxs-lookup"><span data-stu-id="d22b0-134">In this case, they're separate to make them harder to get mixed up.</span></span>

![Page de films avec des liens de modification et les détails marquée pour montrer qu’ils ne sont pas en regard de chacun des autres](deleting-data/_static/image3.png)

<span data-ttu-id="d22b0-136">La nouvelle colonne affiche un lien (`<a>` élément) dont le texte indique « Supprimer ».</span><span class="sxs-lookup"><span data-stu-id="d22b0-136">The new column shows a link (`<a>` element) whose text says "Delete".</span></span> <span data-ttu-id="d22b0-137">La cible du lien (son `href` attribut) est un code qui au final correspond à quelque chose comme cette URL, avec la `id` valeur différente pour chaque film :</span><span class="sxs-lookup"><span data-stu-id="d22b0-137">The target of the link (its `href` attribute) is code that ultimately resolves to something like this URL, with the `id` value different for each movie:</span></span>

[!code-css[Main](deleting-data/samples/sample3.css)]

<span data-ttu-id="d22b0-138">Ce lien appellera une page nommée *DeleteMovie* et passez-lui l’ID du film que vous avez sélectionné.</span><span class="sxs-lookup"><span data-stu-id="d22b0-138">This link will invoke a page named *DeleteMovie* and pass it the ID of the movie you've selected.</span></span>

<span data-ttu-id="d22b0-139">Ce didacticiel n’entrerai pas dans les détails de la construction de ce lien, car il est presque identique à la **modifier** lien du didacticiel précédent ([la mise à jour la base de données dans ASP.NET Web Pages](updating-data.md)).</span><span class="sxs-lookup"><span data-stu-id="d22b0-139">This tutorial won't go into detail about how this link is constructed, because it's almost identical to the **Edit** link from the previous tutorial ([Updating Database Data in ASP.NET Web Pages](updating-data.md)).</span></span>

## <a name="creating-the-delete-page"></a><span data-ttu-id="d22b0-140">Création de la Page Delete</span><span class="sxs-lookup"><span data-stu-id="d22b0-140">Creating the Delete Page</span></span>

<span data-ttu-id="d22b0-141">Vous pouvez désormais créer la page qui sera la cible pour le **supprimer** lien dans la grille.</span><span class="sxs-lookup"><span data-stu-id="d22b0-141">Now you can create the page that will be the target for the **Delete** link in the grid.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="d22b0-142">**Important** la technique de tout d’abord sélectionner un enregistrement à supprimer, puis en utilisant une page distincte et un bouton pour confirmer le processus est extrêmement importante pour la sécurité.</span><span class="sxs-lookup"><span data-stu-id="d22b0-142">**Important** The technique of first selecting a record to delete and then using a separate page and button to confirm the process is extremely important for security.</span></span> <span data-ttu-id="d22b0-143">Comme vous avez lu dans les didacticiels précédents, ce qui *n’importe quel* tri de modification à votre site Web doit *toujours* être effectuée à l’aide d’un formulaire &mdash; , autrement dit, en utilisant une opération HTTP POST.</span><span class="sxs-lookup"><span data-stu-id="d22b0-143">As you've read in previous tutorials, making *any* sort of change to your website should *always* be done using a form &mdash; that is, using an HTTP POST operation.</span></span> <span data-ttu-id="d22b0-144">Si vous l’avez fait possible de modifier le site suffit de cliquer sur un lien (c'est-à-dire, en utilisant une opération GET), les personnes pourrait faire des demandes simples à votre site et supprimer vos données.</span><span class="sxs-lookup"><span data-stu-id="d22b0-144">If you made it possible to change the site just by clicking a link (that is, using a GET operation), people could make simple requests to your site and delete your data.</span></span> <span data-ttu-id="d22b0-145">Même un moteur de recherche qui est l’indexation de votre site peut supprimer par inadvertance des données simplement en suivant les liens.</span><span class="sxs-lookup"><span data-stu-id="d22b0-145">Even a search-engine crawler that's indexing your site could inadvertently delete data just by following links.</span></span>
> 
> <span data-ttu-id="d22b0-146">Lorsque votre application permet de modifier un enregistrement, vous devez présenter l’enregistrement à l’utilisateur pour la modification de toute façon.</span><span class="sxs-lookup"><span data-stu-id="d22b0-146">When your app lets people change a record, you have to present the record to the user for editing anyway.</span></span> <span data-ttu-id="d22b0-147">Mais vous pouvez être tenté d’ignorer cette étape pour la suppression d’un enregistrement.</span><span class="sxs-lookup"><span data-stu-id="d22b0-147">But you might be tempted to skip this step for deleting a record.</span></span> <span data-ttu-id="d22b0-148">N’ignorez pas cette étape, cependant.</span><span class="sxs-lookup"><span data-stu-id="d22b0-148">Don't skip that step, though.</span></span> <span data-ttu-id="d22b0-149">(Il est également utile pour les utilisateurs voient l’enregistrement et de confirmer qu’ils vous supprimez l’enregistrement ils destinés.)</span><span class="sxs-lookup"><span data-stu-id="d22b0-149">(It's also helpful for users to see the record and confirm that they're deleting the record that they intended.)</span></span>
> 
> <span data-ttu-id="d22b0-150">Dans un ensemble de didacticiels suivant, vous verrez comment ajouter la fonctionnalité de connexion pour un utilisateur devra se connecter avant la suppression d’un enregistrement.</span><span class="sxs-lookup"><span data-stu-id="d22b0-150">In a subsequent tutorial set, you'll see how to add login functionality so a user would have to log in before deleting a record.</span></span>


<span data-ttu-id="d22b0-151">Créez une page nommée *DeleteMovie.cshtml* et remplacer celui qui figure dans le fichier avec le balisage suivant :</span><span class="sxs-lookup"><span data-stu-id="d22b0-151">Create a page named *DeleteMovie.cshtml* and replace what's in the file with the following markup:</span></span>

[!code-cshtml[Main](deleting-data/samples/sample4.cshtml)]

<span data-ttu-id="d22b0-152">Ce balisage est similaire à la *EditMovie* pages, à ceci près qu’au lieu d’utiliser des zones de texte (`<input type="text">`), le balisage comprend `<span>` éléments.</span><span class="sxs-lookup"><span data-stu-id="d22b0-152">This markup is like the *EditMovie* pages, except that instead of using text boxes (`<input type="text">`), the markup includes `<span>` elements.</span></span> <span data-ttu-id="d22b0-153">Il n’existe rien ici à modifier.</span><span class="sxs-lookup"><span data-stu-id="d22b0-153">There's nothing here to edit.</span></span> <span data-ttu-id="d22b0-154">Il vous suffit est d’afficher les détails de films afin que les utilisateurs peuvent s’assurer qu’ils vous supprimez le film de droite.</span><span class="sxs-lookup"><span data-stu-id="d22b0-154">All you have to do is display the movie details so that users can make sure that they're deleting the right movie.</span></span>

<span data-ttu-id="d22b0-155">Le balisage contient déjà un lien qui permet à l’utilisateur de revenir à la page de liste de films.</span><span class="sxs-lookup"><span data-stu-id="d22b0-155">The markup already contains a link that lets the user return to the movie listing page.</span></span>

<span data-ttu-id="d22b0-156">Comme dans le *EditMovie* page, l’ID du film sélectionné est stocké dans un champ masqué.</span><span class="sxs-lookup"><span data-stu-id="d22b0-156">As in the *EditMovie* page, the ID of the selected movie is stored in a hidden field.</span></span> <span data-ttu-id="d22b0-157">(Il est passé dans la page en premier lieu en tant que valeur de chaîne de requête.) Il existe un `Html.ValidationSummary` appel qui affiche les erreurs de validation.</span><span class="sxs-lookup"><span data-stu-id="d22b0-157">(It's passed into the page in the first place as a query string value.) There's an `Html.ValidationSummary` call that will display validation errors.</span></span> <span data-ttu-id="d22b0-158">Dans ce cas, l’erreur peut être qu’aucun ID de film a été passé à la page ou que l’ID de film n’est pas valide.</span><span class="sxs-lookup"><span data-stu-id="d22b0-158">In this case, the error might be that no movie ID was passed to the page or that the movie ID is invalid.</span></span> <span data-ttu-id="d22b0-159">Cette situation peut se produire si une personne a exécuté cette page sans un film en sélectionnant d’abord le *films* page.</span><span class="sxs-lookup"><span data-stu-id="d22b0-159">This situation could occur if someone ran this page without first selecting a movie in the *Movies* page.</span></span>

<span data-ttu-id="d22b0-160">La légende du bouton est **supprimer un film**, et son attribut de nom est défini sur `buttonDelete`.</span><span class="sxs-lookup"><span data-stu-id="d22b0-160">The button caption is **Delete Movie**, and its name attribute is set to `buttonDelete`.</span></span> <span data-ttu-id="d22b0-161">Le `name` attribut servira dans le code pour identifier le bouton qui a envoyé le formulaire.</span><span class="sxs-lookup"><span data-stu-id="d22b0-161">The `name` attribute will be used in the code to identify the button that submitted the form.</span></span>

<span data-ttu-id="d22b0-162">Vous devrez écrire du code pour 1) lire les détails de film lorsque la page s’affiche tout d’abord et (2) supprimer le film lorsque l’utilisateur clique sur le bouton.</span><span class="sxs-lookup"><span data-stu-id="d22b0-162">You'll have to write code to 1) read the movie details when the page is first displayed and 2) actually delete the movie when the user clicks the button.</span></span>

## <a name="adding-code-to-read-a-single-movie"></a><span data-ttu-id="d22b0-163">Ajout de Code pour lire un film</span><span class="sxs-lookup"><span data-stu-id="d22b0-163">Adding Code to Read a Single Movie</span></span>

<span data-ttu-id="d22b0-164">En haut de la *DeleteMovie.cshtml* page, ajoutez le bloc de code suivant :</span><span class="sxs-lookup"><span data-stu-id="d22b0-164">At the top of the *DeleteMovie.cshtml* page, add the following code block:</span></span>

[!code-cshtml[Main](deleting-data/samples/sample5.cshtml)]

<span data-ttu-id="d22b0-165">Ce balisage est le même que le code correspondant dans le *EditMovie* page.</span><span class="sxs-lookup"><span data-stu-id="d22b0-165">This markup is the same as the corresponding code in the *EditMovie* page.</span></span> <span data-ttu-id="d22b0-166">Il obtient l’ID de film en dehors de la chaîne de requête et utilise l’ID pour lire un enregistrement à partir de la base de données.</span><span class="sxs-lookup"><span data-stu-id="d22b0-166">It gets the movie ID out of the query string and uses the ID to read a record from the database.</span></span> <span data-ttu-id="d22b0-167">Le code inclut le test de validation (`IsInt()` et `row != null`) pour vous assurer que l’ID de film passé à la page est valide.</span><span class="sxs-lookup"><span data-stu-id="d22b0-167">The code includes the validation test (`IsInt()` and `row != null`) to make sure that the movie ID being passed to the page is valid.</span></span>

<span data-ttu-id="d22b0-168">N’oubliez pas que ce code doit s’exécutent uniquement la première fois que la page s’exécute.</span><span class="sxs-lookup"><span data-stu-id="d22b0-168">Remember that this code should only run the first time the page runs.</span></span> <span data-ttu-id="d22b0-169">Vous ne souhaitez pas relire l’enregistrement de film à partir de la base de données lorsque l’utilisateur clique sur le **supprimer un film** bouton.</span><span class="sxs-lookup"><span data-stu-id="d22b0-169">You don't want to re-read the movie record from the database when the user clicks the **Delete Movie** button.</span></span> <span data-ttu-id="d22b0-170">Par conséquent, du code pour lire le film est à l’intérieur d’un test qui dit que `if(!IsPost)` &mdash; , autrement dit, *si la demande n’est pas une opération post (envoi d’un formulaire)*.</span><span class="sxs-lookup"><span data-stu-id="d22b0-170">Therefore, code to read the movie is inside a test that says `if(!IsPost)` &mdash; that is, *if the request is not a post operation (form submission)*.</span></span>

## <a name="adding-code-to-delete-the-selected-movie"></a><span data-ttu-id="d22b0-171">Ajout de Code pour supprimer le film sélectionné</span><span class="sxs-lookup"><span data-stu-id="d22b0-171">Adding Code to Delete the Selected Movie</span></span>

<span data-ttu-id="d22b0-172">Pour supprimer le film lorsque l’utilisateur clique sur le bouton, ajoutez le code suivant à l’intérieur de l’accolade fermante de la `@` bloc :</span><span class="sxs-lookup"><span data-stu-id="d22b0-172">To delete the movie when the user clicks the button, add the following code just inside the closing brace of the `@` block:</span></span>

[!code-csharp[Main](deleting-data/samples/sample6.cs)]

<span data-ttu-id="d22b0-173">Ce code est similaire au code de mise à jour un enregistrement existant, mais il est plus simple.</span><span class="sxs-lookup"><span data-stu-id="d22b0-173">This code is similar to the code for updating an existing record, but simpler.</span></span> <span data-ttu-id="d22b0-174">Le code exécute essentiellement une SQL `Delete` instruction.</span><span class="sxs-lookup"><span data-stu-id="d22b0-174">The code basically runs a SQL `Delete` statement.</span></span>

 <span data-ttu-id="d22b0-175">Comme dans le *EditMovie* page, le code se trouve dans un `if(IsPost)` bloc.</span><span class="sxs-lookup"><span data-stu-id="d22b0-175">As in the *EditMovie* page, the code is in an `if(IsPost)` block.</span></span> <span data-ttu-id="d22b0-176">Cette fois-ci, le `if()` condition est un peu plus compliquée :</span><span class="sxs-lookup"><span data-stu-id="d22b0-176">This time, the `if()` condition is a little more complicated:</span></span> 

[!code-csharp[Main](deleting-data/samples/sample7.cs)]

<span data-ttu-id="d22b0-177">Il existe deux conditions ici.</span><span class="sxs-lookup"><span data-stu-id="d22b0-177">There are two conditions here.</span></span> <span data-ttu-id="d22b0-178">La première est que la page est soumise, comme vous l’avez vu avant &mdash; `if(IsPost)`.</span><span class="sxs-lookup"><span data-stu-id="d22b0-178">The first is that the page is being submitted, as you've seen before &mdash; `if(IsPost)`.</span></span>

<span data-ttu-id="d22b0-179">La deuxième condition est `!Request["buttonDelete"].IsEmpty()`, ce qui signifie que la demande a un objet nommé `buttonDelete`.</span><span class="sxs-lookup"><span data-stu-id="d22b0-179">The second condition is `!Request["buttonDelete"].IsEmpty()`, meaning that the request has an object named `buttonDelete`.</span></span> <span data-ttu-id="d22b0-180">Il est vrai, c’est un moyen indirect de bouton qui a envoyé le formulaire de test.</span><span class="sxs-lookup"><span data-stu-id="d22b0-180">Admittedly, it's an indirect way of testing which button submitted the form.</span></span> <span data-ttu-id="d22b0-181">Si un formulaire contient plusieurs boutons d’envoi, uniquement le nom du bouton sur lequel l’utilisateur a cliqué sur apparaît dans la demande.</span><span class="sxs-lookup"><span data-stu-id="d22b0-181">If a form contains multiple submit buttons, only the name of the button that was clicked appears in the request.</span></span> <span data-ttu-id="d22b0-182">Par conséquent, logiquement, si le nom d’un bouton spécifique apparaît dans la demande &mdash; ou comme indiqué dans le code, si ce bouton n’est pas vide &mdash; qui est le bouton qui a envoyé le formulaire.</span><span class="sxs-lookup"><span data-stu-id="d22b0-182">Therefore, logically, if the name of a particular button appears in the request &mdash; or as stated in the code, if that button isn't empty &mdash; that's the button that submitted the form.</span></span>

<span data-ttu-id="d22b0-183">Le `&&` moyen de l’opérateur « et » (AND logique).</span><span class="sxs-lookup"><span data-stu-id="d22b0-183">The `&&` operator means "and" (logical AND).</span></span> <span data-ttu-id="d22b0-184">Par conséquent, l’ensemble `if` condition est...</span><span class="sxs-lookup"><span data-stu-id="d22b0-184">Therefore the entire `if` condition is ...</span></span>

<span data-ttu-id="d22b0-185">*Cette demande est une publication (pas une demande de première)*</span><span class="sxs-lookup"><span data-stu-id="d22b0-185">*This request is a post (not a first-time request)*</span></span>  
  
 <span data-ttu-id="d22b0-186">AND</span><span class="sxs-lookup"><span data-stu-id="d22b0-186">AND</span></span>  
  
<span data-ttu-id="d22b0-187">*Le* `buttonDelete` *bouton a été le bouton qui a envoyé le formulaire.*</span><span class="sxs-lookup"><span data-stu-id="d22b0-187">*The* `buttonDelete`*button was the button that submitted the form.*</span></span>

<span data-ttu-id="d22b0-188">Ce formulaire (en réalité, cette page) contienne un seul bouton, c’est pourquoi le test supplémentaire pour `buttonDelete` techniquement n’est pas obligatoire.</span><span class="sxs-lookup"><span data-stu-id="d22b0-188">This form (in fact, this page) contains only one button, so the additional test for `buttonDelete` is technically not required.</span></span> <span data-ttu-id="d22b0-189">Cependant, vous êtes sur le point d’effectuer une opération qui va supprimer définitivement les données.</span><span class="sxs-lookup"><span data-stu-id="d22b0-189">Still, you're about to perform an operation that will permanently remove data.</span></span> <span data-ttu-id="d22b0-190">Vous voulez donc veillez possible que vous effectuez l’opération uniquement lorsque l’utilisateur a explicitement demandé.</span><span class="sxs-lookup"><span data-stu-id="d22b0-190">So you want to be as sure as possible that you're performing the operation only when the user has explicitly requested it.</span></span> <span data-ttu-id="d22b0-191">Par exemple, que vous développée plus loin de cette page et ajouté des autres boutons.</span><span class="sxs-lookup"><span data-stu-id="d22b0-191">For example, suppose that you expanded this page later and added other buttons to it.</span></span> <span data-ttu-id="d22b0-192">Même dans ce cas, le code qui supprime le film s’exécute uniquement si le `buttonDelete` bouton l’utilisateur a cliqué.</span><span class="sxs-lookup"><span data-stu-id="d22b0-192">Even then, the code that deletes the movie will run only if the `buttonDelete` button was clicked.</span></span>

<span data-ttu-id="d22b0-193">Comme dans le *EditMovie* page, vous obtenez l’ID à partir du champ masqué et puis exécutez la commande SQL.</span><span class="sxs-lookup"><span data-stu-id="d22b0-193">As in the *EditMovie* page, you get the ID from the hidden field and then run the SQL command.</span></span> <span data-ttu-id="d22b0-194">La syntaxe pour la `Delete` instruction est :</span><span class="sxs-lookup"><span data-stu-id="d22b0-194">The syntax for the `Delete` statement is:</span></span>

`DELETE FROM table WHERE ID = value`

<span data-ttu-id="d22b0-195">Il est essentiel d’inclure la `WHERE` clause et le code.</span><span class="sxs-lookup"><span data-stu-id="d22b0-195">It's vital to include the `WHERE` clause and the ID.</span></span> <span data-ttu-id="d22b0-196">Si vous omettez la clause WHERE, *tous les enregistrements dans la table seront supprimés*.</span><span class="sxs-lookup"><span data-stu-id="d22b0-196">If you leave out the WHERE clause, *all the records in the table will be deleted*.</span></span> <span data-ttu-id="d22b0-197">Comme vous l’avez vu, vous passez la valeur d’ID pour la commande SQL à l’aide d’un espace réservé.</span><span class="sxs-lookup"><span data-stu-id="d22b0-197">As you have seen, you pass the ID value to the SQL command by using a placeholder.</span></span>

## <a name="testing-the-movie-delete-process"></a><span data-ttu-id="d22b0-198">Tester le processus de suppression de film</span><span class="sxs-lookup"><span data-stu-id="d22b0-198">Testing the Movie Delete Process</span></span>

<span data-ttu-id="d22b0-199">Vous pouvez maintenant tester.</span><span class="sxs-lookup"><span data-stu-id="d22b0-199">Now you can test.</span></span> <span data-ttu-id="d22b0-200">Exécutez le *films* page, puis cliquez sur **supprimer** en regard d’un film.</span><span class="sxs-lookup"><span data-stu-id="d22b0-200">Run the *Movies* page, and click **Delete** next to a movie.</span></span> <span data-ttu-id="d22b0-201">Lorsque le *DeleteMovie* page s’affiche, cliquez sur **supprimer un film**.</span><span class="sxs-lookup"><span data-stu-id="d22b0-201">When the *DeleteMovie* page appears, click **Delete Movie**.</span></span>

![Supprimer la page de film avec le bouton Supprimer un film mis en surbrillance](deleting-data/_static/image4.png)

<span data-ttu-id="d22b0-203">Lorsque vous cliquez sur le bouton, le code supprime les films et retourne à la liste de films.</span><span class="sxs-lookup"><span data-stu-id="d22b0-203">When you click the button, the code deletes the movies and returns to the movie listing.</span></span> <span data-ttu-id="d22b0-204">Vous pouvez y rechercher le film supprimé et confirmez qu’il est bien été supprimé.</span><span class="sxs-lookup"><span data-stu-id="d22b0-204">There you can search for the deleted movie and confirm that it's been deleted.</span></span>

## <a name="coming-up-next"></a><span data-ttu-id="d22b0-205">Prochaine</span><span class="sxs-lookup"><span data-stu-id="d22b0-205">Coming Up Next</span></span>

<span data-ttu-id="d22b0-206">Le didacticiel suivant vous montre comment accorder à toutes les pages sur votre site d’une apparence commune et la disposition.</span><span class="sxs-lookup"><span data-stu-id="d22b0-206">The next tutorial shows you how to give all the pages on your site a common look and layout.</span></span>

## <a name="complete-listing-for-movie-page-updated-with-delete-links"></a><span data-ttu-id="d22b0-207">Intégralité de la Page de film (mis à jour avec les liens de suppression)</span><span class="sxs-lookup"><span data-stu-id="d22b0-207">Complete Listing for Movie Page (Updated with Delete Links)</span></span>

[!code-cshtml[Main](deleting-data/samples/sample8.cshtml)]

## <a name="complete-listing-for-deletemovie-page"></a><span data-ttu-id="d22b0-208">Liste complète de la Page de DeleteMovie</span><span class="sxs-lookup"><span data-stu-id="d22b0-208">Complete Listing for DeleteMovie Page</span></span>

[!code-cshtml[Main](deleting-data/samples/sample9.cshtml)]

## <a name="additional-resources"></a><span data-ttu-id="d22b0-209">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="d22b0-209">Additional Resources</span></span>

- [<span data-ttu-id="d22b0-210">Introduction à la programmation Web ASP.NET en utilisant la syntaxe Razor</span><span class="sxs-lookup"><span data-stu-id="d22b0-210">Introduction to ASP.NET Web Programming by Using the Razor Syntax</span></span>](../introducing-razor-syntax-c.md)
- <span data-ttu-id="d22b0-211">[Instruction DELETE de SQL](http://www.w3schools.com/sql/sql_delete.asp) sur le site W3Schools</span><span class="sxs-lookup"><span data-stu-id="d22b0-211">[SQL DELETE Statement](http://www.w3schools.com/sql/sql_delete.asp) on the W3Schools site</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="d22b0-212">[Précédent](updating-data.md)
> [Suivant](layouts.md)</span><span class="sxs-lookup"><span data-stu-id="d22b0-212">[Previous](updating-data.md)
[Next](layouts.md)</span></span>
