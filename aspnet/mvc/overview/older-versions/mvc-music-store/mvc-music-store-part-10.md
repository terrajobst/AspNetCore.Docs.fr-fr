---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-10
title: 'Partie 10 : Mises à jour finales Navigation et de conception de Site, Conclusion | Microsoft Docs'
author: jongalloway
description: Cette série de didacticiels décrit en détail les étapes prises pour générer l’exemple d’application ASP.NET MVC Music Store. Partie 10 couvre les mises à jour finales de Navigation et S....
ms.author: riande
ms.date: 04/21/2011
ms.assetid: 0c6e4c2f-fcdb-4978-9656-1990c6f15727
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-10
msc.type: authoredcontent
ms.openlocfilehash: f32509701dd112053aa4f31d6552601f961c7413
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41824498"
---
<a name="part-10-final-updates-to-navigation-and-site-design-conclusion"></a><span data-ttu-id="f089a-104">Partie 10 : Mises à jour finales Navigation et de conception de Site, Conclusion</span><span class="sxs-lookup"><span data-stu-id="f089a-104">Part 10: Final Updates to Navigation and Site Design, Conclusion</span></span>
====================
<span data-ttu-id="f089a-105">par [Jon Galloway](https://github.com/jongalloway)</span><span class="sxs-lookup"><span data-stu-id="f089a-105">by [Jon Galloway](https://github.com/jongalloway)</span></span>

> <span data-ttu-id="f089a-106">Le Store de musique MVC est une application du didacticiel qui présente et explique étape par étape comment utiliser ASP.NET MVC et Visual Studio pour le développement web.</span><span class="sxs-lookup"><span data-stu-id="f089a-106">The MVC Music Store is a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio for web development.</span></span>  
>   
> <span data-ttu-id="f089a-107">Le Store de musique MVC est une implémentation de magasin d’exemple léger qui vend des albums de musique en ligne et implémente l’administration de site de base, connexion de l’utilisateur et les fonctionnalités de panier d’achat.</span><span class="sxs-lookup"><span data-stu-id="f089a-107">The MVC Music Store is a lightweight sample store implementation which sells music albums online, and implements basic site administration, user sign-in, and shopping cart functionality.</span></span>  
>   
> <span data-ttu-id="f089a-108">Cette série de didacticiels décrit en détail les étapes prises pour générer l’exemple d’application ASP.NET MVC Music Store.</span><span class="sxs-lookup"><span data-stu-id="f089a-108">This tutorial series details all of the steps taken to build the ASP.NET MVC Music Store sample application.</span></span> <span data-ttu-id="f089a-109">Partie 10 couvre les mises à jour finales de Navigation et de conception de Site, Conclusion.</span><span class="sxs-lookup"><span data-stu-id="f089a-109">Part 10 covers Final Updates to Navigation and Site Design, Conclusion.</span></span>


<span data-ttu-id="f089a-110">Nous avons terminé toutes les fonctionnalités principales pour notre site, mais nous avons toujours certaines fonctionnalités à ajouter à la navigation du site, la page d’accueil et la page Rechercher un Store.</span><span class="sxs-lookup"><span data-stu-id="f089a-110">We've completed all the major functionality for our site, but we still have some features to add to the site navigation, the home page, and the Store Browse page.</span></span>

## <a name="creating-the-shopping-cart-summary-partial-view"></a><span data-ttu-id="f089a-111">Création de la vue partielle résumé panier d’achat</span><span class="sxs-lookup"><span data-stu-id="f089a-111">Creating the Shopping Cart Summary Partial View</span></span>

<span data-ttu-id="f089a-112">Nous devions présenter le nombre d’éléments dans le panier d’achat de l’utilisateur sur l’ensemble du site.</span><span class="sxs-lookup"><span data-stu-id="f089a-112">We want to expose the number of items in the user's shopping cart across the entire site.</span></span>

![](mvc-music-store-part-10/_static/image1.png)

<span data-ttu-id="f089a-113">Nous pouvons facilement implémenter ceci en créant une vue partielle qui est ajoutée à notre Site.master.</span><span class="sxs-lookup"><span data-stu-id="f089a-113">We can easily implement this by creating a partial view which is added to our Site.master.</span></span>

<span data-ttu-id="f089a-114">Comme indiqué précédemment, le contrôleur ShoppingCart inclut une méthode d’action CartSummary qui retourne une vue partielle :</span><span class="sxs-lookup"><span data-stu-id="f089a-114">As shown previously, the ShoppingCart controller includes a CartSummary action method which returns a partial view:</span></span>

[!code-csharp[Main](mvc-music-store-part-10/samples/sample1.cs)]

<span data-ttu-id="f089a-115">Pour créer la vue partielle CartSummary, avec le bouton droit sur le dossier vues/ShoppingCart et sélectionnez Ajouter une vue.</span><span class="sxs-lookup"><span data-stu-id="f089a-115">To create the CartSummary partial view, right-click on the Views/ShoppingCart folder and select Add View.</span></span> <span data-ttu-id="f089a-116">Nom de la vue CartSummary et cochez la case à cocher « Créer une vue partielle » comme indiqué ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="f089a-116">Name the view CartSummary and check the "Create a partial view" checkbox as shown below.</span></span>

![](mvc-music-store-part-10/_static/image2.png)

<span data-ttu-id="f089a-117">La vue partielle CartSummary est très simple : il est simplement un lien vers la vue de ShoppingCart Index qui indique le nombre d’éléments dans le panier d’achat.</span><span class="sxs-lookup"><span data-stu-id="f089a-117">The CartSummary partial view is really simple - it's just a link to the ShoppingCart Index view which shows the number of items in the cart.</span></span> <span data-ttu-id="f089a-118">Le code complet pour CartSummary.cshtml est comme suit :</span><span class="sxs-lookup"><span data-stu-id="f089a-118">The complete code for CartSummary.cshtml is as follows:</span></span>

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample2.cshtml)]

