---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-5
title: 'Partie 5 : La logique métier | Documents Microsoft'
author: JoeStagner
description: Cette série de didacticiels détaille toutes les mesures prises pour générer l’exemple d’application Tailspin Spyworks. Partie 5 ajoute une logique métier.
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/21/2010
ms.topic: article
ms.assetid: eaef475a-ca91-47ea-a4a7-d074005ed80c
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-5
msc.type: authoredcontent
ms.openlocfilehash: e4342e634ef8c4bcf4e0085650a28f414ab23736
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30885079"
---
<a name="part-5-business-logic"></a><span data-ttu-id="6ce9e-104">Partie 5 : La logique métier</span><span class="sxs-lookup"><span data-stu-id="6ce9e-104">Part 5: Business Logic</span></span>
====================
<span data-ttu-id="6ce9e-105">par [Joe Stagner](https://github.com/JoeStagner)</span><span class="sxs-lookup"><span data-stu-id="6ce9e-105">by [Joe Stagner](https://github.com/JoeStagner)</span></span>

> <span data-ttu-id="6ce9e-106">Tailspin Spyworks montre comment extrêmement simple est de créer des applications puissantes et évolutives pour la plateforme .NET.</span><span class="sxs-lookup"><span data-stu-id="6ce9e-106">Tailspin Spyworks demonstrates how extraordinarily simple it is to create powerful, scalable applications for the .NET platform.</span></span> <span data-ttu-id="6ce9e-107">Il illustre comment utiliser les nouvelles fonctionnalités dans ASP.NET 4 pour créer un magasin en ligne, y compris les achats, l’extraction et l’administration.</span><span class="sxs-lookup"><span data-stu-id="6ce9e-107">It shows off how to use the great new features in ASP.NET 4 to build an online store, including shopping, checkout, and administration.</span></span>
> 
> <span data-ttu-id="6ce9e-108">Cette série de didacticiels détaille toutes les mesures prises pour générer l’exemple d’application Tailspin Spyworks.</span><span class="sxs-lookup"><span data-stu-id="6ce9e-108">This tutorial series details all of the steps taken to build the Tailspin Spyworks sample application.</span></span> <span data-ttu-id="6ce9e-109">Partie 5 ajoute une logique métier.</span><span class="sxs-lookup"><span data-stu-id="6ce9e-109">Part 5 adds some business logic.</span></span>


## <a id="_Toc260221671"></a>  <span data-ttu-id="6ce9e-110">Ajout d’une logique métier</span><span class="sxs-lookup"><span data-stu-id="6ce9e-110">Adding Some Business Logic</span></span>

<span data-ttu-id="6ce9e-111">Nous voulons que notre expérience d’achat chaque fois que quelqu'un visite notre site web.</span><span class="sxs-lookup"><span data-stu-id="6ce9e-111">We want our shopping experience to be available whenever someone visits our web site.</span></span> <span data-ttu-id="6ce9e-112">Visiteurs sera en mesure de parcourir et ajouter des éléments au panier d’achat même s’ils ne sont pas inscrits ou connectés.</span><span class="sxs-lookup"><span data-stu-id="6ce9e-112">Visitors will be able to browse and add items to the shopping cart even if they are not registered or logged in.</span></span> <span data-ttu-id="6ce9e-113">Lorsqu’elles sont prêtes à extraire ils reçoivent la possibilité de s’authentifier et si elles ne sont pas encore membres qu’ils seront en mesure de créer un compte.</span><span class="sxs-lookup"><span data-stu-id="6ce9e-113">When they are ready to check out they will be given the option to authenticate and if they are not yet members they will be able to create an account.</span></span>

<span data-ttu-id="6ce9e-114">Cela signifie que nous devons implémenter la logique permettant de convertir le panier d’achat d’un état anonyme dans un état « Utilisateur inscrit ».</span><span class="sxs-lookup"><span data-stu-id="6ce9e-114">This means that we will need to implement the logic to convert the shopping cart from an anonymous state to a "Registered User" state.</span></span>

<span data-ttu-id="6ce9e-115">Nous allons créer un répertoire nommé « Classes » avec le bouton droit sur le dossier puis créez un nouveau fichier « Classe » MyShoppingCart.cs</span><span class="sxs-lookup"><span data-stu-id="6ce9e-115">Let's create a directory named "Classes" then Right-Click on the folder and create a new "Class" file named MyShoppingCart.cs</span></span>

![](tailspin-spyworks-part-5/_static/image1.jpg)

![](tailspin-spyworks-part-5/_static/image1.png)

<span data-ttu-id="6ce9e-116">Comme mentionné précédemment nous sera extension de la classe qui implémente la page MyShoppingCart.aspx et nous fera à l’aide. Construction de puissantes « classe partielle » du réseau.</span><span class="sxs-lookup"><span data-stu-id="6ce9e-116">As previously mentioned we will be extending the class that implements the MyShoppingCart.aspx page and we will do this using .NET's powerful "Partial Class" construct.</span></span>

<span data-ttu-id="6ce9e-117">Voici à quoi ressemble l’appel généré pour notre fichier MyShoppingCart.aspx.cf.</span><span class="sxs-lookup"><span data-stu-id="6ce9e-117">The generated call for our MyShoppingCart.aspx.cf file looks like this.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample1.cs)]

