---
uid: mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2
title: Utilisation de HTML5 et Datepicker calendrier contextuel jQuery UI avec ASP.NET MVC - partie 2 | Microsoft Docs
author: Rick-Anderson
description: Ce didacticiel vous apprend les notions de base de l’utilisation des modèles de l’éditeur, les modèles d’affichage et le calendrier contextuel jQuery UI datepicker dans une MV ASP.NET...
ms.author: riande
ms.date: 08/29/2011
ms.assetid: 21a178de-4c5a-4211-8a9c-74ec576c0f30
msc.legacyurl: /mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2
msc.type: authoredcontent
ms.openlocfilehash: 9b27ccc6ce26e8266947c531d299ba69bbec4fde
ms.sourcegitcommit: 7b4e3936feacb1a8fcea7802aab3e2ea9c8af5b4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/04/2018
ms.locfileid: "48577195"
---
<a name="using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc---part-2"></a>Utilisation de HTML5 et Datepicker calendrier contextuel jQuery UI avec ASP.NET MVC - partie 2
====================
par [Rick Anderson]((https://twitter.com/RickAndMSFT))

> Ce didacticiel vous apprend les notions de base de l’utilisation des modèles de l’éditeur, les modèles d’affichage et le calendrier contextuel jQuery UI datepicker dans une application Web ASP.NET MVC.


## <a name="adding-an-automatic-datetime-template"></a>Ajout d’un modèle de date/heure automatique

Dans la première partie de ce didacticiel, vous avez vu comment vous pouvez ajouter des attributs au modèle pour spécifier explicitement la mise en forme, et comment vous pouvez spécifier explicitement le modèle qui est utilisé pour restituer le modèle. Par exemple, le [DisplayFormat](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx) attribut dans l’explicitement de code suivant spécifie la mise en forme pour le `ReleaseDate` propriété.

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample1.cs)]

Dans l’exemple suivant, le [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) d’attribut, à l’aide de la `Date` énumération, spécifie que le modèle de date doit être utilisé pour restituer le modèle. S’il n’existe aucun modèle de date dans votre projet, le modèle de dates intégrée est utilisé.

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample2.cs)]

Toutefois, ASP. MVC peut effectuer une correspondance de type à l’aide de la convention-sur-configuration, en recherchant un modèle qui correspond au nom d’un type. Cela vous permet de créer un modèle de données met automatiquement en forme sans utiliser les attributs ou le code du tout. Pour cette partie du didacticiel, vous allez créer un modèle qui est automatiquement appliqué aux propriétés de modèle de type [DateTime](https://msdn.microsoft.com/library/system.datetime.aspx). Vous ne devez utiliser un attribut ou une autre configuration pour spécifier que le modèle doit être utilisé pour afficher toutes les propriétés du modèle de type [DateTime](https://msdn.microsoft.com/library/system.datetime.aspx).

Vous apprendrez également la possibilité de personnaliser l’affichage des propriétés individuelles ou même des champs spécifiques.

Pour commencer, nous allons supprimer les informations de mise en forme existantes et afficher des dates complets dans l’application.

Ouvrir le *Movie.cs* de fichiers et commentez la `DataType` attribut sur le `ReleaseDate` propriété :

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample3.cs)]

Appuyez sur CTRL+F5 pour exécuter l'application.

Notez que le `ReleaseDate` propriété affiche désormais la date et l’heure, car il s’agit de la valeur par défaut lorsque aucune information de mise en forme est fournie.

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/_static/image1.png)

### <a name="adding-css-styles-for-testing-new-templates"></a>Ajout de Styles CSS pour le test des nouveaux modèles

Avant de créer un modèle pour mettre en forme des dates, vous allez ajouter quelques règles de style CSS que vous pouvez appliquer aux nouveaux modèles. Ils vous aideront à vérifier que la page rendue utilise le nouveau modèle.

Ouvrez le *Content\Site.cs*s fichier, puis ajoutez les règles CSS suivantes vers le bas du fichier :

[!code-css[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample4.css)]

### <a name="adding-datetime-display-templates"></a>Ajout de modèles d’affichage de date/heure

Vous pouvez maintenant créer le nouveau modèle. Dans le *Views\Movies* dossier, créez un *DisplayTemplates* dossier.