<span data-ttu-id="f089a-119">Nous pouvons inclure une vue partielle dans n’importe quelle page dans le site, y compris le maître du Site, à l’aide de la méthode Html.RenderAction.</span><span class="sxs-lookup"><span data-stu-id="f089a-119">We can include a partial view in any page in the site, including the Site master, by using the Html.RenderAction method.</span></span> <span data-ttu-id="f089a-120">RenderAction nous oblige à spécifier le nom d’Action (« CartSummary ») et le nom du contrôleur (« ShoppingCart ») comme ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="f089a-120">RenderAction requires us to specify the Action Name ("CartSummary") and the Controller Name ("ShoppingCart") as below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample3.cshtml)]

<span data-ttu-id="f089a-121">Avant d’ajouter cela à la disposition du site, nous créerons également le Menu de Genre afin de pouvoir effectuer toutes nos mises à jour Site.master en même temps.</span><span class="sxs-lookup"><span data-stu-id="f089a-121">Before adding this to the site Layout, we will also create the Genre Menu so we can make all of our Site.master updates at one time.</span></span>

## <a name="creating-the-genre-menu-partial-view"></a><span data-ttu-id="f089a-122">Création de la vue partielle du Menu Genre</span><span class="sxs-lookup"><span data-stu-id="f089a-122">Creating the Genre Menu Partial View</span></span>

<span data-ttu-id="f089a-123">Nous pouvons faire beaucoup plus facile pour nos utilisateurs de naviguer dans le magasin en ajoutant un Menu de Genre qui répertorie tous les Genres disponibles dans notre magasin.</span><span class="sxs-lookup"><span data-stu-id="f089a-123">We can make it a lot easier for our users to navigate through the store by adding a Genre Menu which lists all the Genres available in our store.</span></span>

![](mvc-music-store-part-10/_static/image3.png)

<span data-ttu-id="f089a-124">Nous allons suivre les mêmes étapes permettent également de créer une vue partielle GenreMenu, et puis nous pouvons ajouter les deux pour le Site maître.</span><span class="sxs-lookup"><span data-stu-id="f089a-124">We will follow the same steps also create a GenreMenu partial view, and then we can add them both to the Site master.</span></span> <span data-ttu-id="f089a-125">Tout d’abord, ajoutez l’action de contrôleur GenreMenu suivante à la StoreController :</span><span class="sxs-lookup"><span data-stu-id="f089a-125">First, add the following GenreMenu controller action to the StoreController:</span></span>

