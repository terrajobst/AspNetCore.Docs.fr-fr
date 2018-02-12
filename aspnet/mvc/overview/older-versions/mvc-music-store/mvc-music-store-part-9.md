---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-9
title: "Partie 9 : Enregistrement et extraction | Documents Microsoft"
author: jongalloway
description: "Cette série de didacticiels détaille toutes les mesures prises pour générer l’exemple d’application de magasin de musique ASP.NET MVC. Partie 9 couvre l’inscription et l’extraction."
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/21/2011
ms.topic: article
ms.assetid: d65c5c2b-a039-463f-ad29-25cf9fb7a1ba
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-9
msc.type: authoredcontent
ms.openlocfilehash: 1caf836f8c92cbc9ab95e0aa990f81493e577a27
ms.sourcegitcommit: 016f4d58663bcd442930227022de23fb3abee0b3
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/12/2018
---
<a name="part-9-registration-and-checkout"></a><span data-ttu-id="281eb-104">Partie 9 : Enregistrement et extraction</span><span class="sxs-lookup"><span data-stu-id="281eb-104">Part 9: Registration and Checkout</span></span>
====================
<span data-ttu-id="281eb-105">par [Jon Galloway](https://github.com/jongalloway)</span><span class="sxs-lookup"><span data-stu-id="281eb-105">by [Jon Galloway](https://github.com/jongalloway)</span></span>

> <span data-ttu-id="281eb-106">Le magasin de musique MVC est une application du didacticiel qui présente et explique étape par étape comment utiliser ASP.NET MVC et Visual Studio pour le développement web.</span><span class="sxs-lookup"><span data-stu-id="281eb-106">The MVC Music Store is a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio for web development.</span></span>  
>   
> <span data-ttu-id="281eb-107">Le magasin de musique MVC est une implémentation de magasin exemple léger qui vend des albums de musique en ligne et implémente l’administration de site de base, authentification de l’utilisateur et les fonctionnalités de panier d’achat.</span><span class="sxs-lookup"><span data-stu-id="281eb-107">The MVC Music Store is a lightweight sample store implementation which sells music albums online, and implements basic site administration, user sign-in, and shopping cart functionality.</span></span>  
>   
> <span data-ttu-id="281eb-108">Cette série de didacticiels détaille toutes les mesures prises pour générer l’exemple d’application de magasin de musique ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="281eb-108">This tutorial series details all of the steps taken to build the ASP.NET MVC Music Store sample application.</span></span> <span data-ttu-id="281eb-109">Partie 9 couvre l’inscription et l’extraction.</span><span class="sxs-lookup"><span data-stu-id="281eb-109">Part 9 covers Registration and Checkout.</span></span>


<span data-ttu-id="281eb-110">Dans cette section, nous allons créer un CheckoutController qui collectera les adresses du client et les informations de paiement.</span><span class="sxs-lookup"><span data-stu-id="281eb-110">In this section, we will be creating a CheckoutController which will collect the shopper's address and payment information.</span></span> <span data-ttu-id="281eb-111">Nous oblige les utilisateurs à inscrire avec notre site avant la récupération, donc ce contrôleur nécessite une autorisation.</span><span class="sxs-lookup"><span data-stu-id="281eb-111">We will require users to register with our site prior to checking out, so this controller will require authorization.</span></span>

<span data-ttu-id="281eb-112">Les utilisateurs pour accéder à la caisse à partir de leur panier d’achat en cliquant sur le bouton « Valider ».</span><span class="sxs-lookup"><span data-stu-id="281eb-112">Users will navigate to the checkout process from their shopping cart by clicking the "Checkout" button.</span></span>

![](mvc-music-store-part-9/_static/image1.jpg)

<span data-ttu-id="281eb-113">Si l’utilisateur n’est pas connecté, il seront invité à.</span><span class="sxs-lookup"><span data-stu-id="281eb-113">If the user is not logged in, they will be prompted to.</span></span>

![](mvc-music-store-part-9/_static/image1.png)

<span data-ttu-id="281eb-114">Lors de la connexion réussite, l’utilisateur s’affiche alors la vue de l’adresse et de paiement.</span><span class="sxs-lookup"><span data-stu-id="281eb-114">Upon successful login, the user is then shown the Address and Payment view.</span></span>

![](mvc-music-store-part-9/_static/image2.png)

<span data-ttu-id="281eb-115">Une fois qu’ils ont rempli le formulaire et l’ordre, elles sont affichées à l’écran de confirmation de commande.</span><span class="sxs-lookup"><span data-stu-id="281eb-115">Once they have filled the form and submitted the order, they will be shown the order confirmation screen.</span></span>

![](mvc-music-store-part-9/_static/image3.png)

<span data-ttu-id="281eb-116">Essayez d’afficher une commande inexistant ou une commande qui n’appartient pas à vous affiche la vue de l’erreur.</span><span class="sxs-lookup"><span data-stu-id="281eb-116">Attempting to view either a non-existent order or an order that doesn't belong to you will show the Error view.</span></span>

![](mvc-music-store-part-9/_static/image4.png)

## <a name="migrating-the-shopping-cart"></a><span data-ttu-id="281eb-117">Migration du panier d’achat</span><span class="sxs-lookup"><span data-stu-id="281eb-117">Migrating the Shopping Cart</span></span>

<span data-ttu-id="281eb-118">Pendant le processus d’achat est anonyme, lorsque l’utilisateur clique sur le bouton d’extraction, ils seront requises pour l’inscription et connexion.</span><span class="sxs-lookup"><span data-stu-id="281eb-118">While the shopping process is anonymous, when the user clicks on the Checkout button, they will be required to register and login.</span></span> <span data-ttu-id="281eb-119">Utilisateurs attendent que nous met à jour leurs informations de panier d’achat entre les visites, donc nous devrons à associer les informations du panier d’un utilisateur lorsqu’ils suivent l’inscription ou la connexion.</span><span class="sxs-lookup"><span data-stu-id="281eb-119">Users will expect that we will maintain their shopping cart information between visits, so we will need to associate the shopping cart information with a user when they complete registration or login.</span></span>

<span data-ttu-id="281eb-120">Il s’agit en fait très simple à faire, comme notre classe ShoppingCart possède déjà une méthode qui associe tous les éléments dans le panier d’achat actuelle avec un nom d’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="281eb-120">This is actually very simple to do, as our ShoppingCart class already has a method which will associate all the items in the current cart with a username.</span></span> <span data-ttu-id="281eb-121">Vous devrez simplement appeler cette méthode lorsqu’un utilisateur a terminé l’inscription ou la connexion.</span><span class="sxs-lookup"><span data-stu-id="281eb-121">We will just need to call this method when a user completes registration or login.</span></span>

<span data-ttu-id="281eb-122">Ouvrez le **AccountController** classe que nous avons ajoutées lorsque nous étions vous configurez l’appartenance et l’autorisation.</span><span class="sxs-lookup"><span data-stu-id="281eb-122">Open the **AccountController** class that we added when we were setting up Membership and Authorization.</span></span> <span data-ttu-id="281eb-123">Ajouter un à l’aide d’instruction faisant référence à MvcMusicStore.Models, puis ajoutez la méthode MigrateShoppingCart suivante :</span><span class="sxs-lookup"><span data-stu-id="281eb-123">Add a using statement referencing MvcMusicStore.Models, then add the following MigrateShoppingCart method:</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample1.cs)]

