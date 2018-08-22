---
uid: mvc/overview/older-versions/working-with-the-dropdownlist-box-and-jquery/adding-a-new-category-to-the-dropdownlist-using-jquery-ui
title: Ajout d’une nouvelle catégorie au contrôle DropDownList à l’aide de l’interface utilisateur jQuery | Microsoft Docs
author: Rick-Anderson
description: ''
ms.author: riande
ms.date: 01/12/2012
ms.assetid: 44aa1ac4-6ea2-48a2-972d-52710c48eae5
msc.legacyurl: /mvc/overview/older-versions/working-with-the-dropdownlist-box-and-jquery/adding-a-new-category-to-the-dropdownlist-using-jquery-ui
msc.type: authoredcontent
ms.openlocfilehash: 9a893bf55070adde575d223cd527b02d6f0d470c
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41834616"
---
<a name="adding-a-new-category-to-the-dropdownlist-using-jquery-ui"></a><span data-ttu-id="e01b4-102">Ajout d’une nouvelle catégorie au contrôle DropDownList à l’aide de l’interface utilisateur jQuery</span><span class="sxs-lookup"><span data-stu-id="e01b4-102">Adding a New Category to the DropDownList using jQuery UI</span></span>
====================
<span data-ttu-id="e01b4-103">par [Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="e01b4-103">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

<span data-ttu-id="e01b4-104">Le code HTML `Select` balise est idéal pour présenter une liste de données de catégorie fixe, mais il arrive souvent que vous devez ajouter une nouvelle catégorie.</span><span class="sxs-lookup"><span data-stu-id="e01b4-104">The HTML `Select` tag is ideal for presenting a list of fixed category data, but often times you need to add a new category.</span></span> <span data-ttu-id="e01b4-105">Supposons que nous souhaitons ajouter le genre « Opera » pour les catégories dans notre base de données ?</span><span class="sxs-lookup"><span data-stu-id="e01b4-105">Suppose we want to add the genre "Opera" to the categories in our database?</span></span> <span data-ttu-id="e01b4-106">Dans cette section, nous allons utiliser l’interface utilisateur jQuery pour ajouter une boîte de dialogue que nous pouvons utiliser pour ajouter une nouvelle catégorie.</span><span class="sxs-lookup"><span data-stu-id="e01b4-106">In this section, we will use jQuery UI to add a dialog box we can use to add a new category.</span></span> <span data-ttu-id="e01b4-107">L’image ci-dessous montre comment l’interface utilisateur présente dans le navigateur.</span><span class="sxs-lookup"><span data-stu-id="e01b4-107">The image below shows how the UI will present in the browser.</span></span>

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image1.png)

<span data-ttu-id="e01b4-108">Lorsqu’un utilisateur sélectionne le **ajouter un nouveau Genre** lien, une boîte de dialogue contextuelle invite l’utilisateur à un nouveau nom de genre (et éventuellement une description).</span><span class="sxs-lookup"><span data-stu-id="e01b4-108">When a user selects the **Add New Genre** link, a pop-up dialog box prompts the user for a new genre name (and optionally a description).</span></span> <span data-ttu-id="e01b4-109">L’image ci-dessous montrent les **Genre ajouter** boîte de dialogue contextuelle.</span><span class="sxs-lookup"><span data-stu-id="e01b4-109">The image below show the **Add Genre** pop-up dialog.</span></span>

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image2.png)

<span data-ttu-id="e01b4-110">Quand un nouveau nom de genre est entré et la **enregistrer** bouton est actionné, ce qui suit se produit :</span><span class="sxs-lookup"><span data-stu-id="e01b4-110">When a new genre name is entered and the **Save** button is pushed, the following happens:</span></span>