[!code-csharp[Main](mvc-music-store-part-10/samples/sample4.cs)]

<span data-ttu-id="f089a-126">Cette action renvoie une liste de Genres qui sera affiché par la vue partielle, nous allons créer ensuite.</span><span class="sxs-lookup"><span data-stu-id="f089a-126">This action returns a list of Genres which will be displayed by the partial view, which we will create next.</span></span>

<span data-ttu-id="f089a-127">*Remarque : Nous avons ajouté l’attribut [ChildActionOnly] à cette action de contrôleur, ce qui indique que nous voulons uniquement cette action pour être utilisée à partir d’une vue partielle. Cet attribut empêche l’action du contrôleur à partir d’en cours d’exécution en accédant à /Store/GenreMenu. Cela n’est pas nécessaire pour les vues partielles, mais il est judicieux, car nous voulons s’assurer que nos actions de contrôleur sont utilisées comme nous avons l’intention. Nous avons également retourner PartialView plutôt que vue, qui informe le moteur d’affichage qu’il ne doit pas utiliser la mise en page pour cette vue, comme il est inclus dans les autres modes.*</span><span class="sxs-lookup"><span data-stu-id="f089a-127">*Note: We have added the [ChildActionOnly] attribute to this controller action, which indicates that we only want this action to be used from a Partial View. This attribute will prevent the controller action from being executed by browsing to /Store/GenreMenu. This isn't required for partial views, but it is a good practice, since we want to make sure our controller actions are used as we intend. We are also returning PartialView rather than View, which lets the view engine know that it shouldn't use the Layout for this view, as it is being included in other views.*</span></span>

<span data-ttu-id="f089a-128">Avec le bouton droit sur l’action du contrôleur GenreMenu et créer une vue partielle nommée GenreMenu qui est fortement typée à l’aide de la classe de données d’affichage Genre comme indiqué ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="f089a-128">Right-click on the GenreMenu controller action and create a partial view named GenreMenu which is strongly typed using the Genre view data class as shown below.</span></span>

![](mvc-music-store-part-10/_static/image4.png)

<span data-ttu-id="f089a-129">Mettre à jour le code de vue de la vue partielle GenreMenu afficher les éléments à l’aide d’une liste non triée comme suit.</span><span class="sxs-lookup"><span data-stu-id="f089a-129">Update the view code for the GenreMenu partial view to display the items using an unordered list as follows.</span></span>

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample5.cshtml)]

## <a name="updating-site-layout-to-display-our-partial-views"></a><span data-ttu-id="f089a-130">La mise à jour de disposition de Site pour afficher les vues partielles</span><span class="sxs-lookup"><span data-stu-id="f089a-130">Updating Site Layout to display our Partial Views</span></span>

<span data-ttu-id="f089a-131">Nous pouvons ajouter notre vues partielles à la disposition du Site (/vues/Shared/\_Layout.cshtml) en appelant Html.RenderAction().</span><span class="sxs-lookup"><span data-stu-id="f089a-131">We can add our partial views to the Site Layout (/Views/Shared/\_Layout.cshtml) by calling Html.RenderAction().</span></span> <span data-ttu-id="f089a-132">Nous allons ajouter les deux dans, ainsi que des balises supplémentaires pour les afficher, comme indiqué ci-dessous :</span><span class="sxs-lookup"><span data-stu-id="f089a-132">We'll add them both in, as well as some additional markup to display them, as shown below:</span></span>

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample6.cshtml)]

<span data-ttu-id="f089a-133">Lorsque nous exécutons l’application, nous allons maintenant voir le Genre dans la zone de navigation gauche et le résumé du panier en haut.</span><span class="sxs-lookup"><span data-stu-id="f089a-133">Now when we run the application, we will see the Genre in the left navigation area and the Cart Summary at the top.</span></span>

## <a name="update-to-the-store-browse-page"></a><span data-ttu-id="f089a-134">Mettre à jour vers la page Rechercher un Store</span><span class="sxs-lookup"><span data-stu-id="f089a-134">Update to the Store Browse page</span></span>

