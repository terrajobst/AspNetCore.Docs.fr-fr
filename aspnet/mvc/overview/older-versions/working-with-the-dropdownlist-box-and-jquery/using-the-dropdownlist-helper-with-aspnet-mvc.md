---
uid: mvc/overview/older-versions/working-with-the-dropdownlist-box-and-jquery/using-the-dropdownlist-helper-with-aspnet-mvc
title: Utilisation du DropDownList Helper avec ASP.NET MVC | Microsoft Docs
author: Rick-Anderson
description: ''
ms.author: aspnetcontent
ms.date: 01/12/2012
ms.assetid: 53767e05-c8ab-42e1-a94b-22d906195200
msc.legacyurl: /mvc/overview/older-versions/working-with-the-dropdownlist-box-and-jquery/using-the-dropdownlist-helper-with-aspnet-mvc
msc.type: authoredcontent
ms.openlocfilehash: 6dbffe715990de5c0b3b834e354379e414925816
ms.sourcegitcommit: 8f8924ce4eb9effeaf489f177fb01b66867da16f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/24/2018
ms.locfileid: "39219054"
---
<a name="using-the-dropdownlist-helper-with-aspnet-mvc"></a>Utilisation du DropDownList Helper avec ASP.NET MVC
====================
par [Rick Anderson](https://github.com/Rick-Anderson)

Ce didacticiel vous apprend les notions de base de l’utilisation de la [DropDownList](https://msdn.microsoft.com/library/dd492948.aspx) helper et [ListBox](https://msdn.microsoft.com/library/system.web.mvc.html.selectextensions.listbox.aspx) helper dans une application Web ASP.NET MVC. Vous pouvez utiliser Microsoft Visual Web Developer 2010 Express Service Pack 1, qui est une version gratuite de Microsoft Visual Studio pour suivre le didacticiel. Avant de commencer, assurez-vous que vous avez installé les composants requis listés ci-dessous. Vous pouvez installer tous les en cliquant sur le lien suivant : [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). Vous pouvez également installer individuellement les conditions préalables à l’aide des liens suivants :

- [Prérequis pour le Visual Studio Web Developer Express SP1](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack) <a id="post"></a>
- [Mettre à jour des outils ASP.NET MVC 3](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
- [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(runtime + outils prennent en charge)

Si vous utilisez Visual Studio 2010 au lieu de Visual Web Developer 2010, installez les conditions préalables en cliquant sur le lien suivant : [configuration requise de Visual Studio 2010](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack). Ce didacticiel suppose que vous avez terminé la [Introduction à ASP.NET MVC](../getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) didacticiel ou[ASP.NET MVC Music Store](../mvc-music-store/mvc-music-store-part-1.md) didacticiel ou que vous êtes familiarisé avec le développement ASP.NET MVC. Ce didacticiel commence par un projet modifié à partir de la [ASP.NET MVC Music Store](../mvc-music-store/mvc-music-store-part-1.md) didacticiel. Vous pouvez télécharger le projet de démarrage avec le lien suivant [télécharger la version c#](https://archive.msdn.microsoft.com/Project/Download/FileDownload.aspx?ProjectName=aspnetmvcsamples&amp;DownloadId=15829).

Un projet de Visual Web Developer avec la fin du didacticiel code source c# est disponible pour accompagner cette rubrique. [Télécharger](https://code.msdn.microsoft.com/Using-the-DropDownList-67f9367d).

### <a name="what-youll-build"></a>Ce que vous allez générer

Vous allez créer des méthodes d’action et des vues qui utilisent le [DropDownList](https://msdn.microsoft.com/library/system.web.mvc.html.selectextensions.dropdownlist.aspx) helper pour sélectionner une catégorie. Vous utiliserez également **jQuery** pour ajouter une boîte de dialogue Insérer catégorie qui peut être utilisé lorsqu’une nouvelle catégorie (par exemple, le genre ou artiste) est nécessaire. Voici une capture d’écran de la vue Create affichant des liaisons pour ajouter un nouveau genre et ajouter un nouvel artiste.

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image1.png)

### <a name="skills-youll-learn"></a>Vous allez apprendre des compétences

Voici ce que vous allez apprendre :

- Comment utiliser le [DropDownList](https://msdn.microsoft.com/library/system.web.mvc.html.selectextensions.dropdownlist.aspx) helper pour sélectionner les données de catégorie.
- Comment ajouter un **jQuery** boîte de dialogue pour ajouter de nouvelles catégories.

### <a name="getting-started"></a>Prise en main

Commencez par télécharger le projet de démarrage avec le lien suivant, [télécharger](https://archive.msdn.microsoft.com/Project/Download/FileDownload.aspx?ProjectName=aspnetmvcsamples&amp;DownloadId=15829). Dans l’Explorateur Windows, avec le bouton droit, cliquez sur le *DDL\_Starter.zip* fichier et sélectionnez Propriétés. Dans le **DDL\_Starter.zip propriétés** boîte de dialogue, sélectionnez débloquer.

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image2.png)

Cliquez avec le bouton droit sur le DDL\_Starter.zip fichier, puis sélectionnez **extraire tout** pour décompresser le fichier. Ouvrez le *StartMusicStore.sln* fichier avec Visual Web Developer 2010 Express (« Visual Web Developer » ou « VWD » pour faire plus court) ou Visual Studio 2010.

Appuyez sur CTRL + F5 pour exécuter l’application et cliquez sur le **Test** lien.

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image3.png)

Sélectionnez le **sélectionner une catégorie de film (Simple)** lien. Une liste de films Type Sélectionnez s’affiche, avec comédie la valeur sélectionnée.

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image4.png)

Cliquez avec le bouton droit dans le navigateur et sélectionnez Afficher la source. Le code HTML de la page s’affiche. Le code suivant montre le code HTML de l’élément select.

[!code-html[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample1.html)]

Vous pouvez voir que chaque élément dans la liste de sélection a une valeur (0 pour l’Action, 1 pour les pièces de théâtre, comédie 2 et 3 pour science-fiction) et un nom d’affichage (Action, pièces de théâtre, comédie et de science-fiction). Le code ci-dessus est le code HTML standard pour une liste de sélection.

Modifier la liste de sélection pour les pièces de théâtre et appuyez sur la **envoyer** bouton. L’URL dans le navigateur est `http://localhost:2468/Home/CategoryChosen?MovieType=1` et affiche la page **que vous avez sélectionné : 1**.

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image5.png)

Ouvrez le *Controllers\HomeController.cs* de fichiers et d’examiner le `SelectCategory` (méthode).

[!code-csharp[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample2.cs)]

Le [DropDownList](https://msdn.microsoft.com/library/dd492738.aspx) helper utilisé pour créer une liste de sélection HTML nécessite un **IEnumerable&lt;SelectListItem &gt;** , explicitement ou implicitement. Autrement dit, vous pouvez passer le **IEnumerable&lt;SelectListItem &gt;**  explicitement à la [DropDownList](https://msdn.microsoft.com/library/dd492738.aspx) helper, ou vous pouvez ajouter la **IEnumerable&lt; SelectListItem &gt;**  à la [ViewBag](https://blogs.msdn.com/b/rickandy/archive/2011/01/28/dynamic-v-strongly-typed-views.aspx) en utilisant le même nom pour le **SelectListItem** en tant que la propriété de modèle. En passant le **SelectListItem** implicitement et explicitement sont couvertes dans la partie suivante du didacticiel. Le code ci-dessus montre la façon la plus simple possible pour créer un **IEnumerable&lt;SelectListItem &gt;**  et le remplir avec les valeurs et le texte. Remarque la `Comedy` [SelectListItem](https://msdn.microsoft.com/library/system.web.mvc.selectlistitem.aspx) a le [sélectionnés](https://msdn.microsoft.com/library/system.web.mvc.selectlistitem.selected.aspx) propriété définie sur **true ;** cela entraînera la liste de sélection de rendu afficher **comédie** en tant que l’élément sélectionné dans la liste.

Le **IEnumerable&lt;SelectListItem &gt;**  créé ci-dessus est ajouté à la [ViewBag](https://blogs.msdn.com/b/rickandy/archive/2011/01/28/dynamic-v-strongly-typed-views.aspx) portant le nom MovieType. Voici comment nous transmettons le **IEnumerable&lt;SelectListItem &gt;**  implicitement à la [DropDownList](https://msdn.microsoft.com/library/dd492738.aspx) helper indiqué ci-dessous.

Ouvrez le *Views\Home\SelectCategory.cshtml* de fichiers et d’examiner le balisage.

[!code-cshtml[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample3.cshtml)]

Sur la troisième ligne, nous définissons la disposition sur Views/Shared/\_Simple\_Layout.cshtml, qui est une version simplifiée du fichier de mise en forme standard. Nous procéder ainsi pour que l’affichage et le rendu HTML simple.

Dans cet exemple nous pas modifions l’état de l’application, afin de nous envoie les données à l’aide un **HTTP GET**, et non **HTTP POST**. Consultez la section W3C [liste de vérification rapide pour le choix de HTTP GET ou POST](http://www.w3.org/2001/tag/doc/whenToUseGet.html#checklist). Étant donné que nous n'utilisons pas modifier l’application et de l’écran de validation, nous utilisons le [Html.BeginForm](https://msdn.microsoft.com/library/dd460344.aspx) surcharge qui nous permet de spécifier la méthode d’action, la méthode de contrôleur et du formulaire (**HTTP POST** ou **HTTP GET**). Vues contiennent généralement les [Html.BeginForm](https://msdn.microsoft.com/library/dd505244.aspx) surcharge qui n’accepte aucun paramètre. Aucune version de paramètre par défaut pour les données du formulaire vers la version POST de la même méthode d’action et le contrôleur de validation.

La ligne suivante

[!code-cshtml[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample4.cshtml)]

passe un argument de chaîne pour le **DropDownList** helper. Cette chaîne, « MovieType » dans notre exemple, effectue deux opérations :

- Il fournit la clé pour le **DropDownList** helper pour rechercher un **IEnumerable&lt;SelectListItem &gt;**  dans le **ViewBag**.
- Il est lié aux données à l’élément de formulaire MovieType. Si la méthode de soumission est **HTTP GET**, `MovieType` sera une chaîne de requête. Si la méthode de soumission est **HTTP POST**, `MovieType` sera ajoutée dans le corps du message. L’illustration suivante montre la chaîne de requête avec la valeur 1.

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image6.png)

Le code suivant illustre la `CategoryChosen` méthode le formulaire a été envoyé à.

[!code-csharp[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample5.cs)]

Revenez à la page de test et sélectionnez le **HTML SelectList** lien. La page HTML restitue un élément select similaire à la page de test ASP.NET MVC simple. Cliquez avec le bouton droit sur la fenêtre du navigateur et sélectionnez **afficher la source**. Le balisage HTML de la liste de sélection est fondamentalement identique. Page de test, le code HTML, il fonctionne comme la méthode d’action ASP.NET MVC et la vue que précédemment, nous avons testé.

### <a name="improving-the-movie-select-list-with-enums"></a>Amélioration de la liste de sélection de film avec les Enums

Si les catégories dans votre application sont fixes et ne change pas, vous pouvez tirer parti des enums pour rendre votre code plus robuste et plus simple à étendre. Lorsque vous ajoutez une nouvelle catégorie, la valeur de la catégorie appropriée est générée. Le permet d’éviter les erreurs de copier et coller lorsque vous ajoutez une nouvelle catégorie mais que vous oubliez de mettre à jour la valeur de catégorie.

Ouvrez le *Controllers\HomeController.cs* de fichiers et d’examiner le code suivant :

[!code-csharp[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample6.cs)]

Le [enum](https://msdn.microsoft.com/library/sbbt4032(VS.80).aspx) `eMovieCategories` capture les types de quatre films. Le `SetViewBagMovieType` méthode crée le **IEnumerable&lt;SelectListItem &gt;**  à partir de la `eMovieCategories` **enum**et définit le `Selected` propriété à partir de la `selectedMovie` paramètre. Le `SelectCategoryEnum` méthode d’action utilise la même vue en tant que le `SelectCategory` méthode d’action.

Accédez à la page de Test, puis cliquez sur le `Select Movie Category (Enum)` lien. Cette fois, au lieu d’une valeur (nombre) qui est affichée, une chaîne qui représente l’énumération s’affiche.

### <a name="posting-enum-values"></a>Validation des valeurs Enum

Les formulaires HTML sont généralement utilisés pour publier des données sur le serveur. Le code suivant illustre la `HTTP GET` et `HTTP POST` versions de la `SelectCategoryEnumPost` (méthode).

[!code-csharp[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample7.cs)]

En passant un `eMovieCategories` enum pour le `POST` (méthode), nous pouvons extraire la valeur d’énumération et la chaîne de l’enum. Exécuter l’exemple et accédez à la page de Test. Cliquez sur le `Select Movie Category(Enum Post)` lien. Sélectionnez un type de film, puis appuyez sur le bouton Envoyer. L’affichage indique la valeur et le nom du type de film.

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image7.png)

### <a name="creating-a-multiple-section-select-element"></a>Création d’un élément Select Section plusieurs

Le [ListBox](https://msdn.microsoft.com/library/system.web.mvc.html.selectextensions.listbox.aspx) HTML helper restitue le code HTML `<select>` élément avec la `multiple` attribut, qui permet aux utilisateurs d’effectuer des sélections multiples. Accédez au lien de Test, puis sélectionnez le **Multi sélection du pays** lien. L’interface utilisateur de rendu vous permet de sélectionner plusieurs pays. Dans l’image ci-dessous, au Canada et en Chine sont sélectionnés.

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image8.png)

### <a name="examining-the-multiselectcountry-code"></a>Examen du Code MultiSelectCountry

Examinez le code suivant à partir de la *Controllers\HomeController.cs* fichier.

[!code-csharp[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample8.cs)]

Le `GetCountries` méthode crée une liste de pays, puis passe à la `MultiSelectList` constructeur. Le `MultiSelectList` surcharge de constructeur utilisée dans le `GetCountries` ci-dessus de la méthode prend quatre paramètres :

[!code-csharp[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample9.cs)]

1. *éléments*: un [IEnumerable](https://msdn.microsoft.com/library/system.collections.ienumerable.aspx) contenant les éléments dans la liste. Dans l’exemple ci-dessus, la liste de pays.
2. *dataValueField*: le nom de la propriété dans le **IEnumerable** liste qui contient la valeur. Dans l’exemple ci-dessus, le `ID` propriété.
3. *dataTextField*: le nom de la propriété dans le **IEnumerable** liste qui contient les informations à afficher. Dans l’exemple ci-dessus, le `name` propriété.
4. *selectedValues*: la liste des valeurs sélectionnées.

Dans l’exemple ci-dessus, le `MultiSelectCountry` méthode passe un `null` valeur pour les pays sélectionnés, afin de ne sélectionner aucune pays lors de l’interface utilisateur s’affiche. Le code suivant montre le balisage Razor utilisé pour restituer le `MultiSelectCountry` vue.

[!code-cshtml[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample10.cshtml)]

Le programme d’assistance HTML [ListBox](https://msdn.microsoft.com/library/dd470200.aspx) méthode utilisée ci-avant prennent deux paramètres, le nom de la propriété à la liaison de modèle et le [MultiSelectList](https://msdn.microsoft.com/library/system.web.mvc.multiselectlist.aspx) contenant les options select et les valeurs. Le `ViewBag.YouSelected` code ci-dessus est utilisé pour afficher les valeurs des pays que vous avez sélectionné lorsque vous envoyez le formulaire. Examiner la surcharge de la requête HTTP POST de la `MultiSelectCountry` (méthode).

[!code-csharp[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample11.cs)]

Le `ViewBag.YouSelected` propriété dynamique contient les pays sélectionnés, obtenus pour la `Countries` entrée dans la collection de formulaires. Dans cette version, la méthode GetCountries est passée une liste des pays sélectionnés, par conséquent, lorsque le `MultiSelectCountry` s’affiche, les pays sélectionnés sont sélectionnés dans l’interface utilisateur.

### <a name="making-a-select-element-friendly-with-the-harvest-chosen-jquery-plugin"></a>Rendre un sélectionnez élément convivial avec le plug-in de jQuery Harvest choisi

La récolte [choisi](http://harvesthq.github.com/chosen/) plug-in jQuery peut être ajouté à un élément HTML &lt;sélectionnez&gt; élément à créer un utilisateur de l’interface utilisateur convivial. Les images ci-dessous montrent la récolte [choisi](http://harvesthq.github.com/chosen/) plug-in jQuery avec `MultiSelectCountry` vue.

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image9.png)

Dans les deux images ci-dessous, **Canada** est sélectionné.

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image10.png)

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image11.png)

Dans l’image ci-dessus, Canada est sélectionné, et il contient un **x** vous pouvez cliquer pour supprimer la sélection. L’image ci-dessous montre le Canada, Chine, et Japon sélectionné.

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image12.png)

### <a name="hooking-up-the-harvest-chosen-jquery-plugin"></a>Raccorder le plug-in de jQuery Harvest choisi

La section suivante est plus facile à suivre si vous êtes familiarisé avec jQuery. Si vous n’avez jamais utilisé jQuery avant, vous souhaiterez essayez l’une des didacticiels suivants jQuery.

- [Fonctionne de jQuery](http://docs.jquery.com/Tutorials:How_jQuery_Works) par [John Resig](http://ejohn.org/)
- [Mise en route avec jQuery](http://docs.jquery.com/Tutorials:Getting_Started_with_jQuery) par [Jörn Zaefferer](http://bassistance.de/)
- [Exemples de jQuery de Live](http://codylindley.com/blogstuff/js/jquery/#) par [Cody Lindley](http://codylindley.com/)

Le plug-in sélectionné est inclus dans le Kit de démarrage et les projets d’exemple terminé qui accompagnent ce didacticiel. Pour ce didacticiel, vous devez uniquement utiliser jQuery pour raccorder à l’interface utilisateur. Pour utiliser le plug-in jQuery de Harvest choisi dans un projet ASP.NET MVC, vous devez :

1. Télécharger le plug-in sélectionné à partir de [github](https://github.com/harvesthq/chosen/). Cette étape a été effectuée pour vous.
2. Ajouter le dossier choisi à votre projet ASP.NET MVC. Ajouter les ressources à partir du plug-in choisie que vous avez téléchargé à l’étape précédente pour le dossier choisi. Cette étape a été effectuée pour vous.
3. Raccorder le plug-in choisi pour le **DropDownList** ou **ListBox** programme d’assistance HTML.

### <a name="hooking-up-the-chosen-plugin-to-the-multiselectcountry-view"></a>Raccorder le plug-in sélectionné à la vue MultiSelectCountry.

Ouvrez le *Views\Home\MultiSelectCountry.cshtml* fichier, puis ajoutez un `htmlAttributes` paramètre à la `Html.ListBox`. Le paramètre que vous allez ajouter contient un nom de classe pour la liste de sélection (`@class = "chzn-select"`). Le code complet est indiqué ci-dessous :

[!code-cshtml[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample12.cshtml)]

Dans le code ci-dessus, nous avons ajouté l’attribut HTML et la valeur d’attribut `class = "chzn-select"`. Le \@ caractère classe précédente n’a rien à voir avec le moteur d’affichage Razor. `class` est un [mot clé c#](https://msdn.microsoft.com/library/x53a06bb.aspx). Mots clés c# ne peuvent pas être utilisés comme identificateurs, sauf s’ils incluent \@ comme préfixe. Dans l’exemple ci-dessus, `@class` est un identificateur valide mais **classe** n’est pas car **classe** est un mot clé.

Ajoutez des références à la *Chosen/chosen.jquery.js* et *Chosen/chosen.css* fichiers. Le *Chosen/chosen.jquery.js* et implémente le fonctionnellement du plug-in sélectionné. Le *Chosen/chosen.css* fichier fournit le style. Ajoutez ces références au bas de la *Views\Home\MultiSelectCountry.cshtml* fichier. Le code suivant montre comment référencer le plug-in sélectionné.

[!code-cshtml[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample13.cshtml)]

Activer le plug-in sélectionné à l’aide du nom de classe utilisé dans le **Html.ListBox** code. Dans l’exemple ci-dessus, le nom de classe est `chzn-select`. Ajoutez la ligne suivante au bas de la *Views\Home\MultiSelectCountry.cshtml* fichier d’affichage. Cette ligne active le plug-in sélectionné.

[!code-html[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample14.html)]

La ligne suivante est la syntaxe pour appeler la fonction ready jQuery, qui sélectionne l’élément DOM avec le nom de la classe `chzn-select`.

[!code-powershell[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample15.ps1)]

Encapsulé définie alors retourné à partir de l’appel ci-dessus s’applique la méthode choisie (`.chosen();`), qui raccorde le plug-in sélectionné.

Le code suivant montre le terminé *Views\Home\MultiSelectCountry.cshtml* fichier d’affichage.

[!code-cshtml[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample16.cshtml)]

Exécutez l’application et accédez à la `MultiSelectCountry` vue. Essayez d’ajouter et supprimer des pays. Téléchargement de l’exemple fourni contient également un `MultiCountryVM` (méthode) et la vue qui implémente les fonctionnalités de MultiSelectCountry à l’aide d’une vue de modèle au lieu d’un **ViewBag**.

Dans la section suivante, vous verrez comment le mécanisme de génération de modèles automatique ASP.NET MVC fonctionne avec le **DropDownList** helper.

> [!div class="step-by-step"]
> [Next](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper.md)
