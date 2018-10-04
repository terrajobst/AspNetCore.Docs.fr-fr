---
uid: mvc/overview/older-versions/working-with-the-dropdownlist-box-and-jquery/examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper
title: Examiner la façon dont ASP.NET MVC structure du programme d’assistance DropDownList | Microsoft Docs
author: Rick-Anderson
description: ''
ms.author: riande
ms.date: 01/12/2012
ms.assetid: 8921d7f2-21f0-427a-8b27-2df7251174b0
msc.legacyurl: /mvc/overview/older-versions/working-with-the-dropdownlist-box-and-jquery/examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper
msc.type: authoredcontent
ms.openlocfilehash: 542790b7f475cc641ed26ff3187c25c25118e0ed
ms.sourcegitcommit: 7b4e3936feacb1a8fcea7802aab3e2ea9c8af5b4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/04/2018
ms.locfileid: "48577845"
---
<a name="examining--how--aspnet-mvc-scaffolds-the-dropdownlist-helper"></a><span data-ttu-id="8a7ca-102">Examiner la façon dont ASP.NET MVC structure du programme d’assistance DropDownList</span><span class="sxs-lookup"><span data-stu-id="8a7ca-102">Examining  how  ASP.NET MVC scaffolds the DropDownList Helper</span></span>
====================
<span data-ttu-id="8a7ca-103">par [Rick Anderson]((https://twitter.com/RickAndMSFT))</span><span class="sxs-lookup"><span data-stu-id="8a7ca-103">by [Rick Anderson]((https://twitter.com/RickAndMSFT))</span></span>

<span data-ttu-id="8a7ca-104">Dans **l’Explorateur de solutions**, cliquez sur le *contrôleurs* dossier, puis sélectionnez **ajouter un contrôleur**.</span><span class="sxs-lookup"><span data-stu-id="8a7ca-104">In **Solution Explorer**, right-click the *Controllers* folder and then select **Add Controller**.</span></span> <span data-ttu-id="8a7ca-105">Nommez le contrôleur **StoreManagerController**.</span><span class="sxs-lookup"><span data-stu-id="8a7ca-105">Name the controller **StoreManagerController**.</span></span> <span data-ttu-id="8a7ca-106">Définir les options de la **ajouter un contrôleur** boîte de dialogue comme illustré dans l’image ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="8a7ca-106">Set the options for the **Add Controller** dialog as shown in the image below.</span></span>

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image1.png)

<span data-ttu-id="8a7ca-107">Modifier le *StoreManager\Index.cshtml* afficher et supprimer `AlbumArtUrl`.</span><span class="sxs-lookup"><span data-stu-id="8a7ca-107">Edit the *StoreManager\Index.cshtml* view and remove `AlbumArtUrl`.</span></span> <span data-ttu-id="8a7ca-108">Suppression `AlbumArtUrl` améliorer la lisibilité de la présentation.</span><span class="sxs-lookup"><span data-stu-id="8a7ca-108">Removing `AlbumArtUrl` will make the presentation more readable.</span></span> <span data-ttu-id="8a7ca-109">Le code terminé est indiqué ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="8a7ca-109">The completed code is shown below.</span></span>

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample1.cshtml)]

<span data-ttu-id="8a7ca-110">Ouvrez le *Controllers\StoreManagerController.cs* de fichiers et de trouver le `Index` (méthode).</span><span class="sxs-lookup"><span data-stu-id="8a7ca-110">Open the *Controllers\StoreManagerController.cs* file and find the `Index` method.</span></span> <span data-ttu-id="8a7ca-111">Ajouter le `OrderBy` afin que les albums sont triés par tarif.</span><span class="sxs-lookup"><span data-stu-id="8a7ca-111">Add the `OrderBy` clause so the albums will be sorted by price.</span></span> <span data-ttu-id="8a7ca-112">Le code complet est indiqué ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="8a7ca-112">The complete code is shown below.</span></span>

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample2.cs)]

<span data-ttu-id="8a7ca-113">Tri par prix rend plus facile de tester les modifications apportées à la base de données.</span><span class="sxs-lookup"><span data-stu-id="8a7ca-113">Sorting by price will make it easier to test changes to the database.</span></span> <span data-ttu-id="8a7ca-114">Lorsque vous testez le modifier et créez des méthodes, vous pouvez utiliser un peu onéreuse pour les données enregistrées seront affichent en premier.</span><span class="sxs-lookup"><span data-stu-id="8a7ca-114">When you are testing the edit and create methods, you can use a low price so the saved data will appear first.</span></span>

<span data-ttu-id="8a7ca-115">Ouvrez le *StoreManager\Edit.cshtml* fichier.</span><span class="sxs-lookup"><span data-stu-id="8a7ca-115">Open the *StoreManager\Edit.cshtml* file.</span></span> <span data-ttu-id="8a7ca-116">Ajoutez la ligne suivante juste après la balise de légende.</span><span class="sxs-lookup"><span data-stu-id="8a7ca-116">Add the following line just after the legend tag.</span></span>

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample3.cshtml)]

