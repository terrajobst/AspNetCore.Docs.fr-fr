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
ms.openlocfilehash: ab45a14c4eda9e7552af4831851396af3c13dce1
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41835041"
---
<a name="examining--how--aspnet-mvc-scaffolds-the-dropdownlist-helper"></a>Examiner la façon dont ASP.NET MVC structure du programme d’assistance DropDownList
====================
par [Rick Anderson](https://github.com/Rick-Anderson)

Dans **l’Explorateur de solutions**, cliquez sur le *contrôleurs* dossier, puis sélectionnez **ajouter un contrôleur**. Nommez le contrôleur **StoreManagerController**. Définir les options de la **ajouter un contrôleur** boîte de dialogue comme illustré dans l’image ci-dessous.

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image1.png)

Modifier le *StoreManager\Index.cshtml* afficher et supprimer `AlbumArtUrl`. Suppression `AlbumArtUrl` améliorer la lisibilité de la présentation. Le code terminé est indiqué ci-dessous.

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample1.cshtml)]

Ouvrez le *Controllers\StoreManagerController.cs* de fichiers et de trouver le `Index` (méthode). Ajouter le `OrderBy` afin que les albums sont triés par tarif. Le code complet est indiqué ci-dessous.

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample2.cs)]

Tri par prix rend plus facile de tester les modifications apportées à la base de données. Lorsque vous testez le modifier et créez des méthodes, vous pouvez utiliser un peu onéreuse pour les données enregistrées seront affichent en premier.

Ouvrez le *StoreManager\Edit.cshtml* fichier. Ajoutez la ligne suivante juste après la balise de légende.

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample3.cshtml)]

Le code suivant affiche le contexte de cette modification :

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample4.cshtml)]

Le `AlbumId` est nécessaire pour apporter des modifications à un enregistrement de l’album.

Appuyez sur CTRL+F5 pour exécuter l'application. Sélectionnez cette option pour le **administrateur** lier, puis sélectionnez le **créer un nouveau** lien pour créer un nouvel album. Vérifiez que le informations concernant l’album a été enregistré. Modifier un album et vérifiez les modifications apportées sont conservées.

### <a name="the-album-schema"></a>Le schéma d’Album

Le `StoreManager` contrôleur créé par le mécanisme de génération de modèles automatique MVC permet un accès CRUD (Create, Read, Update, Delete) aux albums dans la base de données du magasin de musique. Le schéma pour les informations concernant l’album est présenté ci-dessous :

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image2.png)

Le `Albums` table ne stocke pas le genre d’album et la description, il stocke une clé étrangère vers le `Genres` table. Le `Genres` table contient le nom du genre et la description. De même, le `Albums` table ne contient pas le nom de l’album artistes, mais une clé étrangère vers le `Artists` table. Le `Artists` table contient le nom de l’artiste. Si vous examinez les données dans le `Albums` table, vous pouvez voir chaque ligne contient une clé étrangère à la `Genres` table et une clé étrangère vers le `Artists` table. L’image ci-dessous affiche des données de table à partir de la `Albums` table.

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image3.png)

### <a name="the-html-select-tag"></a>La balise de sélection HTML

Le code HTML `<select>` élément (créé par le code HTML [DropDownList](https://msdn.microsoft.com/library/dd492948.aspx) helper) est utilisé pour afficher une liste complète des valeurs (par exemple, la liste des genres). Pour les formulaires de modification, lorsque la valeur actuelle est connue, la liste de sélection peut afficher la valeur actuelle. Nous l’avons vu ce précédemment lorsque nous définissons la valeur sélectionnée **comédie**. La liste de sélection est idéale pour l’affichage des catégories ou des données de clé étrangère. Le `<select>` élément pour la clé étrangère Genre affiche la liste des noms de genre possible, mais lorsque vous enregistrez le formulaire la propriété Genre est mis à jour avec la valeur de clé étrangère Genre, pas le nom du genre affichée. Dans l’image ci-dessous, le genre sélectionné est **Disco** et est l’artiste **Donna Summer**.

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image4.png)

### <a name="examining-the-aspnet-mvc-scaffolded-code"></a>Examen d’ASP.NET MVC structurée de Code

Ouvrez le *Controllers\StoreManagerController.cs* de fichiers et de trouver le `HTTP GET Create` (méthode).

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample5.cs)]

Le `Create` méthode ajoute deux [SelectList](https://msdn.microsoft.com/library/system.web.mvc.selectlist.aspx) des objets sur le `ViewBag`, un pour contenir les informations de genre et un pour contenir les informations de l’artiste. Le [SelectList](https://msdn.microsoft.com/library/dd505286.aspx) surcharge de constructeur ci-dessus prend trois arguments :

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample6.cs)]