<span data-ttu-id="6ce9e-118">Notez l’utilisation du mot clé « partielle ».</span><span class="sxs-lookup"><span data-stu-id="6ce9e-118">Note the use of the "partial" keyword.</span></span>

<span data-ttu-id="6ce9e-119">Le fichier de classe qui nous vient d’être générées ressemble à ceci.</span><span class="sxs-lookup"><span data-stu-id="6ce9e-119">The class file that we just generated looks like this.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample2.cs)]

<span data-ttu-id="6ce9e-120">Fusionner nos mises en œuvre en ajoutant le mot clé partiels à ce fichier.</span><span class="sxs-lookup"><span data-stu-id="6ce9e-120">We will merge our implementations by adding the partial keyword to this file as well.</span></span>

<span data-ttu-id="6ce9e-121">Notre nouveau fichier de classe ressemble à ceci.</span><span class="sxs-lookup"><span data-stu-id="6ce9e-121">Our new class file now looks like this.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample3.cs)]

<span data-ttu-id="6ce9e-122">La première méthode que vous allez ajouter à la classe est la méthode « AddItem ».</span><span class="sxs-lookup"><span data-stu-id="6ce9e-122">The first method that we will add to our class is the "AddItem" method.</span></span> <span data-ttu-id="6ce9e-123">Il s’agit de la méthode qui sera finalement appelée lorsque l’utilisateur clique sur les liens « Ajouter à l’Art » sur les pages de la liste des produits et des détails sur le produit.</span><span class="sxs-lookup"><span data-stu-id="6ce9e-123">This is the method that will ultimately be called when the user clicks on the "Add to Art" links on the Product List and Product Details pages.</span></span>

<span data-ttu-id="6ce9e-124">Ajoutez le code suivant à en utilisant les instructions en haut de la page.</span><span class="sxs-lookup"><span data-stu-id="6ce9e-124">Append the following to the using statements at the top of the page.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample4.cs)]

<span data-ttu-id="6ce9e-125">Et ajouter cette méthode à la classe MyShoppingCart.</span><span class="sxs-lookup"><span data-stu-id="6ce9e-125">And add this method to the MyShoppingCart class.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample5.cs)]

<span data-ttu-id="6ce9e-126">Nous utilisons LINQ to Entities pour voir si l’élément est déjà dans le panier d’achat.</span><span class="sxs-lookup"><span data-stu-id="6ce9e-126">We are using LINQ to Entities to see if the item is already in the cart.</span></span> <span data-ttu-id="6ce9e-127">Si, par conséquent, nous mettre à jour la quantité commandée de l’élément, sinon nous créer une nouvelle entrée pour l’élément sélectionné</span><span class="sxs-lookup"><span data-stu-id="6ce9e-127">If so, we update the order quantity of the item, otherwise we create a new entry for the selected item</span></span>

