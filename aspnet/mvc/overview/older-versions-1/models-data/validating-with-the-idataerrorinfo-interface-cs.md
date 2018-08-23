---
uid: mvc/overview/older-versions-1/models-data/validating-with-the-idataerrorinfo-interface-cs
title: Validation avec l’Interface IDataErrorInfo (c#) | Microsoft Docs
author: StephenWalther
description: Stephen Walther vous montre comment afficher des messages d’erreur de validation personnalisée en implémentant l’interface IDataErrorInfo dans une classe de modèle.
ms.author: riande
ms.date: 03/02/2009
ms.assetid: 4733b9f1-9999-48fb-8b73-6038fbcc5ecb
msc.legacyurl: /mvc/overview/older-versions-1/models-data/validating-with-the-idataerrorinfo-interface-cs
msc.type: authoredcontent
ms.openlocfilehash: b80535db32c4567135407aeb99967bb40c279ddb
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41838681"
---
<a name="validating-with-the-idataerrorinfo-interface-c"></a><span data-ttu-id="b7bf6-103">Validation avec l’Interface IDataErrorInfo (c#)</span><span class="sxs-lookup"><span data-stu-id="b7bf6-103">Validating with the IDataErrorInfo Interface (C#)</span></span>
====================
<span data-ttu-id="b7bf6-104">par [Stephen Walther](https://github.com/StephenWalther)</span><span class="sxs-lookup"><span data-stu-id="b7bf6-104">by [Stephen Walther](https://github.com/StephenWalther)</span></span>

> <span data-ttu-id="b7bf6-105">Stephen Walther vous montre comment afficher des messages d’erreur de validation personnalisée en implémentant l’interface IDataErrorInfo dans une classe de modèle.</span><span class="sxs-lookup"><span data-stu-id="b7bf6-105">Stephen Walther shows you how to display custom validation error messages by implementing the IDataErrorInfo interface in a model class.</span></span>


<span data-ttu-id="b7bf6-106">L’objectif de ce didacticiel est d’expliquer une approche pour la validation dans une application ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="b7bf6-106">The goal of this tutorial is to explain one approach to performing validation in an ASP.NET MVC application.</span></span> <span data-ttu-id="b7bf6-107">Vous allez apprendre à empêcher un utilisateur d’envoyer un formulaire HTML sans fournir de valeurs pour les champs obligatoires.</span><span class="sxs-lookup"><span data-stu-id="b7bf6-107">You learn how to prevent someone from submitting an HTML form without providing values for required form fields.</span></span> <span data-ttu-id="b7bf6-108">Dans ce didacticiel, vous allez apprendre à effectuer la validation à l’aide de l’interface IErrorDataInfo.</span><span class="sxs-lookup"><span data-stu-id="b7bf6-108">In this tutorial, you learn how to perform validation by using the IErrorDataInfo interface.</span></span>

## <a name="assumptions"></a><span data-ttu-id="b7bf6-109">Assumptions (Hypothèses)</span><span class="sxs-lookup"><span data-stu-id="b7bf6-109">Assumptions</span></span>

<span data-ttu-id="b7bf6-110">Dans ce didacticiel, je vais utiliser la base de données MoviesDB et la table de base de données de films.</span><span class="sxs-lookup"><span data-stu-id="b7bf6-110">In this tutorial, I'll use the MoviesDB database and the Movies database table.</span></span> <span data-ttu-id="b7bf6-111">Cette table comporte les colonnes suivantes :</span><span class="sxs-lookup"><span data-stu-id="b7bf6-111">This table has the following columns:</span></span>

<a id="0.5_table01"></a>


| <span data-ttu-id="b7bf6-112">**Nom de colonne**</span><span class="sxs-lookup"><span data-stu-id="b7bf6-112">**Column Name**</span></span> | <span data-ttu-id="b7bf6-113">**Type de données**</span><span class="sxs-lookup"><span data-stu-id="b7bf6-113">**Data Type**</span></span> | <span data-ttu-id="b7bf6-114">**Autoriser les valeurs null**</span><span class="sxs-lookup"><span data-stu-id="b7bf6-114">**Allow Nulls**</span></span> |
| --- | --- | --- |
| <span data-ttu-id="b7bf6-115">Id</span><span class="sxs-lookup"><span data-stu-id="b7bf6-115">Id</span></span> | <span data-ttu-id="b7bf6-116">Int</span><span class="sxs-lookup"><span data-stu-id="b7bf6-116">Int</span></span> | <span data-ttu-id="b7bf6-117">False</span><span class="sxs-lookup"><span data-stu-id="b7bf6-117">False</span></span> |
| <span data-ttu-id="b7bf6-118">Titre</span><span class="sxs-lookup"><span data-stu-id="b7bf6-118">Title</span></span> | <span data-ttu-id="b7bf6-119">nvarchar (100)</span><span class="sxs-lookup"><span data-stu-id="b7bf6-119">Nvarchar(100)</span></span> | <span data-ttu-id="b7bf6-120">False</span><span class="sxs-lookup"><span data-stu-id="b7bf6-120">False</span></span> |
| <span data-ttu-id="b7bf6-121">Directeur</span><span class="sxs-lookup"><span data-stu-id="b7bf6-121">Director</span></span> | <span data-ttu-id="b7bf6-122">nvarchar (100)</span><span class="sxs-lookup"><span data-stu-id="b7bf6-122">Nvarchar(100)</span></span> | <span data-ttu-id="b7bf6-123">False</span><span class="sxs-lookup"><span data-stu-id="b7bf6-123">False</span></span> |
| <span data-ttu-id="b7bf6-124">DateReleased</span><span class="sxs-lookup"><span data-stu-id="b7bf6-124">DateReleased</span></span> | <span data-ttu-id="b7bf6-125">DateTime</span><span class="sxs-lookup"><span data-stu-id="b7bf6-125">DateTime</span></span> | <span data-ttu-id="b7bf6-126">False</span><span class="sxs-lookup"><span data-stu-id="b7bf6-126">False</span></span> |


<span data-ttu-id="b7bf6-127">Dans ce didacticiel, j’utilise Microsoft Entity Framework pour générer mes classes de modèle de base de données.</span><span class="sxs-lookup"><span data-stu-id="b7bf6-127">In this tutorial, I use the Microsoft Entity Framework to generate my database model classes.</span></span> <span data-ttu-id="b7bf6-128">La classe Movie générée par Entity Framework s’affiche dans la Figure 1.</span><span class="sxs-lookup"><span data-stu-id="b7bf6-128">The Movie class generated by the Entity Framework is displayed in Figure 1.</span></span>


<span data-ttu-id="b7bf6-129">[![L’entité de film](validating-with-the-idataerrorinfo-interface-cs/_static/image1.jpg)](validating-with-the-idataerrorinfo-interface-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="b7bf6-129">[![The Movie entity](validating-with-the-idataerrorinfo-interface-cs/_static/image1.jpg)](validating-with-the-idataerrorinfo-interface-cs/_static/image1.png)</span></span>

<span data-ttu-id="b7bf6-130">**Figure 01**: entité de la vidéo ([cliquez pour afficher l’image en taille réelle](validating-with-the-idataerrorinfo-interface-cs/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="b7bf6-130">**Figure 01**: The Movie entity([Click to view full-size image](validating-with-the-idataerrorinfo-interface-cs/_static/image2.png))</span></span>


> [!NOTE] 
> 
> <span data-ttu-id="b7bf6-131">Pour en savoir plus sur l’utilisation d’Entity Framework pour générer vos classes de modèle de base de données, consultez que mon didacticiel intitulé Création des Classes de modèle avec Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="b7bf6-131">To learn more about using the Entity Framework to generate your database model classes, see my tutorial entitled Creating Model Classes with the Entity Framework.</span></span>


## <a name="the-controller-class"></a><span data-ttu-id="b7bf6-132">La classe de contrôleur</span><span class="sxs-lookup"><span data-stu-id="b7bf6-132">The Controller Class</span></span>

<span data-ttu-id="b7bf6-133">Nous utilisons le contrôleur Home cinéma de liste et que vous créez de nouveaux films.</span><span class="sxs-lookup"><span data-stu-id="b7bf6-133">We use the Home controller to list movies and create new movies.</span></span> <span data-ttu-id="b7bf6-134">Le code de cette classe est contenu dans le Listing 1.</span><span class="sxs-lookup"><span data-stu-id="b7bf6-134">The code for this class is contained in Listing 1.</span></span>

<span data-ttu-id="b7bf6-135">**Liste 1 - Controllers\HomeController.cs**</span><span class="sxs-lookup"><span data-stu-id="b7bf6-135">**Listing 1 - Controllers\HomeController.cs**</span></span>

[!code-csharp[Main](validating-with-the-idataerrorinfo-interface-cs/samples/sample1.cs)]

<span data-ttu-id="b7bf6-136">La classe de contrôleur d’accueil dans le Listing 1 contient deux actions Create().</span><span class="sxs-lookup"><span data-stu-id="b7bf6-136">The Home controller class in Listing 1 contains two Create() actions.</span></span> <span data-ttu-id="b7bf6-137">La première action affiche le formulaire HTML pour la création d’un nouveau film.</span><span class="sxs-lookup"><span data-stu-id="b7bf6-137">The first action displays the HTML form for creating a new movie.</span></span> <span data-ttu-id="b7bf6-138">La deuxième action Create() effectue l’insertion de ce nouveau film dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="b7bf6-138">The second Create() action performs the actual insert of the new movie into the database.</span></span> <span data-ttu-id="b7bf6-139">La deuxième action Create() est appelée lorsque le formulaire affiché par la première action Create() est envoyé au serveur.</span><span class="sxs-lookup"><span data-stu-id="b7bf6-139">The second Create() action is invoked when the form displayed by the first Create() action is submitted to the server.</span></span>

<span data-ttu-id="b7bf6-140">Notez que la seconde action Create() contient les lignes de code suivantes :</span><span class="sxs-lookup"><span data-stu-id="b7bf6-140">Notice that the second Create() action contains the following lines of code:</span></span>

[!code-csharp[Main](validating-with-the-idataerrorinfo-interface-cs/samples/sample2.cs)]

<span data-ttu-id="b7bf6-141">La propriété IsValid retourne false quand il existe une erreur de validation.</span><span class="sxs-lookup"><span data-stu-id="b7bf6-141">The IsValid property returns false when there is a validation error.</span></span> <span data-ttu-id="b7bf6-142">Dans ce cas, la vue Create qui contient le formulaire HTML pour la création d’un film est réaffichée.</span><span class="sxs-lookup"><span data-stu-id="b7bf6-142">In that case, the Create view that contains the HTML form for creating a movie is redisplayed.</span></span>

## <a name="creating-a-partial-class"></a><span data-ttu-id="b7bf6-143">Création d’une classe partielle</span><span class="sxs-lookup"><span data-stu-id="b7bf6-143">Creating a Partial Class</span></span>

<span data-ttu-id="b7bf6-144">La classe Movie est générée par Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="b7bf6-144">The Movie class is generated by the Entity Framework.</span></span> <span data-ttu-id="b7bf6-145">Vous pouvez voir le code de la classe Movie si vous développez le fichier MoviesDBModel.edmx dans la fenêtre Explorateur de solutions et ouvrez le fichier MoviesDBModel.Designer.cs dans l’éditeur de Code (voir Figure 2).</span><span class="sxs-lookup"><span data-stu-id="b7bf6-145">You can see the code for the Movie class if you expand the MoviesDBModel.edmx file in the Solution Explorer window and open the MoviesDBModel.Designer.cs file in the Code Editor (see Figure 2).</span></span>


<span data-ttu-id="b7bf6-146">[![Le code de l’entité de film](validating-with-the-idataerrorinfo-interface-cs/_static/image2.jpg)](validating-with-the-idataerrorinfo-interface-cs/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="b7bf6-146">[![The code for the Movie entity](validating-with-the-idataerrorinfo-interface-cs/_static/image2.jpg)](validating-with-the-idataerrorinfo-interface-cs/_static/image3.png)</span></span>

<span data-ttu-id="b7bf6-147">**Figure 02**: le code de l’entité de film ([cliquez pour afficher l’image en taille réelle](validating-with-the-idataerrorinfo-interface-cs/_static/image4.png))</span><span class="sxs-lookup"><span data-stu-id="b7bf6-147">**Figure 02**: The code for the Movie entity([Click to view full-size image](validating-with-the-idataerrorinfo-interface-cs/_static/image4.png))</span></span>


<span data-ttu-id="b7bf6-148">La classe Movie est une classe partielle.</span><span class="sxs-lookup"><span data-stu-id="b7bf6-148">The Movie class is a partial class.</span></span> <span data-ttu-id="b7bf6-149">Cela signifie que nous pouvons ajouter une autre classe partielle portant le même nom pour étendre les fonctionnalités de la classe Movie.</span><span class="sxs-lookup"><span data-stu-id="b7bf6-149">That means that we can add another partial class with the same name to extend the functionality of the Movie class.</span></span> <span data-ttu-id="b7bf6-150">Nous allons ajouter notre logique de validation à la nouvelle classe partielle.</span><span class="sxs-lookup"><span data-stu-id="b7bf6-150">We'll add our validation logic to the new partial class.</span></span>

<span data-ttu-id="b7bf6-151">Ajoutez la classe dans le Listing 2 dans le dossier Modèles.</span><span class="sxs-lookup"><span data-stu-id="b7bf6-151">Add the class in Listing 2 to the Models folder.</span></span>

<span data-ttu-id="b7bf6-152">**Listing 2 - Models\Movie.cs**</span><span class="sxs-lookup"><span data-stu-id="b7bf6-152">**Listing 2 - Models\Movie.cs**</span></span>

[!code-csharp[Main](validating-with-the-idataerrorinfo-interface-cs/samples/sample3.cs)]

<span data-ttu-id="b7bf6-153">Notez que la classe dans le Listing 2 inclut le *partielle* modificateur.</span><span class="sxs-lookup"><span data-stu-id="b7bf6-153">Notice that the class in Listing 2 includes the *partial* modifier.</span></span> <span data-ttu-id="b7bf6-154">Les méthodes ou les propriétés que vous ajoutez à cette classe font partie de la classe Movie générée par Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="b7bf6-154">Any methods or properties that you add to this class become part of the Movie class generated by the Entity Framework.</span></span>

## <a name="adding-onchanging-and-onchanged-partial-methods"></a><span data-ttu-id="b7bf6-155">Ajout de OnChanging et méthodes de OnChanged partielle</span><span class="sxs-lookup"><span data-stu-id="b7bf6-155">Adding OnChanging and OnChanged Partial Methods</span></span>

<span data-ttu-id="b7bf6-156">Quand Entity Framework génère une classe d’entité, Entity Framework ajoute automatiquement les méthodes partielles à la classe.</span><span class="sxs-lookup"><span data-stu-id="b7bf6-156">When the Entity Framework generates an entity class, the Entity Framework adds partial methods to the class automatically.</span></span> <span data-ttu-id="b7bf6-157">Entity Framework génère des méthodes partielles OnChanging et OnChanged qui correspondent à chaque propriété de la classe.</span><span class="sxs-lookup"><span data-stu-id="b7bf6-157">The Entity Framework generates OnChanging and OnChanged partial methods that correspond to each property of the class.</span></span>

<span data-ttu-id="b7bf6-158">Dans le cas de la classe Movie, Entity Framework crée des méthodes suivantes :</span><span class="sxs-lookup"><span data-stu-id="b7bf6-158">In the case of the Movie class, the Entity Framework creates the following methods:</span></span>

- <span data-ttu-id="b7bf6-159">OnIdChanging</span><span class="sxs-lookup"><span data-stu-id="b7bf6-159">OnIdChanging</span></span>
- <span data-ttu-id="b7bf6-160">OnIdChanged</span><span class="sxs-lookup"><span data-stu-id="b7bf6-160">OnIdChanged</span></span>
- <span data-ttu-id="b7bf6-161">OnTitleChanging</span><span class="sxs-lookup"><span data-stu-id="b7bf6-161">OnTitleChanging</span></span>
- <span data-ttu-id="b7bf6-162">OnTitleChanged</span><span class="sxs-lookup"><span data-stu-id="b7bf6-162">OnTitleChanged</span></span>
- <span data-ttu-id="b7bf6-163">OnDirectorChanging</span><span class="sxs-lookup"><span data-stu-id="b7bf6-163">OnDirectorChanging</span></span>
- <span data-ttu-id="b7bf6-164">OnDirectorChanged</span><span class="sxs-lookup"><span data-stu-id="b7bf6-164">OnDirectorChanged</span></span>
- <span data-ttu-id="b7bf6-165">OnDateReleasedChanging</span><span class="sxs-lookup"><span data-stu-id="b7bf6-165">OnDateReleasedChanging</span></span>
- <span data-ttu-id="b7bf6-166">OnDateReleasedChanged</span><span class="sxs-lookup"><span data-stu-id="b7bf6-166">OnDateReleasedChanged</span></span>

<span data-ttu-id="b7bf6-167">La méthode OnChanging est appelée droite avant la modification de la propriété correspondante.</span><span class="sxs-lookup"><span data-stu-id="b7bf6-167">The OnChanging method is called right before the corresponding property is changed.</span></span> <span data-ttu-id="b7bf6-168">La méthode OnChanged est appelée droit une fois que la propriété est modifiée.</span><span class="sxs-lookup"><span data-stu-id="b7bf6-168">The OnChanged method is called right after the property is changed.</span></span>

<span data-ttu-id="b7bf6-169">Vous pouvez tirer parti de ces méthodes partielles pour ajouter la logique de validation à la classe Movie.</span><span class="sxs-lookup"><span data-stu-id="b7bf6-169">You can take advantage of these partial methods to add validation logic to the Movie class.</span></span> <span data-ttu-id="b7bf6-170">La classe Movie dans le Listing 3 de la mise à jour vérifie que les propriétés Title et directeur sont affectées des valeurs non vides.</span><span class="sxs-lookup"><span data-stu-id="b7bf6-170">The update Movie class in Listing 3 verifies that the Title and Director properties are assigned nonempty values.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="b7bf6-171">Une méthode partielle est une méthode définie dans une classe que vous n’êtes pas obligé de mettre en œuvre.</span><span class="sxs-lookup"><span data-stu-id="b7bf6-171">A partial method is a method defined in a class that you are not required to implement.</span></span> <span data-ttu-id="b7bf6-172">Si vous n’implémentez une méthode partielle ensuite le compilateur supprime la signature de méthode et tous les appels à la méthode, par conséquent, il n’existe aucun coût d’exécution associé à la méthode partielle.</span><span class="sxs-lookup"><span data-stu-id="b7bf6-172">If you don't implement a partial method then the compiler removes the method signature and all calls to the method so there are no run-time costs associated with the partial method.</span></span> <span data-ttu-id="b7bf6-173">Dans l’éditeur de Code Visual Studio, vous pouvez ajouter une méthode partielle en tapant le mot clé *partielle* suivi d’un espace pour afficher une liste des vues partielles pour implémenter.</span><span class="sxs-lookup"><span data-stu-id="b7bf6-173">In the Visual Studio Code Editor, you can add a partial method by typing the keyword *partial* followed by a space to view a list of partials to implement.</span></span>


<span data-ttu-id="b7bf6-174">**Liste 3 - Models\Movie.cs**</span><span class="sxs-lookup"><span data-stu-id="b7bf6-174">**Listing 3 - Models\Movie.cs**</span></span>

[!code-csharp[Main](validating-with-the-idataerrorinfo-interface-cs/samples/sample4.cs)]

<span data-ttu-id="b7bf6-175">Par exemple, si vous essayez d’assigner une chaîne vide à la propriété de titre, un message d’erreur est ensuite affecté à un dictionnaire nommé \_erreurs.</span><span class="sxs-lookup"><span data-stu-id="b7bf6-175">For example, if you attempt to assign an empty string to the Title property, then an error message is assigned to a Dictionary named \_errors.</span></span>

<span data-ttu-id="b7bf6-176">À ce stade, rien ne réellement se produit lorsque vous assignez une chaîne vide à la propriété de titre et une erreur est ajoutée à la private \_champ des erreurs.</span><span class="sxs-lookup"><span data-stu-id="b7bf6-176">At this point, nothing actually happens when you assign an empty string to the Title property and an error is added to the private \_errors field.</span></span> <span data-ttu-id="b7bf6-177">Nous avons besoin implémenter l’interface IDataErrorInfo pour exposer ces erreurs de validation pour l’infrastructure ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="b7bf6-177">We need to implement the IDataErrorInfo interface to expose these validation errors to the ASP.NET MVC framework.</span></span>

## <a name="implementing-the-idataerrorinfo-interface"></a><span data-ttu-id="b7bf6-178">Implémentation de l’Interface IDataErrorInfo</span><span class="sxs-lookup"><span data-stu-id="b7bf6-178">Implementing the IDataErrorInfo Interface</span></span>

<span data-ttu-id="b7bf6-179">L’interface IDataErrorInfo a fait partie du .NET framework depuis la première version.</span><span class="sxs-lookup"><span data-stu-id="b7bf6-179">The IDataErrorInfo interface has been part of the .NET framework since the first version.</span></span> <span data-ttu-id="b7bf6-180">Cette interface est une interface très simple :</span><span class="sxs-lookup"><span data-stu-id="b7bf6-180">This interface is a very simple interface:</span></span>

[!code-csharp[Main](validating-with-the-idataerrorinfo-interface-cs/samples/sample5.cs)]

<span data-ttu-id="b7bf6-181">Si une classe implémente l’interface IDataErrorInfo, l’infrastructure ASP.NET MVC utilise cette interface lors de la création d’une instance de la classe.</span><span class="sxs-lookup"><span data-stu-id="b7bf6-181">If a class implements the IDataErrorInfo interface, the ASP.NET MVC framework will use this interface when creating an instance of the class.</span></span> <span data-ttu-id="b7bf6-182">Par exemple, le contrôleur Home Create() action accepte une instance de la classe Movie :</span><span class="sxs-lookup"><span data-stu-id="b7bf6-182">For example, the Home controller Create() action accepts an instance of the Movie class:</span></span>

[!code-csharp[Main](validating-with-the-idataerrorinfo-interface-cs/samples/sample6.cs)]

<span data-ttu-id="b7bf6-183">L’infrastructure ASP.NET MVC crée l’instance du film passé à l’action Create() à l’aide d’un classeur de modèles (le DefaultModelBinder).</span><span class="sxs-lookup"><span data-stu-id="b7bf6-183">The ASP.NET MVC framework creates the instance of the Movie passed to the Create() action by using a model binder (the DefaultModelBinder).</span></span> <span data-ttu-id="b7bf6-184">Le binder de modèle est responsable de la création d’une instance de l’objet de film en liant les champs de formulaire HTML à une instance de l’objet de film.</span><span class="sxs-lookup"><span data-stu-id="b7bf6-184">The model binder is responsible for creating an instance of the Movie object by binding the HTML form fields to an instance of the Movie object.</span></span>

<span data-ttu-id="b7bf6-185">Le DefaultModelBinder détecte qu’une classe implémente l’interface IDataErrorInfo ou non.</span><span class="sxs-lookup"><span data-stu-id="b7bf6-185">The DefaultModelBinder detects whether or not a class implements the IDataErrorInfo interface.</span></span> <span data-ttu-id="b7bf6-186">Si une classe implémente cette interface le binder de modèle appelle l’indexeur IDataErrorInfo.this pour chaque propriété de la classe.</span><span class="sxs-lookup"><span data-stu-id="b7bf6-186">If a class implements this interface then the model binder invokes the IDataErrorInfo.this indexer for each property of the class.</span></span> <span data-ttu-id="b7bf6-187">Si l’indexeur retourne un message d’erreur le binder de modèle ajoute ce message d’erreur pour modéliser état automatiquement.</span><span class="sxs-lookup"><span data-stu-id="b7bf6-187">If the indexer returns an error message then the model binder adds this error message to model state automatically.</span></span>

<span data-ttu-id="b7bf6-188">Le DefaultModelBinder vérifie également la propriété IDataErrorInfo.Error.</span><span class="sxs-lookup"><span data-stu-id="b7bf6-188">The DefaultModelBinder also checks the IDataErrorInfo.Error property.</span></span> <span data-ttu-id="b7bf6-189">Cette propriété est destinée à représenter les erreurs de validation spécifique de propriétés de non associés à la classe.</span><span class="sxs-lookup"><span data-stu-id="b7bf6-189">This property is intended to represent non-property specific validation errors associated with the class.</span></span> <span data-ttu-id="b7bf6-190">Par exemple, vous souhaiterez peut-être appliquer une règle de validation qui dépend des valeurs de plusieurs propriétés de la classe Movie.</span><span class="sxs-lookup"><span data-stu-id="b7bf6-190">For example, you might want to enforce a validation rule that depends on the values of multiple properties of the Movie class.</span></span> <span data-ttu-id="b7bf6-191">Dans ce cas, vous renvoie une erreur de validation à partir de la propriété de l’erreur.</span><span class="sxs-lookup"><span data-stu-id="b7bf6-191">In that case, you would return a validation error from the Error property.</span></span>

<span data-ttu-id="b7bf6-192">La classe Movie mise à jour sur la liste 4 implémente l’interface IDataErrorInfo.</span><span class="sxs-lookup"><span data-stu-id="b7bf6-192">The updated Movie class in Listing 4 implements the IDataErrorInfo interface.</span></span>

<span data-ttu-id="b7bf6-193">**Liste 4 - Models\Movie.cs (implémente l’interface IDataErrorInfo)**</span><span class="sxs-lookup"><span data-stu-id="b7bf6-193">**Listing 4 - Models\Movie.cs (implements IDataErrorInfo)**</span></span>

[!code-csharp[Main](validating-with-the-idataerrorinfo-interface-cs/samples/sample7.cs)]

<span data-ttu-id="b7bf6-194">Dans la liste 4, la propriété d’indexeur vérifie le \_collection d’erreurs pour voir si elle contient une clé qui correspond au nom de propriété passé à l’indexeur.</span><span class="sxs-lookup"><span data-stu-id="b7bf6-194">In Listing 4, the indexer property checks the \_errors collection to see if it contains a key that corresponds to the property name passed to the indexer.</span></span> <span data-ttu-id="b7bf6-195">Si aucune erreur de validation associé à la propriété n’est une chaîne vide est retournée.</span><span class="sxs-lookup"><span data-stu-id="b7bf6-195">If there is no validation error associated with the property then an empty string is returned.</span></span>

<span data-ttu-id="b7bf6-196">Vous n’avez pas besoin de modifier le contrôleur Home en aucune façon d’utiliser la classe Movie modifiée.</span><span class="sxs-lookup"><span data-stu-id="b7bf6-196">You don't need to modify the Home controller in any way to use the modified Movie class.</span></span> <span data-ttu-id="b7bf6-197">La page affichée dans la Figure 3 illustre que se passe-t-il quand aucune valeur n’est entrée pour les champs de formulaire titre ou directeur.</span><span class="sxs-lookup"><span data-stu-id="b7bf6-197">The page displayed in Figure 3 illustrates what happens when no value is entered for the Title or Director form fields.</span></span>


<span data-ttu-id="b7bf6-198">[![Création automatique de méthodes d’action](validating-with-the-idataerrorinfo-interface-cs/_static/image3.jpg)](validating-with-the-idataerrorinfo-interface-cs/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="b7bf6-198">[![Creating action methods automatically](validating-with-the-idataerrorinfo-interface-cs/_static/image3.jpg)](validating-with-the-idataerrorinfo-interface-cs/_static/image5.png)</span></span>

<span data-ttu-id="b7bf6-199">**Figure 03**: un formulaire avec des valeurs manquantes ([cliquez pour afficher l’image en taille réelle](validating-with-the-idataerrorinfo-interface-cs/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="b7bf6-199">**Figure 03**: A form with missing values ([Click to view full-size image](validating-with-the-idataerrorinfo-interface-cs/_static/image6.png))</span></span>


<span data-ttu-id="b7bf6-200">Notez que la valeur DateReleased est validée automatiquement.</span><span class="sxs-lookup"><span data-stu-id="b7bf6-200">Notice that the DateReleased value is validated automatically.</span></span> <span data-ttu-id="b7bf6-201">Étant donné que la propriété DateReleased n’accepte pas les valeurs NULL, le DefaultModelBinder génère automatiquement une erreur de validation pour cette propriété lorsqu’elle n’a pas une valeur.</span><span class="sxs-lookup"><span data-stu-id="b7bf6-201">Because the DateReleased property does not accept NULL values, the DefaultModelBinder generates a validation error for this property automatically when it does not have a value.</span></span> <span data-ttu-id="b7bf6-202">Si vous souhaitez modifier le message d’erreur pour la propriété DateReleased vous devez créer un classeur de modèles personnalisés.</span><span class="sxs-lookup"><span data-stu-id="b7bf6-202">If you want to modify the error message for the DateReleased property then you need to create a custom model binder.</span></span>

## <a name="summary"></a><span data-ttu-id="b7bf6-203">Récapitulatif</span><span class="sxs-lookup"><span data-stu-id="b7bf6-203">Summary</span></span>

<span data-ttu-id="b7bf6-204">Dans ce didacticiel, vous avez appris à utiliser l’interface IDataErrorInfo pour générer des messages d’erreur de validation.</span><span class="sxs-lookup"><span data-stu-id="b7bf6-204">In this tutorial, you learned how to use the IDataErrorInfo interface to generate validation error messages.</span></span> <span data-ttu-id="b7bf6-205">Tout d’abord, nous avons créé une classe partielle de film qui étend les fonctionnalités de la classe partielle de film générée par Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="b7bf6-205">First, we created a partial Movie class that extends the functionality of the partial Movie class generated by the Entity Framework.</span></span> <span data-ttu-id="b7bf6-206">Ensuite, nous avons ajouté la logique de validation pour les films classe OnTitleChanging() et OnDirectorChanging() méthodes partielles.</span><span class="sxs-lookup"><span data-stu-id="b7bf6-206">Next, we added validation logic to the Movie class OnTitleChanging() and OnDirectorChanging() partial methods.</span></span> <span data-ttu-id="b7bf6-207">Enfin, nous avons implémenté l’interface IDataErrorInfo afin d’exposer ces messages de validation à l’infrastructure ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="b7bf6-207">Finally, we implemented the IDataErrorInfo interface in order to expose these validation messages to the ASP.NET MVC framework.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="b7bf6-208">[Précédent](performing-simple-validation-cs.md)
> [Suivant](validating-with-a-service-layer-cs.md)</span><span class="sxs-lookup"><span data-stu-id="b7bf6-208">[Previous](performing-simple-validation-cs.md)
[Next](validating-with-a-service-layer-cs.md)</span></span>