<span data-ttu-id="f089a-135">La page Rechercher un Store est fonctionnelle, mais ne semble pas très bien.</span><span class="sxs-lookup"><span data-stu-id="f089a-135">The Store Browse page is functional, but doesn't look very good.</span></span> <span data-ttu-id="f089a-136">Nous pouvons mettre à jour la page pour afficher des albums dans une meilleure mise en page en mettant à jour le code de vue (trouvé dans /Views/Store/Browse.cshtml) comme suit :</span><span class="sxs-lookup"><span data-stu-id="f089a-136">We can update the page to show the albums in a better layout by updating the view code (found in /Views/Store/Browse.cshtml) as follows:</span></span>

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample7.cshtml)]

<span data-ttu-id="f089a-137">Ici, nous effectuons utiliser Url.Action plutôt que Html.ActionLink afin que nous pouvons appliquer la mise en forme spéciale pour le lien pour inclure l’illustration de l’album.</span><span class="sxs-lookup"><span data-stu-id="f089a-137">Here we are making use of Url.Action rather than Html.ActionLink so that we can apply special formatting to the link to include the album artwork.</span></span>

<span data-ttu-id="f089a-138">*Remarque : Nous affichons une couverture d’album générique pour ces albums. Ces informations sont stockées dans la base de données et peut être modifiées via le Gestionnaire de Store. Vous pouvez ajouter votre propre signature graphique.*</span><span class="sxs-lookup"><span data-stu-id="f089a-138">*Note: We are displaying a generic album cover for these albums. This information is stored in the database and is editable via the Store Manager. You are welcome to add your own artwork.*</span></span>

<span data-ttu-id="f089a-139">Maintenant, lorsque nous accédez à un Genre, nous verrons albums affichées dans une grille avec l’illustration de l’album.</span><span class="sxs-lookup"><span data-stu-id="f089a-139">Now when we browse to a Genre, we will see the albums shown in a grid with the album artwork.</span></span>

![](mvc-music-store-part-10/_static/image5.png)

## <a name="updating-the-home-page-to-show-top-selling-albums"></a><span data-ttu-id="f089a-140">La mise à jour de la Page d’accueil pour afficher des Albums de vente de haut</span><span class="sxs-lookup"><span data-stu-id="f089a-140">Updating the Home Page to show Top Selling Albums</span></span>

<span data-ttu-id="f089a-141">Nous souhaitons nos meilleures ventes albums sur la page d’accueil pour augmenter les ventes de fonctionnalités.</span><span class="sxs-lookup"><span data-stu-id="f089a-141">We want to feature our top selling albums on the home page to increase sales.</span></span> <span data-ttu-id="f089a-142">Nous allons effectuer certaines mises à jour notre HomeController pour gérer cela, ajoutez dans certains graphiques supplémentaires également.</span><span class="sxs-lookup"><span data-stu-id="f089a-142">We'll make some updates to our HomeController to handle that, and add in some additional graphics as well.</span></span>

<span data-ttu-id="f089a-143">Tout d’abord, nous allons ajouter une propriété de navigation à notre classe Album afin que EntityFramework sache qu’elles sont associées.</span><span class="sxs-lookup"><span data-stu-id="f089a-143">First, we'll add a navigation property to our Album class so that EntityFramework knows that they're associated.</span></span> <span data-ttu-id="f089a-144">Les dernières lignes de notre **Album** classe doit maintenant ressembler à ceci :</span><span class="sxs-lookup"><span data-stu-id="f089a-144">The last few lines of our **Album** class should now look like this:</span></span>

[!code-csharp[Main](mvc-music-store-part-10/samples/sample8.cs)]

<span data-ttu-id="f089a-145">*Remarque : Cela nécessite l’ajout d’un à l’aide de l’instruction à afficher dans l’espace de noms System.Collections.Generic.*</span><span class="sxs-lookup"><span data-stu-id="f089a-145">*Note: This will require adding a using statement to bring in the System.Collections.Generic namespace.*</span></span>

<span data-ttu-id="f089a-146">Tout d’abord, nous allons ajouter un champ storeDB et le MvcMusicStore.Models à l’aide des instructions, comme dans nos autres contrôleurs.</span><span class="sxs-lookup"><span data-stu-id="f089a-146">First, we'll add a storeDB field and the MvcMusicStore.Models using statements, as in our other controllers.</span></span> <span data-ttu-id="f089a-147">Ensuite, nous allons ajouter la méthode suivante au HomeController qui interroge notre base de données pour trouver les meilleurs albums de ventes en fonction de OrderDetails.</span><span class="sxs-lookup"><span data-stu-id="f089a-147">Next, we'll add the following method to the HomeController which queries our database to find top selling albums according to OrderDetails.</span></span>