Dans le *Views\Shared* dossier, créez un *DisplayTemplates* dossier et un *EditorTemplates* dossier.

Les modèles d’affichage dans le *Views\Shared\DisplayTemplates* dossier sera utilisé par tous les contrôleurs. Les modèles d’affichage dans le *Views\Movie\DisplayTemplates* dossier sera utilisé uniquement par le `Movie` contrôleur. (Si un modèle portant le même nom apparaît dans les deux dossiers, le modèle dans le *Views\Movie\DisplayTemplates* dossier, autrement dit, le modèle plus spécifique, est prioritaire pour les vues retournées par la `Movie` contrôleur.)

Dans **l’Explorateur de solutions**, développez le *vues* dossier, développez le *partagé* dossier et avec le bouton droit puis le *Views\Shared\DisplayTemplates* dossier.

Cliquez sur **ajouter** puis cliquez sur **vue**. Le **ajouter une vue** boîte de dialogue s’affiche.

Dans le **nom de la vue** , tapez `DateTime`. (Vous devez utiliser ce nom pour faire correspondre le nom du type).

Sélectionnez le **créer comme une vue partielle** case à cocher. Assurez-vous que le **utiliser une disposition ou la page maître** et **créer une vue fortement typée** cases à cocher ne sont pas sélectionnés.

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/_static/image2.png)

Cliquez sur **Ajouter**. Un *DateTime.cshtml* modèle est créé dans le *Views\Shared\DisplayTemplates*.

L’illustration suivante montre le *vues* dossier dans **l’Explorateur de solutions** après le `DateTime` affichage et l’éditeur de modèles sont créés.

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/_static/image3.png)

Ouvrez le *Views\Shared\DisplayTemplates\DateTime.cshtml* fichier, puis ajoutez le balisage suivant, qui utilise le [String.Format](https://msdn.microsoft.com/library/system.string.format.aspx) méthode pour mettre en forme la propriété comme une date sans heure. (Le `{0:d}` format Spécifie le format de date courte.)

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample5.cs)]

Répétez cette étape pour créer un `DateTime` modèle dans le *Views\Movie\DisplayTemplates* dossier. Utilisez le code suivant dans le *Views\Movie\DisplayTemplates\DateTime.cshtml* fichier.

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample6.cs)]

Le `loud-1` classe CSS provoque la date s’afficher en texte rouge et en gras. Vous avez ajouté le `loud-1` classe CSS comme une mesure temporaire, donc vous pouvez facilement voir que ce modèle est utilisé.

Ce que vous avez fait est créé et modèles ASP.NET utilisera pour afficher des dates personnalisés. Le modèle plus général (dans le *Views\Shared\DisplayTemplates* dossier) affiche une date courte simple. Le modèle est conçue spécifiquement pour le `Movie` contrôleur (dans le *Views\Movies\DisplayTemplates* dossier) affiche une date courte qui est également mis en forme en tant que texte gras rouge.

Appuyez sur CTRL+F5 pour exécuter l'application. Le navigateur restitue la vue Index pour l’application.

Le `ReleaseDate` propriété affiche maintenant la date en gras rouge sans le temps. Cela vous permet de vérifier que le `DateTime` helper basé sur un modèle dans le *Views\Movies\DisplayTemplates* dossier est sélectionné sur la `DateTime` helper basé sur un modèle dans le dossier partagé (*Views\Shared\ DisplayTemplates*).

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/_static/image4.png)

Maintenant renommer le *Views\Movies\DisplayTemplates\DateTime.cshtml* fichier *Views\Movies\DisplayTemplates\LoudDateTime.cshtml*.

Appuyez sur CTRL+F5 pour exécuter l'application.

Cette fois le `ReleaseDate` propriété affiche une date sans heure et sans la police en gras rouge. Cet exemple illustre que le type de modèle qui porte le nom des données (dans ce cas `DateTime`) est automatiquement utilisée pour afficher toutes les propriétés de modèle de ce type. Après avoir renommé le *DateTime.cshtml* fichier *LoudDateTime.cshtml*, ASP.NET trouvé n’est plus un modèle dans le *Views\Movies\DisplayTemplates* dossier, afin qu’il utilisé le *DateTime.cshtml* modèle à partir de la * Views\Movies\Shared\* dossier.

