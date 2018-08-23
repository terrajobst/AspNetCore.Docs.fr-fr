---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-8
title: 'Partie 8 : Panier d’achat avec des mises à jour Ajax | Microsoft Docs'
author: jongalloway
description: Cette série de didacticiels décrit en détail les étapes prises pour générer l’exemple d’application ASP.NET MVC Music Store. Partie 8 couvre le panier d’achat avec des mises à jour Ajax.
ms.author: riande
ms.date: 04/21/2011
ms.assetid: 26b2f55e-ed42-4277-89b0-c941eb754145
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-8
msc.type: authoredcontent
ms.openlocfilehash: cab338e56505c453532a26d794eb7bf4e94555a9
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41831776"
---
<a name="part-8-shopping-cart-with-ajax-updates"></a><span data-ttu-id="70a0d-104">Partie 8 : Panier d’achat avec des mises à jour Ajax</span><span class="sxs-lookup"><span data-stu-id="70a0d-104">Part 8: Shopping Cart with Ajax Updates</span></span>
====================
<span data-ttu-id="70a0d-105">par [Jon Galloway](https://github.com/jongalloway)</span><span class="sxs-lookup"><span data-stu-id="70a0d-105">by [Jon Galloway](https://github.com/jongalloway)</span></span>

> <span data-ttu-id="70a0d-106">Le Store de musique MVC est une application du didacticiel qui présente et explique étape par étape comment utiliser ASP.NET MVC et Visual Studio pour le développement web.</span><span class="sxs-lookup"><span data-stu-id="70a0d-106">The MVC Music Store is a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio for web development.</span></span>  
>   
> <span data-ttu-id="70a0d-107">Le Store de musique MVC est une implémentation de magasin d’exemple léger qui vend des albums de musique en ligne et implémente l’administration de site de base, connexion de l’utilisateur et les fonctionnalités de panier d’achat.</span><span class="sxs-lookup"><span data-stu-id="70a0d-107">The MVC Music Store is a lightweight sample store implementation which sells music albums online, and implements basic site administration, user sign-in, and shopping cart functionality.</span></span>  
>   
> <span data-ttu-id="70a0d-108">Cette série de didacticiels décrit en détail les étapes prises pour générer l’exemple d’application ASP.NET MVC Music Store.</span><span class="sxs-lookup"><span data-stu-id="70a0d-108">This tutorial series details all of the steps taken to build the ASP.NET MVC Music Store sample application.</span></span> <span data-ttu-id="70a0d-109">Partie 8 couvre le panier d’achat avec des mises à jour Ajax.</span><span class="sxs-lookup"><span data-stu-id="70a0d-109">Part 8 covers Shopping Cart with Ajax Updates.</span></span>


<span data-ttu-id="70a0d-110">Nous allons permettre aux utilisateurs de placer des albums dans leur panier d’achat sans l’enregistrer, mais ils doivent s’inscrire en tant qu’invités à l’extraction terminée.</span><span class="sxs-lookup"><span data-stu-id="70a0d-110">We'll allow users to place albums in their cart without registering, but they'll need to register as guests to complete checkout.</span></span> <span data-ttu-id="70a0d-111">Le processus d’achat et l’extraction est divisée en deux contrôleurs : un contrôleur ShoppingCart qui permet d’ajouter des éléments anonymement à un panier d’achat et un contrôleur d’extraction qui gère le processus de validation.</span><span class="sxs-lookup"><span data-stu-id="70a0d-111">The shopping and checkout process will be separated into two controllers: a ShoppingCart Controller which allows anonymously adding items to a cart, and a Checkout Controller which handles the checkout process.</span></span> <span data-ttu-id="70a0d-112">Nous allons commencer par le panier d’achat dans cette section, puis générez le processus de validation dans la section suivante.</span><span class="sxs-lookup"><span data-stu-id="70a0d-112">We'll start with the Shopping Cart in this section, then build the Checkout process in the following section.</span></span>

## <a name="adding-the-cart-order-and-orderdetail-model-classes"></a><span data-ttu-id="70a0d-113">Ajout de classes de modèle de panier, l’ordre et OrderDetail</span><span class="sxs-lookup"><span data-stu-id="70a0d-113">Adding the Cart, Order, and OrderDetail model classes</span></span>

<span data-ttu-id="70a0d-114">Notre processus de panier d’achat et l’extraction rendra utiliser certaines nouvelles classes.</span><span class="sxs-lookup"><span data-stu-id="70a0d-114">Our Shopping Cart and Checkout processes will make use of some new classes.</span></span> <span data-ttu-id="70a0d-115">Cliquez sur le dossier Modèles et ajouter une classe de panier (Cart.cs) avec le code suivant.</span><span class="sxs-lookup"><span data-stu-id="70a0d-115">Right-click the Models folder and add a Cart class (Cart.cs) with the following code.</span></span>

[!code-csharp[Main](mvc-music-store-part-8/samples/sample1.cs)]

<span data-ttu-id="70a0d-116">Cette classe est assez similaire à d’autres personnes que nous avons utilisé jusqu’ici, à l’exception de l’attribut [clé] pour la propriété RecordId.</span><span class="sxs-lookup"><span data-stu-id="70a0d-116">This class is pretty similar to others we've used so far, with the exception of the [Key] attribute for the RecordId property.</span></span> <span data-ttu-id="70a0d-117">Nos articles du panier aura un identificateur de chaîne nommé CartID pour autoriser les achats anonymes, mais la table inclut une clé primaire entière nommée RecordId.</span><span class="sxs-lookup"><span data-stu-id="70a0d-117">Our Cart items will have a string identifier named CartID to allow anonymous shopping, but the table includes an integer primary key named RecordId.</span></span> <span data-ttu-id="70a0d-118">Par convention, Entity Framework Code First attend que la clé primaire pour une table nommée panier sera CartId ou ID, mais nous pouvons facilement remplacer que via des annotations ou code si nous voulons.</span><span class="sxs-lookup"><span data-stu-id="70a0d-118">By convention, Entity Framework Code-First expects that the primary key for a table named Cart will be either CartId or ID, but we can easily override that via annotations or code if we want.</span></span> <span data-ttu-id="70a0d-119">Il s’agit d’un exemple de comment nous pouvons utiliser les conventions simples dans le Code First Entity Framework lorsqu’ils sont adaptés à nous, mais nous ne sommes pas contraints par ces derniers lorsque ce n’est pas.</span><span class="sxs-lookup"><span data-stu-id="70a0d-119">This is an example of how we can use the simple conventions in Entity Framework Code-First when they suit us, but we're not constrained by them when they don't.</span></span>

<span data-ttu-id="70a0d-120">Ensuite, ajoutez une classe de commande (Order.cs) avec le code suivant.</span><span class="sxs-lookup"><span data-stu-id="70a0d-120">Next, add an Order class (Order.cs) with the following code.</span></span>

[!code-csharp[Main](mvc-music-store-part-8/samples/sample2.cs)]

<span data-ttu-id="70a0d-121">Cette classe effectue le suivi des informations de résumé et la remise d’une commande.</span><span class="sxs-lookup"><span data-stu-id="70a0d-121">This class tracks summary and delivery information for an order.</span></span> <span data-ttu-id="70a0d-122">**Il n’est pas compilé encore**, car il a une propriété de navigation OrderDetails qui dépend d’une classe que nous n’avons pas encore créé.</span><span class="sxs-lookup"><span data-stu-id="70a0d-122">**It won't compile yet**, because it has an OrderDetails navigation property which depends on a class we haven't created yet.</span></span> <span data-ttu-id="70a0d-123">Nous allons corriger maintenant en ajoutant une classe nommée OrderDetail.cs, en ajoutant le code suivant.</span><span class="sxs-lookup"><span data-stu-id="70a0d-123">Let's fix that now by adding a class named OrderDetail.cs, adding the following code.</span></span>

[!code-csharp[Main](mvc-music-store-part-8/samples/sample3.cs)]

<span data-ttu-id="70a0d-124">Nous allons effectuer une dernière mise à jour notre classe MusicStoreEntities pour inclure les DbSets qui exposent ces nouvelles classes de modèle, également, y compris un DbSet&lt;artiste&gt;.</span><span class="sxs-lookup"><span data-stu-id="70a0d-124">We'll make one last update to our MusicStoreEntities class to include DbSets which expose those new Model classes, also including a DbSet&lt;Artist&gt;.</span></span> <span data-ttu-id="70a0d-125">La classe MusicStoreEntities mis à jour apparaît comme ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="70a0d-125">The updated MusicStoreEntities class appears as below.</span></span>

[!code-csharp[Main](mvc-music-store-part-8/samples/sample4.cs)]

## <a name="managing-the-shopping-cart-business-logic"></a><span data-ttu-id="70a0d-126">La gestion de la logique métier de panier d’achat</span><span class="sxs-lookup"><span data-stu-id="70a0d-126">Managing the Shopping Cart business logic</span></span>

<span data-ttu-id="70a0d-127">Ensuite, nous allons créer la classe ShoppingCart dans le dossier Models.</span><span class="sxs-lookup"><span data-stu-id="70a0d-127">Next, we'll create the ShoppingCart class in the Models folder.</span></span> <span data-ttu-id="70a0d-128">Le modèle ShoppingCart gère l’accès aux données à la table de panier.</span><span class="sxs-lookup"><span data-stu-id="70a0d-128">The ShoppingCart model handles data access to the Cart table.</span></span> <span data-ttu-id="70a0d-129">En outre, il gère la logique métier pour ajout et suppression d’éléments dans le panier d’achat.</span><span class="sxs-lookup"><span data-stu-id="70a0d-129">Additionally, it will handle the business logic to for adding and removing items from the shopping cart.</span></span>

<span data-ttu-id="70a0d-130">Étant donné que nous ne voulons obliger les utilisateurs à s’inscrire pour un compte simplement à ajouter des éléments à leur panier d’achat, nous affecterons les utilisateurs un identificateur unique temporaire (à l’aide d’un GUID ou un identificateur global unique) lorsqu’ils accèdent au panier d’achat.</span><span class="sxs-lookup"><span data-stu-id="70a0d-130">Since we don't want to require users to sign up for an account just to add items to their shopping cart, we will assign users a temporary unique identifier (using a GUID, or globally unique identifier) when they access the shopping cart.</span></span> <span data-ttu-id="70a0d-131">Nous allons stocker cet ID à l’aide de la classe de Session ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="70a0d-131">We'll store this ID using the ASP.NET Session class.</span></span>

<span data-ttu-id="70a0d-132">*Remarque : La Session ASP.NET est un emplacement pratique pour stocker des informations spécifiques à l’utilisateur qui va expirer après qu’ils quittent le site. Tandis que l’utilisation incorrecte de l’état de session peut avoir des implications en matière de performances sur les sites de grande taille, notre utilisation claire fonctionnera bien à des fins de démonstration.*</span><span class="sxs-lookup"><span data-stu-id="70a0d-132">*Note: The ASP.NET Session is a convenient place to store user-specific information which will expire after they leave the site. While misuse of session state can have performance implications on larger sites, our light use will work well for demonstration purposes.*</span></span>

<span data-ttu-id="70a0d-133">La classe ShoppingCart expose les méthodes suivantes :</span><span class="sxs-lookup"><span data-stu-id="70a0d-133">The ShoppingCart class exposes the following methods:</span></span>

<span data-ttu-id="70a0d-134">**AddToCart** prend un Album en tant que paramètre et l’ajoute au panier de l’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="70a0d-134">**AddToCart** takes an Album as a parameter and adds it to the user's cart.</span></span> <span data-ttu-id="70a0d-135">Étant donné que la table de panier effectue le suivi de la quantité pour chaque album, il inclut une logique pour créer une nouvelle ligne, si nécessaire ou incrémenter seulement la quantité si l’utilisateur a déjà commandé une seule copie de l’album.</span><span class="sxs-lookup"><span data-stu-id="70a0d-135">Since the Cart table tracks quantity for each album, it includes logic to create a new row if needed or just increment the quantity if the user has already ordered one copy of the album.</span></span>

<span data-ttu-id="70a0d-136">**RemoveFromCart** prend un ID de l’Album et le supprime de panier de l’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="70a0d-136">**RemoveFromCart** takes an Album ID and removes it from the user's cart.</span></span> <span data-ttu-id="70a0d-137">Si l’utilisateur avait uniquement une seule copie de l’album dans leur panier d’achat, la ligne est supprimée.</span><span class="sxs-lookup"><span data-stu-id="70a0d-137">If the user only had one copy of the album in their cart, the row is removed.</span></span>

<span data-ttu-id="70a0d-138">**EmptyCart** supprime tous les éléments de panier d’achat d’un utilisateur.</span><span class="sxs-lookup"><span data-stu-id="70a0d-138">**EmptyCart** removes all items from a user's shopping cart.</span></span>

<span data-ttu-id="70a0d-139">**GetCartItems** récupère une liste de CartItems pour l’affichage ou de traitement.</span><span class="sxs-lookup"><span data-stu-id="70a0d-139">**GetCartItems** retrieves a list of CartItems for display or processing.</span></span>

<span data-ttu-id="70a0d-140">**GetCount** récupère un le nombre total d’albums d’un utilisateur possède dans leur panier d’achat.</span><span class="sxs-lookup"><span data-stu-id="70a0d-140">**GetCount** retrieves a the total number of albums a user has in their shopping cart.</span></span>

<span data-ttu-id="70a0d-141">**GetTotal** calcule le coût total de tous les éléments dans le panier d’achat.</span><span class="sxs-lookup"><span data-stu-id="70a0d-141">**GetTotal** calculates the total cost of all items in the cart.</span></span>

<span data-ttu-id="70a0d-142">**CreateOrder** convertit le panier d’achat à une commande pendant la phase d’extraction.</span><span class="sxs-lookup"><span data-stu-id="70a0d-142">**CreateOrder** converts the shopping cart to an order during the checkout phase.</span></span>

<span data-ttu-id="70a0d-143">**GetCart** est une méthode statique qui permet à nos contrôleurs obtenir un objet panier.</span><span class="sxs-lookup"><span data-stu-id="70a0d-143">**GetCart** is a static method which allows our controllers to obtain a cart object.</span></span> <span data-ttu-id="70a0d-144">Il utilise le **GetCartId** méthode pour gérer la lecture de la CartId à partir de la session utilisateur.</span><span class="sxs-lookup"><span data-stu-id="70a0d-144">It uses the **GetCartId** method to handle reading the CartId from the user's session.</span></span> <span data-ttu-id="70a0d-145">La méthode GetCartId requiert HttpContextBase afin qu’il puisse lire CartId de l’utilisateur à partir de la session de l’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="70a0d-145">The GetCartId method requires the HttpContextBase so that it can read the user's CartId from user's session.</span></span>

<span data-ttu-id="70a0d-146">Voici l’ensemble **ShoppingCart classe**:</span><span class="sxs-lookup"><span data-stu-id="70a0d-146">Here's the complete **ShoppingCart class**:</span></span>

[!code-csharp[Main](mvc-music-store-part-8/samples/sample5.cs)]

## <a name="viewmodels"></a><span data-ttu-id="70a0d-147">ViewModels</span><span class="sxs-lookup"><span data-stu-id="70a0d-147">ViewModels</span></span>

<span data-ttu-id="70a0d-148">Notre contrôleur de panier d’achat doit communiquer des informations complexes pour ses vues qui ne correspond pas parfaitement à nos objets de modèle.</span><span class="sxs-lookup"><span data-stu-id="70a0d-148">Our Shopping Cart Controller will need to communicate some complex information to its views which doesn't map cleanly to our Model objects.</span></span> <span data-ttu-id="70a0d-149">Nous ne voulons pas à modifier nos modèles pour l’adapter à nos vues ; Classes de modèle doivent représenter notre domaine, pas l’interface utilisateur.</span><span class="sxs-lookup"><span data-stu-id="70a0d-149">We don't want to modify our Models to suit our views; Model classes should represent our domain, not the user interface.</span></span> <span data-ttu-id="70a0d-150">Une solution consisterait à passer des informations à nos vues à l’aide de la classe ViewBag, comme nous l’avons fait avec les informations de la liste déroulante Store Manager, mais en passant un grand nombre d’informations via ViewBag obtient difficile à gérer.</span><span class="sxs-lookup"><span data-stu-id="70a0d-150">One solution would be to pass the information to our Views using the ViewBag class, as we did with the Store Manager dropdown information, but passing a lot of information via ViewBag gets hard to manage.</span></span>

<span data-ttu-id="70a0d-151">Une solution consiste à utiliser le *ViewModel* modèle.</span><span class="sxs-lookup"><span data-stu-id="70a0d-151">A solution to this is to use the *ViewModel* pattern.</span></span> <span data-ttu-id="70a0d-152">Lorsque vous utilisez ce modèle, que nous créons des classes fortement typées qui sont optimisés pour nos scénarios vue spécifique, et qui exposent des propriétés pour le valeurs/le contenu dynamique requises par nos modèles de vue.</span><span class="sxs-lookup"><span data-stu-id="70a0d-152">When using this pattern we create strongly-typed classes that are optimized for our specific view scenarios, and which expose properties for the dynamic values/content needed by our view templates.</span></span> <span data-ttu-id="70a0d-153">Nos classes de contrôleur peuvent remplir, puis transmettre ces classes d’affichage optimisé à notre modèle de vue à utiliser.</span><span class="sxs-lookup"><span data-stu-id="70a0d-153">Our controller classes can then populate and pass these view-optimized classes to our view template to use.</span></span> <span data-ttu-id="70a0d-154">Cela permet une sécurité de type, la vérification au moment de la compilation et éditeur IntelliSense dans les modèles de vue.</span><span class="sxs-lookup"><span data-stu-id="70a0d-154">This enables type-safety, compile-time checking, and editor IntelliSense within view templates.</span></span>

<span data-ttu-id="70a0d-155">Nous allons créer deux modèles de vue pour une utilisation dans notre contrôleur de panier d’achat : le ShoppingCartViewModel contiendra le contenu du panier d’achat de l’utilisateur et le ShoppingCartRemoveViewModel doit être utilisée pour afficher les informations de confirmation lorsqu’un utilisateur supprime un élément à partir de leur panier d’achat.</span><span class="sxs-lookup"><span data-stu-id="70a0d-155">We'll create two View Models for use in our Shopping Cart controller: the ShoppingCartViewModel will hold the contents of the user's shopping cart, and the ShoppingCartRemoveViewModel will be used to display confirmation information when a user removes something from their cart.</span></span>

<span data-ttu-id="70a0d-156">Nous allons créer un nouveau dossier ViewModels à la racine de notre projet pour organiser les éléments.</span><span class="sxs-lookup"><span data-stu-id="70a0d-156">Let's create a new ViewModels folder in the root of our project to keep things organized.</span></span> <span data-ttu-id="70a0d-157">Cliquez sur le projet, sélectionnez Ajouter / nouveau dossier.</span><span class="sxs-lookup"><span data-stu-id="70a0d-157">Right-click the project, select Add / New Folder.</span></span>

![](mvc-music-store-part-8/_static/image1.jpg)

<span data-ttu-id="70a0d-158">Nommez le dossier ViewModels.</span><span class="sxs-lookup"><span data-stu-id="70a0d-158">Name the folder ViewModels.</span></span>

![](mvc-music-store-part-8/_static/image1.png)

<span data-ttu-id="70a0d-159">Ensuite, ajoutez la classe ShoppingCartViewModel dans le dossier ViewModels.</span><span class="sxs-lookup"><span data-stu-id="70a0d-159">Next, add the ShoppingCartViewModel class in the ViewModels folder.</span></span> <span data-ttu-id="70a0d-160">Il a deux propriétés : une liste des éléments du panier et une valeur décimale pour contenir le prix total de tous les éléments dans le panier d’achat.</span><span class="sxs-lookup"><span data-stu-id="70a0d-160">It has two properties: a list of Cart items, and a decimal value to hold the total price for all items in the cart.</span></span>

[!code-csharp[Main](mvc-music-store-part-8/samples/sample6.cs)]

<span data-ttu-id="70a0d-161">Ajoutez maintenant le ShoppingCartRemoveViewModel au dossier ViewModels, avec les quatre propriétés suivantes.</span><span class="sxs-lookup"><span data-stu-id="70a0d-161">Now add the ShoppingCartRemoveViewModel to the ViewModels folder, with the following four properties.</span></span>

[!code-csharp[Main](mvc-music-store-part-8/samples/sample7.cs)]

## <a name="the-shopping-cart-controller"></a><span data-ttu-id="70a0d-162">Le contrôleur de panier d’achat</span><span class="sxs-lookup"><span data-stu-id="70a0d-162">The Shopping Cart Controller</span></span>

<span data-ttu-id="70a0d-163">Le contrôleur de panier d’achat a trois objectifs principaux : ajout d’éléments à un panier, supprimer des éléments du panier et afficher les éléments dans le panier d’achat.</span><span class="sxs-lookup"><span data-stu-id="70a0d-163">The Shopping Cart controller has three main purposes: adding items to a cart, removing items from the cart, and viewing items in the cart.</span></span> <span data-ttu-id="70a0d-164">Il utilisera les trois classes nous venons de créer : ShoppingCartViewModel, ShoppingCartRemoveViewModel et ShoppingCart.</span><span class="sxs-lookup"><span data-stu-id="70a0d-164">It will make use of the three classes we just created: ShoppingCartViewModel, ShoppingCartRemoveViewModel, and ShoppingCart.</span></span> <span data-ttu-id="70a0d-165">Comme dans les StoreController et le StoreManagerController, nous allons ajouter un champ pour stocker une instance de MusicStoreEntities.</span><span class="sxs-lookup"><span data-stu-id="70a0d-165">As in the StoreController and StoreManagerController, we'll add a field to hold an instance of MusicStoreEntities.</span></span>

<span data-ttu-id="70a0d-166">Ajoutez un nouveau contrôleur de panier d’achat pour le projet en utilisant le modèle de contrôleur vide.</span><span class="sxs-lookup"><span data-stu-id="70a0d-166">Add a new Shopping Cart controller to the project using the Empty controller template.</span></span>

![](mvc-music-store-part-8/_static/image2.png)

<span data-ttu-id="70a0d-167">Voici le contrôleur ShoppingCart terminée.</span><span class="sxs-lookup"><span data-stu-id="70a0d-167">Here's the complete ShoppingCart Controller.</span></span> <span data-ttu-id="70a0d-168">Les actions d’Index et d’ajouter un contrôleur doivent sembler très familière.</span><span class="sxs-lookup"><span data-stu-id="70a0d-168">The Index and Add Controller actions should look very familiar.</span></span> <span data-ttu-id="70a0d-169">Les actions de contrôleur Remove et CartSummary traiter les deux cas spéciaux, ce que nous aborderons dans la section suivante.</span><span class="sxs-lookup"><span data-stu-id="70a0d-169">The Remove and CartSummary controller actions handle two special cases, which we'll discuss in the following section.</span></span>

[!code-csharp[Main](mvc-music-store-part-8/samples/sample8.cs)]

## <a name="ajax-updates-with-jquery"></a><span data-ttu-id="70a0d-170">Mises à jour AJAX avec jQuery</span><span class="sxs-lookup"><span data-stu-id="70a0d-170">Ajax Updates with jQuery</span></span>

<span data-ttu-id="70a0d-171">Nous allons ensuite créer une page d’Index de panier d’achat qui est fortement typée à le ShoppingCartViewModel et utilise le modèle de vue de liste à l’aide de la même méthode comme avant.</span><span class="sxs-lookup"><span data-stu-id="70a0d-171">We'll next create a Shopping Cart Index page that is strongly typed to the ShoppingCartViewModel and uses the List View template using the same method as before.</span></span>

![](mvc-music-store-part-8/_static/image3.png)

<span data-ttu-id="70a0d-172">Toutefois, au lieu d’utiliser un Html.ActionLink pour supprimer des éléments du panier, nous allons utiliser jQuery pour « associer » l’événement click pour tous les liens dans cette vue ayant la classe HTML RemoveLink.</span><span class="sxs-lookup"><span data-stu-id="70a0d-172">However, instead of using an Html.ActionLink to remove items from the cart, we'll use jQuery to "wire up" the click event for all links in this view which have the HTML class RemoveLink.</span></span> <span data-ttu-id="70a0d-173">Au lieu de renvoyer le formulaire, ce gestionnaire d’événements click fera simplement un rappel AJAX à notre action de contrôleur RemoveFromCart.</span><span class="sxs-lookup"><span data-stu-id="70a0d-173">Rather than posting the form, this click event handler will just make an AJAX callback to our RemoveFromCart controller action.</span></span> <span data-ttu-id="70a0d-174">Le RemoveFromCart retourne un résultat sérialisé au format JSON, qui notre rappel jQuery, puis analyse et effectue quatre mises à jour rapides à la page à l’aide de jQuery :</span><span class="sxs-lookup"><span data-stu-id="70a0d-174">The RemoveFromCart returns a JSON serialized result, which our jQuery callback then parses and performs four quick updates to the page using jQuery:</span></span>

- 1. <span data-ttu-id="70a0d-175">Supprime l’album supprimé de la liste</span><span class="sxs-lookup"><span data-stu-id="70a0d-175">Removes the deleted album from the list</span></span>
- 2. <span data-ttu-id="70a0d-176">Met à jour le nombre de panier dans l’en-tête</span><span class="sxs-lookup"><span data-stu-id="70a0d-176">Updates the cart count in the header</span></span>
- 3. <span data-ttu-id="70a0d-177">Affiche un message de mise à jour à l’utilisateur</span><span class="sxs-lookup"><span data-stu-id="70a0d-177">Displays an update message to the user</span></span>
- 4. <span data-ttu-id="70a0d-178">Met à jour le prix total du panier</span><span class="sxs-lookup"><span data-stu-id="70a0d-178">Updates the cart total price</span></span>

<span data-ttu-id="70a0d-179">Étant donné que le scénario de suppression est traité par un rappel Ajax au sein de la vue Index, nous n’avons besoin une vue supplémentaire pour l’action de RemoveFromCart.</span><span class="sxs-lookup"><span data-stu-id="70a0d-179">Since the remove scenario is being handled by an Ajax callback within the Index view, we don't need an additional view for RemoveFromCart action.</span></span> <span data-ttu-id="70a0d-180">Voici le code complet pour la vue /ShoppingCart/Index :</span><span class="sxs-lookup"><span data-stu-id="70a0d-180">Here is the complete code for the /ShoppingCart/Index view:</span></span>

[!code-cshtml[Main](mvc-music-store-part-8/samples/sample9.cshtml)]

<span data-ttu-id="70a0d-181">Pour tester, que nous devons être en mesure d’ajouter des éléments à notre panier d’achat.</span><span class="sxs-lookup"><span data-stu-id="70a0d-181">In order to test this out, we need to be able to add items to our shopping cart.</span></span> <span data-ttu-id="70a0d-182">Nous mettrons à jour notre **Store détails** vue et inclure un bouton « Ajouter au panier ».</span><span class="sxs-lookup"><span data-stu-id="70a0d-182">We'll update our **Store Details** view to include an "Add to cart" button.</span></span> <span data-ttu-id="70a0d-183">Pendant que nous sommes à elle, nous pouvons inclure certaines informations supplémentaires Album dont nous avons ajouté dans la mesure où nous avons mis à jour cette vue : Genre, artiste, prix et pochette d’Album.</span><span class="sxs-lookup"><span data-stu-id="70a0d-183">While we're at it, we can include some of the Album additional information which we've added since we last updated this view: Genre, Artist, Price, and Album Art.</span></span> <span data-ttu-id="70a0d-184">Le code de vue Store détails mis à jour s’affiche comme indiqué ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="70a0d-184">The updated Store Details view code appears as shown below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-8/samples/sample10.cshtml)]

<span data-ttu-id="70a0d-185">Nous pouvons maintenant Cliquez sur via le magasin et tester l’ajout et la suppression des Albums vers et à partir de notre panier d’achat.</span><span class="sxs-lookup"><span data-stu-id="70a0d-185">Now we can click through the store and test adding and removing Albums to and from our shopping cart.</span></span> <span data-ttu-id="70a0d-186">Exécutez l’application et accédez à l’Index de Store.</span><span class="sxs-lookup"><span data-stu-id="70a0d-186">Run the application and browse to the Store Index.</span></span>

![](mvc-music-store-part-8/_static/image4.png)

<span data-ttu-id="70a0d-187">Ensuite, cliquez sur un Genre pour afficher une liste des albums.</span><span class="sxs-lookup"><span data-stu-id="70a0d-187">Next, click on a Genre to view a list of albums.</span></span>

![](mvc-music-store-part-8/_static/image5.png)

<span data-ttu-id="70a0d-188">En cliquant sur un titre d’Album maintenant montre notre vue Détails de l’Album mis à jour, y compris le bouton « Ajouter au panier ».</span><span class="sxs-lookup"><span data-stu-id="70a0d-188">Clicking on an Album title now shows our updated Album Details view, including the "Add to cart" button.</span></span>

![](mvc-music-store-part-8/_static/image6.png)

<span data-ttu-id="70a0d-189">En cliquant sur le bouton « Ajouter au panier » montre notre vue Index de panier d’achat avec la liste de résumé de panier d’achat.</span><span class="sxs-lookup"><span data-stu-id="70a0d-189">Clicking the "Add to cart" button shows our Shopping Cart Index view with the shopping cart summary list.</span></span>

![](mvc-music-store-part-8/_static/image7.png)

<span data-ttu-id="70a0d-190">Après avoir chargé votre panier d’achat, vous pouvez cliquer sur la supprimer à partir du lien du panier pour voir la mise à jour Ajax à votre panier d’achat.</span><span class="sxs-lookup"><span data-stu-id="70a0d-190">After loading up your shopping cart, you can click on the Remove from cart link to see the Ajax update to your shopping cart.</span></span>

![](mvc-music-store-part-8/_static/image8.png)

<span data-ttu-id="70a0d-191">Nous avons développé un travail panier qui permet à des utilisateurs non inscrits à ajouter des éléments à leur panier d’achat.</span><span class="sxs-lookup"><span data-stu-id="70a0d-191">We've built out a working shopping cart which allows unregistered users to add items to their cart.</span></span> <span data-ttu-id="70a0d-192">Dans la section suivante, nous allons pouvoir inscrire et terminer le processus de validation.</span><span class="sxs-lookup"><span data-stu-id="70a0d-192">In the following section, we'll allow them to register and complete the checkout process.</span></span>


> [!div class="step-by-step"]
> <span data-ttu-id="70a0d-193">[Précédent](mvc-music-store-part-7.md)
> [Suivant](mvc-music-store-part-9.md)</span><span class="sxs-lookup"><span data-stu-id="70a0d-193">[Previous](mvc-music-store-part-7.md)
[Next](mvc-music-store-part-9.md)</span></span>