<span data-ttu-id="281eb-124">Ensuite, modifiez l’action d’ouverture de session pour appeler MigrateShoppingCart après la validation de l’utilisateur, comme indiqué ci-dessous :</span><span class="sxs-lookup"><span data-stu-id="281eb-124">Next, modify the LogOn post action to call MigrateShoppingCart after the user has been validated, as shown below:</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample2.cs)]

<span data-ttu-id="281eb-125">Apporter la même modification au Registre de valider l’action, immédiatement après que le compte d’utilisateur est créé avec succès :</span><span class="sxs-lookup"><span data-stu-id="281eb-125">Make the same change to the Register post action, immediately after the user account is successfully created:</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample3.cs)]

<span data-ttu-id="281eb-126">C’est tout - maintenant un panier d’achat anonyme est automatiquement transférée à un compte d’utilisateur après la connexion ou l’inscription réussie.</span><span class="sxs-lookup"><span data-stu-id="281eb-126">That's it - now an anonymous shopping cart will be automatically transferred to a user account upon successful registration or login.</span></span>

## <a name="creating-the-checkoutcontroller"></a><span data-ttu-id="281eb-127">Création de la CheckoutController</span><span class="sxs-lookup"><span data-stu-id="281eb-127">Creating the CheckoutController</span></span>

