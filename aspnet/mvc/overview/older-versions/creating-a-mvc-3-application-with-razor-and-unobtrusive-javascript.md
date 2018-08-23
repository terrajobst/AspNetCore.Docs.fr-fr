---
uid: mvc/overview/older-versions/creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript
title: Création d’un MVC 3 Application avec Razor et JavaScript non Obstrusif | Microsoft Docs
author: microsoft
description: L’exemple d’application web liste utilisateur montre combien il est simple de créer des applications ASP.NET MVC 3 à l’aide du moteur de vue Razor. Les opérations de mappage application exemple...
ms.author: riande
ms.date: 11/01/2010
ms.assetid: 658b149b-d770-46bf-8b4b-4e47cca242f3
msc.legacyurl: /mvc/overview/older-versions/creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript
msc.type: authoredcontent
ms.openlocfilehash: f60ca3e76fda8a18da1ad83e955e5e4759f5e3af
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41827418"
---
<a name="creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript"></a><span data-ttu-id="14b07-104">Création d’un MVC 3 Application avec Razor et JavaScript non Obstrusif</span><span class="sxs-lookup"><span data-stu-id="14b07-104">Creating a MVC 3 Application with Razor and Unobtrusive JavaScript</span></span>
====================
<span data-ttu-id="14b07-105">par [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="14b07-105">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="14b07-106">L’exemple d’application web liste utilisateur montre combien il est simple de créer des applications ASP.NET MVC 3 à l’aide du moteur de vue Razor.</span><span class="sxs-lookup"><span data-stu-id="14b07-106">The User List sample web application demonstrates how simple it is to create ASP.NET MVC 3 applications using the Razor view engine.</span></span> <span data-ttu-id="14b07-107">L’exemple d’application montre comment utiliser le nouveau moteur d’affichage Razor avec ASP.NET MVC version 3 et Visual Studio 2010 pour créer un site Web liste utilisateur fictif qui inclut des fonctionnalités telles que la création, affichage, modification et suppression d’utilisateurs.</span><span class="sxs-lookup"><span data-stu-id="14b07-107">The sample application shows how to use the new Razor view engine with ASP.NET MVC version 3 and Visual Studio 2010 to create a fictional User List website that includes functionality such as creating, displaying, editing, and deleting users.</span></span>
> 
> <span data-ttu-id="14b07-108">Ce didacticiel décrit les étapes qui ont été prises pour générer la liste des utilisateurs exemple ASP.NET MVC 3 d’application.</span><span class="sxs-lookup"><span data-stu-id="14b07-108">This tutorial describes the steps that were taken in order to build the User List sample ASP.NET MVC 3 application.</span></span> <span data-ttu-id="14b07-109">Un projet Visual Studio avec code source c# et VB est disponible pour accompagner cette rubrique : [télécharger](https://code.msdn.microsoft.com/aspnetmvcsamples/Release/ProjectReleases.aspx?ReleaseId=5114).</span><span class="sxs-lookup"><span data-stu-id="14b07-109">A Visual Studio project with C# and VB source code is available to accompany this topic: [Download](https://code.msdn.microsoft.com/aspnetmvcsamples/Release/ProjectReleases.aspx?ReleaseId=5114).</span></span> <span data-ttu-id="14b07-110">Si vous avez des questions à propos de ce didacticiel, veuillez les valider pour le [forum MVC](https://forums.asp.net/1146.aspx).</span><span class="sxs-lookup"><span data-stu-id="14b07-110">If you have questions about this tutorial, please post them to the [MVC forum](https://forums.asp.net/1146.aspx).</span></span>


## <a name="overview"></a><span data-ttu-id="14b07-111">Vue d'ensemble</span><span class="sxs-lookup"><span data-stu-id="14b07-111">Overview</span></span>

<span data-ttu-id="14b07-112">L’application que vous créez est un site Web de liste utilisateur simple.</span><span class="sxs-lookup"><span data-stu-id="14b07-112">The application you'll be building is a simple user list website.</span></span> <span data-ttu-id="14b07-113">Les utilisateurs peuvent entrer, afficher et mettre à jour les informations utilisateur.</span><span class="sxs-lookup"><span data-stu-id="14b07-113">Users can enter, view, and update user information.</span></span>

![Exemple de site](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image1.png)

<span data-ttu-id="14b07-115">Vous pouvez télécharger le projet achevé VB et c# [ici](https://code.msdn.microsoft.com/Creating-a-MVC-3-28883c0f).</span><span class="sxs-lookup"><span data-stu-id="14b07-115">You can download the VB and C# completed project [here](https://code.msdn.microsoft.com/Creating-a-MVC-3-28883c0f).</span></span>

## <a name="creating-the-web-application"></a><span data-ttu-id="14b07-116">Création de l’Application Web</span><span class="sxs-lookup"><span data-stu-id="14b07-116">Creating the Web Application</span></span>

<span data-ttu-id="14b07-117">Pour démarrer le didacticiel, ouvrez Visual Studio 2010 et créer un projet à l’aide du *Application Web de ASP.NET MVC 3* modèle.</span><span class="sxs-lookup"><span data-stu-id="14b07-117">To start the tutorial, open Visual Studio 2010 and create a new project using the *ASP.NET MVC 3 Web Application* template.</span></span> <span data-ttu-id="14b07-118">Nommez l’application &quot;Mvc3Razor&quot;.</span><span class="sxs-lookup"><span data-stu-id="14b07-118">Name the application &quot;Mvc3Razor&quot;.</span></span>

<span data-ttu-id="14b07-119">[![Nouveau projet MVC 3](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image3.png)](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image2.png)</span><span class="sxs-lookup"><span data-stu-id="14b07-119">[![New MVC 3 project](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image3.png)](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image2.png)</span></span>

<span data-ttu-id="14b07-120">Dans le **nouveau projet ASP.NET MVC 3** boîte de dialogue, sélectionnez **Application Internet**, sélectionnez le moteur d’affichage Razor, puis cliquez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="14b07-120">In the **New ASP.NET MVC 3 Project** dialog, select **Internet Application**, select the Razor view engine, and then click **OK**.</span></span>

![Boîte de dialogue Nouveau projet ASP.NET MVC 3](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image4.png)

<span data-ttu-id="14b07-122">Dans ce didacticiel vous ne serez pas utiliser le fournisseur d’appartenances ASP.NET, donc vous pouvez supprimer tous les fichiers associés d’ouverture de session et d’appartenance.</span><span class="sxs-lookup"><span data-stu-id="14b07-122">In this tutorial you will not be using the ASP.NET membership provider, so you can delete all the files associated with logon and membership.</span></span> <span data-ttu-id="14b07-123">Dans **l’Explorateur de solutions**, supprimez les fichiers et répertoires suivants :</span><span class="sxs-lookup"><span data-stu-id="14b07-123">In **Solution Explorer**, remove the following files and directories:</span></span>

- <span data-ttu-id="14b07-124">*Controllers\AccountController*</span><span class="sxs-lookup"><span data-stu-id="14b07-124">*Controllers\AccountController*</span></span>
- <span data-ttu-id="14b07-125">*Models\AccountModels*</span><span class="sxs-lookup"><span data-stu-id="14b07-125">*Models\AccountModels*</span></span>
- <span data-ttu-id="14b07-126">*Views\Shared\\_LogOnPartial*</span><span class="sxs-lookup"><span data-stu-id="14b07-126">*Views\Shared\\_LogOnPartial*</span></span>
- <span data-ttu-id="14b07-127">*Views\Account* (et tous les fichiers dans ce répertoire)</span><span class="sxs-lookup"><span data-stu-id="14b07-127">*Views\Account* (and all the files in this directory)</span></span>

![Soln Exp](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image5.png)

<span data-ttu-id="14b07-129">Modifier le  <em>\_Layout.cshtml</em> fichier et remplacez le balisage à l’intérieur de la `<div>` élément nommé `logindisplay` avec le message <em>&quot;</em>connexion désactivé&quot;.</span><span class="sxs-lookup"><span data-stu-id="14b07-129">Edit the <em>\_Layout.cshtml</em> file and replace the markup inside the `<div>` element named `logindisplay` with the message <em>&quot;</em>Login Disabled&quot;.</span></span> <span data-ttu-id="14b07-130">L’exemple suivant montre le balisage :</span><span class="sxs-lookup"><span data-stu-id="14b07-130">The following example shows the new markup:</span></span>

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample1.cshtml)]