<span data-ttu-id="8a7ca-117">Le code suivant affiche le contexte de cette modification :</span><span class="sxs-lookup"><span data-stu-id="8a7ca-117">The following code shows the context of this change:</span></span>

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample4.cshtml)]

<span data-ttu-id="8a7ca-118">Le `AlbumId` est nécessaire pour apporter des modifications à un enregistrement de l’album.</span><span class="sxs-lookup"><span data-stu-id="8a7ca-118">The `AlbumId` is required to make changes to an album record.</span></span>

<span data-ttu-id="8a7ca-119">Appuyez sur CTRL+F5 pour exécuter l'application.</span><span class="sxs-lookup"><span data-stu-id="8a7ca-119">Press CTRL+F5 to run the application.</span></span> <span data-ttu-id="8a7ca-120">Sélectionnez cette option pour le **administrateur** lier, puis sélectionnez le **créer un nouveau** lien pour créer un nouvel album.</span><span class="sxs-lookup"><span data-stu-id="8a7ca-120">Select to the **Admin** link, then select the **Create New** link to create a new album.</span></span> <span data-ttu-id="8a7ca-121">Vérifiez que le informations concernant l’album a été enregistré.</span><span class="sxs-lookup"><span data-stu-id="8a7ca-121">Verify the album information was saved.</span></span> <span data-ttu-id="8a7ca-122">Modifier un album et vérifiez les modifications apportées sont conservées.</span><span class="sxs-lookup"><span data-stu-id="8a7ca-122">Edit an album and verify the changes you made are persisted.</span></span>

### <a name="the-album-schema"></a><span data-ttu-id="8a7ca-123">Le schéma d’Album</span><span class="sxs-lookup"><span data-stu-id="8a7ca-123">The Album Schema</span></span>

<span data-ttu-id="8a7ca-124">Le `StoreManager` contrôleur créé par le mécanisme de génération de modèles automatique MVC permet un accès CRUD (Create, Read, Update, Delete) aux albums dans la base de données du magasin de musique.</span><span class="sxs-lookup"><span data-stu-id="8a7ca-124">The `StoreManager` controller created by the MVC scaffolding mechanism allows CRUD (Create, Read, Update, Delete) access to the albums in the music store database.</span></span> <span data-ttu-id="8a7ca-125">Le schéma pour les informations concernant l’album est présenté ci-dessous :</span><span class="sxs-lookup"><span data-stu-id="8a7ca-125">The schema for album information is shown below:</span></span>

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image2.png)

<span data-ttu-id="8a7ca-126">Le `Albums` table ne stocke pas le genre d’album et la description, il stocke une clé étrangère vers le `Genres` table.</span><span class="sxs-lookup"><span data-stu-id="8a7ca-126">The `Albums` table does not store the album genre and description, it stores a foreign key to the `Genres` table.</span></span> <span data-ttu-id="8a7ca-127">Le `Genres` table contient le nom du genre et la description.</span><span class="sxs-lookup"><span data-stu-id="8a7ca-127">The `Genres` table contains the genre name and description.</span></span> <span data-ttu-id="8a7ca-128">De même, le `Albums` table ne contient pas le nom de l’album artistes, mais une clé étrangère vers le `Artists` table.</span><span class="sxs-lookup"><span data-stu-id="8a7ca-128">Likewise, the `Albums` table doesn't contain the album artists name, but a foreign key to the `Artists` table.</span></span> <span data-ttu-id="8a7ca-129">Le `Artists` table contient le nom de l’artiste.</span><span class="sxs-lookup"><span data-stu-id="8a7ca-129">The `Artists` table contains the artist's name.</span></span> <span data-ttu-id="8a7ca-130">Si vous examinez les données dans le `Albums` table, vous pouvez voir chaque ligne contient une clé étrangère à la `Genres` table et une clé étrangère vers le `Artists` table.</span><span class="sxs-lookup"><span data-stu-id="8a7ca-130">If you examine the data in the `Albums` table, you can see each row contains a foreign key to the `Genres` table and a foreign key to the `Artists` table.</span></span> <span data-ttu-id="8a7ca-131">L’image ci-dessous affiche des données de table à partir de la `Albums` table.</span><span class="sxs-lookup"><span data-stu-id="8a7ca-131">The image below show some table data from the `Albums` table.</span></span>

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image3.png)

### <a name="the-html-select-tag"></a><span data-ttu-id="8a7ca-132">La balise de sélection HTML</span><span class="sxs-lookup"><span data-stu-id="8a7ca-132">The HTML Select Tag</span></span>

