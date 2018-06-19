---
uid: mvc/overview/older-versions-1/models-data/validating-with-the-idataerrorinfo-interface-vb
title: Validation avec l’Interface IDataErrorInfo (VB) | Documents Microsoft
author: StephenWalther
description: Stephen Walther vous montre comment afficher des messages d’erreur de validation personnalisées en implémentant l’interface IDataErrorInfo dans une classe de modèle.
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/02/2009
ms.topic: article
ms.assetid: 3a8a9d9f-82dd-4959-b7c6-960e9ce95df1
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/models-data/validating-with-the-idataerrorinfo-interface-vb
msc.type: authoredcontent
ms.openlocfilehash: 60df0f934432484e0c97e0caef25c15605beb14f
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30870087"
---
<a name="validating-with-the-idataerrorinfo-interface-vb"></a><span data-ttu-id="22b10-103">Validation avec l’Interface IDataErrorInfo (VB)</span><span class="sxs-lookup"><span data-stu-id="22b10-103">Validating with the IDataErrorInfo Interface (VB)</span></span>
====================
<span data-ttu-id="22b10-104">par [Stephen Walther](https://github.com/StephenWalther)</span><span class="sxs-lookup"><span data-stu-id="22b10-104">by [Stephen Walther](https://github.com/StephenWalther)</span></span>

> <span data-ttu-id="22b10-105">Stephen Walther vous montre comment afficher des messages d’erreur de validation personnalisées en implémentant l’interface IDataErrorInfo dans une classe de modèle.</span><span class="sxs-lookup"><span data-stu-id="22b10-105">Stephen Walther shows you how to display custom validation error messages by implementing the IDataErrorInfo interface in a model class.</span></span>


<span data-ttu-id="22b10-106">L’objectif de ce didacticiel est d’expliquer une méthode pour effectuer la validation dans une application ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="22b10-106">The goal of this tutorial is to explain one approach to performing validation in an ASP.NET MVC application.</span></span> <span data-ttu-id="22b10-107">Vous apprenez à empêcher une personne de l’envoi d’un formulaire HTML sans fournir de valeurs pour les champs obligatoires.</span><span class="sxs-lookup"><span data-stu-id="22b10-107">You learn how to prevent someone from submitting an HTML form without providing values for required form fields.</span></span> <span data-ttu-id="22b10-108">Dans ce didacticiel, vous allez apprendre à effectuer la validation à l’aide de l’interface IErrorDataInfo.</span><span class="sxs-lookup"><span data-stu-id="22b10-108">In this tutorial, you learn how to perform validation by using the IErrorDataInfo interface.</span></span>

## <a name="assumptions"></a><span data-ttu-id="22b10-109">Assumptions (Hypothèses)</span><span class="sxs-lookup"><span data-stu-id="22b10-109">Assumptions</span></span>

<span data-ttu-id="22b10-110">Dans ce didacticiel, je vais utiliser la base de données MoviesDB et la table de base de données de films.</span><span class="sxs-lookup"><span data-stu-id="22b10-110">In this tutorial, I'll use the MoviesDB database and the Movies database table.</span></span> <span data-ttu-id="22b10-111">Cette table comporte les colonnes suivantes :</span><span class="sxs-lookup"><span data-stu-id="22b10-111">This table has the following columns:</span></span>

<a id="0.6_table01"></a>


| <span data-ttu-id="22b10-112">**Nom de la colonne**</span><span class="sxs-lookup"><span data-stu-id="22b10-112">**Column Name**</span></span> | <span data-ttu-id="22b10-113">**Type de données**</span><span class="sxs-lookup"><span data-stu-id="22b10-113">**Data Type**</span></span> | <span data-ttu-id="22b10-114">**Autoriser les valeurs null**</span><span class="sxs-lookup"><span data-stu-id="22b10-114">**Allow Nulls**</span></span> |
| --- | --- | --- |
| <span data-ttu-id="22b10-115">Id</span><span class="sxs-lookup"><span data-stu-id="22b10-115">Id</span></span> | <span data-ttu-id="22b10-116">Int</span><span class="sxs-lookup"><span data-stu-id="22b10-116">Int</span></span> | <span data-ttu-id="22b10-117">False</span><span class="sxs-lookup"><span data-stu-id="22b10-117">False</span></span> |
| <span data-ttu-id="22b10-118">Titre</span><span class="sxs-lookup"><span data-stu-id="22b10-118">Title</span></span> | <span data-ttu-id="22b10-119">Nvarchar(100)</span><span class="sxs-lookup"><span data-stu-id="22b10-119">Nvarchar(100)</span></span> | <span data-ttu-id="22b10-120">False</span><span class="sxs-lookup"><span data-stu-id="22b10-120">False</span></span> |
| <span data-ttu-id="22b10-121">Directeur</span><span class="sxs-lookup"><span data-stu-id="22b10-121">Director</span></span> | <span data-ttu-id="22b10-122">Nvarchar(100)</span><span class="sxs-lookup"><span data-stu-id="22b10-122">Nvarchar(100)</span></span> | <span data-ttu-id="22b10-123">False</span><span class="sxs-lookup"><span data-stu-id="22b10-123">False</span></span> |
| <span data-ttu-id="22b10-124">DateReleased</span><span class="sxs-lookup"><span data-stu-id="22b10-124">DateReleased</span></span> | <span data-ttu-id="22b10-125">DateTime</span><span class="sxs-lookup"><span data-stu-id="22b10-125">DateTime</span></span> | <span data-ttu-id="22b10-126">False</span><span class="sxs-lookup"><span data-stu-id="22b10-126">False</span></span> |


<span data-ttu-id="22b10-127">Dans ce didacticiel, utiliser Microsoft Entity Framework pour générer des classes de mon modèle de base de données.</span><span class="sxs-lookup"><span data-stu-id="22b10-127">In this tutorial, I use the Microsoft Entity Framework to generate my database model classes.</span></span> <span data-ttu-id="22b10-128">La classe de vidéo générée par Entity Framework est affichée dans la Figure 1.</span><span class="sxs-lookup"><span data-stu-id="22b10-128">The Movie class generated by the Entity Framework is displayed in Figure 1.</span></span>


<span data-ttu-id="22b10-129">[![L’entité de film](validating-with-the-idataerrorinfo-interface-vb/_static/image1.jpg)](validating-with-the-idataerrorinfo-interface-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="22b10-129">[![The Movie entity](validating-with-the-idataerrorinfo-interface-vb/_static/image1.jpg)](validating-with-the-idataerrorinfo-interface-vb/_static/image1.png)</span></span>

<span data-ttu-id="22b10-130">**Figure 01**: entité de la séquence ([cliquez pour afficher l’image en taille réelle](validating-with-the-idataerrorinfo-interface-vb/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="22b10-130">**Figure 01**: The Movie entity([Click to view full-size image](validating-with-the-idataerrorinfo-interface-vb/_static/image2.png))</span></span>


> [!NOTE] 
> 
> <span data-ttu-id="22b10-131">Pour en savoir plus sur l’utilisation d’Entity Framework pour générer vos classes de modèle de base de données, consultez que mon didacticiel le droit de créer des Classes de modèle avec Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="22b10-131">To learn more about using the Entity Framework to generate your database model classes, see my tutorial entitled Creating Model Classes with the Entity Framework.</span></span>


## <a name="the-controller-class"></a><span data-ttu-id="22b10-132">La classe de contrôleur</span><span class="sxs-lookup"><span data-stu-id="22b10-132">The Controller Class</span></span>

<span data-ttu-id="22b10-133">Nous utilisent le contrôleur Home cinéma de liste et créer de nouveaux films.</span><span class="sxs-lookup"><span data-stu-id="22b10-133">We use the Home controller to list movies and create new movies.</span></span> <span data-ttu-id="22b10-134">Le code de cette classe est contenu dans la liste 1.</span><span class="sxs-lookup"><span data-stu-id="22b10-134">The code for this class is contained in Listing 1.</span></span>

<span data-ttu-id="22b10-135">**Listing 1 - Controllers\HomeController.vb**</span><span class="sxs-lookup"><span data-stu-id="22b10-135">**Listing 1 - Controllers\HomeController.vb**</span></span>

[!code-vb[Main](validating-with-the-idataerrorinfo-interface-vb/samples/sample1.vb)]

<span data-ttu-id="22b10-136">La classe de contrôleur d’accueil dans la liste 1 contient deux actions Create().</span><span class="sxs-lookup"><span data-stu-id="22b10-136">The Home controller class in Listing 1 contains two Create() actions.</span></span> <span data-ttu-id="22b10-137">La première action affiche le formulaire HTML pour la création d’un film de nouveau.</span><span class="sxs-lookup"><span data-stu-id="22b10-137">The first action displays the HTML form for creating a new movie.</span></span> <span data-ttu-id="22b10-138">La seconde action Create() effectue l’insertion de la nouvelle séquence dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="22b10-138">The second Create() action performs the actual insert of the new movie into the database.</span></span> <span data-ttu-id="22b10-139">La seconde action Create() est appelée lorsque le formulaire affiché par la première action Create() est envoyé au serveur.</span><span class="sxs-lookup"><span data-stu-id="22b10-139">The second Create() action is invoked when the form displayed by the first Create() action is submitted to the server.</span></span>

<span data-ttu-id="22b10-140">Notez que la seconde action Create() contient les lignes de code suivantes :</span><span class="sxs-lookup"><span data-stu-id="22b10-140">Notice that the second Create() action contains the following lines of code:</span></span>

[!code-vb[Main](validating-with-the-idataerrorinfo-interface-vb/samples/sample2.vb)]

<span data-ttu-id="22b10-141">La propriété IsValid retourne la valeur false lorsqu’il existe une erreur de validation.</span><span class="sxs-lookup"><span data-stu-id="22b10-141">The IsValid property returns false when there is a validation error.</span></span> <span data-ttu-id="22b10-142">Dans ce cas, la créer une vue qui contient le formulaire HTML pour la création d’un film s’affiche de nouveau.</span><span class="sxs-lookup"><span data-stu-id="22b10-142">In that case, the Create view that contains the HTML form for creating a movie is redisplayed.</span></span>

## <a name="creating-a-partial-class"></a><span data-ttu-id="22b10-143">Création d’une classe partielle</span><span class="sxs-lookup"><span data-stu-id="22b10-143">Creating a Partial Class</span></span>

<span data-ttu-id="22b10-144">La classe de film est générée par Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="22b10-144">The Movie class is generated by the Entity Framework.</span></span> <span data-ttu-id="22b10-145">Vous pouvez voir le code de la classe de film si vous développez le fichier MoviesDBModel.edmx dans la fenêtre de l’Explorateur de solutions et ouvrez le fichier MoviesDBModel.Designer.vb dans l’éditeur de Code (voir Figure 2).</span><span class="sxs-lookup"><span data-stu-id="22b10-145">You can see the code for the Movie class if you expand the MoviesDBModel.edmx file in the Solution Explorer window and open the MoviesDBModel.Designer.vb file in the Code Editor (see Figure 2).</span></span>


<span data-ttu-id="22b10-146">[![Le code de l’entité de film](validating-with-the-idataerrorinfo-interface-vb/_static/image2.jpg)](validating-with-the-idataerrorinfo-interface-vb/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="22b10-146">[![The code for the Movie entity](validating-with-the-idataerrorinfo-interface-vb/_static/image2.jpg)](validating-with-the-idataerrorinfo-interface-vb/_static/image3.png)</span></span>

<span data-ttu-id="22b10-147">**Figure 02**: le code de l’entité de film ([cliquez pour afficher l’image en taille réelle](validating-with-the-idataerrorinfo-interface-vb/_static/image4.png))</span><span class="sxs-lookup"><span data-stu-id="22b10-147">**Figure 02**: The code for the Movie entity([Click to view full-size image](validating-with-the-idataerrorinfo-interface-vb/_static/image4.png))</span></span>


<span data-ttu-id="22b10-148">La classe de film est une classe partielle.</span><span class="sxs-lookup"><span data-stu-id="22b10-148">The Movie class is a partial class.</span></span> <span data-ttu-id="22b10-149">Cela signifie que nous pouvons ajouter une autre classe partielle portant le même nom pour étendre les fonctionnalités de la classe de film.</span><span class="sxs-lookup"><span data-stu-id="22b10-149">That means that we can add another partial class with the same name to extend the functionality of the Movie class.</span></span> <span data-ttu-id="22b10-150">Nous allons ajouter notre logique de validation à la nouvelle classe partielle.</span><span class="sxs-lookup"><span data-stu-id="22b10-150">We'll add our validation logic to the new partial class.</span></span>

<span data-ttu-id="22b10-151">Ajoutez la classe dans la liste 2 dans le dossier de modèles.</span><span class="sxs-lookup"><span data-stu-id="22b10-151">Add the class in Listing 2 to the Models folder.</span></span>

<span data-ttu-id="22b10-152">**Listing 2 - Models\Movie.vb**</span><span class="sxs-lookup"><span data-stu-id="22b10-152">**Listing 2 - Models\Movie.vb**</span></span>

[!code-vb[Main](validating-with-the-idataerrorinfo-interface-vb/samples/sample3.vb)]

<span data-ttu-id="22b10-153">Notez que la classe dans la liste 2 inclut les *partielle* modificateur.</span><span class="sxs-lookup"><span data-stu-id="22b10-153">Notice that the class in Listing 2 includes the *partial* modifier.</span></span> <span data-ttu-id="22b10-154">Les méthodes ou les propriétés que vous ajoutez à cette classe deviennent partie de la classe de vidéo générée par Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="22b10-154">Any methods or properties that you add to this class become part of the Movie class generated by the Entity Framework.</span></span>

## <a name="adding-onchanging-and-onchanged-partial-methods"></a><span data-ttu-id="22b10-155">Ajout de OnChanging et méthodes de OnChanged partielle</span><span class="sxs-lookup"><span data-stu-id="22b10-155">Adding OnChanging and OnChanged Partial Methods</span></span>

<span data-ttu-id="22b10-156">Lorsque l’Entity Framework génère une classe d’entité, Entity Framework ajoute automatiquement les méthodes partielles à la classe.</span><span class="sxs-lookup"><span data-stu-id="22b10-156">When the Entity Framework generates an entity class, the Entity Framework adds partial methods to the class automatically.</span></span> <span data-ttu-id="22b10-157">Entity Framework génère des méthodes partielles OnChanging et OnChanged qui correspondent à chaque propriété de la classe.</span><span class="sxs-lookup"><span data-stu-id="22b10-157">The Entity Framework generates OnChanging and OnChanged partial methods that correspond to each property of the class.</span></span>

<span data-ttu-id="22b10-158">Dans le cas de la classe de film, Entity Framework crée des méthodes suivantes :</span><span class="sxs-lookup"><span data-stu-id="22b10-158">In the case of the Movie class, the Entity Framework creates the following methods:</span></span>

- <span data-ttu-id="22b10-159">OnIdChanging</span><span class="sxs-lookup"><span data-stu-id="22b10-159">OnIdChanging</span></span>
- <span data-ttu-id="22b10-160">OnIdChanged</span><span class="sxs-lookup"><span data-stu-id="22b10-160">OnIdChanged</span></span>
- <span data-ttu-id="22b10-161">OnTitleChanging</span><span class="sxs-lookup"><span data-stu-id="22b10-161">OnTitleChanging</span></span>
- <span data-ttu-id="22b10-162">OnTitleChanged</span><span class="sxs-lookup"><span data-stu-id="22b10-162">OnTitleChanged</span></span>
- <span data-ttu-id="22b10-163">OnDirectorChanging</span><span class="sxs-lookup"><span data-stu-id="22b10-163">OnDirectorChanging</span></span>
- <span data-ttu-id="22b10-164">OnDirectorChanged</span><span class="sxs-lookup"><span data-stu-id="22b10-164">OnDirectorChanged</span></span>
- <span data-ttu-id="22b10-165">OnDateReleasedChanging</span><span class="sxs-lookup"><span data-stu-id="22b10-165">OnDateReleasedChanging</span></span>
- <span data-ttu-id="22b10-166">OnDateReleasedChanged</span><span class="sxs-lookup"><span data-stu-id="22b10-166">OnDateReleasedChanged</span></span>

<span data-ttu-id="22b10-167">La méthode OnChanging est appelée droite avant la modification de la propriété correspondante.</span><span class="sxs-lookup"><span data-stu-id="22b10-167">The OnChanging method is called right before the corresponding property is changed.</span></span> <span data-ttu-id="22b10-168">La méthode OnChanged est appelée droit une fois que la propriété est modifiée.</span><span class="sxs-lookup"><span data-stu-id="22b10-168">The OnChanged method is called right after the property is changed.</span></span>

<span data-ttu-id="22b10-169">Vous pouvez tirer parti de ces méthodes partielles pour ajouter la logique de validation à la classe de film.</span><span class="sxs-lookup"><span data-stu-id="22b10-169">You can take advantage of these partial methods to add validation logic to the Movie class.</span></span> <span data-ttu-id="22b10-170">La mise à jour de classe de film dans le Listing 3 vérifie que les propriétés Title et directeur sont affectées des valeurs non vides.</span><span class="sxs-lookup"><span data-stu-id="22b10-170">The update Movie class in Listing 3 verifies that the Title and Director properties are assigned nonempty values.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="22b10-171">Une méthode partielle est une méthode définie dans une classe que vous n’êtes pas obligé d’implémenter.</span><span class="sxs-lookup"><span data-stu-id="22b10-171">A partial method is a method defined in a class that you are not required to implement.</span></span> <span data-ttu-id="22b10-172">Si vous n’implémentez pas une méthode partielle ensuite le compilateur supprime la signature de méthode et tous les appels à la méthode, par conséquent, il n’existe aucun coût d’exécution associé à la méthode partielle.</span><span class="sxs-lookup"><span data-stu-id="22b10-172">If you don't implement a partial method then the compiler removes the method signature and all calls to the method so there are no run-time costs associated with the partial method.</span></span> <span data-ttu-id="22b10-173">Dans l’éditeur de Code Visual Studio, vous pouvez ajouter une méthode partielle en tapant le mot clé *partielle* suivi d’un espace pour afficher une liste d’aucun à implémenter.</span><span class="sxs-lookup"><span data-stu-id="22b10-173">In the Visual Studio Code Editor, you can add a partial method by typing the keyword *partial* followed by a space to view a list of partials to implement.</span></span>


<span data-ttu-id="22b10-174">**La liste 3 - Models\Movie.vb**</span><span class="sxs-lookup"><span data-stu-id="22b10-174">**Listing 3 - Models\Movie.vb**</span></span>

[!code-vb[Main](validating-with-the-idataerrorinfo-interface-vb/samples/sample4.vb)]

<span data-ttu-id="22b10-175">Par exemple, si vous essayez d’affecter une chaîne vide à la propriété de titre, un message d’erreur est attribué à un dictionnaire nommé \_erreurs.</span><span class="sxs-lookup"><span data-stu-id="22b10-175">For example, if you attempt to assign an empty string to the Title property, then an error message is assigned to a Dictionary named \_errors.</span></span>

<span data-ttu-id="22b10-176">À ce stade, rien ne réellement se produit lorsque vous assignez une chaîne vide à la propriété de titre et une erreur est ajoutée à privé \_champ d’erreurs.</span><span class="sxs-lookup"><span data-stu-id="22b10-176">At this point, nothing actually happens when you assign an empty string to the Title property and an error is added to the private \_errors field.</span></span> <span data-ttu-id="22b10-177">Nous devons implémenter l’interface IDataErrorInfo pour exposer ces erreurs de validation de l’infrastructure ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="22b10-177">We need to implement the IDataErrorInfo interface to expose these validation errors to the ASP.NET MVC framework.</span></span>

## <a name="implementing-the-idataerrorinfo-interface"></a><span data-ttu-id="22b10-178">Implémentation de l’Interface IDataErrorInfo</span><span class="sxs-lookup"><span data-stu-id="22b10-178">Implementing the IDataErrorInfo Interface</span></span>

<span data-ttu-id="22b10-179">L’interface IDataErrorInfo a fait partie du .NET framework depuis la première version.</span><span class="sxs-lookup"><span data-stu-id="22b10-179">The IDataErrorInfo interface has been part of the .NET framework since the first version.</span></span> <span data-ttu-id="22b10-180">Cette interface est une interface très simple :</span><span class="sxs-lookup"><span data-stu-id="22b10-180">This interface is a very simple interface:</span></span>

[!code-vb[Main](validating-with-the-idataerrorinfo-interface-vb/samples/sample5.vb)]

<span data-ttu-id="22b10-181">Si une classe implémente l’interface IDataErrorInfo, l’infrastructure ASP.NET MVC utilise cette interface lors de la création d’une instance de la classe.</span><span class="sxs-lookup"><span data-stu-id="22b10-181">If a class implements the IDataErrorInfo interface, the ASP.NET MVC framework will use this interface when creating an instance of the class.</span></span> <span data-ttu-id="22b10-182">Par exemple, le contrôleur Home Create() action accepte une instance de la classe de film :</span><span class="sxs-lookup"><span data-stu-id="22b10-182">For example, the Home controller Create() action accepts an instance of the Movie class:</span></span>

[!code-vb[Main](validating-with-the-idataerrorinfo-interface-vb/samples/sample6.vb)]

<span data-ttu-id="22b10-183">L’infrastructure ASP.NET MVC crée l’instance de la séquence passée à l’action Create() à l’aide d’un classeur de modèles (la DefaultModelBinder).</span><span class="sxs-lookup"><span data-stu-id="22b10-183">The ASP.NET MVC framework creates the instance of the Movie passed to the Create() action by using a model binder (the DefaultModelBinder).</span></span> <span data-ttu-id="22b10-184">Le classeur de modèles est responsable de la création d’une instance de l’objet séquence par les champs de formulaire HTML de la liaison à une instance de l’objet.</span><span class="sxs-lookup"><span data-stu-id="22b10-184">The model binder is responsible for creating an instance of the Movie object by binding the HTML form fields to an instance of the Movie object.</span></span>

<span data-ttu-id="22b10-185">Le DefaultModelBinder de détecter si une classe implémente l’interface IDataErrorInfo.</span><span class="sxs-lookup"><span data-stu-id="22b10-185">The DefaultModelBinder detects whether or not a class implements the IDataErrorInfo interface.</span></span> <span data-ttu-id="22b10-186">Si une classe implémente cette interface le classeur de modèles appelle l’indexeur IDataErrorInfo.this pour chaque propriété de la classe.</span><span class="sxs-lookup"><span data-stu-id="22b10-186">If a class implements this interface then the model binder invokes the IDataErrorInfo.this indexer for each property of the class.</span></span> <span data-ttu-id="22b10-187">Si l’indexeur retourne un message d’erreur le classeur de modèles ajoute ce message d’erreur à l’état de modèle automatiquement.</span><span class="sxs-lookup"><span data-stu-id="22b10-187">If the indexer returns an error message then the model binder adds this error message to model state automatically.</span></span>

<span data-ttu-id="22b10-188">Le DefaultModelBinder vérifie également la propriété IDataErrorInfo.Error.</span><span class="sxs-lookup"><span data-stu-id="22b10-188">The DefaultModelBinder also checks the IDataErrorInfo.Error property.</span></span> <span data-ttu-id="22b10-189">Cette propriété est conçue pour représenter les erreurs de validation spécifique de propriétés de non associés à la classe.</span><span class="sxs-lookup"><span data-stu-id="22b10-189">This property is intended to represent non-property specific validation errors associated with the class.</span></span> <span data-ttu-id="22b10-190">Par exemple, vous pouvez souhaiter appliquer une règle de validation qui dépend des valeurs de plusieurs propriétés de la classe de film.</span><span class="sxs-lookup"><span data-stu-id="22b10-190">For example, you might want to enforce a validation rule that depends on the values of multiple properties of the Movie class.</span></span> <span data-ttu-id="22b10-191">Dans ce cas, vous retournerait une erreur de validation de la propriété de l’erreur.</span><span class="sxs-lookup"><span data-stu-id="22b10-191">In that case, you would return a validation error from the Error property.</span></span>

<span data-ttu-id="22b10-192">La classe de film mis à jour dans la liste 4 implémente l’interface IDataErrorInfo.</span><span class="sxs-lookup"><span data-stu-id="22b10-192">The updated Movie class in Listing 4 implements the IDataErrorInfo interface.</span></span>

<span data-ttu-id="22b10-193">**La liste 4 - Models\Movie.vb (implémente IDataErrorInfo)**</span><span class="sxs-lookup"><span data-stu-id="22b10-193">**Listing 4 - Models\Movie.vb (implements IDataErrorInfo)**</span></span>

[!code-vb[Main](validating-with-the-idataerrorinfo-interface-vb/samples/sample7.vb)]

<span data-ttu-id="22b10-194">Dans la liste 4, la propriété d’indexeur vérifie le \_collection d’erreurs pour voir s’il contient une clé qui correspond au nom de propriété passé à l’indexeur.</span><span class="sxs-lookup"><span data-stu-id="22b10-194">In Listing 4, the indexer property checks the \_errors collection to see if it contains a key that corresponds to the property name passed to the indexer.</span></span> <span data-ttu-id="22b10-195">Si aucune erreur de validation associé à la propriété n’est une chaîne vide est retournée.</span><span class="sxs-lookup"><span data-stu-id="22b10-195">If there is no validation error associated with the property then an empty string is returned.</span></span>

<span data-ttu-id="22b10-196">Vous n’avez pas besoin de modifier le contrôleur Home de quelque manière d’utiliser la classe de film modifiée.</span><span class="sxs-lookup"><span data-stu-id="22b10-196">You don't need to modify the Home controller in any way to use the modified Movie class.</span></span> <span data-ttu-id="22b10-197">La page affichée dans la Figure 3 illustre que se passe-t-il quand aucune valeur n’est entrée pour les champs de formulaire titre ou directeur.</span><span class="sxs-lookup"><span data-stu-id="22b10-197">The page displayed in Figure 3 illustrates what happens when no value is entered for the Title or Director form fields.</span></span>


<span data-ttu-id="22b10-198">[![Création automatique de méthodes d’action](validating-with-the-idataerrorinfo-interface-vb/_static/image3.jpg)](validating-with-the-idataerrorinfo-interface-vb/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="22b10-198">[![Creating action methods automatically](validating-with-the-idataerrorinfo-interface-vb/_static/image3.jpg)](validating-with-the-idataerrorinfo-interface-vb/_static/image5.png)</span></span>

<span data-ttu-id="22b10-199">**Figure 03**: un formulaire avec des valeurs manquantes ([cliquez pour afficher l’image en taille réelle](validating-with-the-idataerrorinfo-interface-vb/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="22b10-199">**Figure 03**: A form with missing values ([Click to view full-size image](validating-with-the-idataerrorinfo-interface-vb/_static/image6.png))</span></span>


<span data-ttu-id="22b10-200">Notez que la valeur DateReleased est automatiquement validée.</span><span class="sxs-lookup"><span data-stu-id="22b10-200">Notice that the DateReleased value is validated automatically.</span></span> <span data-ttu-id="22b10-201">Étant donné que la propriété DateReleased n’accepte pas les valeurs NULL, la DefaultModelBinder génère automatiquement une erreur de validation pour cette propriété lorsqu’il n’a pas de valeur.</span><span class="sxs-lookup"><span data-stu-id="22b10-201">Because the DateReleased property does not accept NULL values, the DefaultModelBinder generates a validation error for this property automatically when it does not have a value.</span></span> <span data-ttu-id="22b10-202">Si vous souhaitez modifier le message d’erreur pour la propriété DateReleased, vous devez créer un classeur de modèles personnalisés.</span><span class="sxs-lookup"><span data-stu-id="22b10-202">If you want to modify the error message for the DateReleased property then you need to create a custom model binder.</span></span>

## <a name="summary"></a><span data-ttu-id="22b10-203">Récapitulatif</span><span class="sxs-lookup"><span data-stu-id="22b10-203">Summary</span></span>

<span data-ttu-id="22b10-204">Dans ce didacticiel, vous avez appris à utiliser l’interface IDataErrorInfo pour générer des messages d’erreur de validation.</span><span class="sxs-lookup"><span data-stu-id="22b10-204">In this tutorial, you learned how to use the IDataErrorInfo interface to generate validation error messages.</span></span> <span data-ttu-id="22b10-205">Tout d’abord, nous avons créé une classe partielle de film qui étend les fonctionnalités de la classe partielle de vidéo générée par Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="22b10-205">First, we created a partial Movie class that extends the functionality of the partial Movie class generated by the Entity Framework.</span></span> <span data-ttu-id="22b10-206">Ensuite, nous avons ajouté la logique de validation pour les films classe OnTitleChanging() et OnDirectorChanging() méthodes partielles.</span><span class="sxs-lookup"><span data-stu-id="22b10-206">Next, we added validation logic to the Movie class OnTitleChanging() and OnDirectorChanging() partial methods.</span></span> <span data-ttu-id="22b10-207">Enfin, nous avons implémenté l’interface IDataErrorInfo afin d’exposer ces messages de validation de l’infrastructure ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="22b10-207">Finally, we implemented the IDataErrorInfo interface in order to expose these validation messages to the ASP.NET MVC framework.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="22b10-208">[Précédent](performing-simple-validation-vb.md)
> [Suivant](validating-with-a-service-layer-vb.md)</span><span class="sxs-lookup"><span data-stu-id="22b10-208">[Previous](performing-simple-validation-vb.md)
[Next](validating-with-a-service-layer-vb.md)</span></span>
