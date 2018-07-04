---
uid: web-forms/overview/data-access/paging-and-sorting/sorting-custom-paged-data-vb
title: Tri personnalisé de données (VB) paginées | Microsoft Docs
author: rick-anderson
description: Dans le didacticiel précédent, nous avons appris à implémenter la pagination personnalisée lorsque presentating des données sur une page web. Dans ce didacticiel, nous expliquons comment étendre l’exemple précédent...
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/15/2006
ms.topic: article
ms.assetid: 4823a186-caaf-4116-a318-c7ff4d955ddc
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/data-access/paging-and-sorting/sorting-custom-paged-data-vb
msc.type: authoredcontent
ms.openlocfilehash: 9921b541e0160054f080ff08468ddfc5cc92373b
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37363651"
---
<a name="sorting-custom-paged-data-vb"></a>Tri personnalisé paginée des données (VB)
====================
par [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Télécharger l’exemple d’application](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_26_VB.exe) ou [télécharger le PDF](sorting-custom-paged-data-vb/_static/datatutorial26vb1.pdf)

> Dans le didacticiel précédent, nous avons appris à implémenter la pagination personnalisée lorsque presentating des données sur une page web. Dans ce didacticiel, nous expliquons comment étendre l’exemple précédent pour prendre en charge pour le tri de la pagination personnalisée.


## <a name="introduction"></a>Introduction

Par rapport à la pagination par défaut, la pagination personnalisée peut améliorer les performances de la pagination de données par plusieurs ordres de grandeur, rendre personnalisé pagination le choix de mise en œuvre la pagination de fait, lors de la pagination dans de grandes quantités de données. L’implémentation de la pagination personnalisée est plus complexe que l’implémentation par défaut la pagination, toutefois, en particulier lors de l’ajout de tri à la combinaison. Dans ce didacticiel, nous allons étendre l’exemple à partir de le précédent pour inclure la prise en charge pour le tri *et* la pagination personnalisée.

> [!NOTE]
> Étant donné que ce didacticiel s’appuie sur celui précédente, avant le début prenez un moment pour copier la syntaxe déclarative dans le `<asp:Content>` élément à partir de la page web de didacticiel s précédent (`EfficientPaging.aspx`) et coller entre le `<asp:Content>` élément dans le `SortParameter.aspx` page. Revenir à l’étape 1 de la [Ajout de contrôles de Validation pour l’édition et insertion des Interfaces](../editing-inserting-and-deleting-data/adding-validation-controls-to-the-editing-and-inserting-interfaces-vb.md) didacticiel pour une présentation plus détaillée sur la réplication de la fonctionnalité d’une page ASP.NET sur un autre.


## <a name="step-1-reexamining-the-custom-paging-technique"></a>Étape 1 : Réexamen la Technique de la pagination personnalisée

Pour la pagination personnalisée fonctionne correctement, nous devons implémenter une technique qui peut récupérer efficacement un sous-ensemble particulier d’enregistrements selon certains paramètres de l’Index de ligne de début et le nombre maximal de lignes. Il existe un certain nombre de techniques qui peuvent être utilisées pour atteindre cet objectif. Dans le didacticiel précédent, nous avons étudié pour accomplir cela à l’aide de Microsoft SQL Server 2005 s nouvelle `ROW_NUMBER()` fonction de classement. En bref, le `ROW_NUMBER()` fonction de classement attribue un numéro de ligne pour chaque ligne retournée par une requête qui est classée selon un ordre de tri spécifié. Le sous-ensemble d’enregistrements approprié est ensuite obtenu en retournant une section particulière des résultats numérotées. La requête suivante illustre comment utiliser cette technique pour retourner ces produits numérotés 11 à 20 lorsque le classement des résultats classés par ordre alphabétique par la `ProductName`:


[!code-sql[Main](sorting-custom-paged-data-vb/samples/sample1.sql)]

Cette technique fonctionne bien pour la pagination à l’aide d’un ordre de tri spécifique (`ProductName` triées par ordre alphabétique, dans ce cas), mais la requête doit être modifié pour afficher les résultats triés par une expression de tri différent. Dans l’idéal, la requête ci-dessus pourrait être réécrit pour utiliser un paramètre dans le `OVER` clause, comme suit :


[!code-sql[Main](sorting-custom-paged-data-vb/samples/sample2.sql)]

