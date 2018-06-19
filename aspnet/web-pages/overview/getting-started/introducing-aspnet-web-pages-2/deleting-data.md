---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/deleting-data
title: Présentation des Pages Web ASP.NET - suppression des données de la base de données | Documents Microsoft
author: tfitzmac
description: Ce didacticiel vous montre comment supprimer une entrée de la base de données individuels. Il suppose que vous avez terminé la série grâce à la mise à jour des données de base de données dans ASP.NET Web PA...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/02/2018
ms.topic: article
ms.assetid: 75b5c1cf-84bd-434f-8a86-85c568eb5b09
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/deleting-data
msc.type: authoredcontent
ms.openlocfilehash: 146199e862cd6fa2607671d31633476b1cb67021
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30897414"
---
<a name="introducing-aspnet-web-pages---deleting-database-data"></a><span data-ttu-id="5684c-104">Présentation des Pages Web ASP.NET - suppression des données de la base de données</span><span class="sxs-lookup"><span data-stu-id="5684c-104">Introducing ASP.NET Web Pages - Deleting Database Data</span></span>
====================
<span data-ttu-id="5684c-105">par [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="5684c-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="5684c-106">Ce didacticiel vous montre comment supprimer une entrée de la base de données individuels.</span><span class="sxs-lookup"><span data-stu-id="5684c-106">This tutorial shows you how to delete an individual database entry.</span></span> <span data-ttu-id="5684c-107">Il suppose que vous avez terminé la série via [mise à jour de base de données dans les Pages Web ASP.NET](updating-data.md).</span><span class="sxs-lookup"><span data-stu-id="5684c-107">It assumes you have completed the series through [Updating Database Data in ASP.NET Web Pages](updating-data.md).</span></span>
> 
> <span data-ttu-id="5684c-108">Ce que vous allez apprendre :</span><span class="sxs-lookup"><span data-stu-id="5684c-108">What you'll learn:</span></span>
> 
> - <span data-ttu-id="5684c-109">Comment sélectionner un enregistrement individuel à partir d’une liste d’enregistrements.</span><span class="sxs-lookup"><span data-stu-id="5684c-109">How to select an individual record from a listing of records.</span></span>
> - <span data-ttu-id="5684c-110">Comment supprimer un enregistrement unique d’une base de données.</span><span class="sxs-lookup"><span data-stu-id="5684c-110">How to delete a single record from a database.</span></span>
> - <span data-ttu-id="5684c-111">Comment vérifier que l’utilisateur a cliqué sur un bouton spécifique dans un formulaire.</span><span class="sxs-lookup"><span data-stu-id="5684c-111">How to check that a specific button was clicked in a form.</span></span>
>   
> 
> <span data-ttu-id="5684c-112">Fonctionnalités technologies abordées :</span><span class="sxs-lookup"><span data-stu-id="5684c-112">Features/technologies discussed:</span></span>
> 
> - <span data-ttu-id="5684c-113">Le `WebGrid` helper.</span><span class="sxs-lookup"><span data-stu-id="5684c-113">The `WebGrid` helper.</span></span>
> - <span data-ttu-id="5684c-114">L’instruction SQL `Delete` commande.</span><span class="sxs-lookup"><span data-stu-id="5684c-114">The SQL `Delete` command.</span></span>
> - <span data-ttu-id="5684c-115">Le `Database.Execute` méthode à exécuter SQL `Delete` commande.</span><span class="sxs-lookup"><span data-stu-id="5684c-115">The `Database.Execute` method to run a SQL `Delete` command.</span></span>


## <a name="what-youll-build"></a><span data-ttu-id="5684c-116">Ce que vous allez générer</span><span class="sxs-lookup"><span data-stu-id="5684c-116">What You'll Build</span></span>

<span data-ttu-id="5684c-117">Dans le didacticiel précédent, vous avez appris comment mettre à jour un enregistrement de base de données existant.</span><span class="sxs-lookup"><span data-stu-id="5684c-117">In the previous tutorial, you learned how to update an existing database record.</span></span> <span data-ttu-id="5684c-118">Ce didacticiel est similaire, mais au lieu de la mise à jour de l’enregistrement, vous devez le supprimer.</span><span class="sxs-lookup"><span data-stu-id="5684c-118">This tutorial is similar, except that instead of updating the record, you'll delete it.</span></span> <span data-ttu-id="5684c-119">Les processus sont très similaire, sauf que la suppression est plus simple, donc, ce didacticiel est court.</span><span class="sxs-lookup"><span data-stu-id="5684c-119">The processes are much the same, except that deleting is simpler, so this tutorial will be short.</span></span>

<span data-ttu-id="5684c-120">Dans le *films* page, vous allez mettre à jour le `WebGrid` helper afin qu’elle affiche un **supprimer** lien en regard de chaque séquence pour accompagner le **modifier** lien que vous avez ajoutée précédemment.</span><span class="sxs-lookup"><span data-stu-id="5684c-120">In the *Movies* page, you'll update the `WebGrid` helper so that it displays a **Delete** link next to each movie to accompany the **Edit** link you added earlier.</span></span>

![Page de films affichant un lien Supprimer pour chaque vidéo](deleting-data/_static/image1.png)

<span data-ttu-id="5684c-122">Comme avec la modification, lorsque vous cliquez sur le **supprimer** lien, il vous redirige vers une autre page, où les informations de film sont déjà dans un formulaire :</span><span class="sxs-lookup"><span data-stu-id="5684c-122">As with editing, when you click the **Delete** link, it takes you to a different page, where the movie information is already in a form:</span></span>

![Supprimer la page de film avec un film affiché](deleting-data/_static/image2.png)

<span data-ttu-id="5684c-124">Vous pouvez ensuite cliquer sur le bouton pour supprimer définitivement un enregistrement.</span><span class="sxs-lookup"><span data-stu-id="5684c-124">You can then click the button to delete the record permanently.</span></span>

## <a name="adding-a-delete-link-to-the-movie-listing"></a><span data-ttu-id="5684c-125">Ajout d’un lien de suppression à la liste de films</span><span class="sxs-lookup"><span data-stu-id="5684c-125">Adding a Delete Link to the Movie Listing</span></span>

<span data-ttu-id="5684c-126">Vous allez commencer par ajouter un **supprimer** créer un lien vers le `WebGrid` helper.</span><span class="sxs-lookup"><span data-stu-id="5684c-126">You'll start by adding a **Delete** link to the `WebGrid` helper.</span></span> <span data-ttu-id="5684c-127">Ce lien est similaire à la **modifier** lien que vous avez ajouté à un didacticiel précédent.</span><span class="sxs-lookup"><span data-stu-id="5684c-127">This link is similar to the **Edit** link you added in a previous tutorial.</span></span>

<span data-ttu-id="5684c-128">Ouvrez le *Movies.cshtml* fichier.</span><span class="sxs-lookup"><span data-stu-id="5684c-128">Open the *Movies.cshtml* file.</span></span>

<span data-ttu-id="5684c-129">Modifier la `WebGrid` balisage dans le corps de la page en ajoutant une colonne.</span><span class="sxs-lookup"><span data-stu-id="5684c-129">Change the `WebGrid` markup in the body of the page by adding a column.</span></span> <span data-ttu-id="5684c-130">Voici le balisage modifié :</span><span class="sxs-lookup"><span data-stu-id="5684c-130">Here's the modified markup:</span></span>

[!code-html[Main](deleting-data/samples/sample1.html?highlight=9-10)]

<span data-ttu-id="5684c-131">La nouvelle colonne est celle-ci :</span><span class="sxs-lookup"><span data-stu-id="5684c-131">The new column is this one:</span></span>

[!code-html[Main](deleting-data/samples/sample2.html)]

<span data-ttu-id="5684c-132">La manière de configurer la grille, le **modifier** colonne est plus à gauche dans la grille et la **supprimer** colonne est la plus à droite.</span><span class="sxs-lookup"><span data-stu-id="5684c-132">The way the grid is configured, the **Edit** column is leftmost in the grid and the **Delete** column is rightmost.</span></span> <span data-ttu-id="5684c-133">(Il existe une virgule après la `Year` colonne maintenant, au cas où vous n’avez pas remarquer que.) Rien de spécial sur lequel ces colonnes lien go, et vous pouvez aussi facilement les placer en regard de l’autre.</span><span class="sxs-lookup"><span data-stu-id="5684c-133">(There's a comma after the `Year` column now, in case you didn't notice that.) There's nothing special about where these link columns go, and you could as easily put them next to each other.</span></span> <span data-ttu-id="5684c-134">Dans ce cas, ils sont distincts pour les rendre plus difficile à se mélanger.</span><span class="sxs-lookup"><span data-stu-id="5684c-134">In this case, they're separate to make them harder to get mixed up.</span></span>

![Page de films avec des liens d’édition et les détails marquée pour montrer qu’ils ne sont pas adjacents](deleting-data/_static/image3.png)

<span data-ttu-id="5684c-136">La nouvelle colonne affiche un lien (`<a>` élément) dont le texte indique « Delete ».</span><span class="sxs-lookup"><span data-stu-id="5684c-136">The new column shows a link (`<a>` element) whose text says "Delete".</span></span> <span data-ttu-id="5684c-137">La cible du lien (son `href` attribut) est un code qui correspond finalement à quelque chose comme cette URL, avec la `id` valeur différente pour chaque vidéo :</span><span class="sxs-lookup"><span data-stu-id="5684c-137">The target of the link (its `href` attribute) is code that ultimately resolves to something like this URL, with the `id` value different for each movie:</span></span>

[!code-css[Main](deleting-data/samples/sample3.css)]

<span data-ttu-id="5684c-138">Ce lien appelle une page nommée *DeleteMovie* et lui passer l’ID de la séquence que vous avez sélectionné.</span><span class="sxs-lookup"><span data-stu-id="5684c-138">This link will invoke a page named *DeleteMovie* and pass it the ID of the movie you've selected.</span></span>

<span data-ttu-id="5684c-139">Ce didacticiel ne sera pas entrer dans les détails sur la façon dont ce lien est construit, car il est presque identique à la **modifier** lien à partir du didacticiel précédent ([mise à jour de base de données dans ASP.NET Web Pages](updating-data.md)).</span><span class="sxs-lookup"><span data-stu-id="5684c-139">This tutorial won't go into detail about how this link is constructed, because it's almost identical to the **Edit** link from the previous tutorial ([Updating Database Data in ASP.NET Web Pages](updating-data.md)).</span></span>

## <a name="creating-the-delete-page"></a><span data-ttu-id="5684c-140">Création de la Page de suppression</span><span class="sxs-lookup"><span data-stu-id="5684c-140">Creating the Delete Page</span></span>

<span data-ttu-id="5684c-141">Vous pouvez désormais créer la page qui sera la cible pour le **supprimer** liaison dans la grille.</span><span class="sxs-lookup"><span data-stu-id="5684c-141">Now you can create the page that will be the target for the **Delete** link in the grid.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="5684c-142">**Important** la technique d’abord en sélectionnant un enregistrement à supprimer, puis en utilisant une page distincte et un bouton pour confirmer le processus est extrêmement importante pour la sécurité.</span><span class="sxs-lookup"><span data-stu-id="5684c-142">**Important** The technique of first selecting a record to delete and then using a separate page and button to confirm the process is extremely important for security.</span></span> <span data-ttu-id="5684c-143">Comme vous avez lu dans les didacticiels précédents, ce qui *tout* tri de modification de votre site Web doit *toujours* être effectuée à l’aide d’un formulaire &mdash; , à l’aide d’une opération HTTP POST.</span><span class="sxs-lookup"><span data-stu-id="5684c-143">As you've read in previous tutorials, making *any* sort of change to your website should *always* be done using a form &mdash; that is, using an HTTP POST operation.</span></span> <span data-ttu-id="5684c-144">Si vous l’avez fait possible de modifier le site suffit de cliquer sur un lien (c'est-à-dire, en utilisant une opération GET), les personnes pourrait faire des demandes simples sur votre site et supprimer vos données.</span><span class="sxs-lookup"><span data-stu-id="5684c-144">If you made it possible to change the site just by clicking a link (that is, using a GET operation), people could make simple requests to your site and delete your data.</span></span> <span data-ttu-id="5684c-145">Même un moteur de recherche qui est l’indexation de votre site peut supprimer par inadvertance des données simplement en suivant les liens.</span><span class="sxs-lookup"><span data-stu-id="5684c-145">Even a search-engine crawler that's indexing your site could inadvertently delete data just by following links.</span></span>
> 
> <span data-ttu-id="5684c-146">Lorsque votre application permet de modifier un enregistrement, vous devez présenter l’enregistrement à l’utilisateur pour le modifier quand même.</span><span class="sxs-lookup"><span data-stu-id="5684c-146">When your app lets people change a record, you have to present the record to the user for editing anyway.</span></span> <span data-ttu-id="5684c-147">Mais vous pouvez être tenté d’ignorer cette étape pour la suppression d’un enregistrement.</span><span class="sxs-lookup"><span data-stu-id="5684c-147">But you might be tempted to skip this step for deleting a record.</span></span> <span data-ttu-id="5684c-148">Ne passez pas bien que cette étape.</span><span class="sxs-lookup"><span data-stu-id="5684c-148">Don't skip that step, though.</span></span> <span data-ttu-id="5684c-149">(Il est également utile pour les utilisateurs voient l’enregistrement et de confirmer qu’ils vous supprimez l’enregistrement qui ils ont vocation.)</span><span class="sxs-lookup"><span data-stu-id="5684c-149">(It's also helpful for users to see the record and confirm that they're deleting the record that they intended.)</span></span>
> 
> <span data-ttu-id="5684c-150">Dans un jeu de didacticiel ultérieur, vous verrez comment ajouter des fonctionnalités de connexion pour un utilisateur devra se connecter avant de supprimer un enregistrement.</span><span class="sxs-lookup"><span data-stu-id="5684c-150">In a subsequent tutorial set, you'll see how to add login functionality so a user would have to log in before deleting a record.</span></span>


