---
uid: web-forms/overview/data-access/displaying-data-with-the-datalist-and-repeater/showing-multiple-records-per-row-with-the-datalist-control-vb
title: Affichage de plusieurs enregistrements par ligne avec le contrôle DataList (VB) | Microsoft Docs
author: rick-anderson
description: Dans ce court didacticiel nous allons découvrir comment personnaliser la disposition du contrôle DataList via ses propriétés RepeatColumns et RepeatDirection.
ms.author: riande
ms.date: 09/13/2006
ms.assetid: f555c531-bf33-4699-9987-42dbfef23c1f
msc.legacyurl: /web-forms/overview/data-access/displaying-data-with-the-datalist-and-repeater/showing-multiple-records-per-row-with-the-datalist-control-vb
msc.type: authoredcontent
ms.openlocfilehash: 7a23e8897e4fbdf84c4d6916c22bad9687fa5ee6
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41831739"
---
<a name="showing-multiple-records-per-row-with-the-datalist-control-vb"></a>Affichage de plusieurs enregistrements par ligne avec le contrôle DataList (VB)
====================
par [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Télécharger l’exemple d’application](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_31_VB.exe) ou [télécharger le PDF](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/datatutorial31vb1.pdf)

> Dans ce court didacticiel nous allons découvrir comment personnaliser la disposition du contrôle DataList via ses propriétés RepeatColumns et RepeatDirection.


## <a name="introduction"></a>Introduction

Les exemples de DataList nous ve vu dans les deux derniers didacticiels les ont rendus chaque enregistrement à partir de sa source de données sous forme de ligne dans une seule colonne HTML `<table>`. Il s’agit du comportement de DataList par défaut, mais il est très facile personnaliser l’affichage de DataList telles que les éléments de source de données sont réparties sur une table à plusieurs colonne, à plusieurs ligne. En outre, il s permet de disposer de toutes les données source des éléments affichés dans une seule ligne, plusieurs colonne DataList.

Nous pouvons personnaliser la disposition de DataList s via son `RepeatColumns` et `RepeatDirection` propriétés, qui, respectivement, indiquent le nombre de colonnes est rendu et si ces éléments sont disposées verticalement ou horizontalement. Figure 1, par exemple, montre un contrôle DataList qui affiche des informations sur les produits dans une table avec trois colonnes.


[![Le contrôle DataList montre trois produits par ligne](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image2.png)](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image1.png)

**Figure 1**: le contrôle DataList montre trois produits par ligne ([cliquez pour afficher l’image en taille réelle](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image3.png))


En affichant plusieurs éléments de source de données par ligne, le contrôle DataList peut utiliser plus efficacement les espace d’écran horizontale. Dans ce bref didacticiel, nous allons explorer ces deux propriétés de DataList.

## <a name="step-1-displaying-product-information-in-a-datalist"></a>Étape 1 : Afficher des informations sur les produits dans un contrôle DataList

Avant d’examiner le `RepeatColumns` et `RepeatDirection` propriétés, let s d’abord créer un contrôle DataList sur notre page qui répertorie les informations de produit à l’aide de la disposition de table standard d’une seule colonne, plusieurs lignes. Pour cet exemple, permettent d’afficher le nom de s de produit, la catégorie et le prix en utilisant le balisage suivant s :


[!code-html[Main](showing-multiple-records-per-row-with-the-datalist-control-vb/samples/sample1.html)]

Nous ve vu comment lier des données à un contrôle DataList dans les exemples précédents, je vais déplacer rapidement à travers ces étapes. Commencez par ouvrir le `RepeatColumnAndDirection.aspx` page dans le `DataListRepeaterBasics` dossier et faites glisser un contrôle DataList à partir de la boîte à outils vers le concepteur. À partir de la balise active DataList s, choisir de créer un nouveau ObjectDataSource et configurez-le pour extraire ses données à partir de la `ProductsBLL` classe s `GetProducts` méthode, en choisissant (aucun) option à partir de l’Assistant s INSERT, UPDATE et supprimer des onglets.

