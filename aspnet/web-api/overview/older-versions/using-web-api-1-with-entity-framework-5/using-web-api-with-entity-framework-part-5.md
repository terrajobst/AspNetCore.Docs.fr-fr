---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-5
title: 'Partie 5 : Création d’une interface utilisateur dynamique avec Knockout.js | Microsoft Docs'
author: MikeWasson
description: ''
ms.author: aspnetcontent
ms.date: 07/04/2012
ms.assetid: 9d9cb3b0-f4a7-434e-a508-9fc0ad0eb813
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-5
msc.type: authoredcontent
ms.openlocfilehash: 7d8bd4732ecf73c14251ceebfd667310f6e197b4
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37801691"
---
<a name="part-5-creating-a-dynamic-ui-with-knockoutjs"></a><span data-ttu-id="3c8cf-102">Partie 5 : Création d’une interface utilisateur dynamique avec Knockout.js</span><span class="sxs-lookup"><span data-stu-id="3c8cf-102">Part 5: Creating a Dynamic UI with Knockout.js</span></span>
====================
<span data-ttu-id="3c8cf-103">par [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="3c8cf-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="3c8cf-104">Télécharger le projet terminé</span><span class="sxs-lookup"><span data-stu-id="3c8cf-104">Download Completed Project</span></span>](http://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="creating-a-dynamic-ui-with-knockoutjs"></a><span data-ttu-id="3c8cf-105">Création d’une interface utilisateur dynamique avec Knockout.js</span><span class="sxs-lookup"><span data-stu-id="3c8cf-105">Creating a Dynamic UI with Knockout.js</span></span>

<span data-ttu-id="3c8cf-106">Dans cette section, nous allons utiliser Knockout.js pour ajouter des fonctionnalités à la vue de l’administrateur.</span><span class="sxs-lookup"><span data-stu-id="3c8cf-106">In this section, we'll use Knockout.js to add functionality to the Admin view.</span></span>

<span data-ttu-id="3c8cf-107">[Knockout.js](http://knockoutjs.com/) est une bibliothèque Javascript qui facilite la liaison de contrôles HTML aux données.</span><span class="sxs-lookup"><span data-stu-id="3c8cf-107">[Knockout.js](http://knockoutjs.com/) is a Javascript library that makes it easy to bind HTML controls to data.</span></span> <span data-ttu-id="3c8cf-108">Knockout.js utilise le modèle Model-View-ViewModel (MVVM).</span><span class="sxs-lookup"><span data-stu-id="3c8cf-108">Knockout.js uses the Model-View-ViewModel (MVVM) pattern.</span></span>

- <span data-ttu-id="3c8cf-109">Le *modèle* est la représentation sous forme de côté serveur des données dans le domaine d’entreprise (dans notre cas, les produits et les commandes).</span><span class="sxs-lookup"><span data-stu-id="3c8cf-109">The *model* is the server-side representation of the data in the business domain (in our case, products and orders).</span></span>
- <span data-ttu-id="3c8cf-110">Le *vue* est la couche de présentation (HTML).</span><span class="sxs-lookup"><span data-stu-id="3c8cf-110">The *view* is the presentation layer (HTML).</span></span>
- <span data-ttu-id="3c8cf-111">Le *modèle de vue* est un objet Javascript qui contient les données de modèle.</span><span class="sxs-lookup"><span data-stu-id="3c8cf-111">The *view-model* is a Javascript object that holds the model data.</span></span> <span data-ttu-id="3c8cf-112">Le modèle de vue est une abstraction de code de l’interface utilisateur.</span><span class="sxs-lookup"><span data-stu-id="3c8cf-112">The view-model is a code abstraction of the UI.</span></span> <span data-ttu-id="3c8cf-113">Elle n’a aucune connaissance de la représentation HTML.</span><span class="sxs-lookup"><span data-stu-id="3c8cf-113">It has no knowledge of the HTML representation.</span></span> <span data-ttu-id="3c8cf-114">Au lieu de cela, il représente les fonctionnalités abstraites de la vue, telles que « il s’agit d’une liste d’éléments ».</span><span class="sxs-lookup"><span data-stu-id="3c8cf-114">Instead, it represents abstract features of the view, such as "a list of items".</span></span>

<span data-ttu-id="3c8cf-115">La vue est lié aux données au modèle de vue.</span><span class="sxs-lookup"><span data-stu-id="3c8cf-115">The view is data-bound to the view-model.</span></span> <span data-ttu-id="3c8cf-116">Mises à jour le modèle de vue sont automatiquement reflétées dans la vue.</span><span class="sxs-lookup"><span data-stu-id="3c8cf-116">Updates to the view-model are automatically reflected in the view.</span></span> <span data-ttu-id="3c8cf-117">Le modèle de vue également Obtient les événements à partir de la vue, telles que des clics de bouton et effectue des opérations sur le modèle, telles que la création d’une commande.</span><span class="sxs-lookup"><span data-stu-id="3c8cf-117">The view-model also gets events from the view, such as button clicks, and performs operations on the model, such as creating an order.</span></span>

![](using-web-api-with-entity-framework-part-5/_static/image1.png)

<span data-ttu-id="3c8cf-118">Tout d’abord, nous allons définir le modèle de vue.</span><span class="sxs-lookup"><span data-stu-id="3c8cf-118">First we'll define the view-model.</span></span> <span data-ttu-id="3c8cf-119">Après cela, nous liera le balisage HTML pour le modèle de vue.</span><span class="sxs-lookup"><span data-stu-id="3c8cf-119">After that, we will bind the HTML markup to the view-model.</span></span>

<span data-ttu-id="3c8cf-120">Ajoutez la section suivante de Razor à Admin.cshtml :</span><span class="sxs-lookup"><span data-stu-id="3c8cf-120">Add the following Razor section to Admin.cshtml:</span></span>

[!code-cshtml[Main](using-web-api-with-entity-framework-part-5/samples/sample1.cshtml)]

<span data-ttu-id="3c8cf-121">Vous pouvez ajouter cette section n’importe où dans le fichier.</span><span class="sxs-lookup"><span data-stu-id="3c8cf-121">You can add this section anywhere in the file.</span></span> <span data-ttu-id="3c8cf-122">Lorsque la vue est restituée, la section apparaît au bas de la page HTML, avec le bouton droit avant de fermer le &lt;/corps&gt; balise.</span><span class="sxs-lookup"><span data-stu-id="3c8cf-122">When the view is rendered, the section appears at the bottom of the HTML page, right before the closing &lt;/body&gt; tag.</span></span>

<span data-ttu-id="3c8cf-123">Tous le script pour cette page passera à l’intérieur de la balise de script indiquée par le commentaire :</span><span class="sxs-lookup"><span data-stu-id="3c8cf-123">All of the script for this page will go inside the script tag indicated by the comment:</span></span>

[!code-html[Main](using-web-api-with-entity-framework-part-5/samples/sample2.html)]

<span data-ttu-id="3c8cf-124">Tout d’abord, définissez une classe de modèle de vue :</span><span class="sxs-lookup"><span data-stu-id="3c8cf-124">First, define a view-model class:</span></span>

[!code-javascript[Main](using-web-api-with-entity-framework-part-5/samples/sample3.js)]

<span data-ttu-id="3c8cf-125">**ko.observableArray** est un type spécial d’objet dans Knockout, appelé un *observable*.</span><span class="sxs-lookup"><span data-stu-id="3c8cf-125">**ko.observableArray** is a special kind of object in Knockout, called an *observable*.</span></span> <span data-ttu-id="3c8cf-126">À partir de la [Knockout.js documentation](http://knockoutjs.com/documentation/observables.html): un observable est un « objet JavaScript qui peut avertir les abonnés sur les modifications apportées. »</span><span class="sxs-lookup"><span data-stu-id="3c8cf-126">From the [Knockout.js documentation](http://knockoutjs.com/documentation/observables.html): An observable is a "JavaScript object that can notify subscribers about changes."</span></span> <span data-ttu-id="3c8cf-127">Lorsque le contenu d’un observable change, la vue est automatiquement mis à jour.</span><span class="sxs-lookup"><span data-stu-id="3c8cf-127">When the contents of an observable change, the view is automatically updated to match.</span></span>

<span data-ttu-id="3c8cf-128">Pour remplir le `products` de tableau, d’effectuer une requête AJAX à l’API web.</span><span class="sxs-lookup"><span data-stu-id="3c8cf-128">To populate the `products` array, make an AJAX request to the web API.</span></span> <span data-ttu-id="3c8cf-129">Rappelez-vous que nous avons stocké l’URI de base pour l’API dans le sac d’affichage (voir [partie 4](using-web-api-with-entity-framework-part-4.md) du didacticiel).</span><span class="sxs-lookup"><span data-stu-id="3c8cf-129">Recall that we stored the base URI for the API in the view bag (see [Part 4](using-web-api-with-entity-framework-part-4.md) of the tutorial).</span></span>

[!code-javascript[Main](using-web-api-with-entity-framework-part-5/samples/sample4.js?highlight=5)]

<span data-ttu-id="3c8cf-130">Ensuite, ajoutez des fonctions pour le modèle de vue à créer, mettre à jour et supprimer des produits.</span><span class="sxs-lookup"><span data-stu-id="3c8cf-130">Next, add functions to the view-model to create, update, and delete products.</span></span> <span data-ttu-id="3c8cf-131">Ces fonctions soumettre les appels AJAX à l’API web et utilisent les résultats pour mettre à jour le modèle de vue.</span><span class="sxs-lookup"><span data-stu-id="3c8cf-131">These functions submit AJAX calls to the web API and use the results to update the view-model.</span></span>

[!code-javascript[Main](using-web-api-with-entity-framework-part-5/samples/sample5.js?highlight=7)]

<span data-ttu-id="3c8cf-132">Maintenant la partie la plus importante : lorsque le DOM est fulled appel chargé, le **ko.applyBindings** de fonction et le passer à une nouvelle instance de la `ProductsViewModel`:</span><span class="sxs-lookup"><span data-stu-id="3c8cf-132">Now the most important part: When the DOM is fulled loaded, call the **ko.applyBindings** function and pass in a new instance of the `ProductsViewModel`:</span></span>

[!code-javascript[Main](using-web-api-with-entity-framework-part-5/samples/sample6.js)]

<span data-ttu-id="3c8cf-133">Le **ko.applyBindings** méthode active Knockout et associe le modèle de vue à la vue.</span><span class="sxs-lookup"><span data-stu-id="3c8cf-133">The **ko.applyBindings** method activates Knockout and wires up the view-model to the view.</span></span>

<span data-ttu-id="3c8cf-134">Maintenant que nous avons un modèle de vue, nous pouvons créer les liaisons.</span><span class="sxs-lookup"><span data-stu-id="3c8cf-134">Now that we have a view-model, we can create the bindings.</span></span> <span data-ttu-id="3c8cf-135">Dans Knockout.js, vous faire en ajoutant `data-bind` attributs aux éléments HTML.</span><span class="sxs-lookup"><span data-stu-id="3c8cf-135">In Knockout.js, you do this by adding `data-bind` attributes to HTML elements.</span></span> <span data-ttu-id="3c8cf-136">Par exemple, pour lier une liste HTML à un tableau, utilisez le `foreach` liaison :</span><span class="sxs-lookup"><span data-stu-id="3c8cf-136">For example, to bind an HTML list to an array, use the `foreach` binding:</span></span>

[!code-html[Main](using-web-api-with-entity-framework-part-5/samples/sample7.html?highlight=1)]

<span data-ttu-id="3c8cf-137">Le `foreach` liaison effectue une itération dans le tableau et crée des éléments pour chaque objet enfant dans le tableau.</span><span class="sxs-lookup"><span data-stu-id="3c8cf-137">The `foreach` binding iterates through the array and creates child elements for each object in the array.</span></span> <span data-ttu-id="3c8cf-138">Liaisons sur les éléments enfants peuvent faire référence aux propriétés sur les objets du tableau.</span><span class="sxs-lookup"><span data-stu-id="3c8cf-138">Bindings on the child elements can refer to properties on the array objects.</span></span>

<span data-ttu-id="3c8cf-139">Ajoutez les liaisons suivantes à la liste « produits de mise à jour » :</span><span class="sxs-lookup"><span data-stu-id="3c8cf-139">Add the following bindings to the "update-products" list:</span></span>

[!code-html[Main](using-web-api-with-entity-framework-part-5/samples/sample8.html)]

<span data-ttu-id="3c8cf-140">Le `<li>` élément se produit dans l’étendue de la **foreach** liaison.</span><span class="sxs-lookup"><span data-stu-id="3c8cf-140">The `<li>` element occurs within the scope of the **foreach** binding.</span></span> <span data-ttu-id="3c8cf-141">Que signifie sera de Knockout restitue l’élément une fois pour chaque produit dans le `products` tableau.</span><span class="sxs-lookup"><span data-stu-id="3c8cf-141">That means Knockout will render the element once for each product in the `products` array.</span></span> <span data-ttu-id="3c8cf-142">Toutes les liaisons dans le `<li>` élément font référence à cette instance de produit.</span><span class="sxs-lookup"><span data-stu-id="3c8cf-142">All of the bindings within the `<li>` element refer to that product instance.</span></span> <span data-ttu-id="3c8cf-143">Par exemple, `$data.Name` fait référence à la `Name` propriété sur le produit.</span><span class="sxs-lookup"><span data-stu-id="3c8cf-143">For example, `$data.Name` refers to the `Name` property on the product.</span></span>

<span data-ttu-id="3c8cf-144">Pour définir les valeurs des entrées de texte, utilisez le `value` liaison.</span><span class="sxs-lookup"><span data-stu-id="3c8cf-144">To set the values of the text inputs, use the `value` binding.</span></span> <span data-ttu-id="3c8cf-145">Les boutons sont liés aux fonctions sur la vue de modèle, à l’aide de la `click` liaison.</span><span class="sxs-lookup"><span data-stu-id="3c8cf-145">The buttons are bound to functions on the model-view, using the `click` binding.</span></span> <span data-ttu-id="3c8cf-146">L’instance de produit est passé en tant que paramètre à chaque fonction.</span><span class="sxs-lookup"><span data-stu-id="3c8cf-146">The product instance is passed as a parameter to each function.</span></span> <span data-ttu-id="3c8cf-147">Pour plus d’informations, le [Knockout.js documentation](http://knockoutjs.com/documentation/observables.html) a bonne descriptions des liaisons différentes.</span><span class="sxs-lookup"><span data-stu-id="3c8cf-147">For more information, the [Knockout.js documentation](http://knockoutjs.com/documentation/observables.html) has good descriptions of the various bindings.</span></span>

<span data-ttu-id="3c8cf-148">Ensuite, ajoutez une liaison pour le **soumettre** événement sur le formulaire Ajouter un produit :</span><span class="sxs-lookup"><span data-stu-id="3c8cf-148">Next, add a binding for the **submit** event on the Add Product form:</span></span>

[!code-html[Main](using-web-api-with-entity-framework-part-5/samples/sample9.html)]

<span data-ttu-id="3c8cf-149">Cette liaison appelle le `create` fonction sur le modèle de vue pour créer un nouveau produit.</span><span class="sxs-lookup"><span data-stu-id="3c8cf-149">This binding calls the `create` function on the view-model to create a new product.</span></span>

<span data-ttu-id="3c8cf-150">Voici le code complet pour l’affichage de l’administrateur :</span><span class="sxs-lookup"><span data-stu-id="3c8cf-150">Here is the complete code for the Admin view:</span></span>

[!code-cshtml[Main](using-web-api-with-entity-framework-part-5/samples/sample10.cshtml)]

<span data-ttu-id="3c8cf-151">Exécutez l’application, connectez-vous avec le compte d’administrateur, cliquez sur le lien « Admin ».</span><span class="sxs-lookup"><span data-stu-id="3c8cf-151">Run the application, log in with the Administrator account, and click the "Admin" link.</span></span> <span data-ttu-id="3c8cf-152">Vous devez voir la liste des produits et être en mesure de créer, mettre à jour ou supprimer des produits.</span><span class="sxs-lookup"><span data-stu-id="3c8cf-152">You should see the list of products, and be able to create, update, or delete products.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="3c8cf-153">[Précédent](using-web-api-with-entity-framework-part-4.md)
> [Suivant](using-web-api-with-entity-framework-part-6.md)</span><span class="sxs-lookup"><span data-stu-id="3c8cf-153">[Previous](using-web-api-with-entity-framework-part-4.md)
[Next](using-web-api-with-entity-framework-part-6.md)</span></span>