1. <span data-ttu-id="e01b4-111">Un appel AJAX publie les données à la méthode Create du contrôleur Genre, qui enregistre le genre de nouveau dans la base de données et retourne les nouveau genre d’informations (nom du genre et ID) au format JSON.</span><span class="sxs-lookup"><span data-stu-id="e01b4-111">An AJAX call posts the data to the Create method of the Genre Controller, which saves the new genre to the database and returns the new genre information (genre name and ID) as JSON.</span></span>
2. <span data-ttu-id="e01b4-112">JavaScript ajoute les nouvelles données de genre à la liste de sélection.</span><span class="sxs-lookup"><span data-stu-id="e01b4-112">JavaScript adds the new genre data to the select list.</span></span>
3. <span data-ttu-id="e01b4-113">JavaScript rend le genre de nouveau l’élément sélectionné.</span><span class="sxs-lookup"><span data-stu-id="e01b4-113">JavaScript makes the new genre the selected item.</span></span>

   <span data-ttu-id="e01b4-114">Dans l’image ci-dessous, **Opera** a été ajoutée à la base de données et sélectionnée dans le **Genre** liste déroulante.</span><span class="sxs-lookup"><span data-stu-id="e01b4-114">In the image below, **Opera** was added to the database and selected in the **Genre** drop down list.</span></span> 

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image3.png)

<span data-ttu-id="e01b4-115">Ouvrez le *Views\StoreManager\Create.cshtml* fichier et remplacez le balisage de genre par ce qui suit le code suivant :</span><span class="sxs-lookup"><span data-stu-id="e01b4-115">Open the *Views\StoreManager\Create.cshtml* file and replace the genre markup with the following the following code:</span></span>

[!code-cshtml[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample1.cshtml)]

<span data-ttu-id="e01b4-116">Le `_ChooseGenre` vue partielle contient toute la logique pour raccorder le JavaScript et jQuery, permettent d’implémenter la fonctionnalité de genre Ajouter nouveau.</span><span class="sxs-lookup"><span data-stu-id="e01b4-116">The `_ChooseGenre` partial view will contain all the logic to hook up the JavaScript and jQuery used to implement the add new genre feature.</span></span> <span data-ttu-id="e01b4-117">Une fois que nous avons terminé le code, il sera facile à faire de même avec l’interface utilisateur de l’artiste.</span><span class="sxs-lookup"><span data-stu-id="e01b4-117">Once we have completed the code it will be simple to do the same with the artist UI.</span></span>

<span data-ttu-id="e01b4-118">Dans l’Explorateur de solutions, avec le bouton droit cliquez sur le *Views\StoreManager* dossier et sélectionnez **ajouter**, puis **vue**.</span><span class="sxs-lookup"><span data-stu-id="e01b4-118">In Solution Explorer, right click the *Views\StoreManager* folder and select **Add**, then **View**.</span></span> <span data-ttu-id="e01b4-119">Dans le **nom de la vue** d’entrée, entrez `_ChooseGenre` puis sélectionnez **ajouter**.</span><span class="sxs-lookup"><span data-stu-id="e01b4-119">In the **View name** input, enter `_ChooseGenre` then select **Add**.</span></span> <span data-ttu-id="e01b4-120">Remplacez le balisage dans le *Views\StoreManager\\_ChooseGenre.cshtml* fichier par le code suivant :</span><span class="sxs-lookup"><span data-stu-id="e01b4-120">Replace the markup in the *Views\StoreManager\\_ChooseGenre.cshtml* file with the following:</span></span>

[!code-cshtml[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample2.cshtml)]

<span data-ttu-id="e01b4-121">La première ligne déclare que nous passons dans un `Album` en tant que notre modèle, exactement de la même instruction se trouvant dans la vue de créer de modèle.</span><span class="sxs-lookup"><span data-stu-id="e01b4-121">The first line declares that we are passing in an `Album` as our model, exactly the same model statement found in the Create view.</span></span> <span data-ttu-id="e01b4-122">Les quelques lignes suivantes sont la **étiquette** balisage d’application d’assistance.</span><span class="sxs-lookup"><span data-stu-id="e01b4-122">The next few lines are the **Label** helper markup.</span></span> <span data-ttu-id="e01b4-123">La ligne suivante est la **DropDownList** appeler d’assistance, exactement comme dans la vue de créer d’origine.</span><span class="sxs-lookup"><span data-stu-id="e01b4-123">The next line is the **DropDownList** helper call, exactly the same as in the original Create view.</span></span> <span data-ttu-id="e01b4-124">La ligne suivante ajoute un lien avec le nom `Add New Genre`, et définit le style comme un bouton.</span><span class="sxs-lookup"><span data-stu-id="e01b4-124">The next line adds a link with the name `Add New Genre`, and styles it like a button.</span></span> <span data-ttu-id="e01b4-125">La ligne contenant `ValidationMessageFor` est copiée directement à partir de la vue de créer.</span><span class="sxs-lookup"><span data-stu-id="e01b4-125">The line containing `ValidationMessageFor` is copied directly from the Create view.</span></span> <span data-ttu-id="e01b4-126">Les lignes suivantes :</span><span class="sxs-lookup"><span data-stu-id="e01b4-126">The following lines:</span></span>

