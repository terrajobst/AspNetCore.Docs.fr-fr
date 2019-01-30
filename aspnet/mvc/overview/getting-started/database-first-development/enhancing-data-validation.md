---
uid: mvc/overview/getting-started/database-first-development/enhancing-data-validation
title: 'Tutoriel : Améliorer la validation des données pour EF Database First avec une application ASP.NET MVC'
description: Ce didacticiel se concentre sur l’ajout d’annotations de données au modèle de données pour spécifier les exigences de validation et afficher la mise en forme.
author: Rick-Anderson
ms.author: riande
ms.date: 01/28/2019
ms.topic: tutorial
ms.assetid: 0ed5e67a-34c0-4b57-84a6-802b0fb3cd00
msc.legacyurl: /mvc/overview/getting-started/database-first-development/enhancing-data-validation
msc.type: authoredcontent
ms.openlocfilehash: 85299d70c6cba52c1d40a42edfd429c96318134a
ms.sourcegitcommit: c47d7c131eebbcd8811e31edda210d64cf4b9d6b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2019
ms.locfileid: "55236482"
---
# <a name="tutorial-enhance-data-validation-for-ef-database-first-with-aspnet-mvc-app"></a><span data-ttu-id="9fc76-103">Tutoriel : Améliorer la validation des données pour EF Database First avec une application ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="9fc76-103">Tutorial: Enhance data validation for EF Database First with ASP.NET MVC app</span></span>

<span data-ttu-id="9fc76-104">À l’aide de la structure ASP.NET MVC et Entity Framework, vous pouvez créer une application web qui fournit une interface à une base de données existante.</span><span class="sxs-lookup"><span data-stu-id="9fc76-104">Using MVC, Entity Framework, and ASP.NET Scaffolding, you can create a web application that provides an interface to an existing database.</span></span> <span data-ttu-id="9fc76-105">Cette série de didacticiels vous montre comment générer du code qui permet aux utilisateurs d’afficher, modifier, créer et supprimer automatiquement les données qui résident dans une table de base de données.</span><span class="sxs-lookup"><span data-stu-id="9fc76-105">This tutorial series shows you how to automatically generate code that enables users to display, edit, create, and delete data that resides in a database table.</span></span> <span data-ttu-id="9fc76-106">Le code généré correspond aux colonnes dans la table de base de données.</span><span class="sxs-lookup"><span data-stu-id="9fc76-106">The generated code corresponds to the columns in the database table.</span></span>

<span data-ttu-id="9fc76-107">Ce didacticiel se concentre sur l’ajout d’annotations de données au modèle de données pour spécifier les exigences de validation et afficher la mise en forme.</span><span class="sxs-lookup"><span data-stu-id="9fc76-107">This tutorial focuses on adding data annotations to the data model to specify validation requirements and display formatting.</span></span> <span data-ttu-id="9fc76-108">Il a été amélioré en se basant sur les commentaires des utilisateurs dans la section commentaires.</span><span class="sxs-lookup"><span data-stu-id="9fc76-108">It was improved based on feedback from users in the comments section.</span></span>

<span data-ttu-id="9fc76-109">Dans ce didacticiel, vous avez effectué les actions suivantes :</span><span class="sxs-lookup"><span data-stu-id="9fc76-109">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="9fc76-110">Ajouter des annotations de données</span><span class="sxs-lookup"><span data-stu-id="9fc76-110">Add data annotations</span></span>
> * <span data-ttu-id="9fc76-111">Ajouter des classes de métadonnées</span><span class="sxs-lookup"><span data-stu-id="9fc76-111">Add metadata classes</span></span>

## <a name="prerequisites"></a><span data-ttu-id="9fc76-112">Prérequis</span><span class="sxs-lookup"><span data-stu-id="9fc76-112">Prerequisites</span></span>

* [<span data-ttu-id="9fc76-113">Personnaliser un affichage</span><span class="sxs-lookup"><span data-stu-id="9fc76-113">Customize a view</span></span>](customizing-a-view.md)

## <a name="add-data-annotations"></a><span data-ttu-id="9fc76-114">Ajouter des annotations de données</span><span class="sxs-lookup"><span data-stu-id="9fc76-114">Add data annotations</span></span>