## <a name="adding-the-model"></a><span data-ttu-id="14b07-131">Ajout du modèle</span><span class="sxs-lookup"><span data-stu-id="14b07-131">Adding the Model</span></span>

<span data-ttu-id="14b07-132">Dans **l’Explorateur de solutions**, avec le bouton droit le *modèles* dossier, sélectionnez **ajouter**, puis cliquez sur **classe**.</span><span class="sxs-lookup"><span data-stu-id="14b07-132">In **Solution Explorer**, right-click the *Models* folder, select **Add**, and then click **Class**.</span></span>

![Nouvelle classe utilisateur Mdl](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image6.png)

<span data-ttu-id="14b07-134">Nommez la classe `UserModel`.</span><span class="sxs-lookup"><span data-stu-id="14b07-134">Name the class `UserModel`.</span></span> <span data-ttu-id="14b07-135">Remplacez le contenu de la *UserModel* fichier par le code suivant :</span><span class="sxs-lookup"><span data-stu-id="14b07-135">Replace the contents of the *UserModel* file with the following code:</span></span>

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample2.cs)]

<span data-ttu-id="14b07-136">Le `UserModel` classe représente les utilisateurs.</span><span class="sxs-lookup"><span data-stu-id="14b07-136">The `UserModel` class represents users.</span></span> <span data-ttu-id="14b07-137">Chaque membre de la classe est annoté avec le [requis](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx) attribut à partir de la [DataAnnotations](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) espace de noms.</span><span class="sxs-lookup"><span data-stu-id="14b07-137">Each member of the class is annotated with the [Required](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx) attribute from the [DataAnnotations](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) namespace.</span></span> <span data-ttu-id="14b07-138">Les attributs dans le [DataAnnotations](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) espace de noms fournissent une validation côté client et serveur automatique pour les applications web.</span><span class="sxs-lookup"><span data-stu-id="14b07-138">The attributes in the [DataAnnotations](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) namespace provide automatic client- and server-side validation for web applications.</span></span>

