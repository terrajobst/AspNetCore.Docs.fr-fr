---
uid: mvc/overview/older-versions-1/models-data/validation-with-the-data-annotation-validators-vb
title: Validation avec les validateurs d’Annotation de données (VB) | Documents Microsoft
author: microsoft
description: Tirer parti de classeur de modèles de données d’Annotation pour effectuer la validation au sein d’une application ASP.NET MVC. Découvrez comment utiliser les différents types de programme de validation...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/29/2009
ms.topic: article
ms.assetid: 0d23ff2b-f2ec-434a-be3b-1180beeccba3
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/models-data/validation-with-the-data-annotation-validators-vb
msc.type: authoredcontent
ms.openlocfilehash: d1987182a44a0ad3f91f455342dc934d1dd50267
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30870165"
---
<a name="validation-with-the-data-annotation-validators-vb"></a><span data-ttu-id="ff6bc-104">Validation avec les validateurs d’Annotation de données (Visual Basic)</span><span class="sxs-lookup"><span data-stu-id="ff6bc-104">Validation with the Data Annotation Validators (VB)</span></span>
====================
<span data-ttu-id="ff6bc-105">par [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="ff6bc-105">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="ff6bc-106">Tirer parti de classeur de modèles de données d’Annotation pour effectuer la validation au sein d’une application ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="ff6bc-106">Take advantage of the Data Annotation Model Binder to perform validation within an ASP.NET MVC application.</span></span> <span data-ttu-id="ff6bc-107">Découvrez comment utiliser les différents types d’attributs de validateur et de les manipuler dans Microsoft Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="ff6bc-107">Learn how to use the different types of validator attributes and work with them in the Microsoft Entity Framework.</span></span>


<span data-ttu-id="ff6bc-108">Dans ce didacticiel, vous allez apprendre à utiliser les validateurs d’Annotation de données pour effectuer la validation dans une application ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="ff6bc-108">In this tutorial, you learn how to use the Data Annotation validators to perform validation in an ASP.NET MVC application.</span></span> <span data-ttu-id="ff6bc-109">L’avantage d’utiliser les validateurs d’Annotation de données est qu’ils permettent de vous permet d’effectuer la validation en ajoutant un ou plusieurs attributs, tels que les paramètres requis ou StringLength attribut simplement : pour une propriété de classe.</span><span class="sxs-lookup"><span data-stu-id="ff6bc-109">The advantage of using the Data Annotation validators is that they enable you to perform validation simply by adding one or more attributes – such as the Required or StringLength attribute – to a class property.</span></span>

<span data-ttu-id="ff6bc-110">Avant de pouvoir utiliser les validateurs d’Annotation de données, vous devez télécharger le classeur de modèles des Annotations de données.</span><span class="sxs-lookup"><span data-stu-id="ff6bc-110">Before you can use the Data Annotation validators, you must download the Data Annotations Model Binder.</span></span> <span data-ttu-id="ff6bc-111">Vous pouvez télécharger l’exemple de classeur du modèle d’Annotations de données depuis le site Web CodePlex en cliquant sur [ici](http://aspnet.codeplex.com/Release/ProjectReleases.aspx?ReleaseId=24471).</span><span class="sxs-lookup"><span data-stu-id="ff6bc-111">You can download the Data Annotations Model Binder Sample from the CodePlex website by clicking [here](http://aspnet.codeplex.com/Release/ProjectReleases.aspx?ReleaseId=24471).</span></span>


<span data-ttu-id="ff6bc-112">Il est important de comprendre que le classeur de modèles des Annotations de données n’est pas officiellement partie de l’infrastructure Microsoft ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="ff6bc-112">It is important to understand that the Data Annotations Model Binder is not an official part of the Microsoft ASP.NET MVC framework.</span></span> <span data-ttu-id="ff6bc-113">Bien que le classeur de modèles des Annotations de données a été créé par l’équipe Microsoft ASP.NET MVC, Microsoft n’offre pas de prise en charge officielle du produit pour le classeur de modèles des Annotations de données décrites et utilisé dans ce didacticiel.</span><span class="sxs-lookup"><span data-stu-id="ff6bc-113">Although the Data Annotations Model Binder was created by the Microsoft ASP.NET MVC team, Microsoft does not offer official product support for the Data Annotations Model Binder described and used in this tutorial.</span></span>


## <a name="using-the-data-annotation-model-binder"></a><span data-ttu-id="ff6bc-114">À l’aide du classeur de modèles de données d’Annotation</span><span class="sxs-lookup"><span data-stu-id="ff6bc-114">Using the Data Annotation Model Binder</span></span>

<span data-ttu-id="ff6bc-115">Pour utiliser le classeur de modèles des Annotations de données dans une application ASP.NET MVC, vous devez d’abord ajouter une référence à l’assembly Microsoft.Web.Mvc.DataAnnotations.dll et le fichier System.ComponentModel.DataAnnotations.dll.</span><span class="sxs-lookup"><span data-stu-id="ff6bc-115">In order to use the Data Annotations Model Binder in an ASP.NET MVC application, you first need to add a reference to the Microsoft.Web.Mvc.DataAnnotations.dll assembly and the System.ComponentModel.DataAnnotations.dll assembly.</span></span> <span data-ttu-id="ff6bc-116">Sélectionnez l’option de menu **projet, ajouter une référence**.</span><span class="sxs-lookup"><span data-stu-id="ff6bc-116">Select the menu option **Project, Add Reference**.</span></span> <span data-ttu-id="ff6bc-117">Cliquez ensuite sur le **Parcourir** onglet et accédez à l’emplacement où vous avez téléchargé (et décompressé) l’exemple de classeur de modèles de données Annotations (consultez **Figure 1**).</span><span class="sxs-lookup"><span data-stu-id="ff6bc-117">Next click the **Browse** tab and browse to the location where you downloaded (and unzipped) the Data Annotations Model Binder sample (see **Figure 1**).</span></span>

[![](validation-with-the-data-annotation-validators-vb/_static/image2.png)](validation-with-the-data-annotation-validators-vb/_static/image1.png)

<span data-ttu-id="ff6bc-118">**Figure 1**: ajout d’une référence vers le classeur de modèles de données Annotations ([cliquez pour afficher l’image en taille réelle](validation-with-the-data-annotation-validators-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="ff6bc-118">**Figure 1**: Adding a reference to the Data Annotations Model Binder ([Click to view full-size image](validation-with-the-data-annotation-validators-vb/_static/image3.png))</span></span>

<span data-ttu-id="ff6bc-119">Sélectionnez l’assembly Microsoft.Web.Mvc.DataAnnotations.dll et l’assembly de fichier System.ComponentModel.DataAnnotations.dll et cliquez sur le **OK** bouton.</span><span class="sxs-lookup"><span data-stu-id="ff6bc-119">Select both the Microsoft.Web.Mvc.DataAnnotations.dll assembly and the System.ComponentModel.DataAnnotations.dll assembly and click the **OK** button.</span></span>


<span data-ttu-id="ff6bc-120">Vous ne pouvez pas utiliser l’assembly de fichier System.ComponentModel.DataAnnotations.dll inclus avec Service Pack 1 de .NET Framework avec le classeur de modèles des Annotations de données.</span><span class="sxs-lookup"><span data-stu-id="ff6bc-120">You cannot use the System.ComponentModel.DataAnnotations.dll assembly included with .NET Framework Service Pack 1 with the Data Annotations Model Binder.</span></span> <span data-ttu-id="ff6bc-121">Vous devez utiliser la version de l’assembly de fichier System.ComponentModel.DataAnnotations.dll inclus avec le téléchargement de l’exemple de classeur de données Annotations modèle.</span><span class="sxs-lookup"><span data-stu-id="ff6bc-121">You must use the version of the System.ComponentModel.DataAnnotations.dll assembly included with the Data Annotations Model Binder Sample download.</span></span>


<span data-ttu-id="ff6bc-122">Enfin, vous devez enregistrer le classeur de modèles DataAnnotations dans le fichier Global.asax.</span><span class="sxs-lookup"><span data-stu-id="ff6bc-122">Finally, you need to register the DataAnnotations Model Binder in the Global.asax file.</span></span> <span data-ttu-id="ff6bc-123">Ajoutez la ligne de code suivante à l’Application\_Start() Gestionnaire d’événements pour que l’Application\_méthode Start() ressemble à ceci :</span><span class="sxs-lookup"><span data-stu-id="ff6bc-123">Add the following line of code to the Application\_Start() event handler so that the Application\_Start() method looks like this:</span></span>

[!code-vb[Main](validation-with-the-data-annotation-validators-vb/samples/sample1.vb)]

<span data-ttu-id="ff6bc-124">Cette ligne de code enregistre le DataAnnotationsModelBinder en tant que le classeur de modèles par défaut pour l’ensemble de l’application ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="ff6bc-124">This line of code registers the DataAnnotationsModelBinder as the default model binder for the entire ASP.NET MVC application.</span></span>

## <a name="using-the-data-annotation-validator-attributes"></a><span data-ttu-id="ff6bc-125">En utilisant les attributs de validateur d’Annotation données</span><span class="sxs-lookup"><span data-stu-id="ff6bc-125">Using the Data Annotation Validator Attributes</span></span>

<span data-ttu-id="ff6bc-126">Lorsque vous utilisez le classeur de modèles des Annotations de données, vous utilisez les attributs de validateur pour effectuer la validation.</span><span class="sxs-lookup"><span data-stu-id="ff6bc-126">When you use the Data Annotations Model Binder, you use validator attributes to perform validation.</span></span> <span data-ttu-id="ff6bc-127">L’espace de noms System.ComponentModel.DataAnnotations inclut les attributs du programme de validation suivants :</span><span class="sxs-lookup"><span data-stu-id="ff6bc-127">The System.ComponentModel.DataAnnotations namespace includes the following validator attributes:</span></span>

- <span data-ttu-id="ff6bc-128">Plage : vous permet de vérifier si la valeur d’une propriété se situe entre une plage de valeurs spécifiée.</span><span class="sxs-lookup"><span data-stu-id="ff6bc-128">Range – Enables you to validate whether the value of a property falls between a specified range of values.</span></span>
- <span data-ttu-id="ff6bc-129">ReqularExpression – permet de valider si la valeur d’une propriété correspond à un modèle d’expression régulière spécifié.</span><span class="sxs-lookup"><span data-stu-id="ff6bc-129">ReqularExpression – Enables you to validate whether the value of a property matches a specified regular expression pattern.</span></span>
- <span data-ttu-id="ff6bc-130">Requis : vous permet de marquer une propriété en fonction des besoins.</span><span class="sxs-lookup"><span data-stu-id="ff6bc-130">Required – Enables you to mark a property as required.</span></span>
- <span data-ttu-id="ff6bc-131">StringLength – permet de spécifier une longueur maximale pour une propriété de chaîne.</span><span class="sxs-lookup"><span data-stu-id="ff6bc-131">StringLength – Enables you to specify a maximum length for a string property.</span></span>
- <span data-ttu-id="ff6bc-132">Validation – la classe de base pour tous les attributs de validateur.</span><span class="sxs-lookup"><span data-stu-id="ff6bc-132">Validation – The base class for all validator attributes.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="ff6bc-133">Si vos besoins de validation ne sont pas satisfaites par les validateurs standards vous avez toujours la possibilité de créer un attribut de validateur personnalisé en héritant d’un nouvel attribut de validateur de l’attribut de Validation de base.</span><span class="sxs-lookup"><span data-stu-id="ff6bc-133">If your validation needs are not satisfied by any of the standard validators then you always have the option of creating a custom validator attribute by inheriting a new validator attribute from the base Validation attribute.</span></span>


<span data-ttu-id="ff6bc-134">La classe de produit dans **liste 1** illustre l’utilisation de ces attributs de validateur.</span><span class="sxs-lookup"><span data-stu-id="ff6bc-134">The Product class in **Listing 1** illustrates how to use these validator attributes.</span></span> <span data-ttu-id="ff6bc-135">Les propriétés de nom, Description et prix unitaire sont marquées comme requis.</span><span class="sxs-lookup"><span data-stu-id="ff6bc-135">The Name, Description, and UnitPrice properties are marked as required.</span></span> <span data-ttu-id="ff6bc-136">La propriété Name doit avoir une longueur de chaîne qui est inférieur à 10 caractères.</span><span class="sxs-lookup"><span data-stu-id="ff6bc-136">The Name property must have a string length that is less than 10 characters.</span></span> <span data-ttu-id="ff6bc-137">Enfin, la propriété UnitPrice doit correspondre à un modèle d’expression régulière qui représente un montant en devise.</span><span class="sxs-lookup"><span data-stu-id="ff6bc-137">Finally, the UnitPrice property must match a regular expression pattern that represents a currency amount.</span></span>

[!code-vb[Main](validation-with-the-data-annotation-validators-vb/samples/sample2.vb)]

<span data-ttu-id="ff6bc-138">**La liste 1**: Models\Product.vb</span><span class="sxs-lookup"><span data-stu-id="ff6bc-138">**Listing 1**: Models\Product.vb</span></span>

<span data-ttu-id="ff6bc-139">La classe Product illustre comment utiliser un attribut supplémentaire : l’attribut DisplayName.</span><span class="sxs-lookup"><span data-stu-id="ff6bc-139">The Product class illustrates how to use one additional attribute: the DisplayName attribute.</span></span> <span data-ttu-id="ff6bc-140">L’attribut DisplayName vous permet de modifier le nom de la propriété lorsque la propriété est affichée dans un message d’erreur.</span><span class="sxs-lookup"><span data-stu-id="ff6bc-140">The DisplayName attribute enables you to modify the name of the property when the property is displayed in an error message.</span></span> <span data-ttu-id="ff6bc-141">Au lieu d’afficher le message d’erreur « le champ prix unitaire est requis », vous pouvez afficher le message d’erreur « le champ prix est requis ».</span><span class="sxs-lookup"><span data-stu-id="ff6bc-141">Instead of displaying the error message "The UnitPrice field is required" you can display the error message "The Price field is required".</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="ff6bc-142">Si vous souhaitez personnaliser entièrement le message d’erreur affiché par un programme de validation vous pouvez affecter un message d’erreur personnalisé à la propriété ErrorMessage du validateur comme suit : `<Required(ErrorMessage:="This field needs a value!")>`</span><span class="sxs-lookup"><span data-stu-id="ff6bc-142">If you want to completely customize the error message displayed by a validator then you can assign a custom error message to the validator's ErrorMessage property like this: `<Required(ErrorMessage:="This field needs a value!")>`</span></span>


<span data-ttu-id="ff6bc-143">Vous pouvez utiliser la classe de produit dans **liste 1** avec l’action de contrôleur Create() dans **liste 2**.</span><span class="sxs-lookup"><span data-stu-id="ff6bc-143">You can use the Product class in **Listing 1** with the Create() controller action in **Listing 2**.</span></span> <span data-ttu-id="ff6bc-144">Cette action de contrôleur actualise la vue Create lorsque l’état du modèle contient des erreurs.</span><span class="sxs-lookup"><span data-stu-id="ff6bc-144">This controller action redisplays the Create view when model state contains any errors.</span></span>

[!code-vb[Main](validation-with-the-data-annotation-validators-vb/samples/sample3.vb)]

<span data-ttu-id="ff6bc-145">**Listing 2**: Controllers\ProductController.vb</span><span class="sxs-lookup"><span data-stu-id="ff6bc-145">**Listing 2**: Controllers\ProductController.vb</span></span>

<span data-ttu-id="ff6bc-146">Enfin, vous pouvez créer la vue dans **liste 3** en cliquant sur l’action Create() et en sélectionnant l’option de menu **ajouter une vue**.</span><span class="sxs-lookup"><span data-stu-id="ff6bc-146">Finally, you can create the view in **Listing 3** by right-clicking the Create() action and selecting the menu option **Add View**.</span></span> <span data-ttu-id="ff6bc-147">Créer une vue fortement typée avec la classe de produit en tant que la classe de modèle.</span><span class="sxs-lookup"><span data-stu-id="ff6bc-147">Create a strongly-typed view with the Product class as the model class.</span></span> <span data-ttu-id="ff6bc-148">Sélectionnez **créer** à partir de la liste déroulante contenu afficher (voir **Figure 2**).</span><span class="sxs-lookup"><span data-stu-id="ff6bc-148">Select **Create** from the view content dropdown list (see **Figure 2**).</span></span>

[![](validation-with-the-data-annotation-validators-vb/_static/image5.png)](validation-with-the-data-annotation-validators-vb/_static/image4.png)

<span data-ttu-id="ff6bc-149">**Figure 2**: ajout de la vue de créer</span><span class="sxs-lookup"><span data-stu-id="ff6bc-149">**Figure 2**: Adding the Create View</span></span>

[!code-aspx[Main](validation-with-the-data-annotation-validators-vb/samples/sample4.aspx)]

<span data-ttu-id="ff6bc-150">**La liste 3**: Views\Product\Create.aspx</span><span class="sxs-lookup"><span data-stu-id="ff6bc-150">**Listing 3**: Views\Product\Create.aspx</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="ff6bc-151">Supprimez le champ Id le formulaire de création généré par le **ajouter une vue** option de menu.</span><span class="sxs-lookup"><span data-stu-id="ff6bc-151">Remove the Id field from the Create form generated by the **Add View** menu option.</span></span> <span data-ttu-id="ff6bc-152">Étant donné que le champ Id correspond à une colonne d’identité, vous ne souhaitez autoriser les utilisateurs à entrer une valeur pour ce champ.</span><span class="sxs-lookup"><span data-stu-id="ff6bc-152">Because the Id field corresponds to an Identity column, you don't want to allow users to enter a value for this field.</span></span>


<span data-ttu-id="ff6bc-153">Si vous envoyez le formulaire pour la création d’un produit et que vous n’entrez pas de valeurs pour les champs obligatoires, alors que l’erreur de validation des messages dans **Figure 3** sont affichés.</span><span class="sxs-lookup"><span data-stu-id="ff6bc-153">If you submit the form for creating a Product and you do not enter values for the required fields, then the validation error messages in **Figure 3** are displayed.</span></span>

[![](validation-with-the-data-annotation-validators-vb/_static/image7.png)](validation-with-the-data-annotation-validators-vb/_static/image6.png)

<span data-ttu-id="ff6bc-154">**Figure 3**: les champs requis manquants</span><span class="sxs-lookup"><span data-stu-id="ff6bc-154">**Figure 3**: Missing required fields</span></span>

<span data-ttu-id="ff6bc-155">Si vous entrez un montant en devise non valide, puis le message d’erreur dans **Figure 4** s’affiche.</span><span class="sxs-lookup"><span data-stu-id="ff6bc-155">If you enter an invalid currency amount, then the error message in **Figure 4** is displayed.</span></span>

[![](validation-with-the-data-annotation-validators-vb/_static/image9.png)](validation-with-the-data-annotation-validators-vb/_static/image8.png)

<span data-ttu-id="ff6bc-156">**Figure 4**: montant en devise non valide</span><span class="sxs-lookup"><span data-stu-id="ff6bc-156">**Figure 4**: Invalid currency amount</span></span>

## <a name="using-data-annotation-validators-with-the-entity-framework"></a><span data-ttu-id="ff6bc-157">À l’aide de validateurs d’Annotation de données avec Entity Framework</span><span class="sxs-lookup"><span data-stu-id="ff6bc-157">Using Data Annotation Validators with the Entity Framework</span></span>

<span data-ttu-id="ff6bc-158">Si vous utilisez Microsoft Entity Framework pour générer vos classes de modèle de données vous ne pouvez pas appliquer les attributs de validateur directement à vos classes.</span><span class="sxs-lookup"><span data-stu-id="ff6bc-158">If you are using the Microsoft Entity Framework to generate your data model classes then you cannot apply the validator attributes directly to your classes.</span></span> <span data-ttu-id="ff6bc-159">Étant donné que le Concepteur d’Entity Framework génère les classes du modèle, les modifications apportées aux classes de modèle seront remplacées la prochaine fois que vous apportez des modifications dans le concepteur.</span><span class="sxs-lookup"><span data-stu-id="ff6bc-159">Because the Entity Framework Designer generates the model classes, any changes you make to the model classes will be overwritten the next time you make any changes in the Designer.</span></span>

<span data-ttu-id="ff6bc-160">Si vous souhaitez utiliser les validateurs avec les classes générées par Entity Framework vous devez créer des classes de données de métadonnées.</span><span class="sxs-lookup"><span data-stu-id="ff6bc-160">If you want to use the validators with the classes generated by the Entity Framework then you need to create meta data classes.</span></span> <span data-ttu-id="ff6bc-161">Vous appliquez les validateurs à la classe méta de données au lieu d’appliquer les validateurs à la classe réelle.</span><span class="sxs-lookup"><span data-stu-id="ff6bc-161">You apply the validators to the meta data class instead of applying the validators to the actual class.</span></span>

<span data-ttu-id="ff6bc-162">Par exemple, imaginez que vous avez créé une classe de film à l’aide d’Entity Framework (voir **Figure 5**).</span><span class="sxs-lookup"><span data-stu-id="ff6bc-162">For example, imagine that you have created a Movie class using the Entity Framework (see **Figure 5**).</span></span> <span data-ttu-id="ff6bc-163">En outre, imaginez que vous souhaitez rendre le titre du film et le directeur de propriétés requis.</span><span class="sxs-lookup"><span data-stu-id="ff6bc-163">Imagine, furthermore, that you want to make the Movie Title and Director properties required properties.</span></span> <span data-ttu-id="ff6bc-164">Dans ce cas, vous pouvez créer la classe partielle et la classe méta de données dans **liste 4**.</span><span class="sxs-lookup"><span data-stu-id="ff6bc-164">In that case, you can create the partial class and meta data class in **Listing 4**.</span></span>

[![](validation-with-the-data-annotation-validators-vb/_static/image11.png)](validation-with-the-data-annotation-validators-vb/_static/image10.png)

<span data-ttu-id="ff6bc-165">**Figure 5**: classe de film générée par Entity Framework</span><span class="sxs-lookup"><span data-stu-id="ff6bc-165">**Figure 5**: Movie class generated by Entity Framework</span></span>

[!code-vb[Main](validation-with-the-data-annotation-validators-vb/samples/sample5.vb)]

<span data-ttu-id="ff6bc-166">**La liste 4**: Models\Movie.vb</span><span class="sxs-lookup"><span data-stu-id="ff6bc-166">**Listing 4**: Models\Movie.vb</span></span>

<span data-ttu-id="ff6bc-167">Le fichier dans **liste 4** contient deux classes nommées film et MovieMetaData.</span><span class="sxs-lookup"><span data-stu-id="ff6bc-167">The file in **Listing 4** contains two classes named Movie and MovieMetaData.</span></span> <span data-ttu-id="ff6bc-168">La classe de film est une classe partielle.</span><span class="sxs-lookup"><span data-stu-id="ff6bc-168">The Movie class is a partial class.</span></span> <span data-ttu-id="ff6bc-169">Il correspond à la classe partielle générée par Entity Framework qui est contenue dans le fichier DataModel.Designer.vb.</span><span class="sxs-lookup"><span data-stu-id="ff6bc-169">It corresponds to the partial class generated by the Entity Framework that is contained in the DataModel.Designer.vb file.</span></span>

<span data-ttu-id="ff6bc-170">Actuellement, le .NET framework ne prend pas en charge les propriétés partielles.</span><span class="sxs-lookup"><span data-stu-id="ff6bc-170">Currently, the .NET framework does not support partial properties.</span></span> <span data-ttu-id="ff6bc-171">Par conséquent, il n’existe aucun moyen d’appliquer les attributs de validateur pour les propriétés de la classe de film définie dans le fichier DataModel.Designer.vb en appliquant les attributs de validateur pour les propriétés de la classe de film définie dans le fichier **liste 4**.</span><span class="sxs-lookup"><span data-stu-id="ff6bc-171">Therefore, there is no way to apply the validator attributes to the properties of the Movie class defined in the DataModel.Designer.vb file by applying the validator attributes to the properties of the Movie class defined in the file in **Listing 4**.</span></span>

<span data-ttu-id="ff6bc-172">Notez que la classe partielle du film est décorée avec un attribut MetadataType qui pointe vers la classe MovieMetaData.</span><span class="sxs-lookup"><span data-stu-id="ff6bc-172">Notice that the Movie partial class is decorated with a MetadataType attribute that points at the MovieMetaData class.</span></span> <span data-ttu-id="ff6bc-173">La classe MovieMetaData contient les propriétés du proxy pour les propriétés de la classe de film.</span><span class="sxs-lookup"><span data-stu-id="ff6bc-173">The MovieMetaData class contains proxy properties for the properties of the Movie class.</span></span>

<span data-ttu-id="ff6bc-174">Les attributs du programme de validation sont appliquées aux propriétés de la classe MovieMetaData.</span><span class="sxs-lookup"><span data-stu-id="ff6bc-174">The validator attributes are applied to the properties of the MovieMetaData class.</span></span> <span data-ttu-id="ff6bc-175">Les propriétés du titre, le directeur et DateReleased sont marquées en tant que propriétés requises.</span><span class="sxs-lookup"><span data-stu-id="ff6bc-175">The Title, Director, and DateReleased properties are all marked as required properties.</span></span> <span data-ttu-id="ff6bc-176">La propriété de directeur doit être affectée à une chaîne contenant moins de 5 caractères.</span><span class="sxs-lookup"><span data-stu-id="ff6bc-176">The Director property must be assigned a string that contains less than 5 characters.</span></span> <span data-ttu-id="ff6bc-177">Enfin, l’attribut DisplayName est appliquée à la propriété DateReleased pour afficher un message d’erreur tel que « le champ de Date finale est requis. »</span><span class="sxs-lookup"><span data-stu-id="ff6bc-177">Finally, the DisplayName attribute is applied to the DateReleased property to display an error message like "The Date Released field is required."</span></span> <span data-ttu-id="ff6bc-178">au lieu de l’erreur « le champ DateReleased est requis. »</span><span class="sxs-lookup"><span data-stu-id="ff6bc-178">instead of the error "The DateReleased field is required."</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="ff6bc-179">Notez que les propriétés du proxy dans la classe MovieMetaData n’avez pas besoin représenter les mêmes types que les propriétés correspondantes dans la classe de film.</span><span class="sxs-lookup"><span data-stu-id="ff6bc-179">Notice that the proxy properties in the MovieMetaData class do not need to represent the same types as the corresponding properties in the Movie class.</span></span> <span data-ttu-id="ff6bc-180">Par exemple, la propriété directeur est une propriété de chaîne dans la classe de films et une propriété d’objet dans la classe MovieMetaData.</span><span class="sxs-lookup"><span data-stu-id="ff6bc-180">For example, the Director property is a string property in the Movie class and an object property in the MovieMetaData class.</span></span>


<span data-ttu-id="ff6bc-181">Dans la page de **Figure 6** illustre les messages d’erreur retournés lorsque vous entrez des valeurs non valides pour les propriétés de la vidéo.</span><span class="sxs-lookup"><span data-stu-id="ff6bc-181">The page in **Figure 6** illustrates the error messages returned when you enter invalid values for the Movie properties.</span></span>

[![](validation-with-the-data-annotation-validators-vb/_static/image13.png)](validation-with-the-data-annotation-validators-vb/_static/image12.png)

<span data-ttu-id="ff6bc-182">**Figure 6**: à l’aide de validateurs avec Entity Framework ([cliquez pour afficher l’image en taille réelle](validation-with-the-data-annotation-validators-vb/_static/image14.png))</span><span class="sxs-lookup"><span data-stu-id="ff6bc-182">**Figure 6**: Using validators with the Entity Framework ([Click to view full-size image](validation-with-the-data-annotation-validators-vb/_static/image14.png))</span></span>

## <a name="summary"></a><span data-ttu-id="ff6bc-183">Récapitulatif</span><span class="sxs-lookup"><span data-stu-id="ff6bc-183">Summary</span></span>

<span data-ttu-id="ff6bc-184">Dans ce didacticiel, vous avez appris comment tirer parti de classeur de modèles de données d’Annotation pour effectuer la validation au sein d’une application ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="ff6bc-184">In this tutorial, you learned how to take advantage of the Data Annotation Model Binder to perform validation within an ASP.NET MVC application.</span></span> <span data-ttu-id="ff6bc-185">Vous avez appris à utiliser les différents types d’attributs de validateur comme requis et les attributs de StringLength.</span><span class="sxs-lookup"><span data-stu-id="ff6bc-185">You learned how to use the different types of validator attributes such as the Required and StringLength attributes.</span></span> <span data-ttu-id="ff6bc-186">Vous avez également appris à utiliser ces attributs lorsque vous travaillez avec Microsoft Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="ff6bc-186">You also learned how to use these attributes when working with the Microsoft Entity Framework.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="ff6bc-187">Précédent</span><span class="sxs-lookup"><span data-stu-id="ff6bc-187">Previous</span></span>](validating-with-a-service-layer-vb.md)
