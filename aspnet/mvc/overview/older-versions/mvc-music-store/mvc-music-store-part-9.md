---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-9
title: 'Partie 9 : Inscription et validation | Microsoft Docs'
author: jongalloway
description: Cette série de didacticiels décrit en détail les étapes prises pour générer l’exemple d’application ASP.NET MVC Music Store. Partie 9 couvre l’inscription et l’extraction.
ms.author: riande
ms.date: 04/21/2011
ms.assetid: d65c5c2b-a039-463f-ad29-25cf9fb7a1ba
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-9
msc.type: authoredcontent
ms.openlocfilehash: b3babf1d935774b0ef93d6ab02c8b295998f8afc
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41833459"
---
<a name="part-9-registration-and-checkout"></a><span data-ttu-id="4fe09-104">Partie 9 : Inscription et validation</span><span class="sxs-lookup"><span data-stu-id="4fe09-104">Part 9: Registration and Checkout</span></span>
====================
<span data-ttu-id="4fe09-105">par [Jon Galloway](https://github.com/jongalloway)</span><span class="sxs-lookup"><span data-stu-id="4fe09-105">by [Jon Galloway](https://github.com/jongalloway)</span></span>

> <span data-ttu-id="4fe09-106">Le Store de musique MVC est une application du didacticiel qui présente et explique étape par étape comment utiliser ASP.NET MVC et Visual Studio pour le développement web.</span><span class="sxs-lookup"><span data-stu-id="4fe09-106">The MVC Music Store is a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio for web development.</span></span>  
>   
> <span data-ttu-id="4fe09-107">Le Store de musique MVC est une implémentation de magasin d’exemple léger qui vend des albums de musique en ligne et implémente l’administration de site de base, connexion de l’utilisateur et les fonctionnalités de panier d’achat.</span><span class="sxs-lookup"><span data-stu-id="4fe09-107">The MVC Music Store is a lightweight sample store implementation which sells music albums online, and implements basic site administration, user sign-in, and shopping cart functionality.</span></span>  
>   
> <span data-ttu-id="4fe09-108">Cette série de didacticiels décrit en détail les étapes prises pour générer l’exemple d’application ASP.NET MVC Music Store.</span><span class="sxs-lookup"><span data-stu-id="4fe09-108">This tutorial series details all of the steps taken to build the ASP.NET MVC Music Store sample application.</span></span> <span data-ttu-id="4fe09-109">Partie 9 couvre l’inscription et l’extraction.</span><span class="sxs-lookup"><span data-stu-id="4fe09-109">Part 9 covers Registration and Checkout.</span></span>


<span data-ttu-id="4fe09-110">Dans cette section, nous allons créer un CheckoutController qui recueillera les adresses du client et les informations de paiement.</span><span class="sxs-lookup"><span data-stu-id="4fe09-110">In this section, we will be creating a CheckoutController which will collect the shopper's address and payment information.</span></span> <span data-ttu-id="4fe09-111">Nous oblige les utilisateurs à inscrire auprès de notre site avant la récupération, donc ce contrôleur nécessite une autorisation.</span><span class="sxs-lookup"><span data-stu-id="4fe09-111">We will require users to register with our site prior to checking out, so this controller will require authorization.</span></span>

<span data-ttu-id="4fe09-112">Les utilisateurs pour accéder à la caisse à partir de leur panier d’achat en cliquant sur le bouton « Valider ».</span><span class="sxs-lookup"><span data-stu-id="4fe09-112">Users will navigate to the checkout process from their shopping cart by clicking the "Checkout" button.</span></span>

![](mvc-music-store-part-9/_static/image1.jpg)

<span data-ttu-id="4fe09-113">Si l’utilisateur n’est pas connecté, il seront invité à.</span><span class="sxs-lookup"><span data-stu-id="4fe09-113">If the user is not logged in, they will be prompted to.</span></span>

![](mvc-music-store-part-9/_static/image1.png)

<span data-ttu-id="4fe09-114">Une fois connectés, l’utilisateur voit ensuite la vue de l’adresse et de paiement.</span><span class="sxs-lookup"><span data-stu-id="4fe09-114">Upon successful login, the user is then shown the Address and Payment view.</span></span>

![](mvc-music-store-part-9/_static/image2.png)

<span data-ttu-id="4fe09-115">Une fois qu’ils ont rempli le formulaire et l’ordre, ils seront affichera l’écran de confirmation de commande.</span><span class="sxs-lookup"><span data-stu-id="4fe09-115">Once they have filled the form and submitted the order, they will be shown the order confirmation screen.</span></span>

![](mvc-music-store-part-9/_static/image3.png)

<span data-ttu-id="4fe09-116">Essayez d’afficher une commande inexistant ou une commande qui n’appartient pas à vous affichera la vue de l’erreur.</span><span class="sxs-lookup"><span data-stu-id="4fe09-116">Attempting to view either a non-existent order or an order that doesn't belong to you will show the Error view.</span></span>

![](mvc-music-store-part-9/_static/image4.png)

## <a name="migrating-the-shopping-cart"></a><span data-ttu-id="4fe09-117">Migrer le panier d’achat</span><span class="sxs-lookup"><span data-stu-id="4fe09-117">Migrating the Shopping Cart</span></span>

<span data-ttu-id="4fe09-118">Pendant le processus d’achat est anonyme, lorsque l’utilisateur clique sur le bouton de validation, ils seront requises pour l’inscription et connexion.</span><span class="sxs-lookup"><span data-stu-id="4fe09-118">While the shopping process is anonymous, when the user clicks on the Checkout button, they will be required to register and login.</span></span> <span data-ttu-id="4fe09-119">Les utilisateurs s’attendent que nous met à jour leurs informations de panier d’achat entre les visites, donc nous devons associer les informations du panier d’un utilisateur lorsqu’elles s’achèvent d’inscription ou de connexion.</span><span class="sxs-lookup"><span data-stu-id="4fe09-119">Users will expect that we will maintain their shopping cart information between visits, so we will need to associate the shopping cart information with a user when they complete registration or login.</span></span>

<span data-ttu-id="4fe09-120">Il s’agit en fait très simple à réaliser, comme notre classe ShoppingCart possède déjà une méthode qui associe tous les éléments dans le panier d’achat en cours avec un nom d’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="4fe09-120">This is actually very simple to do, as our ShoppingCart class already has a method which will associate all the items in the current cart with a username.</span></span> <span data-ttu-id="4fe09-121">Nous devrons simplement appeler cette méthode lorsqu’un utilisateur termine l’inscription ou connexion.</span><span class="sxs-lookup"><span data-stu-id="4fe09-121">We will just need to call this method when a user completes registration or login.</span></span>

<span data-ttu-id="4fe09-122">Ouvrez le **AccountController** classe que nous avons ajouté lorsque nous étions vous configurez l’appartenance et l’autorisation.</span><span class="sxs-lookup"><span data-stu-id="4fe09-122">Open the **AccountController** class that we added when we were setting up Membership and Authorization.</span></span> <span data-ttu-id="4fe09-123">Ajouter un à l’aide d’instruction faisant référence à MvcMusicStore.Models, puis ajoutez la méthode MigrateShoppingCart suivante :</span><span class="sxs-lookup"><span data-stu-id="4fe09-123">Add a using statement referencing MvcMusicStore.Models, then add the following MigrateShoppingCart method:</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample1.cs)]