<span data-ttu-id="8a7ca-133">Le code HTML `<select>` élément (créé par le code HTML [DropDownList](https://msdn.microsoft.com/library/dd492948.aspx) helper) est utilisé pour afficher une liste complète des valeurs (par exemple, la liste des genres).</span><span class="sxs-lookup"><span data-stu-id="8a7ca-133">The HTML `<select>` element (created by the HTML [DropDownList](https://msdn.microsoft.com/library/dd492948.aspx) helper) is used to display a complete list of values (such as the list of genres).</span></span> <span data-ttu-id="8a7ca-134">Pour les formulaires de modification, lorsque la valeur actuelle est connue, la liste de sélection peut afficher la valeur actuelle.</span><span class="sxs-lookup"><span data-stu-id="8a7ca-134">For edit forms, when the current value is known, the select list can display the current value.</span></span> <span data-ttu-id="8a7ca-135">Nous l’avons vu ce précédemment lorsque nous définissons la valeur sélectionnée **comédie**.</span><span class="sxs-lookup"><span data-stu-id="8a7ca-135">We saw this previously when we set the selected value to **Comedy**.</span></span> <span data-ttu-id="8a7ca-136">La liste de sélection est idéale pour l’affichage des catégories ou des données de clé étrangère.</span><span class="sxs-lookup"><span data-stu-id="8a7ca-136">The select list is ideal for displaying category or foreign key data.</span></span> <span data-ttu-id="8a7ca-137">Le `<select>` élément pour la clé étrangère Genre affiche la liste des noms de genre possible, mais lorsque vous enregistrez le formulaire la propriété Genre est mis à jour avec la valeur de clé étrangère Genre, pas le nom du genre affichée.</span><span class="sxs-lookup"><span data-stu-id="8a7ca-137">The `<select>` element for the Genre foreign key displays the list of possible genre names, but when you save the form the Genre property is updated with the Genre foreign key value, not the displayed genre name.</span></span> <span data-ttu-id="8a7ca-138">Dans l’image ci-dessous, le genre sélectionné est **Disco** et est l’artiste **Donna Summer**.</span><span class="sxs-lookup"><span data-stu-id="8a7ca-138">In the image below, the genre selected is **Disco** and the artist is **Donna Summer**.</span></span>

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image4.png)

### <a name="examining-the-aspnet-mvc-scaffolded-code"></a><span data-ttu-id="8a7ca-139">Examen d’ASP.NET MVC structurée de Code</span><span class="sxs-lookup"><span data-stu-id="8a7ca-139">Examining the ASP.NET MVC Scaffolded Code</span></span>

<span data-ttu-id="8a7ca-140">Ouvrez le *Controllers\StoreManagerController.cs* de fichiers et de trouver le `HTTP GET Create` (méthode).</span><span class="sxs-lookup"><span data-stu-id="8a7ca-140">Open the *Controllers\StoreManagerController.cs* file and find the `HTTP GET Create` method.</span></span>

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample5.cs)]

<span data-ttu-id="8a7ca-141">Le `Create` méthode ajoute deux [SelectList](https://msdn.microsoft.com/library/system.web.mvc.selectlist.aspx) des objets sur le `ViewBag`, un pour contenir les informations de genre et un pour contenir les informations de l’artiste.</span><span class="sxs-lookup"><span data-stu-id="8a7ca-141">The `Create` method adds two [SelectList](https://msdn.microsoft.com/library/system.web.mvc.selectlist.aspx) objects to the `ViewBag`, one to contain the genre information, and one to contain the artist information.</span></span> <span data-ttu-id="8a7ca-142">Le [SelectList](https://msdn.microsoft.com/library/dd505286.aspx) surcharge de constructeur ci-dessus prend trois arguments :</span><span class="sxs-lookup"><span data-stu-id="8a7ca-142">The [SelectList](https://msdn.microsoft.com/library/dd505286.aspx) constructor overload used above takes three arguments:</span></span>

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample6.cs)]

1. <span data-ttu-id="8a7ca-143">*éléments*: un [IEnumerable](https://msdn.microsoft.com/library/system.collections.ienumerable.aspx) contenant les éléments dans la liste.</span><span class="sxs-lookup"><span data-stu-id="8a7ca-143">*items*: An [IEnumerable](https://msdn.microsoft.com/library/system.collections.ienumerable.aspx) containing the items in the list.</span></span> <span data-ttu-id="8a7ca-144">Dans l’exemple ci-dessus, la liste des genres retourné par `db.Genres`.</span><span class="sxs-lookup"><span data-stu-id="8a7ca-144">In the example above, the list of genres returned by `db.Genres`.</span></span>
2. <span data-ttu-id="8a7ca-145">*dataValueField*: le nom de la propriété dans le **IEnumerable** liste qui contient la valeur de clé.</span><span class="sxs-lookup"><span data-stu-id="8a7ca-145">*dataValueField*: The name of the property in the **IEnumerable** list that contains the key value.</span></span> <span data-ttu-id="8a7ca-146">Dans l’exemple ci-dessus, `GenreId` et `ArtistId`.</span><span class="sxs-lookup"><span data-stu-id="8a7ca-146">In the example above, `GenreId` and `ArtistId`.</span></span>
3. <span data-ttu-id="8a7ca-147">*dataTextField*: le nom de la propriété dans le **IEnumerable** liste qui contient les informations à afficher.</span><span class="sxs-lookup"><span data-stu-id="8a7ca-147">*dataTextField*: The name of the property in the **IEnumerable** list that contains the information to display.</span></span> <span data-ttu-id="8a7ca-148">Dans les artistes et la table de genre, le `name` champ est utilisé.</span><span class="sxs-lookup"><span data-stu-id="8a7ca-148">In both the artists and genre table, the `name` field is used.</span></span>

<span data-ttu-id="8a7ca-149">Ouvrez le *Views\StoreManager\Create.cshtml* de fichiers et d’examiner le `Html.DropDownList` balisage d’assistance pour le champ de genre.</span><span class="sxs-lookup"><span data-stu-id="8a7ca-149">Open the *Views\StoreManager\Create.cshtml* file and examine the `Html.DropDownList` helper markup for the genre field.</span></span>

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample7.cshtml)]