[!code-html[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample3.html)]

<span data-ttu-id="e01b4-127">Crée un élément div masquée, avec l’ID de `genreDialog`.</span><span class="sxs-lookup"><span data-stu-id="e01b4-127">creates a hidden div, with the ID of `genreDialog`.</span></span> <span data-ttu-id="e01b4-128">Nous allons utiliser jQuery pour raccorder notre **Genre ajouter** boîte de dialogue avec l’ID `genreDialog` dans cette balise div.</span><span class="sxs-lookup"><span data-stu-id="e01b4-128">We will use jQuery to hook up our **Add Genre** dialog box with the ID `genreDialog` in this div.</span></span> <span data-ttu-id="e01b4-129">Les deux dernières balises de script contiennent des liens vers les fichiers JavaScript que nous utiliserons pour implémenter la fonctionnalité de genre Ajouter nouveau.</span><span class="sxs-lookup"><span data-stu-id="e01b4-129">The last two script tags contain links to the JavaScript files we will use to implement the add new genre feature.</span></span> <span data-ttu-id="e01b4-130">Le */Scripts/chooseGenre.js* fichier n’est fourni pour vous dans le projet, nous allons l’examiner ultérieurement dans ce didacticiel.</span><span class="sxs-lookup"><span data-stu-id="e01b4-130">The */Scripts/chooseGenre.js* file is provided for you in the project, we will examine it later in the tutorial.</span></span>

<span data-ttu-id="e01b4-131">Exécutez l’application et cliquez sur le **ajouter un nouveau Genre** bouton.</span><span class="sxs-lookup"><span data-stu-id="e01b4-131">Run the application and click on the **Add New Genre** button.</span></span> <span data-ttu-id="e01b4-132">Dans le **Genre ajouter** boîte de dialogue, entrez **Opera** dans le **nom** zone d’entrée.</span><span class="sxs-lookup"><span data-stu-id="e01b4-132">In the **Add Genre** dialog box, enter **Opera** in the **Name** input box.</span></span>

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image4.png)

<span data-ttu-id="e01b4-133">Cliquez sur le bouton **Enregistrer**.</span><span class="sxs-lookup"><span data-stu-id="e01b4-133">Click the **Save** button.</span></span> <span data-ttu-id="e01b4-134">Un appel AJAX crée la catégorie Opera puis remplit la liste déroulante avec Opera et définit Opera comme le genre sélectionné.</span><span class="sxs-lookup"><span data-stu-id="e01b4-134">An AJAX call creates the Opera category and then populates the dropdown list with Opera, and sets Opera as the selected genre.</span></span>

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image5.png)

<span data-ttu-id="e01b4-135">Entrez un artiste, le titre et le prix, puis sélectionnez le **créer** bouton.</span><span class="sxs-lookup"><span data-stu-id="e01b4-135">Enter an artist, title and price, then select the **Create** button.</span></span> <span data-ttu-id="e01b4-136">Si vous entrez un prix inférieur à $8,99, le nouvel album s’affiche en haut de la vue Index.</span><span class="sxs-lookup"><span data-stu-id="e01b4-136">If you enter a price less than $8.99, the new album will appear at the top of the Index view.</span></span> <span data-ttu-id="e01b4-137">Vérifiez que la nouvelle entrée d’album a été enregistrée dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="e01b4-137">Verify the new album entry was saved in the database.</span></span>

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image6.png)

