---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-7
title: 'Partie 7 : Création de la Main Page | Documents Microsoft'
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/04/2012
ms.topic: article
ms.assetid: eb32a17b-626c-4373-9a7d-3387992f3c04
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-7
msc.type: authoredcontent
ms.openlocfilehash: 2c378e68a1e6600daf655c19afbfe355e89496d4
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/06/2018
---
<a name="part-7-creating-the-main-page"></a><span data-ttu-id="03699-102">Partie 7 : Création de la Main Page</span><span class="sxs-lookup"><span data-stu-id="03699-102">Part 7: Creating the Main Page</span></span>
====================
<span data-ttu-id="03699-103">par [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="03699-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="03699-104">Télécharger le projet terminé</span><span class="sxs-lookup"><span data-stu-id="03699-104">Download Completed Project</span></span>](http://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="creating-the-main-page"></a><span data-ttu-id="03699-105">Création de la Main Page</span><span class="sxs-lookup"><span data-stu-id="03699-105">Creating the Main Page</span></span>

<span data-ttu-id="03699-106">Dans cette section, vous allez créer la page principale de l’application.</span><span class="sxs-lookup"><span data-stu-id="03699-106">In this section, you will create the main application page.</span></span> <span data-ttu-id="03699-107">Cette page sera plus complexe que la page d’administration, nous allons l’approche en plusieurs étapes.</span><span class="sxs-lookup"><span data-stu-id="03699-107">This page will be more complex than the Admin page, so we'll approach it in several steps.</span></span> <span data-ttu-id="03699-108">Avant cela, vous verrez certaines techniques de Knockout.js plus avancées.</span><span class="sxs-lookup"><span data-stu-id="03699-108">Along the way, you'll see some more advanced Knockout.js techniques.</span></span> <span data-ttu-id="03699-109">Voici la disposition de la page de base :</span><span class="sxs-lookup"><span data-stu-id="03699-109">Here is the basic layout of the page:</span></span>

![](using-web-api-with-entity-framework-part-7/_static/image1.png)

- <span data-ttu-id="03699-110">« Products » contient un tableau de produits.</span><span class="sxs-lookup"><span data-stu-id="03699-110">"Products" holds an array of products.</span></span>
- <span data-ttu-id="03699-111">« Achat » conserve un tableau de produits et quantités.</span><span class="sxs-lookup"><span data-stu-id="03699-111">"Cart" holds an array of products with quantities.</span></span> <span data-ttu-id="03699-112">En cliquant sur « Add to Cart » met à jour le panier d’achat.</span><span class="sxs-lookup"><span data-stu-id="03699-112">Clicking "Add to Cart" updates the cart.</span></span>
- <span data-ttu-id="03699-113">« Orders » contient un tableau de numéros de commande.</span><span class="sxs-lookup"><span data-stu-id="03699-113">"Orders" holds an array of order IDs.</span></span>
- <span data-ttu-id="03699-114">« Détails » contient un détail de commande, qui est un tableau d’éléments (produits avec des quantités)</span><span class="sxs-lookup"><span data-stu-id="03699-114">"Details" holds an order detail, which is an array of items (products with quantities)</span></span>

<span data-ttu-id="03699-115">Nous allons commencer en définissant une disposition de base en HTML, sans liaison de données ou d’un script.</span><span class="sxs-lookup"><span data-stu-id="03699-115">We'll start by defining some basic layout in HTML, with no data binding or script.</span></span> <span data-ttu-id="03699-116">Ouvrez le fichier Views/Home/Index.cshtml et remplacez tout le contenu avec les éléments suivants :</span><span class="sxs-lookup"><span data-stu-id="03699-116">Open the file Views/Home/Index.cshtml and replace all of the contents with the following:</span></span>

[!code-html[Main](using-web-api-with-entity-framework-part-7/samples/sample1.html)]

<span data-ttu-id="03699-117">Ensuite, ajoutez une section de Scripts et créer un modèle de vue vide :</span><span class="sxs-lookup"><span data-stu-id="03699-117">Next, add a Scripts section and create an empty view-model:</span></span>

[!code-cshtml[Main](using-web-api-with-entity-framework-part-7/samples/sample2.cshtml)]

<span data-ttu-id="03699-118">Selon la conception esquissée précédemment, notre modèle de vue doit observables pour les produits, achat, commandes et détails.</span><span class="sxs-lookup"><span data-stu-id="03699-118">Based on the design sketched earlier, our view model needs observables for products, cart, orders, and details.</span></span> <span data-ttu-id="03699-119">Ajoutez les variables suivantes à la `AppViewModel` objet :</span><span class="sxs-lookup"><span data-stu-id="03699-119">Add the following variables to the `AppViewModel` object:</span></span>

[!code-javascript[Main](using-web-api-with-entity-framework-part-7/samples/sample3.js)]

<span data-ttu-id="03699-120">Les utilisateurs peuvent ajouter des éléments dans la liste de produits dans le panier et supprimer des éléments dans le panier d’achat.</span><span class="sxs-lookup"><span data-stu-id="03699-120">Users can add items from the products list into the cart, and remove items from the cart.</span></span> <span data-ttu-id="03699-121">Pour encapsuler ces fonctions, nous allons créer une autre classe de modèle d’affichage qui représente un produit.</span><span class="sxs-lookup"><span data-stu-id="03699-121">To encapsulate these functions, we'll create another view-model class that represents a product.</span></span> <span data-ttu-id="03699-122">Ajoutez le code suivant à `AppViewModel` :</span><span class="sxs-lookup"><span data-stu-id="03699-122">Add the following code to `AppViewModel`:</span></span>

[!code-javascript[Main](using-web-api-with-entity-framework-part-7/samples/sample4.js?highlight=4)]

<span data-ttu-id="03699-123">Le `ProductViewModel` classe contient deux fonctions qui sont utilisées pour déplacer le produit vers et depuis le panier d’achat : `addItemToCart` ajoute une unité du produit dans le panier d’achat, et `removeAllFromCart` supprime toutes les quantités du produit.</span><span class="sxs-lookup"><span data-stu-id="03699-123">The `ProductViewModel` class contains two functions that are used to move the product to and from the cart: `addItemToCart` adds one unit of the product to the cart, and `removeAllFromCart` removes all quantities of the product.</span></span>

<span data-ttu-id="03699-124">Les utilisateurs peuvent sélectionner une commande existante et obtenir les détails de commande.</span><span class="sxs-lookup"><span data-stu-id="03699-124">Users can select an existing order and get the order details.</span></span> <span data-ttu-id="03699-125">Nous allons encapsuler cette fonctionnalité dans un autre modèle de vue :</span><span class="sxs-lookup"><span data-stu-id="03699-125">We'll encapsulate this functionality into another view-model:</span></span>

[!code-javascript[Main](using-web-api-with-entity-framework-part-7/samples/sample5.js?highlight=4)]

<span data-ttu-id="03699-126">Le `OrderDetailsViewModel` est initialisé avec une commande, et il extrait les détails de commande en envoyant une requête AJAX au serveur.</span><span class="sxs-lookup"><span data-stu-id="03699-126">The `OrderDetailsViewModel` is initialized with an order, and it fetches the order details by sending an AJAX request to the server.</span></span>

<span data-ttu-id="03699-127">En outre, notez le `total` propriété sur le `OrderDetailsViewModel`.</span><span class="sxs-lookup"><span data-stu-id="03699-127">Also, notice the `total` property on the `OrderDetailsViewModel`.</span></span> <span data-ttu-id="03699-128">Cette propriété est un type spécial d’observable appelé un [calculée observable](http://knockoutjs.com/documentation/computedObservables.html).</span><span class="sxs-lookup"><span data-stu-id="03699-128">This property is a special kind of observable called a [computed observable](http://knockoutjs.com/documentation/computedObservables.html).</span></span> <span data-ttu-id="03699-129">Comme son nom l’indique, un observable calculée vous permet de lier les données à une valeur calculée&#8212;dans ce cas, le coût total de la commande.</span><span class="sxs-lookup"><span data-stu-id="03699-129">As the name implies, a computed observable lets you data bind to a computed value&#8212;in this case, the total cost of the order.</span></span>

<span data-ttu-id="03699-130">Ensuite, ajoutez ces fonctions à `AppViewModel`:</span><span class="sxs-lookup"><span data-stu-id="03699-130">Next, add these functions to `AppViewModel`:</span></span>

- <span data-ttu-id="03699-131">`resetCart` Supprime tous les éléments du panier.</span><span class="sxs-lookup"><span data-stu-id="03699-131">`resetCart` removes all items from the cart.</span></span>
- <span data-ttu-id="03699-132">`getDetails` Obtient les détails d’une commande (par pusing un nouveau `OrderDetailsViewModel` sur la `details` liste).</span><span class="sxs-lookup"><span data-stu-id="03699-132">`getDetails` gets the details for an order (by pusing a new `OrderDetailsViewModel` onto the `details` list).</span></span>
- <span data-ttu-id="03699-133">`createOrder` Crée une nouvelle commande et vide le panier d’achat.</span><span class="sxs-lookup"><span data-stu-id="03699-133">`createOrder` creates a new order and empties the cart.</span></span>


[!code-javascript[Main](using-web-api-with-entity-framework-part-7/samples/sample6.js?highlight=4)]

<span data-ttu-id="03699-134">Enfin, initialiser le modèle d’affichage en effectuant les requêtes AJAX pour les produits et les commandes :</span><span class="sxs-lookup"><span data-stu-id="03699-134">Finally, initialize the view model by making AJAX requests for the products and orders:</span></span>

[!code-javascript[Main](using-web-api-with-entity-framework-part-7/samples/sample7.js)]

<span data-ttu-id="03699-135">OK, cela représente beaucoup de code, mais nous l’avons réalisé des pas à pas, et j’espère que la conception est désactivée.</span><span class="sxs-lookup"><span data-stu-id="03699-135">OK, that's a lot of code, but we built it up step-by-step, so hopefully the design is clear.</span></span> <span data-ttu-id="03699-136">Maintenant, nous pouvons ajouter certaines liaisons Knockout.js au code HTML.</span><span class="sxs-lookup"><span data-stu-id="03699-136">Now we can add some Knockout.js bindings to the HTML.</span></span>

<span data-ttu-id="03699-137">**Produits**</span><span class="sxs-lookup"><span data-stu-id="03699-137">**Products**</span></span>

<span data-ttu-id="03699-138">Voici les liaisons pour la liste de produits :</span><span class="sxs-lookup"><span data-stu-id="03699-138">Here are the bindings for the product list:</span></span>

[!code-html[Main](using-web-api-with-entity-framework-part-7/samples/sample8.html)]

<span data-ttu-id="03699-139">Il effectue une itération sur le tableau de produits et affiche le nom et le prix.</span><span class="sxs-lookup"><span data-stu-id="03699-139">This iterates over the products array and displays the name and price.</span></span> <span data-ttu-id="03699-140">Le bouton « Ajouter à la commande » est visible uniquement lorsque l’utilisateur est connecté.</span><span class="sxs-lookup"><span data-stu-id="03699-140">The "Add to Order" button is visible only when the user is logged in.</span></span>

<span data-ttu-id="03699-141">Les appels de bouton « Ajouter à la commande » `addItemToCart` sur la `ProductViewModel` instance pour le produit.</span><span class="sxs-lookup"><span data-stu-id="03699-141">The "Add to Order" button calls `addItemToCart` on the `ProductViewModel` instance for the product.</span></span> <span data-ttu-id="03699-142">Cela montre une fonctionnalité intéressante de Knockout.js : lorsqu’un modèle d’affichage contient des autres modèles de vue, vous pouvez appliquer ces liaisons au modèle interne.</span><span class="sxs-lookup"><span data-stu-id="03699-142">This demonstrates a nice feature of Knockout.js: When a view-model contains other view-models, you can apply the bindings to the inner model.</span></span> <span data-ttu-id="03699-143">Dans cet exemple, les liaisons dans le `foreach` sont appliquées à chaque le `ProductViewModel` instances.</span><span class="sxs-lookup"><span data-stu-id="03699-143">In this example, the bindings within the `foreach` are applied to each of the `ProductViewModel` instances.</span></span> <span data-ttu-id="03699-144">Cette approche est beaucoup plus claire que le placement de toutes les fonctionnalités dans un modèle d’affichage unique.</span><span class="sxs-lookup"><span data-stu-id="03699-144">This approach is much cleaner than putting all of the functionality into a single view-model.</span></span>

<span data-ttu-id="03699-145">**Cart**</span><span class="sxs-lookup"><span data-stu-id="03699-145">**Cart**</span></span>

<span data-ttu-id="03699-146">Voici les liaisons pour le panier d’achat :</span><span class="sxs-lookup"><span data-stu-id="03699-146">Here are the bindings for the cart:</span></span>

[!code-html[Main](using-web-api-with-entity-framework-part-7/samples/sample9.html)]

<span data-ttu-id="03699-147">Il effectue une itération sur le tableau de panier et affiche le nom, le prix et la quantité.</span><span class="sxs-lookup"><span data-stu-id="03699-147">This iterates over the cart array and displays the name, price, and quantity.</span></span> <span data-ttu-id="03699-148">Notez que le lien « Supprimer » et le bouton « Créer une commande » sont liées à des fonctions de modèle d’affichage.</span><span class="sxs-lookup"><span data-stu-id="03699-148">Note that the "Remove" link and the "Create Order" button are bound to view-model functions.</span></span>

<span data-ttu-id="03699-149">**Orders**</span><span class="sxs-lookup"><span data-stu-id="03699-149">**Orders**</span></span>

<span data-ttu-id="03699-150">Voici les liaisons pour la liste des commandes :</span><span class="sxs-lookup"><span data-stu-id="03699-150">Here are the bindings for the orders list:</span></span>

[!code-html[Main](using-web-api-with-entity-framework-part-7/samples/sample10.html)]

<span data-ttu-id="03699-151">Il effectue une itération sur les commandes et affiche l’ID de commande.</span><span class="sxs-lookup"><span data-stu-id="03699-151">This iterates over the orders and shows the order ID.</span></span> <span data-ttu-id="03699-152">L’événement de clic sur le lien est lié à la `getDetails` (fonction).</span><span class="sxs-lookup"><span data-stu-id="03699-152">The click event on the link is bound to the `getDetails` function.</span></span>

<span data-ttu-id="03699-153">**Détails de la commande**</span><span class="sxs-lookup"><span data-stu-id="03699-153">**Order Details**</span></span>

<span data-ttu-id="03699-154">Voici les liaisons pour les détails de commande :</span><span class="sxs-lookup"><span data-stu-id="03699-154">Here are the bindings for the order details:</span></span>

[!code-html[Main](using-web-api-with-entity-framework-part-7/samples/sample11.html)]

<span data-ttu-id="03699-155">Il effectue une itération sur les éléments dans l’ordre et affiche les produits, prix et quantité.</span><span class="sxs-lookup"><span data-stu-id="03699-155">This iterates over the items in the order and displays the product, price, and quanity.</span></span> <span data-ttu-id="03699-156">L’élément div environnant est visible uniquement si le tableau de détails contient un ou plusieurs éléments.</span><span class="sxs-lookup"><span data-stu-id="03699-156">The surrounding div is visible only if the details array contains one or more items.</span></span>

## <a name="conclusion"></a><span data-ttu-id="03699-157">Conclusion</span><span class="sxs-lookup"><span data-stu-id="03699-157">Conclusion</span></span>

<span data-ttu-id="03699-158">Dans ce didacticiel, vous avez créé une application qui utilise Entity Framework pour communiquer avec la base de données et les API de Web ASP.NET pour fournir une interface publique sur la couche données.</span><span class="sxs-lookup"><span data-stu-id="03699-158">In this tutorial, you created an application that uses Entity Framework to communicate with the database, and ASP.NET Web API to provide a public-facing interface on top of the data layer.</span></span> <span data-ttu-id="03699-159">ASP.NET MVC 4 nous permettent de restituer les pages HTML et Knockout.js plus jQuery pour fournir des interactions dynamiques sans rechargements de la page.</span><span class="sxs-lookup"><span data-stu-id="03699-159">We use ASP.NET MVC 4 to render the HTML pages, and Knockout.js plus jQuery to provide dynamic interactions without page reloads.</span></span>

<span data-ttu-id="03699-160">Ressources supplémentaires :</span><span class="sxs-lookup"><span data-stu-id="03699-160">Additional resources:</span></span>

- [<span data-ttu-id="03699-161">ASP.NET Data Access Content Map</span><span class="sxs-lookup"><span data-stu-id="03699-161">ASP.NET Data Access Content Map</span></span>](https://msdn.microsoft.com/library/6759sth4.aspx)
- [<span data-ttu-id="03699-162">Centre de développement Entity Framework</span><span class="sxs-lookup"><span data-stu-id="03699-162">Entity Framework Developer Center</span></span>](https://msdn.microsoft.com/data/ef)

> [!div class="step-by-step"]
> [<span data-ttu-id="03699-163">Précédent</span><span class="sxs-lookup"><span data-stu-id="03699-163">Previous</span></span>](using-web-api-with-entity-framework-part-6.md)
