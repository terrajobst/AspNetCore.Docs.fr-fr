---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-10
title: "Partie 10 : Les mises à jour finales à la Navigation et la conception de Site, Conclusion | Documents Microsoft"
author: jongalloway
description: "Cette série de didacticiels détaille toutes les mesures prises pour générer l’exemple d’application de magasin de musique ASP.NET MVC. Partie 10 couvre les mises à jour finales à la Navigation et s..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/21/2011
ms.topic: article
ms.assetid: 0c6e4c2f-fcdb-4978-9656-1990c6f15727
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-10
msc.type: authoredcontent
ms.openlocfilehash: 2a65e4b793b615c45cdf31166e0a000ae72ee534
ms.sourcegitcommit: 016f4d58663bcd442930227022de23fb3abee0b3
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/12/2018
---
<a name="part-10-final-updates-to-navigation-and-site-design-conclusion"></a><span data-ttu-id="95832-104">Partie 10 : Les mises à jour finales à la Navigation et la conception de Site, Conclusion</span><span class="sxs-lookup"><span data-stu-id="95832-104">Part 10: Final Updates to Navigation and Site Design, Conclusion</span></span>
====================
<span data-ttu-id="95832-105">par [Jon Galloway](https://github.com/jongalloway)</span><span class="sxs-lookup"><span data-stu-id="95832-105">by [Jon Galloway](https://github.com/jongalloway)</span></span>

> <span data-ttu-id="95832-106">Le magasin de musique MVC est une application du didacticiel qui présente et explique étape par étape comment utiliser ASP.NET MVC et Visual Studio pour le développement web.</span><span class="sxs-lookup"><span data-stu-id="95832-106">The MVC Music Store is a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio for web development.</span></span>  
>   
> <span data-ttu-id="95832-107">Le magasin de musique MVC est une implémentation de magasin exemple léger qui vend des albums de musique en ligne et implémente l’administration de site de base, authentification de l’utilisateur et les fonctionnalités de panier d’achat.</span><span class="sxs-lookup"><span data-stu-id="95832-107">The MVC Music Store is a lightweight sample store implementation which sells music albums online, and implements basic site administration, user sign-in, and shopping cart functionality.</span></span>  
>   
> <span data-ttu-id="95832-108">Cette série de didacticiels détaille toutes les mesures prises pour générer l’exemple d’application de magasin de musique ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="95832-108">This tutorial series details all of the steps taken to build the ASP.NET MVC Music Store sample application.</span></span> <span data-ttu-id="95832-109">Partie 10 couvre les mises à jour finales à la Navigation et la conception de Site, Conclusion.</span><span class="sxs-lookup"><span data-stu-id="95832-109">Part 10 covers Final Updates to Navigation and Site Design, Conclusion.</span></span>


<span data-ttu-id="95832-110">Nous avons terminé toutes les fonctionnalités principales de notre site, mais nous avons toujours certaines fonctionnalités à ajouter à la navigation du site, la page d’accueil et la page Rechercher un magasin.</span><span class="sxs-lookup"><span data-stu-id="95832-110">We've completed all the major functionality for our site, but we still have some features to add to the site navigation, the home page, and the Store Browse page.</span></span>

## <a name="creating-the-shopping-cart-summary-partial-view"></a><span data-ttu-id="95832-111">Création de la vue partielle résumé panier d’achat</span><span class="sxs-lookup"><span data-stu-id="95832-111">Creating the Shopping Cart Summary Partial View</span></span>

<span data-ttu-id="95832-112">Vous souhaitez exposer le nombre d’éléments dans le panier d’achat de l’utilisateur sur l’intégralité du site.</span><span class="sxs-lookup"><span data-stu-id="95832-112">We want to expose the number of items in the user's shopping cart across the entire site.</span></span>

![](mvc-music-store-part-10/_static/image1.png)

<span data-ttu-id="95832-113">Nous pouvons facilement implémenter cela en créant une vue partielle, qui est ajoutée à notre Site.master.</span><span class="sxs-lookup"><span data-stu-id="95832-113">We can easily implement this by creating a partial view which is added to our Site.master.</span></span>

<span data-ttu-id="95832-114">Comme indiqué précédemment, le contrôleur ShoppingCart inclut une méthode d’action CartSummary qui retourne une vue partielle :</span><span class="sxs-lookup"><span data-stu-id="95832-114">As shown previously, the ShoppingCart controller includes a CartSummary action method which returns a partial view:</span></span>

[!code-csharp[Main](mvc-music-store-part-10/samples/sample1.cs)]

<span data-ttu-id="95832-115">Pour créer la vue partielle CartSummary, avec le bouton droit sur le dossier vues/ShoppingCart et sélectionnez Ajouter une vue.</span><span class="sxs-lookup"><span data-stu-id="95832-115">To create the CartSummary partial view, right-click on the Views/ShoppingCart folder and select Add View.</span></span> <span data-ttu-id="95832-116">Nom de la vue CartSummary et vérifiez la case à cocher « Créer une vue partielle » comme indiqué ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="95832-116">Name the view CartSummary and check the "Create a partial view" checkbox as shown below.</span></span>

![](mvc-music-store-part-10/_static/image2.png)

<span data-ttu-id="95832-117">La vue partielle CartSummary est très simple : il s’agit simplement d’un lien vers la vue ShoppingCart Index qui indique le nombre d’éléments dans le panier d’achat.</span><span class="sxs-lookup"><span data-stu-id="95832-117">The CartSummary partial view is really simple - it's just a link to the ShoppingCart Index view which shows the number of items in the cart.</span></span> <span data-ttu-id="95832-118">Le code complet pour CartSummary.cshtml est la suivante :</span><span class="sxs-lookup"><span data-stu-id="95832-118">The complete code for CartSummary.cshtml is as follows:</span></span>

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample2.cshtml)]

