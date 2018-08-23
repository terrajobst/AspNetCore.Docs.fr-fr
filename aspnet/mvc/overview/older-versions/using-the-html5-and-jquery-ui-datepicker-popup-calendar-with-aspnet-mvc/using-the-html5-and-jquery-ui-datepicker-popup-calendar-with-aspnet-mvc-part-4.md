---
uid: mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4
title: Utilisation de HTML5 et Datepicker calendrier contextuel jQuery UI avec ASP.NET MVC - partie 4 | Microsoft Docs
author: Rick-Anderson
description: Ce didacticiel vous apprend les notions de base de l’utilisation des modèles de l’éditeur, les modèles d’affichage et le calendrier contextuel jQuery UI datepicker dans une MV ASP.NET...
ms.author: riande
ms.date: 08/29/2011
ms.assetid: 57666c69-2b0f-423a-a61d-be49547fa585
msc.legacyurl: /mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4
msc.type: authoredcontent
ms.openlocfilehash: 58c0ae54e31c2cb4e85da11f7402ea248d9dfb3b
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41831185"
---
<a name="using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc---part-4"></a>Utilisation de HTML5 et Datepicker calendrier contextuel jQuery UI avec ASP.NET MVC - partie 4
====================
par [Rick Anderson](https://github.com/Rick-Anderson)

> Ce didacticiel vous apprend les notions de base de l’utilisation des modèles de l’éditeur, les modèles d’affichage et le calendrier contextuel jQuery UI datepicker dans une application Web ASP.NET MVC.


### <a name="adding-a-template-for-editing-dates"></a>Ajout d’un modèle pour la modification des Dates

Dans cette section, vous allez créer un modèle à modifier les dates qui seront appliqués lorsque ASP.NET MVC affiche l’interface utilisateur pour la modification des propriétés de modèle qui sont marquées avec le **Date** énumération de la [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) attribut. Le modèle s’affiche uniquement la date ; temps s’affichera pas. Dans le modèle que vous utiliserez le [jQuery UI Datepicker](http://jqueryui.com/demos/datepicker/) calendrier contextuel à fournir un moyen de modifier les dates.

Pour commencer, ouvrez le *Movie.cs* fichier, puis ajoutez le [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx) attribut avec le **Date** énumération à la `ReleaseDate` propriété, comme indiqué dans le code suivant :

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample1.cs)]

Ce code provoque le `ReleaseDate` champ s’affiche sans l’heure dans les deux afficher les modèles et modifier des modèles. Si votre application contient un *date.cshtml* modèle dans le *Views\Shared\EditorTemplates* dossier ou dans le *Views\Movies\EditorTemplates* dossier, ce modèle sera utilisé pour restituer les `DateTime` propriété lors de la modification. Sinon, le système de création de modèles ASP.NET intégré affiche la propriété en tant que date.

Appuyez sur CTRL+F5 pour exécuter l'application. Sélectionnez un lien pour vérifier que le champ d’entrée pour la date de publication s’affiche uniquement la date de modification.

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/_static/image1.png)

Dans **l’Explorateur de solutions**, développez le *vues* dossier, développez le *partagé* dossier et avec le bouton droit puis le *Views\Shared\EditorTemplates* dossier.

Cliquez sur **ajouter**, puis cliquez sur **vue**. Le **ajouter une vue** boîte de dialogue s’affiche.

Dans le **nom de la vue** , tapez &quot;Date&quot;.

Sélectionnez le **créer comme une vue partielle** case à cocher. Assurez-vous que le **utiliser une disposition ou la page maître** et **créer une vue fortement typée** cases à cocher ne sont pas sélectionnés.

Cliquez sur **Ajouter**. Le *Views\Shared\EditorTemplates\Date.cshtml* modèle est créé.

Ajoutez le code suivant à la *Views\Shared\EditorTemplates\Date.cshtml* modèle.

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample2.cshtml)]