Après avoir créé et liaison ObjectDataSource nouvelle pour le contrôle DataList, Visual Studio crée automatiquement un `ItemTemplate` qui affiche le nom et la valeur pour chacun des champs de données de produit. Ajuster la `ItemTemplate` directement via le balisage déclaratif ou de modifier les modèles option dans la balise active DataList s afin qu’il utilise le balisage ci-dessus, en remplaçant le *Product Name*, *nom de catégorie* , et *prix* texte avec les contrôles Label qui utilisent la syntaxe de liaison de données appropriée pour affecter des valeurs à leurs `Text` propriétés. Après la mise à jour le `ItemTemplate`, votre balisage déclaratif s de page doit ressembler à ce qui suit :


[!code-aspx[Main](showing-multiple-records-per-row-with-the-datalist-control-vb/samples/sample2.aspx)]

Notez que je ve inclus un spécificateur de format dans le `Eval` syntaxe de liaison de données pour le `UnitPrice`, mise en forme de la valeur retournée sous forme de devise - `Eval("UnitPrice", "{0:C}").`

Prenez un moment pour consulter la page dans un navigateur. Comme le montre la Figure 2, le contrôle DataList est rendu sous la forme d’une table d’une seule colonne, plusieurs ligne de produits.


[![Par défaut, les convertisseurs de DataList sous forme de tableau colonne unique, plusieurs ligne](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image5.png)](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image4.png)

**Figure 2**: par défaut, le contrôle DataList rendu sous la forme d’une seule colonne, Table de plusieurs lignes ([cliquez pour afficher l’image en taille réelle](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image6.png))


## <a name="step-2-changing-the-datalist-s-layout-direction"></a>Étape 2 : Modification de la Direction de mise en page de s DataList