<span data-ttu-id="9fc76-115">Comme vous l’avez vu dans une rubrique précédente, certaines règles de validation de données sont automatiquement appliquées à l’entrée utilisateur.</span><span class="sxs-lookup"><span data-stu-id="9fc76-115">As you saw in an earlier topic, some data validation rules are automatically applied to the user input.</span></span> <span data-ttu-id="9fc76-116">Par exemple, vous pouvez fournir uniquement un nombre pour la propriété de classe.</span><span class="sxs-lookup"><span data-stu-id="9fc76-116">For example, you can only provide a number for the Grade property.</span></span> <span data-ttu-id="9fc76-117">Pour spécifier plusieurs règles de validation de données, vous pouvez ajouter des annotations de données à votre classe de modèle.</span><span class="sxs-lookup"><span data-stu-id="9fc76-117">To specify more data validation rules, you can add data annotations to your model class.</span></span> <span data-ttu-id="9fc76-118">Ces annotations sont appliquées dans toute votre application web pour la propriété spécifiée.</span><span class="sxs-lookup"><span data-stu-id="9fc76-118">These annotations are applied throughout your web application for the specified property.</span></span> <span data-ttu-id="9fc76-119">Vous pouvez également appliquer des attributs de mise en forme qui modifient la façon dont les propriétés sont affichées ; comme la modification de la valeur utilisée pour les étiquettes de texte.</span><span class="sxs-lookup"><span data-stu-id="9fc76-119">You can also apply formatting attributes that change how the properties are displayed; such as, changing the value used for text labels.</span></span>

<span data-ttu-id="9fc76-120">Dans ce didacticiel, vous allez ajouter des annotations de données pour limiter la longueur des valeurs fournies pour les propriétés FirstName, LastName et MiddleName.</span><span class="sxs-lookup"><span data-stu-id="9fc76-120">In this tutorial, you will add data annotations to restrict the length of the values provided for the FirstName, LastName, and MiddleName properties.</span></span> <span data-ttu-id="9fc76-121">Dans la base de données, ces valeurs sont limitées à 50 caractères ; Toutefois, dans votre application web cette limite de caractère n’est actuellement pas appliquée.</span><span class="sxs-lookup"><span data-stu-id="9fc76-121">In the database, these values are limited to 50 characters; however, in your web application that character limit is currently not enforced.</span></span> <span data-ttu-id="9fc76-122">Si un utilisateur fournit plus de 50 caractères pour l’une de ces valeurs, la page se bloque lorsque vous tentez d’enregistrer la valeur dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="9fc76-122">If a user provides more than 50 characters for one of those values, the page will crash when attempting to save the value to the database.</span></span> <span data-ttu-id="9fc76-123">Vous allez également restreindre le niveau pour les valeurs comprises entre 0 et 4.</span><span class="sxs-lookup"><span data-stu-id="9fc76-123">You will also restrict Grade to values between 0 and 4.</span></span>

<span data-ttu-id="9fc76-124">Sélectionnez **modèles** > **ContosoModel.edmx** > **ContosoModel.tt** et ouvrez le *Student.cs* fichier.</span><span class="sxs-lookup"><span data-stu-id="9fc76-124">Select **Models** > **ContosoModel.edmx** > **ContosoModel.tt** and open the *Student.cs* file.</span></span> <span data-ttu-id="9fc76-125">Ajoutez le code en surbrillance suivant à la classe.</span><span class="sxs-lookup"><span data-stu-id="9fc76-125">Add the following highlighted code to the class.</span></span>

[!code-csharp[Main](enhancing-data-validation/samples/sample1.cs?highlight=5,15,17,20)]

<span data-ttu-id="9fc76-126">Ouvrez *Enrollment.cs* et ajoutez le code en surbrillance suivant.</span><span class="sxs-lookup"><span data-stu-id="9fc76-126">Open *Enrollment.cs* and add the following highlighted code.</span></span>

[!code-csharp[Main](enhancing-data-validation/samples/sample2.cs?highlight=5,10)]

<span data-ttu-id="9fc76-127">Générez la solution.</span><span class="sxs-lookup"><span data-stu-id="9fc76-127">Build the solution.</span></span>

<span data-ttu-id="9fc76-128">Cliquez sur **liste d’étudiants** et sélectionnez **modifier**.</span><span class="sxs-lookup"><span data-stu-id="9fc76-128">Click **List of students** and select **Edit**.</span></span> <span data-ttu-id="9fc76-129">Si vous essayez d’entrer plus de 50 caractères, un message d’erreur s’affiche.</span><span class="sxs-lookup"><span data-stu-id="9fc76-129">If you attempt to enter more than 50 characters, an error message is displayed.</span></span>

![afficher le message d’erreur](enhancing-data-validation/_static/image1.png)

<span data-ttu-id="9fc76-131">Revenez à la page d’accueil.</span><span class="sxs-lookup"><span data-stu-id="9fc76-131">Go back to the home page.</span></span> <span data-ttu-id="9fc76-132">Cliquez sur **liste des inscriptions** et sélectionnez **modifier**.</span><span class="sxs-lookup"><span data-stu-id="9fc76-132">Click **List of enrollments** and select **Edit**.</span></span> <span data-ttu-id="9fc76-133">Essaie de fournir un niveau au-dessus de 4.</span><span class="sxs-lookup"><span data-stu-id="9fc76-133">Attempt to provide a grade above 4.</span></span> <span data-ttu-id="9fc76-134">Vous recevez cette erreur : *Le champ niveau doit être comprise entre 0 et 4.*</span><span class="sxs-lookup"><span data-stu-id="9fc76-134">You will receive this error: *The field Grade must be between 0 and 4.*</span></span>