<span data-ttu-id="281eb-128">Avec le bouton droit sur le dossier contrôleurs et ajoutez un nouveau contrôleur au projet nommé CheckoutController à l’aide du modèle de contrôleur vide.</span><span class="sxs-lookup"><span data-stu-id="281eb-128">Right-click on the Controllers folder and add a new Controller to the project named CheckoutController using the Empty controller template.</span></span>

![](mvc-music-store-part-9/_static/image5.png)

<span data-ttu-id="281eb-129">Tout d’abord, ajoutez l’attribut Authorize au-dessus de la déclaration de classe de contrôleur pour obliger les utilisateurs à inscrire avant l’extraction :</span><span class="sxs-lookup"><span data-stu-id="281eb-129">First, add the Authorize attribute above the Controller class declaration to require users to register before checkout:</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample4.cs)]

<span data-ttu-id="281eb-130">*Remarque : Ceci est similaire à la modification que nous avons effectuées précédemment pour le StoreManagerController, mais dans ce cas l’attribut Authorize nécessaire que l’utilisateur soit dans un rôle d’administrateur. Dans le contrôleur d’extraction, nous allons nécessitant l’utilisateur est connecté, mais ne sont pas exiger qu’ils soient des administrateurs.*</span><span class="sxs-lookup"><span data-stu-id="281eb-130">*Note: This is similar to the change we previously made to the StoreManagerController, but in that case the Authorize attribute required that the user be in an Administrator role. In the Checkout Controller, we're requiring the user be logged in but aren't requiring that they be administrators.*</span></span>

<span data-ttu-id="281eb-131">Par souci de simplicité, nous ne sera pas gérer les informations de paiement dans ce didacticiel.</span><span class="sxs-lookup"><span data-stu-id="281eb-131">For the sake of simplicity, we won't be dealing with payment information in this tutorial.</span></span> <span data-ttu-id="281eb-132">Au lieu de cela, nous autorisons les utilisateurs à extraire à l’aide d’un code promotionnel.</span><span class="sxs-lookup"><span data-stu-id="281eb-132">Instead, we are allowing users to check out using a promotional code.</span></span> <span data-ttu-id="281eb-133">Nous allons stocker ce code promotionnel à l’aide d’une constante nommée du PromoCode.</span><span class="sxs-lookup"><span data-stu-id="281eb-133">We will store this promotional code using a constant named PromoCode.</span></span>

<span data-ttu-id="281eb-134">Comme dans le StoreController, nous allons déclarer un champ pour stocker une instance de la classe MusicStoreEntities, nommée storeDB.</span><span class="sxs-lookup"><span data-stu-id="281eb-134">As in the StoreController, we'll declare a field to hold an instance of the MusicStoreEntities class, named storeDB.</span></span> <span data-ttu-id="281eb-135">Pour rendre utilisent la classe MusicStoreEntities, nous devons ajouter une à l’aide de l’instruction pour l’espace de noms MvcMusicStore.Models.</span><span class="sxs-lookup"><span data-stu-id="281eb-135">In order to make use of the MusicStoreEntities class, we will need to add a using statement for the MvcMusicStore.Models namespace.</span></span> <span data-ttu-id="281eb-136">La partie supérieure de notre contrôleur retrait s’affiche ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="281eb-136">The top of our Checkout controller appears below.</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample5.cs)]

<span data-ttu-id="281eb-137">Le CheckoutController aura les actions de contrôleur suivantes :</span><span class="sxs-lookup"><span data-stu-id="281eb-137">The CheckoutController will have the following controller actions:</span></span>

