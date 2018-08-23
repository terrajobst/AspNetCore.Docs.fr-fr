---
uid: web-forms/overview/data-access/working-with-batched-data/batch-deleting-cs
title: Lot de suppression (c#) | Microsoft Docs
author: rick-anderson
description: Découvrez comment supprimer des enregistrements de base de données multiples dans une seule opération. Dans la couche d’Interface utilisateur, nous reposent sur un GridView amélioré créé dans une version tut...
ms.author: riande
ms.date: 06/26/2007
ms.assetid: ac6916d0-a5ab-4218-9760-7ba9e72d258c
msc.legacyurl: /web-forms/overview/data-access/working-with-batched-data/batch-deleting-cs
msc.type: authoredcontent
ms.openlocfilehash: c5b4d3c21fad9000ae50ecb35a5d94d176a135ee
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41826104"
---
<a name="batch-deleting-c"></a>Lot de suppression (c#)
====================
par [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Télécharger le Code](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_65_CS.zip) ou [télécharger le PDF](batch-deleting-cs/_static/datatutorial65cs1.pdf)

> Découvrez comment supprimer des enregistrements de base de données multiples dans une seule opération. Dans la couche d’Interface utilisateur, nous reposent sur un GridView amélioré créé dans un tutoriel précédent. Dans la couche d’accès aux données nous encapsuler les plusieurs opérations de suppression dans une transaction pour vous assurer que toutes les suppressions réussissent ou que toutes les suppressions sont annulées.


## <a name="introduction"></a>Introduction

Le [didacticiel précédent](batch-updating-cs.md) exploré la création d’un lot de modification d’interface à l’aide d’un GridView entièrement modifiable. Dans les situations où les utilisateurs couramment modifiez le nombre d’enregistrements à la fois, une interface de modification d’une sélection nécessiteront beaucoup moins de publications (postback) et de contexte de clavier-souris commutateurs, ce qui améliore l’efficacité de s utilisateur final. Cette technique est de même utile pour les pages où il est courant pour les utilisateurs à supprimer de nombreux enregistrements d’un coup.

Toute personne ayant utilisé un client de messagerie en ligne est déjà familiarisé avec un des plus courants suppression par lots interfaces : bouton d’une case à cocher de chaque ligne dans une grille avec un correspondant supprimer tous les éléments sélectionnés (voir Figure 1). Ce didacticiel est court au lieu de cela, car nous ve déjà fait tout le travail dans les didacticiels précédents lors de la création de l’interface basée sur le web et une méthode pour supprimer une série d’enregistrements en une seule opération atomique. Dans le [Ajout d’une colonne GridView de cases à cocher](../enhancing-the-gridview/adding-a-gridview-column-of-checkboxes-cs.md) didacticiel, nous avons créé un GridView avec une colonne de cases à cocher et dans le [encapsulant les Modifications de base de données dans une Transaction](wrapping-database-modifications-within-a-transaction-cs.md) didacticiel, nous avons créé une méthode dans la couche BLL par une transaction pour supprimer un `List<T>` de `ProductID` valeurs. Dans ce didacticiel, nous s’appuient sur et fusion de notre expérience précédente pour créer un exemple de suppression du lot de travail.


[![Chaque ligne inclut une case à cocher](batch-deleting-cs/_static/image1.gif)](batch-deleting-cs/_static/image1.png)

**Figure 1**: chaque ligne inclut une case à cocher ([cliquez pour afficher l’image en taille réelle](batch-deleting-cs/_static/image2.png))


## <a name="step-1-creating-the-batch-deleting-interface"></a>Étape 1 : Création du lot de suppression de l’Interface

Étant donné que nous avons déjà créé le lot de suppression de l’interface dans le [Ajout d’une colonne GridView de cases à cocher](../enhancing-the-gridview/adding-a-gridview-column-of-checkboxes-cs.md) didacticiel, nous pouvons simplement le copier à `BatchDelete.aspx` au lieu de sa création à partir de zéro. Commencez par ouvrir le `BatchDelete.aspx` page dans le `BatchData` dossier et le `CheckBoxField.aspx` page dans le `EnhancedGridView` dossier. À partir de la `CheckBoxField.aspx` page, accédez à la vue de Source et copier le balisage entre les `<asp:Content>` balises comme indiqué dans la Figure 2.


[![Copiez le balisage déclaratif de CheckBoxField.aspx dans le Presse-papiers](batch-deleting-cs/_static/image2.gif)](batch-deleting-cs/_static/image3.png)

**Figure 2**: copier le balisage déclaratif de `CheckBoxField.aspx` dans le Presse-papiers ([cliquez pour afficher l’image en taille réelle](batch-deleting-cs/_static/image4.png))


Ensuite, accédez à la vue de Source dans `BatchDelete.aspx` et collez le contenu du Presse-papiers dans le `<asp:Content>` balises. Même, copiez et collez le code à partir de la classe code-behind dans `CheckBoxField.aspx.cs` à dans la classe code-behind dans `BatchDelete.aspx.cs` (le `DeleteSelectedProducts` bouton s `Click` Gestionnaire d’événements, le `ToggleCheckState` (méthode) et le `Click` gestionnaires d’événements pour le `CheckAll` et `UncheckAll` boutons). Après avoir copié sur ce contenu, la `BatchDelete.aspx` classe code-behind de page s doit contenir le code suivant :


[!code-csharp[Main](batch-deleting-cs/samples/sample1.cs)]

Après avoir copié sur le balisage déclaratif et le code source, prenez un moment pour tester `BatchDelete.aspx` en l’affichant via un navigateur. Vous devriez voir un GridView répertoriant les dix premiers produits dans un GridView avec chaque ligne indiquant le nom de produit s, la catégorie et le prix, ainsi que d’une case à cocher. Il doit y avoir trois boutons : tout, décochez toutes les et supprimer des produits sélectionnés. En cliquant sur le bouton Vérifier tout de sélectionne toutes les cases à cocher, tandis que désélectionner tout Efface toutes les cases à cocher. En cliquant sur Supprimer les produits sélectionnés affiche un message qui répertorie les `ProductID` valeurs des produits sélectionnés, mais ne supprime ne pas réellement les produits.


[![L’Interface à partir de CheckBoxField.aspx a été déplacé vers BatchDeleting.aspx](batch-deleting-cs/_static/image3.gif)](batch-deleting-cs/_static/image5.png)

**Figure 3**: l’Interface à partir de `CheckBoxField.aspx` a été déplacé vers `BatchDeleting.aspx` ([cliquez pour afficher l’image en taille réelle](batch-deleting-cs/_static/image6.png))


## <a name="step-2-deleting-the-checked-products-using-transactions"></a>Étape 2 : Supprimer les produits activés à l’aide de Transactions

Avec le lot de suppression de l’interface correctement copié vers `BatchDeleting.aspx`, que ne reste à jour le code afin que le bouton Supprimer les produits sélectionnés supprime les produits activés à l’aide de la `DeleteProductsWithTransaction` méthode dans la `ProductsBLL` classe. Cette méthode, ajoutée dans le [encapsulant les Modifications de base de données dans une Transaction](wrapping-database-modifications-within-a-transaction-cs.md) didacticiel, accepte comme entrée un `List<T>` de `ProductID` valeurs et supprime chaque correspondants `ProductID` dans l’étendue d’un transaction.

Le `DeleteSelectedProducts` bouton s `Click` Gestionnaire d’événements utilise actuellement les éléments suivants `foreach` boucle pour effectuer une itération dans chaque ligne GridView :


[!code-csharp[Main](batch-deleting-cs/samples/sample2.cs)]

Pour chaque ligne, le `ProductSelector` contrôle Web de la case à cocher est référencé par programmation. Si elle est activée, la ligne s `ProductID` est récupéré à partir de la `DataKeys` collection et le `DeleteResults` étiquette s `Text` propriété est mise à jour pour inclure un message indiquant que la ligne a été sélectionnée pour suppression.

Le code ci-dessus ne supprime pas réellement les enregistrements en tant que l’appel à la `ProductsBLL` classe s `Delete` méthode est commentée. Ont été cette logique de suppression à appliquer, le code supprime les produits, mais pas dans une opération atomique. Autrement dit, si les suppressions peu premier dans la séquence a réussi, mais une version plus récente a échoué (peut-être en raison d’une violation de contrainte de clé étrangère), une exception serait levée, mais ces produits déjà supprimés resterait supprimés.

Afin de garantir l’atomicité, nous devons utiliser à la place la `ProductsBLL` classe s `DeleteProductsWithTransaction` (méthode). Étant donné que cette méthode accepte une liste de `ProductID` valeurs, nous devons tout d’abord compiler cette liste à partir de la grille et puis passez-le en tant que paramètre. Nous créons d’abord une instance d’un `List<T>` de type `int`. Dans le `foreach` boucle, nous devons ajouter les produits sélectionnés `ProductID` valeurs à ce `List<T>`. Après la boucle cela `List<T>` doit être passé à la `ProductsBLL` classe s `DeleteProductsWithTransaction` (méthode). Mise à jour le `DeleteSelectedProducts` bouton s `Click` Gestionnaire d’événements par le code suivant :


[!code-csharp[Main](batch-deleting-cs/samples/sample3.cs)]

Le code de mise à jour crée un `List<T>` de type `int` (`productIDsToDelete`) et la remplit avec les `ProductID` valeurs à supprimer. Après le `foreach` boucle, s’il existe au moins un produit sélectionné, le `ProductsBLL` classe s `DeleteProductsWithTransaction` méthode est appelée et reçoit de cette liste. Le `DeleteResults` étiquette est également affichée et les données reliées à GridView (afin que les enregistrements qui vient d’être supprimé n’apparaissent plus sous forme de lignes dans la grille).

Figure 4 illustre le contrôle GridView après qu’un nombre de lignes ont été sélectionné pour suppression. La figure 5 illustre l’écran immédiatement après la suppression de produits sélectionnée a cliqué. Notez que dans la Figure 5 la `ProductID` les valeurs des enregistrements supprimés sont affichées dans l’étiquette située sous le contrôle GridView et les lignes ne sont plus dans le contrôle GridView.


[![Les produits sélectionnés seront supprimés.](batch-deleting-cs/_static/image4.gif)](batch-deleting-cs/_static/image7.png)

**Figure 4**: The sélectionné produits seront supprimées ([cliquez pour afficher l’image en taille réelle](batch-deleting-cs/_static/image8.png))


[![Les valeurs de ProductID produits supprimés sont répertoriés sous le GridView.](batch-deleting-cs/_static/image5.gif)](batch-deleting-cs/_static/image9.png)

**Figure 5**: les produits supprimés `ProductID` les valeurs sont répertoriées sous le GridView ([cliquez pour afficher l’image en taille réelle](batch-deleting-cs/_static/image10.png))


> [!NOTE]
> Pour tester le `DeleteProductsWithTransaction` atomicité méthode s, ajoutez manuellement une entrée pour un produit dans le `Order Details` de table, puis que vous essayez de supprimer ce produit (ainsi que d’autres). Vous recevrez une violation de contrainte de clé étrangère lorsque vous tentez de supprimer le produit avec une commande associée, mais notez comment les autres suppressions de produits sélectionnés sont restaurées.


## <a name="summary"></a>Récapitulatif

Création d’un lot de suppression de l’interface implique l’ajout d’un GridView avec une colonne de cases à cocher et un site Web de bouton qui, contrôler quand activé, l’opération va supprimer toutes les lignes sélectionnées en une seule opération atomique. Dans ce didacticiel, nous avons créé une telle interface par rabouter travailler dans deux didacticiels précédents, [Ajout d’une colonne GridView de cases à cocher](../enhancing-the-gridview/adding-a-gridview-column-of-checkboxes-cs.md) et [encapsulant les Modifications de base de données dans une Transaction](wrapping-database-modifications-within-a-transaction-cs.md). Dans le premier didacticiel, nous avons créé un GridView avec une colonne de cases à cocher et dans ce dernier, nous avons implémenté une méthode dans la couche BLL qui, lorsqu’il est passé un `List<T>` de `ProductID` valeurs, les supprimer tout dans l’étendue d’une transaction.

Dans le didacticiel suivant, nous allons créer une interface pour effectuer des insertions du lot.

Bonne programmation !

## <a name="about-the-author"></a>À propos de l’auteur

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), auteur de sept les livres sur ASP/ASP.NET et fondateur de [4GuysFromRolla.com](http://www.4guysfromrolla.com), travaille avec les technologies Web Microsoft depuis 1998. Scott fonctionne comme un consultant indépendant, formateur et writer. Son dernier ouvrage est [*SAM animer vous-même ASP.NET 2.0 des dernières 24 heures*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Il peut être contacté à [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou via son blog, qui se trouve à [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Remerciements

Cette série de didacticiels a été révisée par plusieurs réviseurs utiles. Les réviseurs tête pour ce didacticiel ont été Hilton Giesenow et Teresa Murphy. Qui souhaitent consulter mes prochains articles MSDN ? Dans ce cas, envoyez-moi une ligne à [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Précédent](batch-updating-cs.md)
> [Suivant](batch-inserting-cs.md)