<span data-ttu-id="95832-119">Nous pouvons inclure une vue partielle dans n’importe quelle page dans le site, y compris le maître de Site, à l’aide de la méthode Html.RenderAction.</span><span class="sxs-lookup"><span data-stu-id="95832-119">We can include a partial view in any page in the site, including the Site master, by using the Html.RenderAction method.</span></span> <span data-ttu-id="95832-120">RenderAction nous oblige à spécifier le nom de l’Action (« CartSummary ») et le nom du contrôleur (« ShoppingCart ») comme ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="95832-120">RenderAction requires us to specify the Action Name ("CartSummary") and the Controller Name ("ShoppingCart") as below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample3.cshtml)]

<span data-ttu-id="95832-121">Avant d’ajouter cela à la disposition du site, nous allons également créer le Menu de Genre, par conséquent, nous pouvons toutes nos mises à jour Site.master en même temps.</span><span class="sxs-lookup"><span data-stu-id="95832-121">Before adding this to the site Layout, we will also create the Genre Menu so we can make all of our Site.master updates at one time.</span></span>

## <a name="creating-the-genre-menu-partial-view"></a><span data-ttu-id="95832-122">Création de la vue partielle du Menu Genre</span><span class="sxs-lookup"><span data-stu-id="95832-122">Creating the Genre Menu Partial View</span></span>

<span data-ttu-id="95832-123">Nous pouvons effectuer beaucoup plus facile pour nos utilisateurs de naviguer dans le magasin en ajoutant un Menu de Genre qui répertorie tous les Genres disponibles dans notre magasin.</span><span class="sxs-lookup"><span data-stu-id="95832-123">We can make it a lot easier for our users to navigate through the store by adding a Genre Menu which lists all the Genres available in our store.</span></span>

![](mvc-music-store-part-10/_static/image3.png)

<span data-ttu-id="95832-124">Nous allons suivre les mêmes étapes également créent une vue partielle GenreMenu, et nous pouvons ensuite les ajouter au contrôleur de Site.</span><span class="sxs-lookup"><span data-stu-id="95832-124">We will follow the same steps also create a GenreMenu partial view, and then we can add them both to the Site master.</span></span> <span data-ttu-id="95832-125">Tout d’abord, ajoutez l’action de contrôleur GenreMenu suivante à la StoreController :</span><span class="sxs-lookup"><span data-stu-id="95832-125">First, add the following GenreMenu controller action to the StoreController:</span></span>

[!code-csharp[Main](mvc-music-store-part-10/samples/sample4.cs)]