## <a name="add-metadata-classes"></a><span data-ttu-id="9fc76-135">Ajouter des classes de métadonnées</span><span class="sxs-lookup"><span data-stu-id="9fc76-135">Add metadata classes</span></span>

<span data-ttu-id="9fc76-136">Ajout d’attributs de validation directement à la classe de modèle fonctionne lorsque vous ne prévoyez pas de la base de données pour modifier ; Toutefois, si vos modifications de base de données et vous devez régénérer la classe de modèle, vous allez perdre tous les attributs que vous aviez appliqué à la classe de modèle.</span><span class="sxs-lookup"><span data-stu-id="9fc76-136">Adding the validation attributes directly to the model class works when you do not expect the database to change; however, if your database changes and you need to regenerate the model class, you will lose all of the attributes you had applied to the model class.</span></span> <span data-ttu-id="9fc76-137">Cette approche peut être très inefficace et sujette à la perte de règles de validation important.</span><span class="sxs-lookup"><span data-stu-id="9fc76-137">This approach can be very inefficient and prone to losing important validation rules.</span></span>

<span data-ttu-id="9fc76-138">Pour éviter ce problème, vous pouvez ajouter une classe de métadonnées qui contient les attributs.</span><span class="sxs-lookup"><span data-stu-id="9fc76-138">To avoid this problem, you can add a metadata class that contains the attributes.</span></span> <span data-ttu-id="9fc76-139">Lorsque vous associez la classe de modèle à la classe de métadonnées, ces attributs sont appliqués au modèle.</span><span class="sxs-lookup"><span data-stu-id="9fc76-139">When you associate the model class to the metadata class, those attributes are applied to the model.</span></span> <span data-ttu-id="9fc76-140">Dans cette approche, la classe de modèle peut être régénérée sans perdre tous les attributs qui ont été appliqués à la classe de métadonnées.</span><span class="sxs-lookup"><span data-stu-id="9fc76-140">In this approach, the model class can be regenerated without losing all of the attributes that have been applied to the metadata class.</span></span>

<span data-ttu-id="9fc76-141">Dans le **modèles** dossier, ajoutez une classe nommée *Metadata.cs*.</span><span class="sxs-lookup"><span data-stu-id="9fc76-141">In the **Models** folder, add a class named *Metadata.cs*.</span></span>

<span data-ttu-id="9fc76-142">Remplacez le code dans *Metadata.cs* par le code suivant.</span><span class="sxs-lookup"><span data-stu-id="9fc76-142">Replace the code in *Metadata.cs* with the following code.</span></span>

[!code-csharp[Main](enhancing-data-validation/samples/sample3.cs)]

<span data-ttu-id="9fc76-143">Ces classes de métadonnées contient tous les attributs de validation que vous aviez précédemment appliqués aux classes de modèle.</span><span class="sxs-lookup"><span data-stu-id="9fc76-143">These metadata classes contain all of the validation attributes that you had previously applied to the model classes.</span></span> <span data-ttu-id="9fc76-144">Le **affichage** attribut est utilisé pour modifier la valeur utilisée pour les étiquettes de texte.</span><span class="sxs-lookup"><span data-stu-id="9fc76-144">The **Display** attribute is used to change the value used for text labels.</span></span>

<span data-ttu-id="9fc76-145">Maintenant, vous devez associer les classes de modèle avec les classes de métadonnées.</span><span class="sxs-lookup"><span data-stu-id="9fc76-145">Now, you must associate the model classes with the metadata classes.</span></span>

<span data-ttu-id="9fc76-146">Dans le **modèles** dossier, ajoutez une classe nommée *PartialClasses.cs*.</span><span class="sxs-lookup"><span data-stu-id="9fc76-146">In the **Models** folder, add a class named *PartialClasses.cs*.</span></span>

<span data-ttu-id="9fc76-147">Remplacez le contenu du fichier par le code suivant.</span><span class="sxs-lookup"><span data-stu-id="9fc76-147">Replace the contents of the file with the following code.</span></span>

[!code-csharp[Main](enhancing-data-validation/samples/sample4.cs)]