<span data-ttu-id="281eb-138">**AddressAndPayment (méthode GET)** affiche un formulaire pour permettre à l’utilisateur à entrer leurs informations.</span><span class="sxs-lookup"><span data-stu-id="281eb-138">**AddressAndPayment (GET method)** will display a form to allow the user to enter their information.</span></span>

<span data-ttu-id="281eb-139">**AddressAndPayment (méthode POST)** valide l’entrée et traiter la commande.</span><span class="sxs-lookup"><span data-stu-id="281eb-139">**AddressAndPayment (POST method)** will validate the input and process the order.</span></span>

<span data-ttu-id="281eb-140">**Complète** s’affichera une fois que l’utilisateur a terminé avec succès le processus de validation.</span><span class="sxs-lookup"><span data-stu-id="281eb-140">**Complete** will be shown after a user has successfully finished the checkout process.</span></span> <span data-ttu-id="281eb-141">Cette vue doit contenir le numéro de commande de l’utilisateur, confirmation.</span><span class="sxs-lookup"><span data-stu-id="281eb-141">This view will include the user's order number, as confirmation.</span></span>

<span data-ttu-id="281eb-142">Tout d’abord, nous allons renommer l’action de contrôleur d’Index (qui a été générée lorsque nous avons créé le contrôleur) à AddressAndPayment.</span><span class="sxs-lookup"><span data-stu-id="281eb-142">First, let's rename the Index controller action (which was generated when we created the controller) to AddressAndPayment.</span></span> <span data-ttu-id="281eb-143">Cette action de contrôleur affiche simplement du formulaire de commande, il est inutile de toutes les informations de modèle.</span><span class="sxs-lookup"><span data-stu-id="281eb-143">This controller action just displays the checkout form, so it doesn't require any model information.</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample6.cs)]

<span data-ttu-id="281eb-144">Notre méthode AddressAndPayment POST suivront le même modèle que nous avons utilisés dans le StoreManagerController : il essaiera d’accepter l’envoi du formulaire et terminer la commande et ré-affiche le formulaire en cas d’échec.</span><span class="sxs-lookup"><span data-stu-id="281eb-144">Our AddressAndPayment POST method will follow the same pattern we used in the StoreManagerController: it will try to accept the form submission and complete the order, and will re-display the form if it fails.</span></span>

<span data-ttu-id="281eb-145">Une fois la validation de l’entrée du formulaire répond à nos exigences de validation d’une commande, nous allons vérifier la valeur du formulaire du PromoCode directement.</span><span class="sxs-lookup"><span data-stu-id="281eb-145">After validating the form input meets our validation requirements for an Order, we will check the PromoCode form value directly.</span></span> <span data-ttu-id="281eb-146">En supposant que tout est correct, que nous allons enregistrer les informations mises à jour avec l’ordre, indiquer à l’objet ShoppingCart pour terminer le processus de commande et rediriger vers l’action terminée.</span><span class="sxs-lookup"><span data-stu-id="281eb-146">Assuming everything is correct, we will save the updated information with the order, tell the ShoppingCart object to complete the order process, and redirect to the Complete action.</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample7.cs)]

<span data-ttu-id="281eb-147">En cas de réussite du processus d’extraction, les utilisateurs s’affichera à l’action de contrôleur complète.</span><span class="sxs-lookup"><span data-stu-id="281eb-147">Upon successful completion of the checkout process, users will be redirected to the Complete controller action.</span></span> <span data-ttu-id="281eb-148">Cette action effectue une vérification simple pour valider que la commande appartient bien à l’utilisateur connecté avant d’afficher le numéro de commande comme une confirmation.</span><span class="sxs-lookup"><span data-stu-id="281eb-148">This action will perform a simple check to validate that the order does indeed belong to the logged-in user before showing the order number as a confirmation.</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample8.cs)]

<span data-ttu-id="281eb-149">*Remarque : La vue erreur créée automatiquement pour nous dans le dossier /Views/Shared lorsque nous avons commencé le projet.*</span><span class="sxs-lookup"><span data-stu-id="281eb-149">*Note: The Error view was automatically created for us in the /Views/Shared folder when we began the project.*</span></span>