<span data-ttu-id="95832-126">Cette action renvoie une liste de Genres qui sera affiché par la vue partielle, ce qui nous allons créer ensuite.</span><span class="sxs-lookup"><span data-stu-id="95832-126">This action returns a list of Genres which will be displayed by the partial view, which we will create next.</span></span>

<span data-ttu-id="95832-127">*Remarque : Nous avons ajouté l’attribut [ChildActionOnly] à cette action de contrôleur, ce qui indique que nous voulons uniquement cette action pour être utilisée à partir d’une vue partielle. Cet attribut empêche l’action de contrôleur à partir d’en cours d’exécution en accédant à /Store/GenreMenu. Cela n’est pas nécessaire pour les vues partielles, mais il est conseillé, étant donné que nous voulons que nos actions de contrôleur sont utilisées comme nous avons l’intention. Nous sommes également retourner PartialView plutôt que de vue, qui informe le moteur de vue qu’il ne doit pas utiliser la mise en page pour cette vue, car elle est incluse dans les autres modes.*</span><span class="sxs-lookup"><span data-stu-id="95832-127">*Note: We have added the [ChildActionOnly] attribute to this controller action, which indicates that we only want this action to be used from a Partial View. This attribute will prevent the controller action from being executed by browsing to /Store/GenreMenu. This isn't required for partial views, but it is a good practice, since we want to make sure our controller actions are used as we intend. We are also returning PartialView rather than View, which lets the view engine know that it shouldn't use the Layout for this view, as it is being included in other views.*</span></span>

<span data-ttu-id="95832-128">Avec le bouton droit sur l’action du contrôleur GenreMenu et créer une vue partielle nommée GenreMenu fortement typé à l’aide de la classe de données d’affichage Genre comme indiqué ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="95832-128">Right-click on the GenreMenu controller action and create a partial view named GenreMenu which is strongly typed using the Genre view data class as shown below.</span></span>

![](mvc-music-store-part-10/_static/image4.png)

<span data-ttu-id="95832-129">Mettre à jour le code de la vue de la vue partielle de GenreMenu afficher les éléments à l’aide d’une liste non triée comme suit.</span><span class="sxs-lookup"><span data-stu-id="95832-129">Update the view code for the GenreMenu partial view to display the items using an unordered list as follows.</span></span>

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample5.cshtml)]

## <a name="updating-site-layout-to-display-our-partial-views"></a><span data-ttu-id="95832-130">Mise à jour de la mise en page de Site pour afficher ses vues partielles</span><span class="sxs-lookup"><span data-stu-id="95832-130">Updating Site Layout to display our Partial Views</span></span>

<span data-ttu-id="95832-131">Nous pouvons ajouter nos vues partielles à la disposition de Site (/vues/Shared/\_Layout.cshtml) en appelant Html.RenderAction().</span><span class="sxs-lookup"><span data-stu-id="95832-131">We can add our partial views to the Site Layout (/Views/Shared/\_Layout.cshtml) by calling Html.RenderAction().</span></span> <span data-ttu-id="95832-132">Nous allons ajouter tous les deux dans, ainsi que des balises supplémentaires pour les afficher, comme indiqué ci-dessous :</span><span class="sxs-lookup"><span data-stu-id="95832-132">We'll add them both in, as well as some additional markup to display them, as shown below:</span></span>

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample6.cshtml)]

<span data-ttu-id="95832-133">Lorsque nous exécutons l’application, nous allons maintenant voir le Genre dans la zone de navigation gauche et le résumé du panier en haut.</span><span class="sxs-lookup"><span data-stu-id="95832-133">Now when we run the application, we will see the Genre in the left navigation area and the Cart Summary at the top.</span></span>

## <a name="update-to-the-store-browse-page"></a><span data-ttu-id="95832-134">Mettre à jour vers la page Rechercher un magasin</span><span class="sxs-lookup"><span data-stu-id="95832-134">Update to the Store Browse page</span></span>