<span data-ttu-id="4fe09-124">Ensuite, modifiez l’action d’ouverture de session pour appeler MigrateShoppingCart une fois que l’utilisateur a été validé, comme indiqué ci-dessous :</span><span class="sxs-lookup"><span data-stu-id="4fe09-124">Next, modify the LogOn post action to call MigrateShoppingCart after the user has been validated, as shown below:</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample2.cs)]

<span data-ttu-id="4fe09-125">Apporter la même modification au Registre à l’action, de publication immédiatement après que le compte d’utilisateur est créé avec succès :</span><span class="sxs-lookup"><span data-stu-id="4fe09-125">Make the same change to the Register post action, immediately after the user account is successfully created:</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample3.cs)]

<span data-ttu-id="4fe09-126">C’est tout - maintenant un panier d’achat anonyme est automatiquement transférée à un compte d’utilisateur lors de l’inscription réussie ou la connexion.</span><span class="sxs-lookup"><span data-stu-id="4fe09-126">That's it - now an anonymous shopping cart will be automatically transferred to a user account upon successful registration or login.</span></span>

## <a name="creating-the-checkoutcontroller"></a><span data-ttu-id="4fe09-127">Création de la CheckoutController</span><span class="sxs-lookup"><span data-stu-id="4fe09-127">Creating the CheckoutController</span></span>