[!code-csharp[Main](mvc-music-store-part-10/samples/sample9.cs)]

<span data-ttu-id="f089a-148">Il s’agit d’une méthode privée, étant donné que nous ne voulons pas pour le rendre disponible en tant qu’une action de contrôleur.</span><span class="sxs-lookup"><span data-stu-id="f089a-148">This is a private method, since we don't want to make it available as a controller action.</span></span> <span data-ttu-id="f089a-149">Nous allons l’inclure dans le HomeController par souci de simplicité, mais il est conseillé de déplacer votre logique métier dans des classes de service distinct comme il convient.</span><span class="sxs-lookup"><span data-stu-id="f089a-149">We are including it in the HomeController for simplicity, but you are encouraged to move your business logic into separate service classes as appropriate.</span></span>

<span data-ttu-id="f089a-150">Ceci en place, nous pouvons mettre à jour l’action du contrôleur pour interroger les 5 vendant des albums et les retourner à la vue Index.</span><span class="sxs-lookup"><span data-stu-id="f089a-150">With that in place, we can update the Index controller action to query the top 5 selling albums and return them to the view.</span></span>

[!code-csharp[Main](mvc-music-store-part-10/samples/sample10.cs)]

<span data-ttu-id="f089a-151">Le code complet pour le HomeController mis à jour est comme indiqué ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="f089a-151">The complete code for the updated HomeController is as shown below.</span></span>

[!code-csharp[Main](mvc-music-store-part-10/samples/sample11.cs)]

<span data-ttu-id="f089a-152">Enfin, nous allons devoir mettre à jour notre vue d’accueil d’Index pour qu’il puisse afficher une liste d’albums par la mise à jour le type de modèle et l’ajout de la liste des albums vers le bas.</span><span class="sxs-lookup"><span data-stu-id="f089a-152">Finally, we'll need to update our Home Index view so that it can display a list of albums by updating the Model type and adding the album list to the bottom.</span></span> <span data-ttu-id="f089a-153">Nous allons cette occasion pour également ajouter un titre et une section de promotion à la page.</span><span class="sxs-lookup"><span data-stu-id="f089a-153">We will take this opportunity to also add a heading and a promotion section to the page.</span></span>

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample12.cshtml)]

<span data-ttu-id="f089a-154">Maintenant, lorsque nous exécutons l’application, nous verrons notre page d’accueil mises à jour avec les meilleurs albums de ventes et nos messages promotionnels.</span><span class="sxs-lookup"><span data-stu-id="f089a-154">Now when we run the application, we'll see our updated home page with top selling albums and our promotional message.</span></span>

![](mvc-music-store-part-10/_static/image1.jpg)

## <a name="conclusion"></a><span data-ttu-id="f089a-155">Conclusion</span><span class="sxs-lookup"><span data-stu-id="f089a-155">Conclusion</span></span>

<span data-ttu-id="f089a-156">Nous avons vu que ASP.NET MVC facilite la création d’un site Web sophistiqué avec un accès de base de données, l’appartenance, AJAX, etc.</span><span class="sxs-lookup"><span data-stu-id="f089a-156">We've seen that ASP.NET MVC makes it easy to create a sophisticated website with database access, membership, AJAX, etc.</span></span> <span data-ttu-id="f089a-157">assez rapidement.</span><span class="sxs-lookup"><span data-stu-id="f089a-157">pretty quickly.</span></span> <span data-ttu-id="f089a-158">J’espère que ce didacticiel vous a donné les outils que vous avez besoin pour commencer à créer votre propre MVC ASP.NET applications !</span><span class="sxs-lookup"><span data-stu-id="f089a-158">Hopefully this tutorial has given you the tools you need to get started building your own ASP.NET MVC applications!</span></span>


> [!div class="step-by-step"]
> [<span data-ttu-id="f089a-159">Précédent</span><span class="sxs-lookup"><span data-stu-id="f089a-159">Previous</span></span>](mvc-music-store-part-9.md)
