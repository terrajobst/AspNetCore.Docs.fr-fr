---
uid: web-forms/overview/data-access/advanced-data-access-scenarios/working-with-computed-columns-vb
title: Utilisation des colonnes calculées (VB) | Microsoft Docs
author: rick-anderson
description: Lorsque vous créez une table de base de données, Microsoft SQL Server vous permet de définir une colonne calculée, dont la valeur est calculée à partir d’une expression pouvant généralement referen...
ms.author: riande
ms.date: 08/03/2007
ms.assetid: 5811b8ff-ed56-40fc-9397-6b69ae09a8f6
msc.legacyurl: /web-forms/overview/data-access/advanced-data-access-scenarios/working-with-computed-columns-vb
msc.type: authoredcontent
ms.openlocfilehash: 81254408252c5e786c938d4eb8beb1c7a2b65218
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41835071"
---
<a name="working-with-computed-columns-vb"></a>Utilisation des colonnes calculées (VB)
====================
par [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Télécharger le Code](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_71_VB.zip) ou [télécharger le PDF](working-with-computed-columns-vb/_static/datatutorial71vb1.pdf)

> Lorsque vous créez une table de base de données, Microsoft SQL Server vous permet de définir une colonne calculée, dont la valeur est calculée à partir d’une expression qui référence généralement les autres valeurs dans le même enregistrement de base de données. Ces valeurs sont en lecture seule à la base de données, ce qui nécessite des considérations particulières lorsque vous travaillez avec les TableAdapters. Dans ce didacticiel, nous apprendre relever les défis posés par les colonnes calculées.


## <a name="introduction"></a>Introduction