<span data-ttu-id="4fe09-128">Avec le bouton droit sur le dossier Controllers et ajoutez un nouveau contrôleur au projet nommé CheckoutController en utilisant le modèle de contrôleur vide.</span><span class="sxs-lookup"><span data-stu-id="4fe09-128">Right-click on the Controllers folder and add a new Controller to the project named CheckoutController using the Empty controller template.</span></span>

![](mvc-music-store-part-9/_static/image5.png)

<span data-ttu-id="4fe09-129">Tout d’abord, ajoutez l’attribut Authorize au-dessus de la déclaration de classe de contrôleur pour obliger les utilisateurs à inscrire avant d’extraction :</span><span class="sxs-lookup"><span data-stu-id="4fe09-129">First, add the Authorize attribute above the Controller class declaration to require users to register before checkout:</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample4.cs)]

<span data-ttu-id="4fe09-130">*Remarque : Ceci est similaire à la modification que nous avons apportées précédemment à le StoreManagerController, mais dans ce cas l’attribut Authorize nécessaire que l’utilisateur soit dans un rôle d’administrateur. Dans le contrôleur de validation, nous allons nécessitant l’utilisateur est connecté, mais ne sont pas exiger qu’ils soient des administrateurs.*</span><span class="sxs-lookup"><span data-stu-id="4fe09-130">*Note: This is similar to the change we previously made to the StoreManagerController, but in that case the Authorize attribute required that the user be in an Administrator role. In the Checkout Controller, we're requiring the user be logged in but aren't requiring that they be administrators.*</span></span>

<span data-ttu-id="4fe09-131">Par souci de simplicité, nous ne sont pas affaire à des informations de paiement dans ce didacticiel.</span><span class="sxs-lookup"><span data-stu-id="4fe09-131">For the sake of simplicity, we won't be dealing with payment information in this tutorial.</span></span> <span data-ttu-id="4fe09-132">Au lieu de cela, nous autorisons les utilisateurs à consulter à l’aide d’un code promotionnel.</span><span class="sxs-lookup"><span data-stu-id="4fe09-132">Instead, we are allowing users to check out using a promotional code.</span></span> <span data-ttu-id="4fe09-133">Nous allons stocker ce code promotionnel à l’aide d’une constante nommée code promo.</span><span class="sxs-lookup"><span data-stu-id="4fe09-133">We will store this promotional code using a constant named PromoCode.</span></span>

<span data-ttu-id="4fe09-134">Comme dans le StoreController, nous allons déclarer un champ devant contenir une instance de la classe MusicStoreEntities, nommée storeDB.</span><span class="sxs-lookup"><span data-stu-id="4fe09-134">As in the StoreController, we'll declare a field to hold an instance of the MusicStoreEntities class, named storeDB.</span></span> <span data-ttu-id="4fe09-135">Afin de rendre utilisation de la classe MusicStoreEntities, nous devons ajouter une à l’aide de l’instruction pour l’espace de noms MvcMusicStore.Models.</span><span class="sxs-lookup"><span data-stu-id="4fe09-135">In order to make use of the MusicStoreEntities class, we will need to add a using statement for the MvcMusicStore.Models namespace.</span></span> <span data-ttu-id="4fe09-136">La partie supérieure de notre contrôleur Checkout apparaît ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="4fe09-136">The top of our Checkout controller appears below.</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample5.cs)]

<span data-ttu-id="4fe09-137">Le CheckoutController aura les actions de contrôleur suivantes :</span><span class="sxs-lookup"><span data-stu-id="4fe09-137">The CheckoutController will have the following controller actions:</span></span>