<span data-ttu-id="281eb-150">Le code CheckoutController complet est comme suit :</span><span class="sxs-lookup"><span data-stu-id="281eb-150">The complete CheckoutController code is as follows:</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample9.cs)]

## <a name="adding-the-addressandpayment-view"></a><span data-ttu-id="281eb-151">Ajout de la vue AddressAndPayment</span><span class="sxs-lookup"><span data-stu-id="281eb-151">Adding the AddressAndPayment view</span></span>

<span data-ttu-id="281eb-152">Maintenant, nous allons créer la vue AddressAndPayment.</span><span class="sxs-lookup"><span data-stu-id="281eb-152">Now, let's create the AddressAndPayment view.</span></span> <span data-ttu-id="281eb-153">Avec le bouton droit sur une des actions de contrôleur AddressAndPayment et ajouter une vue nommée AddressAndPayment qui est fortement typé comme une commande et utilise le modèle de modification, comme indiqué ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="281eb-153">Right-click on one of the AddressAndPayment controller actions and add a view named AddressAndPayment which is strongly typed as an Order and uses the Edit template, as shown below.</span></span>

![](mvc-music-store-part-9/_static/image6.png)

<span data-ttu-id="281eb-154">Cette vue fera utilise deux techniques que nous avons examiné pendant la génération de la vue StoreManagerEdit :</span><span class="sxs-lookup"><span data-stu-id="281eb-154">This view will make use of two of the techniques we looked at while building the StoreManagerEdit view:</span></span>

- <span data-ttu-id="281eb-155">Nous allons utiliser Html.EditorForModel() pour afficher les champs de formulaire pour le modèle de commande</span><span class="sxs-lookup"><span data-stu-id="281eb-155">We will use Html.EditorForModel() to display form fields for the Order model</span></span>
- <span data-ttu-id="281eb-156">Nous reposera sur les règles de validation à l’aide d’une classe de commande avec les attributs de validation</span><span class="sxs-lookup"><span data-stu-id="281eb-156">We will leverage validation rules using an Order class with validation attributes</span></span>

<span data-ttu-id="281eb-157">Nous allons commencer en mettant à jour le code du formulaire pour utiliser Html.EditorForModel(), suivi d’une zone de texte supplémentaire pour le Code de promotion.</span><span class="sxs-lookup"><span data-stu-id="281eb-157">We'll start by updating the form code to use Html.EditorForModel(), followed by an additional textbox for the Promo Code.</span></span> <span data-ttu-id="281eb-158">Le code complet pour la vue AddressAndPayment est indiqué ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="281eb-158">The complete code for the AddressAndPayment view is shown below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-9/samples/sample10.cshtml)]

## <a name="defining-validation-rules-for-the-order"></a><span data-ttu-id="281eb-159">Définition des règles de validation de la commande</span><span class="sxs-lookup"><span data-stu-id="281eb-159">Defining validation rules for the Order</span></span>

<span data-ttu-id="281eb-160">Maintenant que notre vue est définie, nous allons définir des règles de validation de notre modèle de commande comme nous l’avons fait précédemment pour le modèle de l’Album.</span><span class="sxs-lookup"><span data-stu-id="281eb-160">Now that our view is set up, we will set up the validation rules for our Order model as we did previously for the Album model.</span></span> <span data-ttu-id="281eb-161">Avec le bouton droit sur le dossier de modèles et ajoutez une classe nommée ordre.</span><span class="sxs-lookup"><span data-stu-id="281eb-161">Right-click on the Models folder and add a class named Order.</span></span> <span data-ttu-id="281eb-162">Outre les attributs de validation que nous avons utilisé précédemment pour l’Album, nous sera également utiliser une Expression régulière pour valider l’adresse de messagerie de l’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="281eb-162">In addition to the validation attributes we used previously for the Album, we will also be using a Regular Expression to validate the user's email address.</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample11.cs)]