1. *éléments*: un [IEnumerable](https://msdn.microsoft.com/library/system.collections.ienumerable.aspx) contenant les éléments dans la liste. Dans l’exemple ci-dessus, la liste des genres retourné par `db.Genres`.
2. *dataValueField*: le nom de la propriété dans le **IEnumerable** liste qui contient la valeur de clé. Dans l’exemple ci-dessus, `GenreId` et `ArtistId`.
3. *dataTextField*: le nom de la propriété dans le **IEnumerable** liste qui contient les informations à afficher. Dans les artistes et la table de genre, le `name` champ est utilisé.

Ouvrez le *Views\StoreManager\Create.cshtml* de fichiers et d’examiner le `Html.DropDownList` balisage d’assistance pour le champ de genre.

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample7.cshtml)]

La première ligne indique que la vue create prend un `Album` modèle. Dans le `Create` méthode illustrée ci-dessus, aucun modèle n’a été passé, donc la vue obtient un **null** `Album` modèle. À ce stade, nous allons créer un nouvel album donc nous n’avez aucun `Album` données pour celui-ci.

Le [Html.DropDownList](https://msdn.microsoft.com/library/dd492948.aspx) surcharge ci-dessus prend le nom du champ à lier au modèle. Il utilise également ce nom pour rechercher un **ViewBag** objet contenant un [SelectList](https://msdn.microsoft.com/library/dd505286.aspx) objet. À l’aide de cette surcharge, vous êtes amené à nom le **ViewBag SelectList** objet `GenreId`. Le deuxième paramètre (`String.Empty`) est le texte à afficher lorsque aucun élément n’est sélectionné. C’est exactement ce que nous voulons lors de la création d’un nouvel album. Si vous supprimé le deuxième paramètre et utilisez le code suivant :

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample8.cshtml)]

La liste de sélection est par défaut pour le premier élément, ou Rock dans notre exemple.

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image5.png)

Examen du `HTTP POST Create` (méthode).

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample9.cs)]

Cette surcharge de la `Create` méthode prend un `album` objet, créé par le système de liaison de modèle ASP.NET MVC à partir des valeurs de formulaire publiées. Lorsque vous envoyez un nouvel album, si l’état du modèle est valide et il n’existe aucune erreur de base de données, le nouvel album est ajouté à la base de données. L’image suivante illustre la création d’un nouvel album.

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image6.png)

Vous pouvez utiliser la [outil fiddler](http://www.fiddler2.com/fiddler2/) pour examiner les valeurs de formulaire publiées cette liaison de modèle ASP.NET MVC utilise pour créer l’objet de l’album.

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image7.png).

### <a name="refactoring-the-viewbag-selectlist-creation"></a>Refactorisation de la création de ViewBag SelectList

Les deux le `Edit` méthodes et les `HTTP POST Create` méthode avoir un code identique pour configurer le **SelectList** dans le **ViewBag**. Dans l’esprit de [DRY](http://en.wikipedia.org/wiki/Don't_repeat_yourself), nous devez refactoriser ce code. Nous allons rendre l’utilisation de ce refactorisé le code plus tard.

Créez une nouvelle méthode pour ajouter un genre et un artiste **SelectList** à la **ViewBag**.

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample10.cs)]

Remplacez les deux lignes définissant le `ViewBag` dans chacune de la `Create` et `Edit` méthodes avec un appel à la `SetGenreArtistViewBag` (méthode). Le code terminé est indiqué ci-dessous.

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample11.cs)]

Créer un nouvel album et modifier un album pour vérifier que les modifications fonctionnent.

### <a name="explicitly-passing-the-selectlist-to-the-dropdownlist"></a>En fournissant explicitement le SelectList au contrôle DropDownList

Les vues create et edit créée à l’aide de la génération de modèles automatique ASP.NET MVC les **DropDownList** surcharge :

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample12.cs)]

Le `DropDownList` balisage de la vue create est indiqué ci-dessous.

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample13.cshtml)]

Étant donné que le `ViewBag` propriété pour le `SelectList` est nommé `GenreId`, le **DropDownList** helper utilisera le `GenreId` **SelectList** dans le **ViewBag** . Dans l’exemple suivant **DropDownList** de surcharge, le `SelectList` soit passée explicitement dans.

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample14.cs)]

Ouvrez le *Views\StoreManager\Edit.cshtml* du fichier et changer le **DropDownList** appel à transmettre explicitement le **SelectList**, à l’aide de la surcharge ci-dessus. Cela pour la catégorie de Genre. Le code complet est indiqué ci-dessous :

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample15.cshtml)]

Exécutez l’application et cliquez sur le **administrateur** lier, puis accédez à un album de Jazz et sélectionnez le **modifier** lien.

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image8.png)