La première ligne déclare le modèle pour être un `DateTime` type. Bien que vous n’avez pas besoin de déclarer le type de modèle en cours de modification et afficher les modèles, il est recommandé de sorte que la compilation la vérification du modèle passé à la vue. (Un autre avantage est que vous obtenez ensuite IntelliSense pour le modèle dans la vue dans Visual Studio). Si le type de modèle n’est pas déclaré, ASP.NET MVC considère comme un [dynamique](https://msdn.microsoft.com/library/dd264741.aspx) tapez et il n’existe aucune compilation la vérification du type. Si vous déclarez le modèle pour être un `DateTime` type, elle devienne fortement typée.

La deuxième ligne est simplement littéral balisage HTML qui affiche &quot;à l’aide de modèle de Date&quot; avant un champ de date. Vous utiliserez cette ligne temporairement pour vérifier que ce modèle de date est utilisé.

La ligne suivante est un [Html.TextBox](https://msdn.microsoft.com/library/system.web.mvc.html.inputextensions.textbox.aspx) helper qui restitue une `input` champ qui est une zone de texte. Le troisième paramètre de l’application d’assistance utilise un type anonyme pour définir la classe pour la zone de texte `datefield` et le type à `date`. (Étant donné que `class` est un réservé en c#, vous devez utiliser le `@` caractère d’échappement le `class` attribut dans l’analyseur c#.)

Le `date` type est un type d’entrée de HTML5 qui permet aux navigateurs prenant en charge HTML5 restituer un contrôle de calendrier HTML5. Par la suite, vous allez ajouter du code JavaScript pour raccorder le sélecteur de dates jQuery pour le `Html.TextBox` à l’aide de l’élément le `datefield` classe.

Appuyez sur CTRL+F5 pour exécuter l'application. Vous pouvez vérifier que le `ReleaseDate` propriété dans la vue edit est à l’aide du modèle de modification, car le modèle affiche &quot;à l’aide de modèle de Date&quot; juste avant la `ReleaseDate` texte d’entrée de zone, comme indiqué dans cette image :

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/_static/image2.png)

Dans votre navigateur, affichez la source de la page. (Par exemple, avec le bouton droit de la page et sélectionnez **afficher la source**.) Ce qui suit illustre certaines du balisage de la page, illustrant le `class` et `type` attributs dans le code HTML restitué.

[!code-html[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample3.html)]

Retour à la *Views\Shared\EditorTemplates\Date.cshtml* modèle et supprimer le &quot;à l’aide de modèle de Date&quot; balisage. Maintenant le modèle complet ressemble à ceci :

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample4.cshtml)]

### <a name="adding-a-jquery-ui-datepicker-popup-calendar-using-nuget"></a>Ajout d’un sélecteur de dates calendrier contextuel jQuery UI à l’aide de NuGet

Dans cette section, vous ajouterez le [jQuery UI datepicker](http://jqueryui.com/demos/datepicker/) calendrier contextuel pour le modèle de date de modification. Le [jQuery UI](http://jqueryui.com/) bibliothèque prend en charge avancée de l’animation, les effets et widgets personnalisables. Il est basé sur la bibliothèque JavaScript jQuery. Le calendrier contextuel datepicker rend facile et naturel d’entrer des dates à l’aide d’un calendrier au lieu d’entrer une chaîne. Le calendrier contextuel limite également les utilisateurs à des dates juridiques : entrée de texte ordinaires pour une date vous permet de vous entrez quelque chose comme `2/33/1999` (février 33rd, 1999), mais la [jQuery UI datepicker](http://jqueryui.com/demos/datepicker/) qui n’autorise pas le calendrier contextuel.

Tout d’abord, vous devez installer les bibliothèques d’interface utilisateur jQuery. Pour ce faire, vous allez utiliser NuGet, qui est un gestionnaire de package qui est inclus dans les versions de Service Pack 1 de Visual Studio 2010 et Visual Web Developer.

Dans Visual Web Developer, à partir de la **outils** menu, sélectionnez **Library Package Manager** , puis sélectionnez **gérer les Packages NuGet**.

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/_static/image3.png)

Remarque : Si le **outils** menu n’affiche pas le **Library Package Manager** commande, vous devez installer le package NuGet en suivant les instructions la [installation NuGet](http://docs.nuget.org/docs/start-here/installing-nuget) page de le site Web NuGet.   
  
Si vous utilisez Visual Studio au lieu de Visual Web Developer, à partir de la **outils** menu, sélectionnez **Library Package Manager** , puis sélectionnez **ajouter une référence de Package de bibliothèque**.

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/_static/image4.png)