<span data-ttu-id="4fe09-138">**AddressAndPayment (méthode GET)** affiche un formulaire pour autoriser l’utilisateur à entrer leurs informations.</span><span class="sxs-lookup"><span data-stu-id="4fe09-138">**AddressAndPayment (GET method)** will display a form to allow the user to enter their information.</span></span>

<span data-ttu-id="4fe09-139">**AddressAndPayment (méthode POST)** valider l’entrée et de traiter la commande.</span><span class="sxs-lookup"><span data-stu-id="4fe09-139">**AddressAndPayment (POST method)** will validate the input and process the order.</span></span>

<span data-ttu-id="4fe09-140">**Complète** s’affichera une fois un utilisateur a terminé avec succès le processus de validation.</span><span class="sxs-lookup"><span data-stu-id="4fe09-140">**Complete** will be shown after a user has successfully finished the checkout process.</span></span> <span data-ttu-id="4fe09-141">Cet affichage inclura le numéro de commande de l’utilisateur, en tant que confirmation.</span><span class="sxs-lookup"><span data-stu-id="4fe09-141">This view will include the user's order number, as confirmation.</span></span>

<span data-ttu-id="4fe09-142">Tout d’abord, nous allons renommer l’action de contrôleur d’Index (qui a été générée lorsque nous avons créé le contrôleur) à AddressAndPayment.</span><span class="sxs-lookup"><span data-stu-id="4fe09-142">First, let's rename the Index controller action (which was generated when we created the controller) to AddressAndPayment.</span></span> <span data-ttu-id="4fe09-143">Cette action de contrôleur affiche simplement le formulaire d’extraction, afin qu’il ne nécessite des informations sur le modèle.</span><span class="sxs-lookup"><span data-stu-id="4fe09-143">This controller action just displays the checkout form, so it doesn't require any model information.</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample6.cs)]

<span data-ttu-id="4fe09-144">Notre méthode AddressAndPayment POST suivra le même modèle que nous avons utilisé dans le StoreManagerController : il essaiera d’accepter l’envoi du formulaire et terminer la commande et ré-affiche le formulaire en cas d’échec.</span><span class="sxs-lookup"><span data-stu-id="4fe09-144">Our AddressAndPayment POST method will follow the same pattern we used in the StoreManagerController: it will try to accept the form submission and complete the order, and will re-display the form if it fails.</span></span>

<span data-ttu-id="4fe09-145">Une fois la validation de l’entrée de formulaire répond à nos exigences de validation d’une commande, nous allons vérifier la valeur de formulaire code promo directement.</span><span class="sxs-lookup"><span data-stu-id="4fe09-145">After validating the form input meets our validation requirements for an Order, we will check the PromoCode form value directly.</span></span> <span data-ttu-id="4fe09-146">En supposant que tout est correct, que nous allons enregistrer les informations mises à jour avec l’ordre, indiquer à l’objet ShoppingCart pour terminer le processus de commande et rediriger vers l’action terminée.</span><span class="sxs-lookup"><span data-stu-id="4fe09-146">Assuming everything is correct, we will save the updated information with the order, tell the ShoppingCart object to complete the order process, and redirect to the Complete action.</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample7.cs)]

<span data-ttu-id="4fe09-147">En cas de réussite du processus d’extraction, les utilisateurs seront redirigés vers l’action de contrôleur complète.</span><span class="sxs-lookup"><span data-stu-id="4fe09-147">Upon successful completion of the checkout process, users will be redirected to the Complete controller action.</span></span> <span data-ttu-id="4fe09-148">Cette action effectue une vérification simple pour valider que la commande appartient bien à l’utilisateur connecté avant d’afficher le numéro de commande en tant qu’une confirmation.</span><span class="sxs-lookup"><span data-stu-id="4fe09-148">This action will perform a simple check to validate that the order does indeed belong to the logged-in user before showing the order number as a confirmation.</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample8.cs)]

<span data-ttu-id="4fe09-149">*Remarque : La vue de l’erreur a été automatiquement créée pour nous dans le dossier /Views/Shared lorsque nous avons commencé le projet.*</span><span class="sxs-lookup"><span data-stu-id="4fe09-149">*Note: The Error view was automatically created for us in the /Views/Shared folder when we began the project.*</span></span>