<span data-ttu-id="e01b4-138">Essayez de créer un nouveau genre avec uniquement une lettre.</span><span class="sxs-lookup"><span data-stu-id="e01b4-138">Try creating a new genre with only one letter.</span></span> <span data-ttu-id="e01b4-139">Le code suivant dans le *Models\Genre.cs* fichier définit la longueur minimale et maximale du nom du genre.</span><span class="sxs-lookup"><span data-stu-id="e01b4-139">The following code in the *Models\Genre.cs* file sets the minimum and maximum length of the genre name.</span></span>

[!code-csharp[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample4.cs)]

<span data-ttu-id="e01b4-140">Validation côté client signale que vous devez entrer une chaîne entre 2 et 20 caractères.</span><span class="sxs-lookup"><span data-stu-id="e01b4-140">Client side validation reports you must enter a string between 2 and 20 characters.</span></span>

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image7.png)

### <a name="examining-how-a-new-genre-is-added-to-the-database-and-the-select-list"></a><span data-ttu-id="e01b4-141">Examiner comment un Genre nouveau est ajouté à la base de données et de la liste Select.</span><span class="sxs-lookup"><span data-stu-id="e01b4-141">Examining How a New Genre is Added to the Database and the Select List.</span></span>

<span data-ttu-id="e01b4-142">Ouvrez le *Scripts\chooseGenre.js* de fichiers et examinez le code.</span><span class="sxs-lookup"><span data-stu-id="e01b4-142">Open the *Scripts\chooseGenre.js* file and examine the code.</span></span>

[!code-javascript[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample5.js)]

<span data-ttu-id="e01b4-143">La deuxième ligne utilise l’ID `genreDialog` pour créer une boîte de dialogue de la balise div dans le *Views\StoreManager\\_ChooseGenre.cshtml* fichier.</span><span class="sxs-lookup"><span data-stu-id="e01b4-143">The second line uses the ID `genreDialog` to create a dialog box on the div tag in the *Views\StoreManager\\_ChooseGenre.cshtml* file.</span></span> <span data-ttu-id="e01b4-144">La plupart des paramètres nommés est explicites.</span><span class="sxs-lookup"><span data-stu-id="e01b4-144">Most of the named parameters are self explanatory.</span></span> <span data-ttu-id="e01b4-145">Le `autoOpen` paramètre est défini sur false, en sélectionnant le **créer un Genre** bouton permet d’ouvrir la boîte de dialogue explicitement (cette opération est décrite ce dernier sur).</span><span class="sxs-lookup"><span data-stu-id="e01b4-145">The `autoOpen` parameter is set to false, selecting the **Create Genre** button will open the dialogue explicitly (this is described latter on).</span></span> <span data-ttu-id="e01b4-146">La boîte de dialogue a deux boutons, **enregistrer** et **Annuler**.</span><span class="sxs-lookup"><span data-stu-id="e01b4-146">The dialog has two buttons, **Save** and **Cancel**.</span></span> <span data-ttu-id="e01b4-147">Le **Annuler** bouton ferme la boîte de dialogue.</span><span class="sxs-lookup"><span data-stu-id="e01b4-147">The **Cancel** button closes the dialog.</span></span> <span data-ttu-id="e01b4-148">Le code suivant illustre la **enregistrer** bouton de fonction.</span><span class="sxs-lookup"><span data-stu-id="e01b4-148">The following code shows the **Save** button function.</span></span>

[!code-javascript[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample6.js)]

<span data-ttu-id="e01b4-149">Le `var createGenreForm` est sélectionné dans le `createGenreForm` ID.</span><span class="sxs-lookup"><span data-stu-id="e01b4-149">The `var createGenreForm` is selected from the `createGenreForm` ID.</span></span> <span data-ttu-id="e01b4-150">Le `createGenreForm` ID a été défini dans le code suivant est trouvé dans le *Views\Genre\\_CreateGenre.cshtml* fichier.</span><span class="sxs-lookup"><span data-stu-id="e01b4-150">The `createGenreForm` ID was set in the following code found in the *Views\Genre\\_CreateGenre.cshtml* file.</span></span>