Dans le **MVCMovie - gérer les Packages NuGet** boîte de dialogue, cliquez sur le **Online** onglet sur la gauche, puis entrez &quot;jQuery.UI&quot; dans la zone de recherche. Sélectionnez j **WIdgets d’interface utilisateur requête : Datepicker**, puis sélectionnez le **installer** bouton.

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/_static/image5.png)

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/_static/image6.png)

NuGet ajoute ces versions de débogage et réduits de jQuery Core de l’interface utilisateur et le sélecteur de dates jQuery UI à votre projet :

- *jquery.ui.core.js*
- *jquery.ui.core.min.js*
- *jquery.ui.datepicker.js*
- *jquery.ui.datepicker.min.js*

Remarque : Les versions de débogage (les fichiers sans les *. min.js* extension) sont utiles pour le débogage, mais dans un site de production, vous devez inclure uniquement les versions minimisées.

Pour utiliser réellement le sélecteur de dates jQuery, vous devez créer un script de jQuery se raccorde le widget de calendrier pour le modèle de modification. Dans **l’Explorateur de solutions**, avec le bouton droit le *Scripts* dossier et sélectionnez **ajouter**, puis **un nouvel élément**, puis **JScript Fichier**. Nommez le fichier *DatePickerReady.js*.

Ajoutez le code suivant à la *DatePickerReady.js* fichier :

[!code-javascript[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample5.js)]

Si vous n’êtes pas familiarisé avec jQuery, voici une brève explication de ce fait : la première ligne est la &quot;jQuery prêt&quot; (fonction), qui est appelée lorsque tous les éléments DOM dans une page de chargement. La deuxième ligne sélectionne tous les éléments DOM qui portent le nom de la classe `datefield`, puis appelle le `datepicker` (fonction) pour chacun d’eux. (N’oubliez pas que vous avez ajouté le `datefield` classe à la *Views\Shared\EditorTemplates\Date.cshtml* modèle précédemment dans ce didacticiel.)

Ensuite, ouvrez le *Views\Shared\\_Layout.cshtml* fichier. Vous devez ajouter des références aux fichiers suivants, qui sont tous obligatoires afin que vous puissiez utiliser le sélecteur de dates :

- *Content/Themes/base/jQuery.UI.Core.CSS*
- *Content/Themes/base/jQuery.UI.DatePicker.CSS*
- *Content/Themes/base/jQuery.UI.Theme.CSS*
- *jquery.ui.core.min.js*
- *jquery.ui.datepicker.min.js*
- *DatePickerReady.js*

L’exemple suivant montre le code réel que vous devez ajouter au bas de la `head` élément dans le *Views\Shared\\_Layout.cshtml* fichier.

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample6.cshtml)]

L’ensemble `head` section est illustrée ici :

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample7.cshtml)]

Le [helper contenu URL](https://msdn.microsoft.com/library/system.web.mvc.urlhelper.content.aspx) méthode convertit le chemin d’accès de ressource à un chemin d’accès absolu. Vous devez utiliser `@URL.Content` pour référencer correctement ces ressources lors de l’application s’exécute sur IIS.

Appuyez sur CTRL+F5 pour exécuter l'application. Sélectionnez un lien de modification, puis placez le point d’insertion dans le **ReleaseDate** champ. Le calendrier de fenêtre contextuelle de l’interface utilisateur jQuery s’affiche.

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/_static/image7.png)