<span data-ttu-id="6ce9e-128">Pour pouvoir appeler cette méthode, nous implémenterons une page AddToCart.aspx qui non seulement cette méthode de classe, mais affiche ensuite un panier = après l’ajout de l’élément actuel.</span><span class="sxs-lookup"><span data-stu-id="6ce9e-128">In order to call this method we will implement an AddToCart.aspx page that not only class this method but then displayed the current shopping a=cart after the item has been added.</span></span>

<span data-ttu-id="6ce9e-129">Avec le bouton droit sur le nom de la solution dans l’Explorateur de solutions et ajoutez et nouvelle page nommée AddToCart.aspx comme nous l’avons fait précédemment.</span><span class="sxs-lookup"><span data-stu-id="6ce9e-129">Right-Click on the solution name in the solution explorer and add and new page named AddToCart.aspx as we have done previously.</span></span>

<span data-ttu-id="6ce9e-130">Pendant que nous pourrions utiliser cette page pour afficher les résultats intermédiaires telles que des problèmes de stock faibles, etc., dans notre implémentation, la page ne sont pas effectuer le rendu, mais plutôt appeler la logique de « Add » et rediriger.</span><span class="sxs-lookup"><span data-stu-id="6ce9e-130">While we could use this page to display interim results like low stock issues, etc, in our implementation, the page will not actually render, but rather call the "Add" logic and redirect.</span></span>

<span data-ttu-id="6ce9e-131">Pour ce faire, nous ajouterons le code suivant à la Page\_événement de chargement.</span><span class="sxs-lookup"><span data-stu-id="6ce9e-131">To accomplish this we'll add the following code to the Page\_Load event.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample6.cs)]

<span data-ttu-id="6ce9e-132">Notez que nous récupérons le produit à ajouter au panier d’achat à partir d’un paramètre de chaîne de requête et en appelant la méthode AddItem de notre classe.</span><span class="sxs-lookup"><span data-stu-id="6ce9e-132">Note that we are retrieving the product to add to the shopping cart from a QueryString parameter and calling the AddItem method of our class.</span></span>

<span data-ttu-id="6ce9e-133">En supposant qu’aucune erreur ne se produisent lorsque le contrôle est passé à la page SHoppingCart.aspx qui nous implémenterons entièrement ensuite.</span><span class="sxs-lookup"><span data-stu-id="6ce9e-133">Assuming no errors are encountered control is passed to the SHoppingCart.aspx page which we will fully implement next.</span></span> <span data-ttu-id="6ce9e-134">S’il doit y avoir une erreur nous lève une exception.</span><span class="sxs-lookup"><span data-stu-id="6ce9e-134">If there should be an error we throw an exception.</span></span>

<span data-ttu-id="6ce9e-135">Actuellement nous n'avons pas encore implémenté un gestionnaire d’erreurs globales pour cette exception va passer non prise en charge par notre application, mais nous sera bientôt remédier.</span><span class="sxs-lookup"><span data-stu-id="6ce9e-135">Currently we have not yet implemented a global error handler so this exception would go unhandled by our application but we will remedy this shortly.</span></span>

