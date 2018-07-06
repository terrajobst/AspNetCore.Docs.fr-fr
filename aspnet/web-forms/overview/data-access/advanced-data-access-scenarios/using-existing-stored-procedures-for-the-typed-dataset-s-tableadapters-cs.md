---
uid: web-forms/overview/data-access/advanced-data-access-scenarios/using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs
title: Utilisation existante des procédures stockées pour les TableAdapters de DataSet typé (c#) | Microsoft Docs
author: rick-anderson
description: Dans le didacticiel précédent, nous avons appris à utiliser l’Assistant TableAdapter pour générer de nouvelles procédures stockées. Dans ce didacticiel vous apprendre comment le TableAdapter même...
ms.author: aspnetcontent
ms.date: 07/18/2007
ms.assetid: 440bef2a-1641-4238-99e3-8e2d44e7d94c
msc.legacyurl: /web-forms/overview/data-access/advanced-data-access-scenarios/using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs
msc.type: authoredcontent
ms.openlocfilehash: 3057b7c296c82b3db740d9fd345aaa222ca31328
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37828868"
---
<a name="using-existing-stored-procedures-for-the-typed-datasets-tableadapters-c"></a>Utilisation existante des procédures stockées pour les TableAdapters de DataSet typé (c#)
====================
par [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Télécharger le Code](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_68_CS.zip) ou [télécharger le PDF](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/datatutorial68cs1.pdf)

> Dans le didacticiel précédent, nous avons appris à utiliser l’Assistant TableAdapter pour générer de nouvelles procédures stockées. Dans ce didacticiel vous apprendre le fonctionnement de l’Assistant TableAdapter même avec des procédures stockées existantes. Nous avons également comment ajouter manuellement les nouvelles procédures stockées à notre base de données.


## <a name="introduction"></a>Introduction

Dans le [didacticiel précédent](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs.md) nous l’avons vu comment le DataSet typée TableAdapters pourrait être configurés pour utiliser des procédures stockées pour l’accès aux données, plutôt qu’ad hoc des instructions SQL. En particulier, nous avons examiné comment l’Assistant TableAdapter automatiquement crée ces procédures stockées. Lors du portage d’une application héritée vers ASP.NET 2.0 ou lors de la création d’un site Web ASP.NET 2.0 autour d’un modèle de données existant, sans doute que la base de données contient déjà les procédures stockées que nous avons besoin. Vous pouvez également créer vos procédures stockées à la main ou via un outil autre que l’Assistant TableAdapter qui génère automatiquement vos procédures stockées.

Dans ce didacticiel, nous allons examiner comment configurer le TableAdapter pour utiliser des procédures stockées existantes. Dans la mesure où la base de données Northwind n’a qu’un petit ensemble de procédures stockées intégrées, nous allons également examiner les étapes nécessaires pour ajouter manuellement les nouvelles procédures stockées dans la base de données via l’environnement Visual Studio. Laissez s commencer !

> [!NOTE]
> Dans le [encapsulant les Modifications de base de données dans une Transaction](../working-with-batched-data/wrapping-database-modifications-within-a-transaction-cs.md) didacticiel, nous avons ajouté des méthodes au TableAdapter pour prendre en charge des transactions (`BeginTransaction`, `CommitTransaction`, et ainsi de suite). Vous pouvez également les transactions peuvent être gérées entièrement dans une procédure stockée, qui ne requiert aucune modification au code de couche d’accès aux données. Dans ce didacticiel, nous allons explorer les commandes T-SQL utilisées pour exécuter une procédure stockée s les instructions dans l’étendue d’une transaction.


## <a name="step-1-adding-stored-procedures-to-the-northwind-database"></a>Étape 1 : Ajouter des procédures stockées à la base de données Northwind

Visual Studio facilite l’ajout de nouvelles procédures stockées pour une base de données. S permettent d’ajouter une nouvelle procédure stockée à la base de données Northwind qui retourne toutes les colonnes à partir de la `Products` table pour ceux qui ont un particulier `CategoryID` valeur. Dans la fenêtre Explorateur de serveurs, développez la base de données Northwind afin que ses dossiers - les schémas de base de données, Tables, vues et ainsi de suite - sont affichés. Comme nous l’avons vu dans le didacticiel précédent, le dossier Stored Procedures contient les base de données s des procédures stockées existantes. Pour ajouter une nouvelle procédure stockée, cliquez simplement avec le bouton droit le dossier Stored Procedures et choisissez l’option Ajouter une nouvelle procédure stockée dans le menu contextuel.


[![Cliquez sur le dossier de procédures stockées et ajouter une nouvelle procédure stockée](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image2.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image1.png)

**Figure 1**: cliquez sur le dossier de procédures stockées et ajouter une nouvelle procédure stockée ([cliquez pour afficher l’image en taille réelle](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image3.png))


Comme le montre la Figure 1, en sélectionnant l’option Ajouter une nouvelle procédure stockée ouvre une fenêtre de script dans Visual Studio avec le contour du script SQL nécessaire pour créer la procédure stockée. C’est notre travail pour étoffer ce script et l’exécuter, à quel point la procédure stockée sera ajoutée à la base de données.

Entrez le script suivant :


[!code-sql[Main](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/samples/sample1.sql)]

Ce script, lors de l’exécution, ajoutera une nouvelle procédure stockée à la base de données Northwind nommé `Products_SelectByCategoryID`. Cette procédure stockée accepte un seul paramètre d’entrée (`@CategoryID`, de type `int`) et elle retourne tous les champs pour les produits avec une mise en correspondance `CategoryID` valeur.

Pour exécuter cette `CREATE PROCEDURE` de script et ajoutez la procédure stockée à la base de données, cliquez sur l’icône Enregistrer dans la barre d’outils ou appuyez sur Ctrl + S. Après cela, les actualisations de dossier Stored Procedures, montrant nouvellement créé la procédure stockée. En outre, le script dans la fenêtre changera subtilité de `CREATE PROCEDURE dbo.Products_SelectProductByCategoryID` à `ALTER PROCEDURE` `dbo.Products_SelectProductByCategoryID`. `CREATE PROCEDURE` Ajoute une nouvelle procédure stockée à la base de données, tandis que `ALTER PROCEDURE` mettre à jour. Étant donné que le début du script est passé à `ALTER PROCEDURE`, modifier les procédures stockées d’entrée paramètres ou des instructions SQL et en cliquant sur l’icône Enregistrer met à jour la procédure stockée avec ces modifications.

La figure 2 montre Visual Studio après le `Products_SelectByCategoryID` procédure stockée a été enregistrée.


[![La procédure stockée Products_SelectByCategoryID a été ajouté à la base de données](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image5.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image4.png)

**Figure 2**: la procédure stockée `Products_SelectByCategoryID` a été ajouté à la base de données ([cliquez pour afficher l’image en taille réelle](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image6.png))


## <a name="step-2-configuring-the-tableadapter-to-use-an-existing-stored-procedure"></a>Étape 2 : Configuration du TableAdapter pour utiliser une procédure stockée existante

Maintenant que le `Products_SelectByCategoryID` procédure stockée a été ajouté à la base de données, nous pouvons concigure notre couche d’accès aux données pour utiliser cette procédure stockée lorsqu’une de ses méthodes est appelée. En particulier, nous allons ajouter un `GetProducstByCategoryID(categoryID)` méthode à la `ProductsTableAdapter` dans le `NorthwindWithSprocs` DataSet typée qui appelle le `Products_SelectByCategoryID` nous venons de créer de procédure stockée.

Commencez par ouvrir le `NorthwindWithSprocs` jeu de données. Avec le bouton droit sur le `ProductsTableAdapter` et choisissez Ajouter une requête pour lancer l’Assistant Configuration de requête TableAdapter. Dans le [didacticiel précédent](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs.md) nous avons opté pour que le TableAdapter à créer une nouvelle procédure stockée pour nous. Pour ce didacticiel, toutefois, nous souhaitons associer la nouvelle méthode TableAdapter à l’objet existant `Products_SelectByCategoryID` procédure stockée. Par conséquent, choisissez l’option de procédure stockée existante utiliser à partir de l’étape de première Assistant s et puis cliquez sur Suivant.


[![Choisissez l’utilisation existante d’Option de procédure stockée](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image8.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image7.png)

**Figure 3**: choisissez l’utiliser l’existant de la procédure Option stockée ([cliquez pour afficher l’image en taille réelle](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image9.png))


L’écran suivant fournit qu'une liste déroulante remplie avec la base de données s des procédures stockées. Sélection d’une procédure stockée répertorie ses paramètres d’entrée sur la gauche et les champs de données retournés (le cas échéant) sur la droite. Choisissez le `Products_SelectByCategoryID` procédure dans la liste et cliquez sur Suivant.


[![Choisir le Products_SelectByCategoryID procédure stockée](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image11.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image10.png)

**Figure 4**: choisir le `Products_SelectByCategoryID` la procédure stockée ([cliquez pour afficher l’image en taille réelle](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image12.png))


L’écran suivant nous demande quel type de données est retourné par la procédure stockée et notre réponse ici détermine le type retourné par la méthode de s TableAdapter. Par exemple, si nous indiquent que les données tabulaires sont retournées, la méthode retournera un `ProductsDataTable` instance remplie avec les enregistrements renvoyés par la procédure stockée. En revanche, si nous indiquent que cette procédure stockée retourne une valeur unique TableAdapter retournera un `object` qui est assigné à la valeur dans la première colonne du premier enregistrement retourné par la procédure stockée.

Dans la mesure où le `Products_SelectByCategoryID` procédure stockée retourne tous les produits qui appartiennent à une catégorie particulière, sélectionnez la première réponse - des données tabulaires et cliquez sur Suivant.


[![Indiquer que la procédure stockée retourne des données tabulaires](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image14.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image13.png)

**Figure 5**: indiquer que la procédure stockée retourne des données tabulaires ([cliquez pour afficher l’image en taille réelle](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image15.png))


Ne reste qu’à indiquer ce que les modèles de méthode à utiliser suivie des noms de ces méthodes. Laissez les deux le remplissage une DataTable et renvoyer un options DataTable cochée, mais renommez les méthodes à `FillByCategoryID` et `GetProductsByCategoryID`. Puis cliquez sur Suivant pour consulter un résumé des tâches que l’Assistant va effectuer. Si tout semble correct, cliquez sur Terminer.


[![Nom de la FillByCategoryID de méthodes et GetProductsByCategoryID](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image17.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image16.png)

**Figure 6**: nommez les méthodes `FillByCategoryID` et `GetProductsByCategoryID` ([cliquez pour afficher l’image en taille réelle](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image18.png))


> [!NOTE]
> Les méthodes TableAdapter que nous venons de créer, `FillByCategoryID` et `GetProductsByCategoryID`, un paramètre d’entrée de type attendu `int`. Cette valeur de paramètre d’entrée est transmise dans la procédure stockée via son `@CategoryID` paramètre. Si vous modifiez le `Products_SelectByCategory` stockées des paramètres de procédure s, vous devez également mettre à jour les paramètres de ces méthodes TableAdapter. Comme indiqué dans le [didacticiel précédent](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs.md), cela est possible de deux manières : par l’ajout ou de supprimer manuellement les paramètres à partir de la collection de paramètres ou en réexécutant l’Assistant TableAdapter.


## <a name="step-3-adding-agetproductsbycategoryidcategoryidmethod-to-the-bll"></a>Étape 3 : Ajout d’un`GetProductsByCategoryID(categoryID)`à la couche BLL (méthode)

Avec le `GetProductsByCategoryID` DAL terminée, l’étape suivante consiste à fournir un accès à cette méthode dans la couche de logique métier. Ouvrez le `ProductsBLLWithSprocs` fichier de classe et ajoutez la méthode suivante :


[!code-csharp[Main](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/samples/sample2.cs)]

Cette méthode de la couche BLL renvoie simplement le `ProductsDataTable` retourné à partir de la `ProductsTableAdapter` s `GetProductsByCategoryID` (méthode). Le `DataObjectMethodAttribute` attribut fournit des métadonnées utilisée par l’Assistant Configurer la Source de données de s ObjectDataSource. En particulier, cette méthode s’affiche dans la liste déroulante de s onglet sélection.

## <a name="step-4-displaying-products-by-category"></a>Étape 4 : Affichage des produits par catégorie

Pour tester récemment ajouté `Products_SelectByCategoryID` procédure stockée et les méthodes correspondantes DAL et la couche BLL, permettent de créer une page ASP.NET qui contient un contrôle DropDownList et un GridView s. L’objet DropDownList listera toutes les catégories dans la base de données tandis que le contrôle GridView affiche les produits appartenant à la catégorie sélectionnée.

> [!NOTE]
> Nous les interfaces maître/détail ve créé à l’aide de DropDownList dans les didacticiels précédents. Pour plus approfondie sur l’implémentation de ce rapport maître/détail, reportez-vous à la [filtrage de maître/détail avec un DropDownList](../masterdetail/master-detail-filtering-with-a-dropdownlist-cs.md) didacticiel.


Ouvrez le `ExistingSprocs.aspx` page dans le `AdvancedDAL` dossier et faites glisser un contrôle DropDownList à partir de la boîte à outils vers le concepteur. Définir les opérations de mappage DropDownList `ID` propriété `Categories` et son `AutoPostBack` propriété `true`. Ensuite, à partir de sa balise active, lier l’objet DropDownList pour un nouveau ObjectDataSource nommé `CategoriesDataSource`. Configurer l’ObjectDataSource afin qu’il récupère ses données à partir de la `CategoriesBLL` classe s `GetCategories` (méthode). Définir les listes déroulantes dans la mise à jour, insertion et supprimer des onglets à (None).


[![Récupérer des données à partir de la méthode GetCategories CategoriesBLL classe s](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image20.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image19.png)

**Figure 7**: récupérer les données à partir de la `CategoriesBLL` classe s `GetCategories` (méthode) ([cliquez pour afficher l’image en taille réelle](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image21.png))


[![Définir les listes déroulantes dans la mise à jour, insertion et supprimer des onglets à (None)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image23.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image22.png)

**Figure 8**: la valeur est la liste déroulante répertorie dans la mise à jour, insertion et supprimer des onglets (aucun) ([cliquez pour afficher l’image en taille réelle](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image24.png))


À l’issue de l’Assistant ObjectDataSource, configurer la liste DropDownList pour afficher le `CategoryName` champ de données et d’utiliser le `CategoryID` champ en tant que le `Value` pour chaque `ListItem`.

À ce stade, le balisage déclaratif s DropDownList et ObjectDataSource doit similaire à ce qui suit :


[!code-aspx[Main](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/samples/sample3.aspx)]

Ensuite, faites glisser un GridView sur le concepteur, en le plaçant en dessous de la liste DropDownList. Définir les opérations de mappage GridView `ID` à `ProductsByCategory` et, à partir de sa balise active, liez-le à une nouvelle ObjectDataSource nommé `ProductsByCategoryDataSource`. Configurer le `ProductsByCategoryDataSource` ObjectDataSource à utiliser le `ProductsBLLWithSprocs` (classe), en fait, il récupérer ses données à l’aide de la `GetProductsByCategoryID(categoryID)` (méthode). Car ce GridView est uniquement utilisé pour afficher des données, définir les listes déroulantes dans la mise à jour, insérer, supprimer des onglets (aucun) et cliquez sur Suivant.


[![Configurer pour utiliser la classe ProductsBLLWithSprocs ObjectDataSource](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image26.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image25.png)

**Figure 9**: configurer ObjectDataSource à utiliser le `ProductsBLLWithSprocs` classe ([cliquez pour afficher l’image en taille réelle](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image27.png))


[![Récupérer des données à partir de la méthode GetProductsByCategoryID(categoryID)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image29.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image28.png)

**Figure 10**: récupérer les données à partir de la `GetProductsByCategoryID(categoryID)` (méthode) ([cliquez pour afficher l’image en taille réelle](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image30.png))


La méthode choisie dans l’onglet Sélection attend un paramètre, donc l’étape finale de l’Assistant nous demande la source du paramètre s. Définition de la liste de liste déroulante de source de paramètre au contrôle et choisissez la `Categories` contrôle à partir de la liste déroulante ControlID. Cliquez sur Terminer pour terminer l’Assistant.


[![Utilisez la liste DropDownList catégories comme Source de paramètre categoryID](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image32.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image31.png)

**Figure 11**: utilisez le `Categories` DropDownList comme Source de la `categoryID` paramètre ([cliquez pour afficher l’image en taille réelle](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image33.png))


À la fin de l’Assistant ObjectDataSource, Visual Studio ajoute BoundFields et un CheckBoxField pour chacun des champs de données de produit. N’hésitez pas à personnaliser ces champs comme vous le souhaitez.

Visitez la page via un navigateur. Lorsque vous visitez la page de que la catégorie boissons est sélectionnée et les produits correspondants répertoriés dans la grille. Modification de la liste déroulante à une autre catégorie, comme la Figure 12 montre, provoque une publication (postback) et recharge la grille avec les produits de la catégorie qui vient d’être sélectionnée.


[![Les produits dans la catégorie de produit sont affichés.](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image35.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image34.png)

**Figure 12**: les produits dans la catégorie de produit sont affichés ([cliquez pour afficher l’image en taille réelle](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image36.png))


## <a name="step-5-wrapping-a-stored-procedure-s-statements-within-the-scope-of-a-transaction"></a>Étape 5 : Encapsulant une instructions s de procédure stockée dans l’étendue d’une Transaction

Dans le [encapsulant les Modifications de base de données dans une Transaction](../working-with-batched-data/wrapping-database-modifications-within-a-transaction-cs.md) didacticiel, nous avons abordé les techniques d’exécution d’une série d’instructions de modification de base de données dans l’étendue d’une transaction. Souvenez-vous que les modifications effectuées dans le cadre d’une transaction, soit que toutes réussissent ou échouent, garantir l’atomicité. Techniques pour l’utilisation de transactions sont les suivantes :

- En utilisant les classes dans le `System.Transactions` espace de noms,
- La couche d’accès aux données d’utiliser les classes ADO.NET comme `SqlTransaction`, et
- Ajouter les commandes de transaction de T-SQL directement dans la procédure stockée

Le *encapsulant les Modifications de base de données dans une Transaction* didacticiel utilisé les classes ADO.NET dans la couche DAL. Le reste de ce didacticiel examine comment gérer une transaction à l’aide des commandes T-SQL à partir d’une procédure stockée.

Les trois commandes SQL clés pour un démarrage manuel, la validation et la restauration d’une transaction sont `BEGIN TRANSACTION`, `COMMIT TRANSACTION`, et `ROLLBACK TRANSACTION`, respectivement. Comme avec l’approche ADO.NET, lors de l’utilisation de transactions à partir d’une procédure stockée, nous devons appliquer le modèle suivant :

1. Indiquer le début d’une transaction.
2. Exécutez les instructions SQL qui composent la transaction.
3. S’il existe une erreur dans l’une des instructions à l’étape 2, restaurez la transaction
4. Si toutes les instructions à l’étape 2 se termine sans erreur, validez la transaction.

Ce modèle peut être implémenté dans la syntaxe T-SQL en utilisant le modèle suivant :


[!code-sql[Main](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/samples/sample4.sql)]

Le modèle commence par définir un `TRY...CATCH` bloquer, une construction de nouveau vers SQL Server 2005. Comme avec les `try...catch` bloque en c#, le code SQL `TRY...CATCH` bloc exécute les instructions dans le `TRY` bloc. Si n’importe quelle instruction génère une erreur, le contrôle est immédiatement transféré à la `CATCH` bloc.

Si aucune erreur de l’exécution des instructions SQL cette composition de la transaction, la `COMMIT TRANSACTION` instruction valide les modifications et termine la transaction. Si, toutefois, une des instructions entraîne une erreur, le `ROLLBACK TRANSACTION` dans le `CATCH` bloc retourne la base de données à son état avant le début de la transaction. La procédure stockée génère également une erreur à l’aide de la [commande RAISERROR](https://msdn.microsoft.com/library/ms178592.aspx), ce qui conduit un `SqlException` être relevées dans l’application.

> [!NOTE]
> Dans la mesure où le `TRY...CATCH` bloc est une nouveauté de SQL Server 2005, le modèle ci-dessus ne fonctionnera pas si vous utilisez des versions antérieures de Microsoft SQL Server. Si vous n’utilisez pas SQL Server 2005, consultez [la gestion des Transactions dans les procédures stockées SQL Server](http://www.4guysfromrolla.com/webtech/080305-1.shtml) pour un modèle qui fonctionne avec d’autres versions de SQL Server.


Laissez s examiner un exemple concret. Une contrainte de clé étrangère existe entre le `Categories` et `Products` tables, ce qui signifie que chaque `CategoryID` champ dans le `Products` table doit correspondre à un `CategoryID` valeur dans le `Categories` table. Toute action qui viole cette contrainte, tels que la tentative de suppression d’une catégorie qui est associé à des produits, entraîne une violation de contrainte de clé étrangère. Pour vérifier ceci, revisiter l’exemple de mise à jour et suppression des données binaires existantes dans la section de données binaires utilisation (`~/BinaryData/UpdatingAndDeleting.aspx`). Cette page répertorie chaque catégorie dans le système, ainsi que les boutons Modifier et supprimer (voir Figure 13), mais si vous essayez de supprimer une catégorie qui est associé à des produits - tels que des boissons - la suppression échoue en raison d’une violation de contrainte de clé étrangère (voir Figure 14).


[![Chaque catégorie est affichée dans un GridView avec les boutons Modifier et supprimer](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image38.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image37.png)

**Figure 13**: chaque catégorie est affichée dans un GridView avec les boutons Modifier et supprimer ([cliquez pour afficher l’image en taille réelle](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image39.png))


[![Vous ne pouvez pas supprimer une catégorie de produits existants](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image41.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image40.png)

**Figure 14**: vous ne pouvez pas supprimer une catégorie de produits existants ([cliquez pour afficher l’image en taille réelle](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image42.png))


Imaginez, cependant, que nous voulons autoriser des catégories pour être supprimée indépendamment de si ils sont associés à des produits. Supprimer une catégorie de produits, imaginez que nous souhaitons également supprimer ses produits existantes (même si une autre option consisterait à simplement définir ses produits `CategoryID` valeurs `NULL`). Cette fonctionnalité pourrait être implémentée via les règles en cascade de la contrainte de clé étrangère. Vous pouvez également nous pourrions créer une procédure stockée qui accepte un `@CategoryID` paramètre d’entrée et, lorsqu’elle est appelée, supprime de manière explicite tous les produits associés, puis la catégorie spécifiée.

Notre première tentative de ce type de procédure stockée peut se présenter comme suit :


[!code-sql[Main](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/samples/sample5.sql)]

Ceci supprimera définitivement la catégorie et les produits associés, il n’effectue pas, dans le cadre d’une transaction. Imaginez qu’il existe une autre contrainte de clé étrangère sur `Categories` cela empêchera la suppression d’un particulier `@CategoryID` valeur. Le problème est que dans ce cas tous les produits seront supprimées avant que nous tentons de supprimer la catégorie. Le résultat net est que pour une catégorie de ce type, cette procédure stockée supprime toutes ses produits tandis que la catégorie est restée dans la mesure où il contient toujours des enregistrements dans une autre table.

Si la procédure stockée ont été encapsulée dans l’étendue d’une transaction, toutefois, les suppressions à la `Products` table seront annulée en cas d’échec de la suppression `Categories`. Le script suivant de la procédure stockée utilise une transaction pour garantir l’atomicité entre les deux `DELETE` instructions :


[!code-sql[Main](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/samples/sample6.sql)]

Prenez un moment pour ajouter le `Categories_Delete` une procédure stockée à la base de données Northwind. Font référence à l’étape 1 pour obtenir des instructions sur l’ajout de procédures stockées pour une base de données.

## <a name="step-6-updating-thecategoriestableadapter"></a>Étape 6 : Mise à jour le`CategoriesTableAdapter`

Pendant que nous avons ajouté la `Categories_Delete` une procédure stockée à la base de données, la couche DAL est actuellement configurée pour utiliser des instructions SQL ad hoc pour effectuer la suppression. Nous devons mettre à jour le `CategoriesTableAdapter` et lui demander d’utiliser le `Categories_Delete` procédure stockée à la place.

> [!NOTE]
> Précédemment dans ce didacticiel nous travaillions avec le `NorthwindWithSprocs` jeu de données. Mais que le jeu de données a uniquement une seule entité, `ProductsDataTable`, et nous devons utiliser des catégories. Par conséquent, pour le reste de ce didacticiel lorsque j’aborderai le m Data Access Layer I faisant référence à la `Northwind` jeu de données, celui que nous avons tout d’abord créés dans le [création d’une couche d’accès aux données](../introduction/creating-a-data-access-layer-cs.md) didacticiel.


Ouvrez le Northwind DataSet, sélectionnez le `CategoriesTableAdapter`et accédez à la fenêtre Propriétés. Les listes de la fenêtre Propriétés la `InsertCommand`, `UpdateCommand`, `DeleteCommand`, et `SelectCommand` utilisé par le TableAdapter, ainsi que ses informations de nom et une connexion. Développez le `DeleteCommand` propriété pour afficher ses détails. Comme le montre la Figure 15, le `DeleteCommand` s `ComamndType` propriété a la valeur texte, ce qui lui indique pour envoyer le texte le `CommandText` propriété en tant que requête SQL ad hoc.


![Sélectionnez le CategoriesTableAdapter dans le concepteur pour afficher ses propriétés dans la fenêtre Propriétés](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image43.png)

**Figure 15**: sélectionnez le `CategoriesTableAdapter` dans le concepteur pour afficher ses propriétés dans la fenêtre Propriétés


Pour modifier ces paramètres, sélectionnez le texte (DeleteCommand) dans la fenêtre Propriétés, puis choisissez (nouveau) dans la liste déroulante. Cela efface les paramètres pour le `CommandText`, `CommandType`, et `Parameters` propriétés. Ensuite, définissez le `CommandType` propriété `StoredProcedure` puis tapez le nom de la procédure stockée pour le `CommandText` (`dbo.Categories_Delete`). Si vous veillez à entrer des propriétés dans cet ordre : tout d’abord le `CommandType` , puis le `CommandText` -Visual Studio est automatiquement rempli la collection de paramètres. Si vous n’entrez pas de ces propriétés dans cet ordre, vous devrez ajouter manuellement les paramètres via l’éditeur de collections Parameters. Dans les deux cas, il s prudente de cliquer sur le bouton de sélection dans la propriété de paramètres pour afficher l’éditeur de Collection de paramètres pour vérifier que les modifications des paramètres appropriés pour les paramètres ont été apportées (voir Figure 16). Si vous ne voyez pas tous les paramètres dans la boîte de dialogue, ajoutez le `@CategoryID` paramètre manuellement (vous n’avez pas besoin ajouter le `@RETURN_VALUE` paramètre).


![Assurez-vous que les paramètres sont corrects](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image44.png)

**Figure 16**: Assurez-vous que les paramètres sont corrects


Une fois que la couche DAL a été mis à jour, suppression d’une catégorie automatiquement supprimer toutes ses produits associés et le faire dans le cadre d’une transaction. Pour vérifier ceci, revenir à la page de mise à jour et suppression des données binaires existantes, puis cliquez sur le bouton Supprimer pour l’une des catégories. Avec un seul clic de la souris, la catégorie et tous ses produits associés seront supprimés.

> [!NOTE]
> Avant de tester le `Categories_Delete` procédure stockée, qui supprime un nombre de produits, ainsi que la catégorie sélectionnée, il est recommandé de faire une copie de sauvegarde de votre base de données. Si vous utilisez le `NORTHWND.MDF` dans la base de données `App_Data`, simplement fermer Visual Studio et copiez les fichiers MDF et LDF dans `App_Data` à un autre dossier. Après avoir testé la fonctionnalité, vous pouvez restaurer la base de données en fermant Visual Studio et les fichiers en remplaçant l’actuel MDF et LDF dans `App_Data` avec les copies de sauvegarde.


## <a name="summary"></a>Récapitulatif

Alors que l’Assistant TableAdapter s génèrent automatiquement des procédures stockées pour nous, voici les heures lorsque nous pourrions disposent de ces procédures stockées créées ou à les créer manuellement ou avec d’autres outils à la place. Pour prendre en compte ces scénarios, le TableAdapter peut également être configuré pour pointer vers une procédure stockée existante. Dans ce didacticiel, nous avons vu comment ajouter manuellement des procédures stockées pour une base de données via l’environnement Visual Studio et comment lier les méthodes de s TableAdapter à ces procédures stockées. Nous avons examiné également les commandes T-SQL et le modèle de script utilisé pour le démarrage, la validation et annulation de transactions à partir d’une procédure stockée.

Bonne programmation !

## <a name="about-the-author"></a>À propos de l’auteur

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), auteur de sept les livres sur ASP/ASP.NET et fondateur de [4GuysFromRolla.com](http://www.4guysfromrolla.com), travaille avec les technologies Web Microsoft depuis 1998. Scott fonctionne comme un consultant indépendant, formateur et writer. Son dernier ouvrage est [*SAM animer vous-même ASP.NET 2.0 des dernières 24 heures*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Il peut être contacté à [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou via son blog, qui se trouve à [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Remerciements

Cette série de didacticiels a été révisée par plusieurs réviseurs utiles. Les réviseurs tête pour ce didacticiel ont été Hilton Geisenow, S ren Jacob Lauritsen et Teresa Murphy. Qui souhaitent consulter mes prochains articles MSDN ? Dans ce cas, envoyez-moi une ligne à [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Précédent](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs.md)
> [Suivant](updating-the-tableadapter-to-use-joins-cs.md)
