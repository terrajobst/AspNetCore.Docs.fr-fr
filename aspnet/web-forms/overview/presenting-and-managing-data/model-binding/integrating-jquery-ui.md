---
uid: web-forms/overview/presenting-and-managing-data/model-binding/integrating-jquery-ui
title: Intégration de sélecteur de dates JQuery UI avec la liaison de modèle et les web forms | Documents Microsoft
author: tfitzmac
description: Cette série de didacticiels montre les aspects de base de l’utilisation de la liaison de modèle avec un projet Web Forms ASP.NET. Liaison de modèle permet une interaction de données plus droites-...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/27/2014
ms.topic: article
ms.assetid: 3cbab37b-fb0f-4751-9ec4-74e068c3f380
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/integrating-jquery-ui
msc.type: authoredcontent
ms.openlocfilehash: 126262b440f3e914a7fac3f0b7eeadb4f648d2bb
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/06/2018
---
<a name="integrating-jquery-ui-datepicker-with-model-binding-and-web-forms"></a><span data-ttu-id="2d16b-104">Intégration de sélecteur de dates JQuery UI avec la liaison de modèle et les web forms</span><span class="sxs-lookup"><span data-stu-id="2d16b-104">Integrating JQuery UI Datepicker with model binding and web forms</span></span>
====================
<span data-ttu-id="2d16b-105">par [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="2d16b-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="2d16b-106">Cette série de didacticiels montre les aspects de base de l’utilisation de la liaison de modèle avec un projet Web Forms ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="2d16b-106">This tutorial series demonstrates basic aspects of using model binding with an ASP.NET Web Forms project.</span></span> <span data-ttu-id="2d16b-107">Liaison de modèle permet une interaction de données plus simple que vous traitez des données des objets de source (comme ObjectDataSource ou SqlDataSource).</span><span class="sxs-lookup"><span data-stu-id="2d16b-107">Model binding makes data interaction more straight-forward than dealing with data source objects (such as ObjectDataSource or SqlDataSource).</span></span> <span data-ttu-id="2d16b-108">Cette série commence par la partie introductive et déplace vers des concepts plus avancés dans des didacticiels ultérieurs.</span><span class="sxs-lookup"><span data-stu-id="2d16b-108">This series starts with introductory material and moves to more advanced concepts in later tutorials.</span></span>
> 
> <span data-ttu-id="2d16b-109">Ce didacticiel montre comment ajouter la JQuery UI [widget sélecteur](http://jqueryui.com/datepicker/) à un formulaire Web et utiliser le modèle de liaison pour mettre à jour la base de données avec la valeur sélectionnée.</span><span class="sxs-lookup"><span data-stu-id="2d16b-109">This tutorial shows how to add the JQuery UI [Datepicker widget](http://jqueryui.com/datepicker/) to a Web Form, and use model binding to update the database with the selected value.</span></span>
> 
> <span data-ttu-id="2d16b-110">Ce didacticiel s’appuie sur le projet créé dans le [première](retrieving-data.md) et [deuxième](updating-deleting-and-creating-data.md) parties de la série.</span><span class="sxs-lookup"><span data-stu-id="2d16b-110">This tutorial builds on the project created in the [first](retrieving-data.md) and [second](updating-deleting-and-creating-data.md) parts of the series.</span></span>
> 
> <span data-ttu-id="2d16b-111">Vous pouvez [télécharger](https://go.microsoft.com/fwlink/?LinkId=286116) le projet complet en c# ou VB.</span><span class="sxs-lookup"><span data-stu-id="2d16b-111">You can [download](https://go.microsoft.com/fwlink/?LinkId=286116) the complete project in C# or VB.</span></span> <span data-ttu-id="2d16b-112">Le code téléchargeable fonctionne avec Visual Studio 2012 ou Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="2d16b-112">The downloadable code works with either Visual Studio 2012 or Visual Studio 2013.</span></span> <span data-ttu-id="2d16b-113">Elle utilise le modèle Visual Studio 2012, qui est légèrement différent de celle du modèle de Visual Studio 2013 indiqué dans ce didacticiel.</span><span class="sxs-lookup"><span data-stu-id="2d16b-113">It uses the Visual Studio 2012 template, which is slightly different than the Visual Studio 2013 template shown in this tutorial.</span></span>


## <a name="what-youll-build"></a><span data-ttu-id="2d16b-114">Vous allez générer</span><span class="sxs-lookup"><span data-stu-id="2d16b-114">What you'll build</span></span>

<span data-ttu-id="2d16b-115">Dans ce didacticiel, vous effectuerez les tâches suivantes :</span><span class="sxs-lookup"><span data-stu-id="2d16b-115">In this tutorial, you'll:</span></span>

1. <span data-ttu-id="2d16b-116">Ajouter une propriété à votre modèle pour enregistrer la date d’inscription de l’étudiant</span><span class="sxs-lookup"><span data-stu-id="2d16b-116">Add a property to your model to record the student's enrollment date</span></span>
2. <span data-ttu-id="2d16b-117">Activer l’utilisateur de sélectionner la date d’inscription à l’aide du sélecteur de dates JQuery UI de widget</span><span class="sxs-lookup"><span data-stu-id="2d16b-117">Enable the user to select the enrollment date using the JQuery UI Datepicker widget</span></span>
3. <span data-ttu-id="2d16b-118">Appliquer les règles de validation pour la date d’inscription</span><span class="sxs-lookup"><span data-stu-id="2d16b-118">Enforce validation rules for the enrollment date</span></span>

<span data-ttu-id="2d16b-119">Le widget de sélecteur de dates JQuery UI permet aux utilisateurs de facilement sélectionner une date dans le calendrier qui s’affiche lorsque l’utilisateur interagit avec le champ.</span><span class="sxs-lookup"><span data-stu-id="2d16b-119">The JQuery UI Datepicker widget enables users to easily select a date from a calendar that pops up when the user interacts with the field.</span></span> <span data-ttu-id="2d16b-120">À l’aide de ce widget peut être plus pratique pour les utilisateurs à taper manuellement une date.</span><span class="sxs-lookup"><span data-stu-id="2d16b-120">Using this widget can be more convenient for users than manually typing a date.</span></span> <span data-ttu-id="2d16b-121">Intégration du widget de sélecteur de dates dans une page qui utilise la liaison de modèle pour les opérations de données requiert uniquement une petite quantité de travail supplémentaire.</span><span class="sxs-lookup"><span data-stu-id="2d16b-121">Integrating the Datepicker widget into a page that uses model binding for data operations requires only a small amount of additional work.</span></span>

## <a name="add-a-new-property-to-the-model"></a><span data-ttu-id="2d16b-122">Ajouter une nouvelle propriété au modèle</span><span class="sxs-lookup"><span data-stu-id="2d16b-122">Add a new property to the model</span></span>

<span data-ttu-id="2d16b-123">Tout d’abord, vous allez ajouter un **Datetime** propriété à votre étudiant pour modéliser et migrer cette modification à la base de données.</span><span class="sxs-lookup"><span data-stu-id="2d16b-123">First, you will add a **Datetime** property to your Student model and migrate that change to the database.</span></span> <span data-ttu-id="2d16b-124">Ouvrez **UniversityModels.cs**et ajoutez le code en surbrillance vers le modèle de Student.</span><span class="sxs-lookup"><span data-stu-id="2d16b-124">Open **UniversityModels.cs**, and add the highlighted code to the Student model.</span></span>

[!code-csharp[Main](integrating-jquery-ui/samples/sample1.cs?highlight=16-18)]

<span data-ttu-id="2d16b-125">Le **RangeAttribute** est inclus pour appliquer des règles de validation pour la propriété.</span><span class="sxs-lookup"><span data-stu-id="2d16b-125">The **RangeAttribute** is included to enforce validation rules for the property.</span></span> <span data-ttu-id="2d16b-126">Pour ce didacticiel, nous supposerons que Contoso University a été fondée sur le 1er janvier 2013 et par conséquent, les dates antérieures d’inscription ne sont pas valides.</span><span class="sxs-lookup"><span data-stu-id="2d16b-126">For this tutorial, we will assume that Contoso University was founded on January 1st, 2013 and therefore earlier enrollment dates are not valid.</span></span>

<span data-ttu-id="2d16b-127">Dans la fenêtre de gestion des packages, ajouter une migration en exécutant la commande **migration ajouter AddEnrollmentDate**.</span><span class="sxs-lookup"><span data-stu-id="2d16b-127">In the Package Management window, add a migration by running the command **add-migration AddEnrollmentDate**.</span></span> <span data-ttu-id="2d16b-128">Notez que le code de migration ajoute la nouvelle colonne Datetime à la table d’étudiants.</span><span class="sxs-lookup"><span data-stu-id="2d16b-128">Notice that the migration code adds the new Datetime column to the Student table.</span></span> <span data-ttu-id="2d16b-129">Pour faire correspondre la valeur que vous avez spécifié dans le RangeAttribute, ajoutez une valeur par défaut pour la nouvelle colonne, comme indiqué dans le code en surbrillance ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="2d16b-129">To match the value you specified in the RangeAttribute, add a default value for the new column, as shown in the highlighted code below.</span></span>

[!code-csharp[Main](integrating-jquery-ui/samples/sample2.cs?highlight=11)]

<span data-ttu-id="2d16b-130">Enregistrez vos modifications dans le fichier de migration.</span><span class="sxs-lookup"><span data-stu-id="2d16b-130">Save your change to the migration file.</span></span>

<span data-ttu-id="2d16b-131">Vous n’avez pas besoin pour les données de valeur initiale à nouveau.</span><span class="sxs-lookup"><span data-stu-id="2d16b-131">You do not need to seed the data again.</span></span> <span data-ttu-id="2d16b-132">Par conséquent, ouvrez **Configuration.cs** dans le dossier Migrations et supprimez ou mettez en commentaire le code dans le **Seed** (méthode).</span><span class="sxs-lookup"><span data-stu-id="2d16b-132">Therefore, open **Configuration.cs** in the Migrations folder and remove or comment out the code in the **Seed** method.</span></span> <span data-ttu-id="2d16b-133">Enregistrez et fermez le fichier.</span><span class="sxs-lookup"><span data-stu-id="2d16b-133">Save and close the file.</span></span>

<span data-ttu-id="2d16b-134">Maintenant, exécutez la commande **mise à jour de la base de données**.</span><span class="sxs-lookup"><span data-stu-id="2d16b-134">Now, run the command **update-database**.</span></span> <span data-ttu-id="2d16b-135">Notez que la colonne existe maintenant dans la base de données et tous les enregistrements existants ont la valeur par défaut pour EnrollmentDate.</span><span class="sxs-lookup"><span data-stu-id="2d16b-135">Notice that the column now exists in the database and all of the existing records have the default value for EnrollmentDate.</span></span>

## <a name="add-dynamic-controls-for-enrollment-date"></a><span data-ttu-id="2d16b-136">Ajoutez des contrôles dynamiques pour la date d’inscription</span><span class="sxs-lookup"><span data-stu-id="2d16b-136">Add dynamic controls for enrollment date</span></span>

<span data-ttu-id="2d16b-137">Vous allez maintenant ajouter des contrôles pour afficher et modifier la date d’inscription.</span><span class="sxs-lookup"><span data-stu-id="2d16b-137">You will now add controls for displaying and editing the enrollment date.</span></span> <span data-ttu-id="2d16b-138">À ce stade, la valeur est modifiée par une zone de texte.</span><span class="sxs-lookup"><span data-stu-id="2d16b-138">At this point, the value is edited through a text box.</span></span> <span data-ttu-id="2d16b-139">Plus loin dans ce didacticiel, vous allez modifier la zone de texte pour le widget de JQuery Datepicker.</span><span class="sxs-lookup"><span data-stu-id="2d16b-139">Later in the tutorial, you will change the text box to the JQuery Datepicker widget.</span></span>

<span data-ttu-id="2d16b-140">Tout d’abord, il est important de noter que vous n’avez pas besoin effectuer toute modification apportée à la **AddStudent.aspx** fichier.</span><span class="sxs-lookup"><span data-stu-id="2d16b-140">First, it is important to note that you do not need to make any change to the **AddStudent.aspx** file.</span></span> <span data-ttu-id="2d16b-141">Le contrôle DynamicEntity apparaîtront automatiquement la nouvelle propriété.</span><span class="sxs-lookup"><span data-stu-id="2d16b-141">The DynamicEntity control will automatically render the new property.</span></span>

<span data-ttu-id="2d16b-142">Ouvrez **Students.aspx**et ajoutez le code en surbrillance suivant.</span><span class="sxs-lookup"><span data-stu-id="2d16b-142">Open **Students.aspx**, and add the following highlighted code.</span></span>

[!code-aspx[Main](integrating-jquery-ui/samples/sample3.aspx?highlight=13)]

<span data-ttu-id="2d16b-143">Exécutez l’application et remarquez que vous pouvez définir la valeur de la date d’inscription en tapant une date.</span><span class="sxs-lookup"><span data-stu-id="2d16b-143">Run the application and notice that you can set the value of the enrollment date by typing a date.</span></span> <span data-ttu-id="2d16b-144">Lorsque vous ajoutez un nouvel étudiant :</span><span class="sxs-lookup"><span data-stu-id="2d16b-144">When adding a new student:</span></span>

![définir la date](integrating-jquery-ui/_static/image1.png)

<span data-ttu-id="2d16b-146">Ou bien, la modification d’une valeur existante :</span><span class="sxs-lookup"><span data-stu-id="2d16b-146">Or, editing an existing value:</span></span>

![modifier la date](integrating-jquery-ui/_static/image2.png)

<span data-ttu-id="2d16b-148">Tapez la date, mais cela ne peut pas être l’expérience utilisateur que vous souhaitez fournir.</span><span class="sxs-lookup"><span data-stu-id="2d16b-148">Typing the date works, but it might not be the customer experience you wish to provide.</span></span> <span data-ttu-id="2d16b-149">Dans la section suivante, vous allez activer la sélection d’une date dans un calendrier.</span><span class="sxs-lookup"><span data-stu-id="2d16b-149">In the next section, you will enable selecting a date through a calendar.</span></span>

## <a name="install-nuget-package-to-work-with-jquery-ui"></a><span data-ttu-id="2d16b-150">Installation du package NuGet pour travailler avec JQuery UI</span><span class="sxs-lookup"><span data-stu-id="2d16b-150">Install NuGet package to work with JQuery UI</span></span>

<span data-ttu-id="2d16b-151">Le **l’interface utilisateur de jus** package NuGet s’intègre facilement des widgets JQuery UI dans votre application web.</span><span class="sxs-lookup"><span data-stu-id="2d16b-151">The **Juice UI** NuGet package enables easy integration of the JQuery UI widgets into your web application.</span></span> <span data-ttu-id="2d16b-152">Pour utiliser ce package, vous devez l’installer via NuGet.</span><span class="sxs-lookup"><span data-stu-id="2d16b-152">To use this package, install it through NuGet.</span></span>

![Ajouter une interface utilisateur de jus](integrating-jquery-ui/_static/image3.png)

<span data-ttu-id="2d16b-154">La version de l’interface utilisateur de jus que vous installez peut entrer en conflit avec la version de JQuery dans votre application.</span><span class="sxs-lookup"><span data-stu-id="2d16b-154">The version of Juice UI that you install may conflict with the version of JQuery in your application.</span></span> <span data-ttu-id="2d16b-155">Avant de poursuivre ce didacticiel, essayez d’exécuter votre application.</span><span class="sxs-lookup"><span data-stu-id="2d16b-155">Before proceeding with this tutorial, try running your application.</span></span> <span data-ttu-id="2d16b-156">Si vous rencontrez une erreur de JavaScript, vous devez rapprocher la version de JQuery.</span><span class="sxs-lookup"><span data-stu-id="2d16b-156">If you encounter a JavaScript error, you need to reconcile the JQuery version.</span></span> <span data-ttu-id="2d16b-157">Vous pouvez ajouter la version attendue de JQuery vers votre dossier de Scripts (version 1.8.2 au moment de l’écriture de ce didacticiel), ou dans Site.master, spécifiez le chemin d’accès au fichier JQuery.</span><span class="sxs-lookup"><span data-stu-id="2d16b-157">You can either add the expected version of JQuery to your Scripts folder (version 1.8.2 at time of writing this tutorial), or in Site.master specify the path to the JQuery file.</span></span>

[!code-aspx[Main](integrating-jquery-ui/samples/sample4.aspx)]

## <a name="customize-datetime-template-to-include-datepicker-widget"></a><span data-ttu-id="2d16b-158">Personnaliser le modèle de date/heure pour inclure le widget de sélecteur de dates</span><span class="sxs-lookup"><span data-stu-id="2d16b-158">Customize DateTime template to include Datepicker widget</span></span>

<span data-ttu-id="2d16b-159">Vous allez ajouter le widget de sélecteur de dates pour le modèle de données dynamiques pour la modification d’une valeur datetime.</span><span class="sxs-lookup"><span data-stu-id="2d16b-159">You will add the Datepicker widget to the dynamic data template for editing a datetime value.</span></span> <span data-ttu-id="2d16b-160">En ajoutant le widget au modèle, il est restitué dans les deux le formulaire pour ajouter un nouvel étudiant et dans l’affichage de grille pour les étudiants éditions automatiquement.</span><span class="sxs-lookup"><span data-stu-id="2d16b-160">By adding the widget to the template, it is automatically rendered in both the form for adding a new student and in the grid view for editing students.</span></span> <span data-ttu-id="2d16b-161">Ouvrez **DateTime\_Data Edit.ascx**et ajoutez le code en surbrillance suivant.</span><span class="sxs-lookup"><span data-stu-id="2d16b-161">Open **DateTime\_Edit.ascx**, and add the following highlighted code.</span></span>

[!code-aspx[Main](integrating-jquery-ui/samples/sample5.aspx?highlight=3)]

<span data-ttu-id="2d16b-162">Dans le fichier code-behind, vous allez définir les dates minimales et maximales pour le sélecteur de dates.</span><span class="sxs-lookup"><span data-stu-id="2d16b-162">In the code-behind file, you will set the minimum and maximum dates for the DatePicker.</span></span> <span data-ttu-id="2d16b-163">En définissant ces valeurs, vous empêchera les utilisateurs de naviguer vers les dates non valides.</span><span class="sxs-lookup"><span data-stu-id="2d16b-163">By setting these values, you will prevent users from navigating to invalid dates.</span></span> <span data-ttu-id="2d16b-164">Vous allez extraire les valeurs minimales et maximales de la **RangeAttribute** sur la propriété DateTime, le cas échéant.</span><span class="sxs-lookup"><span data-stu-id="2d16b-164">You will retrieve the minimum and maximum values from the **RangeAttribute** on the DateTime property, if one is provided.</span></span> <span data-ttu-id="2d16b-165">Ouvrez **DateTime\_Edit.ascx.cs**et ajoutez le code en surbrillance suivant à la Page\_Load (méthode).</span><span class="sxs-lookup"><span data-stu-id="2d16b-165">Open **DateTime\_Edit.ascx.cs**, and add the following highlighted code to the Page\_Load method.</span></span>

[!code-csharp[Main](integrating-jquery-ui/samples/sample6.cs?highlight=9-14)]

<span data-ttu-id="2d16b-166">Exécuter l’application web et accédez à la page AddStudent.</span><span class="sxs-lookup"><span data-stu-id="2d16b-166">Run the web application and navigate to the AddStudent page.</span></span> <span data-ttu-id="2d16b-167">Fournir des valeurs pour les champs et notez que lorsque vous cliquez sur la zone de texte pour la Date d’inscription, le calendrier s’affiche.</span><span class="sxs-lookup"><span data-stu-id="2d16b-167">Provide values for the fields and notice that when you click on the text box for Enrollment Date, the calendar is displayed.</span></span>

![Sélecteur de dates](integrating-jquery-ui/_static/image4.png)

<span data-ttu-id="2d16b-169">Sélectionner une date, puis cliquez sur **insérer**.</span><span class="sxs-lookup"><span data-stu-id="2d16b-169">Pick a date, and click **Insert**.</span></span> <span data-ttu-id="2d16b-170">Le RangeAttribute effectue la validation sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="2d16b-170">The RangeAttribute enforces validation on the server.</span></span> <span data-ttu-id="2d16b-171">En définissant la propriété minDate le sélecteur de dates, vous également appliquez la validation sur le client.</span><span class="sxs-lookup"><span data-stu-id="2d16b-171">By setting the minDate property on the Datepicker, you also apply validation on the client.</span></span> <span data-ttu-id="2d16b-172">Le calendrier ne permet pas l’utilisateur de naviguer jusqu'à une date avant la valeur de minDate.</span><span class="sxs-lookup"><span data-stu-id="2d16b-172">The calendar does not let the user navigate to a date prior to the value of minDate.</span></span>

<span data-ttu-id="2d16b-173">Lorsque vous modifiez un enregistrement dans la vue grille, le calendrier s’affiche également.</span><span class="sxs-lookup"><span data-stu-id="2d16b-173">When you edit a record in the grid view, the calendar is also displayed.</span></span>

![Sélecteur de dates dans le contrôle GridView](integrating-jquery-ui/_static/image5.png)

## <a name="conclusion"></a><span data-ttu-id="2d16b-175">Conclusion</span><span class="sxs-lookup"><span data-stu-id="2d16b-175">Conclusion</span></span>

<span data-ttu-id="2d16b-176">Dans ce didacticiel, vous avez appris comment incorporer un widget de JQuery dans un formulaire web qui utilise la liaison de modèle.</span><span class="sxs-lookup"><span data-stu-id="2d16b-176">In this tutorial, you learned how to incorporate a JQuery widget into a web form that uses model binding.</span></span>

<span data-ttu-id="2d16b-177">Dans la prochaine [didacticiel](using-query-string-values-to-retrieve-data.md), vous allez utiliser une valeur de chaîne de requête lors de la sélection de données.</span><span class="sxs-lookup"><span data-stu-id="2d16b-177">In the next [tutorial](using-query-string-values-to-retrieve-data.md), you will use a query string value when selecting data.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="2d16b-178">[Précédent](sorting-paging-and-filtering-data.md)
> [Suivant](using-query-string-values-to-retrieve-data.md)</span><span class="sxs-lookup"><span data-stu-id="2d16b-178">[Previous](sorting-paging-and-filtering-data.md)
[Next](using-query-string-values-to-retrieve-data.md)</span></span>
