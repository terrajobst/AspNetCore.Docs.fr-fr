---
uid: web-forms/overview/data-access/introduction/creating-a-business-logic-layer-cs
title: Création d’une couche de logique métier (c#) | Microsoft Docs
author: rick-anderson
description: Dans ce didacticiel, nous allons voir comment centraliser vos règles d’entreprise dans une couche BLL (Business Logic) qui sert d’intermédiaire pour l’échange de données entre t...
ms.author: riande
ms.date: 03/31/2010
ms.assetid: 85554606-47cb-4e4f-9848-eed9da579056
msc.legacyurl: /web-forms/overview/data-access/introduction/creating-a-business-logic-layer-cs
msc.type: authoredcontent
ms.openlocfilehash: 0db90f1e87bcaac51ca08ef1a8b258c93be8f613
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41825362"
---
<a name="creating-a-business-logic-layer-c"></a>Création d’une couche de logique métier (c#)
====================
par [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Télécharger l’exemple d’application](http://download.microsoft.com/download/4/6/3/463cf87c-4724-4cbc-b7b5-3f866f43ba50/ASPNET_Data_Tutorial_2_CS.exe) ou [télécharger le PDF](creating-a-business-logic-layer-cs/_static/datatutorial02cs1.pdf)

> Dans ce didacticiel, nous allons voir comment centraliser vos règles d’entreprise dans une couche BLL (Business Logic) qui sert d’intermédiaire pour l’échange de données entre la couche de présentation et la couche DAL.


## <a name="introduction"></a>Introduction

La couche DAL (Data Access) est créé dans le [premier didacticiel](creating-a-data-access-layer-cs.md) sépare correctement les données d’accès logique à partir de la logique de présentation. Toutefois, alors que la couche DAL sépare les détails d’accès aux données à partir de la couche de présentation, il n’applique pas les règles d’entreprise qui peuvent s’appliquer. Par exemple, pour notre application nous souhaiterons peut-être interdire la `CategoryID` ou `SupplierID` champs de la `Products` table d’être modifiées lorsque le `Discontinued` champ est défini sur 1, ou nous pourrions appliquer des règles d’ancienneté, interdisant les situations dans lesquelles un employé est géré par une personne qui a été embauchés après leur. Un autre scénario courant est d’autorisation peut-être seuls les utilisateurs dans un rôle particulier peuvent supprimer des produits ou peuvent modifier le `UnitPrice` valeur.

Dans ce didacticiel, nous allons voir comment centraliser ces règles d’entreprise dans une couche BLL (Business Logic) qui sert d’intermédiaire pour l’échange de données entre la couche de présentation et la couche DAL. Dans une application réelle, la couche BLL doit être implémentée comme un projet de bibliothèque de classes distinct ; Toutefois, pour ces didacticiels, nous allons implémenter la couche BLL comme une série de classes dans notre `App_Code` dossier afin de simplifier la structure de projet. La figure 1 illustre les relations architecturales entre la couche de présentation, BLL et DAL.


![La couche BLL sépare la couche de présentation de la couche d’accès aux données et impose des règles d’entreprise](creating-a-business-logic-layer-cs/_static/image1.png)

**Figure 1**: la couche BLL sépare la couche de présentation de la couche d’accès aux données et impose des règles d’entreprise


## <a name="step-1-creating-the-bll-classes"></a>Étape 1 : Création des Classes de couche de logique métier

Notre BLL est composée de quatre classes, une pour chaque TableAdapter dans la couche DAL ; chacune de ces classes de la couche BLL aura des méthodes pour récupérer, insertion, mise à jour et si vous supprimez le TableAdapter respectif dans la couche DAL, en appliquant les règles métier appropriée.

Pour plus de proprement séparer les classes liées BLL et DAL, nous allons créer deux sous-dossiers dans le `App_Code` dossier, `DAL` et `BLL`. Simplement avec le bouton droit sur le `App_Code` dossier dans l’Explorateur de solutions et choisissez Nouveau dossier. Après avoir créé ces deux dossiers, déplacer le DataSet typé créé dans le premier didacticiel dans le `DAL` sous-dossier.

Ensuite, créez les quatre fichiers de classe de couche de logique métier dans le `BLL` sous-dossier. Pour ce faire, cliquez sur le `BLL` sous-dossier, choisissez Ajouter un nouvel élément, puis choisissez le modèle de classe. Nommez les quatre classes `ProductsBLL`, `CategoriesBLL`, `SuppliersBLL`, et `EmployeesBLL`.


![Ajoutez quatre nouvelles Classes dans le dossier App_Code](creating-a-business-logic-layer-cs/_static/image2.png)

**Figure 2**: ajouter quatre nouvelles Classes pour la `App_Code` dossier


Ensuite, nous allons ajouter des méthodes pour chacune des classes simplement d’intégrer les méthodes définies pour les TableAdapters dans le premier didacticiel. Pour l’instant, ces méthodes simplement appelle directement dans la couche DAL ; Nous allons retourner par la suite pour ajouter une logique métier nécessaire.

> [!NOTE]
> Si vous utilisez Visual Studio Standard Edition ou ultérieur (autrement dit, vous êtes *pas* à l’aide de Visual Web Developer), vous pouvez éventuellement concevoir vos classes visuellement à l’aide de la [Concepteur de classes](https://msdn.microsoft.com/library/default.asp?url=/library/dv_vstechart/html/clssdsgnr.asp). Reportez-vous à la [classe concepteur Blog](https://blogs.msdn.com/classdesigner/default.aspx) pour plus d’informations sur cette nouvelle fonctionnalité dans Visual Studio.


Pour le `ProductsBLL` nous devons ajouter un total de sept méthodes de classe :

- `GetProducts()` Retourne tous les produits
- `GetProductByProductID(productID)` Retourne le produit avec l’ID de produit spécifié
- `GetProductsByCategoryID(categoryID)` Retourne tous les produits à partir de la catégorie spécifiée
- `GetProductsBySupplier(supplierID)` Retourne tous les produits du fournisseur spécifié
- `AddProduct(productName, supplierID, categoryID, quantityPerUnit, unitPrice, unitsInStock, unitsOnOrder, reorderLevel, discontinued)` Insère un nouveau produit dans la base de données en utilisant les valeurs passées dans ; Retourne le `ProductID` valeur de l’enregistrement qui vient d’être inséré.
- `UpdateProduct(productName, supplierID, categoryID, quantityPerUnit, unitPrice, unitsInStock, unitsOnOrder, reorderLevel, discontinued, productID)` met à jour un produit existant dans la base de données en utilisant les valeurs passées dans ; Retourne `true` si précisément une ligne a été mis à jour, `false` sinon
- `DeleteProduct(productID)` Supprime le produit spécifié à partir de la base de données

ProductsBLL.cs


[!code-csharp[Main](creating-a-business-logic-layer-cs/samples/sample1.cs)]

Les méthodes qui retournent des données `GetProducts`, `GetProductByProductID`, `GetProductsByCategoryID`, et `GetProductBySuppliersID` sont assez simples car ils appellent simplement vers le bas dans la couche DAL. Bien que dans certains scénarios il peut y avoir des règles d’entreprise qui doivent être implémentées à ce niveau (par exemple, les règles d’autorisation en fonction de l’utilisateur actuellement connecté ou le rôle auquel appartient l’utilisateur), nous laisserons simplement ces méthodes en tant que-est. Pour ces méthodes, puis, la couche BLL sert simplement un proxy via lequel la couche de présentation accède aux données sous-jacentes à partir de la couche d’accès aux données.

Le `AddProduct` et `UpdateProduct` méthodes à la fois prendre en tant que paramètres, les valeurs pour les différents champs de produit et ajouter un nouveau produit ou mettre à jour une existante, respectivement. Depuis de nombreuses le `Product` les colonnes de la table peuvent accepter `NULL` valeurs (`CategoryID`, `SupplierID`, et `UnitPrice`, pour citer que quelques), les paramètres d’entrée pour `AddProduct` et `UpdateProduct` qui correspondent à une telle utilisation de colonnes [types nullable](https://msdn.microsoft.com/library/1t3y8s4s(v=vs.80).aspx). Types Nullable débutez avec .NET 2.0 et fournir une technique de qui indique si un type valeur doit, au lieu de cela, être `null`. En c#, vous pouvez marquer un type valeur comme un type nullable en ajoutant `?` après le type (comme `int? x;`). Reportez-vous à la [Types Nullable](https://msdn.microsoft.com/library/1t3y8s4s.aspx) section dans le [Guide de programmation c#](https://msdn.microsoft.com/library/67ef8sbd%28VS.80%29.aspx) pour plus d’informations.

Les trois méthodes retournent une valeur booléenne indiquant si une ligne a été insérée, mise à jour ou supprimée, car l’opération peut ne pas entraîne une ligne concernée. Par exemple, si le développeur de pages appelle `DeleteProduct` en passant un `ProductID` pour un produit inexistante, la `DELETE` instruction émise à la base de données n’a aucun effet et par conséquent le `DeleteProduct` méthode retournera `false`.

Notez que lorsque vous ajoutez un nouveau produit ou de la mise à jour un nous prendre les valeurs du produit nouveau ou modifié des champs comme une liste de valeurs scalaires par opposition à accepter un `ProductsRow` instance. Cette approche a été choisie, car le `ProductsRow` classe dérive de ADO.NET `DataRow` (classe), qui n’a pas de constructeur sans paramètre par défaut. Pour créer un nouveau `ProductsRow` instance, nous devons d’abord créer un `ProductsDataTable` de l’instance, puis appelez ses `NewProductRow()` (méthode) (ce que nous faisons `AddProduct`). Cet inconvénient élevant sa tête lorsque nous tourner vers insertion et mise à jour de produits à l’aide de l’ObjectDataSource. En bref, ObjectDataSource tente de créer une instance des paramètres d’entrée. Si la méthode de la couche BLL attend un `ProductsRow` instance, ObjectDataSource tente de créer un, mais échouer en raison du manque d’un constructeur sans paramètre par défaut. Pour plus d’informations sur ce problème, consultez les publications de Forums ASP.NET deux suivantes : [ObjectDataSources de la mise à jour avec les jeux de données fortement typée](https://forums.asp.net/1098630/ShowPost.aspx), et [problème avec ObjectDataSource et fortement typée DataSet](https://forums.asp.net/1048212/ShowPost.aspx).

Ensuite, dans les deux `AddProduct` et `UpdateProduct`, le code crée un `ProductsRow` de l’instance et le remplit avec les valeurs passées simplement. Lors de l’affectation de valeurs à celui de DataColumns d’un DataRow plusieurs vérifications de validation au niveau du champ peuvent se produire. Par conséquent, remettre manuellement les valeurs transmises dans un DataRow permet de garantir la validité des données transmises à la méthode de la couche BLL. Malheureusement les classes de DataRow fortement typé générés par Visual Studio n’utilisent pas les types nullable. Au lieu de cela, pour indiquer qu’une colonne de données particulière dans un DataRow doit correspondre à un `NULL` de base de données de valeur, nous devons utiliser le `SetColumnNameNull()` (méthode).

Dans `UpdateProduct` nous charger tout d’abord dans le produit pour mettre à jour à l’aide de `GetProductByProductID(productID)`. Bien que cela puisse sembler un aller-retour inutile à la base de données, ce trajet supplémentaire s’avère utile dans les futures didacticiels qui explorent l’accès concurrentiel optimiste. L’accès concurrentiel optimiste est une technique pour vous assurer que deux utilisateurs qui travaillent simultanément sur les mêmes données écraser accidentellement les modifications par un autre. En saisissant l’enregistrement complet rend également plus facile de créer des méthodes de mise à jour dans la couche BLL qui modifient uniquement un sous-ensemble de colonnes de DataRow. Lorsque nous explorons les `SuppliersBLL` nous verrons un exemple de ce type de classe.

Enfin, notez que le `ProductsBLL` classe a le [attribut DataObject](https://msdn.microsoft.com/library/system.componentmodel.dataobjectattribute.aspx) appliquée (la `[System.ComponentModel.DataObject]` syntaxe juste avant l’instruction de classe vers le haut du fichier) et les méthodes ont [ Les attributs DataObjectMethodAttribute](https://msdn.microsoft.com/library/system.componentmodel.dataobjectmethodattribute.aspx). Le `DataObject` attribut marque la classe comme étant un objet pouvant être lié à un [contrôle ObjectDataSource](https://msdn.microsoft.com/library/9a4kyhcx.aspx), tandis que le `DataObjectMethodAttribute` indique l’objectif de la méthode. Nous verrons dans un avenir didacticiels, ObjectDataSource d’ASP.NET 2.0 rend plus facile de façon déclarative accéder aux données à partir d’une classe. Pour aider à filtrer la liste des classes possibles à lier dans l’Assistant de l’ObjectDataSource, par défaut uniquement les classes marquées comme `DataObjects` figurent dans la liste déroulante de l’Assistant. Le `ProductsBLL` classe fonctionnera aussi bien sans ces attributs, mais ajouter rend plus facile à utiliser dans l’Assistant de l’ObjectDataSource.

## <a name="adding-the-other-classes"></a>Ajout d’autres Classes

Avec la `ProductsBLL` classe complète, nous avons besoin ajouter des classes pour l’utilisation des catégories, les fournisseurs et les employés. Prenez un moment pour créer les classes et les concepts de l’exemple ci-dessus d’à l’aide des méthodes suivantes :

- **CategoriesBLL.cs**

    - `GetCategories()`
    - `GetCategoryByCategoryID(categoryID)`
- **SuppliersBLL.cs**

    - `GetSuppliers()`
    - `GetSupplierBySupplierID(supplierID)`
    - `GetSuppliersByCountry(country)`
    - `UpdateSupplierAddress(supplierID, address, city, country)`
- **EmployeesBLL.cs**

    - `GetEmployees()`
    - `GetEmployeeByEmployeeID(employeeID)`
    - `GetEmployeesByManager(managerID)`

La méthode à noter est la `SuppliersBLL` la classe `UpdateSupplierAddress` (méthode). Cette méthode fournit une interface pour la mise à jour uniquement les informations d’adresse du fournisseur. En interne, cette méthode lit dans le `SupplierDataRow` objet spécifié `supplierID` (à l’aide de `GetSupplierBySupplierID`), définit ses propriétés liées aux adresses et appelle ensuite vers le bas dans la `SupplierDataTable`de `Update` (méthode). Le `UpdateSupplierAddress` méthode suit :


[!code-csharp[Main](creating-a-business-logic-layer-cs/samples/sample2.cs)]

Reportez-vous au téléchargement de cet article pour mon implémentation complète de classes de la couche BLL.

## <a name="step-2-accessing-the-typed-datasets-through-the-bll-classes"></a>Étape 2 : L’accès aux groupes de données typés via les Classes de la couche de logique métier

Dans le premier didacticiel nous avons cité des exemples de travailler directement avec le DataSet typé par programmation, mais avec l’ajout de nos classes de la couche BLL, la couche de présentation doit fonctionner par rapport à la couche BLL à la place. Dans le `AllProducts.aspx` exemple à partir du premier didacticiel, le `ProductsTableAdapter` a été utilisée pour lier la liste des produits à un GridView, comme indiqué dans le code suivant :


[!code-csharp[Main](creating-a-business-logic-layer-cs/samples/sample3.cs)]

Pour utiliser la couche BLL de nouvelles classes, tout cela doit être modifiée est la première ligne de code suffit de remplacer le `ProductsTableAdapter` de l’objet avec un `ProductBLL` objet :


[!code-csharp[Main](creating-a-business-logic-layer-cs/samples/sample4.cs)]

Les classes de la couche BLL sont également accessible déclarative (comme vous pouvez le DataSet typé) à l’aide de l’ObjectDataSource. Nous allons aborder l’ObjectDataSource plus en détail dans les didacticiels suivants.


[![La liste de produits s’affiche dans un GridView](creating-a-business-logic-layer-cs/_static/image4.png)](creating-a-business-logic-layer-cs/_static/image3.png)

**Figure 3**: la liste de produits s’affiche dans un GridView ([cliquez pour afficher l’image en taille réelle](creating-a-business-logic-layer-cs/_static/image5.png))


## <a name="step-3-adding-field-level-validation-to-the-datarow-classes"></a>Étape 3 : Ajout d’une Validation au niveau du champ pour les Classes de DataRow

Validation au niveau du champ sont les vérifications relatives aux valeurs de propriété des objets métier pendant l’insertion ou la mise à jour. Certaines règles de validation au niveau du champ pour les produits sont les suivants :

- Le `ProductName` champ doit être de 40 caractères ou moins
- Le `QuantityPerUnit` champ doit être de 20 caractères ou moins
- Le `ProductID`, `ProductName`, et `Discontinued` champs sont obligatoires, mais tous les autres champs sont facultatifs
- Le `UnitPrice`, `UnitsInStock`, `UnitsOnOrder`, et `ReorderLevel` champs doivent être supérieure ou égale à zéro

Ces règles peuvent et doivent être exprimés au niveau de la base de données. La limite de caractères sur la `ProductName` et `QuantityPerUnit` champs sont capturées par les types de données de ces colonnes dans le `Products` table (`nvarchar(40)` et `nvarchar(20)`, respectivement). Si les champs sont obligatoires et facultatifs sont exprimées par si la colonne de table de base de données autorise `NULL` s. Quatre [contraintes check](https://msdn.microsoft.com/library/ms188258.aspx) existe, visant à garantir que seules les valeurs supérieures ou égales à zéro peuvent rendre dans le `UnitPrice`, `UnitsInStock`, `UnitsOnOrder`, ou `ReorderLevel` colonnes.

En plus en appliquant ces règles à la base de données qu’ils doivent également être appliquées au niveau du jeu de données. En fait, la longueur de champ et si une valeur est obligatoire ou facultatif sont déjà capturés pour l’ensemble de chaque DataTable de DataColumns. Pour afficher la validation au niveau du champ existante fournie automatiquement, accédez au Concepteur de DataSet, sélectionnez un champ à partir d’une des tables et puis accédez à la fenêtre Propriétés. Comme le montre la Figure 4, le `QuantityPerUnit` DataColumn dans le `ProductsDataTable` a une longueur maximale de 20 caractères et autorise `NULL` valeurs. Si vous tentez de définir la `ProductsDataRow`de `QuantityPerUnit` propriété une valeur de chaîne plue de 20 caractères un `ArgumentException` sera levée.


[![Le DataColumn fournit une Validation au niveau du champ de base](creating-a-business-logic-layer-cs/_static/image7.png)](creating-a-business-logic-layer-cs/_static/image6.png)

**Figure 4**: le DataColumn fournit au niveau du champ Validation de base ([cliquez pour afficher l’image en taille réelle](creating-a-business-logic-layer-cs/_static/image8.png))


Malheureusement, nous ne pouvons pas spécifier les vérifications des limites, telles que la `UnitPrice` valeur doit être supérieure ou égale à zéro, via la fenêtre Propriétés. Pour fournir ce type de validation au niveau du champ que nous avons besoin créer un gestionnaire d’événements pour le DataTable [ColumnChanging](https://msdn.microsoft.com/library/system.data.datatable.columnchanging%28VS.80%29.aspx) événement. Comme mentionné dans le [didacticiel précédent](creating-a-data-access-layer-cs.md), les objets DataSet, DataTables et DataRow créés par le DataSet typé peuvent être étendus via l’utilisation des classes partielles. À l’aide de cette technique, nous pouvons créer un `ColumnChanging` Gestionnaire d’événements pour le `ProductsDataTable` classe. Commencez par créer une classe dans le `App_Code` dossier nommé `ProductsDataTable.ColumnChanging.cs`.


[![Ajoutez une nouvelle classe dans le dossier App_Code](creating-a-business-logic-layer-cs/_static/image10.png)](creating-a-business-logic-layer-cs/_static/image9.png)

**Figure 5**: ajouter une nouvelle classe à la `App_Code` dossier ([cliquez pour afficher l’image en taille réelle](creating-a-business-logic-layer-cs/_static/image11.png))


Ensuite, créez un gestionnaire d’événements pour le `ColumnChanging` événement qui garantit que le `UnitPrice`, `UnitsInStock`, `UnitsOnOrder`, et `ReorderLevel` les valeurs de colonne (si ce n’est pas `NULL`) sont supérieurs ou égaux à zéro. Si une telle colonne est hors limites, lever une `ArgumentException`.

ProductsDataTable.ColumnChanging.cs


[!code-csharp[Main](creating-a-business-logic-layer-cs/samples/sample5.cs)]

## <a name="step-4-adding-custom-business-rules-to-the-blls-classes"></a>Étape 4 : Ajout de règles d’entreprise personnalisées pour les Classes de la couche BLL

En plus de la validation au niveau du champ, il peut exister des règles d’entreprise personnalisées haut niveau qui impliquent des entités différentes ou des concepts ne pouvant pas être exprimées au niveau de la colonne unique, tel que :

- Si un produit a été supprimé, son `UnitPrice` ne peut pas être mis à jour
- Pays d’un employé de résidence doit être le même que le pays de son gestionnaire de résidence
- Un produit ne peut pas être supprimé s’il s’agit du seul produit fourni par le fournisseur

Les classes de la couche BLL doivent contenir des vérifications pour assurer la conformité aux règles d’entreprise de l’application. Ces vérifications peuvent être ajoutées directement aux méthodes auquel elles s’appliquent.

Imaginez que notre entreprise déterminée par les règles qu’un produit pas pu être marqué supprimée s’il s’agissait du seul produit à partir d’un fournisseur donné. Autrement dit, si produit *X* a été le seul produit nous achetés à partir du fournisseur *Y*, nous ne pouvons pas marquer *X* comme interrompue ; si, toutefois, fournisseur *Y*nous fourni avec trois produits, *A*, *B*, et *C*, puis nous aurions pu marquer tout et tous ces éléments comme abandonné. Une règle d’entreprise impair, mais les règles d’entreprise et le bon sens ne sont pas alignées toujours !

Pour appliquer cette règle d’entreprise dans le `UpdateProducts` méthode nous démarre en vérifiant si `Discontinued` a été défini sur `true` et, si par conséquent, nous devrions appeler `GetProductsBySupplierID` pour déterminer combien de produits que nous avons acheté à partir de ce fournisseur. Si seul un produit acheté à partir de ce fournisseur, nous levons une `ApplicationException`.


[!code-csharp[Main](creating-a-business-logic-layer-cs/samples/sample6.cs)]

## <a name="responding-to-validation-errors-in-the-presentation-tier"></a>Répondre aux erreurs de Validation dans la couche de présentation

Lors de l’appel de la couche BLL à partir de la couche de présentation que nous pouvons décider tenter de gérer les exceptions qui peuvent être déclenchées ou leur permettent de se propagent dans ASP.NET (ce qui déclenche la `HttpApplication`de `Error` événement). Pour gérer une exception lorsque vous travaillez par programmation de la couche BLL, nous pouvons utiliser un [try... catch](https://msdn.microsoft.com/library/0yd65esw.aspx) bloc, comme le montre l’exemple suivant :


[!code-csharp[Main](creating-a-business-logic-layer-cs/samples/sample7.cs)]

Comme nous le verrons dans un avenir didacticiels, gestion des exceptions qui se propagent de la couche BLL lors de l’utilisation de données Web de contrôle pour l’insertion, la mise à jour, ou la suppression des données peut être gérée directement dans un gestionnaire d’événements au lieu d’avoir à la ligne de code dans `try...catch` blocs.

## <a name="summary"></a>Récapitulatif

Une application bien conçue est judicieusement de couches distinctes, chacune encapsule un rôle particulier. Dans le premier didacticiel de cette série d’articles, nous avons créé une couche d’accès aux données à l’aide de données typés ; Dans ce didacticiel nous intégrée une couche de logique métier en tant que série de classes de notre application `App_Code` dossier appel descendant dans notre DAL. La couche BLL implémente la logique au niveau du champ et de niveau entreprise pour notre application. Outre la création d’une couche de logique métier distinct, comme nous l’avons fait dans ce didacticiel, une autre option consiste à étendre les méthodes des TableAdapters via l’utilisation des classes partielles. Toutefois, à l’aide de cette technique ne permet pas de substituer des méthodes existantes, ni il sépare notre DAL et notre BLL comme correctement en tant que l’approche que nous avons pris dans cet article.

Avec la couche DAL et de la couche BLL terminée, nous sommes prêts à démarrer sur notre couche de présentation. Dans le [didacticiel suivant](master-pages-and-site-navigation-cs.md) nous allons prendre un bref détour à partir des rubriques d’accès aux données et définir une disposition de page cohérente à utiliser dans les didacticiels.

Bonne programmation !

## <a name="about-the-author"></a>À propos de l’auteur

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), auteur de sept les livres sur ASP/ASP.NET et fondateur de [4GuysFromRolla.com](http://www.4guysfromrolla.com), travaille avec les technologies Web Microsoft depuis 1998. Scott fonctionne comme un consultant indépendant, formateur et writer. Son dernier ouvrage est [*SAM animer vous-même ASP.NET 2.0 des dernières 24 heures*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Il peut être contacté à [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou via son blog, qui se trouve à [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Remerciements

Cette série de didacticiels a été révisée par plusieurs réviseurs utiles. Les réviseurs tête pour ce didacticiel ont été Liz Shulok, Dennis Patterson, Carlos Santos et Hilton Giesenow. Qui souhaitent consulter mes prochains articles MSDN ? Dans ce cas, envoyez-moi une ligne à [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Précédent](creating-a-data-access-layer-cs.md)
> [Suivant](master-pages-and-site-navigation-cs.md)