[!code-cshtml[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample7.cshtml)]

<span data-ttu-id="e01b4-151">Le [Html.BeginForm](https://msdn.microsoft.com/library/dd492714.aspx) surcharge d’assistance utilisée dans le *Views\Genre\\_CreateGenre.cshtml* fichier génère du code HTML avec un attribut d’action contenant l’URL pour envoyer le formulaire.</span><span class="sxs-lookup"><span data-stu-id="e01b4-151">The [Html.BeginForm](https://msdn.microsoft.com/library/dd492714.aspx) helper overload used in the *Views\Genre\\_CreateGenre.cshtml* file generates HTML with an action attribute containing the URL to submit the form.</span></span> <span data-ttu-id="e01b4-152">Vous pouvez le voir en affichant la page d’album de créer dans un navigateur et en sélectionnant show source dans le navigateur.</span><span class="sxs-lookup"><span data-stu-id="e01b4-152">You can see this by displaying the create album page in a browser and selecting show source in the browser.</span></span> <span data-ttu-id="e01b4-153">Le balisage suivant montre le code HTML généré, qui contient la balise de formulaire.</span><span class="sxs-lookup"><span data-stu-id="e01b4-153">The following markup shows the generated HTML containing the form tag.</span></span>

[!code-html[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample8.html)]

<span data-ttu-id="e01b4-154">Le jQuery `$.post` ligne effectue un appel AJAX à l’attribut action (`/StoreManager/Create`), puis transmet les données à partir de la **créer un Genre** boîte de dialogue.</span><span class="sxs-lookup"><span data-stu-id="e01b4-154">The jQuery `$.post` line makes an AJAX call to the action attribute (`/StoreManager/Create`) and passes in the data from the **Create Genre** dialog box.</span></span> <span data-ttu-id="e01b4-155">Les données se composent du nom pour le genre de nouveau et une description facultative.</span><span class="sxs-lookup"><span data-stu-id="e01b4-155">The data consists of the name for the new genre and an optional description.</span></span> <span data-ttu-id="e01b4-156">Si l’appel AJAX est réussie, le nouveau nom du genre et la valeur sont ajoutés à la balise et le nouveau genre est défini sur la valeur sélectionnée.</span><span class="sxs-lookup"><span data-stu-id="e01b4-156">If the AJAX call is successful, the new genre name and value are added to the Select markup, and the new genre is set to the selected value.</span></span> <span data-ttu-id="e01b4-157">Comme il s’agit de balise générée dynamiquement, vous ne voyez la nouvelle option select en affichant la source dans le navigateur.</span><span class="sxs-lookup"><span data-stu-id="e01b4-157">Because this is dynamically generated markup, you can't see the new select option by viewing the source in the browser.</span></span> <span data-ttu-id="e01b4-158">Vous pouvez voir le nouveau code HTML avec les outils de développement Internet Explorer 9 F12.</span><span class="sxs-lookup"><span data-stu-id="e01b4-158">You can see the new HTML with the IE 9 F12 developer tools.</span></span> <span data-ttu-id="e01b4-159">Pour afficher la nouvelle option Sélectionnez, dans Internet Explorer 9, appuyez sur la touche F12 pour démarrer les outils de développement F12.</span><span class="sxs-lookup"><span data-stu-id="e01b4-159">To view the new select option, in Internet Explorer 9, hit the F12 key to start the F12 developer tools.</span></span> <span data-ttu-id="e01b4-160">Accédez à la page Créer et ajouter un nouveau genre pour le nouveau genre est sélectionné dans la liste de sélection de genre.</span><span class="sxs-lookup"><span data-stu-id="e01b4-160">Navigate to the Create page and add a new genre so the new genre is selected in the genre select list.</span></span> <span data-ttu-id="e01b4-161">Dans les outils de développement F12 :</span><span class="sxs-lookup"><span data-stu-id="e01b4-161">In the F12 developer tools:</span></span>

1. <span data-ttu-id="e01b4-162">Sélectionnez l’onglet HTML.</span><span class="sxs-lookup"><span data-stu-id="e01b4-162">Select the HTML tab.</span></span>
2. <span data-ttu-id="e01b4-163">Appuyez sur l’icône d’actualisation.</span><span class="sxs-lookup"><span data-stu-id="e01b4-163">Hit the refresh icon.</span></span>  
    ![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image8.png)
3. <span data-ttu-id="e01b4-164">Dans la zone de recherche, entrez GenreID.</span><span class="sxs-lookup"><span data-stu-id="e01b4-164">In the search box, enter GenreID.</span></span>
4. <span data-ttu-id="e01b4-165">À l’aide de l’icône suivante,</span><span class="sxs-lookup"><span data-stu-id="e01b4-165">Using the next icon,</span></span>   
    ![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image9.png)  
   <span data-ttu-id="e01b4-166">Accédez à la balise de sélection suivante :</span><span class="sxs-lookup"><span data-stu-id="e01b4-166">navigate to the following select tag:</span></span>

    [!code-html[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample9.html)]
5. <span data-ttu-id="e01b4-167">Développez la dernière valeur d’option.</span><span class="sxs-lookup"><span data-stu-id="e01b4-167">Expand the last option value.</span></span>

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image10.png)