<span data-ttu-id="8a7ca-150">La première ligne indique que la vue create prend un `Album` modèle.</span><span class="sxs-lookup"><span data-stu-id="8a7ca-150">The first line shows that the create view takes an `Album` model.</span></span> <span data-ttu-id="8a7ca-151">Dans le `Create` méthode illustrée ci-dessus, aucun modèle n’a été passé, donc la vue obtient un **null** `Album` modèle.</span><span class="sxs-lookup"><span data-stu-id="8a7ca-151">In the `Create` method shown above, no model was passed, so the view gets a **null** `Album` model.</span></span> <span data-ttu-id="8a7ca-152">À ce stade, nous allons créer un nouvel album donc nous n’avez aucun `Album` données pour celui-ci.</span><span class="sxs-lookup"><span data-stu-id="8a7ca-152">At this point we are creating a new album so we don't have any `Album` data for it.</span></span>

<span data-ttu-id="8a7ca-153">Le [Html.DropDownList](https://msdn.microsoft.com/library/dd492948.aspx) surcharge ci-dessus prend le nom du champ à lier au modèle.</span><span class="sxs-lookup"><span data-stu-id="8a7ca-153">The [Html.DropDownList](https://msdn.microsoft.com/library/dd492948.aspx) overload shown above takes the name of the field to bind to the model.</span></span> <span data-ttu-id="8a7ca-154">Il utilise également ce nom pour rechercher un **ViewBag** objet contenant un [SelectList](https://msdn.microsoft.com/library/dd505286.aspx) objet.</span><span class="sxs-lookup"><span data-stu-id="8a7ca-154">It also uses this name to look for a **ViewBag** object containing a [SelectList](https://msdn.microsoft.com/library/dd505286.aspx) object.</span></span> <span data-ttu-id="8a7ca-155">À l’aide de cette surcharge, vous êtes amené à nom le **ViewBag SelectList** objet `GenreId`.</span><span class="sxs-lookup"><span data-stu-id="8a7ca-155">Using this overload, you are required to name the **ViewBag SelectList** object `GenreId`.</span></span> <span data-ttu-id="8a7ca-156">Le deuxième paramètre (`String.Empty`) est le texte à afficher lorsque aucun élément n’est sélectionné.</span><span class="sxs-lookup"><span data-stu-id="8a7ca-156">The second parameter (`String.Empty`) is the text to display when no item is selected.</span></span> <span data-ttu-id="8a7ca-157">C’est exactement ce que nous voulons lors de la création d’un nouvel album.</span><span class="sxs-lookup"><span data-stu-id="8a7ca-157">This is exactly what we want when creating a new album.</span></span> <span data-ttu-id="8a7ca-158">Si vous supprimé le deuxième paramètre et utilisez le code suivant :</span><span class="sxs-lookup"><span data-stu-id="8a7ca-158">If you removed the second parameter and used the following code:</span></span>

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample8.cshtml)]

<span data-ttu-id="8a7ca-159">La liste de sélection est par défaut pour le premier élément, ou Rock dans notre exemple.</span><span class="sxs-lookup"><span data-stu-id="8a7ca-159">The select list would default to the first element, or Rock in our sample.</span></span>

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image5.png)

<span data-ttu-id="8a7ca-160">Examen du `HTTP POST Create` (méthode).</span><span class="sxs-lookup"><span data-stu-id="8a7ca-160">Examining the `HTTP POST Create` method.</span></span>

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample9.cs)]

<span data-ttu-id="8a7ca-161">Cette surcharge de la `Create` méthode prend un `album` objet, créé par le système de liaison de modèle ASP.NET MVC à partir des valeurs de formulaire publiées.</span><span class="sxs-lookup"><span data-stu-id="8a7ca-161">This overload of the `Create` method takes an `album` object, created by the ASP.NET MVC model binding system from the form values posted.</span></span> <span data-ttu-id="8a7ca-162">Lorsque vous envoyez un nouvel album, si l’état du modèle est valide et il n’existe aucune erreur de base de données, le nouvel album est ajouté à la base de données.</span><span class="sxs-lookup"><span data-stu-id="8a7ca-162">When you submit a new album, if model state is valid and there are no database errors, the new album is added the database.</span></span> <span data-ttu-id="8a7ca-163">L’image suivante illustre la création d’un nouvel album.</span><span class="sxs-lookup"><span data-stu-id="8a7ca-163">The following image shows the creation of a new album.</span></span>

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image6.png)