<span data-ttu-id="281eb-163">Essayez d’envoyer le formulaire avec manquant ou informations non valides maintenant message d’erreur apparaît à l’aide de la validation côté client.</span><span class="sxs-lookup"><span data-stu-id="281eb-163">Attempting to submit the form with missing or invalid information will now show error message using client-side validation.</span></span>

![](mvc-music-store-part-9/_static/image7.png)

<span data-ttu-id="281eb-164">OK, nous avons fait plus partie du travail pour le processus de validation ; Nous avons simplement quelques extrémités d’et probabilités pour terminer.</span><span class="sxs-lookup"><span data-stu-id="281eb-164">Okay, we've done most of the hard work for the checkout process; we just have a few odds and ends to finish.</span></span> <span data-ttu-id="281eb-165">Nous devons ajouter deux vues simples, et nous avons besoin prendre en charge de la procédure de transfert des informations d’achat pendant le processus de connexion.</span><span class="sxs-lookup"><span data-stu-id="281eb-165">We need to add two simple views, and we need to take care of the handoff of the cart information during the login process.</span></span>

## <a name="adding-the-checkout-complete-view"></a><span data-ttu-id="281eb-166">Ajout de la vue complète de l’extraction</span><span class="sxs-lookup"><span data-stu-id="281eb-166">Adding the Checkout Complete view</span></span>

<span data-ttu-id="281eb-167">La vue complète de l’extraction est assez simple, car il a juste besoin d’afficher l’ID de commande.</span><span class="sxs-lookup"><span data-stu-id="281eb-167">The Checkout Complete view is pretty simple, as it just needs to display the Order ID.</span></span> <span data-ttu-id="281eb-168">Avec le bouton droit sur l’action de contrôleur complète et ajouter une vue nommée complète qui est fortement typé comme un entier.</span><span class="sxs-lookup"><span data-stu-id="281eb-168">Right-click on the Complete controller action and add a view named Complete which is strongly typed as an int.</span></span>

![](mvc-music-store-part-9/_static/image8.png)

<span data-ttu-id="281eb-169">Maintenant nous mettrons à jour le code de la vue pour afficher l’ID de commande, comme indiqué ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="281eb-169">Now we will update the view code to display the Order ID, as shown below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-9/samples/sample12.cshtml)]

## <a name="updating-the-error-view"></a><span data-ttu-id="281eb-170">Mise à jour de la vue de l’erreur</span><span class="sxs-lookup"><span data-stu-id="281eb-170">Updating The Error view</span></span>

<span data-ttu-id="281eb-171">Le modèle par défaut inclut une vue de l’erreur dans le dossier vues partagé afin qu’il puisse être réutilisé ailleurs dans le site.</span><span class="sxs-lookup"><span data-stu-id="281eb-171">The default template includes an Error view in the Shared views folder so that it can be re-used elsewhere in the site.</span></span> <span data-ttu-id="281eb-172">Cet affichage de l’erreur contient une erreur très simple et n’utilise pas notre site mise en page, donc nous allons mettre à jour.</span><span class="sxs-lookup"><span data-stu-id="281eb-172">This Error view contains a very simple error and doesn't use our site Layout, so we'll update it.</span></span>

<span data-ttu-id="281eb-173">Dans la mesure où il s’agit d’une page d’erreur générique, le contenu est très simple.</span><span class="sxs-lookup"><span data-stu-id="281eb-173">Since this is a generic error page, the content is very simple.</span></span> <span data-ttu-id="281eb-174">Nous allons inclure un message et un lien pour accéder à la page précédente dans l’historique si l’utilisateur souhaite retenter leur action.</span><span class="sxs-lookup"><span data-stu-id="281eb-174">We'll include a message and a link to navigate to the previous page in history if the user wants to re-try their action.</span></span>

[!code-cshtml[Main](mvc-music-store-part-9/samples/sample13.cshtml)]


>[!div class="step-by-step"]
<span data-ttu-id="281eb-175">[Précédent](mvc-music-store-part-8.md)
[Suivant](mvc-music-store-part-10.md)</span><span class="sxs-lookup"><span data-stu-id="281eb-175">[Previous](mvc-music-store-part-8.md)
[Next](mvc-music-store-part-10.md)</span></span>