<span data-ttu-id="4fe09-150">Le code CheckoutController complet est comme suit :</span><span class="sxs-lookup"><span data-stu-id="4fe09-150">The complete CheckoutController code is as follows:</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample9.cs)]

## <a name="adding-the-addressandpayment-view"></a><span data-ttu-id="4fe09-151">Ajout de la vue AddressAndPayment</span><span class="sxs-lookup"><span data-stu-id="4fe09-151">Adding the AddressAndPayment view</span></span>

<span data-ttu-id="4fe09-152">Maintenant, nous allons créer la vue AddressAndPayment.</span><span class="sxs-lookup"><span data-stu-id="4fe09-152">Now, let's create the AddressAndPayment view.</span></span> <span data-ttu-id="4fe09-153">Avec le bouton droit sur l’une des actions de contrôleur AddressAndPayment et ajouter une vue nommée AddressAndPayment qui est fortement typée en tant que commande et utilise le modèle de modification, comme indiqué ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="4fe09-153">Right-click on one of the AddressAndPayment controller actions and add a view named AddressAndPayment which is strongly typed as an Order and uses the Edit template, as shown below.</span></span>

![](mvc-music-store-part-9/_static/image6.png)

<span data-ttu-id="4fe09-154">Cette vue rendra utilise deux des techniques que nous avons examiné lors de la création de la vue StoreManagerEdit :</span><span class="sxs-lookup"><span data-stu-id="4fe09-154">This view will make use of two of the techniques we looked at while building the StoreManagerEdit view:</span></span>

- <span data-ttu-id="4fe09-155">Nous allons utiliser Html.EditorForModel() pour afficher les champs pour le modèle de commande</span><span class="sxs-lookup"><span data-stu-id="4fe09-155">We will use Html.EditorForModel() to display form fields for the Order model</span></span>
- <span data-ttu-id="4fe09-156">Nous allons tirer parti des règles de validation à l’aide d’une classe de commande avec les attributs de validation</span><span class="sxs-lookup"><span data-stu-id="4fe09-156">We will leverage validation rules using an Order class with validation attributes</span></span>

<span data-ttu-id="4fe09-157">Nous allons commencer en mettant à jour le code de formulaire pour utiliser Html.EditorForModel(), suivi d’une zone de texte supplémentaire pour le Code de promotion.</span><span class="sxs-lookup"><span data-stu-id="4fe09-157">We'll start by updating the form code to use Html.EditorForModel(), followed by an additional textbox for the Promo Code.</span></span> <span data-ttu-id="4fe09-158">Le code complet pour la vue AddressAndPayment est indiqué ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="4fe09-158">The complete code for the AddressAndPayment view is shown below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-9/samples/sample10.cshtml)]

## <a name="defining-validation-rules-for-the-order"></a><span data-ttu-id="4fe09-159">Définition des règles de validation de la commande</span><span class="sxs-lookup"><span data-stu-id="4fe09-159">Defining validation rules for the Order</span></span>

<span data-ttu-id="4fe09-160">Maintenant que notre vue est configuré, nous allons configurer les règles de validation pour notre modèle de commande comme nous l’avons fait précédemment pour le modèle de l’Album.</span><span class="sxs-lookup"><span data-stu-id="4fe09-160">Now that our view is set up, we will set up the validation rules for our Order model as we did previously for the Album model.</span></span> <span data-ttu-id="4fe09-161">Avec le bouton droit sur le dossier Models et ajoutez une classe nommée ordre.</span><span class="sxs-lookup"><span data-stu-id="4fe09-161">Right-click on the Models folder and add a class named Order.</span></span> <span data-ttu-id="4fe09-162">Outre les attributs de validation que nous avons utilisé précédemment pour l’Album, nous allons également utiliser une Expression régulière pour valider l’adresse de messagerie de l’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="4fe09-162">In addition to the validation attributes we used previously for the Album, we will also be using a Regular Expression to validate the user's email address.</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample11.cs)]