<span data-ttu-id="95832-135">La page Rechercher un magasin est fonctionnelle, mais ne ressemble pas très bien.</span><span class="sxs-lookup"><span data-stu-id="95832-135">The Store Browse page is functional, but doesn't look very good.</span></span> <span data-ttu-id="95832-136">Nous pouvons mettre à jour de la page pour afficher les albums dans une disposition de mieux en mettant à jour le code de vue (trouvé dans /Views/Store/Browse.cshtml) comme suit :</span><span class="sxs-lookup"><span data-stu-id="95832-136">We can update the page to show the albums in a better layout by updating the view code (found in /Views/Store/Browse.cshtml) as follows:</span></span>

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample7.cshtml)]

<span data-ttu-id="95832-137">Ici, nous effectuons utiliser Url.Action plutôt que Html.ActionLink afin que nous pouvons appliquer une mise en forme spéciale sur le lien pour inclure l’illustration de l’album.</span><span class="sxs-lookup"><span data-stu-id="95832-137">Here we are making use of Url.Action rather than Html.ActionLink so that we can apply special formatting to the link to include the album artwork.</span></span>

<span data-ttu-id="95832-138">*Remarque : Nous affichons un garde album générique pour ces albums. Ces informations sont stockées dans la base de données et peut être modifiées via le Gestionnaire de magasin. Vous pouvez ajouter vos propres graphiques.*</span><span class="sxs-lookup"><span data-stu-id="95832-138">*Note: We are displaying a generic album cover for these albums. This information is stored in the database and is editable via the Store Manager. You are welcome to add your own artwork.*</span></span>

<span data-ttu-id="95832-139">Maintenant lorsque nous accédez à un Genre, nous allons voir les albums affichées dans une grille avec l’illustration de l’album.</span><span class="sxs-lookup"><span data-stu-id="95832-139">Now when we browse to a Genre, we will see the albums shown in a grid with the album artwork.</span></span>

![](mvc-music-store-part-10/_static/image5.png)

## <a name="updating-the-home-page-to-show-top-selling-albums"></a><span data-ttu-id="95832-140">Mise à jour de la Page d’accueil pour afficher des Albums de vente de haut</span><span class="sxs-lookup"><span data-stu-id="95832-140">Updating the Home Page to show Top Selling Albums</span></span>

<span data-ttu-id="95832-141">Nous souhaitons notre albums sur la page d’accueil d’augmenter les ventes de meilleurs.</span><span class="sxs-lookup"><span data-stu-id="95832-141">We want to feature our top selling albums on the home page to increase sales.</span></span> <span data-ttu-id="95832-142">Nous allons effectuer certaines mises à jour notre HomeController pour gérer cela, ajoutez dans certains graphiques supplémentaires également.</span><span class="sxs-lookup"><span data-stu-id="95832-142">We'll make some updates to our HomeController to handle that, and add in some additional graphics as well.</span></span>

<span data-ttu-id="95832-143">Tout d’abord, nous allons ajouter une propriété de navigation à la classe Album afin que EntityFramework sait qu’ils sont associés.</span><span class="sxs-lookup"><span data-stu-id="95832-143">First, we'll add a navigation property to our Album class so that EntityFramework knows that they're associated.</span></span> <span data-ttu-id="95832-144">Les deux dernières lignes de notre **Album** classe doit maintenant ressembler à ceci :</span><span class="sxs-lookup"><span data-stu-id="95832-144">The last few lines of our **Album** class should now look like this:</span></span>

[!code-csharp[Main](mvc-music-store-part-10/samples/sample8.cs)]

<span data-ttu-id="95832-145">*Remarque : Vous devrez ajouter une à l’aide de l’instruction pour placer dans l’espace de noms System.Collections.Generic.*</span><span class="sxs-lookup"><span data-stu-id="95832-145">*Note: This will require adding a using statement to bring in the System.Collections.Generic namespace.*</span></span>

<span data-ttu-id="95832-146">Tout d’abord, nous allons ajouter un champ storeDB et le MvcMusicStore.Models à l’aide des instructions, comme dans nos autres contrôleurs.</span><span class="sxs-lookup"><span data-stu-id="95832-146">First, we'll add a storeDB field and the MvcMusicStore.Models using statements, as in our other controllers.</span></span> <span data-ttu-id="95832-147">Ensuite, nous allons ajouter la méthode suivante pour le fichier HomeController qui interroge notre base de données pour trouver les meilleurs albums ventes en fonction de OrderDetails.</span><span class="sxs-lookup"><span data-stu-id="95832-147">Next, we'll add the following method to the HomeController which queries our database to find top selling albums according to OrderDetails.</span></span>

