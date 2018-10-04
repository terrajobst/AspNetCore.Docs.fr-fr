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
ms.openlocfilehash: 9fb95d22be473a4318520a391fa424106246a054
ms.sourcegitcommit: 7b4e3936feacb1a8fcea7802aab3e2ea9c8af5b4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/04/2018
ms.locfileid: "48576350"
---
<a name="adding-a-new-category-to-the-dropdownlist-using-jquery-ui"></a>Ajout d’une nouvelle catégorie au contrôle DropDownList à l’aide de l’interface utilisateur jQuery
====================
par [Rick Anderson]((https://twitter.com/RickAndMSFT))

Le code HTML `Select` balise est idéal pour présenter une liste de données de catégorie fixe, mais il arrive souvent que vous devez ajouter une nouvelle catégorie. Supposons que nous souhaitons ajouter le genre « Opera » pour les catégories dans notre base de données ? Dans cette section, nous allons utiliser l’interface utilisateur jQuery pour ajouter une boîte de dialogue que nous pouvons utiliser pour ajouter une nouvelle catégorie. L’image ci-dessous montre comment l’interface utilisateur présente dans le navigateur.

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image1.png)

Lorsqu’un utilisateur sélectionne le **ajouter un nouveau Genre** lien, une boîte de dialogue contextuelle invite l’utilisateur à un nouveau nom de genre (et éventuellement une description). L’image ci-dessous montrent les **Genre ajouter** boîte de dialogue contextuelle.

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image2.png)

Quand un nouveau nom de genre est entré et la **enregistrer** bouton est actionné, ce qui suit se produit :

1. Un appel AJAX publie les données à la méthode Create du contrôleur Genre, qui enregistre le genre de nouveau dans la base de données et retourne les nouveau genre d’informations (nom du genre et ID) au format JSON.
2. JavaScript ajoute les nouvelles données de genre à la liste de sélection.
3. JavaScript rend le genre de nouveau l’élément sélectionné.

   Dans l’image ci-dessous, **Opera** a été ajoutée à la base de données et sélectionnée dans le **Genre** liste déroulante. 

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image3.png)

Ouvrez le *Views\StoreManager\Create.cshtml* fichier et remplacez le balisage de genre par ce qui suit le code suivant :

[!code-cshtml[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample1.cshtml)]

Le `_ChooseGenre` vue partielle contient toute la logique pour raccorder le JavaScript et jQuery, permettent d’implémenter la fonctionnalité de genre Ajouter nouveau. Une fois que nous avons terminé le code, il sera facile à faire de même avec l’interface utilisateur de l’artiste.

Dans l’Explorateur de solutions, avec le bouton droit cliquez sur le *Views\StoreManager* dossier et sélectionnez **ajouter**, puis **vue**. Dans le **nom de la vue** d’entrée, entrez `_ChooseGenre` puis sélectionnez **ajouter**. Remplacez le balisage dans le *Views\StoreManager\\_ChooseGenre.cshtml* fichier par le code suivant :

[!code-cshtml[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample2.cshtml)]

La première ligne déclare que nous passons dans un `Album` en tant que notre modèle, exactement de la même instruction se trouvant dans la vue de créer de modèle. Les quelques lignes suivantes sont la **étiquette** balisage d’application d’assistance. La ligne suivante est la **DropDownList** appeler d’assistance, exactement comme dans la vue de créer d’origine. La ligne suivante ajoute un lien avec le nom `Add New Genre`, et définit le style comme un bouton. La ligne contenant `ValidationMessageFor` est copiée directement à partir de la vue de créer. Les lignes suivantes :

[!code-html[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample3.html)]

Crée un élément div masquée, avec l’ID de `genreDialog`. Nous allons utiliser jQuery pour raccorder notre **Genre ajouter** boîte de dialogue avec l’ID `genreDialog` dans cette balise div. Les deux dernières balises de script contiennent des liens vers les fichiers JavaScript que nous utiliserons pour implémenter la fonctionnalité de genre Ajouter nouveau. Le */Scripts/chooseGenre.js* fichier n’est fourni pour vous dans le projet, nous allons l’examiner ultérieurement dans ce didacticiel.

Exécutez l’application et cliquez sur le **ajouter un nouveau Genre** bouton. Dans le **Genre ajouter** boîte de dialogue, entrez **Opera** dans le **nom** zone d’entrée.

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image4.png)

Cliquez sur le bouton **Enregistrer**. Un appel AJAX crée la catégorie Opera puis remplit la liste déroulante avec Opera et définit Opera comme le genre sélectionné.

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image5.png)

Entrez un artiste, le titre et le prix, puis sélectionnez le **créer** bouton. Si vous entrez un prix inférieur à $8,99, le nouvel album s’affiche en haut de la vue Index. Vérifiez que la nouvelle entrée d’album a été enregistrée dans la base de données.

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image6.png)

Essayez de créer un nouveau genre avec uniquement une lettre. Le code suivant dans le *Models\Genre.cs* fichier définit la longueur minimale et maximale du nom du genre.

[!code-csharp[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample4.cs)]

Validation côté client signale que vous devez entrer une chaîne entre 2 et 20 caractères.

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image7.png)

### <a name="examining-how-a-new-genre-is-added-to-the-database-and-the-select-list"></a>Examiner comment un Genre nouveau est ajouté à la base de données et de la liste Select.