<span data-ttu-id="6ce9e-136">Notez également l’utilisation de l’instruction Debug.Fail() (disponible via `using System.Diagnostics;)`</span><span class="sxs-lookup"><span data-stu-id="6ce9e-136">Note also the use of the statement Debug.Fail() (available via `using System.Diagnostics;)`</span></span>

<span data-ttu-id="6ce9e-137">Est de l’application s’exécute dans le débogueur, cette méthode affiche une boîte de dialogue détaillée avec des informations sur l’état des applications, ainsi que le message d’erreur qui nous spécifions.</span><span class="sxs-lookup"><span data-stu-id="6ce9e-137">Is the application is running inside the debugger, this method will display a detailed dialog with information about the applications state along with the error message that we specify.</span></span>

<span data-ttu-id="6ce9e-138">Lors de l’exécution en production la Debug.Fail() instruction est ignorée.</span><span class="sxs-lookup"><span data-stu-id="6ce9e-138">When running in production the Debug.Fail() statement is ignored.</span></span>

<span data-ttu-id="6ce9e-139">Vous noterez dans le code situé au-dessus d’un appel à une méthode dans nos noms de classe de panier d’achat « GetShoppingCartId ».</span><span class="sxs-lookup"><span data-stu-id="6ce9e-139">You will note in the code above a call to a method in our shopping cart class names "GetShoppingCartId".</span></span>

<span data-ttu-id="6ce9e-140">Ajoutez le code pour implémenter la méthode comme suit.</span><span class="sxs-lookup"><span data-stu-id="6ce9e-140">Add the code to implement the method as follows.</span></span>

<span data-ttu-id="6ce9e-141">Notez que nous avons également ajouté une mise à jour et l’extraction des boutons et une étiquette où nous pouvons afficher le panier d’achat « total ».</span><span class="sxs-lookup"><span data-stu-id="6ce9e-141">Note that we've also added update and checkout buttons and a label where we can display the cart "total".</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample7.cs)]

<span data-ttu-id="6ce9e-142">Nous pouvons maintenant ajouter des éléments à notre panier d’achat, mais nous n’avons pas implémenté la logique permettant d’afficher le panier après l’ajout d’un produit.</span><span class="sxs-lookup"><span data-stu-id="6ce9e-142">We can now add items to our shopping cart but we have not implemented the logic to display the cart after a product has been added.</span></span>

<span data-ttu-id="6ce9e-143">Par conséquent, dans la page MyShoppingCart.aspx nous allons ajouter un contrôle EntityDataSource et un contrôle GridVire comme suit.</span><span class="sxs-lookup"><span data-stu-id="6ce9e-143">So, in the MyShoppingCart.aspx page we'll add an EntityDataSource control and a GridVire control as follows.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-5/samples/sample8.aspx)]

<span data-ttu-id="6ce9e-144">Appelez le formulaire dans le concepteur afin que vous pouvez cliquer sur le bouton de mise à jour le panier et générer le Gestionnaire d’événements click qui est spécifié dans la déclaration dans le balisage.</span><span class="sxs-lookup"><span data-stu-id="6ce9e-144">Call up the form in the designer so that you can double click on the Update Cart button and generate the click event handler that is specified in the declaration in the markup.</span></span>

<span data-ttu-id="6ce9e-145">Nous allons implémenter les détails ultérieurement, mais cela nous générez et exécutez notre application sans erreurs.</span><span class="sxs-lookup"><span data-stu-id="6ce9e-145">We'll implement the details later but doing this will let us build and run our application without errors.</span></span>

<span data-ttu-id="6ce9e-146">Lorsque vous exécutez l’application et que vous ajoutez un élément dans le panier d’achat vous voyez ceci.</span><span class="sxs-lookup"><span data-stu-id="6ce9e-146">When you run the application and add an item to the shopping cart you will see this.</span></span>

![](tailspin-spyworks-part-5/_static/image2.jpg)

<span data-ttu-id="6ce9e-147">Notez que nous avons sorties à partir de l’affichage de grille « default » en implémentant les trois colonnes personnalisées.</span><span class="sxs-lookup"><span data-stu-id="6ce9e-147">Note that we have deviated from the "default" grid display by implementing three custom columns.</span></span>

<span data-ttu-id="6ce9e-148">Le premier est un champ « Lié » pour la quantité modifiable :</span><span class="sxs-lookup"><span data-stu-id="6ce9e-148">The first is an Editable, "Bound" field for the Quantity:</span></span>

[!code-aspx[Main](tailspin-spyworks-part-5/samples/sample9.aspx)]

<span data-ttu-id="6ce9e-149">La prochaine est une colonne « calculée » qui affiche l’élément de ligne de total (l’élément de coût fois la quantité à commander) :</span><span class="sxs-lookup"><span data-stu-id="6ce9e-149">The next is a "calculated" column that displays the line item total (the item cost times the quantity to be ordered):</span></span>

[!code-aspx[Main](tailspin-spyworks-part-5/samples/sample10.aspx)]

<span data-ttu-id="6ce9e-150">Enfin, nous avons une colonne personnalisée qui contient un contrôle de case à cocher l’utilisateur utilisera pour indiquer que l’élément doit être supprimé du plan d’achat.</span><span class="sxs-lookup"><span data-stu-id="6ce9e-150">Lastly we have a custom column that contains a CheckBox control that the user will use to indicate that the item should be removed from the shopping chart.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-5/samples/sample11.aspx)]