Tout le comportement par défaut pour le contrôle DataList consiste à disposer de ses éléments verticalement dans une table une seule colonne, plusieurs ligne, ce comportement peut facilement modifiables via le contrôle DataList s [ `RepeatDirection` propriété](https://msdn.microsoft.com/system.web.ui.webcontrols.datalist.repeatdirection.aspx). Le `RepeatDirection` propriété peut accepter une des deux valeurs possibles : `Horizontal` ou `Vertical` (la valeur par défaut).

En modifiant le `RepeatDirection` propriété à partir de `Vertical` à `Horizontal`, le contrôle DataList restitue ses enregistrements dans une seule ligne, la création d’une colonne par un élément de source de données. Pour illustrer cela, cliquez sur le contrôle DataList dans le concepteur et puis, dans la fenêtre Propriétés, modifiez le `RepeatDirection` propriété à partir de `Vertical` à `Horiztonal`. Immédiatement lors de cette façon, le concepteur ajuste la disposition du contrôle DataList s, création d’une interface de ligne unique, plusieurs colonne (voir Figure 3).


[![Les éléments RepeatDirection propriété détermine comment la Direction le s DataList sont présentés](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image8.png)](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image7.png)

**Figure 3**: le `RepeatDirection` propriété détermine comment les éléments de la Direction du s DataList sont présentés ([cliquez pour afficher l’image en taille réelle](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image9.png))


Lors de l’affichage de petites quantités de données, une seule ligne, table à plusieurs colonnes peut être un moyen idéal d’optimiser l’écran. Toutefois, pour des volumes plus importants de données, une seule ligne requièrent plusieurs colonnes, les notifications Push ces éléments se trouvent que t peut tenir sur l’écran hors tension à droite. Figure 4 montre les produits lors du rendu d’un contrôle DataList seule ligne. Dans la mesure où il existe de nombreux produits (plus de 80), l’utilisateur aura à faire défiler vers la droite pour afficher des informations sur chacun des produits.


[![Pour les Sources de données suffisamment grand, une seule colonne de DataList nécessitera un défilement Horizontal](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image11.png)](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image10.png)

**Figure 4**: pour suffisamment grandes Sources de données, une seule colonne DataList sera requièrent un défilement Horizontal ([cliquez pour afficher l’image en taille réelle](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image12.png))


## <a name="step-3-displaying-data-in-a-multi-column-multi-row-table"></a>Étape 3 : Affichage des données dans une Table à plusieurs colonne, plusieurs ligne

Pour créer un contrôle DataList multi-colonne, plusieurs ligne, nous devons définir la [ `RepeatColumns` propriété](https://msdn.microsoft.com/system.web.ui.webcontrols.datalist.repeatcolumns.aspx) au nombre de colonnes à afficher. Par défaut, le `RepeatColumns` propriété est définie sur 0, ce qui entraîne le contrôle DataList afficher tous ses éléments dans une seule ligne ou une colonne (selon la valeur de la `RepeatDirection` propriété).

Dans notre exemple, permettent d’afficher les trois produits par ligne de table s. Par conséquent, définissez le `RepeatColumns` 3 à la propriété. Après avoir apporté cette modification, prenez un moment pour afficher les résultats dans un navigateur. Comme le montre la Figure 5, les produits sont maintenant répertoriées dans un tableau de trois colonnes et plusieurs ligne.


[![Trois produits sont affichés par ligne](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image14.png)](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image13.png)

**Figure 5**: trois produits sont affichés par ligne ([cliquez pour afficher l’image en taille réelle](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image15.png))


Le `RepeatDirection` propriété affecte la disposition des éléments dans le contrôle DataList. La figure 5 illustre les résultats avec la `RepeatDirection` propriété définie sur `Horizontal`. Notez que les trois premiers produits Chai, suivi modifications et affiché sont disposés de gauche à droite, de haut en bas. Les trois produits (commençant par s Chef Anton Cajun Seasoning) s’affichent dans une ligne sous les trois premiers. Modification de la `RepeatDirection` propriété retour au `Vertical`, toutefois, distille ces produits de haut en bas, gauche à droite, comme le montre la Figure 6.


[![Ici, les produits sont présentés verticale](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image17.png)](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image16.png)

**Figure 6**: ici, les produits sont présentés Out verticalement ([cliquez pour afficher l’image en taille réelle](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image18.png))


Le nombre de lignes affichées dans la table résultante varie selon le nombre total d’enregistrements liés à la DataList. Précisément, il s le plafond du nombre total d’éléments de source de données divisé par le `RepeatColumns` valeur de propriété. Dans la mesure où le `Products` table comporte actuellement des 84 produits, qui est divisible par 3, il existe des 28 lignes. Si le nombre d’éléments dans la source de données et la `RepeatColumns` valeur de propriété n’est pas divisible, puis la dernière ligne ou la colonne aura des cellules vides. Si le `RepeatDirection` a la valeur `Vertical`, puis la dernière colonne aura des cellules vides ; si `RepeatDirection` est `Horizontal`, puis la dernière ligne aura les cellules vides.

## <a name="summary"></a>Récapitulatif

Le contrôle DataList, par défaut, répertorie ses éléments dans une table contenant la colonne unique, plusieurs ligne, qui reproduit la disposition d’un GridView avec TemplateField unique. Même si cette mise en page par défaut est acceptable, nous pouvons optimiser l’écran en affichant plusieurs éléments de source de données par ligne. Pour accomplir ceci est une simple question de la définition du contrôle DataList s `RepeatColumns` propriété au nombre de colonnes à afficher par ligne. En outre, le contrôle DataList s `RepeatDirection` propriété peut être utilisée pour indiquer si le contenu de la table à plusieurs colonne, plusieurs ligne doit être disposé horizontalement de gauche à droite, de haut en bas ou verticalement de haut en bas, de gauche à droite.

## <a name="about-the-author"></a>À propos de l’auteur

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), auteur de sept les livres sur ASP/ASP.NET et fondateur de [4GuysFromRolla.com](http://www.4guysfromrolla.com), travaille avec les technologies Web Microsoft depuis 1998. Scott fonctionne comme un consultant indépendant, formateur et writer. Son dernier ouvrage est [*SAM animer vous-même ASP.NET 2.0 des dernières 24 heures*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Il peut être contacté à [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou via son blog, qui se trouve à [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Remerciements

Cette série de didacticiels a été révisée par plusieurs réviseurs utiles. Entraîner un réviseur pour ce didacticiel a été John Suru. Qui souhaitent consulter mes prochains articles MSDN ? Dans ce cas, envoyez-moi une ligne à [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Précédent](formatting-the-datalist-and-repeater-based-upon-data-vb.md)
> [Suivant](nested-data-web-controls-vb.md)