Au lieu d’afficher Jazz en tant que le genre actuellement sélectionné, comme le ROC est affiché. Lorsque l’argument de chaîne (la propriété à lier) et le **SelectList** objet portent le même nom, la valeur sélectionnée n’est pas utilisée. Lorsqu’il n’existe aucune valeur sélectionnée est fourni, les navigateurs par défaut vers le premier élément dans le **SelectList**(c'est-à-dire **Rock** dans l’exemple ci-dessus). Il s’agit d’une limitation connue de la **DropDownList** helper.

Ouvrez le *Controllers\StoreManagerController.cs* du fichier et changer la **SelectList** noms à l’objet `Genres` et `Artists`. Le code complet est indiqué ci-dessous :

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample16.cs)]

Les noms Genres et artistes sont meilleurs noms pour les catégories, car ils contiennent plus de simplement l’ID de chaque catégorie. La refactorisation, que nous l’avons fait précédemment porté ses fruits. Au lieu de modifier le **ViewBag** dans les quatre méthodes, nos modifications étaient isolées dans le `SetGenreArtistViewBag` (méthode).

Modifier le **DropDownList** appeler dans la créer et modifier des affichages pour utiliser la nouvelle **SelectList** noms. Le balisage pour le mode édition est indiqué ci-dessous :

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample17.cshtml)]

La vue Create nécessite une chaîne vide pour le premier élément dans le SelectList empêcher l’affichage.

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample18.cshtml)]

Créer un nouvel album et modifier un album pour vérifier que les modifications fonctionnent. Tester le code de la modifier en sélectionnant un album avec un genre autre que Rock.

### <a name="using-a-view-model-with-the-dropdownlist-helper"></a>À l’aide d’un modèle de vue avec l’application d’assistance DropDownList

Créer une nouvelle classe dans le dossier ViewModels nommé `AlbumSelectListViewModel`. Remplacez le code dans la `AlbumSelectListViewModel` classe par le code suivant :

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample19.cs)]

Le `AlbumSelectListViewModel` constructeur accepte un album, une liste d’artistes et genres et crée un objet contenant l’album et un `SelectList` pour les genres et artistes.

Générez le projet afin que la `AlbumSelectListViewModel` est disponible lorsque nous créons une vue à l’étape suivante.

Ajouter un `EditVM` méthode à la `StoreManagerController`. Le code terminé est indiqué ci-dessous.

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample20.cs)]

Bouton droit sur `AlbumSelectListViewModel`, sélectionnez **résoudre**, puis **à l’aide de MvcMusicStore.ViewModels ;**.

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image9.png)

Vous pouvez également ajouter les éléments suivants à l’aide d’instruction :

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample21.cs)]

Bouton droit sur `EditVM` et sélectionnez **ajouter une vue**. Utilisez les options ci-dessous.

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image10.png)

Sélectionnez **ajouter**, puis remplacez le contenu de la *Views\StoreManager\EditVM.cshtml* fichier par le code suivant :

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample22.cshtml)]

Le `EditVM` balisage est très similaire à l’original `Edit` balisage avec les exceptions suivantes.

- Modéliser les propriétés dans le `Edit` vue sont au format `model.property`(par exemple, `model.Title` ). Modéliser les propriétés dans le `EditVm` vue sont au format `model.Album.property`(par exemple, `model.Album.Title`). C’est parce que le `EditVM` vue est passée à un conteneur un `Album`, et non un `Album` comme dans le `Edit` vue.
- Le **DropDownList** deuxième paramètre est fourni à partir du modèle de vue, pas le **ViewBag**.
- Le **BeginForm** helper dans la `EditVM` vue publie explicitement vers le `Edit` méthode d’action. En publiant revenir à la `Edit` action, nous n’avons pas à écrire un `HTTP POST EditVM` action et vous pouvez réutiliser le `HTTP POST` `Edit` action.

Exécutez l’application et modifier un album. Modifier l’URL à utiliser `EditVM`. Modifier un champ et appuyez sur la **enregistrer** bouton pour vérifier le code fonctionne.

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image11.png)

### <a name="which-approach-should-you-use"></a>Quelle approche devez-vous utiliser ?

Tous les trois approches indiqués sont acceptables. De nombreux développeurs préfèrent à explictily passe le `SelectList` à la `DropDownList` à l’aide de la `ViewBag`. Cette approche présente l’avantage de ce qui vous donne la souplesse d’utilisation d’un nom plus approprié pour la collection. L’inconvénient est que vous ne pouvez pas nommer le `ViewBag SelectList` le même nom que la propriété de modèle d’objet.

Certains développeurs préfèrent l’approche ViewModel. D’autres Examinez le balisage plus détaillé et généré HTML de l’approche ViewModel un inconvénient.

Dans cette section, nous avons appris à l’aide de trois approches le **DropDownList** avec des données de catégorie. Dans la section suivante, nous allons montrer comment ajouter une nouvelle catégorie.

> [!div class="step-by-step"]
> [Précédent](using-the-dropdownlist-helper-with-aspnet-mvc.md)
> [Suivant](adding-a-new-category-to-the-dropdownlist-using-jquery-ui.md)