Malheureusement, paramétrés `ORDER BY` clauses ne sont pas autorisés. Au lieu de cela, nous devons créer une procédure stockée qui accepte un `@sortExpression` paramètre d’entrée, mais utilise une seule des solutions de contournement suivantes :

- Écrire des requêtes codées en dur pour chacune des expressions de tri qui peuvent être utilisées ; Ensuite, utilisez `IF/ELSE` instructions T-SQL pour déterminer la requête à exécuter.
- Utilisez un `CASE` instruction pour fournir dynamique `ORDER BY` expressions basées sur le `@sortExpressio` n paramètre d’entrée ; consultez l’utilisé à la section relative aux résultats de requête de tri dynamiquement dans [la puissance de SQL `CASE` instructions](http://www.4guysfromrolla.com/webtech/102704-1.shtml) Pour plus d’informations.
- Concevoir la requête appropriée sous forme de chaîne dans la procédure stockée, puis utilisez [le `sp_executesql` procédure stockée système](https://msdn.microsoft.com/library/ms188001.aspx) pour exécuter la requête dynamique.

Chacune de ces solutions de contournement présente certains inconvénients. La première option n’est pas aussi facile à gérer que les deux autres car elle nécessite que vous créez une requête pour chaque expression de tri possibles. Par conséquent, si vous décidez ultérieurement ajouter des champs de nouveau, pouvant être triés au GridView vous devrez également revenir en arrière et mettez à jour de la procédure stockée. La deuxième approche a des particularités qui présentent des problèmes de performances lors du tri des colonnes de la base de données autre qu’une chaîne et souffre également les mêmes problèmes de facilité de maintenance comme la première. Et le troisième choix, qui utilise SQL dynamique, introduit le risque d’attaque par injection SQL si une personne malveillante est en mesure d’exécuter la procédure stockée, en passant les valeurs de paramètre d’entrée de leur choix.

Si aucune de ces approches est parfait, je pense que la troisième option est la meilleure des trois. L’utilisation de code SQL dynamique, il offre un niveau de flexibilité que les deux autres ne le faites pas. En outre, une attaque d’injection SQL ne peut être exploitée que si une personne malveillante est en mesure d’exécuter la procédure stockée, en passant les paramètres d’entrée de son choix. Dans la mesure où la couche DAL utilise des requêtes paramétrables, ADO.NET permet de protéger ces paramètres sont envoyés à la base de données à travers l’architecture, ce qui signifie que la vulnérabilité d’attaque par injection de code SQL existe uniquement si l’attaquant peut exécuter directement la procédure stockée.

Pour implémenter cette fonctionnalité, créez une nouvelle procédure stockée dans la base de données Northwind nommé `GetProductsPagedAndSorted`. Cette procédure stockée doit accepter trois paramètres d’entrée : `@sortExpression`, un paramètre d’entrée de type `nvarchar(100`) qui spécifie la façon dont les résultats doivent être triées et est injecté directement après le `ORDER BY` texte dans le `OVER` clause ; et `@startRowIndex` et `@maximumRows`, les mêmes paramètres d’entrée de deux entiers à partir de la `GetProductsPaged` examiné dans le didacticiel précédent de procédure stockée. Créer le `GetProductsPagedAndSorted` procédure stockée utilisant le script suivant :


[!code-sql[Main](sorting-custom-paged-data-vb/samples/sample3.sql)]

La procédure stockée démarre en s’assurant qu’une valeur pour le `@sortExpression` paramètre a été spécifié. S’il est manquant, les résultats sont classés par `ProductID`. Ensuite, la requête SQL dynamique est construite. Notez que la requête SQL dynamique ici diffère légèrement de nos requêtes précédentes permet de récupérer toutes les lignes de la table Products. Dans les exemples précédents, nous avons obtenu à chaque catégorie associée produit s s et fournisseur noms s à l’aide d’une sous-requête. Cette décision a été effectuée dans le [création d’une couche d’accès aux données](../introduction/creating-a-data-access-layer-vb.md) didacticiel et a été effectuée à la place à l’aide de `JOIN` s, car le TableAdapter ne peut pas créer automatiquement l’insertion associée, mettre à jour et supprimer des méthodes pour par exemple requêtes. Le `GetProductsPagedAndSorted` une procédure stockée, cependant, doit utiliser `JOIN` s pour les résultats doivent être classés aux noms de catégorie ou le fournisseur.

Cette requête dynamique est développée en concaténant les parties de la requête statique et le `@sortExpression`, `@startRowIndex`, et `@maximumRows` paramètres. Dans la mesure où `@startRowIndex` et `@maximumRows` sont des paramètres de type entier, ils doivent être converties en nvarchars pour pouvoir être concaténés correctement. Une fois que cette requête SQL dynamique a été construite, elle est exécutée via `sp_executesql`.

Prenez un moment pour tester cette procédure stockée avec des valeurs différentes pour le `@sortExpression`, `@startRowIndex`, et `@maximumRows` paramètres. À partir de l’Explorateur de serveurs, avec le bouton droit sur le nom de la procédure stockée et choisissez Exécuter. Cela fera apparaître la boîte de dialogue Exécuter la procédure stockée dans laquelle vous pouvez entrer les paramètres d’entrée (voir Figure 1). Pour trier les résultats par le nom de catégorie, utilisez CategoryName pour le `@sortExpression` valeur du paramètre ; pour trier par nom de société fournisseur s, utilisez CompanyName. Après avoir entré les valeurs de paramètres, cliquez sur OK. Les résultats sont affichés dans la fenêtre Sortie. Figure 2 présente les résultats retournant les produits classés 11 à 20 lors de la commande par le `UnitPrice` dans l’ordre décroissant.


![Essayez les différentes valeurs des procédure stockée s trois paramètres d’entrée](sorting-custom-paged-data-vb/_static/image1.png)

**Figure 1**: essayer différentes valeurs pour la procédure stockée s trois paramètres d’entrée


[![La procédure stockée s les résultats sont affichés dans la fenêtre Sortie](sorting-custom-paged-data-vb/_static/image3.png)](sorting-custom-paged-data-vb/_static/image2.png)

**Figure 2**: la procédure stockée s les résultats sont affichés dans la fenêtre de sortie ([cliquez pour afficher l’image en taille réelle](sorting-custom-paged-data-vb/_static/image4.png))


> [!NOTE]
> Lorsque les résultats de classement par spécifié `ORDER BY` colonne dans la `OVER` clause, SQL Server doit trier les résultats. Ceci est une opération rapide s’il y a un index cluster sur l’ou les colonnes sont classées par les résultats ou s’il existe une couverture d’index, mais peut être plus coûteux dans le cas contraire. Pour améliorer les performances pour les requêtes suffisamment grands, envisagez d’ajouter un index non cluster pour la colonne selon laquelle les résultats sont triés par. Reportez-vous à [fonctions de classement et de performances dans SQL Server 2005](http://www.sql-server-performance.com/ak_ranking_functions.asp) pour plus d’informations.


## <a name="step-2-augmenting-the-data-access-and-business-logic-layers"></a>Étape 2 : Augmentation de l’accès aux données et les couches de logique métier

Avec le `GetProductsPagedAndSorted` une procédure stockée créée, l’étape suivante consiste à fournir un moyen pour exécuter cette procédure stockée via notre architecture d’application. Cela implique l’ajout d’une méthode appropriée à la couche DAL et la couche BLL. Permettent de commencer par ajouter une méthode à la couche DAL s. Ouvrir le `Northwind.xsd` DataSet typée, avec le bouton droit sur le `ProductsTableAdapter`, puis choisissez l’option Ajouter une requête dans le menu contextuel. Comme nous l’avons fait dans le didacticiel précédent, nous souhaitons configurer cette nouvelle méthode de la couche DAL pour utiliser une procédure stockée existante - `GetProductsPagedAndSorted`, dans ce cas. Commencez par indiquant que vous voulez que la nouvelle méthode de TableAdapter à utiliser une procédure stockée existante.


![Choisissez d’utiliser une procédure stockée existante](sorting-custom-paged-data-vb/_static/image5.png)

**Figure 3**: choisissez d’utiliser une procédure stockée existante


Pour spécifier la procédure stockée à utiliser, sélectionnez le `GetProductsPagedAndSorted` procédure dans la liste déroulante est stockée dans l’écran suivant.


![Utiliser le GetProductsPagedAndSorted procédure stockée](sorting-custom-paged-data-vb/_static/image6.png)

**Figure 4**: utiliser le GetProductsPagedAndSorted procédure stockée


Cette procédure stockée renvoie un jeu d’enregistrements comme ses résultats par conséquent, dans l’écran suivant, indiquent qu’il retourne des données tabulaires.


![Indiquer que la procédure stockée retourne des données tabulaires](sorting-custom-paged-data-vb/_static/image7.png)

**Figure 5**: indiquer que la procédure stockée retourne des données tabulaires


Enfin, de créer un jeu de méthodes de la couche DAL qui utilisent les deux le remplissage et de retourner un DataTable des modèles, les méthodes d’affectation de noms `FillPagedAndSorted` et `GetProductsPagedAndSorted`, respectivement.


![Choisissez les noms de méthodes](sorting-custom-paged-data-vb/_static/image8.png)

**Figure 6**: choisissez les noms de méthodes


Maintenant que nous avons ve étendu la couche DAL, nous vous êtes prêt à se tourner vers la couche BLL. Ouvrez le `ProductsBLL` fichier de classe et ajoutez une nouvelle méthode `GetProductsPagedAndSorted`. Cette méthode doit accepter trois paramètres d’entrée `sortExpression`, `startRowIndex`, et `maximumRows` et doit simplement appeler vers le bas dans la couche DAL s `GetProductsPagedAndSorted` (méthode), comme suit :


[!code-vb[Main](sorting-custom-paged-data-vb/samples/sample4.vb)]

## <a name="step-3-configuring-the-objectdatasource-to-pass-in-the-sortexpression-parameter"></a>Étape 3 : Configuration ObjectDataSource à passer dans le paramètre SortExpression

Avoir augmenté la couche DAL et la couche BLL afin d’inclure des méthodes qui utilisent la `GetProductsPagedAndSorted` une procédure stockée, tout ce qui reste consiste à configurer ObjectDataSource dans le `SortParameter.aspx` page à utiliser la nouvelle méthode de la couche de logique métier et à passer dans le `SortExpression` paramètre basé sur le colonne de l’utilisateur a demandé à trier les résultats.

Commencez par modifier le s ObjectDataSource `SelectMethod` de `GetProductsPaged` à `GetProductsPagedAndSorted`. Cela est possible via l’Assistant Configurer la Source de données, à partir de la fenêtre Propriétés, ou directement par le biais de la syntaxe déclarative. Ensuite, nous devons fournir une valeur pour les opérations de mappage ObjectDataSource [ `SortParameterName` propriété](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.sortparametername.aspx). Si cette propriété est définie, ObjectDataSource tente de passer dans le s GridView `SortExpression` propriété le `SelectMethod`. En particulier, ObjectDataSource recherche un paramètre d’entrée dont le nom est égal à la valeur de la `SortParameterName` propriété. Depuis la couche BLL s `GetProductsPagedAndSorted` méthode a le paramètre d’entrée de tri expression nommé `sortExpression`, définissez le s ObjectDataSource `SortExpression` propriété SortExpression.

Après avoir apporté ces deux modifications, la syntaxe déclarative de s ObjectDataSource doit ressembler à ce qui suit :


[!code-aspx[Main](sorting-custom-paged-data-vb/samples/sample5.aspx)]

> [!NOTE]
> Comme avec le didacticiel précédent, faire en sorte que ObjectDataSource cas *pas* inclure les paramètres d’entrée sortExpression, startRowIndex ou maximumRows dans sa collection SelectParameters.


Pour activer le tri dans le contrôle GridView, simplement cocher la case à cocher Activer le tri dans la balise active de s GridView, qui définit les opérations de mappage GridView `AllowSorting` propriété `true` et à l’origine le texte d’en-tête pour chaque colonne se présentent sous la forme d’un bouton de lien. Lorsque l’utilisateur final clique sur l’un de l’en-tête de type LinkButton, s’ensuit une publication (postback) et se produira comme suit :

1. Les mises à jour de GridView son [ `SortExpression` propriété](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.sortexpression.aspx) à la valeur de la `SortExpression` du champ dont le lien en-tête utilisateur a cliqué sur
2. ObjectDataSource appelle la couche BLL s `GetProductsPagedAndSorted` méthode, en passant le s GridView `SortExpression` propriété en tant que la valeur de la méthode s `sortExpression` paramètre d’entrée (ainsi que le texte approprié `startRowIndex` et `maximumRows` les valeurs de paramètre d’entrée)
3. La couche BLL appelle la couche DAL s `GetProductsPagedAndSorted` (méthode)
4. La couche DAL exécute le `GetProductsPagedAndSorted` procédure stockée, en passant dans le `@sortExpression` paramètre (avec le `@startRowIndex` et `@maximumRows` les valeurs de paramètre d’entrée)
5. La procédure stockée retourne un sous-ensemble approprié de données à la couche BLL, qui renvoie à ObjectDataSource ; ces données sont ensuite liées au GridView, rendues en HTML et envoyées à l’utilisateur final

La figure 7 illustre la première page de résultats triés par le `UnitPrice` dans l’ordre croissant.


[![Les résultats sont triés par le prix unitaire](sorting-custom-paged-data-vb/_static/image10.png)](sorting-custom-paged-data-vb/_static/image9.png)

**Figure 7**: les résultats sont triés par le prix unitaire ([cliquez pour afficher l’image en taille réelle](sorting-custom-paged-data-vb/_static/image11.png))


Bien que l’implémentation actuelle peut trier correctement les résultats par nom de produit, nom de catégorie, quantité par unité et prix unitaire, une tentative classer les résultats par le fournisseur nom aboutit à une exception runtime (voir Figure 8).


![Une tentative trier les résultats par les résultats de fournisseur dans l’Exception de Runtime suivante](sorting-custom-paged-data-vb/_static/image12.png)

**Figure 8**: tentative de trier les résultats par les résultats de fournisseur dans l’Exception de Runtime suivante


Cette exception se produit parce que le `SortExpression` des s GridView `SupplierName` BoundField est défini sur `SupplierName`. Toutefois, le fournisseur s nom dans le `Suppliers` table est effectivement appelée `CompanyName` nous avons été un alias de ce nom de colonne en tant que `SupplierName`. Toutefois, le `OVER` clause utilisé par le `ROW_NUMBER()` ne peut pas utiliser l’alias de fonction et doit utiliser le nom de colonne réelle. Par conséquent, modifiez le `SupplierName` BoundField s `SortExpression` à partir du nom de fournisseur CompanyName (voir Figure 9). Comme le montre la Figure 10, après cette modification, que les résultats peuvent être triées par le fournisseur.


![Modifier l’élément SortExpression s de nom fournisseur BoundField CompanyName](sorting-custom-paged-data-vb/_static/image13.png)

**Figure 9**: modifier l’élément SortExpression s de nom fournisseur BoundField CompanyName


[![Les résultats peuvent désormais être triées par fournisseur](sorting-custom-paged-data-vb/_static/image15.png)](sorting-custom-paged-data-vb/_static/image14.png)

**Figure 10**: les résultats peuvent désormais être triées par fournisseur ([cliquez pour afficher l’image en taille réelle](sorting-custom-paged-data-vb/_static/image16.png))


## <a name="summary"></a>Récapitulatif

L’implémentation de la pagination personnalisée, que nous avons examiné dans le didacticiel précédent obligatoire que l’ordre selon lequel les résultats étaient à trier être spécifié au moment du design. En bref, cela signifiait que l’implémentation de la pagination personnalisée, que nous avons implémenté pas pu, en même temps, fournir des fonctionnalités de tri. Dans ce didacticiel, nous avons surmonté cette limitation en étendant la procédure stockée à partir de la première à inclure un `@sortExpression` paramètre d’entrée par laquelle les résultats pourraient être triées.

Une fois la création de cette procédure stockée et création de nouvelles méthodes dans la couche DAL et la couche BLL, nous étions capables d’implémenter un GridView qui proposés à la fois le tri et personnalisées en configurant l’ObjectDataSource à passer dans le GridView/s en cours de pagination `SortExpression` propriété à la couche BLL `SelectMethod`.

Bonne programmation !

## <a name="about-the-author"></a>À propos de l’auteur

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), auteur de sept les livres sur ASP/ASP.NET et fondateur de [4GuysFromRolla.com](http://www.4guysfromrolla.com), travaille avec les technologies Web Microsoft depuis 1998. Scott fonctionne comme un consultant indépendant, formateur et writer. Son dernier ouvrage est [*SAM animer vous-même ASP.NET 2.0 des dernières 24 heures*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Il peut être contacté à [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou via son blog, qui se trouve à [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Remerciements

Cette série de didacticiels a été révisée par plusieurs réviseurs utiles. Entraîner un réviseur pour ce didacticiel a été Carlos Santos. Qui souhaitent consulter mes prochains articles MSDN ? Dans ce cas, envoyez-moi une ligne à [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Précédent](efficiently-paging-through-large-amounts-of-data-vb.md)
> [Suivant](creating-a-customized-sorting-user-interface-vb.md)