![](tailspin-spyworks-part-5/_static/image3.jpg)

<span data-ttu-id="6ce9e-151">Comme vous pouvez le voir, l’ordre de Total de la ligne est vide, nous allons donc ajouter une logique pour calculer le Total des commandes.</span><span class="sxs-lookup"><span data-stu-id="6ce9e-151">As you can see, the Order Total line is empty so let's add some logic to calculate the Order Total.</span></span>

<span data-ttu-id="6ce9e-152">Nous allons tout d’abord implémenter une méthode « GetTotal » à la classe MyShoppingCart.</span><span class="sxs-lookup"><span data-stu-id="6ce9e-152">We'll first implement a "GetTotal" method to our MyShoppingCart Class.</span></span>

<span data-ttu-id="6ce9e-153">Dans le fichier MyShoppingCart.cs, ajoutez le code suivant.</span><span class="sxs-lookup"><span data-stu-id="6ce9e-153">In the MyShoppingCart.cs file add the following code.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample12.cs)]

<span data-ttu-id="6ce9e-154">Ensuite, dans la Page\_Gestionnaire d’événements Load nous allons peut appeler la méthode GetTotal.</span><span class="sxs-lookup"><span data-stu-id="6ce9e-154">Then in the Page\_Load event handler we'll can call our GetTotal method.</span></span> <span data-ttu-id="6ce9e-155">En même temps, nous allons ajouter un test pour voir si le panier d’achat est vide et ajustez l’affichage en conséquence, s’il s’agit.</span><span class="sxs-lookup"><span data-stu-id="6ce9e-155">At the same time we'll add a test to see if the shopping cart is empty and adjust the display accordingly if it is.</span></span>

<span data-ttu-id="6ce9e-156">Si le panier d’achat est vide nous maintenant cela :</span><span class="sxs-lookup"><span data-stu-id="6ce9e-156">Now if the shopping cart is empty we get this:</span></span>

![](tailspin-spyworks-part-5/_static/image4.jpg)

<span data-ttu-id="6ce9e-157">Et dans le cas contraire, le total.</span><span class="sxs-lookup"><span data-stu-id="6ce9e-157">And if not, we see our total.</span></span>

![](tailspin-spyworks-part-5/_static/image5.jpg)

<span data-ttu-id="6ce9e-158">Toutefois, cette page n’est pas encore terminée.</span><span class="sxs-lookup"><span data-stu-id="6ce9e-158">However, this page is not yet complete.</span></span>

<span data-ttu-id="6ce9e-159">Nous devons logique supplémentaire pour recalculer le panier d’achat en supprimant les éléments marqués pour suppression et en déterminant les nouvelles valeurs de quantité certaines peuvent avoir été modifiée dans la grille par l’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="6ce9e-159">We will need additional logic to recalculate the shopping cart by removing items marked for removal and by determining new quantity values as some may have been changed in the grid by the user.</span></span>

<span data-ttu-id="6ce9e-160">Permet d’ajouter une méthode « RemoveItem » à notre classe de panier d’achat dans MyShoppingCart.cs pour gérer le cas lorsqu’un utilisateur marque un élément pour la suppression.</span><span class="sxs-lookup"><span data-stu-id="6ce9e-160">Lets add a "RemoveItem" method to our shopping cart class in MyShoppingCart.cs to handle the case when a user marks an item for removal.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample13.cs)]

<span data-ttu-id="6ce9e-161">Maintenant nous allons ad une méthode pour gérer le cas lorsqu’un utilisateur modifie simplement la qualité pour être classés dans le GridView.</span><span class="sxs-lookup"><span data-stu-id="6ce9e-161">Now let's ad a method to handle the circumstance when a user simply changes the quality to be ordered in the GridView.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample14.cs)]

<span data-ttu-id="6ce9e-162">Avec les fonctionnalités de base de mise à jour et suppression en place, nous pouvons implémenter la logique qui met à jour le panier d’achat dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="6ce9e-162">With the basic Remove and Update features in place we can implement the logic that actually updates the shopping cart in the database.</span></span> <span data-ttu-id="6ce9e-163">(Dans MyShoppingCart.cs)</span><span class="sxs-lookup"><span data-stu-id="6ce9e-163">(In MyShoppingCart.cs)</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample15.cs)]

