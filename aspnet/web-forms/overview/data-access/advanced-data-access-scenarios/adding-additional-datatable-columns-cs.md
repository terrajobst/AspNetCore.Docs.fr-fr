---
uid: web-forms/overview/data-access/advanced-data-access-scenarios/adding-additional-datatable-columns-cs
title: Ajout de colonnes de DataTable supplémentaires (c#) | Microsoft Docs
author: rick-anderson
description: Lorsque vous utilisez l’Assistant TableAdapter pour créer un DataSet typé, le DataTable contient les colonnes retournées par la requête de base de données principale. Mais là...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/18/2007
ms.topic: article
ms.assetid: 615f3361-f21f-4338-8bc1-fce8ae071de9
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/data-access/advanced-data-access-scenarios/adding-additional-datatable-columns-cs
msc.type: authoredcontent
ms.openlocfilehash: 373ba6d3ec86ea2ab26bfecf39647776694960de
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37393981"
---
<a name="adding-additional-datatable-columns-c"></a>Ajout de colonnes de DataTable supplémentaires (c#)
====================
par [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Télécharger le Code](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_70_CS.zip) ou [télécharger le PDF](adding-additional-datatable-columns-cs/_static/datatutorial70cs1.pdf)

> Lorsque vous utilisez l’Assistant TableAdapter pour créer un DataSet typé, le DataTable contient les colonnes retournées par la requête de base de données principale. Mais il arrive parfois lorsque la table de données doit inclure des colonnes supplémentaires. Dans ce didacticiel, nous Découvrez pourquoi les procédures stockées sont recommandées lorsque nous avons besoin des colonnes de DataTable supplémentaires.


## <a name="introduction"></a>Introduction

Lorsque vous ajoutez un TableAdapter à un DataSet typé, le schéma s DataTable correspondant est déterminé par la requête principale de TableAdapter s. Par exemple, si la requête principale renvoie les champs de données *A*, *B*, et *C*, la table de données aura trois colonnes correspondantes nommées *A*, *B*, et *C*. Outre sa requête principale, un TableAdapter peut inclure des requêtes supplémentaires qui retournent, par exemple, un sous-ensemble des données en fonction de certains paramètres. Par exemple, en plus de la `ProductsTableAdapter` requête principale s, qui retourne des informations sur tous les produits, elle contient également des méthodes comme `GetProductsByCategoryID(categoryID)` et `GetProductByProductID(productID)`, qui renvoient des informations de produit spécifique en fonction d’un paramètre fourni.

Le modèle d’avoir le schéma s DataTable reflètent la requête principale de TableAdapter s fonctionne bien si toutes les méthodes TableAdapter s retournent les mêmes ou moins de champs de données que celles spécifiées dans la requête principale. Si une méthode du TableAdapter doit retourner les champs de données supplémentaires, nous devons développer le schéma s DataTable en conséquence. Dans le [maître/détail à l’aide d’une liste à puces des enregistrements maîtres avec une DataList des détails](../filtering-scenarios-with-the-datalist-and-repeater/master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs.md) didacticiel, nous avons ajouté une méthode à la `CategoriesTableAdapter` qui a retourné le `CategoryID`, `CategoryName`, et `Description` définies dans les champs de données la requête principale plue `NumberOfProducts`, un champ de données supplémentaires qui a signalé le nombre de produits associées à chaque catégorie. Nous avons ajouté manuellement une nouvelle colonne à la `CategoriesDataTable` afin de capturer le `NumberOfProducts` valeur à partir de cette nouvelle méthode de champ de données.

Comme indiqué dans le [télécharger des fichiers](../working-with-binary-files/uploading-files-cs.md) didacticiel, vous devez veiller avec les TableAdapters qui utilisent des instructions SQL ad hoc et ont des méthodes dont les champs de données ne correspondent pas précisément la requête principale. Si l’Assistant Configuration de TableAdapter est relancée, elle sera mise à jour toutes les méthodes TableAdapter s afin que leur liste de champs de données correspond à la requête principale. Par conséquent, toutes les méthodes avec des listes de colonne personnalisé seront revenir à la liste de colonnes de la requête principale s et ne retourne pas les données attendues. Ce problème ne se pose pas lors de l’utilisation de procédures stockées.

Dans ce didacticiel, nous allons examiner comment étendre un schéma de s DataTable pour inclure des colonnes supplémentaires. En raison de la fragilité du TableAdapter lors de l’utilisation d’instructions SQL ad hoc, dans ce didacticiel, nous allons utiliser des procédures stockées. Reportez-vous à la [création de nouvelles procédures stockées pour s DataSet typée TableAdapters](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs.md) et [à l’aide des procédures stockées existantes pour s DataSet typée TableAdapters](https://data-access/tutorials/using-existing-stored-procedures-for-the-typed-dataset-amp-rsquo-s-tableadapters-cs) didacticiels pour plus d’informations sur configuration d’un TableAdapter pour utiliser des procédures stockées.

## <a name="step-1-adding-apricequartilecolumn-to-theproductsdatatable"></a>Étape 1 : Ajout d’un`PriceQuartile`colonne à la`ProductsDataTable`

Dans le *création de nouvelles procédures stockées pour s DataSet typée TableAdapters* didacticiel, nous avons créé un DataSet typé nommé `NorthwindWithSprocs`. Ce jeu de données contient actuellement des deux DataTables : `ProductsDataTable` et `EmployeesDataTable`. Le `ProductsTableAdapter` a trois méthodes suivantes :

- `GetProducts` -la requête principale, qui retourne tous les enregistrements à partir de la `Products` table
- `GetProductsByCategoryID(categoryID)` -Retourne tous les produits avec la valeur *categoryID*.
- `GetProductByProductID(productID)` -Retourne le produit avec la valeur *productID*.

La requête principale et toutes les deux méthodes supplémentaires retournent le même ensemble de champs de données, à savoir toutes les colonnes à partir de la `Products` table. Il n’y a aucune sous-requêtes corrélées ou `JOIN` s extraction de données connexes à partir de la `Categories` ou `Suppliers` tables. Par conséquent, le `ProductsDataTable` a une colonne correspondante pour chaque champ dans le `Products` table.

Pour ce didacticiel, s permettent d’ajouter une méthode à la `ProductsTableAdapter` nommé `GetProductsWithPriceQuartile` qui retourne tous les produits. En plus des champs de données de produit standard, `GetProductsWithPriceQuartile` inclut également un `PriceQuartile` champ de données qui indique dans quel quartile se situe le prix du produit s. Par exemple, les produits dont les prix sont dans les 25 % plus coûteuses aura un `PriceQuartile` la valeur 1, tandis que ceux dont les prix se situent dans la partie inférieure de 25 % aura une valeur de 4. Avant de nous soucier de la création de la procédure stockée pour retourner ces informations, toutefois, nous devons d’abord mettre à jour le `ProductsDataTable` pour inclure une colonne contenant le `PriceQuartile` résultats lorsque la `GetProductsWithPriceQuartile` méthode est utilisée.

Ouvrez le `NorthwindWithSprocs` jeu de données et avec le bouton droit sur le `ProductsDataTable`. Cliquez sur Ajouter dans le menu contextuel, puis choisissez colonne.


[![Ajouter une nouvelle colonne à la ProductsDataTable](adding-additional-datatable-columns-cs/_static/image2.png)](adding-additional-datatable-columns-cs/_static/image1.png)

**Figure 1**: ajouter une nouvelle colonne à la `ProductsDataTable` ([cliquez pour afficher l’image en taille réelle](adding-additional-datatable-columns-cs/_static/image3.png))


Cela ajoutera une nouvelle colonne au DataTable nommé Column1 de type `System.String`. Nous devons mettre à jour de ce nom de colonne s à PriceQuartile et son type `System.Int32` , car il sera utilisé pour contenir un nombre compris entre 1 et 4. Sélectionnez la colonne nouvellement ajouté dans le `ProductsDataTable` et, à partir de la fenêtre Propriétés, définissez la `Name` propriété PriceQuartile et le `DataType` propriété `System.Int32`.


[![Définir les propriétés de type de données et le nouveau nom de colonne s](adding-additional-datatable-columns-cs/_static/image5.png)](adding-additional-datatable-columns-cs/_static/image4.png)

**Figure 2**: définir la nouvelle colonne s `Name` et `DataType` propriétés ([cliquez pour afficher l’image en taille réelle](adding-additional-datatable-columns-cs/_static/image6.png))


Comme le montre la Figure 2, il existe des propriétés supplémentaires qui peuvent être définies, telles que si les valeurs dans la colonne doivent être uniques, si la colonne est une colonne à incrémentation automatique, la base de données ou non `NULL` valeurs sont autorisées et ainsi de suite. Laissez ces valeurs définies à leurs valeurs par défaut.

## <a name="step-2-creating-thegetproductswithpricequartilemethod"></a>Étape 2 : Création de la`GetProductsWithPriceQuartile`(méthode)

Maintenant que le `ProductsDataTable` a été mis à jour pour inclure le `PriceQuartile` colonne, nous sommes prêts à créer la `GetProductsWithPriceQuartile` (méthode). Démarrer en cliquant sur le TableAdapter et en choisissant Ajouter une requête dans le menu contextuel. Ceci fait apparaître l’Assistant Configuration de requêtes TableAdapter, qui demande tout d’abord nous si nous souhaitons utiliser les instructions SQL ad hoc ou une procédure stockée nouveau ou existante. Étant donné que nous ne pas mais une procédure stockée qui retourne les données de quartile prix, permettent de s autoriser le TableAdapter créer cette procédure stockée pour nous. Sélectionnez l’option de procédure stockée nouveau de créer et cliquez sur Suivant.


[![Demander à l’Assistant TableAdapter pour créer la procédure stockée pour nous](adding-additional-datatable-columns-cs/_static/image8.png)](adding-additional-datatable-columns-cs/_static/image7.png)

**Figure 3**: demander à l’Assistant TableAdapter pour créer le stockées procédure pour nous ([cliquez pour afficher l’image en taille réelle](adding-additional-datatable-columns-cs/_static/image9.png))


Dans l’écran suivant, illustré Figure 4, l’Assistant nous demande quel type de requête à ajouter. Dans la mesure où le `GetProductsWithPriceQuartile` méthode retournera toutes les colonnes et les enregistrements à partir de la `Products` de table, sélectionnez l’instruction SELECT qui retourne des lignes, cliquez sur Suivant.


[![Notre requête sera une instruction SELECT qui retourne plusieurs lignes](adding-additional-datatable-columns-cs/_static/image11.png)](adding-additional-datatable-columns-cs/_static/image10.png)

**Figure 4**: notre requête sera un `SELECT` instruction qui retourne plusieurs lignes ([cliquez pour afficher l’image en taille réelle](adding-additional-datatable-columns-cs/_static/image12.png))


Ensuite, nous sommes invités pour le `SELECT` requête. Dans l’Assistant, entrez la requête suivante :


[!code-sql[Main](adding-additional-datatable-columns-cs/samples/sample1.sql)]

La requête ci-dessus utilise SQL Server 2005 s nouveau [ `NTILE` fonction](https://msdn.microsoft.com/library/ms175126.aspx) pour diviser les résultats en quatre groupes où les groupes sont déterminées par la `UnitPrice` valeurs triées dans l’ordre décroissant.

Malheureusement, le Générateur de requêtes ne sait pas comment analyser le `OVER` mot clé et affichera une erreur lors de l’analyse de la requête ci-dessus. Par conséquent, entrez la requête ci-dessus directement dans la zone de texte dans l’Assistant sans utiliser le Générateur de requêtes.

> [!NOTE]
> Pour plus d’informations sur les s NTILE et SQL Server 2005 autres fonctions de classement, consultez [retour de résultats classés avec Microsoft SQL Server 2005](http://www.4guysfromrolla.com/webtech/010406-1.shtml) et [section fonctions de classement](https://msdn.microsoft.com/library/ms189798.aspx) à partir de la [SQL Server 2005 Books Online](https://msdn.microsoft.com/library/ms189798.aspx).


Après avoir entré la `SELECT` requête et en cliquant sur Suivant, l’Assistant nous invite à fournir un nom pour la procédure stockée est créé. Nommez la nouvelle procédure stockée `Products_SelectWithPriceQuartile` et cliquez sur Suivant.


[![Nom de la procédure stockée Products_SelectWithPriceQuartile](adding-additional-datatable-columns-cs/_static/image14.png)](adding-additional-datatable-columns-cs/_static/image13.png)

**Figure 5**: nom de la procédure stockée `Products_SelectWithPriceQuartile` ([cliquez pour afficher l’image en taille réelle](adding-additional-datatable-columns-cs/_static/image15.png))


Enfin, nous allons vous y êtes invités à nommer les méthodes TableAdapter. Laissez les deux le remplissage un DataTable et retourner un DataTable les cases cochées et les méthodes `FillWithPriceQuartile` et `GetProductsWithPriceQuartile`.


[![Nom du TableAdapter s méthodes et cliquez sur Terminer](adding-additional-datatable-columns-cs/_static/image17.png)](adding-additional-datatable-columns-cs/_static/image16.png)

**Figure 6**: nommer le TableAdapter s méthodes et cliquez sur Terminer ([cliquez pour afficher l’image en taille réelle](adding-additional-datatable-columns-cs/_static/image18.png))


Avec le `SELECT` requête spécifiée et la procédure stockée et les méthodes TableAdapter nommés, cliquez sur Terminer pour terminer l’Assistant. À ce stade que vous receviez un message d’avertissement ou deux à partir de l’Assistant, indiquant que le `OVER` construction SQL ou une instruction n’est pas pris en charge. Vous pouvez ignorer ces avertissements.

À l’issue de l’Assistant, le TableAdapter doit inclure le `FillWithPriceQuartile` et `GetProductsWithPriceQuartile` méthodes et la base de données doivent inclure une procédure stockée nommée `Products_SelectWithPriceQuartile`. Prenez un moment pour vérifier que le TableAdapter ne contient en effet de cette nouvelle méthode et que la procédure stockée a été correctement ajoutée à la base de données. Lors de la vérification de la base de données, si vous ne voyez pas la procédure stockée try effectuant un clic droit sur le dossier de procédures stockées et en choisissant d’actualisation.


![Vérifiez qu’une nouvelle méthode a été ajoutée au TableAdapter](adding-additional-datatable-columns-cs/_static/image19.png)

**Figure 7**: Vérifiez qu’une nouvelle méthode a été ajoutée au TableAdapter


[![Assurez-vous que la base de données contienne le Products_SelectWithPriceQuartile procédure stockée](adding-additional-datatable-columns-cs/_static/image21.png)](adding-additional-datatable-columns-cs/_static/image20.png)

**Figure 8**: Vérifiez que la base de données contient le `Products_SelectWithPriceQuartile` la procédure stockée ([cliquez pour afficher l’image en taille réelle](adding-additional-datatable-columns-cs/_static/image22.png))


> [!NOTE]
> Un des avantages de l’utilisation de procédures stockées au lieu d’instructions SQL ad hoc est que réexécuter l’Assistant Configuration de TableAdapter ne modifiera pas les listes de colonnes de procédures stockées. Vérifier cela en effectuant un clic droit sur le TableAdapter, en choisissant l’option de configuration dans le menu contextuel pour démarrer l’Assistant et puis en cliquant sur Terminer pour terminer. Ensuite, accédez à la base de données et la vue la `Products_SelectWithPriceQuartile` procédure stockée. Notez que sa liste de colonnes n’a pas été modifiée. Avions nous avons été à l’aide des instructions SQL ad hoc, exécuter à nouveau l’Assistant Configuration de TableAdapter aurait été rétablies cette liste de colonnes de requête s pour correspondre à la liste des colonnes requête principale, en supprimant l’instruction NTILE à partir de la requête utilisée par le `GetProductsWithPriceQuartile` (méthode).


Lors de la couche d’accès aux données s `GetProductsWithPriceQuartile` méthode est appelée, le TableAdapter exécute le `Products_SelectWithPriceQuartile` procédure stockée et ajoute une ligne à la `ProductsDataTable` pour chaque enregistrement retourné. Les champs de données retournés par la procédure stockée sont mappées à la `ProductsDataTable` des colonnes de s. Dans la mesure où il existe un `PriceQuartile` champ de données retourné à partir de la procédure stockée, sa valeur est assignée à la `ProductsDataTable` s `PriceQuartile` colonne.

Pour les méthodes TableAdapter dont les requêtes ne retournent pas un `PriceQuartile` champ de données, le `PriceQuartile` valeur de colonne s est la valeur spécifiée par son `DefaultValue` propriété. Comme le montre la Figure 2, cette valeur est définie sur `DBNull`, la valeur par défaut. Si vous préférez une valeur par défaut différente, il suffit de définir le `DefaultValue` propriété en conséquence. Assurez-vous simplement que le `DefaultValue` valeur n’est valide selon la colonne s `DataType` (par exemple, `System.Int32` pour la `PriceQuartile` colonne).

À ce stade, nous avons effectué les étapes nécessaires pour l’ajout d’une colonne supplémentaire à un DataTable. Pour vérifier que cette colonne supplémentaire fonctionne comme prévu, permettent de créer une page ASP.NET qui affiche chaque produit s nom, prix et quartile de prix s. Avant cela, cependant, nous devons d’abord mettre à jour de la couche de logique métier pour inclure une méthode qui appelle vers le bas de la couche DAL s `GetProductsWithPriceQuartile` (méthode). Nous mettre à jour de la couche BLL ensuite, à l’étape 3 et ensuite créer la page ASP.NET à l’étape 4.

## <a name="step-3-augmenting-the-business-logic-layer"></a>Étape 3 : Augmentation de la couche de logique métier

Avant de pouvoir utiliser le nouveau `GetProductsWithPriceQuartile` méthode à partir de la couche de présentation, nous devons tout d’abord ajouter une méthode correspondante à la couche BLL. Ouvrez le `ProductsBLLWithSprocs` fichier de classe et ajoutez le code suivant :


[!code-csharp[Main](adding-additional-datatable-columns-cs/samples/sample2.cs)]

Comme les autres méthodes de récupération de données dans `ProductsBLLWithSprocs`, le `GetProductsWithPriceQuartile` méthode appelle simplement la couche DAL s correspondant `GetProductsWithPriceQuartile` méthode et retourne ses résultats.

## <a name="step-4-displaying-the-price-quartile-information-in-an-aspnet-web-page"></a>Étape 4 : Afficher les informations de Quartile prix dans une Page Web ASP.NET

Avec l’ajout de la couche BLL terminer nous re prêt à créer une page ASP.NET qui affiche le quartile prix pour chaque produit. Ouvrir le `AddingColumns.aspx` page dans le `AdvancedDAL` dossier et faites glisser un GridView à partir de la boîte à outils vers le concepteur, en définissant son `ID` propriété `Products`. À partir de la balise active de s GridView, liez-le à une nouvelle ObjectDataSource nommé `ProductsDataSource`. Configurer l’ObjectDataSource à utiliser le `ProductsBLLWithSprocs` classe s `GetProductsWithPriceQuartile` (méthode). Dans la mesure où il s’agit d’une grille en lecture seule, définissez les listes déroulantes dans la mise à jour, insertion et supprimer des onglets à (None).


[![Configurer pour utiliser la classe ProductsBLLWithSprocs ObjectDataSource](adding-additional-datatable-columns-cs/_static/image24.png)](adding-additional-datatable-columns-cs/_static/image23.png)

**Figure 9**: configurer ObjectDataSource à utiliser le `ProductsBLLWithSprocs` classe ([cliquez pour afficher l’image en taille réelle](adding-additional-datatable-columns-cs/_static/image25.png))


[![Récupérer des informations sur les produits à partir de la méthode GetProductsWithPriceQuartile](adding-additional-datatable-columns-cs/_static/image27.png)](adding-additional-datatable-columns-cs/_static/image26.png)

**Figure 10**: récupérer des informations sur le produit à partir de la `GetProductsWithPriceQuartile` (méthode) ([cliquez pour afficher l’image en taille réelle](adding-additional-datatable-columns-cs/_static/image28.png))


À l’issue de l’Assistant Configurer la Source de données, Visual Studio ajoute automatiquement un BoundField ou du CheckBoxField au GridView pour chacun des champs de données retournés par la méthode. Un de ces champs de données est `PriceQuartile`, qui est la colonne que nous avons ajouté à la `ProductsDataTable` à l’étape 1.

Modifier les champs de s GridView, supprimant tout sauf la `ProductName`, `UnitPrice`, et `PriceQuartile` BoundFields. Configurer le `UnitPrice` BoundField à mettre en forme sa valeur comme une devise et ont le `UnitPrice` et `PriceQuartile` BoundFields et centre alignée à droite, respectivement. Enfin, mettez à jour de la BoundFields restantes `HeaderText` propriétés produit, les prix et les prix Quartile, respectivement. En outre, cochez la case à cocher Activer le tri à partir de la balise active de GridView s.

Après ces modifications, le balisage déclaratif s GridView et ObjectDataSource doit ressembler à ce qui suit :


[!code-aspx[Main](adding-additional-datatable-columns-cs/samples/sample3.aspx)]

Figure 11 illustre cette page quand consultées via un navigateur. Notez que, tout d’abord, les produits sont classés par leur prix dans l’ordre décroissant avec chaque produit est attribué approprié `PriceQuartile` valeur. Bien entendu ces données peuvent être triées par d’autres critères, avec la valeur de colonne de prix Quartile toujours refléter le classement de produit s en ce qui concerne les prix (voir Figure 12).


[![Les produits sont triés par leurs prix](adding-additional-datatable-columns-cs/_static/image30.png)](adding-additional-datatable-columns-cs/_static/image29.png)

**Figure 11**: les produits sont triés par leurs prix ([cliquez pour afficher l’image en taille réelle](adding-additional-datatable-columns-cs/_static/image31.png))


[![Les produits sont classés par nom](adding-additional-datatable-columns-cs/_static/image33.png)](adding-additional-datatable-columns-cs/_static/image32.png)

**Figure 12**: les produits sont triés par leur nom ([cliquez pour afficher l’image en taille réelle](adding-additional-datatable-columns-cs/_static/image34.png))


> [!NOTE]
> Avec quelques lignes de code nous aurions pu augmenter le contrôle GridView afin qu’il les lignes de produit en fonction de la couleur leurs `PriceQuartile` valeur. Nous pourrions couleur ces produits dans le premier quartile vert clair, celles figurant dans le deuxième quartile un jaune clair et ainsi de suite. Je vous encourage à prendre un moment pour ajouter cette fonctionnalité. Si vous avez besoin d’un rappel de mise en forme d’un GridView, consultez le [mise en forme en fonction lors de données personnalisées](../custom-formatting/custom-formatting-based-upon-data-cs.md) didacticiel.


## <a name="an-alternative-approach---creating-another-tableadapter"></a>Une autre approche - création d’un autre TableAdapter

Comme nous l’avons vu dans ce didacticiel, lors de l’ajout d’une méthode à un TableAdapter qui renvoie les champs de données autres que ceux décrits par la requête principale, nous pouvons ajouter des colonnes correspondantes à la table de données. Une telle approche, cependant, fonctionne bien uniquement s’il existe un petit nombre de méthodes dans le TableAdapter qui retournent des différents champs de données et si ces champs de données de remplacement ne varient pas trop à partir de la requête principale.

Au lieu d’ajouter des colonnes à la table de données, vous pouvez ajouter un autre TableAdapter au jeu de données qui contient les méthodes à partir de la première TableAdapter qui retournent des différents champs de données. Pour ce didacticiel, plutôt que d’ajouter le `PriceQuartile` colonne à la `ProductsDataTable` (où il est utilisé uniquement par le `GetProductsWithPriceQuartile` (méthode)), nous aurions pu ajouter un TableAdapter supplémentaire au jeu de données nommé `ProductsWithPriceQuartileTableAdapter` servant le `Products_SelectWithPriceQuartile` stockées procédure en tant que sa requête principale. Les pages ASP.NET qui nécessaires pour obtenir des informations de produit avec le quartile prix utiliserait le `ProductsWithPriceQuartileTableAdapter`, tandis que ceux qui n’a pas pu continuer à utiliser le `ProductsTableAdapter`.

En ajoutant un nouveau TableAdapter, les tables de données restent untarnished et leurs colonnes reflètent précisément les champs de données retournés par leurs méthodes de s TableAdapter. Toutefois, les TableAdapters supplémentaires peut introduire des fonctionnalités et les tâches répétitives. Par exemple, si ces des pages ASP.NET qui affiche le `PriceQuartile` colonne également nécessaire pour fournir insert, update et delete prise en charge, le `ProductsWithPriceQuartileTableAdapter` doit avoir son `InsertCommand`, `UpdateCommand`, et `DeleteCommand` propriétés correctement configuré. Bien que ces propriétés mettrait en miroir le `ProductsTableAdapter` s, cette configuration présente une étape supplémentaire. En outre, il existe maintenant deux façons de mettre à jour, supprimer ou ajouter un produit à la base de données - via la `ProductsTableAdapter` et `ProductsWithPriceQuartileTableAdapter` classes.

Le téléchargement de ce didacticiel inclut une `ProductsWithPriceQuartileTableAdapter` classe dans le `NorthwindWithSprocs` jeu de données qui illustre cette approche alternative.

## <a name="summary"></a>Récapitulatif

Dans la plupart des scénarios, toutes les méthodes dans un TableAdapter retournera le même ensemble de champs de données, mais parfois, quand une méthode particulière ou deux devra peut-être retourner un champ supplémentaire. Par exemple, dans le [maître/détail à l’aide d’une liste à puces des enregistrements maîtres avec une DataList des détails](../filtering-scenarios-with-the-datalist-and-repeater/master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs.md) didacticiel, nous avons ajouté une méthode à la `CategoriesTableAdapter` qui, en plus des champs de données de la requête principale s, retournés un `NumberOfProducts` champ qui a signalé le nombre de produits associées à chaque catégorie. Dans ce didacticiel nous avons vu de l’ajout d’une méthode dans le `ProductsTableAdapter` qui a retourné un `PriceQuartile` champ en plus des champs de données de la requête principale s. Pour capturer des données supplémentaires champs retournés par les méthodes de s TableAdapter que nous devons ajouter des colonnes correspondantes dans le DataTable.

Si vous envisagez sur l’ajout manuel des colonnes à la table de données, il est recommandé que le TableAdapter utiliser des procédures stockées. Si le TableAdapter utilise des instructions SQL ad hoc, n’importe quel moment de l’Assistant Configuration de TableAdapter est exécuté toutes les méthodes rétablir des listes de champs de données pour les champs de données retournés par la requête principale. Ce problème ne s’étend pas aux procédures stockées, c’est pourquoi ils sont recommandés et ont été utilisés dans ce didacticiel.

Bonne programmation !

## <a name="about-the-author"></a>À propos de l’auteur

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), auteur de sept les livres sur ASP/ASP.NET et fondateur de [4GuysFromRolla.com](http://www.4guysfromrolla.com), travaille avec les technologies Web Microsoft depuis 1998. Scott fonctionne comme un consultant indépendant, formateur et writer. Son dernier ouvrage est [*SAM animer vous-même ASP.NET 2.0 des dernières 24 heures*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Il peut être contacté à [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou via son blog, qui se trouve à [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Remerciements

Cette série de didacticiels a été révisée par plusieurs réviseurs utiles. Les réviseurs tête pour ce didacticiel ont été Randy Schmidt, Goor Jacky, Bernadette Leigh et Hilton Giesenow. Qui souhaitent consulter mes prochains articles MSDN ? Dans ce cas, envoyez-moi une ligne à [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Précédent](updating-the-tableadapter-to-use-joins-cs.md)
> [Suivant](working-with-computed-columns-cs.md)