Permet à Microsoft SQL Server pour  *[les colonnes calculées](https://msdn.microsoft.com/library/ms191250.aspx)*, qui sont des colonnes dont les valeurs sont calculées à partir d’une expression qui référence généralement les valeurs des autres colonnes dans la même table. Par exemple, un modèle de données de suivi de temps peut avoir une table nommée `ServiceLog` avec des colonnes, y compris `ServicePerformed`, `EmployeeID`, `Rate`, et `Duration`, entre autres. Tandis que le montant dû par service, élément (la vitesse multipliée par la durée) peut être calculée via une page web ou une autre interface de programmation, il peut être utile d’inclure une colonne dans la `ServiceLog` table nommée `AmountDue` qui a signalé cette plus d’informations. Cette colonne peut être créée comme une colonne, mais elle aurait besoin d’être mis à jour à tout moment le `Rate` ou `Duration` les valeurs de colonne modifiées. Une meilleure approche consisterait à lancer le `AmountDue` une colonne calculée à l’aide de l’expression de colonne `Rate * Duration`. Entraînerait SQL Server permet de calculer automatiquement le `AmountDue` valeur chaque fois qu’il a été référencé dans une requête de la colonne.

Dans la mesure où une valeur de colonne calculée s est déterminée par une expression, ces colonnes sont en lecture seule et par conséquent ne peut pas attribuer une valeur correspondante dans `INSERT` ou `UPDATE` instructions. Toutefois, lorsque les colonnes calculées font partie de la requête principale d’un TableAdapter qui utilise des instructions SQL ad hoc, ils sont automatiquement inclus dans générées automatiquement `INSERT` et `UPDATE` instructions. Par conséquent, le TableAdapter s `INSERT` et `UPDATE` requêtes et `InsertCommand` et `UpdateCommand` propriétés doivent être mis à jour pour supprimer les références à toutes les colonnes calculées.

L’un des défis de l’utilisation de colonnes avec un TableAdapter qui utilise des instructions SQL ad hoc calculées est que le TableAdapter s `INSERT` et `UPDATE` requêtes sont automatiquement régénérées dès que l’Assistant Configuration de TableAdapter est terminé. Par conséquent, les colonnes calculées supprimé manuellement à partir de la `INSERT` et `UPDATE` requêtes réapparaissent si l’Assistant est relancée. Bien que les TableAdapters qui utilisent des procédures stockées ne pas être affectées par cette vulnérabilité, elles n’ont pas leurs propres particularités que nous étudierons à l’étape 3.

Dans ce didacticiel, nous allons ajouter une colonne calculée à la `Suppliers` de table dans la base de données Northwind, puis créez un TableAdapter correspondant pour travailler avec cette table et sa colonne calculée. Nous aurons notre TableAdapter et utiliser des procédures stockées au lieu d’instructions SQL ad hoc afin que notre t ne de personnalisations perdues lors de l’Assistant Configuration de TableAdapter est utilisé.

Laissez s commencer !

## <a name="step-1-adding-a-computed-column-to-thesupplierstable"></a>Étape 1 : Ajout d’une colonne calculée à la`Suppliers`Table

La base de données Northwind n’a pas les colonnes calculées afin de nous devons ajouter un nous-mêmes. Pour ce didacticiel permettre s ajouter une colonne calculée à la `Suppliers` table appelée `FullContactName` qui retourne le nom de contact s, titre et la société elles travaillent pour le format suivant : `ContactName` (`ContactTitle`, `CompanyName`). Cela calculée de colonne peut être utilisée dans les rapports lors de l’affichage d’informations sur les fournisseurs.

Commencez par ouvrir le `Suppliers` définition de la table en cliquant sur le `Suppliers` de table dans l’Explorateur de serveurs et en choisissant Ouvrir la définition de Table dans le menu contextuel. Ceci affichera les colonnes de la table et leurs propriétés, telles que leur type de données, si elles permettent `NULL` s et ainsi de suite. Pour ajouter une colonne calculée, commencez par taper le nom de la colonne dans la définition de table. Ensuite, entrez son expression dans la zone de texte (formule) dans la section de la spécification de la colonne calculée dans la fenêtre Propriétés de colonne (voir Figure 1). Nom de la colonne calculée `FullContactName` et utiliser l’expression suivante :


[!code-sql[Main](working-with-computed-columns-vb/samples/sample1.sql)]

Notez que les chaînes peuvent être concaténées dans SQL à l’aide de la `+` opérateur. La `CASE` instruction peut être utilisée comme un conditionnel dans un langage de programmation traditionnel. Dans l’expression ci-dessus le `CASE` instruction peut être lu comme : si `ContactTitle` n’est pas `NULL` puis générer la `ContactTitle` valeur concaténée avec une virgule, sinon émettre rien. Pour plus d’informations sur l’utilité de la `CASE` instruction, consultez [la puissance de SQL `CASE` instructions](http://www.4guysfromrolla.com/webtech/102704-1.shtml).

> [!NOTE]
> Au lieu d’utiliser un `CASE` instruction ici, nous aurions également pu utiliser `ISNULL(ContactTitle, '')`. [`ISNULL(checkExpression, replacementValue)`](https://msdn.microsoft.com/library/ms184325.aspx) Retourne *checkExpression* si elle n’est pas NULL, sinon elle retourne *replacementValue*. While soit `ISNULL` ou `CASE` fonctionnera dans ce cas, il existe des scénarios plus complexes où la flexibilité de la `CASE` instruction ne peut pas être mis en correspondance par `ISNULL`.


Après avoir ajouté cette colonne calculée votre écran doit ressembler à la capture d’écran de la Figure 1.


[![Ajouter une colonne calculée nommée FullContactName à la Table Suppliers](working-with-computed-columns-vb/_static/image2.png)](working-with-computed-columns-vb/_static/image1.png)

**Figure 1**: ajouter une colonne calculée nommée `FullContactName` à la `Suppliers` Table ([cliquez pour afficher l’image en taille réelle](working-with-computed-columns-vb/_static/image3.png))


Après la colonne calculée d’affectation de noms et en entrant son expression, enregistrer les modifications apportées à la table en accédant au menu fichier et en choisissant Enregistrer en cliquant sur l’icône Enregistrer dans la barre d’outils ou en appuyant sur Ctrl + S `Suppliers`.

L’enregistrement de la table doit s’actualiser l’Explorateur de serveurs, y compris la colonne juste-ajouté dans le `Suppliers` liste des colonnes de table s. En outre, l’expression entrée dans la zone de texte (formule) ajuste automatiquement à une expression équivalente qui élimine l’espace blanc inutile, entoure les noms de colonne entre crochets (`[]`) et inclut des parenthèses pour afficher de manière plus explicite l’ordre des opérations :


[!code-sql[Main](working-with-computed-columns-vb/samples/sample2.sql)]

Pour plus d’informations sur les colonnes calculées dans Microsoft SQL Server, reportez-vous à la [documentation technique](https://msdn.microsoft.com/library/ms191250.aspx). Consultez également le [Comment : spécifier les colonnes calculées](https://msdn.microsoft.com/library/ms188300.aspx) pour une procédure pas à pas de créer des colonnes calculées.

> [!NOTE]
> Par défaut, les colonnes calculées ne sont pas stockées physiquement dans la table mais sont recalculées à la place de chaque fois qu’ils sont référencés dans une requête. En cochant la case à cocher est rendue persistante, toutefois, vous pouvez demander à SQL Server pour le stockage physique de la colonne calculée dans la table. Ainsi, un index doit être créé sur la colonne calculée, ce qui peut améliorer les performances des requêtes qui utilisent la valeur de colonne calculée dans leurs `WHERE` clauses. Consultez [création d’index sur des colonnes calculées](https://msdn.microsoft.com/library/ms189292.aspx) pour plus d’informations.


## <a name="step-2-viewing-the-computed-column-s-values"></a>Étape 2 : Afficher les valeurs des colonnes calculées s

Avant de commencer travail sur la couche Data Access, s permettent de prendre une minute pour afficher le `FullContactName` valeurs. À partir de l’Explorateur de serveurs, cliquez sur le `Suppliers` nom de la table et choisissez Nouvelle requête dans le menu contextuel. Cela fera apparaître une fenêtre de requête que vous êtes invité à choisir les tables à inclure dans la requête. Ajouter le `Suppliers` de table et cliquez sur Fermer. Ensuite, vérifiez la `CompanyName`, `ContactName`, `ContactTitle`, et `FullContactName` colonnes à partir de la table Suppliers. Enfin, cliquez sur l’icône de point d’exclamation rouge dans la barre d’outils pour exécuter la requête et afficher les résultats.

Comme le montre la Figure 2, les résultats incluent `FullContactName`, qui répertorie les `CompanyName`, `ContactName`, et `ContactTitle` colonnes en utilisant le format `ContactName` (`ContactTitle`, `CompanyName`).


[![Le FullContactName utilise le Format ContactName (identifient, CompanyName)](working-with-computed-columns-vb/_static/image5.png)](working-with-computed-columns-vb/_static/image4.png)

**Figure 2**: le `FullContactName` utilise le Format `ContactName` (`ContactTitle`, `CompanyName`) ([cliquez pour afficher l’image en taille réelle](working-with-computed-columns-vb/_static/image6.png))


## <a name="step-3-adding-thesupplierstableadapterto-the-data-access-layer"></a>Étape 3 : Ajout de la`SuppliersTableAdapter`à la couche d’accès aux données

Pour pouvoir fonctionner avec les informations de fournisseur dans notre application, nous devons d’abord créer un TableAdapter et un objet DataTable dans notre DAL. Dans l’idéal, cette opération s’effectue à l’aide de la même procédure simple examinée dans les didacticiels précédents. Toutefois, travailler avec des colonnes calculées présente quelques plis qui méritent discussion.

Si vous utilisez un TableAdapter qui utilise des instructions SQL ad hoc, vous pouvez simplement inclure la colonne calculée dans la requête principale de s TableAdapter via l’Assistant Configuration de TableAdapter. Ceci, cependant, génère automatiquement `INSERT` et `UPDATE` les instructions contenant la colonne calculée. Si vous tentez d’exécuter une de ces méthodes, un `SqlException` avec le message de la colonne *ColumnName* ne peut pas être modifié, car il est soit une colonne calculée, soit le résultat d’un opérateur UNION est levé. Bien que le `INSERT` et `UPDATE` instruction peut être ajustée manuellement via le TableAdapter s `InsertCommand` et `UpdateCommand` propriétés, ces personnalisations seront perdues chaque fois que l’Assistant Configuration de TableAdapter est relancée.

En raison de la fragilité de TableAdapters qui utilisent des instructions SQL ad hoc, il est recommandé que nous utilisons des procédures stockées lorsque vous travaillez avec des colonnes calculées. Si vous utilisez des procédures stockées existantes, il suffit de configurer le TableAdapter comme indiqué dans le [à l’aide des procédures stockées existantes pour s DataSet typée TableAdapters](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb.md) didacticiel. Toutefois, si vous avez l’Assistant TableAdapter de créer les procédures stockées pour vous, il est important d’initialement omettre les colonnes calculées à partir de la requête principale. Si vous incluez une colonne calculée dans la requête principale, l’Assistant Configuration de TableAdapter vous informe, à l’achèvement, qu’il ne peut pas créer les procédures stockées correspondantes. En bref, nous devons configurer initialement le TableAdapter à l’aide d’une requête de libérer à la colonne principale calculée et puis mettre à jour manuellement la procédure stockée correspondante et les opérations de mappage TableAdapter `SelectCommand` pour inclure la colonne calculée. Cette approche est similaire à celui utilisé dans le [mise à jour le TableAdapter à utiliser](updating-the-tableadapter-to-use-joins-vb.md)`JOIN`*s* didacticiel.

Pour ce didacticiel, permettent d’ajouter un nouveau TableAdapter et lui demander de générer automatiquement les procédures stockées pour nous s. Par conséquent, nous devons initialement omettre le `FullContactName` une colonne calculée à partir de la requête principale.

Commencez par ouvrir le `NorthwindWithSprocs` jeu de données dans le `~/App_Code/DAL` dossier. Avec le bouton droit dans le concepteur et, dans le menu contextuel, choisissez d’ajouter un nouveau TableAdapter. Cette action lance l’Assistant Configuration de TableAdapter. Spécifiez la base de données pour interroger des données à partir de (`NORTHWNDConnectionString` de `Web.config`) et cliquez sur Suivant. Étant donné que nous n’avons pas encore créé de toutes les procédures stockées pour l’interrogation ou modification de la `Suppliers` de table, sélectionnez Créer de nouvelles procédures stockées option afin que l’Assistant va créer pour nous et cliquez sur Suivant.


[![Choisissez les créer nouvelles procédures stockées Option](working-with-computed-columns-vb/_static/image8.png)](working-with-computed-columns-vb/_static/image7.png)

**Figure 3**: choisissez des créer nouvelles procédures stockées Option ([cliquez pour afficher l’image en taille réelle](working-with-computed-columns-vb/_static/image9.png))


L’étape suivante nous demande la requête principale. Entrez la requête suivante, qui retourne le `SupplierID`, `CompanyName`, `ContactName`, et `ContactTitle` colonnes pour chaque fournisseur. Notez que cette requête omet volontairement la colonne calculée (`FullContactName`) ; nous mettrons à jour la procédure stockée correspondante pour inclure cette colonne à l’étape 4.


[!code-sql[Main](working-with-computed-columns-vb/samples/sample3.sql)]

Après avoir saisi la requête principale et en cliquant sur Suivant, l’Assistant nous permet de nommer les quatre procédures stockées, qu'il génère. Nommez ces procédures stockées `Suppliers_Select`, `Suppliers_Insert`, `Suppliers_Update`, et `Suppliers_Delete`, comme l’illustre la Figure 4.


[![Personnalisez les noms des procédures stockées générées automatiquement](working-with-computed-columns-vb/_static/image11.png)](working-with-computed-columns-vb/_static/image10.png)

**Figure 4**: personnalisez les noms des procédures stockées Auto-Generated ([cliquez pour afficher l’image en taille réelle](working-with-computed-columns-vb/_static/image12.png))


L’étape suivante de l’Assistant permet de nommer les méthodes de s TableAdapter et spécifier les modèles utilisés pour l’accès et mise à jour des données. Laissez toutes les cases à trois cocher activées, mais les renommer le `GetData` méthode `GetSuppliers`. Cliquez sur Terminer pour terminer l’Assistant.


[![Renommez la méthode GetData GetSuppliers](working-with-computed-columns-vb/_static/image14.png)](working-with-computed-columns-vb/_static/image13.png)

**Figure 5**: renommer le `GetData` méthode `GetSuppliers` ([cliquez pour afficher l’image en taille réelle](working-with-computed-columns-vb/_static/image15.png))


Lorsque vous cliquez sur Terminer, l’Assistant créer les quatre procédures stockées et ajouter le TableAdapter et DataTable correspondant au jeu de données typé.

## <a name="step-4-including-the-computed-column-in-the-tableadapter-s-main-query"></a>Étape 4 : Y compris la colonne calculée dans la requête principale de TableAdapter s

Nous devons maintenant mettre à jour le TableAdapter et DataTable créé à l’étape 3 pour inclure la `FullContactName` colonne calculée. Cela implique deux étapes :

1. La mise à jour le `Suppliers_Select` procédure stockée pour retourner le `FullContactName` une colonne calculée, et
2. La mise à jour de la table de données pour inclure un correspondant `FullContactName` colonne.

Démarrez en accédant à l’Explorateur de serveurs et de descente dans le dossier Stored Procedures. Ouvrez le `Suppliers_Select` procédure stockée et mise à jour le `SELECT` requête afin d’inclure la `FullContactName` colonne calculée :


[!code-sql[Main](working-with-computed-columns-vb/samples/sample4.sql)]

Enregistrer les modifications apportées à la procédure stockée en cliquant sur l’icône Enregistrer dans la barre d’outils, en appuyant sur Ctrl + S ou en choisissant l’enregistrement `Suppliers_Select` option dans le menu fichier.

Ensuite, revenez dans le Concepteur de DataSet, avec le bouton droit sur le `SuppliersTableAdapter`et choisissez configurer dans le menu contextuel. Notez que le `Suppliers_Select` colonne inclut désormais la `FullContactName` colonne dans sa collection de colonnes de données.


[![Exécutez l’Assistant de Configuration de s TableAdapter pour mettre à jour les colonnes de s DataTable](working-with-computed-columns-vb/_static/image17.png)](working-with-computed-columns-vb/_static/image16.png)

**Figure 6**: exécuter l’Assistant de Configuration pour mettre à jour les colonnes de s DataTable du TableAdapter s ([cliquez pour afficher l’image en taille réelle](working-with-computed-columns-vb/_static/image18.png))


Cliquez sur Terminer pour terminer l’Assistant. Cela ajoutera automatiquement une colonne correspondante pour le `SuppliersDataTable`. L’Assistant TableAdapter est suffisamment intelligent pour détecter les `FullContactName` colonne est une colonne calculée et en lecture seule. Par conséquent, il définit la colonne s `ReadOnly` propriété `true`. Pour vérifier cela, sélectionnez la colonne à partir de la `SuppliersDataTable` , puis accédez à la fenêtre Propriétés (voir Figure 7). Notez que le `FullContactName` colonne s `DataType` et `MaxLength` propriétés sont également définies en conséquence.


[![La colonne FullContactName est marquée comme étant en lecture seule](working-with-computed-columns-vb/_static/image20.png)](working-with-computed-columns-vb/_static/image19.png)

**Figure 7**: le `FullContactName` colonne est marquée comme étant en lecture seule ([cliquez pour afficher l’image en taille réelle](working-with-computed-columns-vb/_static/image21.png))


## <a name="step-5-adding-agetsupplierbysupplieridmethod-to-the-tableadapter"></a>Étape 5 : Ajout d’un`GetSupplierBySupplierID`méthode au TableAdapter

Pour ce didacticiel, nous allons créer une page ASP.NET qui affiche les fournisseurs dans une grille modifiable. Dans au-delà de didacticiels nous avons mis à jour un enregistrement unique de la couche de logique métier en récupérant qu’enregistrement particulier à partir de la couche DAL sous forme de DataTable fortement typée, en mettant à jour ses propriétés et envoyer la mise à jour de la table de données serveur à la couche DAL pour propager les modifications apportées à la base de données. Pour accomplir cette première étape - récupération de l’enregistrement mis à jour à partir de la couche DAL - nous devons tout d’abord ajouter un `GetSupplierBySupplierID(supplierID)` méthode à la couche DAL.

Avec le bouton droit sur le `SuppliersTableAdapter` dans la conception du jeu de données et choisissez l’option Ajouter une requête dans le menu contextuel. Comme nous l’avons fait à l’étape 3, laisser l’Assistant générer une nouvelle procédure stockée pour nous par l’option Créer nouvelle procédure stockée (voir Figure 3 pour une capture d’écran de cette étape). Étant donné que cette méthode retourne un enregistrement avec plusieurs colonnes, indiquent que nous souhaitons utiliser une requête SQL qui est une instruction SELECT qui retourne des lignes et cliquez sur Suivant.


[![Choisissez l’instruction SELECT qui retourne des lignes, Option](working-with-computed-columns-vb/_static/image23.png)](working-with-computed-columns-vb/_static/image22.png)

**Figure 8**: choisissez l’instruction SELECT qui retourne des lignes Option ([cliquez pour afficher l’image en taille réelle](working-with-computed-columns-vb/_static/image24.png))


L’étape suivante nous demande de la requête à utiliser pour cette méthode. Entrez la commande suivante, qui retourne les mêmes champs de données en tant que la requête principale, mais pour un fournisseur particulier.


[!code-sql[Main](working-with-computed-columns-vb/samples/sample5.sql)]

L’écran suivant nous demande de nom de la procédure stockée qui sera généré automatiquement. Nom de cette procédure stockée `Suppliers_SelectBySupplierID` et cliquez sur Suivant.


[![Nom de la procédure stockée Suppliers_SelectBySupplierID](working-with-computed-columns-vb/_static/image26.png)](working-with-computed-columns-vb/_static/image25.png)

**Figure 9**: nom de la procédure stockée `Suppliers_SelectBySupplierID` ([cliquez pour afficher l’image en taille réelle](working-with-computed-columns-vb/_static/image27.png))


Enfin, les invites de l’Assistant nous pour les données d’accéder aux modèles et des noms de méthodes à utiliser pour le TableAdapter. Laissez les cases cochées, mais renommer le `FillBy` et `GetDataBy` méthodes à `FillBySupplierID` et `GetSupplierBySupplierID`, respectivement.


[![Nom de la FillBySupplierID méthodes TableAdapter et GetSupplierBySupplierID](working-with-computed-columns-vb/_static/image29.png)](working-with-computed-columns-vb/_static/image28.png)

**Figure 10**: nommez les méthodes TableAdapter `FillBySupplierID` et `GetSupplierBySupplierID` ([cliquez pour afficher l’image en taille réelle](working-with-computed-columns-vb/_static/image30.png))


Cliquez sur Terminer pour terminer l’Assistant.

## <a name="step-6-creating-the-business-logic-layer"></a>Étape 6 : Création de la couche de logique métier

Avant de créer une page ASP.NET qui utilise la colonne calculée créée à l’étape 1, nous devons d’abord ajouter les méthodes correspondantes dans la couche BLL. Notre page ASP.NET, que nous allons créer à l’étape 7, permettra aux utilisateurs d’afficher et modifier des fournisseurs. Par conséquent, nous avons besoin de notre BLL à proposer au minimum, une méthode pour obtenir tous les fournisseurs et une autre pour mettre à jour d’un fournisseur particulier.

Créer un nouveau fichier de classe nommé `SuppliersBLLWithSprocs` dans le `~/App_Code/BLL` dossier et ajoutez le code suivant :


[!code-vb[Main](working-with-computed-columns-vb/samples/sample6.vb)]

Comme les autres classes de la couche BLL, `SuppliersBLLWithSprocs` a un `Protected` `Adapter` propriété qui retourne une instance de la `SuppliersTableAdapter` classe ainsi que deux `Public` méthodes : `GetSuppliers` et `UpdateSupplier`. Le `GetSuppliers` méthode appelle et retourne le `SuppliersDataTable` retourné par le correspondantes `GetSupplier` méthode dans la couche d’accès aux données. Le `UpdateSupplier` méthode récupère des informations sur le fournisseur mis à jour via un appel à la couche DAL s `GetSupplierBySupplierID(supplierID)` (méthode). Il met ensuite à jour le `CategoryName`, `ContactName`, et `ContactTitle` propriétés et valide ces modifications à la base de données en appelant la couche d’accès aux données s `Update` méthode, en passant le texte modifié `SuppliersRow` objet.

> [!NOTE]
> À l’exception de `SupplierID` et `CompanyName`, toutes les colonnes dans la table Suppliers autorisent `NULL` valeurs. Par conséquent, si le passé dans `contactName` ou `contactTitle` sont des paramètres `Nothing` nous devons définir le correspondantes `ContactName` et `ContactTitle` propriétés à un `NULL` valeur de la base de données à l’aide la `SetContactNameNull` et `SetContactTitleNull`méthodes, respectivement.


## <a name="step-7-working-with-the-computed-column-from-the-presentation-layer"></a>Étape 7 : Utilisation de la colonne calculée à partir de la couche de présentation

La colonne calculée est ajouté à la `Suppliers` table et la couche DAL et la couche BLL mis à jour en conséquence, nous sommes prêts à créer une page ASP.NET qui fonctionne avec la `FullContactName` colonne calculée. Commencez par ouvrir le `ComputedColumns.aspx` page dans le `AdvancedDAL` dossier et faites glisser un GridView à partir de la boîte à outils vers le concepteur. Définir les opérations de mappage GridView `ID` propriété `Suppliers` et, à partir de sa balise active, liez-le à une nouvelle ObjectDataSource nommé `SuppliersDataSource`. Configurer l’ObjectDataSource à utiliser le `SuppliersBLLWithSprocs` classe que nous avons ajouté la sauvegarde à l’étape 6, puis cliquez sur Suivant.


[![Configurer pour utiliser la classe SuppliersBLLWithSprocs ObjectDataSource](working-with-computed-columns-vb/_static/image32.png)](working-with-computed-columns-vb/_static/image31.png)

**Figure 11**: configurer ObjectDataSource à utiliser le `SuppliersBLLWithSprocs` classe ([cliquez pour afficher l’image en taille réelle](working-with-computed-columns-vb/_static/image33.png))


Il existe seulement deux méthodes définies dans le `SuppliersBLLWithSprocs` classe : `GetSuppliers` et `UpdateSupplier`. Assurez-vous que ces deux méthodes sont spécifiées dans l’instruction SELECT et mettre à jour des onglets, respectivement et cliquez sur Terminer pour terminer la configuration de l’ObjectDataSource.

À l’achèvement de l’Assistant Configuration de Source de données, Visual Studio ajoute un BoundField pour chacun des champs de données retournés. Supprimer le `SupplierID` BoundField et modifier le `HeaderText` propriétés de la `CompanyName`, `ContactName`, `ContactTitle`, et `FullContactName` BoundFields à l’entreprise, nom du Contact, titre et nom complet du Contact, respectivement. À partir de la balise active, cochez la case Activer la modification pour activer les fonctionnalités d’édition intégrées s GridView.

Outre l’ajout de BoundFields au GridView, fin de l’Assistant Source de données entraîne également Visual Studio définir les opérations de mappage ObjectDataSource `OldValuesParameterFormatString` propriété original\_{0}. Rétablir ce paramètre à sa valeur par défaut, {0} .

Après avoir apporté ces modifications pour les contrôles GridView et ObjectDataSource, leur balisage déclaratif doit ressembler à ce qui suit :


[!code-aspx[Main](working-with-computed-columns-vb/samples/sample7.aspx)]

Ensuite, visitez cette page via un navigateur. Comme le montre la Figure 12, chaque fournisseur est répertorié dans une grille qui inclut le `FullContactName` sous forme de colonne, dont la valeur est simplement la concaténation des trois autres colonnes `ContactName` (`ContactTitle`, `CompanyName`).


[![Chaque fournisseur est répertorié dans la grille](working-with-computed-columns-vb/_static/image35.png)](working-with-computed-columns-vb/_static/image34.png)

**Figure 12**: chaque fournisseur est répertorié dans la grille ([cliquez pour afficher l’image en taille réelle](working-with-computed-columns-vb/_static/image36.png))


En cliquant sur le bouton Modifier pour un fournisseur particulier entraîne une publication (postback) et a cette ligne affiché dans sa modification de l’interface (voir Figure 13). Les trois premières colonnes s’affichent dans l’interface de modification par défaut : une zone de texte contrôle dont `Text` propriété est définie sur la valeur du champ de données. La `FullContactName` colonne, cependant, reste sous forme de texte. Lorsque les BoundFields ont été ajoutés au contrôle GridView à la fin de l’Assistant Configuration de Source de données, le `FullContactName` BoundField s `ReadOnly` propriété a été définie sur `True` car correspondant `FullContactName` colonne dans la `SuppliersDataTable` a son `ReadOnly` propriété définie sur `True`. Comme indiqué à l’étape 4, le `FullContactName` s `ReadOnly` propriété a été définie sur `True` , car le TableAdapter a détecté que la colonne a une colonne calculée.


[![La colonne FullContactName n’est pas modifiable.](working-with-computed-columns-vb/_static/image38.png)](working-with-computed-columns-vb/_static/image37.png)

**Figure 13**: le `FullContactName` colonne n’est pas modifiable ([cliquez pour afficher l’image en taille réelle](working-with-computed-columns-vb/_static/image39.png))


Continuons et mettre à jour la valeur d’un ou plusieurs des colonnes modifiables et cliquez sur la mise à jour. Notez comment la `FullContactName` valeur s est automatiquement mis à jour pour refléter la modification.

> [!NOTE]
> Le contrôle GridView utilise actuellement BoundFields pour les champs modifiables, ce qui entraîne la rédaction d’interface par défaut. Dans la mesure où le `CompanyName` champ est obligatoire, il doit être converti en TemplateField qui inclut un contrôle RequiredFieldValidator. Je laisse cela en guise d’exercice pour les lecteurs que cela intéresse. Consultez le [Ajout de contrôles de Validation pour l’édition et insertion des Interfaces](../editing-inserting-and-deleting-data/adding-validation-controls-to-the-editing-and-inserting-interfaces-vb.md) didacticiel pour obtenir des instructions pas à pas sur la conversion en un BoundField TemplateField et ajout de contrôles de validation.


## <a name="summary"></a>Récapitulatif

Lorsque vous définissez le schéma pour une table, Microsoft SQL Server permet l’inclusion de colonnes calculées. Il s’agit des colonnes dont les valeurs sont calculées à partir d’une expression qui référence généralement les valeurs des autres colonnes dans le même enregistrement. Depuis les valeurs pour les colonnes calculées sont basées sur une expression, ils sont en lecture seule et ne peut pas être assignées une valeur dans un `INSERT` ou `UPDATE` instruction. Cela introduit des problèmes lors de l’utilisation d’une colonne calculée dans la requête principale d’un TableAdapter qui essaie de générer automatiquement le correspondant `INSERT`, `UPDATE`, et `DELETE` instructions.

Dans ce didacticiel, nous avons abordé les techniques pour contourner les défis posés par les colonnes calculées. En particulier, nous avons utilisé des procédures stockées dans notre TableAdapter pour surmonter la fragilité inhérents aux TableAdapters qui utilisent des instructions SQL ad hoc. Lorsque ayant l’Assistant TableAdapter à créer des procédures stockées, il est important que nous avons la requête principale initialement omettre les colonnes calculées, car leur présence empêche les procédures stockées de modification de données d’être générée. Une fois le TableAdapter a été initialement configuré, son `SelectCommand` procédure stockée peut être remaniée pour inclure les colonnes calculées.

Bonne programmation !

## <a name="about-the-author"></a>À propos de l’auteur

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), auteur de sept les livres sur ASP/ASP.NET et fondateur de [4GuysFromRolla.com](http://www.4guysfromrolla.com), travaille avec les technologies Web Microsoft depuis 1998. Scott fonctionne comme un consultant indépendant, formateur et writer. Son dernier ouvrage est [*SAM animer vous-même ASP.NET 2.0 des dernières 24 heures*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Il peut être contacté à [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou via son blog, qui se trouve à [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Remerciements

Cette série de didacticiels a été révisée par plusieurs réviseurs utiles. Les réviseurs tête pour ce didacticiel ont été Hilton Geisenow et Teresa Murphy. Qui souhaitent consulter mes prochains articles MSDN ? Dans ce cas, envoyez-moi une ligne à [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Précédent](adding-additional-datatable-columns-vb.md)
> [Suivant](configuring-the-data-access-layer-s-connection-and-command-level-settings-vb.md)