<span data-ttu-id="14b07-139">Ouvrir le `HomeController` classe et ajoutez un `using` directive afin que vous pouvez accéder à la `UserModel` et `Users` classes :</span><span class="sxs-lookup"><span data-stu-id="14b07-139">Open the `HomeController` class and add a `using` directive so that you can access the `UserModel` and `Users` classes:</span></span>

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample3.cs)]

<span data-ttu-id="14b07-140">Juste après le `HomeController` déclaration, ajoutez le commentaire suivant et la référence à un `Users` classe :</span><span class="sxs-lookup"><span data-stu-id="14b07-140">Just after the `HomeController` declaration, add the following comment and the reference to a `Users` class:</span></span>

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample4.cs)]

<span data-ttu-id="14b07-141">Le `Users` classe est un magasin de données simplifiée, en mémoire que vous utiliserez dans ce didacticiel.</span><span class="sxs-lookup"><span data-stu-id="14b07-141">The `Users` class is a simplified, in-memory data store that you'll use in this tutorial.</span></span> <span data-ttu-id="14b07-142">Dans une application réelle, vous utiliseriez une base de données pour stocker les informations utilisateur.</span><span class="sxs-lookup"><span data-stu-id="14b07-142">In a real application you would use a database to store user information.</span></span> <span data-ttu-id="14b07-143">Les premières lignes de la `HomeController` fichier sont affichés dans l’exemple suivant :</span><span class="sxs-lookup"><span data-stu-id="14b07-143">The first few lines of the `HomeController` file are shown in the following example:</span></span>

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample5.cs)]

<span data-ttu-id="14b07-144">Générez l’application afin que le modèle utilisateur sera disponible pour l’Assistant génération de modèles automatique à l’étape suivante.</span><span class="sxs-lookup"><span data-stu-id="14b07-144">Build the application so that the user model will be available to the scaffolding wizard in the next step.</span></span>

## <a name="creating-the-default-view"></a><span data-ttu-id="14b07-145">Création de la vue par défaut</span><span class="sxs-lookup"><span data-stu-id="14b07-145">Creating the Default View</span></span>

<span data-ttu-id="14b07-146">L’étape suivante consiste à ajouter une méthode d’action et pour afficher les utilisateurs.</span><span class="sxs-lookup"><span data-stu-id="14b07-146">The next step is to add an action method and view to display the users.</span></span>

<span data-ttu-id="14b07-147">Supprimez le *Views\Home\Index* fichier.</span><span class="sxs-lookup"><span data-stu-id="14b07-147">Delete the existing *Views\Home\Index* file.</span></span> <span data-ttu-id="14b07-148">Vous allez créer un nouveau *Index* fichier pour afficher les utilisateurs.</span><span class="sxs-lookup"><span data-stu-id="14b07-148">You will create a new *Index* file to display the users.</span></span>

<span data-ttu-id="14b07-149">Dans le `HomeController` de classe, remplacez le contenu de la `Index` méthode avec le code suivant :</span><span class="sxs-lookup"><span data-stu-id="14b07-149">In the `HomeController` class, replace the contents of the `Index` method with the following code:</span></span>

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample6.cs)]

<span data-ttu-id="14b07-150">Avec le bouton droit à l’intérieur de la `Index` (méthode), puis cliquez sur **ajouter une vue**.</span><span class="sxs-lookup"><span data-stu-id="14b07-150">Right-click inside the `Index` method and then click **Add View**.</span></span>

![Ajouter une vue](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image7.png)