Ouvrez le *Scripts\chooseGenre.js* de fichiers et examinez le code.

[!code-javascript[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample5.js)]

La deuxième ligne utilise l’ID `genreDialog` pour créer une boîte de dialogue de la balise div dans le *Views\StoreManager\\_ChooseGenre.cshtml* fichier. La plupart des paramètres nommés est explicites. Le `autoOpen` paramètre est défini sur false, en sélectionnant le **créer un Genre** bouton permet d’ouvrir la boîte de dialogue explicitement (cette opération est décrite ce dernier sur). La boîte de dialogue a deux boutons, **enregistrer** et **Annuler**. Le **Annuler** bouton ferme la boîte de dialogue. Le code suivant illustre la **enregistrer** bouton de fonction.

[!code-javascript[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample6.js)]

Le `var createGenreForm` est sélectionné dans le `createGenreForm` ID. Le `createGenreForm` ID a été défini dans le code suivant est trouvé dans le *Views\Genre\\_CreateGenre.cshtml* fichier.

[!code-cshtml[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample7.cshtml)]

Le [Html.BeginForm](https://msdn.microsoft.com/library/dd492714.aspx) surcharge d’assistance utilisée dans le *Views\Genre\\_CreateGenre.cshtml* fichier génère du code HTML avec un attribut d’action contenant l’URL pour envoyer le formulaire. Vous pouvez le voir en affichant la page d’album de créer dans un navigateur et en sélectionnant show source dans le navigateur. Le balisage suivant montre le code HTML généré, qui contient la balise de formulaire.

[!code-html[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample8.html)]

Le jQuery `$.post` ligne effectue un appel AJAX à l’attribut action (`/StoreManager/Create`), puis transmet les données à partir de la **créer un Genre** boîte de dialogue. Les données se composent du nom pour le genre de nouveau et une description facultative. Si l’appel AJAX est réussie, le nouveau nom du genre et la valeur sont ajoutés à la balise et le nouveau genre est défini sur la valeur sélectionnée. Comme il s’agit de balise générée dynamiquement, vous ne voyez la nouvelle option select en affichant la source dans le navigateur. Vous pouvez voir le nouveau code HTML avec les outils de développement Internet Explorer 9 F12. Pour afficher la nouvelle option Sélectionnez, dans Internet Explorer 9, appuyez sur la touche F12 pour démarrer les outils de développement F12. Accédez à la page Créer et ajouter un nouveau genre pour le nouveau genre est sélectionné dans la liste de sélection de genre. Dans les outils de développement F12 :

1. Sélectionnez l’onglet HTML.
2. Appuyez sur l’icône d’actualisation.  
    ![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image8.png)
3. Dans la zone de recherche, entrez GenreID.
4. À l’aide de l’icône suivante,   
    ![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image9.png)  
   Accédez à la balise de sélection suivante :

    [!code-html[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample9.html)]
5. Développez la dernière valeur d’option.

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image10.png)

Le code suivant dans le *Scripts\chooseGenre.js* fichier illustre la façon dont le **ajouter un nouveau Genre** bouton se connecte à l’événement click et comment la **ajouter un nouveau Genre** boîte de dialogue est créé.

[!code-javascript[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample10.js)]

La première ligne crée une fonction de clic attachée à la **ajouter un nouveau Genre** bouton. Le balisage suivant à partir de la Views\StoreManager\\_ChooseGenre.cshtml fichier montre comment la **ajouter un nouveau Genre** bouton est créé :

[!code-cshtml[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample11.cshtml)]

La méthode load crée et ouvre la boîte de dialogue Ajouter un Genre et appelle le jQuery `parse` méthode afin de la validation côté client se produit sur les données entrées dans la boîte de dialogue.

Dans cette section, vous avez appris à créer une boîte de dialogue qui peut être utilisé pour ajouter de nouvelles données de catégorie à une liste de sélection. Vous pouvez suivre la même procédure pour créer l’interface utilisateur pour ajouter un nouvel artiste à la liste de sélection artiste. Ce didacticiel vous a apporté une vue d’ensemble de l’utilisation du programme d’assistance ASP.NET MVC HTML **DropDownList**. Pour plus d’informations sur l’utilisation de la **DropDownList**, consultez la section Références addition ci-dessous. Faites-nous savoir si ce didacticiel a été utile.

Rick.Anderson[at]Microsoft.com

### <a name="additional-references"></a>Références supplémentaires

- [ASP.NET MVC – Cascading de liste déroulante répertorie didacticiel](https://weblogs.asp.net/raduenuca/archive/2011/03/06/asp-net-mvc-cascading-dropdown-lists-tutorial-part-1-defining-the-problem-and-the-context.aspx) par [Radu Enuca](https://weblogs.asp.net/raduenuca/default.aspx)
- [Choisi](http://harvesthq.github.com/chosen/) un plug-in d’un script JavaScript qui prennent en charge la sélection multiple et de filtrage.

### <a name="contributors"></a>Contributors

- [Radu Enuca](https://weblogs.asp.net/raduenuca/default.aspx)
- Jean-Sébastien Goupil
- [Brad Wilson.](http://bradwilson.typepad.com/)

### <a name="reviewers"></a>Réviseurs

- Jean-Sébastien Goupil
- [Brad Wilson.](http://bradwilson.typepad.com/)
- Mike Pope
- Tom Dykstra

> [!div class="step-by-step"]
> [Précédent](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper.md)