<span data-ttu-id="9fc76-148">Notez que chaque classe est marquée comme un `partial` classe et chacune correspond au nom et l’espace de noms que la classe qui est générée automatiquement.</span><span class="sxs-lookup"><span data-stu-id="9fc76-148">Notice that each class is marked as a `partial` class, and each matches the name and namespace as the class that is automatically generated.</span></span> <span data-ttu-id="9fc76-149">En appliquant l’attribut de métadonnées à la classe partielle, vous assurer que les attributs de validation de données seront être appliqués à la classe générée automatiquement.</span><span class="sxs-lookup"><span data-stu-id="9fc76-149">By applying the metadata attribute to the partial class, you ensure that the data validation attributes will be applied to the automatically-generated class.</span></span> <span data-ttu-id="9fc76-150">Ces attributs ne seront pas perdues lorsque vous régénérez les classes de modèle, car l’attribut de métadonnées est appliqué dans des classes partielles qui ne sont pas régénérées.</span><span class="sxs-lookup"><span data-stu-id="9fc76-150">These attributes will not be lost when you regenerate the model classes because the metadata attribute is applied in partial classes that are not regenerated.</span></span>

<span data-ttu-id="9fc76-151">Pour régénérer les classes générées automatiquement, ouvrez le *ContosoModel.edmx* fichier.</span><span class="sxs-lookup"><span data-stu-id="9fc76-151">To regenerate the automatically-generated classes, open the *ContosoModel.edmx* file.</span></span> <span data-ttu-id="9fc76-152">Une fois encore, avec le bouton droit sur l’aire de conception et sélectionnez **modèle de mise à jour à partir de la base de données**.</span><span class="sxs-lookup"><span data-stu-id="9fc76-152">Once again, right-click on the design surface and select **Update Model from Database**.</span></span> <span data-ttu-id="9fc76-153">Même si vous n’avez pas modifié la base de données, ce processus sera régénérer les classes.</span><span class="sxs-lookup"><span data-stu-id="9fc76-153">Even though you have not changed the database, this process will regenerate the classes.</span></span> <span data-ttu-id="9fc76-154">Dans le **Actualiser** onglet, sélectionnez **Tables** et **Terminer**.</span><span class="sxs-lookup"><span data-stu-id="9fc76-154">In the **Refresh** tab, select **Tables** and **Finish**.</span></span>

<span data-ttu-id="9fc76-155">Enregistrer le *ContosoModel.edmx* fichier pour appliquer les modifications.</span><span class="sxs-lookup"><span data-stu-id="9fc76-155">Save the *ContosoModel.edmx* file to apply the changes.</span></span>

<span data-ttu-id="9fc76-156">Ouvrez le *Student.cs* fichier ou le *Enrollment.cs* fichier et notez que les attributs de validation de données vous avez appliqué précédemment ne sont plus dans le fichier.</span><span class="sxs-lookup"><span data-stu-id="9fc76-156">Open the *Student.cs* file or the *Enrollment.cs* file, and notice that the data validation attributes you applied earlier are no longer in the file.</span></span> <span data-ttu-id="9fc76-157">Toutefois, exécutez l’application et notez que les règles de validation sont toujours appliqués lorsque vous entrez des données.</span><span class="sxs-lookup"><span data-stu-id="9fc76-157">However, run the application, and notice that the validation rules are still applied when you enter data.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="9fc76-158">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="9fc76-158">Additional resources</span></span>

<span data-ttu-id="9fc76-159">Pour obtenir une liste complète des annotations de validation de données que vous pouvez appliquer aux classes et propriétés, consultez [System.ComponentModel.DataAnnotations](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx).</span><span class="sxs-lookup"><span data-stu-id="9fc76-159">For a full list of data validation annotations you can apply to properties and classes, see [System.ComponentModel.DataAnnotations](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx).</span></span>

## <a name="next-steps"></a><span data-ttu-id="9fc76-160">Étapes suivantes</span><span class="sxs-lookup"><span data-stu-id="9fc76-160">Next steps</span></span>

<span data-ttu-id="9fc76-161">Dans ce didacticiel, vous avez effectué les actions suivantes :</span><span class="sxs-lookup"><span data-stu-id="9fc76-161">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="9fc76-162">Annotations de données ajoutées</span><span class="sxs-lookup"><span data-stu-id="9fc76-162">Added data annotations</span></span>
> * <span data-ttu-id="9fc76-163">Classes de métadonnées ajoutés</span><span class="sxs-lookup"><span data-stu-id="9fc76-163">Added metadata classes</span></span>

<span data-ttu-id="9fc76-164">Passez au didacticiel suivant pour apprendre à publier l’application web et la base de données vers Azure.</span><span class="sxs-lookup"><span data-stu-id="9fc76-164">Advance to the next tutorial to learn how to publish the web app and database to Azure.</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="9fc76-165">Publier sur Azure</span><span class="sxs-lookup"><span data-stu-id="9fc76-165">Publish to Azure</span></span>](publish-to-azure.md)