<span data-ttu-id="14b07-152">Sélectionnez le **créer une vue fortement typée** option.</span><span class="sxs-lookup"><span data-stu-id="14b07-152">Select the **Create a strongly-typed view** option.</span></span> <span data-ttu-id="14b07-153">Pour **afficher la classe de données**, sélectionnez **Mvc3Razor.Models.UserModel**.</span><span class="sxs-lookup"><span data-stu-id="14b07-153">For **View data class**, select **Mvc3Razor.Models.UserModel**.</span></span> <span data-ttu-id="14b07-154">(Si vous ne voyez pas **Mvc3Razor.Models.UserModel** dans le **afficher la classe de données** boîte, vous avez besoin générer le projet.) Assurez-vous que le moteur d’affichage est défini sur **Razor**.</span><span class="sxs-lookup"><span data-stu-id="14b07-154">(If you don't see **Mvc3Razor.Models.UserModel** in the **View data class** box, you need to build the project.) Make sure that the view engine is set to **Razor**.</span></span> <span data-ttu-id="14b07-155">Définissez **afficher le contenu** à **liste** puis cliquez sur **ajouter**.</span><span class="sxs-lookup"><span data-stu-id="14b07-155">Set **View content** to **List** and then click **Add**.</span></span>

![Ajouter une vue d’Index](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image8.png)

<span data-ttu-id="14b07-157">La nouvelle vue de structure automatiquement les données utilisateur qui sont passées à la `Index` vue.</span><span class="sxs-lookup"><span data-stu-id="14b07-157">The new view automatically scaffolds the user data that's passed to the `Index` view.</span></span> <span data-ttu-id="14b07-158">Examiner le nouvellement généré *Views\Home\Index* fichier.</span><span class="sxs-lookup"><span data-stu-id="14b07-158">Examine the newly generated *Views\Home\Index* file.</span></span> <span data-ttu-id="14b07-159">Le **créer un nouveau**, **modifier**, **détails**, et **supprimer** liens ne fonctionnent pas, mais le reste de la page est fonctionnel.</span><span class="sxs-lookup"><span data-stu-id="14b07-159">The **Create New**, **Edit**, **Details**, and **Delete** links don't work, but the rest of the page is functional.</span></span> <span data-ttu-id="14b07-160">Exécutez la page.</span><span class="sxs-lookup"><span data-stu-id="14b07-160">Run the page.</span></span> <span data-ttu-id="14b07-161">Vous voyez une liste d’utilisateurs.</span><span class="sxs-lookup"><span data-stu-id="14b07-161">You see a list of users.</span></span>

![Page d’index](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image9.png)

<span data-ttu-id="14b07-163">Ouvrez le *Index.cshtml* fichier et remplacez le `ActionLink` balisage pour **modifier**, **détails**, et **supprimer** par le code suivant :</span><span class="sxs-lookup"><span data-stu-id="14b07-163">Open the *Index.cshtml* file and replace the `ActionLink` markup for **Edit**, **Details**, and **Delete** with the following code:</span></span>

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample7.cshtml)]

<span data-ttu-id="14b07-164">Le nom d’utilisateur est utilisé comme ID pour rechercher l’enregistrement sélectionné dans le **modifier**, **détails**, et **supprimer** des liens.</span><span class="sxs-lookup"><span data-stu-id="14b07-164">The user name is used as the ID to find the selected record in the **Edit**, **Details**, and **Delete** links.</span></span>

## <a name="creating-the-details-view"></a><span data-ttu-id="14b07-165">Création de la vue Détails</span><span class="sxs-lookup"><span data-stu-id="14b07-165">Creating the Details View</span></span>

<span data-ttu-id="14b07-166">L’étape suivante consiste à ajouter un `Details` méthode d’action et de la vue afin d’afficher les détails de l’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="14b07-166">The next step is to add a `Details` action method and view in order to display user details.</span></span>

![Détails](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image10.png)

<span data-ttu-id="14b07-168">Ajoutez le code suivant `Details` méthode au contrôleur home :</span><span class="sxs-lookup"><span data-stu-id="14b07-168">Add the following `Details` method to the home controller:</span></span>

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample8.cs)]

<span data-ttu-id="14b07-169">Avec le bouton droit à l’intérieur de la `Details` (méthode), puis sélectionnez <strong>ajouter une vue</strong>.</span><span class="sxs-lookup"><span data-stu-id="14b07-169">Right-click inside the `Details` method and then select <strong>Add View</strong>.</span></span> <span data-ttu-id="14b07-170">Vérifiez que le <strong>afficher la classe de données</strong> boîte contient <strong>Mvc3Razor.Models.UserModel</strong><em>.</em></span><span class="sxs-lookup"><span data-stu-id="14b07-170">Verify that the <strong>View data class</strong> box contains <strong>Mvc3Razor.Models.UserModel</strong><em>.</em></span></span> <span data-ttu-id="14b07-171">Définissez <strong>afficher le contenu</strong> à <strong>détails</strong> puis cliquez sur <strong>ajouter</strong>.</span><span class="sxs-lookup"><span data-stu-id="14b07-171">Set <strong>View content</strong> to <strong>Details</strong> and then click <strong>Add</strong>.</span></span>

![Ajouter une vue de détails](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image11.png)