<span data-ttu-id="5684c-151">Créer une page nommée *DeleteMovie.cshtml* et remplacez ce qui est dans le fichier avec le balisage suivant :</span><span class="sxs-lookup"><span data-stu-id="5684c-151">Create a page named *DeleteMovie.cshtml* and replace what's in the file with the following markup:</span></span>

[!code-cshtml[Main](deleting-data/samples/sample4.cshtml)]

<span data-ttu-id="5684c-152">Ce balisage est similaire à la *EditMovie* pages, à ceci près qu’au lieu d’utiliser des zones de texte (`<input type="text">`), le balisage inclut `<span>` éléments.</span><span class="sxs-lookup"><span data-stu-id="5684c-152">This markup is like the *EditMovie* pages, except that instead of using text boxes (`<input type="text">`), the markup includes `<span>` elements.</span></span> <span data-ttu-id="5684c-153">Il n’y a rien ici à modifier.</span><span class="sxs-lookup"><span data-stu-id="5684c-153">There's nothing here to edit.</span></span> <span data-ttu-id="5684c-154">Il vous est afficher les détails des films afin que les utilisateurs peuvent s’assurer qu’ils vous supprimez le film à droite.</span><span class="sxs-lookup"><span data-stu-id="5684c-154">All you have to do is display the movie details so that users can make sure that they're deleting the right movie.</span></span>