<span data-ttu-id="8a7ca-164">Vous pouvez utiliser la [outil fiddler](http://www.fiddler2.com/fiddler2/) pour examiner les valeurs de formulaire publiées cette liaison de modèle ASP.NET MVC utilise pour créer l’objet de l’album.</span><span class="sxs-lookup"><span data-stu-id="8a7ca-164">You can use the [fiddler tool](http://www.fiddler2.com/fiddler2/) to examine the posted form values that ASP.NET MVC model binding uses to create the album object.</span></span>

<span data-ttu-id="8a7ca-165">![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image7.png).</span><span class="sxs-lookup"><span data-stu-id="8a7ca-165">![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image7.png).</span></span>

### <a name="refactoring-the-viewbag-selectlist-creation"></a><span data-ttu-id="8a7ca-166">Refactorisation de la création de ViewBag SelectList</span><span class="sxs-lookup"><span data-stu-id="8a7ca-166">Refactoring the ViewBag SelectList Creation</span></span>

<span data-ttu-id="8a7ca-167">Les deux le `Edit` méthodes et les `HTTP POST Create` méthode avoir un code identique pour configurer le **SelectList** dans le **ViewBag**.</span><span class="sxs-lookup"><span data-stu-id="8a7ca-167">Both the `Edit` methods and the `HTTP POST Create` method have identical code to set up the **SelectList** in the **ViewBag**.</span></span> <span data-ttu-id="8a7ca-168">Dans l’esprit de [DRY](http://en.wikipedia.org/wiki/Don't_repeat_yourself), nous devez refactoriser ce code.</span><span class="sxs-lookup"><span data-stu-id="8a7ca-168">In the spirit of [DRY](http://en.wikipedia.org/wiki/Don't_repeat_yourself), we will refactor this code.</span></span> <span data-ttu-id="8a7ca-169">Nous allons rendre l’utilisation de ce refactorisé le code plus tard.</span><span class="sxs-lookup"><span data-stu-id="8a7ca-169">We'll make use of this refactored code later.</span></span>

<span data-ttu-id="8a7ca-170">Créez une nouvelle méthode pour ajouter un genre et un artiste **SelectList** à la **ViewBag**.</span><span class="sxs-lookup"><span data-stu-id="8a7ca-170">Create a new method to add a genre and artist **SelectList** to the **ViewBag**.</span></span>

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample10.cs)]

<span data-ttu-id="8a7ca-171">Remplacez les deux lignes définissant le `ViewBag` dans chacune de la `Create` et `Edit` méthodes avec un appel à la `SetGenreArtistViewBag` (méthode).</span><span class="sxs-lookup"><span data-stu-id="8a7ca-171">Replace the two lines setting the `ViewBag` in each of the `Create` and `Edit` methods with a call to the `SetGenreArtistViewBag` method.</span></span> <span data-ttu-id="8a7ca-172">Le code terminé est indiqué ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="8a7ca-172">The completed code is shown below.</span></span>

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample11.cs)]

<span data-ttu-id="8a7ca-173">Créer un nouvel album et modifier un album pour vérifier que les modifications fonctionnent.</span><span class="sxs-lookup"><span data-stu-id="8a7ca-173">Create a new album and edit an album to verify the changes work.</span></span>

### <a name="explicitly-passing-the-selectlist-to-the-dropdownlist"></a><span data-ttu-id="8a7ca-174">En fournissant explicitement le SelectList au contrôle DropDownList</span><span class="sxs-lookup"><span data-stu-id="8a7ca-174">Explicitly Passing the SelectList to the DropDownList</span></span>

<span data-ttu-id="8a7ca-175">Les vues create et edit créée à l’aide de la génération de modèles automatique ASP.NET MVC les **DropDownList** surcharge :</span><span class="sxs-lookup"><span data-stu-id="8a7ca-175">The create and edit views created by the ASP.NET MVC scaffolding use the following **DropDownList** overload:</span></span>

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample12.cs)]

<span data-ttu-id="8a7ca-176">Le `DropDownList` balisage de la vue create est indiqué ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="8a7ca-176">The `DropDownList` markup for the create view is shown below.</span></span>

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample13.cshtml)]

<span data-ttu-id="8a7ca-177">Étant donné que le `ViewBag` propriété pour le `SelectList` est nommé `GenreId`, le **DropDownList** helper utilisera le `GenreId` **SelectList** dans le **ViewBag** .</span><span class="sxs-lookup"><span data-stu-id="8a7ca-177">Because the `ViewBag` property for the `SelectList` is named `GenreId`, the **DropDownList** helper will use the `GenreId`**SelectList** in the **ViewBag**.</span></span> <span data-ttu-id="8a7ca-178">Dans l’exemple suivant **DropDownList** de surcharge, le `SelectList` soit passée explicitement dans.</span><span class="sxs-lookup"><span data-stu-id="8a7ca-178">In the following **DropDownList** overload, the `SelectList` is explicitly passed in.</span></span>

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample14.cs)]