<span data-ttu-id="14b07-173">Exécutez l’application et sélectionnez un lien Détails.</span><span class="sxs-lookup"><span data-stu-id="14b07-173">Run the application and select a details link.</span></span> <span data-ttu-id="14b07-174">La génération de modèles automatique montre chaque propriété dans le modèle.</span><span class="sxs-lookup"><span data-stu-id="14b07-174">The automatic scaffolding shows each property in the model.</span></span>

![Détails](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image12.png)

## <a name="creating-the-edit-view"></a><span data-ttu-id="14b07-176">Création de la vue de modification</span><span class="sxs-lookup"><span data-stu-id="14b07-176">Creating the Edit View</span></span>

<span data-ttu-id="14b07-177">Ajoutez le code suivant `Edit` méthode au contrôleur home.</span><span class="sxs-lookup"><span data-stu-id="14b07-177">Add the following `Edit` method to the home controller.</span></span>

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample9.cs)]

<span data-ttu-id="14b07-178">Ajouter une vue comme dans les étapes précédentes, mais définissez **afficher le contenu** à **modifier**.</span><span class="sxs-lookup"><span data-stu-id="14b07-178">Add a view as in the previous steps, but set **View content** to **Edit**.</span></span>

![Ajouter une vue de modification](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image13.png)

<span data-ttu-id="14b07-180">Exécutez l’application et modifier le prénom et le nom d’un des utilisateurs.</span><span class="sxs-lookup"><span data-stu-id="14b07-180">Run the application and edit the first and last name of one of the users.</span></span> <span data-ttu-id="14b07-181">Si vous violez l’une `DataAnnotation` contraintes qui ont été appliqués à la `UserModel` (classe), lorsque vous envoyez le formulaire, vous verrez des erreurs de validation qui se sont produites par le code serveur.</span><span class="sxs-lookup"><span data-stu-id="14b07-181">If you violate any `DataAnnotation` constraints that have been applied to the `UserModel` class, when you submit the form, you will see validation errors that are produced by server code.</span></span> <span data-ttu-id="14b07-182">Par exemple, si vous modifiez le nom de la première &quot;Ann&quot; à &quot;A&quot;, lorsque vous envoyez le formulaire, l’erreur suivante s’affiche sur le formulaire :</span><span class="sxs-lookup"><span data-stu-id="14b07-182">For example, if you change the first name &quot;Ann&quot; to &quot;A&quot;, when you submit the form, the following error is displayed on the form:</span></span>

`The field First Name must be a string with a minimum length of 3 and a maximum length of 8.`

<span data-ttu-id="14b07-183">Dans ce didacticiel, vous êtes en traitant le nom d’utilisateur comme clé primaire.</span><span class="sxs-lookup"><span data-stu-id="14b07-183">In this tutorial, you're treating the user name as the primary key.</span></span> <span data-ttu-id="14b07-184">Par conséquent, la propriété de nom d’utilisateur ne peut pas être modifiée.</span><span class="sxs-lookup"><span data-stu-id="14b07-184">Therefore, the user name property cannot be changed.</span></span> <span data-ttu-id="14b07-185">Dans le *Edit.cshtml* de fichiers, juste après la `Html.BeginForm` instruction, définissez le nom d’utilisateur doit être un champ masqué.</span><span class="sxs-lookup"><span data-stu-id="14b07-185">In the *Edit.cshtml* file, just after the `Html.BeginForm` statement, set the user name to be a hidden field.</span></span> <span data-ttu-id="14b07-186">Cela entraîne la propriété devant être transmis dans le modèle.</span><span class="sxs-lookup"><span data-stu-id="14b07-186">This causes the property to be passed in the model.</span></span> <span data-ttu-id="14b07-187">Le fragment de code suivant montre le placement de la `Hidden` instruction :</span><span class="sxs-lookup"><span data-stu-id="14b07-187">The following code fragment shows the placement of the `Hidden` statement:</span></span>

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample10.cshtml)]

<span data-ttu-id="14b07-188">Remplacez le `TextBoxFor` et `ValidationMessageFor` balisage pour le nom d’utilisateur avec un `DisplayFor` appeler.</span><span class="sxs-lookup"><span data-stu-id="14b07-188">Replace the `TextBoxFor` and `ValidationMessageFor` markup for the user name with a `DisplayFor` call.</span></span> <span data-ttu-id="14b07-189">Le `DisplayFor` méthode affiche la propriété sous la forme d’un élément en lecture seule.</span><span class="sxs-lookup"><span data-stu-id="14b07-189">The `DisplayFor` method displays the property as a read-only element.</span></span> <span data-ttu-id="14b07-190">L’exemple suivant montre le balisage complet.</span><span class="sxs-lookup"><span data-stu-id="14b07-190">The following example shows the completed markup.</span></span> <span data-ttu-id="14b07-191">La version d’origine `TextBoxFor` et `ValidationMessageFor` appels sont mis en commentaire avec les caractères de commentaire begin et end-commentaire Razor (`@* *@`)</span><span class="sxs-lookup"><span data-stu-id="14b07-191">The original `TextBoxFor` and `ValidationMessageFor` calls are commented out with the Razor begin-comment and end-comment characters (`@* *@`)</span></span>

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample11.cshtml)]