<span data-ttu-id="6ce9e-164">Vous remarquerez que cette méthode attend deux paramètres.</span><span class="sxs-lookup"><span data-stu-id="6ce9e-164">You'll note that this method expects two parameters.</span></span> <span data-ttu-id="6ce9e-165">Un est l’Id de panier d’achat et l’autre est un tableau d’objets de type de défini par l’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="6ce9e-165">One is the shopping cart Id and the other is an array of objects of user defined type.</span></span>

<span data-ttu-id="6ce9e-166">Pour minimiser la dépendance de notre logique sur les caractéristiques d’interface utilisateur, nous avons défini une structure de données que nous pouvons utiliser pour passer les éléments de panier d’achat à notre code sans avoir à accéder directement le contrôle GridView notre méthode de.</span><span class="sxs-lookup"><span data-stu-id="6ce9e-166">So as to minimize the dependency of our logic on user interface specifics, we've defined a data structure that we can use to pass the shopping cart items to our code without our method needing to directly access the GridView control.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample16.cs)]

<span data-ttu-id="6ce9e-167">Dans notre fichier MyShoppingCart.aspx.cs nous pouvons utiliser cette structure dans notre gestionnaire d’événements de cliquez sur bouton mise à jour comme suit.</span><span class="sxs-lookup"><span data-stu-id="6ce9e-167">In our MyShoppingCart.aspx.cs file we can use this structure in our Update Button Click Event handler as follows.</span></span> <span data-ttu-id="6ce9e-168">Notez qu’en plus de la mise à jour le panier nous mettrons à jour le total de l’achat.</span><span class="sxs-lookup"><span data-stu-id="6ce9e-168">Note that in addition to updating the cart we will update the cart total as well.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample17.cs)]

<span data-ttu-id="6ce9e-169">Notez, avec un intérêt particulier, cette ligne de code :</span><span class="sxs-lookup"><span data-stu-id="6ce9e-169">Note with particular interest this line of code:</span></span>

[!code-javascript[Main](tailspin-spyworks-part-5/samples/sample18.js)]

<span data-ttu-id="6ce9e-170">GetValues() est une fonction d’assistance spéciales qui nous implémenterons dans MyShoppingCart.aspx.cs comme suit.</span><span class="sxs-lookup"><span data-stu-id="6ce9e-170">GetValues() is a special helper function that we will implement in MyShoppingCart.aspx.cs as follows.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample19.cs)]

<span data-ttu-id="6ce9e-171">Cela fournit un moyen adéquat pour accéder aux valeurs des éléments liés dans notre contrôle GridView.</span><span class="sxs-lookup"><span data-stu-id="6ce9e-171">This provides a clean way to access the values of the bound elements in our GridView control.</span></span> <span data-ttu-id="6ce9e-172">Étant donné que notre contrôle de case à cocher « Supprimer l’élément » n’est pas lié nous allons y accéder via la méthode FindControl().</span><span class="sxs-lookup"><span data-stu-id="6ce9e-172">Since our "Remove Item" CheckBox Control is not bound we'll access it via the FindControl() method.</span></span>

<span data-ttu-id="6ce9e-173">À ce stade du développement de votre projet, nous recevons prêts à mettre en œuvre le processus de validation.</span><span class="sxs-lookup"><span data-stu-id="6ce9e-173">At this stage in your project's development we are getting ready to implement the checkout process.</span></span>

<span data-ttu-id="6ce9e-174">Avant cela nous allons utiliser Visual Studio pour générer la base de données d’appartenance et ajouter un utilisateur dans le référentiel d’appartenance.</span><span class="sxs-lookup"><span data-stu-id="6ce9e-174">Before doing so let's use Visual Studio to generate the membership database and add a user to the membership repository.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="6ce9e-175">[Précédent](tailspin-spyworks-part-4.md)
> [Suivant](tailspin-spyworks-part-6.md)</span><span class="sxs-lookup"><span data-stu-id="6ce9e-175">[Previous](tailspin-spyworks-part-4.md)
[Next](tailspin-spyworks-part-6.md)</span></span>