<span data-ttu-id="8a7ca-179">Ouvrez le *Views\StoreManager\Edit.cshtml* du fichier et changer le **DropDownList** appel à transmettre explicitement le **SelectList**, à l’aide de la surcharge ci-dessus.</span><span class="sxs-lookup"><span data-stu-id="8a7ca-179">Open the *Views\StoreManager\Edit.cshtml* file, and change the **DropDownList** call to explicitly pass in the **SelectList**, using the overload above.</span></span> <span data-ttu-id="8a7ca-180">Cela pour la catégorie de Genre.</span><span class="sxs-lookup"><span data-stu-id="8a7ca-180">Do this for the Genre category.</span></span> <span data-ttu-id="8a7ca-181">Le code complet est indiqué ci-dessous :</span><span class="sxs-lookup"><span data-stu-id="8a7ca-181">The completed code is shown below:</span></span>

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample15.cshtml)]

<span data-ttu-id="8a7ca-182">Exécutez l’application et cliquez sur le **administrateur** lier, puis accédez à un album de Jazz et sélectionnez le **modifier** lien.</span><span class="sxs-lookup"><span data-stu-id="8a7ca-182">Run the application and click the **Admin** link, then navigate to a Jazz album and select the **Edit** link.</span></span>

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image8.png)

<span data-ttu-id="8a7ca-183">Au lieu d’afficher Jazz en tant que le genre actuellement sélectionné, comme le ROC est affiché.</span><span class="sxs-lookup"><span data-stu-id="8a7ca-183">Instead of showing Jazz as the currently selected genre, Rock is displayed.</span></span> <span data-ttu-id="8a7ca-184">Lorsque l’argument de chaîne (la propriété à lier) et le **SelectList** objet portent le même nom, la valeur sélectionnée n’est pas utilisée.</span><span class="sxs-lookup"><span data-stu-id="8a7ca-184">When the string argument (the property to bind) and the **SelectList** object have the same name, the selected value is not used.</span></span> <span data-ttu-id="8a7ca-185">Lorsqu’il n’existe aucune valeur sélectionnée est fourni, les navigateurs par défaut vers le premier élément dans le **SelectList**(c'est-à-dire **Rock** dans l’exemple ci-dessus).</span><span class="sxs-lookup"><span data-stu-id="8a7ca-185">When there is no selected value provided, browsers default to the first element in the **SelectList**(which is **Rock** in the example above).</span></span> <span data-ttu-id="8a7ca-186">Il s’agit d’une limitation connue de la **DropDownList** helper.</span><span class="sxs-lookup"><span data-stu-id="8a7ca-186">This is a known limitation of the **DropDownList** helper.</span></span>

<span data-ttu-id="8a7ca-187">Ouvrez le *Controllers\StoreManagerController.cs* du fichier et changer la **SelectList** noms à l’objet `Genres` et `Artists`.</span><span class="sxs-lookup"><span data-stu-id="8a7ca-187">Open the *Controllers\StoreManagerController.cs* file and change the **SelectList** object names to `Genres` and `Artists`.</span></span> <span data-ttu-id="8a7ca-188">Le code complet est indiqué ci-dessous :</span><span class="sxs-lookup"><span data-stu-id="8a7ca-188">The completed code is shown below:</span></span>

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample16.cs)]

<span data-ttu-id="8a7ca-189">Les noms Genres et artistes sont meilleurs noms pour les catégories, car ils contiennent plus de simplement l’ID de chaque catégorie.</span><span class="sxs-lookup"><span data-stu-id="8a7ca-189">The names Genres and Artists are better names for the categories, as they contain more than just the ID of each category.</span></span> <span data-ttu-id="8a7ca-190">La refactorisation, que nous l’avons fait précédemment porté ses fruits.</span><span class="sxs-lookup"><span data-stu-id="8a7ca-190">The refactoring we did earlier paid off.</span></span> <span data-ttu-id="8a7ca-191">Au lieu de modifier le **ViewBag** dans les quatre méthodes, nos modifications étaient isolées dans le `SetGenreArtistViewBag` (méthode).</span><span class="sxs-lookup"><span data-stu-id="8a7ca-191">Instead of changing the **ViewBag** in four methods, our changes were isolated to the `SetGenreArtistViewBag` method.</span></span>

<span data-ttu-id="8a7ca-192">Modifier le **DropDownList** appeler dans la créer et modifier des affichages pour utiliser la nouvelle **SelectList** noms.</span><span class="sxs-lookup"><span data-stu-id="8a7ca-192">Change the **DropDownList** call in the create and edit views to use the new **SelectList** names.</span></span> <span data-ttu-id="8a7ca-193">Le balisage pour le mode édition est indiqué ci-dessous :</span><span class="sxs-lookup"><span data-stu-id="8a7ca-193">The new markup for the edit view is shown below:</span></span>

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample17.cshtml)]

<span data-ttu-id="8a7ca-194">La vue Create nécessite une chaîne vide pour le premier élément dans le SelectList empêcher l’affichage.</span><span class="sxs-lookup"><span data-stu-id="8a7ca-194">The Create view requires an empty string to prevent the first item in the SelectList from being displayed.</span></span>

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample18.cshtml)]