<span data-ttu-id="4fe09-163">Essayez d’envoyer le formulaire avec manquant ou des informations non valides s’affichent désormais de message d’erreur à l’aide de la validation côté client.</span><span class="sxs-lookup"><span data-stu-id="4fe09-163">Attempting to submit the form with missing or invalid information will now show error message using client-side validation.</span></span>

![](mvc-music-store-part-9/_static/image7.png)

<span data-ttu-id="4fe09-164">OK, nous l’avons fait plus partie du travail pour le processus de validation ; Nous avons simplement quelques terminaisons and de probabilités pour terminer.</span><span class="sxs-lookup"><span data-stu-id="4fe09-164">Okay, we've done most of the hard work for the checkout process; we just have a few odds and ends to finish.</span></span> <span data-ttu-id="4fe09-165">Nous devons ajouter deux vues simples, et nous avons besoin prendre en charge de la procédure de transfert des informations de panier pendant le processus de connexion.</span><span class="sxs-lookup"><span data-stu-id="4fe09-165">We need to add two simple views, and we need to take care of the handoff of the cart information during the login process.</span></span>

## <a name="adding-the-checkout-complete-view"></a><span data-ttu-id="4fe09-166">Ajout de la vue complète d’extraction</span><span class="sxs-lookup"><span data-stu-id="4fe09-166">Adding the Checkout Complete view</span></span>

<span data-ttu-id="4fe09-167">La vue complète d’extraction est assez simple, car il a juste besoin d’afficher l’ID de commande.</span><span class="sxs-lookup"><span data-stu-id="4fe09-167">The Checkout Complete view is pretty simple, as it just needs to display the Order ID.</span></span> <span data-ttu-id="4fe09-168">Avec le bouton droit sur l’action de contrôleur complète et ajouter une vue nommée complète qui est fortement typée en tant qu’un entier.</span><span class="sxs-lookup"><span data-stu-id="4fe09-168">Right-click on the Complete controller action and add a view named Complete which is strongly typed as an int.</span></span>

![](mvc-music-store-part-9/_static/image8.png)

<span data-ttu-id="4fe09-169">Maintenant nous mettrons à jour le code de vue pour afficher l’ID de commande, comme indiqué ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="4fe09-169">Now we will update the view code to display the Order ID, as shown below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-9/samples/sample12.cshtml)]

## <a name="updating-the-error-view"></a><span data-ttu-id="4fe09-170">Erreur lors de la vue de la mise à jour</span><span class="sxs-lookup"><span data-stu-id="4fe09-170">Updating The Error view</span></span>

<span data-ttu-id="4fe09-171">Le modèle par défaut inclut une vue de l’erreur dans le dossier de vues Shared afin qu’il puisse être réutilisé ailleurs dans le site.</span><span class="sxs-lookup"><span data-stu-id="4fe09-171">The default template includes an Error view in the Shared views folder so that it can be re-used elsewhere in the site.</span></span> <span data-ttu-id="4fe09-172">Cette vue de l’erreur contient une erreur très simple et n’utilise pas notre site de mise en page, donc nous allons mettre à jour.</span><span class="sxs-lookup"><span data-stu-id="4fe09-172">This Error view contains a very simple error and doesn't use our site Layout, so we'll update it.</span></span>

<span data-ttu-id="4fe09-173">Dans la mesure où il s’agit d’une page d’erreur générique, le contenu est très simple.</span><span class="sxs-lookup"><span data-stu-id="4fe09-173">Since this is a generic error page, the content is very simple.</span></span> <span data-ttu-id="4fe09-174">Nous allons inclure un message et un lien pour accéder à la page précédente dans l’historique si l’utilisateur souhaite réessayer leur action.</span><span class="sxs-lookup"><span data-stu-id="4fe09-174">We'll include a message and a link to navigate to the previous page in history if the user wants to re-try their action.</span></span>

[!code-cshtml[Main](mvc-music-store-part-9/samples/sample13.cshtml)]


> [!div class="step-by-step"]
> <span data-ttu-id="4fe09-175">[Précédent](mvc-music-store-part-8.md)
> [Suivant](mvc-music-store-part-10.md)</span><span class="sxs-lookup"><span data-stu-id="4fe09-175">[Previous](mvc-music-store-part-8.md)
[Next](mvc-music-store-part-10.md)</span></span>
