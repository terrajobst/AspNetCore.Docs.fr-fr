---
uid: mvc/overview/older-versions-1/models-data/performing-simple-validation-cs
title: Effectuer une Validation Simple (c#) | Microsoft Docs
author: StephenWalther
description: Découvrez comment effectuer la validation dans une application ASP.NET MVC. Dans ce didacticiel, Stephen Walther présente l’état du modèle et l’application d’assistance de validation HTML...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/02/2009
ms.topic: article
ms.assetid: 21383c9d-6aea-4bad-a99b-b5f2c9d6503f
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions-1/models-data/performing-simple-validation-cs
msc.type: authoredcontent
ms.openlocfilehash: 622d4bc40b2f403c0a4efe0ee799a68f1adbe4fd
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37377413"
---
<a name="performing-simple-validation-c"></a><span data-ttu-id="a2a7a-104">Effectuer une Validation Simple (c#)</span><span class="sxs-lookup"><span data-stu-id="a2a7a-104">Performing Simple Validation (C#)</span></span>
====================
<span data-ttu-id="a2a7a-105">par [Stephen Walther](https://github.com/StephenWalther)</span><span class="sxs-lookup"><span data-stu-id="a2a7a-105">by [Stephen Walther](https://github.com/StephenWalther)</span></span>

> <span data-ttu-id="a2a7a-106">Découvrez comment effectuer la validation dans une application ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="a2a7a-106">Learn how to perform validation in an ASP.NET MVC application.</span></span> <span data-ttu-id="a2a7a-107">Dans ce didacticiel, Stephen Walther présente vous à l’état du modèle et les programmes d’assistance HTML de validation.</span><span class="sxs-lookup"><span data-stu-id="a2a7a-107">In this tutorial, Stephen Walther introduces you to model state and the validation HTML helpers.</span></span>


<span data-ttu-id="a2a7a-108">L’objectif de ce didacticiel est d’expliquer comment vous pouvez effectuer la validation au sein d’une application ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="a2a7a-108">The goal of this tutorial is to explain how you can perform validation within an ASP.NET MVC application.</span></span> <span data-ttu-id="a2a7a-109">Par exemple, vous allez apprendre à empêcher un utilisateur d’envoyer un formulaire qui ne contient-elle pas une valeur pour un champ obligatoire.</span><span class="sxs-lookup"><span data-stu-id="a2a7a-109">For example, you learn how to prevent someone from submitting a form that does not contain a value for a required field.</span></span> <span data-ttu-id="a2a7a-110">Vous allez apprendre à utiliser l’état du modèle et les programmes d’assistance HTML validation.</span><span class="sxs-lookup"><span data-stu-id="a2a7a-110">You learn how to use model state and the validation HTML helpers.</span></span>

## <a name="understanding-model-state"></a><span data-ttu-id="a2a7a-111">État de modèle de présentation</span><span class="sxs-lookup"><span data-stu-id="a2a7a-111">Understanding Model State</span></span>

<span data-ttu-id="a2a7a-112">Vous utilisez - état du modèle, ou plus précisément, le dictionnaire d’états de modèle - pour représenter les erreurs de validation.</span><span class="sxs-lookup"><span data-stu-id="a2a7a-112">You use model state - or more accurately, the model state dictionary - to represent validation errors.</span></span> <span data-ttu-id="a2a7a-113">Par exemple, l’action Create() dans le Listing 1 valide les propriétés d’une classe de produit avant d’ajouter la classe de produit à une base de données.</span><span class="sxs-lookup"><span data-stu-id="a2a7a-113">For example, the Create() action in Listing 1 validates the properties of a Product class before adding the Product class to a database.</span></span>


<span data-ttu-id="a2a7a-114">Je ne suis pas recommandant que vous ajoutez votre logique de validation ou de la base de données à un contrôleur.</span><span class="sxs-lookup"><span data-stu-id="a2a7a-114">I'm not recommending that you add your validation or database logic to a controller.</span></span> <span data-ttu-id="a2a7a-115">Un contrôleur doit contenir uniquement les logique liée au contrôle de flux d’application.</span><span class="sxs-lookup"><span data-stu-id="a2a7a-115">A controller should contain only logic related to application flow control.</span></span> <span data-ttu-id="a2a7a-116">Nous effectuons un raccourci pour simplifier les choses.</span><span class="sxs-lookup"><span data-stu-id="a2a7a-116">We are taking a shortcut to keep things simple.</span></span>


<span data-ttu-id="a2a7a-117">**Liste 1 - Controllers\ProductController.cs**</span><span class="sxs-lookup"><span data-stu-id="a2a7a-117">**Listing 1 - Controllers\ProductController.cs**</span></span>

[!code-csharp[Main](performing-simple-validation-cs/samples/sample1.cs)]

<span data-ttu-id="a2a7a-118">Dans la liste 1, les propriétés Name, Description et UnitsInStock de la classe de produit sont validées.</span><span class="sxs-lookup"><span data-stu-id="a2a7a-118">In Listing 1, the Name, Description, and UnitsInStock properties of the Product class are validated.</span></span> <span data-ttu-id="a2a7a-119">Si une de ces propriétés échoue un test de validation une erreur est ajoutée au dictionnaire d’états de modèle (représenté par la propriété ModelState de la classe de contrôleur).</span><span class="sxs-lookup"><span data-stu-id="a2a7a-119">If any of these properties fail a validation test then an error is added to the model state dictionary (represented by the ModelState property of the Controller class).</span></span>

<span data-ttu-id="a2a7a-120">S’il existe des erreurs dans l’état du modèle la propriété ModelState.IsValid retourne false.</span><span class="sxs-lookup"><span data-stu-id="a2a7a-120">If there are any errors in model state then the ModelState.IsValid property returns false.</span></span> <span data-ttu-id="a2a7a-121">Dans ce cas, le formulaire HTML pour la création d’un nouveau produit s’affiche de nouveau.</span><span class="sxs-lookup"><span data-stu-id="a2a7a-121">In that case, the HTML form for creating a new product is redisplayed.</span></span> <span data-ttu-id="a2a7a-122">Sinon, s’il n’existe aucune erreur de validation, le nouveau produit est ajouté à la base de données.</span><span class="sxs-lookup"><span data-stu-id="a2a7a-122">Otherwise, if there are no validation errors, the new Product is added to the database.</span></span>

## <a name="using-the-validation-helpers"></a><span data-ttu-id="a2a7a-123">À l’aide des Helpers de Validation</span><span class="sxs-lookup"><span data-stu-id="a2a7a-123">Using the Validation Helpers</span></span>

<span data-ttu-id="a2a7a-124">L’infrastructure ASP.NET MVC inclut les deux programmes d’assistance de validation : l’application d’assistance Html.ValidationMessage() et du programme d’assistance Html.ValidationSummary().</span><span class="sxs-lookup"><span data-stu-id="a2a7a-124">The ASP.NET MVC framework includes two validation helpers: the Html.ValidationMessage() helper and the Html.ValidationSummary() helper.</span></span> <span data-ttu-id="a2a7a-125">Ces deux programmes d’assistance dans une vue vous permet d’afficher des messages d’erreur de validation.</span><span class="sxs-lookup"><span data-stu-id="a2a7a-125">You use these two helpers in a view to display validation error messages.</span></span>

<span data-ttu-id="a2a7a-126">Les programmes d’assistance Html.ValidationMessage() et Html.ValidationSummary() sont utilisés dans les vues Create et Edit qui sont générés automatiquement par la génération de modèles automatique ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="a2a7a-126">The Html.ValidationMessage() and Html.ValidationSummary() helpers are used in the Create and Edit views that are generated automatically by the ASP.NET MVC scaffolding.</span></span> <span data-ttu-id="a2a7a-127">Suivez ces étapes pour générer la vue de créer :</span><span class="sxs-lookup"><span data-stu-id="a2a7a-127">Follow these steps to generate the Create view:</span></span>

1. <span data-ttu-id="a2a7a-128">L’action Create() dans le contrôleur de produit de clic droit et sélectionnez l’option de menu **ajouter une vue** (voir Figure 1).</span><span class="sxs-lookup"><span data-stu-id="a2a7a-128">Right-click the Create() action in the Product controller and select the menu option **Add View** (see Figure 1).</span></span>
2. <span data-ttu-id="a2a7a-129">Dans le **ajouter une vue** boîte de dialogue, cochez la case intitulée **créer une vue fortement typée** (voir Figure 2).</span><span class="sxs-lookup"><span data-stu-id="a2a7a-129">In the **Add View** dialog, check the checkbox labeled **Create a strongly-typed view** (see Figure 2).</span></span>
3. <span data-ttu-id="a2a7a-130">À partir de la **afficher la classe de données** liste déroulante, sélectionnez la classe Product.</span><span class="sxs-lookup"><span data-stu-id="a2a7a-130">From the **View data class** dropdown list, select the Product class.</span></span>
4. <span data-ttu-id="a2a7a-131">À partir de la **afficher le contenu** liste déroulante, sélectionnez Créer.</span><span class="sxs-lookup"><span data-stu-id="a2a7a-131">From the **View content** dropdown list, select Create.</span></span>
5. <span data-ttu-id="a2a7a-132">Cliquez sur le bouton **Ajouter**.</span><span class="sxs-lookup"><span data-stu-id="a2a7a-132">Click the **Add** button.</span></span>


<span data-ttu-id="a2a7a-133">Assurez-vous que vous générez votre application avant d’ajouter une vue.</span><span class="sxs-lookup"><span data-stu-id="a2a7a-133">Make sure that you build your application before adding a view.</span></span> <span data-ttu-id="a2a7a-134">Sinon, la liste des classes n’apparaître pas dans le **afficher la classe de données** liste déroulante.</span><span class="sxs-lookup"><span data-stu-id="a2a7a-134">Otherwise, the list of classes won't appear in the **View data class** dropdown list.</span></span>


<span data-ttu-id="a2a7a-135">[![La boîte de dialogue Nouveau projet](performing-simple-validation-cs/_static/image1.jpg)](performing-simple-validation-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="a2a7a-135">[![The New Project dialog box](performing-simple-validation-cs/_static/image1.jpg)](performing-simple-validation-cs/_static/image1.png)</span></span>

<span data-ttu-id="a2a7a-136">**Figure 01**: ajout d’une vue ([cliquez pour afficher l’image en taille réelle](performing-simple-validation-cs/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="a2a7a-136">**Figure 01**: Adding a view([Click to view full-size image](performing-simple-validation-cs/_static/image2.png))</span></span>


<span data-ttu-id="a2a7a-137">[![La boîte de dialogue Nouveau projet](performing-simple-validation-cs/_static/image2.jpg)](performing-simple-validation-cs/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="a2a7a-137">[![The New Project dialog box](performing-simple-validation-cs/_static/image2.jpg)](performing-simple-validation-cs/_static/image3.png)</span></span>

<span data-ttu-id="a2a7a-138">**Figure 02**: création d’une vue fortement typée ([cliquez pour afficher l’image en taille réelle](performing-simple-validation-cs/_static/image4.png))</span><span class="sxs-lookup"><span data-stu-id="a2a7a-138">**Figure 02**: Creating a strongly-typed view ([Click to view full-size image](performing-simple-validation-cs/_static/image4.png))</span></span>


<span data-ttu-id="a2a7a-139">Après avoir effectué ces étapes, vous obtenez la vue de créer dans le Listing 2.</span><span class="sxs-lookup"><span data-stu-id="a2a7a-139">After you complete these steps, you get the Create view in Listing 2.</span></span>

<span data-ttu-id="a2a7a-140">**Listing 2 - Views\Product\Create.aspx**</span><span class="sxs-lookup"><span data-stu-id="a2a7a-140">**Listing 2 - Views\Product\Create.aspx**</span></span>

[!code-aspx[Main](performing-simple-validation-cs/samples/sample2.aspx)]

<span data-ttu-id="a2a7a-141">Dans la liste 2, l’application d’assistance Html.ValidationSummary() est appelée immédiatement au-dessus du formulaire HTML.</span><span class="sxs-lookup"><span data-stu-id="a2a7a-141">In Listing 2, the Html.ValidationSummary() helper is called immediately above the HTML form.</span></span> <span data-ttu-id="a2a7a-142">Ce programme d’assistance est utilisé pour afficher la liste des messages d’erreur de validation.</span><span class="sxs-lookup"><span data-stu-id="a2a7a-142">This helper is used to display a list of validation error messages.</span></span> <span data-ttu-id="a2a7a-143">Le programme d’assistance Html.ValidationSummary() restitue les erreurs dans une liste à puces.</span><span class="sxs-lookup"><span data-stu-id="a2a7a-143">The Html.ValidationSummary() helper renders the errors in a bulleted list.</span></span>

<span data-ttu-id="a2a7a-144">Le programme d’assistance Html.ValidationMessage() est appelée en regard de chacun des champs de formulaire HTML.</span><span class="sxs-lookup"><span data-stu-id="a2a7a-144">The Html.ValidationMessage() helper is called next to each of the HTML form fields.</span></span> <span data-ttu-id="a2a7a-145">Ce programme d’assistance est utilisé pour afficher un message d’erreur juste à côté d’un champ de formulaire.</span><span class="sxs-lookup"><span data-stu-id="a2a7a-145">This helper is used to display an error message right next to a form field.</span></span> <span data-ttu-id="a2a7a-146">Dans le cas de Listing 2, l’application d’assistance Html.ValidationMessage() affiche un astérisque lorsqu’il existe une erreur.</span><span class="sxs-lookup"><span data-stu-id="a2a7a-146">In the case of Listing 2, the Html.ValidationMessage() helper displays an asterisk when there is an error.</span></span>

<span data-ttu-id="a2a7a-147">La page dans la Figure 3 illustre les messages d’erreur affichés par les programmes d’assistance de validation lorsque le formulaire est envoyé avec les champs manquants et des valeurs non valides.</span><span class="sxs-lookup"><span data-stu-id="a2a7a-147">The page in Figure 3 illustrates the error messages rendered by the validation helpers when the form is submitted with missing fields and invalid values.</span></span>


<span data-ttu-id="a2a7a-148">[![La boîte de dialogue Nouveau projet](performing-simple-validation-cs/_static/image3.jpg)](performing-simple-validation-cs/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="a2a7a-148">[![The New Project dialog box](performing-simple-validation-cs/_static/image3.jpg)](performing-simple-validation-cs/_static/image5.png)</span></span>

<span data-ttu-id="a2a7a-149">**Figure 03**: Create The view, soumise avec des problèmes ([cliquez pour afficher l’image en taille réelle](performing-simple-validation-cs/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="a2a7a-149">**Figure 03**: The Create view submitted with problems ([Click to view full-size image](performing-simple-validation-cs/_static/image6.png))</span></span>


<span data-ttu-id="a2a7a-150">Notez que l’apparence du contenu HTML d’entrée de champs sont également modifiés quand il existe une erreur de validation.</span><span class="sxs-lookup"><span data-stu-id="a2a7a-150">Notice that the appearance of the HTML input fields are also modified when there is a validation error.</span></span> <span data-ttu-id="a2a7a-151">Les convertisseurs d’assistance de Html.TextBox() un *classe = « erreur de validation d’entrée »* attribut lorsqu’il existe une erreur de validation associé à la propriété restituée par l’application d’assistance Html.TextBox().</span><span class="sxs-lookup"><span data-stu-id="a2a7a-151">The Html.TextBox() helper renders a *class="input-validation-error"* attribute when there is a validation error associated with the property rendered by the Html.TextBox() helper.</span></span>

<span data-ttu-id="a2a7a-152">Il existe trois classes de feuille style en cascade utilisées pour contrôler l’apparence des erreurs de validation :</span><span class="sxs-lookup"><span data-stu-id="a2a7a-152">There are three cascading style sheet classes used to control the appearance of validation errors:</span></span>

- <span data-ttu-id="a2a7a-153">entrée--erreur de validation est appliquée à la &lt;d’entrée&gt; balise rendue par Html.TextBox() helper.</span><span class="sxs-lookup"><span data-stu-id="a2a7a-153">input-validation-error - Applied to the &lt;input&gt; tag rendered by Html.TextBox() helper.</span></span>
- <span data-ttu-id="a2a7a-154">champ--erreur de validation est appliquée à la &lt;span&gt; balise rendue par l’application d’assistance Html.ValidationMessage().</span><span class="sxs-lookup"><span data-stu-id="a2a7a-154">field-validation-error - Applied to the &lt;span&gt; tag rendered by the Html.ValidationMessage() helper.</span></span>
- <span data-ttu-id="a2a7a-155">Résumé-erreurs de validation - appliquée à la &lt;ul&gt; balise rendue par l’application d’assistance Html.ValidationSumamry().</span><span class="sxs-lookup"><span data-stu-id="a2a7a-155">validation-summary-errors - Applied to the &lt;ul&gt; tag rendered by the Html.ValidationSumamry() helper.</span></span>

<span data-ttu-id="a2a7a-156">Vous pouvez modifier ces classes de feuille de style en cascade et par conséquent de modifier l’apparence des erreurs de validation, en modifiant le fichier Site.css situé dans le dossier de contenu.</span><span class="sxs-lookup"><span data-stu-id="a2a7a-156">You can modify these cascading style sheet classes, and therefore modify the appearance of the validation errors, by modifying the Site.css file located in the Content folder.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="a2a7a-157">La classe HtmlHelper inclut les propriétés statiques en lecture seule pour récupérer les noms de la validation connexes CSS classes.</span><span class="sxs-lookup"><span data-stu-id="a2a7a-157">The HtmlHelper class includes read-only static properties for retrieving the names of the validation related CSS classes.</span></span> <span data-ttu-id="a2a7a-158">Ces propriétés statiques sont nommées ValidationInputCssClassName, ValidationFieldCssClassName et ValidationSummaryCssClassName.</span><span class="sxs-lookup"><span data-stu-id="a2a7a-158">These static properties are named ValidationInputCssClassName, ValidationFieldCssClassName, and ValidationSummaryCssClassName.</span></span>


## <a name="prebinding-validation-and-postbinding-validation"></a><span data-ttu-id="a2a7a-159">Prebinding Validation et la Validation de Postbinding</span><span class="sxs-lookup"><span data-stu-id="a2a7a-159">Prebinding Validation and Postbinding Validation</span></span>

<span data-ttu-id="a2a7a-160">Si vous envoyez le formulaire HTML pour la création d’un produit et que vous entrez une valeur non valide pour le champ price et aucune valeur pour le champ UnitsInStock, vous obtiendrez les messages de validation affichés dans la Figure 4.</span><span class="sxs-lookup"><span data-stu-id="a2a7a-160">If you submit the HTML form for creating a Product, and you enter an invalid value for the price field and no value for the UnitsInStock field, then you'll get the validation messages displayed in Figure 4.</span></span> <span data-ttu-id="a2a7a-161">D'où proviennent ces messages d’erreur de validation ?</span><span class="sxs-lookup"><span data-stu-id="a2a7a-161">Where do these validation error messages come from?</span></span>


<span data-ttu-id="a2a7a-162">[![La boîte de dialogue Nouveau projet](performing-simple-validation-cs/_static/image4.jpg)](performing-simple-validation-cs/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="a2a7a-162">[![The New Project dialog box](performing-simple-validation-cs/_static/image4.jpg)](performing-simple-validation-cs/_static/image7.png)</span></span>

<span data-ttu-id="a2a7a-163">**Figure 04**: erreurs de Validation Prebinding ([cliquez pour afficher l’image en taille réelle](performing-simple-validation-cs/_static/image8.png))</span><span class="sxs-lookup"><span data-stu-id="a2a7a-163">**Figure 04**: Prebinding Validation Errors([Click to view full-size image](performing-simple-validation-cs/_static/image8.png))</span></span>


<span data-ttu-id="a2a7a-164">Il existe en fait deux types de messages d’erreur de validation - ceux générés avant que les champs de formulaire HTML sont liées à une classe et ceux générés une fois que les champs de formulaire sont liés à la classe.</span><span class="sxs-lookup"><span data-stu-id="a2a7a-164">There are actually two types of validation error messages - those generated before the HTML form fields are bound to a class and those generated after the form fields are bound to the class.</span></span> <span data-ttu-id="a2a7a-165">En d’autres termes, il existe prebinding des erreurs de validation et postbinding des erreurs de validation.</span><span class="sxs-lookup"><span data-stu-id="a2a7a-165">In other words, there are prebinding validation errors and postbinding validation errors.</span></span>

<span data-ttu-id="a2a7a-166">L’action Create() exposée par le contrôleur de produit dans le Listing 1 accepte une instance de la classe de produit.</span><span class="sxs-lookup"><span data-stu-id="a2a7a-166">The Create() action exposed by the Product controller in Listing 1 accepts an instance of the Product class.</span></span> <span data-ttu-id="a2a7a-167">La signature de la méthode Create ressemble à ceci :</span><span class="sxs-lookup"><span data-stu-id="a2a7a-167">The signature of the Create method looks like this:</span></span>

[!code-csharp[Main](performing-simple-validation-cs/samples/sample3.cs)]

<span data-ttu-id="a2a7a-168">Les valeurs des champs de formulaire HTML dans le formulaire de création sont liés à la classe productToCreate par ce que l'on appelle un binder de modèle.</span><span class="sxs-lookup"><span data-stu-id="a2a7a-168">The values of the HTML form fields from the Create form are bound to the productToCreate class by something called a model binder.</span></span> <span data-ttu-id="a2a7a-169">Le binder de modèle par défaut ajoute automatiquement un message d’erreur à l’état du modèle lorsqu’il ne peut pas lier un champ de formulaire à une propriété de formulaire.</span><span class="sxs-lookup"><span data-stu-id="a2a7a-169">The default model binder adds an error message to model state automatically when it cannot bind a form field to a form property.</span></span>

<span data-ttu-id="a2a7a-170">Le binder de modèle par défaut ne peut pas lier la chaîne « apple » à la propriété de prix de la classe de produit.</span><span class="sxs-lookup"><span data-stu-id="a2a7a-170">The default model binder cannot bind the string "apple" to the Price property of the Product class.</span></span> <span data-ttu-id="a2a7a-171">Impossible d’assigner une chaîne à une propriété décimale.</span><span class="sxs-lookup"><span data-stu-id="a2a7a-171">You can't assign a string to a decimal property.</span></span> <span data-ttu-id="a2a7a-172">Par conséquent, le binder de modèle ajoute une erreur à l’état du modèle.</span><span class="sxs-lookup"><span data-stu-id="a2a7a-172">Therefore, the model binder adds an error to model state.</span></span>

<span data-ttu-id="a2a7a-173">Le binder de modèle par défaut ne peut pas également affecter une valeur null à une propriété qui n’accepte pas les valeurs NULL.</span><span class="sxs-lookup"><span data-stu-id="a2a7a-173">The default model binder also cannot assign a null value to a property that does not accept nulls.</span></span> <span data-ttu-id="a2a7a-174">En particulier, le binder de modèle ne peut pas assigner une valeur null à la propriété UnitsInStock.</span><span class="sxs-lookup"><span data-stu-id="a2a7a-174">In particular, the model binder cannot assign a null value to the UnitsInStock property.</span></span> <span data-ttu-id="a2a7a-175">Une fois encore, le binder de modèle renonce et ajoute un message d’erreur à l’état du modèle.</span><span class="sxs-lookup"><span data-stu-id="a2a7a-175">Once again, the model binder gives up and adds an error message to model state.</span></span>

<span data-ttu-id="a2a7a-176">Si vous souhaitez personnaliser l’apparence de ces messages d’erreur de prebinding vous devez créer des chaînes de ressources pour ces messages.</span><span class="sxs-lookup"><span data-stu-id="a2a7a-176">If you want to customize the appearance of these prebinding error messages then you need to create resource strings for these messages.</span></span>

## <a name="summary"></a><span data-ttu-id="a2a7a-177">Récapitulatif</span><span class="sxs-lookup"><span data-stu-id="a2a7a-177">Summary</span></span>

<span data-ttu-id="a2a7a-178">L’objectif de ce didacticiel a été pour décrire les mécanismes de base de validation dans l’infrastructure ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="a2a7a-178">The goal of this tutorial was to describe the basic mechanics of validation in the ASP.NET MVC framework.</span></span> <span data-ttu-id="a2a7a-179">Vous avez appris comment utiliser l’état du modèle et les programmes d’assistance HTML validation.</span><span class="sxs-lookup"><span data-stu-id="a2a7a-179">You learned how to use model state and the validation HTML helpers.</span></span> <span data-ttu-id="a2a7a-180">Nous avons abordé également la distinction entre prebinding et postbinding de validation.</span><span class="sxs-lookup"><span data-stu-id="a2a7a-180">We also discussed the distinction between prebinding and postbinding validation.</span></span> <span data-ttu-id="a2a7a-181">Dans d’autres didacticiels, nous allons aborder les différentes stratégies pour déplacer votre code de validation en dehors de vos contrôleurs et dans vos classes de modèle.</span><span class="sxs-lookup"><span data-stu-id="a2a7a-181">In other tutorials, we'll discuss various strategies for moving your validation code out of your controllers and into your model classes.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="a2a7a-182">[Précédent](displaying-a-table-of-database-data-cs.md)
> [Suivant](validating-with-the-idataerrorinfo-interface-cs.md)</span><span class="sxs-lookup"><span data-stu-id="a2a7a-182">[Previous](displaying-a-table-of-database-data-cs.md)
[Next](validating-with-the-idataerrorinfo-interface-cs.md)</span></span>