[!code-csharp[Main](mvc-music-store-part-10/samples/sample9.cs)]

<span data-ttu-id="95832-148">Il s’agit d’une méthode privée, étant donné que nous ne voulons pas rendre disponible en tant qu’une action de contrôleur.</span><span class="sxs-lookup"><span data-stu-id="95832-148">This is a private method, since we don't want to make it available as a controller action.</span></span> <span data-ttu-id="95832-149">Nous allons l’inclure dans le HomeController par souci de simplicité, mais il est recommandé de déplacer votre logique métier dans les classes de service distinct comme il convient.</span><span class="sxs-lookup"><span data-stu-id="95832-149">We are including it in the HomeController for simplicity, but you are encouraged to move your business logic into separate service classes as appropriate.</span></span>

<span data-ttu-id="95832-150">Cela en place, nous pouvons mettre à jour l’action de contrôleur d’Index pour interroger les 5 premières albums de vente et les renvoyer à la vue.</span><span class="sxs-lookup"><span data-stu-id="95832-150">With that in place, we can update the Index controller action to query the top 5 selling albums and return them to the view.</span></span>

[!code-csharp[Main](mvc-music-store-part-10/samples/sample10.cs)]

<span data-ttu-id="95832-151">Le code complet pour le fichier HomeController mis à jour est comme indiqué ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="95832-151">The complete code for the updated HomeController is as shown below.</span></span>

[!code-csharp[Main](mvc-music-store-part-10/samples/sample11.cs)]

<span data-ttu-id="95832-152">Enfin, nous devrons mettre à jour notre vue accueil Index afin qu’il peut afficher une liste d’albums par le type de modèle de mise à jour et l’ajout de la liste des albums vers le bas.</span><span class="sxs-lookup"><span data-stu-id="95832-152">Finally, we'll need to update our Home Index view so that it can display a list of albums by updating the Model type and adding the album list to the bottom.</span></span> <span data-ttu-id="95832-153">Nous allons prendre cette opportunité pour également ajouter un titre et une section de promotion à la page.</span><span class="sxs-lookup"><span data-stu-id="95832-153">We will take this opportunity to also add a heading and a promotion section to the page.</span></span>

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample12.cshtml)]

<span data-ttu-id="95832-154">Maintenant lorsque nous exécutons l’application, nous allons voir notre page d’accueil mises à jour avec les meilleurs albums ventes et notre message promotionnel.</span><span class="sxs-lookup"><span data-stu-id="95832-154">Now when we run the application, we'll see our updated home page with top selling albums and our promotional message.</span></span>

![](mvc-music-store-part-10/_static/image1.jpg)

## <a name="conclusion"></a><span data-ttu-id="95832-155">Conclusion</span><span class="sxs-lookup"><span data-stu-id="95832-155">Conclusion</span></span>

<span data-ttu-id="95832-156">Nous avons vu que ASP.NET MVC facilite la création d’un site Web sophistiqué avec un accès de base de données, l’appartenance, AJAX, etc.</span><span class="sxs-lookup"><span data-stu-id="95832-156">We've seen that ASP.NET MVC makes it easy to create a sophisticated website with database access, membership, AJAX, etc.</span></span> <span data-ttu-id="95832-157">assez rapidement.</span><span class="sxs-lookup"><span data-stu-id="95832-157">pretty quickly.</span></span> <span data-ttu-id="95832-158">Nous espérons que ce didacticiel vous a accordé les outils que vous avez besoin pour commencer à créer votre propre MVC ASP.NET applications !</span><span class="sxs-lookup"><span data-stu-id="95832-158">Hopefully this tutorial has given you the tools you need to get started building your own ASP.NET MVC applications!</span></span>


>[!div class="step-by-step"]
[<span data-ttu-id="95832-159">Précédent</span><span class="sxs-lookup"><span data-stu-id="95832-159">Previous</span></span>](mvc-music-store-part-9.md)