(La correspondance de modèle respecte la casse, donc vous pouvez créer le nom de fichier de modèle avec toutes les casses. Par exemple, *DATETIME.chstml, datetime.cshtml*, et *DaTeTiMe.cshtml* correspondrait à tous les `DateTime` type.)

Pour passer en revue : à ce stade, le `ReleaseDate` de champ est affiché à l’aide de la *Views\Movies\DisplayTemplates\DateTime.cshtml* modèle, qui affiche les données à l’aide d’un format de date courte, mais sinon n’ajoute aucun format spécial.

### <a name="using-uihint-to-specify-a-display-template"></a>À l’aide de la propriété UIHint pour spécifier un modèle d’affichage

Si votre application web possède de nombreuses `DateTime` champs par défaut que vous souhaitez afficher la totalité ou la plupart d'entre eux dans le format de date uniquement, le *DateTime.cshtml* modèle constitue une bonne approche. Mais que se passe-t-il si vous avez quelques dates où vous souhaitez afficher la date et heure complètes ? Aucun problème. Vous pouvez créer un modèle supplémentaire et utiliser le [UIHint](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.uihintattribute.uihint.aspx) attribut pour spécifier la mise en forme pour la date et heure complètes. Vous pouvez ensuite appliquer de manière sélective ce modèle. Vous pouvez utiliser la [UIHint](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.uihintattribute.uihint.aspx) attribut au niveau du modèle, ou vous pouvez spécifier le modèle à l’intérieur d’une vue. Dans cette section, vous allez apprendre à utiliser le `UIHint` attribut à modifier de manière sélective la mise en forme pour certaines instances de champs date-heure.

Ouvrez le *Views\Movies\DisplayTemplates\LoudDateTime.cshtml* fichier et remplacez le code existant par le code suivant :

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample7.cshtml)]

Cela provoque la date et heure complètes à afficher et ajoute la classe CSS qui met le texte vert et de grande taille.

Ouvrez le *Movie.cs* fichier, puis ajoutez le [UIHint](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.uihintattribute.uihint.aspx) attribut le `ReleaseDate` propriété, comme indiqué dans l’exemple suivant :

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample8.cs)]

Ce code indique à ASP.NET MVC que lorsqu’il affiche le `ReleaseDate` propriété (en particulier et pas seulement les `DateTime` objet), il doit utiliser le *LoudDateTime.cshtml* modèle.

Appuyez sur CTRL+F5 pour exécuter l'application.

Notez que le `ReleaseDate` propriété affiche désormais la date et l’heure dans une grande taille de police vert.

Retour à la `UIHint` d’attribut dans le *Movie.cs* de fichiers et de mettre en commentaire la *LoudDateTime.cshtml* modèle ne sera pas utilisé. Exécutez de nouveau l'application. La date de publication n’est pas affichée volumineux et vert. Cela vérifie que le *Views\Shared\DisplayTemplates\DateTime.cshtml* modèle est utilisé dans les vues d’Index et Details.

Comme mentionné précédemment, vous pouvez également appliquer un modèle dans une vue, ce qui vous permet d’appliquer le modèle à une instance individuelle de certaines données. Ouvrez le *Views\Movies\Details.cshtml* vue. Ajouter `"LoudDateTime"` comme deuxième paramètre de la [Html.DisplayFor](https://msdn.microsoft.com/library/ee407420.aspx) appeler pour le `ReleaseDate` champ. Le code complet se présente ainsi :

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample9.cshtml)]

Cela spécifie que le `LoudDateTime` modèle doit être utilisé pour afficher la propriété de modèle, quel que soit les attributs qui sont appliqués au modèle.

Appuyez sur CTRL+F5 pour exécuter l'application.

Vérifiez que la page d’index de film est à l’aide de la *Views\Shared\DisplayTemplates\DateTime.cshtml* modèle (rouge en gras) et le *Movie\Details* à l’aide de la page le *Views\Movies\ DisplayTemplates\LoudDateTime.cshtml* modèle (volumineux et vert).

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/_static/image5.png)

Dans la section suivante, vous allez créer un modèle pour un type complexe.

> [!div class="step-by-step"]
> [Précédent](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-1.md)
> [Suivant](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3.md)
