---
uid: mvc/overview/older-versions-1/models-data/performing-simple-validation-cs
title: "Exécution de la Validation Simple (c#) | Documents Microsoft"
author: StephenWalther
description: "Découvrez comment effectuer la validation dans une application ASP.NET MVC. Dans ce didacticiel, Stephen Walther présente l’état du modèle et l’application d’assistance de la validation HTML..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/02/2009
ms.topic: article
ms.assetid: 21383c9d-6aea-4bad-a99b-b5f2c9d6503f
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/models-data/performing-simple-validation-cs
msc.type: authoredcontent
ms.openlocfilehash: 005872308d9d4d8ac7feb12dd5ab1fc463d0140e
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/10/2017
---
<a name="performing-simple-validation-c"></a><span data-ttu-id="eb1a8-104">Exécution de la Validation Simple (c#)</span><span class="sxs-lookup"><span data-stu-id="eb1a8-104">Performing Simple Validation (C#)</span></span>
====================
<span data-ttu-id="eb1a8-105">par [Stephen Walther](https://github.com/StephenWalther)</span><span class="sxs-lookup"><span data-stu-id="eb1a8-105">by [Stephen Walther](https://github.com/StephenWalther)</span></span>

> <span data-ttu-id="eb1a8-106">Découvrez comment effectuer la validation dans une application ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="eb1a8-106">Learn how to perform validation in an ASP.NET MVC application.</span></span> <span data-ttu-id="eb1a8-107">Dans ce didacticiel, Stephen Walther présente vous à l’état du modèle et les programmes d’assistance HTML de la validation.</span><span class="sxs-lookup"><span data-stu-id="eb1a8-107">In this tutorial, Stephen Walther introduces you to model state and the validation HTML helpers.</span></span>


<span data-ttu-id="eb1a8-108">L’objectif de ce didacticiel est d’expliquer comment vous pouvez effectuer la validation au sein d’une application ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="eb1a8-108">The goal of this tutorial is to explain how you can perform validation within an ASP.NET MVC application.</span></span> <span data-ttu-id="eb1a8-109">Par exemple, vous découvrez comment empêcher une personne de l’envoi d’un formulaire qui ne contient pas une valeur pour un champ obligatoire.</span><span class="sxs-lookup"><span data-stu-id="eb1a8-109">For example, you learn how to prevent someone from submitting a form that does not contain a value for a required field.</span></span> <span data-ttu-id="eb1a8-110">Vous apprenez à utiliser l’état du modèle et les programmes d’assistance HTML de la validation.</span><span class="sxs-lookup"><span data-stu-id="eb1a8-110">You learn how to use model state and the validation HTML helpers.</span></span>

## <a name="understanding-model-state"></a><span data-ttu-id="eb1a8-111">État de modèle de présentation</span><span class="sxs-lookup"><span data-stu-id="eb1a8-111">Understanding Model State</span></span>

<span data-ttu-id="eb1a8-112">Vous utilisez - état du modèle, ou plus précisément, le dictionnaire d’états de modèle - pour représenter des erreurs de validation.</span><span class="sxs-lookup"><span data-stu-id="eb1a8-112">You use model state - or more accurately, the model state dictionary - to represent validation errors.</span></span> <span data-ttu-id="eb1a8-113">Par exemple, l’action dans la liste 1 Create() valide les propriétés d’une classe de produit avant d’ajouter la classe de produit pour une base de données.</span><span class="sxs-lookup"><span data-stu-id="eb1a8-113">For example, the Create() action in Listing 1 validates the properties of a Product class before adding the Product class to a database.</span></span>


<span data-ttu-id="eb1a8-114">Je ne suis pas recommandation que vous ajoutez votre logique de validation ou de la base de données à un contrôleur.</span><span class="sxs-lookup"><span data-stu-id="eb1a8-114">I'm not recommending that you add your validation or database logic to a controller.</span></span> <span data-ttu-id="eb1a8-115">Un contrôleur doit contenir uniquement la logique relatives au contrôle de flux d’application.</span><span class="sxs-lookup"><span data-stu-id="eb1a8-115">A controller should contain only logic related to application flow control.</span></span> <span data-ttu-id="eb1a8-116">Nous prenons un raccourci pour simplifier les choses.</span><span class="sxs-lookup"><span data-stu-id="eb1a8-116">We are taking a shortcut to keep things simple.</span></span>


<span data-ttu-id="eb1a8-117">**La liste 1 - Controllers\ProductController.cs**</span><span class="sxs-lookup"><span data-stu-id="eb1a8-117">**Listing 1 - Controllers\ProductController.cs**</span></span>

[!code-csharp[Main](performing-simple-validation-cs/samples/sample1.cs)]

<span data-ttu-id="eb1a8-118">Dans la liste de 1, les propriétés de nom, Description et UnitsInStock de la classe de produit sont validées.</span><span class="sxs-lookup"><span data-stu-id="eb1a8-118">In Listing 1, the Name, Description, and UnitsInStock properties of the Product class are validated.</span></span> <span data-ttu-id="eb1a8-119">Si une de ces propriétés échoue un test de validation une erreur est ajoutée au dictionnaire d’états du modèle (représenté par la propriété ModelState de la classe de contrôleur).</span><span class="sxs-lookup"><span data-stu-id="eb1a8-119">If any of these properties fail a validation test then an error is added to the model state dictionary (represented by the ModelState property of the Controller class).</span></span>

<span data-ttu-id="eb1a8-120">S’il existe des erreurs dans l’état de modèle la propriété ModelState.IsValid retourne false.</span><span class="sxs-lookup"><span data-stu-id="eb1a8-120">If there are any errors in model state then the ModelState.IsValid property returns false.</span></span> <span data-ttu-id="eb1a8-121">Dans ce cas, le formulaire HTML pour la création d’un nouveau produit s’affiche de nouveau.</span><span class="sxs-lookup"><span data-stu-id="eb1a8-121">In that case, the HTML form for creating a new product is redisplayed.</span></span> <span data-ttu-id="eb1a8-122">Sinon, s’il n’y a aucune erreur de validation, le nouveau produit est ajouté à la base de données.</span><span class="sxs-lookup"><span data-stu-id="eb1a8-122">Otherwise, if there are no validation errors, the new Product is added to the database.</span></span>

## <a name="using-the-validation-helpers"></a><span data-ttu-id="eb1a8-123">À l’aide de programmes d’assistance des Validation</span><span class="sxs-lookup"><span data-stu-id="eb1a8-123">Using the Validation Helpers</span></span>

<span data-ttu-id="eb1a8-124">L’infrastructure ASP.NET MVC inclut les deux programmes d’assistance de validation : l’application d’assistance Html.ValidationMessage() et l’application d’assistance Html.ValidationSummary().</span><span class="sxs-lookup"><span data-stu-id="eb1a8-124">The ASP.NET MVC framework includes two validation helpers: the Html.ValidationMessage() helper and the Html.ValidationSummary() helper.</span></span> <span data-ttu-id="eb1a8-125">Ces deux programmes d’assistance dans une vue vous permet d’afficher des messages d’erreur de validation.</span><span class="sxs-lookup"><span data-stu-id="eb1a8-125">You use these two helpers in a view to display validation error messages.</span></span>

<span data-ttu-id="eb1a8-126">Les programmes d’assistance Html.ValidationMessage() et Html.ValidationSummary() sont utilisés dans les créer et modifier des vues qui sont générés automatiquement par la génération de modèles automatique ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="eb1a8-126">The Html.ValidationMessage() and Html.ValidationSummary() helpers are used in the Create and Edit views that are generated automatically by the ASP.NET MVC scaffolding.</span></span> <span data-ttu-id="eb1a8-127">Suivez ces étapes pour générer la vue de créer :</span><span class="sxs-lookup"><span data-stu-id="eb1a8-127">Follow these steps to generate the Create view:</span></span>

1. <span data-ttu-id="eb1a8-128">Cliquez sur l’action Create() dans le contrôleur de produit et sélectionnez l’option de menu **ajouter une vue** (voir Figure 1).</span><span class="sxs-lookup"><span data-stu-id="eb1a8-128">Right-click the Create() action in the Product controller and select the menu option **Add View** (see Figure 1).</span></span>
2. <span data-ttu-id="eb1a8-129">Dans le **ajouter une vue** boîte de dialogue, cochez la case intitulée **créer une vue fortement typée** (voir Figure 2).</span><span class="sxs-lookup"><span data-stu-id="eb1a8-129">In the **Add View** dialog, check the checkbox labeled **Create a strongly-typed view** (see Figure 2).</span></span>
3. <span data-ttu-id="eb1a8-130">À partir de la **afficher la classe de données** liste déroulante, sélectionnez la classe de produit.</span><span class="sxs-lookup"><span data-stu-id="eb1a8-130">From the **View data class** dropdown list, select the Product class.</span></span>
4. <span data-ttu-id="eb1a8-131">À partir de la **afficher le contenu** liste déroulante, sélectionnez Créer.</span><span class="sxs-lookup"><span data-stu-id="eb1a8-131">From the **View content** dropdown list, select Create.</span></span>
5. <span data-ttu-id="eb1a8-132">Cliquez sur le bouton **Ajouter**.</span><span class="sxs-lookup"><span data-stu-id="eb1a8-132">Click the **Add** button.</span></span>


<span data-ttu-id="eb1a8-133">Assurez-vous que vous générez votre application avant l’ajout d’une vue.</span><span class="sxs-lookup"><span data-stu-id="eb1a8-133">Make sure that you build your application before adding a view.</span></span> <span data-ttu-id="eb1a8-134">Dans le cas contraire, la liste de classes n’apparaît plus dans le **afficher la classe de données** liste déroulante.</span><span class="sxs-lookup"><span data-stu-id="eb1a8-134">Otherwise, the list of classes won't appear in the **View data class** dropdown list.</span></span>


<span data-ttu-id="eb1a8-135">[![La boîte de dialogue Nouveau projet](performing-simple-validation-cs/_static/image1.jpg)](performing-simple-validation-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="eb1a8-135">[![The New Project dialog box](performing-simple-validation-cs/_static/image1.jpg)](performing-simple-validation-cs/_static/image1.png)</span></span>

<span data-ttu-id="eb1a8-136">**Figure 01**: ajout d’une vue ([cliquez pour afficher l’image en taille réelle](performing-simple-validation-cs/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="eb1a8-136">**Figure 01**: Adding a view([Click to view full-size image](performing-simple-validation-cs/_static/image2.png))</span></span>


<span data-ttu-id="eb1a8-137">[![La boîte de dialogue Nouveau projet](performing-simple-validation-cs/_static/image2.jpg)](performing-simple-validation-cs/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="eb1a8-137">[![The New Project dialog box](performing-simple-validation-cs/_static/image2.jpg)](performing-simple-validation-cs/_static/image3.png)</span></span>

<span data-ttu-id="eb1a8-138">**Figure 02**: création d’une vue fortement typée ([cliquez pour afficher l’image en taille réelle](performing-simple-validation-cs/_static/image4.png))</span><span class="sxs-lookup"><span data-stu-id="eb1a8-138">**Figure 02**: Creating a strongly-typed view ([Click to view full-size image](performing-simple-validation-cs/_static/image4.png))</span></span>


<span data-ttu-id="eb1a8-139">Après avoir effectué ces étapes, vous obtenez la vue de créer dans la liste 2.</span><span class="sxs-lookup"><span data-stu-id="eb1a8-139">After you complete these steps, you get the Create view in Listing 2.</span></span>

<span data-ttu-id="eb1a8-140">**Listing 2 - Views\Product\Create.aspx**</span><span class="sxs-lookup"><span data-stu-id="eb1a8-140">**Listing 2 - Views\Product\Create.aspx**</span></span>

[!code-aspx[Main](performing-simple-validation-cs/samples/sample2.aspx)]

<span data-ttu-id="eb1a8-141">Dans la liste 2, l’application d’assistance Html.ValidationSummary() est appelée immédiatement au-dessus du formulaire HTML.</span><span class="sxs-lookup"><span data-stu-id="eb1a8-141">In Listing 2, the Html.ValidationSummary() helper is called immediately above the HTML form.</span></span> <span data-ttu-id="eb1a8-142">Ce programme d’assistance est utilisé pour afficher la liste des messages d’erreur de validation.</span><span class="sxs-lookup"><span data-stu-id="eb1a8-142">This helper is used to display a list of validation error messages.</span></span> <span data-ttu-id="eb1a8-143">L’application d’assistance Html.ValidationSummary() restitue les erreurs dans une liste à puces.</span><span class="sxs-lookup"><span data-stu-id="eb1a8-143">The Html.ValidationSummary() helper renders the errors in a bulleted list.</span></span>

<span data-ttu-id="eb1a8-144">L’application d’assistance Html.ValidationMessage() est appelée en regard de chacun des champs de formulaire HTML.</span><span class="sxs-lookup"><span data-stu-id="eb1a8-144">The Html.ValidationMessage() helper is called next to each of the HTML form fields.</span></span> <span data-ttu-id="eb1a8-145">Ce programme d’assistance est utilisé pour afficher un message d’erreur à droite en regard d’un champ de formulaire.</span><span class="sxs-lookup"><span data-stu-id="eb1a8-145">This helper is used to display an error message right next to a form field.</span></span> <span data-ttu-id="eb1a8-146">Dans le cas de liste 2, l’application d’assistance Html.ValidationMessage() affiche un astérisque lorsqu’il existe une erreur.</span><span class="sxs-lookup"><span data-stu-id="eb1a8-146">In the case of Listing 2, the Html.ValidationMessage() helper displays an asterisk when there is an error.</span></span>

<span data-ttu-id="eb1a8-147">La page dans la Figure 3 illustre les messages d’erreur rendus par les programmes d’assistance de validation lorsque le formulaire est envoyé avec les champs manquants et des valeurs non valides.</span><span class="sxs-lookup"><span data-stu-id="eb1a8-147">The page in Figure 3 illustrates the error messages rendered by the validation helpers when the form is submitted with missing fields and invalid values.</span></span>


<span data-ttu-id="eb1a8-148">[![La boîte de dialogue Nouveau projet](performing-simple-validation-cs/_static/image3.jpg)](performing-simple-validation-cs/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="eb1a8-148">[![The New Project dialog box](performing-simple-validation-cs/_static/image3.jpg)](performing-simple-validation-cs/_static/image5.png)</span></span>

<span data-ttu-id="eb1a8-149">**Figure 03**: créer la vue, soumise avec des problèmes ([cliquez pour afficher l’image en taille réelle](performing-simple-validation-cs/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="eb1a8-149">**Figure 03**: The Create view submitted with problems ([Click to view full-size image](performing-simple-validation-cs/_static/image6.png))</span></span>


<span data-ttu-id="eb1a8-150">Notez que l’apparence du contenu HTML d’entrée, les champs sont également modifiés lorsqu’il existe une erreur de validation.</span><span class="sxs-lookup"><span data-stu-id="eb1a8-150">Notice that the appearance of the HTML input fields are also modified when there is a validation error.</span></span> <span data-ttu-id="eb1a8-151">Les convertisseurs d’assistance de Html.TextBox() un *classe = « erreur de validation d’entrée »* attribut lorsqu’il existe une erreur de validation associé à la propriété restituée par l’application d’assistance Html.TextBox().</span><span class="sxs-lookup"><span data-stu-id="eb1a8-151">The Html.TextBox() helper renders a *class="input-validation-error"* attribute when there is a validation error associated with the property rendered by the Html.TextBox() helper.</span></span>

<span data-ttu-id="eb1a8-152">Il existe trois classes de feuille de style en cascade utilisées pour contrôler l’apparence des erreurs de validation :</span><span class="sxs-lookup"><span data-stu-id="eb1a8-152">There are three cascading style sheet classes used to control the appearance of validation errors:</span></span>

- <span data-ttu-id="eb1a8-153">entrée-validation-erreur - appliquée à la &lt;d’entrée&gt; balise rendue par l’application d’assistance Html.TextBox().</span><span class="sxs-lookup"><span data-stu-id="eb1a8-153">input-validation-error - Applied to the &lt;input&gt; tag rendered by Html.TextBox() helper.</span></span>
- <span data-ttu-id="eb1a8-154">champ-validation-erreur - appliquée à la &lt;span&gt; balise rendue par l’application d’assistance Html.ValidationMessage().</span><span class="sxs-lookup"><span data-stu-id="eb1a8-154">field-validation-error - Applied to the &lt;span&gt; tag rendered by the Html.ValidationMessage() helper.</span></span>
- <span data-ttu-id="eb1a8-155">validation-résumé-erreurs - appliquée à la &lt;ul&gt; balise rendue par l’application d’assistance Html.ValidationSumamry().</span><span class="sxs-lookup"><span data-stu-id="eb1a8-155">validation-summary-errors - Applied to the &lt;ul&gt; tag rendered by the Html.ValidationSumamry() helper.</span></span>

<span data-ttu-id="eb1a8-156">Vous pouvez modifier ces classes de feuille de style en cascade et par conséquent de modifier l’apparence des erreurs de validation, en modifiant le fichier Site.css situé dans le dossier de contenu.</span><span class="sxs-lookup"><span data-stu-id="eb1a8-156">You can modify these cascading style sheet classes, and therefore modify the appearance of the validation errors, by modifying the Site.css file located in the Content folder.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="eb1a8-157">La classe HtmlHelper inclut les propriétés statiques en lecture seule pour les noms de la validation de la récupération des connexes CSS classes.</span><span class="sxs-lookup"><span data-stu-id="eb1a8-157">The HtmlHelper class includes read-only static properties for retrieving the names of the validation related CSS classes.</span></span> <span data-ttu-id="eb1a8-158">Ces propriétés statiques sont nommées ValidationInputCssClassName, ValidationFieldCssClassName et ValidationSummaryCssClassName.</span><span class="sxs-lookup"><span data-stu-id="eb1a8-158">These static properties are named ValidationInputCssClassName, ValidationFieldCssClassName, and ValidationSummaryCssClassName.</span></span>


## <a name="prebinding-validation-and-postbinding-validation"></a><span data-ttu-id="eb1a8-159">Prebinding de Validation et la Validation de Postbinding</span><span class="sxs-lookup"><span data-stu-id="eb1a8-159">Prebinding Validation and Postbinding Validation</span></span>

<span data-ttu-id="eb1a8-160">Si vous envoyez le formulaire HTML pour la création d’un produit et que vous entrez une valeur non valide pour le champ prix et aucune valeur pour le champ UnitsInStock, vous obtenez les messages de validation affichées dans la Figure 4.</span><span class="sxs-lookup"><span data-stu-id="eb1a8-160">If you submit the HTML form for creating a Product, and you enter an invalid value for the price field and no value for the UnitsInStock field, then you'll get the validation messages displayed in Figure 4.</span></span> <span data-ttu-id="eb1a8-161">D'où proviennent ces messages d’erreur de validation ?</span><span class="sxs-lookup"><span data-stu-id="eb1a8-161">Where do these validation error messages come from?</span></span>


<span data-ttu-id="eb1a8-162">[![La boîte de dialogue Nouveau projet](performing-simple-validation-cs/_static/image4.jpg)](performing-simple-validation-cs/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="eb1a8-162">[![The New Project dialog box](performing-simple-validation-cs/_static/image4.jpg)](performing-simple-validation-cs/_static/image7.png)</span></span>

<span data-ttu-id="eb1a8-163">**Figure 04**: erreurs de Validation Prebinding ([cliquez pour afficher l’image en taille réelle](performing-simple-validation-cs/_static/image8.png))</span><span class="sxs-lookup"><span data-stu-id="eb1a8-163">**Figure 04**: Prebinding Validation Errors([Click to view full-size image](performing-simple-validation-cs/_static/image8.png))</span></span>


<span data-ttu-id="eb1a8-164">Il existe en fait deux types de messages d’erreur de validation - ceux générés avant que les champs de formulaire HTML sont liés à une classe et ceux générés une fois les champs de formulaire sont liés à la classe.</span><span class="sxs-lookup"><span data-stu-id="eb1a8-164">There are actually two types of validation error messages - those generated before the HTML form fields are bound to a class and those generated after the form fields are bound to the class.</span></span> <span data-ttu-id="eb1a8-165">En d’autres termes, il existe prebinding des erreurs de validation et postbinding des erreurs de validation.</span><span class="sxs-lookup"><span data-stu-id="eb1a8-165">In other words, there are prebinding validation errors and postbinding validation errors.</span></span>

<span data-ttu-id="eb1a8-166">L’action Create() exposée par le contrôleur de produit dans la liste 1 accepte une instance de la classe de produit.</span><span class="sxs-lookup"><span data-stu-id="eb1a8-166">The Create() action exposed by the Product controller in Listing 1 accepts an instance of the Product class.</span></span> <span data-ttu-id="eb1a8-167">La signature de la méthode Create ressemble à ceci :</span><span class="sxs-lookup"><span data-stu-id="eb1a8-167">The signature of the Create method looks like this:</span></span>

[!code-csharp[Main](performing-simple-validation-cs/samples/sample3.cs)]

<span data-ttu-id="eb1a8-168">Les valeurs des champs de formulaire HTML dans le formulaire de création sont liés à la classe productToCreate par ce que l'on appelle un classeur de modèles.</span><span class="sxs-lookup"><span data-stu-id="eb1a8-168">The values of the HTML form fields from the Create form are bound to the productToCreate class by something called a model binder.</span></span> <span data-ttu-id="eb1a8-169">Le classeur de modèles par défaut ajoute automatiquement un message d’erreur à l’état du modèle lorsqu’il ne peut pas lier un champ de formulaire à une propriété de formulaire.</span><span class="sxs-lookup"><span data-stu-id="eb1a8-169">The default model binder adds an error message to model state automatically when it cannot bind a form field to a form property.</span></span>

<span data-ttu-id="eb1a8-170">Le classeur de modèles par défaut ne peut pas lier la chaîne « apple » à la propriété de prix de la classe de produit.</span><span class="sxs-lookup"><span data-stu-id="eb1a8-170">The default model binder cannot bind the string "apple" to the Price property of the Product class.</span></span> <span data-ttu-id="eb1a8-171">Vous ne pouvez pas affecter une chaîne à une propriété décimale.</span><span class="sxs-lookup"><span data-stu-id="eb1a8-171">You can't assign a string to a decimal property.</span></span> <span data-ttu-id="eb1a8-172">Par conséquent, le classeur de modèles ajoute une erreur à l’état du modèle.</span><span class="sxs-lookup"><span data-stu-id="eb1a8-172">Therefore, the model binder adds an error to model state.</span></span>

<span data-ttu-id="eb1a8-173">Le classeur de modèles par défaut ne peut pas également affecter une valeur null à une propriété qui n’accepte pas les valeurs NULL.</span><span class="sxs-lookup"><span data-stu-id="eb1a8-173">The default model binder also cannot assign a null value to a property that does not accept nulls.</span></span> <span data-ttu-id="eb1a8-174">En particulier, le binder de modèle ne peut pas assigner une valeur null à la propriété UnitsInStock.</span><span class="sxs-lookup"><span data-stu-id="eb1a8-174">In particular, the model binder cannot assign a null value to the UnitsInStock property.</span></span> <span data-ttu-id="eb1a8-175">Une fois encore, le classeur de modèles abandonne et ajoute un message d’erreur à l’état du modèle.</span><span class="sxs-lookup"><span data-stu-id="eb1a8-175">Once again, the model binder gives up and adds an error message to model state.</span></span>

<span data-ttu-id="eb1a8-176">Si vous souhaitez personnaliser l’apparence de ces messages d’erreur de prebinding vous devez créer des chaînes de ressources pour ces messages.</span><span class="sxs-lookup"><span data-stu-id="eb1a8-176">If you want to customize the appearance of these prebinding error messages then you need to create resource strings for these messages.</span></span>

## <a name="summary"></a><span data-ttu-id="eb1a8-177">Résumé</span><span class="sxs-lookup"><span data-stu-id="eb1a8-177">Summary</span></span>

<span data-ttu-id="eb1a8-178">L’objectif de ce didacticiel a pour décrire les mécanismes de base de validation dans l’infrastructure ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="eb1a8-178">The goal of this tutorial was to describe the basic mechanics of validation in the ASP.NET MVC framework.</span></span> <span data-ttu-id="eb1a8-179">Vous avez appris à utiliser l’état du modèle et les programmes d’assistance HTML de la validation.</span><span class="sxs-lookup"><span data-stu-id="eb1a8-179">You learned how to use model state and the validation HTML helpers.</span></span> <span data-ttu-id="eb1a8-180">Nous avons abordé également la distinction entre prebinding et postbinding de validation.</span><span class="sxs-lookup"><span data-stu-id="eb1a8-180">We also discussed the distinction between prebinding and postbinding validation.</span></span> <span data-ttu-id="eb1a8-181">Dans d’autres didacticiels, nous aborderons diverses stratégies pour le déplacement de votre code de validation en dehors de vos contrôleurs et dans vos classes de modèle.</span><span class="sxs-lookup"><span data-stu-id="eb1a8-181">In other tutorials, we'll discuss various strategies for moving your validation code out of your controllers and into your model classes.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="eb1a8-182">[Précédent](displaying-a-table-of-database-data-cs.md)
[Suivant](validating-with-the-idataerrorinfo-interface-cs.md)</span><span class="sxs-lookup"><span data-stu-id="eb1a8-182">[Previous](displaying-a-table-of-database-data-cs.md)
[Next](validating-with-the-idataerrorinfo-interface-cs.md)</span></span>