## <a name="enabling-client-side-validation"></a><span data-ttu-id="14b07-192">L’activation de la Validation côté Client</span><span class="sxs-lookup"><span data-stu-id="14b07-192">Enabling Client-Side Validation</span></span>

<span data-ttu-id="14b07-193">Pour activer la validation côté client dans ASP.NET MVC 3, vous devez définir deux indicateurs, et vous devez inclure les trois fichiers JavaScript.</span><span class="sxs-lookup"><span data-stu-id="14b07-193">To enable client-side validation in ASP.NET MVC 3, you must set two flags and you must include three JavaScript files.</span></span>

<span data-ttu-id="14b07-194">Ouvrez l’application *Web.config* fichier.</span><span class="sxs-lookup"><span data-stu-id="14b07-194">Open the application's *Web.config* file.</span></span> <span data-ttu-id="14b07-195">Vérifiez `that ClientValidationEnabled` et `UnobtrusiveJavaScriptEnabled` sont définies sur true dans les paramètres d’application.</span><span class="sxs-lookup"><span data-stu-id="14b07-195">Verify `that ClientValidationEnabled` and `UnobtrusiveJavaScriptEnabled` are set to true in the application settings.</span></span> <span data-ttu-id="14b07-196">Le fragment suivant à partir de la racine *Web.config* fichier montre les paramètres appropriés :</span><span class="sxs-lookup"><span data-stu-id="14b07-196">The following fragment from the root *Web.config* file shows the correct settings:</span></span>

[!code-xml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample12.xml)]

<span data-ttu-id="14b07-197">Paramètre `UnobtrusiveJavaScriptEnabled` sur vrai Ajax discrète active et validation client non obstructive.</span><span class="sxs-lookup"><span data-stu-id="14b07-197">Setting `UnobtrusiveJavaScriptEnabled` to true enables unobtrusive Ajax and unobtrusive client validation.</span></span> <span data-ttu-id="14b07-198">Lorsque vous utilisez la validation non obstrusive, les règles de validation sont transformées en attributs HTML5.</span><span class="sxs-lookup"><span data-stu-id="14b07-198">When you use unobtrusive validation, the validation rules are turned into HTML5 attributes.</span></span> <span data-ttu-id="14b07-199">Noms d’attributs HTML5 peuvent contenir uniquement des lettres minuscules, chiffres et des tirets.</span><span class="sxs-lookup"><span data-stu-id="14b07-199">HTML5 attribute names can consist of only lowercase letters, numbers, and dashes.</span></span>

<span data-ttu-id="14b07-200">Paramètre `ClientValidationEnabled` à la valeur true permet la validation côté client.</span><span class="sxs-lookup"><span data-stu-id="14b07-200">Setting `ClientValidationEnabled` to true enables client-side validation.</span></span> <span data-ttu-id="14b07-201">En définissant ces clés dans l’application *Web.config* fichier, vous activez la validation côté client et JavaScript non obstrusif pour l’application entière.</span><span class="sxs-lookup"><span data-stu-id="14b07-201">By setting these keys in the application *Web.config* file, you enable client validation and unobtrusive JavaScript for the entire application.</span></span> <span data-ttu-id="14b07-202">Vous pouvez également activer ou désactiver ces paramètres dans les vues individuelles ou les méthodes de contrôleur en utilisant le code suivant :</span><span class="sxs-lookup"><span data-stu-id="14b07-202">You can also enable or disable these settings in individual views or in controller methods using the following code:</span></span>

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample13.cs)]

<span data-ttu-id="14b07-203">Vous devez également inclure plusieurs fichiers JavaScript dans l’affichage.</span><span class="sxs-lookup"><span data-stu-id="14b07-203">You also need to include several JavaScript files in the rendered view.</span></span> <span data-ttu-id="14b07-204">Un moyen simple d’inclure le code JavaScript dans toutes les vues consiste à les ajouter à la *Views\Shared\\_Layout.cshtml* fichier.</span><span class="sxs-lookup"><span data-stu-id="14b07-204">An easy way to include the JavaScript in all views is to add them to the *Views\Shared\\_Layout.cshtml* file.</span></span> <span data-ttu-id="14b07-205">Remplacez le `<head>` élément de la  *\_Layout.cshtml* fichier par le code suivant :</span><span class="sxs-lookup"><span data-stu-id="14b07-205">Replace the `<head>` element of the *\_Layout.cshtml* file with the following code:</span></span>

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample14.cshtml)]