<span data-ttu-id="e01b4-168">Le code suivant dans le *Scripts\chooseGenre.js* fichier illustre la façon dont le **ajouter un nouveau Genre** bouton se connecte à l’événement click et comment la **ajouter un nouveau Genre** boîte de dialogue est créé.</span><span class="sxs-lookup"><span data-stu-id="e01b4-168">The following code in the *Scripts\chooseGenre.js* file shows the how the **Add New Genre** button gets connected to the click event, and how the **Add New Genre** dialog box is created.</span></span>

[!code-javascript[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample10.js)]

<span data-ttu-id="e01b4-169">La première ligne crée une fonction de clic attachée à la **ajouter un nouveau Genre** bouton.</span><span class="sxs-lookup"><span data-stu-id="e01b4-169">The first line creates a click function attached to the **Add New Genre** button.</span></span> <span data-ttu-id="e01b4-170">Le balisage suivant à partir de la Views\StoreManager\\_ChooseGenre.cshtml fichier montre comment la **ajouter un nouveau Genre** bouton est créé :</span><span class="sxs-lookup"><span data-stu-id="e01b4-170">The following markup from the Views\StoreManager\\_ChooseGenre.cshtml file shows how the **Add New Genre** button is created:</span></span>

[!code-cshtml[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample11.cshtml)]

<span data-ttu-id="e01b4-171">La méthode load crée et ouvre la boîte de dialogue Ajouter un Genre et appelle le jQuery `parse` méthode afin de la validation côté client se produit sur les données entrées dans la boîte de dialogue.</span><span class="sxs-lookup"><span data-stu-id="e01b4-171">The load method creates and opens the Add Genre dialog and calls the jQuery `parse` method so client validation occurs on data entered in the dialog.</span></span>

<span data-ttu-id="e01b4-172">Dans cette section, vous avez appris à créer une boîte de dialogue qui peut être utilisé pour ajouter de nouvelles données de catégorie à une liste de sélection.</span><span class="sxs-lookup"><span data-stu-id="e01b4-172">In this section you have learned how to create a dialog that can be used to add new category data to a select list.</span></span> <span data-ttu-id="e01b4-173">Vous pouvez suivre la même procédure pour créer l’interface utilisateur pour ajouter un nouvel artiste à la liste de sélection artiste.</span><span class="sxs-lookup"><span data-stu-id="e01b4-173">You can follow the same procedure to create UI to add a new artist to the artist select list.</span></span> <span data-ttu-id="e01b4-174">Ce didacticiel vous a apporté une vue d’ensemble de l’utilisation du programme d’assistance ASP.NET MVC HTML **DropDownList**.</span><span class="sxs-lookup"><span data-stu-id="e01b4-174">This tutorial has given an overview of working with the ASP.NET MVC HTML helper **DropDownList**.</span></span> <span data-ttu-id="e01b4-175">Pour plus d’informations sur l’utilisation de la **DropDownList**, consultez la section Références addition ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="e01b4-175">For additional information on working with the **DropDownList**, see the addition references section below.</span></span> <span data-ttu-id="e01b4-176">Faites-nous savoir si ce didacticiel a été utile.</span><span class="sxs-lookup"><span data-stu-id="e01b4-176">Please let us know if this tutorial has been helpful.</span></span>