<span data-ttu-id="5684c-155">Le balisage contient déjà un lien qui permet à l’utilisateur de revenir à la page de liste de films.</span><span class="sxs-lookup"><span data-stu-id="5684c-155">The markup already contains a link that lets the user return to the movie listing page.</span></span>

<span data-ttu-id="5684c-156">Comme dans le *EditMovie* page, l’ID de la séquence sélectionnée est stockée dans un champ masqué.</span><span class="sxs-lookup"><span data-stu-id="5684c-156">As in the *EditMovie* page, the ID of the selected movie is stored in a hidden field.</span></span> <span data-ttu-id="5684c-157">(Il est passé dans la page en premier lieu en tant que valeur de chaîne de requête.) Il existe un `Html.ValidationSummary` appel qui affiche les erreurs de validation.</span><span class="sxs-lookup"><span data-stu-id="5684c-157">(It's passed into the page in the first place as a query string value.) There's an `Html.ValidationSummary` call that will display validation errors.</span></span> <span data-ttu-id="5684c-158">Dans ce cas, l’erreur peut être qu’aucun ID de film a été transmis à la page ou que l’ID de film n’est pas valide.</span><span class="sxs-lookup"><span data-stu-id="5684c-158">In this case, the error might be that no movie ID was passed to the page or that the movie ID is invalid.</span></span> <span data-ttu-id="5684c-159">Cette situation peut se produire si une personne a exécuté cette page sans avoir à sélectionner un film dans le *films* page.</span><span class="sxs-lookup"><span data-stu-id="5684c-159">This situation could occur if someone ran this page without first selecting a movie in the *Movies* page.</span></span>

<span data-ttu-id="5684c-160">La légende du bouton est **supprimer un film**, et son attribut de nom est défini sur `buttonDelete`.</span><span class="sxs-lookup"><span data-stu-id="5684c-160">The button caption is **Delete Movie**, and its name attribute is set to `buttonDelete`.</span></span> <span data-ttu-id="5684c-161">Le `name` attribut servira dans le code pour identifier le bouton qui a envoyé le formulaire.</span><span class="sxs-lookup"><span data-stu-id="5684c-161">The `name` attribute will be used in the code to identify the button that submitted the form.</span></span>

<span data-ttu-id="5684c-162">Vous devrez écrire du code pour (1) lire les détails de film lorsque la page s’affiche tout d’abord et (2) réellement supprimer le film lorsque l’utilisateur clique sur le bouton.</span><span class="sxs-lookup"><span data-stu-id="5684c-162">You'll have to write code to 1) read the movie details when the page is first displayed and 2) actually delete the movie when the user clicks the button.</span></span>

## <a name="adding-code-to-read-a-single-movie"></a><span data-ttu-id="5684c-163">Ajout de Code pour lire un film unique</span><span class="sxs-lookup"><span data-stu-id="5684c-163">Adding Code to Read a Single Movie</span></span>

<span data-ttu-id="5684c-164">En haut de la *DeleteMovie.cshtml* , ajoutez le bloc de code suivant :</span><span class="sxs-lookup"><span data-stu-id="5684c-164">At the top of the *DeleteMovie.cshtml* page, add the following code block:</span></span>

[!code-cshtml[Main](deleting-data/samples/sample5.cshtml)]

<span data-ttu-id="5684c-165">Ce balisage est le même que le code correspondant dans le *EditMovie* page.</span><span class="sxs-lookup"><span data-stu-id="5684c-165">This markup is the same as the corresponding code in the *EditMovie* page.</span></span> <span data-ttu-id="5684c-166">Il obtient l’ID de film hors de la chaîne de requête et utilise l’ID pour lire un enregistrement à partir de la base de données.</span><span class="sxs-lookup"><span data-stu-id="5684c-166">It gets the movie ID out of the query string and uses the ID to read a record from the database.</span></span> <span data-ttu-id="5684c-167">Le code inclut le test de validation (`IsInt()` et `row != null`) pour vous assurer que l’ID de film qui est passée à la page est valide.</span><span class="sxs-lookup"><span data-stu-id="5684c-167">The code includes the validation test (`IsInt()` and `row != null`) to make sure that the movie ID being passed to the page is valid.</span></span>

<span data-ttu-id="5684c-168">N’oubliez pas que ce code doit exécuter uniquement la première fois que la page s’exécute.</span><span class="sxs-lookup"><span data-stu-id="5684c-168">Remember that this code should only run the first time the page runs.</span></span> <span data-ttu-id="5684c-169">Vous ne souhaitez pas relire l’enregistrement vidéo à partir de la base de données lorsque l’utilisateur clique sur le **supprimer un film** bouton.</span><span class="sxs-lookup"><span data-stu-id="5684c-169">You don't want to re-read the movie record from the database when the user clicks the **Delete Movie** button.</span></span> <span data-ttu-id="5684c-170">Par conséquent, le code pour lire le film est à l’intérieur d’un test qui indique que `if(!IsPost)` &mdash; , *si la demande n’est pas une opération post (envoi du formulaire)*.</span><span class="sxs-lookup"><span data-stu-id="5684c-170">Therefore, code to read the movie is inside a test that says `if(!IsPost)` &mdash; that is, *if the request is not a post operation (form submission)*.</span></span>

## <a name="adding-code-to-delete-the-selected-movie"></a><span data-ttu-id="5684c-171">Ajout de Code pour supprimer le projet sélectionné</span><span class="sxs-lookup"><span data-stu-id="5684c-171">Adding Code to Delete the Selected Movie</span></span>

<span data-ttu-id="5684c-172">Pour supprimer le film lorsque l’utilisateur clique sur le bouton, ajoutez le code suivant à l’intérieur de l’accolade fermante de la `@` bloc :</span><span class="sxs-lookup"><span data-stu-id="5684c-172">To delete the movie when the user clicks the button, add the following code just inside the closing brace of the `@` block:</span></span>

[!code-csharp[Main](deleting-data/samples/sample6.cs)]

<span data-ttu-id="5684c-173">Ce code est semblable au code de mise à jour un enregistrement existant, mais il est plus simple.</span><span class="sxs-lookup"><span data-stu-id="5684c-173">This code is similar to the code for updating an existing record, but simpler.</span></span> <span data-ttu-id="5684c-174">Le code exécute essentiellement SQL `Delete` instruction.</span><span class="sxs-lookup"><span data-stu-id="5684c-174">The code basically runs a SQL `Delete` statement.</span></span>

 <span data-ttu-id="5684c-175">Comme dans le *EditMovie* page, le code est dans un `if(IsPost)` bloc.</span><span class="sxs-lookup"><span data-stu-id="5684c-175">As in the *EditMovie* page, the code is in an `if(IsPost)` block.</span></span> <span data-ttu-id="5684c-176">Cette fois-ci, le `if()` condition est un peu plus complexe :</span><span class="sxs-lookup"><span data-stu-id="5684c-176">This time, the `if()` condition is a little more complicated:</span></span> 

[!code-csharp[Main](deleting-data/samples/sample7.cs)]

<span data-ttu-id="5684c-177">Il existe deux conditions ici.</span><span class="sxs-lookup"><span data-stu-id="5684c-177">There are two conditions here.</span></span> <span data-ttu-id="5684c-178">La première est que la page est soumise, comme vous l’avez vu avant &mdash; `if(IsPost)`.</span><span class="sxs-lookup"><span data-stu-id="5684c-178">The first is that the page is being submitted, as you've seen before &mdash; `if(IsPost)`.</span></span>

<span data-ttu-id="5684c-179">La deuxième condition est `!Request["buttonDelete"].IsEmpty()`, ce qui signifie que la demande comporte un objet nommé `buttonDelete`.</span><span class="sxs-lookup"><span data-stu-id="5684c-179">The second condition is `!Request["buttonDelete"].IsEmpty()`, meaning that the request has an object named `buttonDelete`.</span></span> <span data-ttu-id="5684c-180">Certes, il est un moyen indirect de bouton qui a envoyé le formulaire de test.</span><span class="sxs-lookup"><span data-stu-id="5684c-180">Admittedly, it's an indirect way of testing which button submitted the form.</span></span> <span data-ttu-id="5684c-181">Si un formulaire contient plusieurs boutons d’envoi, uniquement le nom de l’utilisateur a cliqué sur le bouton s’affiche dans la demande.</span><span class="sxs-lookup"><span data-stu-id="5684c-181">If a form contains multiple submit buttons, only the name of the button that was clicked appears in the request.</span></span> <span data-ttu-id="5684c-182">Par conséquent, logiquement, si le nom d’un bouton apparaît dans la demande &mdash; ou comme indiqué dans le code, si ce bouton n’est pas vide &mdash; qui est le bouton qui a envoyé le formulaire.</span><span class="sxs-lookup"><span data-stu-id="5684c-182">Therefore, logically, if the name of a particular button appears in the request &mdash; or as stated in the code, if that button isn't empty &mdash; that's the button that submitted the form.</span></span>

<span data-ttu-id="5684c-183">Le `&&` moyen de l’opérateur « et » (et logique).</span><span class="sxs-lookup"><span data-stu-id="5684c-183">The `&&` operator means "and" (logical AND).</span></span> <span data-ttu-id="5684c-184">Par conséquent, l’ensemble `if` condition est en cours...</span><span class="sxs-lookup"><span data-stu-id="5684c-184">Therefore the entire `if` condition is ...</span></span>

<span data-ttu-id="5684c-185">*Cette demande est une demande post (pas une demande de première)*</span><span class="sxs-lookup"><span data-stu-id="5684c-185">*This request is a post (not a first-time request)*</span></span>  
  
 <span data-ttu-id="5684c-186">AND</span><span class="sxs-lookup"><span data-stu-id="5684c-186">AND</span></span>  
  
<span data-ttu-id="5684c-187">*Le* `buttonDelete` *bouton est le bouton qui a envoyé le formulaire.*</span><span class="sxs-lookup"><span data-stu-id="5684c-187">*The* `buttonDelete`*button was the button that submitted the form.*</span></span>

<span data-ttu-id="5684c-188">Ce formulaire (en fait, cette page) contient un seul bouton, par conséquent, le test supplémentaire pour `buttonDelete` techniquement n’est pas obligatoire.</span><span class="sxs-lookup"><span data-stu-id="5684c-188">This form (in fact, this page) contains only one button, so the additional test for `buttonDelete` is technically not required.</span></span> <span data-ttu-id="5684c-189">Cependant, vous êtes sur le point d’effectuer une opération qui va supprimer définitivement les données.</span><span class="sxs-lookup"><span data-stu-id="5684c-189">Still, you're about to perform an operation that will permanently remove data.</span></span> <span data-ttu-id="5684c-190">Vous souhaitez donc veillez possible que vous effectuez l’opération uniquement lorsque l’utilisateur a explicitement demandé.</span><span class="sxs-lookup"><span data-stu-id="5684c-190">So you want to be as sure as possible that you're performing the operation only when the user has explicitly requested it.</span></span> <span data-ttu-id="5684c-191">Par exemple, que vous développée plus loin de cette page et ajouté des autres boutons à celle-ci.</span><span class="sxs-lookup"><span data-stu-id="5684c-191">For example, suppose that you expanded this page later and added other buttons to it.</span></span> <span data-ttu-id="5684c-192">Même alors, le code qui supprime le film s’exécutera uniquement si la `buttonDelete` bouton a été utilisé.</span><span class="sxs-lookup"><span data-stu-id="5684c-192">Even then, the code that deletes the movie will run only if the `buttonDelete` button was clicked.</span></span>

<span data-ttu-id="5684c-193">Comme dans le *EditMovie* page, que vous obtenez l’ID à partir du champ masqué et puis exécutez la commande SQL.</span><span class="sxs-lookup"><span data-stu-id="5684c-193">As in the *EditMovie* page, you get the ID from the hidden field and then run the SQL command.</span></span> <span data-ttu-id="5684c-194">La syntaxe de la `Delete` instruction est :</span><span class="sxs-lookup"><span data-stu-id="5684c-194">The syntax for the `Delete` statement is:</span></span>

`DELETE FROM table WHERE ID = value`

<span data-ttu-id="5684c-195">Il est essentiel d’inclure la `WHERE` clause et le code.</span><span class="sxs-lookup"><span data-stu-id="5684c-195">It's vital to include the `WHERE` clause and the ID.</span></span> <span data-ttu-id="5684c-196">Si vous omettez la clause WHERE, *tous les enregistrements dans la table seront supprimés*.</span><span class="sxs-lookup"><span data-stu-id="5684c-196">If you leave out the WHERE clause, *all the records in the table will be deleted*.</span></span> <span data-ttu-id="5684c-197">Comme vous l’avez vu, vous passez la valeur d’ID pour la commande SQL à l’aide d’un espace réservé.</span><span class="sxs-lookup"><span data-stu-id="5684c-197">As you have seen, you pass the ID value to the SQL command by using a placeholder.</span></span>

## <a name="testing-the-movie-delete-process"></a><span data-ttu-id="5684c-198">Tester le processus de suppression de film</span><span class="sxs-lookup"><span data-stu-id="5684c-198">Testing the Movie Delete Process</span></span>

<span data-ttu-id="5684c-199">Vous pouvez maintenant tester.</span><span class="sxs-lookup"><span data-stu-id="5684c-199">Now you can test.</span></span> <span data-ttu-id="5684c-200">Exécutez le *films* page, puis cliquez sur **supprimer** en regard d’un film.</span><span class="sxs-lookup"><span data-stu-id="5684c-200">Run the *Movies* page, and click **Delete** next to a movie.</span></span> <span data-ttu-id="5684c-201">Lorsque le *DeleteMovie* page s’affiche, cliquez sur **supprimer un film**.</span><span class="sxs-lookup"><span data-stu-id="5684c-201">When the *DeleteMovie* page appears, click **Delete Movie**.</span></span>

![Supprimer la page de film avec un bouton Supprimer un film mis en surbrillance](deleting-data/_static/image4.png)

<span data-ttu-id="5684c-203">Lorsque vous cliquez sur le bouton, le code supprime les films et retourne à la liste de films.</span><span class="sxs-lookup"><span data-stu-id="5684c-203">When you click the button, the code deletes the movies and returns to the movie listing.</span></span> <span data-ttu-id="5684c-204">Vous pouvez y rechercher le film supprimé et vérifiez qu’il est supprimé.</span><span class="sxs-lookup"><span data-stu-id="5684c-204">There you can search for the deleted movie and confirm that it's been deleted.</span></span>

## <a name="coming-up-next"></a><span data-ttu-id="5684c-205">Prochaine</span><span class="sxs-lookup"><span data-stu-id="5684c-205">Coming Up Next</span></span>

<span data-ttu-id="5684c-206">Le didacticiel suivant vous montre comment permettre à toutes les pages sur votre site une apparence commune et une mise en page.</span><span class="sxs-lookup"><span data-stu-id="5684c-206">The next tutorial shows you how to give all the pages on your site a common look and layout.</span></span>

## <a name="complete-listing-for-movie-page-updated-with-delete-links"></a><span data-ttu-id="5684c-207">Liste complète pour la Page de film (mis à jour avec les liens de suppression)</span><span class="sxs-lookup"><span data-stu-id="5684c-207">Complete Listing for Movie Page (Updated with Delete Links)</span></span>

[!code-cshtml[Main](deleting-data/samples/sample8.cshtml)]

## <a name="complete-listing-for-deletemovie-page"></a><span data-ttu-id="5684c-208">Liste complète de la Page de DeleteMovie</span><span class="sxs-lookup"><span data-stu-id="5684c-208">Complete Listing for DeleteMovie Page</span></span>

[!code-cshtml[Main](deleting-data/samples/sample9.cshtml)]

## <a name="additional-resources"></a><span data-ttu-id="5684c-209">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="5684c-209">Additional Resources</span></span>

- [<span data-ttu-id="5684c-210">Introduction à la programmation Web ASP.NET à l’aide de la syntaxe Razor</span><span class="sxs-lookup"><span data-stu-id="5684c-210">Introduction to ASP.NET Web Programming by Using the Razor Syntax</span></span>](../introducing-razor-syntax-c.md)
- <span data-ttu-id="5684c-211">[Instruction DELETE de SQL](http://www.w3schools.com/sql/sql_delete.asp) sur le site W3Schools</span><span class="sxs-lookup"><span data-stu-id="5684c-211">[SQL DELETE Statement](http://www.w3schools.com/sql/sql_delete.asp) on the W3Schools site</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="5684c-212">[Précédent](updating-data.md)
> [Suivant](layouts.md)</span><span class="sxs-lookup"><span data-stu-id="5684c-212">[Previous](updating-data.md)
[Next](layouts.md)</span></span>