<span data-ttu-id="14b07-206">Les deux premiers scripts jQuery sont hébergés par le Microsoft Ajax Content Delivery Network (CDN).</span><span class="sxs-lookup"><span data-stu-id="14b07-206">The first two jQuery scripts are hosted by the Microsoft Ajax Content Delivery Network (CDN).</span></span> <span data-ttu-id="14b07-207">En tirant parti du CDN Microsoft Ajax, vous pouvez considérablement améliorer les performances du premier accès de vos applications.</span><span class="sxs-lookup"><span data-stu-id="14b07-207">By taking advantage of the Microsoft Ajax CDN, you can significantly improve the first-hit performance of your applications.</span></span>

<span data-ttu-id="14b07-208">Exécutez l’application et cliquez sur un lien de modification.</span><span class="sxs-lookup"><span data-stu-id="14b07-208">Run the application and click an edit link.</span></span> <span data-ttu-id="14b07-209">Afficher le code source dans le navigateur.</span><span class="sxs-lookup"><span data-stu-id="14b07-209">View the page's source in the browser.</span></span> <span data-ttu-id="14b07-210">La source du navigateur montre beaucoup d’attributs sous la forme `data-val` (pour la validation de données).</span><span class="sxs-lookup"><span data-stu-id="14b07-210">The browser source shows many attributes of the form `data-val` (for data validation).</span></span> <span data-ttu-id="14b07-211">Lors de la validation côté client et le code JavaScript non obstrusif est activé, les champs d’entrée avec une règle de validation côté client contiennent le `data-val="true"` attribut pour déclencher la validation client non obstructive.</span><span class="sxs-lookup"><span data-stu-id="14b07-211">When client validation and unobtrusive JavaScript is enabled, input fields with a client-validation rule contain the `data-val="true"` attribute to trigger unobtrusive client validation.</span></span> <span data-ttu-id="14b07-212">Par exemple, le `City` champ dans le modèle a été décorée avec le [requis](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx) attribut, ce qui se traduit dans le code HTML indiqué dans l’exemple suivant :</span><span class="sxs-lookup"><span data-stu-id="14b07-212">For example, the `City` field in the model was decorated with the [Required](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx) attribute, which results in the HTML shown in the following example:</span></span>

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample15.cshtml)]

<span data-ttu-id="14b07-213">Pour chaque règle de validation côté client, un attribut est ajouté qui présente sous la forme `data-val-rulename="message"`.</span><span class="sxs-lookup"><span data-stu-id="14b07-213">For each client-validation rule, an attribute is added that has the form `data-val-rulename="message"`.</span></span> <span data-ttu-id="14b07-214">À l’aide de la `City` champ exemple montré précédemment, la règle de validation côté client nécessaire génère le `data-val-required` attribut et le message &quot;champ de la ville est obligatoire&quot;.</span><span class="sxs-lookup"><span data-stu-id="14b07-214">Using the `City` field example shown earlier, the required client-validation rule generates the `data-val-required` attribute and the message &quot;The City field is required&quot;.</span></span> <span data-ttu-id="14b07-215">Exécutez l’application, modifiez un des utilisateurs et effacer le `City` champ.</span><span class="sxs-lookup"><span data-stu-id="14b07-215">Run the application, edit one of the users, and clear the `City` field.</span></span> <span data-ttu-id="14b07-216">Si vous le déplacez hors du champ, vous voyez un message d’erreur de validation côté client.</span><span class="sxs-lookup"><span data-stu-id="14b07-216">When you tab out of the field, you see a client-side validation error message.</span></span>

![Ville requis](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image14.png)

<span data-ttu-id="14b07-218">De même, pour chaque paramètre dans la règle de validation côté client, un attribut est ajouté qui présente sous la forme `data-val-rulename-paramname=paramvalue`.</span><span class="sxs-lookup"><span data-stu-id="14b07-218">Similarly, for each parameter in the client-validation rule, an attribute is added that has the form `data-val-rulename-paramname=paramvalue`.</span></span> <span data-ttu-id="14b07-219">Par exemple, le `FirstName` propriété est annotée avec le [StringLength](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx) d’attribut et spécifie une longueur minimale de 3 et une longueur maximale de 8.</span><span class="sxs-lookup"><span data-stu-id="14b07-219">For example, the `FirstName` property is annotated with the [StringLength](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx) attribute and specifies a minimum length of 3 and a maximum length of 8.</span></span> <span data-ttu-id="14b07-220">La règle de validation de données nommée `length` porte le nom de paramètre `max` et la valeur du paramètre 8.</span><span class="sxs-lookup"><span data-stu-id="14b07-220">The data validation rule named `length` has the parameter name `max` and the parameter value 8.</span></span> <span data-ttu-id="14b07-221">L’exemple suivant montre le code HTML généré pour le `FirstName` champ lorsque vous modifiez un des utilisateurs :</span><span class="sxs-lookup"><span data-stu-id="14b07-221">The following shows the HTML that is generated for the `FirstName` field when you edit one of the users:</span></span>

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample16.cshtml)]