<span data-ttu-id="8a7ca-195">Créer un nouvel album et modifier un album pour vérifier que les modifications fonctionnent.</span><span class="sxs-lookup"><span data-stu-id="8a7ca-195">Create a new album and edit an album to verify the changes work.</span></span> <span data-ttu-id="8a7ca-196">Tester le code de la modifier en sélectionnant un album avec un genre autre que Rock.</span><span class="sxs-lookup"><span data-stu-id="8a7ca-196">Test the edit code by selecting an album with a genre other than Rock.</span></span>

### <a name="using-a-view-model-with-the-dropdownlist-helper"></a><span data-ttu-id="8a7ca-197">À l’aide d’un modèle de vue avec l’application d’assistance DropDownList</span><span class="sxs-lookup"><span data-stu-id="8a7ca-197">Using a View Model with the DropDownList Helper</span></span>

<span data-ttu-id="8a7ca-198">Créer une nouvelle classe dans le dossier ViewModels nommé `AlbumSelectListViewModel`.</span><span class="sxs-lookup"><span data-stu-id="8a7ca-198">Create a new class in the ViewModels folder named `AlbumSelectListViewModel`.</span></span> <span data-ttu-id="8a7ca-199">Remplacez le code dans la `AlbumSelectListViewModel` classe par le code suivant :</span><span class="sxs-lookup"><span data-stu-id="8a7ca-199">Replace the code in the `AlbumSelectListViewModel` class with the following:</span></span>

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample19.cs)]

<span data-ttu-id="8a7ca-200">Le `AlbumSelectListViewModel` constructeur accepte un album, une liste d’artistes et genres et crée un objet contenant l’album et un `SelectList` pour les genres et artistes.</span><span class="sxs-lookup"><span data-stu-id="8a7ca-200">The `AlbumSelectListViewModel` constructor takes an album, a list of artists and genres and creates an object containing the album and a `SelectList` for genres and artists.</span></span>

<span data-ttu-id="8a7ca-201">Générez le projet afin que la `AlbumSelectListViewModel` est disponible lorsque nous créons une vue à l’étape suivante.</span><span class="sxs-lookup"><span data-stu-id="8a7ca-201">Build the project so the `AlbumSelectListViewModel` is available when we create a view in the next step.</span></span>

<span data-ttu-id="8a7ca-202">Ajouter un `EditVM` méthode à la `StoreManagerController`.</span><span class="sxs-lookup"><span data-stu-id="8a7ca-202">Add an `EditVM` method to the `StoreManagerController`.</span></span> <span data-ttu-id="8a7ca-203">Le code terminé est indiqué ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="8a7ca-203">The completed code is shown below.</span></span>

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample20.cs)]

<span data-ttu-id="8a7ca-204">Bouton droit sur `AlbumSelectListViewModel`, sélectionnez **résoudre**, puis **à l’aide de MvcMusicStore.ViewModels ;**.</span><span class="sxs-lookup"><span data-stu-id="8a7ca-204">Right click `AlbumSelectListViewModel`, select **Resolve**, then **using MvcMusicStore.ViewModels;**.</span></span>

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image9.png)

<span data-ttu-id="8a7ca-205">Vous pouvez également ajouter les éléments suivants à l’aide d’instruction :</span><span class="sxs-lookup"><span data-stu-id="8a7ca-205">Alternatively, you can add the following using statement:</span></span>

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample21.cs)]

<span data-ttu-id="8a7ca-206">Bouton droit sur `EditVM` et sélectionnez **ajouter une vue**.</span><span class="sxs-lookup"><span data-stu-id="8a7ca-206">Right click `EditVM` and select **Add View**.</span></span> <span data-ttu-id="8a7ca-207">Utilisez les options ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="8a7ca-207">Use the options shown below.</span></span>

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image10.png)

<span data-ttu-id="8a7ca-208">Sélectionnez **ajouter**, puis remplacez le contenu de la *Views\StoreManager\EditVM.cshtml* fichier par le code suivant :</span><span class="sxs-lookup"><span data-stu-id="8a7ca-208">Select **Add**, then replace the contents of the *Views\StoreManager\EditVM.cshtml* file with the following:</span></span>

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample22.cshtml)]

<span data-ttu-id="8a7ca-209">Le `EditVM` balisage est très similaire à l’original `Edit` balisage avec les exceptions suivantes.</span><span class="sxs-lookup"><span data-stu-id="8a7ca-209">The `EditVM` markup is very similar to the original `Edit` markup with the following exceptions.</span></span>