Comme la plupart des contrôles de jQuery, datepicker vous permet de personnaliser de manière intensive. Pour plus d’informations, consultez [personnalisation Visual : conception d’un thème de l’interface utilisateur jQuery](http://learn.jquery.com/jquery-ui/getting-started/#visual-customization-designing-a-jquery-ui-theme) sur le [jQuery UI](http://learn.jquery.com/jquery-ui/getting-started/) site.

### <a name="supporting-the-html5-date-input-control"></a>Prise en charge le contrôle d’entrée de Date HTML5

Comme plusieurs navigateurs prennent en charge HTML5, vous souhaiterez utiliser HTML5 natif de saisie, tels que le `date` élément d’entrée et n’utilisez pas le calendrier de l’interface utilisateur jQuery. Vous pouvez ajouter une logique à votre application pour utiliser automatiquement les contrôles HTML5 si le navigateur prend en charge les. Pour ce faire, remplacez le contenu de la *DatePickerReady.js* fichier par le code suivant :

[!code-javascript[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample8.js)]

La première ligne de ce script utilise Modernizr pour vérifier que l’entrée de date HTML5 est pris en charge. Si elle n’est pas pris en charge, le sélecteur de dates jQuery UI est raccordé à la place. ([Modernizr](http://www.modernizr.com/docs/) est une bibliothèque JavaScript open source qui détecte la disponibilité des implémentations natives de HTML5 et CSS3. Modernizr est inclus dans tous les nouveaux projets ASP.NET MVC que vous créez.)

Après avoir apporté cette modification, vous pouvez la tester à l’aide d’un navigateur qui prend en charge de HTML5, telles que Opera 11. Exécutez l’application à l’aide d’un navigateur compatible HTML5 et modifier une entrée de film. Le contrôle de date HTML5 est utilisé au lieu du calendrier de fenêtre contextuelle de l’interface utilisateur jQuery :

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/_static/image8.png)

Étant donné que les nouvelles versions de navigateurs sont implémentation HTML5 de façon incrémentielle, une bonne approche pour l’instant consiste à ajouter du code à votre site Web qui prend en charge un large éventail de prise en charge HTML5. Par exemple, un plus robuste *DatePickerReady.js* script ci-dessous qui vous permet de votre site destinées aux navigateurs qui prennent en charge seulement partiellement le contrôle de date HTML5.

[!code-javascript[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample9.js)]

Ce script sélectionne HTML5 `input` éléments de type `date` qui ne prennent entièrement en charge le contrôle de date HTML5. Pour ces éléments, elle se raccorde le calendrier de fenêtre contextuelle de l’interface utilisateur jQuery, puis modifie le `type` de l’attribut `date` à `text`. En modifiant le `type` de l’attribut `date` à `text`, prise en charge de date HTML5 partielle est éliminé. Encore plus robustes *DatePickerReady.js* script, consultez [JSFIDDLE](http://jsfiddle.net/XSTK8/15/).

### <a name="adding-nullable-dates-to-the-templates"></a>Ajout de Dates non Nullable pour les modèles

Si vous utilisez un des modèles de date existant et que vous passez une date null, vous obtiendrez une erreur d’exécution. Pour rendre les modèles de date plus robuste, vous allez les modifier pour gérer des valeurs null. Pour prendre en charge les dates non nullable, modifiez le code dans le *Views\Shared\DisplayTemplates\DateTime.cshtml* à ce qui suit :

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample10.cshtml)]

Le code retourne une chaîne vide lorsque le modèle est **null**.

Modifier le code dans le *Views\Shared\EditorTemplates\Date.cshtml* fichier à ce qui suit :

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample11.cshtml)]

Lorsque ce code s’exécute, si le modèle n’est pas null, le modèle `DateTime` valeur est utilisée. Si le modèle est null, la date actuelle est utilisée à la place.

### <a name="wrapup"></a>Conclusion

Ce didacticiel a couvert les principes fondamentaux de programmes d’assistance basés sur des modèles ASP.NET et vous montre comment utiliser le calendrier contextuel du sélecteur jQuery UI dans une application ASP.NET MVC. Pour plus d’informations, essayez ces ressources :

- Pour plus d’informations sur la localisation, consultez le blog de Rajeesh [JQueryUI Datepicker dans ASP.NET MVC](http://www.rajeeshcv.com/2010/02/jqueryui-datepicker-in-asp-net-mvc/).
- Pour plus d’informations sur l’interface utilisateur jQuery, consultez [jQuery UI](http://docs.jquery.com/UI).
- Pour plus d’informations sur la façon de localiser le contrôle datepicker, consultez [Datepicker / l’interface utilisateur/localisation](http://docs.jquery.com/UI/Datepicker/Localization).
- Pour plus d’informations sur les modèles ASP.NET MVC, consultez les billets de blog de Brad Wilson sur [modèles ASP.NET MVC 2](http://bradwilson.typepad.com/blog/2009/10/aspnet-mvc-2-templates-part-1-introduction.html). Bien que la série a été écrit pour ASP.NET MVC 2, le matériel est toujours applicable pour la version actuelle d’ASP.NET MVC.

> [!div class="step-by-step"]
> [Précédent](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3.md)