<span data-ttu-id="14b07-222">Pour plus d’informations sur la validation client non obstructive, consultez l’entrée [Unobtrusive validation côté Client dans ASP.NET MVC 3](http://bradwilson.typepad.com/blog/2010/10/mvc3-unobtrusive-validation.html) dans le blog de Brad Wilson.</span><span class="sxs-lookup"><span data-stu-id="14b07-222">For more information about unobtrusive client validation, see the entry [Unobtrusive Client Validation in ASP.NET MVC 3](http://bradwilson.typepad.com/blog/2010/10/mvc3-unobtrusive-validation.html) in Brad Wilson's blog.</span></span>

> [!NOTE]
> <span data-ttu-id="14b07-223">Dans la version bêta d’ASP.NET MVC 3, vous avez parfois besoin envoyer le formulaire afin de démarrer la validation côté client.</span><span class="sxs-lookup"><span data-stu-id="14b07-223">In ASP.NET MVC 3 Beta, you sometimes need to submit the form in order to start client-side validation.</span></span> <span data-ttu-id="14b07-224">Cela peut être modifié pour la version finale.</span><span class="sxs-lookup"><span data-stu-id="14b07-224">This might be changed for the final release.</span></span>


## <a name="creating-the-create-view"></a><span data-ttu-id="14b07-225">Création de la vue Create</span><span class="sxs-lookup"><span data-stu-id="14b07-225">Creating the Create View</span></span>

<span data-ttu-id="14b07-226">L’étape suivante consiste à ajouter un `Create` méthode d’action et de la vue afin de permettre à l’utilisateur créer un nouvel utilisateur.</span><span class="sxs-lookup"><span data-stu-id="14b07-226">The next step is to add a `Create` action method and view in order to enable the user to create a new user.</span></span> <span data-ttu-id="14b07-227">Ajoutez le code suivant `Create` méthode au contrôleur home :</span><span class="sxs-lookup"><span data-stu-id="14b07-227">Add the following `Create` method to the home controller:</span></span>

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample17.cs)]

<span data-ttu-id="14b07-228">Ajouter une vue comme dans les étapes précédentes, mais définissez **afficher le contenu** à **créer**.</span><span class="sxs-lookup"><span data-stu-id="14b07-228">Add a view as in the previous steps, but set **View content** to **Create**.</span></span>

![Créer une vue](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image15.png)

<span data-ttu-id="14b07-230">Exécutez l’application, sélectionnez le **créer** lier, puis ajoutez un nouvel utilisateur.</span><span class="sxs-lookup"><span data-stu-id="14b07-230">Run the application, select the **Create** link, and add a new user.</span></span> <span data-ttu-id="14b07-231">Le `Create` méthode tire automatiquement parti de validation côté client et côté serveur.</span><span class="sxs-lookup"><span data-stu-id="14b07-231">The `Create` method automatically takes advantage of client-side and server-side validation.</span></span> <span data-ttu-id="14b07-232">Essayez d’entrer un nom d’utilisateur qui contient un espace blanc, tel que &quot;Ben X&quot;.</span><span class="sxs-lookup"><span data-stu-id="14b07-232">Try to enter a user name that contains white space, such as &quot;Ben X&quot;.</span></span> <span data-ttu-id="14b07-233">Lorsque vous quittez le champ nom d’utilisateur, une erreur de validation côté client (`White space is not allowed`) s’affiche.</span><span class="sxs-lookup"><span data-stu-id="14b07-233">When you tab out of the user name field, a client-side validation error (`White space is not allowed`) is displayed.</span></span>

## <a name="add-the-delete-method"></a><span data-ttu-id="14b07-234">Ajoutez la méthode Delete</span><span class="sxs-lookup"><span data-stu-id="14b07-234">Add the Delete method</span></span>

<span data-ttu-id="14b07-235">Pour suivre ce didacticiel, ajoutez le code suivant `Delete` méthode au contrôleur home :</span><span class="sxs-lookup"><span data-stu-id="14b07-235">To complete the tutorial, add the following `Delete` method to the home controller:</span></span>

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample18.cs)]

<span data-ttu-id="14b07-236">Ajouter un `Delete` vue comme dans les étapes précédentes, en définissant **afficher le contenu** à **supprimer**.</span><span class="sxs-lookup"><span data-stu-id="14b07-236">Add a `Delete` view as in the previous steps, setting **View content** to **Delete**.</span></span>

![Supprimer la vue](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image16.png)

<span data-ttu-id="14b07-238">Vous disposez maintenant d’une application ASP.NET MVC 3 simple mais totalement opérationnelle avec la validation.</span><span class="sxs-lookup"><span data-stu-id="14b07-238">You now have a simple but fully functional ASP.NET MVC 3 application with validation.</span></span>