- <span data-ttu-id="8a7ca-210">Modéliser les propriétés dans le `Edit` vue sont au format `model.property`(par exemple, `model.Title` ).</span><span class="sxs-lookup"><span data-stu-id="8a7ca-210">Model properties in the `Edit` view are of the form `model.property`(for example, `model.Title` ).</span></span> <span data-ttu-id="8a7ca-211">Modéliser les propriétés dans le `EditVm` vue sont au format `model.Album.property`(par exemple, `model.Album.Title`).</span><span class="sxs-lookup"><span data-stu-id="8a7ca-211">Model properties in the `EditVm` view are of the form `model.Album.property`(for example, `model.Album.Title`).</span></span> <span data-ttu-id="8a7ca-212">C’est parce que le `EditVM` vue est passée à un conteneur un `Album`, et non un `Album` comme dans le `Edit` vue.</span><span class="sxs-lookup"><span data-stu-id="8a7ca-212">That's because the `EditVM` view is passed a container for an `Album`, not an `Album` as in the `Edit` view.</span></span>
- <span data-ttu-id="8a7ca-213">Le **DropDownList** deuxième paramètre est fourni à partir du modèle de vue, pas le **ViewBag**.</span><span class="sxs-lookup"><span data-stu-id="8a7ca-213">The **DropDownList** second parameter comes from the view model, not the **ViewBag**.</span></span>
- <span data-ttu-id="8a7ca-214">Le **BeginForm** helper dans la `EditVM` vue publie explicitement vers le `Edit` méthode d’action.</span><span class="sxs-lookup"><span data-stu-id="8a7ca-214">The **BeginForm** helper in the `EditVM` view explicitly posts back to the `Edit` action method.</span></span> <span data-ttu-id="8a7ca-215">En publiant revenir à la `Edit` action, nous n’avons pas à écrire un `HTTP POST EditVM` action et vous pouvez réutiliser le `HTTP POST` `Edit` action.</span><span class="sxs-lookup"><span data-stu-id="8a7ca-215">By posting back to the `Edit` action, we don't have to write an `HTTP POST EditVM` action and can reuse the `HTTP POST` `Edit` action.</span></span>

<span data-ttu-id="8a7ca-216">Exécutez l’application et modifier un album.</span><span class="sxs-lookup"><span data-stu-id="8a7ca-216">Run the application and edit an album.</span></span> <span data-ttu-id="8a7ca-217">Modifier l’URL à utiliser `EditVM`.</span><span class="sxs-lookup"><span data-stu-id="8a7ca-217">Change the URL to use `EditVM`.</span></span> <span data-ttu-id="8a7ca-218">Modifier un champ et appuyez sur la **enregistrer** bouton pour vérifier le code fonctionne.</span><span class="sxs-lookup"><span data-stu-id="8a7ca-218">Change a field and hit the **Save** button to verify the code is working.</span></span>

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image11.png)

### <a name="which-approach-should-you-use"></a><span data-ttu-id="8a7ca-219">Quelle approche devez-vous utiliser ?</span><span class="sxs-lookup"><span data-stu-id="8a7ca-219">Which Approach Should You Use?</span></span>

<span data-ttu-id="8a7ca-220">Tous les trois approches indiqués sont acceptables.</span><span class="sxs-lookup"><span data-stu-id="8a7ca-220">All three approaches shown are acceptible.</span></span> <span data-ttu-id="8a7ca-221">De nombreux développeurs préfèrent à explictily passe le `SelectList` à la `DropDownList` à l’aide de la `ViewBag`.</span><span class="sxs-lookup"><span data-stu-id="8a7ca-221">Many developers prefer to explictily pass the `SelectList` to the `DropDownList` using the `ViewBag`.</span></span> <span data-ttu-id="8a7ca-222">Cette approche présente l’avantage de ce qui vous donne la souplesse d’utilisation d’un nom plus approprié pour la collection.</span><span class="sxs-lookup"><span data-stu-id="8a7ca-222">This approach has the added advantage of giving you the flexibility of using a more appropriate name for the collection.</span></span> <span data-ttu-id="8a7ca-223">L’inconvénient est que vous ne pouvez pas nommer le `ViewBag SelectList` le même nom que la propriété de modèle d’objet.</span><span class="sxs-lookup"><span data-stu-id="8a7ca-223">The one caveat is you cannot name the `ViewBag SelectList` object the same name as the model property.</span></span>

<span data-ttu-id="8a7ca-224">Certains développeurs préfèrent l’approche ViewModel.</span><span class="sxs-lookup"><span data-stu-id="8a7ca-224">Some developers prefer the ViewModel approach.</span></span> <span data-ttu-id="8a7ca-225">D’autres Examinez le balisage plus détaillé et généré HTML de l’approche ViewModel un inconvénient.</span><span class="sxs-lookup"><span data-stu-id="8a7ca-225">Others consider the more verbose markup and generated HTML of the ViewModel approach a disadvantage.</span></span>

<span data-ttu-id="8a7ca-226">Dans cette section, nous avons appris à l’aide de trois approches le **DropDownList** avec des données de catégorie.</span><span class="sxs-lookup"><span data-stu-id="8a7ca-226">In this section we have learned three approaches to using the **DropDownList** with category data.</span></span> <span data-ttu-id="8a7ca-227">Dans la section suivante, nous allons montrer comment ajouter une nouvelle catégorie.</span><span class="sxs-lookup"><span data-stu-id="8a7ca-227">In the next section, we'll show how to add a new category.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="8a7ca-228">[Précédent](using-the-dropdownlist-helper-with-aspnet-mvc.md)
> [Suivant](adding-a-new-category-to-the-dropdownlist-using-jquery-ui.md)</span><span class="sxs-lookup"><span data-stu-id="8a7ca-228">[Previous](using-the-dropdownlist-helper-with-aspnet-mvc.md)
[Next](adding-a-new-category-to-the-dropdownlist-using-jquery-ui.md)</span></span>
