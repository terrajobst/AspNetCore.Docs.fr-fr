---
uid: mvc/overview/getting-started/database-first-development/enhancing-data-validation
title: 'Base de données EF tout d’abord avec ASP.NET MVC : amélioration de la Validation des données | Documents Microsoft'
author: tfitzmac
description: À l’aide de la structure d’ASP.NET MVC et Entity Framework, vous pouvez créer une application web qui fournit une interface à une base de données existante. Ce didacticiel seri...
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/29/2014
ms.topic: article
ms.assetid: 0ed5e67a-34c0-4b57-84a6-802b0fb3cd00
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/database-first-development/enhancing-data-validation
msc.type: authoredcontent
ms.openlocfilehash: 8ea2e94db7956b76c5ccf0a139ac024e38910b49
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30879606"
---
<a name="ef-database-first-with-aspnet-mvc-enhancing-data-validation"></a><span data-ttu-id="4d9a3-104">Base de données EF tout d’abord avec ASP.NET MVC : amélioration de la Validation des données</span><span class="sxs-lookup"><span data-stu-id="4d9a3-104">EF Database First with ASP.NET MVC: Enhancing Data Validation</span></span>
====================
<span data-ttu-id="4d9a3-105">par [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="4d9a3-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="4d9a3-106">À l’aide de la structure d’ASP.NET MVC et Entity Framework, vous pouvez créer une application web qui fournit une interface à une base de données existante.</span><span class="sxs-lookup"><span data-stu-id="4d9a3-106">Using MVC, Entity Framework, and ASP.NET Scaffolding, you can create a web application that provides an interface to an existing database.</span></span> <span data-ttu-id="4d9a3-107">Cette série de didacticiels vous montre comment automatiquement générer du code qui permet aux utilisateurs d’afficher, modifier, créer et supprimer des données qui résident dans une table de base de données.</span><span class="sxs-lookup"><span data-stu-id="4d9a3-107">This tutorial series shows you how to automatically generate code that enables users to display, edit, create, and delete data that resides in a database table.</span></span> <span data-ttu-id="4d9a3-108">Le code généré correspond aux colonnes dans la table de base de données.</span><span class="sxs-lookup"><span data-stu-id="4d9a3-108">The generated code corresponds to the columns in the database table.</span></span>
> 
> <span data-ttu-id="4d9a3-109">Cette partie de la série se concentre sur l’ajout d’annotations de données au modèle de données pour spécifier les exigences de validation et afficher la mise en forme.</span><span class="sxs-lookup"><span data-stu-id="4d9a3-109">This part of the series focuses on adding data annotations to the data model to specify validation requirements and display formatting.</span></span> <span data-ttu-id="4d9a3-110">Il a été améliorée en fonction des commentaires des utilisateurs dans la section commentaires.</span><span class="sxs-lookup"><span data-stu-id="4d9a3-110">It was improved based on feedback from users in the comments section.</span></span>


## <a name="add-data-annotations"></a><span data-ttu-id="4d9a3-111">Ajouter des annotations de données</span><span class="sxs-lookup"><span data-stu-id="4d9a3-111">Add data annotations</span></span>

<span data-ttu-id="4d9a3-112">Comme vous l’avez vu dans une rubrique précédente, certaines règles de validation de données sont automatiquement appliquées à l’entrée d’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="4d9a3-112">As you saw in an earlier topic, some data validation rules are automatically applied to the user input.</span></span> <span data-ttu-id="4d9a3-113">Par exemple, vous pouvez fournir uniquement un nombre pour la propriété de classe.</span><span class="sxs-lookup"><span data-stu-id="4d9a3-113">For example, you can only provide a number for the Grade property.</span></span> <span data-ttu-id="4d9a3-114">Pour spécifier plusieurs règles de validation de données, vous pouvez ajouter des annotations de données à votre classe de modèle.</span><span class="sxs-lookup"><span data-stu-id="4d9a3-114">To specify more data validation rules, you can add data annotations to your model class.</span></span> <span data-ttu-id="4d9a3-115">Ces annotations sont appliquées dans l’ensemble de votre application web pour la propriété spécifiée.</span><span class="sxs-lookup"><span data-stu-id="4d9a3-115">These annotations are applied throughout your web application for the specified property.</span></span> <span data-ttu-id="4d9a3-116">Vous pouvez également appliquer des attributs de mise en forme qui modifient la façon dont les propriétés sont affichées ; comme la modification de la valeur utilisée pour les étiquettes de texte.</span><span class="sxs-lookup"><span data-stu-id="4d9a3-116">You can also apply formatting attributes that change how the properties are displayed; such as, changing the value used for text labels.</span></span>

<span data-ttu-id="4d9a3-117">Dans ce didacticiel, vous allez ajouter des annotations de données pour limiter la longueur des valeurs fournies pour les propriétés FirstName, LastName et MiddleName.</span><span class="sxs-lookup"><span data-stu-id="4d9a3-117">In this tutorial, you will add data annotations to restrict the length of the values provided for the FirstName, LastName, and MiddleName properties.</span></span> <span data-ttu-id="4d9a3-118">Dans la base de données, ces valeurs sont limités à 50 caractères. Toutefois, dans votre application web ce caractère est actuellement pas appliqué.</span><span class="sxs-lookup"><span data-stu-id="4d9a3-118">In the database, these values are limited to 50 characters; however, in your web application that character limit is currently not enforced.</span></span> <span data-ttu-id="4d9a3-119">Si un utilisateur fournit plus de 50 caractères pour l’une de ces valeurs, la page se bloque quand vous tentez d’enregistrer la valeur dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="4d9a3-119">If a user provides more than 50 characters for one of those values, the page will crash when attempting to save the value to the database.</span></span> <span data-ttu-id="4d9a3-120">Vous allez également restreindre le niveau pour les valeurs comprises entre 0 et 4.</span><span class="sxs-lookup"><span data-stu-id="4d9a3-120">You will also restrict Grade to values between 0 and 4.</span></span>

<span data-ttu-id="4d9a3-121">Ouvrez le **Student.cs** de fichiers dans le **modèles** dossier.</span><span class="sxs-lookup"><span data-stu-id="4d9a3-121">Open the **Student.cs** file in the **Models** folder.</span></span> <span data-ttu-id="4d9a3-122">Ajoutez le code en surbrillance suivant à la classe.</span><span class="sxs-lookup"><span data-stu-id="4d9a3-122">Add the following highlighted code to the class.</span></span>

[!code-csharp[Main](enhancing-data-validation/samples/sample1.cs?highlight=5,15,17,20)]

<span data-ttu-id="4d9a3-123">Dans Enrollment.cs, ajoutez le code en surbrillance suivant.</span><span class="sxs-lookup"><span data-stu-id="4d9a3-123">In Enrollment.cs, add the following highlighted code.</span></span>

[!code-csharp[Main](enhancing-data-validation/samples/sample2.cs?highlight=5,10)]

<span data-ttu-id="4d9a3-124">Générez la solution.</span><span class="sxs-lookup"><span data-stu-id="4d9a3-124">Build the solution.</span></span>

<span data-ttu-id="4d9a3-125">Accédez à une page de modification ou de création d’un étudiant.</span><span class="sxs-lookup"><span data-stu-id="4d9a3-125">Browse to a page for editing or creating a student.</span></span> <span data-ttu-id="4d9a3-126">Si vous essayez d’entrer plus de 50 caractères, un message d’erreur s’affiche.</span><span class="sxs-lookup"><span data-stu-id="4d9a3-126">If you attempt to enter more than 50 characters, an error message is displayed.</span></span>

![afficher le message d’erreur](enhancing-data-validation/_static/image1.png)

<span data-ttu-id="4d9a3-128">Accédez à la page de modification des inscriptions et tentent de fournir un niveau au-dessus de 4.</span><span class="sxs-lookup"><span data-stu-id="4d9a3-128">Browse to the page for editing enrollments, and attempt to provide a grade above 4.</span></span>

![Erreur de plage de niveau](enhancing-data-validation/_static/image2.png)

<span data-ttu-id="4d9a3-130">Pour obtenir une liste complète des annotations de validation de données que vous pouvez appliquer aux classes et propriétés, consultez [System.ComponentModel.DataAnnotations](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx).</span><span class="sxs-lookup"><span data-stu-id="4d9a3-130">For a full list of data validation annotations you can apply to properties and classes, see [System.ComponentModel.DataAnnotations](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx).</span></span>

## <a name="add-metadata-classes"></a><span data-ttu-id="4d9a3-131">Ajouter des classes de métadonnées</span><span class="sxs-lookup"><span data-stu-id="4d9a3-131">Add metadata classes</span></span>

<span data-ttu-id="4d9a3-132">Ajout d’attributs de validation directement à la classe de modèle fonctionne lorsque vous ne prévoyez pas de la base de données pour modifier ; Toutefois, si vos modifications de la base de données et que vous devez régénérer la classe de modèle, vous perdrez tous les attributs que vous aviez appliqué à la classe de modèle.</span><span class="sxs-lookup"><span data-stu-id="4d9a3-132">Adding the validation attributes directly to the model class works when you do not expect the database to change; however, if your database changes and you need to regenerate the model class, you will lose all of the attributes you had applied to the model class.</span></span> <span data-ttu-id="4d9a3-133">Cette approche peut être très inefficace et sujette à la perte des règles de validation importantes.</span><span class="sxs-lookup"><span data-stu-id="4d9a3-133">This approach can be very inefficient and prone to losing important validation rules.</span></span>

<span data-ttu-id="4d9a3-134">Pour éviter ce problème, vous pouvez ajouter une classe de métadonnées qui contient les attributs.</span><span class="sxs-lookup"><span data-stu-id="4d9a3-134">To avoid this problem, you can add a metadata class that contains the attributes.</span></span> <span data-ttu-id="4d9a3-135">Lorsque vous associez la classe de modèle pour la classe de métadonnées, ces attributs sont appliqués au modèle.</span><span class="sxs-lookup"><span data-stu-id="4d9a3-135">When you associate the model class to the metadata class, those attributes are applied to the model.</span></span> <span data-ttu-id="4d9a3-136">Dans cette approche, la classe de modèle peut être régénérée sans perte de tous les attributs qui ont été appliqués à la classe de métadonnées.</span><span class="sxs-lookup"><span data-stu-id="4d9a3-136">In this approach, the model class can be regenerated without losing all of the attributes that have been applied to the metadata class.</span></span>

<span data-ttu-id="4d9a3-137">Dans le **modèles** dossier, ajoutez une classe nommée **Metadata.cs**.</span><span class="sxs-lookup"><span data-stu-id="4d9a3-137">In the **Models** folder, add a class named **Metadata.cs**.</span></span>

![ajouter la classe de métadonnées](enhancing-data-validation/_static/image3.png)

<span data-ttu-id="4d9a3-139">Remplacez le code dans Metadata.cs par le code suivant.</span><span class="sxs-lookup"><span data-stu-id="4d9a3-139">Replace the code in Metadata.cs with the following code.</span></span>

[!code-csharp[Main](enhancing-data-validation/samples/sample3.cs)]

<span data-ttu-id="4d9a3-140">Ces classes de métadonnées contiennent tous les attributs de validation que vous aviez précédemment appliqué aux classes de modèle.</span><span class="sxs-lookup"><span data-stu-id="4d9a3-140">These metadata classes contain all of the validation attributes that you had previously applied to the model classes.</span></span> <span data-ttu-id="4d9a3-141">Le **affichage** attribut est utilisé pour modifier la valeur utilisée pour les étiquettes de texte.</span><span class="sxs-lookup"><span data-stu-id="4d9a3-141">The **Display** attribute is used to change the value used for text labels.</span></span>

<span data-ttu-id="4d9a3-142">Maintenant, vous devez associer les classes de modèle avec les classes de métadonnées.</span><span class="sxs-lookup"><span data-stu-id="4d9a3-142">Now, you must associate the model classes with the metadata classes.</span></span>

<span data-ttu-id="4d9a3-143">Dans le **modèles** dossier, ajoutez une classe nommée **PartialClasses.cs**.</span><span class="sxs-lookup"><span data-stu-id="4d9a3-143">In the **Models** folder, add a class named **PartialClasses.cs**.</span></span>

<span data-ttu-id="4d9a3-144">Remplacez le contenu du fichier par le code suivant.</span><span class="sxs-lookup"><span data-stu-id="4d9a3-144">Replace the contents of the file with the following code.</span></span>

[!code-csharp[Main](enhancing-data-validation/samples/sample4.cs)]

<span data-ttu-id="4d9a3-145">Notez que chaque classe est marquée comme un `partial` classe et chacun correspond au nom et l’espace de noms que la classe qui est générée automatiquement.</span><span class="sxs-lookup"><span data-stu-id="4d9a3-145">Notice that each class is marked as a `partial` class, and each matches the name and namespace as the class that is automatically generated.</span></span> <span data-ttu-id="4d9a3-146">En appliquant l’attribut de métadonnées à la classe partielle, vous assurez que les attributs de validation de données seront être appliquées à la classe générée automatiquement.</span><span class="sxs-lookup"><span data-stu-id="4d9a3-146">By applying the metadata attribute to the partial class, you ensure that the data validation attributes will be applied to the automatically-generated class.</span></span> <span data-ttu-id="4d9a3-147">Ces attributs ne seront pas perdues lorsque vous régénérez les classes du modèle, car l’attribut de métadonnées est appliqué dans des classes partielles qui ne sont pas régénérées.</span><span class="sxs-lookup"><span data-stu-id="4d9a3-147">These attributes will not be lost when you regenerate the model classes because the metadata attribute is applied in partial classes that are not regenerated.</span></span>

<span data-ttu-id="4d9a3-148">Pour régénérer les classes générées automatiquement, ouvrez le fichier ContosoModel.edmx.</span><span class="sxs-lookup"><span data-stu-id="4d9a3-148">To regenerate the automatically-generated classes, open the ContosoModel.edmx file.</span></span> <span data-ttu-id="4d9a3-149">Une fois encore, avec le bouton droit sur l’aire de conception et sélectionnez **modèle de mise à jour à partir de la base de données**.</span><span class="sxs-lookup"><span data-stu-id="4d9a3-149">Once again, right-click on the design surface and select **Update Model from Database**.</span></span> <span data-ttu-id="4d9a3-150">Même si vous n’avez pas modifié la base de données, ce processus sera régénérer les classes.</span><span class="sxs-lookup"><span data-stu-id="4d9a3-150">Even though you have not changed the database, this process will regenerate the classes.</span></span> <span data-ttu-id="4d9a3-151">Dans le **Actualiser** onglet, sélectionnez **Tables** et **Terminer**.</span><span class="sxs-lookup"><span data-stu-id="4d9a3-151">In the **Refresh** tab, select **Tables** and **Finish**.</span></span>

![Actualiser les tables](enhancing-data-validation/_static/image4.png)

<span data-ttu-id="4d9a3-153">Enregistrez le fichier ContosoModel.edmx pour appliquer les modifications.</span><span class="sxs-lookup"><span data-stu-id="4d9a3-153">Save the ContosoModel.edmx file to apply the changes.</span></span>

<span data-ttu-id="4d9a3-154">Ouvrez le fichier Student.cs ou le fichier Enrollment.cs et notez que les attributs de validation de données que vous appliqué précédemment ne sont plus dans le fichier.</span><span class="sxs-lookup"><span data-stu-id="4d9a3-154">Open the Student.cs file or the Enrollment.cs file, and notice that the data validation attributes you applied earlier are no longer in the file.</span></span> <span data-ttu-id="4d9a3-155">Toutefois, exécutez l’application et notez que les règles de validation sont toujours appliquées lorsque vous entrez des données.</span><span class="sxs-lookup"><span data-stu-id="4d9a3-155">However, run the application, and notice that the validation rules are still applied when you enter data.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="4d9a3-156">[Précédent](customizing-a-view.md)
> [Suivant](publish-to-azure.md)</span><span class="sxs-lookup"><span data-stu-id="4d9a3-156">[Previous](customizing-a-view.md)
[Next](publish-to-azure.md)</span></span>
