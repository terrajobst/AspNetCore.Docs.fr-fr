---
uid: web-forms/overview/data-access/custom-button-actions-with-the-datalist-and-repeater/custom-buttons-in-the-datalist-and-repeater-cs
title: Boutons personnalisés dans les contrôles DataList et Repeater (c#) | Microsoft Docs
author: rick-anderson
description: Dans ce didacticiel, nous allons créer une interface qui utilise un répéteur pour répertorier les catégories dans le système, chaque catégorie de fournir un bouton pour afficher ses associ...
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/13/2006
ms.topic: article
ms.assetid: 1f42e332-78dc-438b-9e35-0c97aa0ad929
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/data-access/custom-button-actions-with-the-datalist-and-repeater/custom-buttons-in-the-datalist-and-repeater-cs
msc.type: authoredcontent
ms.openlocfilehash: 802f52e8e4e1ca1addec3321503ac92474ffd6b8
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37369484"
---
<a name="custom-buttons-in-the-datalist-and-repeater-c"></a>Boutons personnalisés dans les contrôles DataList et Repeater (c#)
====================
par [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Télécharger l’exemple d’application](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_46_CS.exe) ou [télécharger le PDF](custom-buttons-in-the-datalist-and-repeater-cs/_static/datatutorial46cs1.pdf)

> Dans ce didacticiel, nous allons créer une interface qui utilise un répéteur pour répertorier les catégories dans le système, chaque catégorie de fournir un bouton pour afficher ses produits associés à l’aide d’un contrôle BulletedList.


## <a name="introduction"></a>Introduction

Dans les derniers didacticiels dix-sept contrôles DataList et Repeater, nous ve créé à la fois les exemples en lecture seule et la modification et suppression des exemples. Pour faciliter la modification et suppression de fonctionnalités au sein d’un contrôle DataList, nous avons ajouté des boutons à la DataList s `ItemTemplate` qui, lorsque vous cliquez dessus, a provoqué une publication (postback) et a déclenché un événement de DataList correspondant au bouton s `CommandName` propriété. Par exemple, ajout d’un bouton à la `ItemTemplate` avec un `CommandName` valeur de propriété de modification provoque le contrôle DataList s `EditCommand` à déclencher lors de publication (postback) ; l’autre avec la `CommandName` supprimer déclenche le `DeleteCommand`.

En outre pour modifier et supprimer des boutons, les contrôles DataList et Repeater peuvent également inclure boutons LinkButton et/ou ImageButtons qui, lorsque vous cliquez dessus, effectuer une logique côté serveur personnalisée. Dans ce didacticiel, nous allons créer une interface qui utilise un répéteur pour répertorier les catégories dans le système. Pour chaque catégorie, le Repeater inclut un bouton pour afficher la catégorie de produits s associés à l’aide d’un contrôle BulletedList (voir Figure 1).


[![En cliquant sur le lien se produits Show affiche les produits de s catégorie dans une liste à puces](custom-buttons-in-the-datalist-and-repeater-cs/_static/image2.png)](custom-buttons-in-the-datalist-and-repeater-cs/_static/image1.png)

**Figure 1**: en cliquant sur les affichages de lien Afficher les produits de la catégorie s produits dans une liste à puces ([cliquez pour afficher l’image en taille réelle](custom-buttons-in-the-datalist-and-repeater-cs/_static/image3.png))


## <a name="step-1-adding-the-custom-button-tutorial-web-pages"></a>Étape 1 : Ajout de Pages Web didacticiel bouton personnalisé

Avant d’examiner comment ajouter un bouton personnalisé, permettent de s tout d’abord prendre un moment pour créer les pages ASP.NET dans notre projet de site Web que nous avons besoin pour ce didacticiel. Commencez par ajouter un nouveau dossier nommé `CustomButtonsDataListRepeater`. Ensuite, ajoutez les deux pages ASP.NET suivantes à ce dossier, en veillant à associer chaque page avec le `Site.master` page maître :

- `Default.aspx`
- `CustomButtons.aspx`


![Ajouter les Pages ASP.NET pour les didacticiels liés de boutons personnalisés](custom-buttons-in-the-datalist-and-repeater-cs/_static/image4.png)

**Figure 2**: ajoutez les Pages ASP.NET pour les didacticiels liés de boutons personnalisés


Comme dans les autres dossiers, `Default.aspx` dans le `CustomButtonsDataListRepeater` dossier répertorie les didacticiels dans sa section. N’oubliez pas que le `SectionLevelTutorialListing.ascx` contrôle utilisateur fournit cette fonctionnalité. Ajoutez ce contrôle utilisateur à `Default.aspx` en le faisant glisser à partir de l’Explorateur de solutions sur la page s en mode Création.


[![Ajouter le contrôle utilisateur de SectionLevelTutorialListing.ascx à Default.aspx](custom-buttons-in-the-datalist-and-repeater-cs/_static/image6.png)](custom-buttons-in-the-datalist-and-repeater-cs/_static/image5.png)

**Figure 3**: ajouter la `SectionLevelTutorialListing.ascx` contrôle utilisateur à `Default.aspx` ([cliquez pour afficher l’image en taille réelle](custom-buttons-in-the-datalist-and-repeater-cs/_static/image7.png))


Enfin, ajoutez les pages en tant qu’entrées pour le `Web.sitemap` fichier. Plus précisément, ajoutez le balisage suivant après la pagination et tri avec les contrôles DataList et Repeater `<siteMapNode>`:


[!code-xml[Main](custom-buttons-in-the-datalist-and-repeater-cs/samples/sample1.xml)]

Après la mise à jour `Web.sitemap`, prenez un moment pour afficher le site Web de didacticiels via un navigateur. Le menu de gauche inclut désormais les éléments de la modification, insertion et suppression des didacticiels.


![Le plan de Site comprend maintenant une entrée pour le didacticiel de boutons personnalisés](custom-buttons-in-the-datalist-and-repeater-cs/_static/image8.png)

**Figure 4**: le plan de Site comprend maintenant une entrée pour le didacticiel de boutons personnalisés


## <a name="step-2-adding-the-list-of-categories"></a>Étape 2 : Ajout de la liste des catégories

Pour ce didacticiel, nous avons besoin créer un répéteur qui répertorie toutes les catégories, ainsi que d’afficher les produits LinkButton qui, lorsque vous cliquez dessus, affiche les produits de la catégorie associée s dans une liste à puces. Permettent de s tout d’abord créer un répéteur simple qui répertorie les catégories dans le système. Commencez par ouvrir le `CustomButtons.aspx` page dans le `CustomButtonsDataListRepeater` dossier. Faites glisser un répéteur à partir de la boîte à outils vers le concepteur et le jeu de son `ID` propriété `Categories`. Ensuite, créez un nouveau contrôle de source de données à partir de la balise active de s Repeater. En particulier, créer un nouveau contrôle ObjectDataSource nommé `CategoriesDataSource` qui sélectionne ses données à partir de la `CategoriesBLL` classe s `GetCategories()` (méthode).


[![Configurer pour utiliser la méthode de GetCategories() CategoriesBLL classe s ObjectDataSource](custom-buttons-in-the-datalist-and-repeater-cs/_static/image10.png)](custom-buttons-in-the-datalist-and-repeater-cs/_static/image9.png)

**Figure 5**: configurer ObjectDataSource à utiliser le `CategoriesBLL` classe s `GetCategories()` (méthode) ([cliquez pour afficher l’image en taille réelle](custom-buttons-in-the-datalist-and-repeater-cs/_static/image11.png))


Contrairement au contrôle DataList, pour laquelle Visual Studio crée une valeur par défaut `ItemTemplate` selon la source de données, les modèles Repeater s doivent être définies manuellement. En outre, les modèles Repeater s doivent être créées et modifiées de façon déclarative (autrement dit, s’il y a sans modifier les modèles d’option dans la balise active Repeater s).

Cliquez sur l’onglet Source dans le coin inférieur gauche et ajoutez un `ItemTemplate` qui affiche le nom de catégorie s dans un `<h3>` élément et sa description dans un paragraphe de balise, inclure un `SeparatorTemplate` qui affiche une barre horizontale (`<hr />`) entre chaque catégorie. Ajoutez également un LinkButton avec son `Text` produits afficher la valeur de propriété. Après avoir effectué ces étapes, votre balisage déclaratif s de page doit ressembler à ce qui suit :


[!code-aspx[Main](custom-buttons-in-the-datalist-and-repeater-cs/samples/sample2.aspx)]

Figure 6 montre la page lorsqu’ils sont affichés via un navigateur. Chaque nom de catégorie et la description sont répertorié. Le bouton Afficher les produits, d’un clic, entraîne une publication, mais n’effectue pas encore aucune action.


[![Chaque nom de catégorie et la Description s’affiche, ainsi que d’afficher les produits LinkButton](custom-buttons-in-the-datalist-and-repeater-cs/_static/image13.png)](custom-buttons-in-the-datalist-and-repeater-cs/_static/image12.png)

**Figure 6**: s de chaque catégorie nom et Description s’affiche, ainsi que d’afficher les produits LinkButton ([cliquez pour afficher l’image en taille réelle](custom-buttons-in-the-datalist-and-repeater-cs/_static/image14.png))


## <a name="step-3-executing-server-side-logic-when-the-show-products-linkbutton-is-clicked"></a>Étape 3 : L’exécution côté serveur logique lors de l’afficher produits LinkButton est cliqué.

Chaque fois que l’utilisateur clique sur un bouton, LinkButton ou ImageButton dans un modèle dans un contrôle DataList ou le Repeater, une publication (postback) et les opérations de mappage DataList ou Repeater `ItemCommand` événement est déclenché. Outre le `ItemCommand` événement, le contrôle DataList contrôle peut-être également déclencher des événements plus spécifiques et un autre si le bouton s `CommandName` propriété est définie sur une des chaînes réservés (supprimer, modifier, Annuler, Update ou Select), mais la `ItemCommand` événement est *toujours* déclenché.

Lorsqu’un clic est effectué au sein d’un contrôle DataList ou le Repeater, souvent, nous avons besoin passer le long d’un utilisateur a cliqué sur le bouton (dans le cas qu’il peut y avoir plusieurs boutons dans le contrôle, tels que les deux une modification et bouton Supprimer) et peut-être des informations supplémentaires (telles que la valeur de clé primaire de l’élément dont bouton). Le bouton, LinkButton et ImageButton fournissent deux propriétés dont les valeurs sont transmises à la `ItemCommand` Gestionnaire d’événements :

- `CommandName` une chaîne, généralement utilisée pour identifier chaque bouton dans le modèle
- `CommandArgument` couramment utilisé pour contenir la valeur d’un champ de données, telles que la valeur de clé primaire

Pour cet exemple, définissez le LinkButton s `CommandName` propriété ShowProducts et lier la valeur de clé primaire enregistrement s actuel `CategoryID` à la `CommandArgument` propriété à l’aide de la syntaxe de liaison de données `CategoryArgument='<%# Eval("CategoryID") %>'`. Après la spécification de ces deux propriétés, la syntaxe déclarative de s LinkButton doit ressembler à ce qui suit :


[!code-aspx[Main](custom-buttons-in-the-datalist-and-repeater-cs/samples/sample3.aspx)]

Lorsque le bouton est activé, une publication (postback) et les opérations de mappage DataList ou Repeater `ItemCommand` événement est déclenché. Le Gestionnaire d’événements est passé le bouton s `CommandName` et `CommandArgument` valeurs.

Créer un gestionnaire d’événements pour le contrôle Repeater s `ItemCommand` événements et notez le deuxième paramètre passé dans le Gestionnaire d’événements (nommé `e`). Ce deuxième paramètre est de type [ `RepeaterCommandEventArgs` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.repeatercommandeventargs.aspx) et contient les quatre propriétés suivantes :

- `CommandArgument` la valeur du bouton concerné s `CommandArgument` propriété
- `CommandName` la valeur du bouton s `CommandName` propriété
- `CommandSource` une référence au contrôle de bouton qui a été cliqué
- `Item` une référence à la [ `RepeaterItem` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.repeateritem.aspx) qui contient le bouton qui a été cliqué ; chaque enregistrement lié à la répétition est reconnu comme un `RepeaterItem`

Depuis la catégorie sélectionnée s `CategoryID` est transmis le `CommandArgument` propriété, nous pouvons obtenir l’ensemble des produits associés à la catégorie sélectionnée dans le `ItemCommand` Gestionnaire d’événements. Ces produits peuvent ensuite être liées à un contrôle BulletedList dans le `ItemTemplate` (qui nous ve encore à ajouter). Tout ce qui reste, puis, consiste à ajouter l’élément BulletedList, référencez-le dans le `ItemCommand` Gestionnaire d’événements et le lier l’ensemble des produits pour la catégorie sélectionnée, nous les aborderons à l’étape 4.

> [!NOTE]
> Le contrôle DataList s `ItemCommand` un objet de type est passé au gestionnaire d’événements [ `DataListCommandEventArgs` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalistcommandeventargs.aspx), qui offre les quatre mêmes propriétés que le `RepeaterCommandEventArgs` classe.


## <a name="step-4-displaying-the-selected-category-s-products-in-a-bulleted-list"></a>Étape 4 : Afficher les produits de s catégorie sélectionnée dans une liste à puces

Les produits de la catégorie sélectionnée s peuvent être affichées dans le répéteur s `ItemTemplate` à l’aide de n’importe quel nombre de contrôles. Nous pourrions ajouter qu'un autre imbriqués Repeater, DataList, DropDownList, une GridView et ainsi de suite. Cependant, étant donné que nous souhaitons afficher les produits sous forme de liste à puces, nous allons utiliser le contrôle BulletedList. Retour à la `CustomButtons.aspx` balisage déclaratif s, ajoutez un contrôle BulletedList à la `ItemTemplate` après le LinkButton produits afficher. Définir le s BulletedLists `ID` à `ProductsInCategory`. BulletedList affiche la valeur du champ de données spécifié par le biais de la `DataTextField` propriété ; étant donné que ce contrôle aura des informations sur les produits lié à celui-ci, définissez le `DataTextField` propriété `ProductName`.


[!code-aspx[Main](custom-buttons-in-the-datalist-and-repeater-cs/samples/sample4.aspx)]

Dans le `ItemCommand` Gestionnaire d’événements, ce contrôle à l’aide de la référence `e.Item.FindControl("ProductsInCategory")` et liez-le à l’ensemble des produits associés à la catégorie sélectionnée.


[!code-csharp[Main](custom-buttons-in-the-datalist-and-repeater-cs/samples/sample5.cs)]

Avant d’effectuer toute action dans le `ItemCommand` Gestionnaire d’événements, il s prudent de vérifier tout d’abord la valeur d’entrant `CommandName`. Dans la mesure où le `ItemCommand` Gestionnaire d’événements est déclenchée lorsque, *n’importe quel* bouton est activé, s’il existe plusieurs boutons dans le modèle, utilisez la `CommandName` valeur pour déterminer l’action à entreprendre. Vérification de la `CommandName` ici est discutable, étant donné que nous n’avons qu’un seul bouton, mais c’est une bonne habitude au formulaire. Ensuite, le `CategoryID` la catégorie sélectionnée ont été récupérées à partir de la `CommandArgument` propriété. Le contrôle BulletedList dans le modèle est ensuite référencé et lié aux résultats de la `ProductsBLL` classe s `GetProductsByCategoryID(categoryID)` (méthode).

Dans les didacticiels précédents utiliser les boutons au sein d’un contrôle DataList, par exemple [une vue d’ensemble de modification et suppression des données dans le contrôle DataList](../editing-and-deleting-data-through-the-datalist/an-overview-of-editing-and-deleting-data-in-the-datalist-cs.md), nous avons déterminé la valeur de clé primaire d’un élément donné via le `DataKeys` collection. Bien que cette approche fonctionne bien avec le contrôle DataList, Repeater n’a pas un `DataKeys` propriété. Au lieu de cela, nous devons utiliser une autre approche pour fournir la valeur de clé primaire, telles que via le bouton s `CommandArgument` propriété ou en attribuant la valeur de clé primaire à un contrôle Web Label masqué dans le modèle et de lire sa valeur de retour dans le `ItemCommand`Gestionnaire d’événements à l’aide `e.Item.FindControl("LabelID")`.

Après avoir effectué la `ItemCommand` Gestionnaire d’événements, prenez un moment pour tester cette page dans un navigateur. Comme le montre la Figure 7, en cliquant sur les produits afficher lien entraîne une publication (postback) et affiche les produits de la catégorie sélectionnée dans un BulletedList. En outre, notez que ces informations produit restent, même si vous cliquez sur autres catégories de liens d’afficher les produits.

> [!NOTE]
> Si vous souhaitez modifier le comportement de ce rapport, telles que les produits qu’une seule catégorie s sont répertoriés à la fois, il suffit de définir le contrôle BulletedList s `EnableViewState` propriété `False`.


[![Un BulletedList est utilisé pour afficher les produits de la catégorie sélectionnée](custom-buttons-in-the-datalist-and-repeater-cs/_static/image16.png)](custom-buttons-in-the-datalist-and-repeater-cs/_static/image15.png)

**Figure 7**: A BulletedList est utilisé pour afficher les produits de la catégorie sélectionnée ([cliquez pour afficher l’image en taille réelle](custom-buttons-in-the-datalist-and-repeater-cs/_static/image17.png))


## <a name="summary"></a>Récapitulatif

Les contrôles DataList et Repeater peuvent inclure n’importe quel nombre de boutons, de type LinkButton ou ImageButtons au sein de leurs modèles. Ces boutons, lorsque vous cliquez dessus, provoquer une publication (postback) et déclencher le `ItemCommand` événement. Pour associer les action côté serveur personnalisée avec un bouton, créez un gestionnaire d’événements pour le `ItemCommand` événement. Dans ce gestionnaire d’événements, vérifiez tout d’abord entrant `CommandName` valeur pour déterminer quel bouton l’utilisateur a cliqué. Des informations supplémentaires peuvent éventuellement être fournies par le bouton s `CommandArgument` propriété.

Bonne programmation !

## <a name="about-the-author"></a>À propos de l’auteur

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), auteur de sept les livres sur ASP/ASP.NET et fondateur de [4GuysFromRolla.com](http://www.4guysfromrolla.com), travaille avec les technologies Web Microsoft depuis 1998. Scott fonctionne comme un consultant indépendant, formateur et writer. Son dernier ouvrage est [*SAM animer vous-même ASP.NET 2.0 des dernières 24 heures*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Il peut être contacté à [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou via son blog, qui se trouve à [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Remerciements

Cette série de didacticiels a été révisée par plusieurs réviseurs utiles. Entraîner un réviseur pour ce didacticiel a été Dennis Patterson. Qui souhaitent consulter mes prochains articles MSDN ? Dans ce cas, envoyez-moi une ligne à [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Next](custom-buttons-in-the-datalist-and-repeater-vb.md)