<span data-ttu-id="e01b4-177">Rick.Anderson[at]Microsoft.com</span><span class="sxs-lookup"><span data-stu-id="e01b4-177">Rick.Anderson[at]Microsoft.com</span></span>

### <a name="additional-references"></a><span data-ttu-id="e01b4-178">Références supplémentaires</span><span class="sxs-lookup"><span data-stu-id="e01b4-178">Additional References</span></span>

- <span data-ttu-id="e01b4-179">[ASP.NET MVC – Cascading de liste déroulante répertorie didacticiel](https://weblogs.asp.net/raduenuca/archive/2011/03/06/asp-net-mvc-cascading-dropdown-lists-tutorial-part-1-defining-the-problem-and-the-context.aspx) par [Radu Enuca](https://weblogs.asp.net/raduenuca/default.aspx)</span><span class="sxs-lookup"><span data-stu-id="e01b4-179">[ASP.NET MVC–Cascading Dropdown Lists Tutorial](https://weblogs.asp.net/raduenuca/archive/2011/03/06/asp-net-mvc-cascading-dropdown-lists-tutorial-part-1-defining-the-problem-and-the-context.aspx) by [Radu Enuca](https://weblogs.asp.net/raduenuca/default.aspx)</span></span>
- <span data-ttu-id="e01b4-180">[Choisi](http://harvesthq.github.com/chosen/) un plug-in d’un script JavaScript qui prennent en charge la sélection multiple et de filtrage.</span><span class="sxs-lookup"><span data-stu-id="e01b4-180">[Chosen](http://harvesthq.github.com/chosen/) A JavaScript plugin that support multi-select and filtering.</span></span>

### <a name="contributors"></a><span data-ttu-id="e01b4-181">Contributors</span><span class="sxs-lookup"><span data-stu-id="e01b4-181">Contributors</span></span>

- [<span data-ttu-id="e01b4-182">Radu Enuca</span><span class="sxs-lookup"><span data-stu-id="e01b4-182">Radu Enuca</span></span>](https://weblogs.asp.net/raduenuca/default.aspx)
- <span data-ttu-id="e01b4-183">Jean-Sébastien Goupil</span><span class="sxs-lookup"><span data-stu-id="e01b4-183">Jean-Sébastien Goupil</span></span>
- [<span data-ttu-id="e01b4-184">Brad Wilson.</span><span class="sxs-lookup"><span data-stu-id="e01b4-184">Brad Wilson</span></span>](http://bradwilson.typepad.com/)

### <a name="reviewers"></a><span data-ttu-id="e01b4-185">Réviseurs</span><span class="sxs-lookup"><span data-stu-id="e01b4-185">Reviewers</span></span>

- <span data-ttu-id="e01b4-186">Jean-Sébastien Goupil</span><span class="sxs-lookup"><span data-stu-id="e01b4-186">Jean-Sébastien Goupil</span></span>
- [<span data-ttu-id="e01b4-187">Brad Wilson.</span><span class="sxs-lookup"><span data-stu-id="e01b4-187">Brad Wilson</span></span>](http://bradwilson.typepad.com/)
- <span data-ttu-id="e01b4-188">Mike Pope</span><span class="sxs-lookup"><span data-stu-id="e01b4-188">Mike Pope</span></span>
- <span data-ttu-id="e01b4-189">Tom Dykstra</span><span class="sxs-lookup"><span data-stu-id="e01b4-189">Tom Dykstra</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="e01b4-190">Précédent</span><span class="sxs-lookup"><span data-stu-id="e01b4-190">Previous</span></span>](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper.md)
