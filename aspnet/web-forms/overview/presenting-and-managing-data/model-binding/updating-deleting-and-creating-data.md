---
uid: web-forms/overview/presenting-and-managing-data/model-binding/updating-deleting-and-creating-data
title: Mise à jour, suppression et la création des données avec la liaison de modèle et les web forms | Documents Microsoft
author: tfitzmac
description: Cette série de didacticiels montre les aspects de base de l’utilisation de la liaison de modèle avec un projet Web Forms ASP.NET. Liaison de modèle permet une interaction de données plus droites-...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/27/2014
ms.topic: article
ms.assetid: 602baa94-5a4f-46eb-a717-7a9e539c1db4
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/updating-deleting-and-creating-data
msc.type: authoredcontent
ms.openlocfilehash: e6536f7858afde5faf3aedd34f3cbe95c5ed0d53
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30885843"
---
<a name="updating-deleting-and-creating-data-with-model-binding-and-web-forms"></a><span data-ttu-id="1bb47-104">Mise à jour, suppression et la création des données avec la liaison de modèle et les web forms</span><span class="sxs-lookup"><span data-stu-id="1bb47-104">Updating, deleting, and creating data with model binding and web forms</span></span>
====================
<span data-ttu-id="1bb47-105">par [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="1bb47-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="1bb47-106">Cette série de didacticiels montre les aspects de base de l’utilisation de la liaison de modèle avec un projet Web Forms ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="1bb47-106">This tutorial series demonstrates basic aspects of using model binding with an ASP.NET Web Forms project.</span></span> <span data-ttu-id="1bb47-107">Liaison de modèle permet une interaction de données plus simple que vous traitez des données des objets de source (comme ObjectDataSource ou SqlDataSource).</span><span class="sxs-lookup"><span data-stu-id="1bb47-107">Model binding makes data interaction more straight-forward than dealing with data source objects (such as ObjectDataSource or SqlDataSource).</span></span> <span data-ttu-id="1bb47-108">Cette série commence par la partie introductive et déplace vers des concepts plus avancés dans des didacticiels ultérieurs.</span><span class="sxs-lookup"><span data-stu-id="1bb47-108">This series starts with introductory material and moves to more advanced concepts in later tutorials.</span></span>
> 
> <span data-ttu-id="1bb47-109">Ce didacticiel montre comment créer, mettre à jour et supprimer des données avec la liaison de modèle.</span><span class="sxs-lookup"><span data-stu-id="1bb47-109">This tutorial shows how to create, update, and delete data with model binding.</span></span> <span data-ttu-id="1bb47-110">Vous allez définir les propriétés suivantes :</span><span class="sxs-lookup"><span data-stu-id="1bb47-110">You will set the following properties:</span></span>
> 
> - <span data-ttu-id="1bb47-111">DeleteMethod</span><span class="sxs-lookup"><span data-stu-id="1bb47-111">DeleteMethod</span></span>
> - <span data-ttu-id="1bb47-112">InsertMethod</span><span class="sxs-lookup"><span data-stu-id="1bb47-112">InsertMethod</span></span>
> - <span data-ttu-id="1bb47-113">UpdateMethod</span><span class="sxs-lookup"><span data-stu-id="1bb47-113">UpdateMethod</span></span>
> 
> <span data-ttu-id="1bb47-114">Ces propriétés reçoivent le nom de la méthode qui gère l’opération correspondante.</span><span class="sxs-lookup"><span data-stu-id="1bb47-114">These properties receive the name of the method that handles the corresponding operation.</span></span> <span data-ttu-id="1bb47-115">Dans cette méthode, vous fournissez la logique permettant d’interagir avec les données.</span><span class="sxs-lookup"><span data-stu-id="1bb47-115">Within that method, you provide the logic for interacting with the data.</span></span>
> 
> <span data-ttu-id="1bb47-116">Ce didacticiel s’appuie sur le projet créé dans la première [partie](retrieving-data.md) de la série.</span><span class="sxs-lookup"><span data-stu-id="1bb47-116">This tutorial builds on the project created in the first [part](retrieving-data.md) of the series.</span></span>
> 
> <span data-ttu-id="1bb47-117">Vous pouvez [télécharger](https://go.microsoft.com/fwlink/?LinkId=286116) le projet complet en c# ou VB.</span><span class="sxs-lookup"><span data-stu-id="1bb47-117">You can [download](https://go.microsoft.com/fwlink/?LinkId=286116) the complete project in C# or VB.</span></span> <span data-ttu-id="1bb47-118">Le code téléchargeable fonctionne avec Visual Studio 2012 ou Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="1bb47-118">The downloadable code works with either Visual Studio 2012 or Visual Studio 2013.</span></span> <span data-ttu-id="1bb47-119">Elle utilise le modèle Visual Studio 2012, qui est légèrement différent de celle du modèle de Visual Studio 2013 indiqué dans ce didacticiel.</span><span class="sxs-lookup"><span data-stu-id="1bb47-119">It uses the Visual Studio 2012 template, which is slightly different than the Visual Studio 2013 template shown in this tutorial.</span></span>


## <a name="what-youll-build"></a><span data-ttu-id="1bb47-120">Vous allez générer</span><span class="sxs-lookup"><span data-stu-id="1bb47-120">What you'll build</span></span>

<span data-ttu-id="1bb47-121">Dans ce didacticiel, vous effectuerez les tâches suivantes :</span><span class="sxs-lookup"><span data-stu-id="1bb47-121">In this tutorial, you'll:</span></span>

1. <span data-ttu-id="1bb47-122">Ajouter des modèles de données dynamiques</span><span class="sxs-lookup"><span data-stu-id="1bb47-122">Add dynamic data templates</span></span>
2. <span data-ttu-id="1bb47-123">Activer la mise à jour et suppression des données par le biais des méthodes de liaison de modèle</span><span class="sxs-lookup"><span data-stu-id="1bb47-123">Enable updating and deleting data through model binding methods</span></span>
3. <span data-ttu-id="1bb47-124">Appliquer des règles de validation de données - activer la création d’un nouvel enregistrement dans la base de données</span><span class="sxs-lookup"><span data-stu-id="1bb47-124">Apply data validation rules - Enable creating a new record in the database</span></span>

## <a name="add-dynamic-data-templates"></a><span data-ttu-id="1bb47-125">Ajouter des modèles de données dynamiques</span><span class="sxs-lookup"><span data-stu-id="1bb47-125">Add dynamic data templates</span></span>

<span data-ttu-id="1bb47-126">Pour offrir la meilleure expérience utilisateur et de réduire la redondance du code, vous allez utiliser les modèles de données dynamiques.</span><span class="sxs-lookup"><span data-stu-id="1bb47-126">To provide the best user experience and minimize code repetition, you will use dynamic data templates.</span></span> <span data-ttu-id="1bb47-127">Vous pouvez facilement intégrer des modèles prédéfinis dynamic data à votre site existant en installant un package NuGet.</span><span class="sxs-lookup"><span data-stu-id="1bb47-127">You can easily integrate pre-built dynamic data templates into your existing site by installing a NuGet package.</span></span>

<span data-ttu-id="1bb47-128">À partir de la **gérer les Packages NuGet**, installez le **DynamicDataTemplatesCS**.</span><span class="sxs-lookup"><span data-stu-id="1bb47-128">From the **Manage NuGet Packages**, install the **DynamicDataTemplatesCS**.</span></span>

![modèles de données dynamiques](updating-deleting-and-creating-data/_static/image1.png)

<span data-ttu-id="1bb47-130">Notez que votre projet inclut maintenant un dossier nommé **DynamicData**.</span><span class="sxs-lookup"><span data-stu-id="1bb47-130">Notice that your project now includes a folder named **DynamicData**.</span></span> <span data-ttu-id="1bb47-131">Dans ce dossier, vous trouverez des modèles qui sont automatiquement appliqués aux contrôles dynamiques persistants dans vos formulaires web.</span><span class="sxs-lookup"><span data-stu-id="1bb47-131">In that folder, you will find the templates that are automatically applied to dynamic controls in your web forms.</span></span>

![dossier de données dynamiques](updating-deleting-and-creating-data/_static/image2.png)

## <a name="enable-updating-and-deleting"></a><span data-ttu-id="1bb47-133">Activer mise à jour et suppression</span><span class="sxs-lookup"><span data-stu-id="1bb47-133">Enable updating and deleting</span></span>

<span data-ttu-id="1bb47-134">Permettre aux utilisateurs de mettre à jour et supprimer des enregistrements dans la base de données est très similaire au processus de récupération des données.</span><span class="sxs-lookup"><span data-stu-id="1bb47-134">Enabling users to update and delete records in the database is very similar to the process for retrieving data.</span></span> <span data-ttu-id="1bb47-135">Dans le **UpdateMethod** et **DeleteMethod** propriétés, vous spécifiez les noms des méthodes qui effectuent ces opérations.</span><span class="sxs-lookup"><span data-stu-id="1bb47-135">In the **UpdateMethod** and **DeleteMethod** properties, you specify the names of the methods that perform those operations.</span></span> <span data-ttu-id="1bb47-136">Avec un contrôle GridView, vous pouvez également spécifier la génération automatique de modifier et supprimer des boutons.</span><span class="sxs-lookup"><span data-stu-id="1bb47-136">With a GridView control, you can also specify the automatic generation of edit and delete buttons.</span></span> <span data-ttu-id="1bb47-137">Le code en surbrillance suivant montre les ajouts au code GridView.</span><span class="sxs-lookup"><span data-stu-id="1bb47-137">The following highlighted code shows the additions to the GridView code.</span></span>

[!code-aspx[Main](updating-deleting-and-creating-data/samples/sample1.aspx?highlight=4-5)]

<span data-ttu-id="1bb47-138">Dans le fichier code-behind, ajoutez une à l’aide de l’instruction pour **System.Data.Entity.Infrastructure**.</span><span class="sxs-lookup"><span data-stu-id="1bb47-138">In the code-behind file, add a using statement for **System.Data.Entity.Infrastructure**.</span></span>

[!code-csharp[Main](updating-deleting-and-creating-data/samples/sample2.cs)]

<span data-ttu-id="1bb47-139">Ensuite, ajouter la mise à jour suivant et supprimer les méthodes.</span><span class="sxs-lookup"><span data-stu-id="1bb47-139">Then, add the following update and delete methods.</span></span>

[!code-csharp[Main](updating-deleting-and-creating-data/samples/sample3.cs)]

<span data-ttu-id="1bb47-140">Le **TryUpdateModel** méthode s’applique les valeurs liées aux données correspondantes à partir du formulaire web à l’élément de données.</span><span class="sxs-lookup"><span data-stu-id="1bb47-140">The **TryUpdateModel** method applies the matching data-bound values from the web form to the data item.</span></span> <span data-ttu-id="1bb47-141">L’élément de données est récupéré en fonction de la valeur du paramètre id.</span><span class="sxs-lookup"><span data-stu-id="1bb47-141">The data item is retrieved based on the value of the id parameter.</span></span>

## <a name="enforce-validation-requirements"></a><span data-ttu-id="1bb47-142">Appliquer des critères de validation</span><span class="sxs-lookup"><span data-stu-id="1bb47-142">Enforce validation requirements</span></span>

<span data-ttu-id="1bb47-143">Les attributs de validation que vous avez appliqués aux propriétés dans la classe Student FirstName, LastName et année sont appliquées automatiquement lors de la mise à jour les données.</span><span class="sxs-lookup"><span data-stu-id="1bb47-143">The validation attributes that you applied to the FirstName, LastName, and Year properties in the Student class are automatically enforced when updating the data.</span></span> <span data-ttu-id="1bb47-144">Les contrôles DynamicField ajoutent des programmes de validation client et serveur basés sur les attributs de validation.</span><span class="sxs-lookup"><span data-stu-id="1bb47-144">The DynamicField controls add client and server validators based on the validation attributes.</span></span> <span data-ttu-id="1bb47-145">Les propriétés FirstName et LastName sont tous deux requis.</span><span class="sxs-lookup"><span data-stu-id="1bb47-145">The FirstName and LastName properties are both required.</span></span> <span data-ttu-id="1bb47-146">Prénom ne peut pas dépasser 20 caractères et LastName ne peut pas dépasser 40 caractères.</span><span class="sxs-lookup"><span data-stu-id="1bb47-146">FirstName cannot exceed 20 characters in length, and LastName cannot exceed 40 characters.</span></span> <span data-ttu-id="1bb47-147">Année doit être une valeur valide pour l’énumération AcademicYear.</span><span class="sxs-lookup"><span data-stu-id="1bb47-147">Year must be a valid value for the AcademicYear enumeration.</span></span>

<span data-ttu-id="1bb47-148">Si l’utilisateur ne respecte pas une des conditions de validation, la mise à jour n’est pas effectuée.</span><span class="sxs-lookup"><span data-stu-id="1bb47-148">If the user violates one of the validation requirements, the update does not proceed.</span></span> <span data-ttu-id="1bb47-149">Pour voir le message d’erreur, ajoutez un contrôle ValidationSummary ci-dessus le GridView.</span><span class="sxs-lookup"><span data-stu-id="1bb47-149">To see the error message, add a ValidationSummary control above the GridView.</span></span> <span data-ttu-id="1bb47-150">Pour afficher les erreurs de validation à partir de la liaison de modèle, définissez la **ShowModelStateErrors** propriété **true**.</span><span class="sxs-lookup"><span data-stu-id="1bb47-150">To display the validation errors from model binding, set the **ShowModelStateErrors** property set to **true**.</span></span> 

[!code-aspx[Main](updating-deleting-and-creating-data/samples/sample4.aspx)]

<span data-ttu-id="1bb47-151">Exécuter l’application web et mettre à jour et supprimer les enregistrements.</span><span class="sxs-lookup"><span data-stu-id="1bb47-151">Run the web application, and update and delete any of the records.</span></span>

![Mettre à jour des données](updating-deleting-and-creating-data/_static/image3.png)

<span data-ttu-id="1bb47-153">Notez que lorsque vous en mode d’édition, la valeur de la propriété de l’année est restituée automatiquement comme une zone de liste déroulante.</span><span class="sxs-lookup"><span data-stu-id="1bb47-153">Notice that when in the edit mode the value for the Year property is automatically rendered as a drop down list.</span></span> <span data-ttu-id="1bb47-154">La propriété de l’année est une valeur d’énumération, et le modèle de données dynamiques pour une valeur d’énumération spécifie une liste déroulante de modification.</span><span class="sxs-lookup"><span data-stu-id="1bb47-154">The Year property is an enumeration value, and the dynamic data template for an enumeration value specifies a drop down list for editing.</span></span> <span data-ttu-id="1bb47-155">Vous pouvez trouver ce modèle en ouvrant le **énumération\_Data Edit.ascx** de fichiers dans le **DynamicData**/**FieldTemplates** dossier.</span><span class="sxs-lookup"><span data-stu-id="1bb47-155">You can find that template by opening the **Enumeration\_Edit.ascx** file in the **DynamicData**/**FieldTemplates** folder.</span></span>

<span data-ttu-id="1bb47-156">Si vous fournissez des valeurs valides, la mise à jour se termine correctement.</span><span class="sxs-lookup"><span data-stu-id="1bb47-156">If you provide valid values, the update completes successfully.</span></span> <span data-ttu-id="1bb47-157">Si vous ne respectez pas une des conditions de validation, la mise à jour s’arrête et un message d’erreur s’affiche au-dessus de la grille.</span><span class="sxs-lookup"><span data-stu-id="1bb47-157">If you violate one of the validation requirements, the update does not proceed and an error message is displayed above the grid.</span></span>

![Message d’erreur](updating-deleting-and-creating-data/_static/image4.png)

## <a name="add-new-records"></a><span data-ttu-id="1bb47-159">Ajout de nouveaux enregistrements</span><span class="sxs-lookup"><span data-stu-id="1bb47-159">Add new records</span></span>

<span data-ttu-id="1bb47-160">Le contrôle GridView n’inclut pas le **InsertMethod** propriété et ne peut donc pas être utilisé pour ajouter un nouvel enregistrement avec la liaison de modèle.</span><span class="sxs-lookup"><span data-stu-id="1bb47-160">The GridView control does not include the **InsertMethod** property and therefore cannot be used for adding a new record with model binding.</span></span> <span data-ttu-id="1bb47-161">Vous pouvez trouver la propriété InsertMethod dans le **FormView**, **DetailsView**, ou **ListView** contrôles.</span><span class="sxs-lookup"><span data-stu-id="1bb47-161">You can find the InsertMethod property in the **FormView**, **DetailsView**, or **ListView** controls.</span></span> <span data-ttu-id="1bb47-162">Dans ce didacticiel, vous utiliserez un contrôle FormView pour ajouter un nouvel enregistrement.</span><span class="sxs-lookup"><span data-stu-id="1bb47-162">In this tutorial, you will use a FormView control to add a new record.</span></span>

<span data-ttu-id="1bb47-163">Tout d’abord, ajoutez un lien vers la nouvelle page, que vous allez créer pour ajouter un nouvel enregistrement.</span><span class="sxs-lookup"><span data-stu-id="1bb47-163">First, add a link to the new page you will create for adding a new record.</span></span> <span data-ttu-id="1bb47-164">Au-dessus du contrôle ValidationSummary, ajoutez :</span><span class="sxs-lookup"><span data-stu-id="1bb47-164">Above the ValidationSummary, add:</span></span>

[!code-aspx[Main](updating-deleting-and-creating-data/samples/sample5.aspx)]

<span data-ttu-id="1bb47-165">Le nouveau lien s’affiche en haut du contenu de la page d’étudiants.</span><span class="sxs-lookup"><span data-stu-id="1bb47-165">The new link will appear at the top of the content for the Students page.</span></span>

![nouveau lien](updating-deleting-and-creating-data/_static/image5.png)

<span data-ttu-id="1bb47-167">Ensuite, ajoutez un nouveau formulaire web à l’aide d’une page maître et nommez-le **AddStudent**.</span><span class="sxs-lookup"><span data-stu-id="1bb47-167">Then, add a new web form using a master page, and name it **AddStudent**.</span></span> <span data-ttu-id="1bb47-168">Sélectionnez la page maître Site.Master.</span><span class="sxs-lookup"><span data-stu-id="1bb47-168">Select Site.Master as the master page.</span></span>

<span data-ttu-id="1bb47-169">Vous rendrez les champs pour l’ajout d’un nouvel étudiant en utilisant un **DynamicEntity** contrôle.</span><span class="sxs-lookup"><span data-stu-id="1bb47-169">You will render the fields for adding a new student by using a **DynamicEntity** control.</span></span> <span data-ttu-id="1bb47-170">Le contrôle DynamicEntity restitue que propriétés modifiables de la classe spécifiée dans la propriété ItemType.</span><span class="sxs-lookup"><span data-stu-id="1bb47-170">The DynamicEntity control renders that editable properties in the class specified in the ItemType property.</span></span> <span data-ttu-id="1bb47-171">La propriété StudentID a été marquée avec la **[ScaffoldColumn(false)]** d’attribut afin qu’il n’est pas rendu.</span><span class="sxs-lookup"><span data-stu-id="1bb47-171">The StudentID property was marked with the **[ScaffoldColumn(false)]** attribute so it is not rendered.</span></span> <span data-ttu-id="1bb47-172">Dans l’espace réservé MainContent de la page AddStudent, ajoutez le code suivant.</span><span class="sxs-lookup"><span data-stu-id="1bb47-172">In the MainContent placeholder of the AddStudent page, add the following code.</span></span>

[!code-aspx[Main](updating-deleting-and-creating-data/samples/sample6.aspx)]

<span data-ttu-id="1bb47-173">Dans le fichier code-behind (AddStudent.aspx.cs), ajoutez un **à l’aide de** instruction pour le **ContosoUniversityModelBinding.Models** espace de noms.</span><span class="sxs-lookup"><span data-stu-id="1bb47-173">In the code-behind file (AddStudent.aspx.cs), add a **using** statement for the **ContosoUniversityModelBinding.Models** namespace.</span></span>

[!code-csharp[Main](updating-deleting-and-creating-data/samples/sample7.cs)]

<span data-ttu-id="1bb47-174">Ensuite, ajoutez les méthodes suivantes pour spécifier comment insérer un nouvel enregistrement et un gestionnaire d’événements pour le bouton Annuler.</span><span class="sxs-lookup"><span data-stu-id="1bb47-174">Then, add the following methods to specify how to insert a new record and an event handler for the cancel button.</span></span>

[!code-csharp[Main](updating-deleting-and-creating-data/samples/sample8.cs)]

<span data-ttu-id="1bb47-175">Enregistrez toutes les modifications.</span><span class="sxs-lookup"><span data-stu-id="1bb47-175">Save all of the changes.</span></span>

<span data-ttu-id="1bb47-176">Exécuter l’application web et créer un nouvel étudiant.</span><span class="sxs-lookup"><span data-stu-id="1bb47-176">Run the web application and create a new student.</span></span>

![Ajouter Nouvel étudiant](updating-deleting-and-creating-data/_static/image6.png)

<span data-ttu-id="1bb47-178">Cliquez sur **insérer** et notez le nouvel étudiant a été créé.</span><span class="sxs-lookup"><span data-stu-id="1bb47-178">Click **Insert** and notice the new student has been created.</span></span>

![afficher le nouvel étudiant](updating-deleting-and-creating-data/_static/image7.png)

## <a name="conclusion"></a><span data-ttu-id="1bb47-180">Conclusion</span><span class="sxs-lookup"><span data-stu-id="1bb47-180">Conclusion</span></span>

<span data-ttu-id="1bb47-181">Dans ce didacticiel, vous activé la mise à jour, la suppression et la création de données.</span><span class="sxs-lookup"><span data-stu-id="1bb47-181">In this tutorial, you enabled updating, deleting, and creating data.</span></span> <span data-ttu-id="1bb47-182">Vous assurer que les règles de validation sont appliquées lors de l’interaction avec les données.</span><span class="sxs-lookup"><span data-stu-id="1bb47-182">You ensured validation rules are applied when interacting with the data.</span></span>

<span data-ttu-id="1bb47-183">Dans la prochaine [didacticiel](sorting-paging-and-filtering-data.md) dans cette série, vous allez activer le tri, la pagination et le filtrage des données.</span><span class="sxs-lookup"><span data-stu-id="1bb47-183">In the next [tutorial](sorting-paging-and-filtering-data.md) in this series, you will enable sorting, paging, and filtering data.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="1bb47-184">[Précédent](retrieving-data.md)
> [Suivant](sorting-paging-and-filtering-data.md)</span><span class="sxs-lookup"><span data-stu-id="1bb47-184">[Previous](retrieving-data.md)
[Next](sorting-paging-and-filtering-data.md)</span></span>
