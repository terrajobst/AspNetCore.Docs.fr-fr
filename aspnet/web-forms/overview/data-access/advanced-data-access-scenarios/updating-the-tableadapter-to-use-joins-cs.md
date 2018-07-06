---
uid: web-forms/overview/data-access/advanced-data-access-scenarios/updating-the-tableadapter-to-use-joins-cs
title: Mise à jour le TableAdapter à utiliser des jointures (c#) | Microsoft Docs
author: rick-anderson
description: Lorsque vous travaillez avec une base de données, il est courant pour demander des données qui sont réparties sur plusieurs tables. Pour récupérer des données à partir de deux tables différentes, nous pouvons utiliser soit...
ms.author: aspnetcontent
ms.date: 07/18/2007
ms.assetid: 675531a7-cb54-4dd6-89ac-2636e4c285a5
msc.legacyurl: /web-forms/overview/data-access/advanced-data-access-scenarios/updating-the-tableadapter-to-use-joins-cs
msc.type: authoredcontent
ms.openlocfilehash: c59ab13eb5e5e548055c6c8c9343a8d5c4cabe7d
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37841119"
---
<a name="updating-the-tableadapter-to-use-joins-c"></a>Mise à jour le TableAdapter à utiliser des jointures (c#)
====================
par [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Télécharger le Code](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_69_CS.zip) ou [télécharger le PDF](updating-the-tableadapter-to-use-joins-cs/_static/datatutorial69cs1.pdf)

> Lorsque vous travaillez avec une base de données, il est courant pour demander des données qui sont réparties sur plusieurs tables. Pour récupérer des données à partir de deux tables différentes, nous pouvons utiliser une sous-requête corrélée ou sur une opération de jointure. Dans ce didacticiel, nous comparons les sous-requêtes en corrélation et la syntaxe de jointure avant de regarder comment créer un TableAdapter qui inclut une jointure dans sa requête principale.


## <a name="introduction"></a>Introduction

Avec les bases de données relationnelles les données, nous nous intéressons dans l’utilisation sont souvent réparties sur plusieurs tables. Par exemple, lors de l’affichage des informations sur les produits nous souhaitons probablement répertorier chaque catégorie de produit s correspondante et les noms des fournisseurs s. Le `Products` table a `CategoryID` et `SupplierID` valeurs, mais les noms de catégorie et le fournisseur réels sont dans le `Categories` et `Suppliers` des tables, respectivement.

Pour récupérer des informations à partir d’une autre table liée, nous pouvons utiliser *sous-requêtes corrélées* ou `JOIN` *s*. Une sous-requête corrélée est imbriquée `SELECT` requête qui fait référence aux colonnes dans la requête externe. Par exemple, dans le [création d’une couche d’accès aux données](../introduction/creating-a-data-access-layer-cs.md) didacticiel, nous avons utilisé deux sous-requêtes corrélées dans le `ProductsTableAdapter` requête principale de s pour retourner les noms de catégorie et le fournisseur pour chaque produit. Un `JOIN` est une construction SQL qui fusionne des lignes connexes à partir de deux tables différentes. Nous avons utilisé un `JOIN` dans le [interrogation des données avec le contrôle SqlDataSource](../accessing-the-database-directly-from-an-aspnet-page/querying-data-with-the-sqldatasource-control-cs.md) didacticiel pour afficher des informations de catégorie en même temps que chaque produit.

La raison pour laquelle nous avons abstained de l’utilisation de `JOIN` s avec les TableAdapters est en raison des limitations dans l’Assistant TableAdapter s pour générer automatiquement des correspondant `INSERT`, `UPDATE`, et `DELETE` instructions. Plus précisément, si la requête principale de TableAdapter s contient une `JOIN` s, le TableAdapter ne peut pas créer automatiquement les instructions SQL ad hoc ou les procédures stockées pour son `InsertCommand`, `UpdateCommand`, et `DeleteCommand` propriétés.

Dans ce didacticiel, nous allons comparer brièvement et le contraste des sous-requêtes corrélées et `JOIN` s avant d’Explorer la création d’un TableAdapter qui inclut `JOIN` s dans sa requête principale.

## <a name="comparing-and-contrasting-correlated-subqueries-andjoin-s"></a>En comparant et différenciant les sous-requêtes en corrélation et`JOIN` s

N’oubliez pas que le `ProductsTableAdapter` créé dans le premier didacticiel de la `Northwind` jeu de données utilise des sous-requêtes corrélées pour ramener chaque nom catégorie et le fournisseur correspondant produit s. Le `ProductsTableAdapter` requête principale s est indiqué ci-dessous.


[!code-sql[Main](updating-the-tableadapter-to-use-joins-cs/samples/sample1.sql)]

Les deux en corrélation sous-requêtes - `(SELECT CategoryName FROM Categories WHERE Categories.CategoryID = Products.CategoryID)` et `(SELECT CompanyName FROM Suppliers WHERE Suppliers.SupplierID = Products.SupplierID)` -sont `SELECT` requêtes qui retournent une valeur unique par produit comme une colonne supplémentaire dans la liste externe `SELECT` liste des colonnes de l’instruction s.

Vous pouvez également un `JOIN` peut être utilisée pour retourner chaque nom de fournisseur et la catégorie de produit s. La requête suivante retourne le même résultat que celui ci-dessus, mais utilise `JOIN` s à la place des sous-requêtes :


[!code-sql[Main](updating-the-tableadapter-to-use-joins-cs/samples/sample2.sql)]

Un `JOIN` fusionne les enregistrements d’une table avec des enregistrements à partir d’une autre table en fonction de certains critères. Dans la requête ci-dessus, par exemple, le `LEFT JOIN Categories ON Categories.CategoryID = Products.CategoryID` demande à SQL Server pour la fusion de chaque enregistrement de produit avec la catégorie enregistrement dont la propriété `CategoryID` valeur correspond au produit s `CategoryID` valeur. Le résultat fusionné nous permet de travailler avec les champs de catégorie correspondant pour chaque produit (tel que `CategoryName`).

> [!NOTE]
> `JOIN` s sont couramment utilisés lors de l’interrogation des données à partir de bases de données relationnelles. Si vous ne connaissez pas le `JOIN` syntaxe ou devoir d’agrémenter un peu sur son utilisation, je d recommande le [didacticiel SQL Join](http://www.w3schools.com/sql/sql_join.asp) à [W3 Schools](http://www.w3schools.com/). Également important de lecture sont le [ `JOIN` notions de base](https://msdn.microsoft.com/library/ms191517.aspx) et [principes de base](https://msdn.microsoft.com/library/ms189575.aspx) sections de la [la documentation en ligne de SQL](https://msdn.microsoft.com/library/ms130214.aspx).


Dans la mesure où `JOIN` s et des sous-requêtes corrélées peuvent être utilisés pour récupérer les données associées provenant d’autres tables, de nombreux développeurs sont laissés leurs têtes infructueuses et vous vous demandez de l’approche à utiliser. Tous les gourous SQL je ve ont rapporté à peu près la même chose, qu’il ne peu performante que SQL Server génère des plans d’exécution à peu près identique. Leur avis, il faut utiliser la technique que vous et votre équipe sont convient le mieux. Il mérite de noter que, après particulier ce Conseil ces experts expriment immédiatement leurs préférences de `JOIN` s sur les sous-requêtes corrélées.

Lors de la création d’une couche d’accès aux données à l’aide de jeux de données typé, les outils fonctionnent mieux lors de l’utilisation de sous-requêtes. En particulier, l’Assistant TableAdapter s pas génère automatiquement correspondant `INSERT`, `UPDATE`, et `DELETE` instructions si la requête principale contient une `JOIN` s, mais génère automatiquement ces instructions lorsque corrélées sous-requêtes sont utilisés.

Pour Explorer cet inconvénient, créez un DataSet typé temporaire dans le `~/App_Code/DAL` dossier. Au cours de l’Assistant Configuration de TableAdapter, choisir d’utiliser des instructions SQL ad hoc et entrez les informations suivantes `SELECT` requête (voir Figure 1) :


[!code-sql[Main](updating-the-tableadapter-to-use-joins-cs/samples/sample3.sql)]


[![Entrez une requête principale qui contient des jointures](updating-the-tableadapter-to-use-joins-cs/_static/image2.png)](updating-the-tableadapter-to-use-joins-cs/_static/image1.png)

**Figure 1**: entrez une requête principale qui contient `JOIN` s ([cliquez pour afficher l’image en taille réelle](updating-the-tableadapter-to-use-joins-cs/_static/image3.png))


Par défaut, le TableAdapter créera automatiquement `INSERT`, `UPDATE`, et `DELETE` instructions en fonction de la requête principale. Si vous cliquez sur le bouton Avancé, vous pouvez voir que cette fonctionnalité est activée. En dépit de ce paramètre, le TableAdapter sera pas en mesure de créer le `INSERT`, `UPDATE`, et `DELETE` instructions, car la requête principale contient un `JOIN`.


![Entrez une requête principale qui contient des jointures](updating-the-tableadapter-to-use-joins-cs/_static/image4.png)

**Figure 2**: entrez une requête principale contenant `JOIN` s


Cliquez sur Terminer pour terminer l’Assistant. À ce stade, votre Concepteur de DataSet s inclut un seul TableAdapter avec un DataTable avec des colonnes pour chacun des champs retournés dans le `SELECT` liste des colonnes de requête s. Cela inclut la `CategoryName` et `SupplierName`, comme le montre la Figure 3.


![La table de données inclut une colonne pour chaque champ renvoyé dans la liste des colonnes](updating-the-tableadapter-to-use-joins-cs/_static/image5.png)

**Figure 3**: la table de données inclut une colonne pour chaque champ retourné dans la liste des colonnes


Alors que la table de données a les colonnes appropriées, le TableAdapter ne dispose pas des valeurs pour ses `InsertCommand`, `UpdateCommand`, et `DeleteCommand` propriétés. Pour vérifier cela, cliquez sur le TableAdapter dans le concepteur et passez à la fenêtre Propriétés. Il vous verrez que le `InsertCommand`, `UpdateCommand`, et `DeleteCommand` propriétés sont définies à (None).


[![La propriété InsertCommand, UpdateCommand et DeleteCommand propriétés sont définies à (None)](updating-the-tableadapter-to-use-joins-cs/_static/image7.png)](updating-the-tableadapter-to-use-joins-cs/_static/image6.png)

**Figure 4**: le `InsertCommand`, `UpdateCommand`, et `DeleteCommand` propriétés sont définies à (None) ([cliquez pour afficher l’image en taille réelle](updating-the-tableadapter-to-use-joins-cs/_static/image8.png))


Pour contourner cet inconvénient, nous pouvons fournir manuellement les instructions SQL et les paramètres pour le `InsertCommand`, `UpdateCommand`, et `DeleteCommand` propriétés via la fenêtre Propriétés. Ou bien, nous pouvons commencer en configurant la requête principale de s TableAdapter à *pas* inclure tous `JOIN` s. Cela permettra le `INSERT`, `UPDATE`, et `DELETE` à des instructions pour être généré automatiquement pour nous. À l’issue de l’Assistant, nous pourrions ensuite mettre à jour manuellement le TableAdapter s `SelectCommand` à partir de la fenêtre Propriétés afin qu’il inclue le `JOIN` syntaxe.

Bien que cette approche fonctionne, il est très fragile lorsqu’à l’aide de requêtes SQL ad hoc, car chaque fois que la requête principale de s TableAdapter est reconfiguré via l’Assistant, générée automatiquement `INSERT`, `UPDATE`, et `DELETE` instructions sont recréées. Cela signifie que toutes les personnalisations plus tard, nous avons apporté seraient perdues si nous avez cliqué sur le TableAdapter a choisi de configurer dans le menu contextuel et terminé l’Assistant à nouveau.

La fragilité de la s TableAdapter générées automatiquement `INSERT`, `UPDATE`, et `DELETE` instructions est, fort heureusement, limité aux instructions SQL ad hoc. Si votre TableAdapter utilise des procédures stockées, vous pouvez personnaliser le `SelectCommand`, `InsertCommand`, `UpdateCommand`, ou `DeleteCommand` des procédures stockées et réexécutez l’Assistant Configuration de TableAdapter sans avoir à craindre que les procédures stockées seront modifié.

Le prochain plusieurs étapes, nous allons créer un TableAdapter qui, au départ, utilise une requête principale, qui omet les `JOIN` s afin que le correspondantes insérer, mettre à jour et procédures stockées de suppression sera généré automatiquement. Nous met ensuite à jour le `SelectCommand` donc qui utilise un `JOIN` qui retourne des colonnes supplémentaires à partir de tables associées. Enfin, nous créer une classe correspondante de la couche de logique métier et expliquent comment utiliser le TableAdapter dans une page web ASP.NET.

## <a name="step-1-creating-the-tableadapter-using-a-simplified-main-query"></a>Étape 1 : Création du TableAdapter à l’aide d’une requête principale simplifiée

Pour ce didacticiel, nous allons ajouter un TableAdapter et la table de données fortement typées pour la `Employees` table dans le `NorthwindWithSprocs` jeu de données. Le `Employees` table contient un `ReportsTo` champ qui spécifié le `EmployeeID` de l’employé s responsable. Par exemple, les employés Anne Dodsworth a un `ReportTo` valeur 5, qui est le `EmployeeID` de Steven Buchanan. Par conséquent, Anne signale à Steven, son responsable. En même temps que la création de rapports chaque employé s `ReportsTo` valeur, nous souhaitions également récupérer le nom de son responsable. Cela peut être accompli à l’aide un `JOIN`. Mais l’utilisation un `JOIN` lors de la création du TableAdapter empêche l’Assistant à partir de la génération automatique de l’insertion correspondante, mettre à jour et supprimer des fonctionnalités. Par conséquent, nous allons commencer par la création d’un TableAdapter dont requête principale ne contienne aucun `JOIN` s. Ensuite, à l’étape 2, nous mettrons à jour la procédure stockée de requête principale pour récupérer le nom de gestionnaire s via un `JOIN`.

Commencez par ouvrir le `NorthwindWithSprocs` jeu de données dans le `~/App_Code/DAL` dossier. Avec le bouton droit sur le concepteur, sélectionnez l’option Ajouter dans le menu contextuel et choisissez l’élément de menu du TableAdapter. Cette action lance l’Assistant Configuration de TableAdapter. Comme la Figure 5 illustre, laisser l’Assistant créer des procédures stockées et cliquez sur Suivant. Pour un rappel sur la création de nouvelles procédures stockées à partir l’Assistant TableAdapter s, consultez le [création de nouvelles procédures stockées pour s DataSet typée TableAdapters](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs.md) didacticiel.


[![Sélectionnez les créer nouvelles procédures stockées Option](updating-the-tableadapter-to-use-joins-cs/_static/image10.png)](updating-the-tableadapter-to-use-joins-cs/_static/image9.png)

**Figure 5**: sélectionnez la créer nouveau des procédures stockées Option ([cliquez pour afficher l’image en taille réelle](updating-the-tableadapter-to-use-joins-cs/_static/image11.png))


Utilisez la commande suivante `SELECT` instruction pour la requête principale de TableAdapter s :


[!code-sql[Main](updating-the-tableadapter-to-use-joins-cs/samples/sample4.sql)]

Étant donné que cette requête ne contienne pas `JOIN` s, l’Assistant TableAdapter crée automatiquement des procédures stockées avec correspondant `INSERT`, `UPDATE`, et `DELETE` instructions, ainsi que d’une procédure stockée pour l’exécution de la main requête.

L’étape suivante nous permet de nommer les procédures stockées de TableAdapter. Utilisez les noms `Employees_Select`, `Employees_Insert`, `Employees_Update`, et `Employees_Delete`, comme illustré dans la Figure 6.


[![Nommez les procédures stockées de TableAdapter](updating-the-tableadapter-to-use-joins-cs/_static/image13.png)](updating-the-tableadapter-to-use-joins-cs/_static/image12.png)

**Figure 6**: nommez les procédures stockées de TableAdapter s ([cliquez pour afficher l’image en taille réelle](updating-the-tableadapter-to-use-joins-cs/_static/image14.png))


L’étape finale nous invite à nommer les méthodes de s TableAdapter. Utilisez `Fill` et `GetEmployees` en tant que les noms de méthode. Veillez également à laisser les créer des méthodes pour envoyer des mises à jour directement à la case à cocher de la base de données (GenerateDBDirectMethods) activée.


[![Nom du remplissage de méthodes TableAdapter s et GetEmployees](updating-the-tableadapter-to-use-joins-cs/_static/image16.png)](updating-the-tableadapter-to-use-joins-cs/_static/image15.png)

**Figure 7**: nommez les méthodes du TableAdapter s `Fill` et `GetEmployees` ([cliquez pour afficher l’image en taille réelle](updating-the-tableadapter-to-use-joins-cs/_static/image17.png))


À l’issue de l’Assistant, prenez un moment pour examiner les procédures stockées dans la base de données. Vous devez voir quatre nouveaux : `Employees_Select`, `Employees_Insert`, `Employees_Update`, et `Employees_Delete`. Ensuite, inspecter la `EmployeesDataTable` et `EmployeesTableAdapter` venez de créer. La table de données contient une colonne pour chaque champ renvoyé par la requête principale. Cliquez sur le TableAdapter et passez à la fenêtre Propriétés. Il vous verrez que le `InsertCommand`, `UpdateCommand`, et `DeleteCommand` propriétés sont correctement configurées pour appeler les procédures stockées correspondantes.


[![Le TableAdapter inclut Insert, Update et supprimer des fonctionnalités](updating-the-tableadapter-to-use-joins-cs/_static/image19.png)](updating-the-tableadapter-to-use-joins-cs/_static/image18.png)

**Figure 8**: le TableAdapter inclut Insert, Update et supprimer des fonctionnalités ([cliquez pour afficher l’image en taille réelle](updating-the-tableadapter-to-use-joins-cs/_static/image20.png))


Lors de l’insertion, mise à jour et supprimer des procédures stockées créées automatiquement et le `InsertCommand`, `UpdateCommand`, et `DeleteCommand` propriétés configurées correctement, nous sommes prêts à personnaliser le `SelectCommand` s procédure stockée pour retourner supplémentaires informations sur chaque employé s responsable. Plus précisément, nous devons mettre à jour le `Employees_Select` une procédure stockée à utiliser un `JOIN` et retourner le Gestionnaire de s `FirstName` et `LastName` valeurs. Une fois la procédure stockée a été mis à jour, nous devrons mettre à jour la table de données pour qu’elle inclue ces colonnes supplémentaires. Nous les aborderons ces deux tâches dans les étapes 2 et 3.

## <a name="step-2-customizing-the-stored-procedure-to-include-ajoin"></a>Étape 2 : Personnalisation de la procédure stockée pour inclure un`JOIN`

Commencez par passer à l’Explorateur de serveurs, la descente dans le dossier de procédures stockées de base de données Northwind et l’ouverture du `Employees_Select` procédure stockée. Si vous ne voyez pas cette procédure stockée, avec le bouton droit sur le dossier Stored Procedures, cliquez sur Actualiser. Mettre à jour de la procédure stockée afin qu’il utilise un `LEFT JOIN` pour retourner le Gestionnaire de s tout d’abord et du nom :


[!code-sql[Main](updating-the-tableadapter-to-use-joins-cs/samples/sample5.sql)]

Après la mise à jour le `SELECT` instruction, enregistrez les modifications apportées en accédant au menu fichier et en choisissant Enregistrer `Employees_Select`. Vous pouvez également, cliquez sur l’icône Enregistrer dans la barre d’outils ou appuyez sur Ctrl + S. Après avoir enregistré vos modifications, cliquez sur le `Employees_Select` procédure stockée dans l’Explorateur de serveurs et cliquez sur Exécuter. Cela sera exécuter la procédure stockée et afficher ses résultats dans la fenêtre de sortie (voir la Figure 9).


[![Les résultats de procédures stockées sont affichés dans la fenêtre Sortie](updating-the-tableadapter-to-use-joins-cs/_static/image22.png)](updating-the-tableadapter-to-use-joins-cs/_static/image21.png)

**Figure 9**: les résultats des procédures stockées sont affichés dans la fenêtre Sortie ([cliquez pour afficher l’image en taille réelle](updating-the-tableadapter-to-use-joins-cs/_static/image23.png))


## <a name="step-3-updating-the-datatable-s-columns"></a>Étape 3 : Mise à jour les colonnes de s DataTable

À ce stade, le `Employees_Select` procédure stockée renvoie `ManagerFirstName` et `ManagerLastName` valeurs, mais le `EmployeesDataTable` manque de ces colonnes. Ces colonnes manquantes peuvent être ajoutées à la table de données de deux manières :

- **Manuellement** - avec le bouton droit sur la table de données dans le Concepteur de DataSet et, dans le menu Ajouter, choisissez la colonne. Vous pouvez nommer la colonne, puis définissez ses propriétés en conséquence.
- **Automatiquement** -l’Assistant Configuration de TableAdapter met à jour les colonnes de s DataTable pour refléter les champs retournés par la `SelectCommand` procédure stockée. Lorsque vous utilisez des instructions SQL ad hoc, l’Assistant va également supprimer la `InsertCommand`, `UpdateCommand`, et `DeleteCommand` propriétés depuis le `SelectCommand` contient maintenant un `JOIN`. Mais lorsque vous utilisez des procédures stockées, ces propriétés d’une commande restent intactes.

Nous avons déjà exploré manuellement ajouter des colonnes de table de données dans les didacticiels précédents, y compris [maître/détail à l’aide d’une liste à puces des enregistrements maîtres avec une DataList des détails](../filtering-scenarios-with-the-datalist-and-repeater/master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs.md) et [télécharger des fichiers](../working-with-binary-files/uploading-files-cs.md), et nous allons Examinez à nouveau ce processus plus en détail dans notre didacticiel suivant. Pour ce didacticiel, toutefois, laisser s utiliser l’approche automatique via l’Assistant Configuration de TableAdapter.

Démarrer en cliquant sur le `EmployeesTableAdapter` et en sélectionnant configurer dans le menu contextuel. Ceci fait apparaître l’Assistant Configuration de TableAdapter, qui répertorie les procédures stockées utilisées pour la sélection, insertion, mise à jour et suppression, ainsi que leurs valeurs de retour et les paramètres (le cas échéant). La figure 10 illustre cet Assistant. Ici, nous pouvons voir que le `Employees_Select` procédure stockée maintenant renvoie le `ManagerFirstName` et `ManagerLastName` champs.


[![Le montre l’Assistant la liste des colonnes mises à jour pour le Employees_Select de procédure stockée](updating-the-tableadapter-to-use-joins-cs/_static/image25.png)](updating-the-tableadapter-to-use-joins-cs/_static/image24.png)

**Figure 10**: l’Assistant affiche la liste des colonnes mises à jour pour le `Employees_Select` la procédure stockée ([cliquez pour afficher l’image en taille réelle](updating-the-tableadapter-to-use-joins-cs/_static/image26.png))


Terminez l’Assistant en cliquant sur Terminer. Une fois de retour vers le Concepteur de DataSet, la `EmployeesDataTable` inclut deux colonnes supplémentaires : `ManagerFirstName` et `ManagerLastName`.


[![Le EmployeesDataTable contient deux nouvelles colonnes](updating-the-tableadapter-to-use-joins-cs/_static/image28.png)](updating-the-tableadapter-to-use-joins-cs/_static/image27.png)

**Figure 11**: le `EmployeesDataTable` contient deux nouvelles colonnes ([cliquez pour afficher l’image en taille réelle](updating-the-tableadapter-to-use-joins-cs/_static/image29.png))


Pour montrer que la mise à jour `Employees_Select` procédure stockée est en vigueur et que l’instruction insert, update et delete des fonctionnalités du TableAdapter sont toujours fonctionnels, s permettent de créer une page web qui permet aux utilisateurs d’afficher et supprimer des employés. Avant de créer une telle page, toutefois, nous devons tout d’abord créer une nouvelle classe dans la couche de logique métier pour travailler avec les employés de la `NorthwindWithSprocs` jeu de données. À l’étape 4, nous allons créer un `EmployeesBLLWithSprocs` classe. À l’étape 5, nous allons utiliser cette classe à partir d’une page ASP.NET.

## <a name="step-4-implementing-the-business-logic-layer"></a>Étape 4 : Implémentation de la couche de logique métier

Créer un nouveau fichier de classe dans le `~/App_Code/BLL` dossier nommé `EmployeesBLLWithSprocs.cs`. Cette classe imite la sémantique existants `EmployeesBLL` (classe), uniquement cette nouvelle une fournit des méthodes moins et utilise le `NorthwindWithSprocs` jeu de données (au lieu du `Northwind` jeu de données). Ajoutez le code suivant à la classe `EmployeesBLLWithSprocs`.


[!code-csharp[Main](updating-the-tableadapter-to-use-joins-cs/samples/sample6.cs)]

Le `EmployeesBLLWithSprocs` classe s `Adapter` propriété retourne une instance de la `NorthwindWithSprocs` DataSet s `EmployeesTableAdapter`. Cela est utilisé par la classe s `GetEmployees` et `DeleteEmployee` méthodes. Le `GetEmployees` les appels de méthode le `EmployeesTableAdapter` s correspondant `GetEmployees` (méthode), qui appelle le `Employees_Select` procédure stockée et remplit ses résultats dans un `EmployeeDataTable`. Le `DeleteEmployee` méthode même appelle le `EmployeesTableAdapter` s `Delete` (méthode), qui appelle le `Employees_Delete` procédure stockée.

## <a name="step-5-working-with-the-data-in-the-presentation-layer"></a>Étape 5 : Utilisation des données dans la couche de présentation

Avec la `EmployeesBLLWithSprocs` classe terminée, nous vous êtes prêt à travailler avec des données sur les employés via une page ASP.NET. Ouvrir le `JOINs.aspx` page dans le `AdvancedDAL` dossier et faites glisser un GridView à partir de la boîte à outils vers le concepteur, en définissant son `ID` propriété `Employees`. Ensuite, à partir de la balise active de s GridView, lier la grille pour un nouveau contrôle ObjectDataSource nommé `EmployeesDataSource`.

Configurer l’ObjectDataSource à utiliser le `EmployeesBLLWithSprocs` classe et, dans les onglets SELECT et DELETE, vérifiez que le `GetEmployees` et `DeleteEmployee` méthodes sont sélectionnés dans la liste déroulante. Cliquez sur Terminer pour terminer la configuration de s ObjectDataSource.


[![Configurer pour utiliser la classe EmployeesBLLWithSprocs ObjectDataSource](updating-the-tableadapter-to-use-joins-cs/_static/image31.png)](updating-the-tableadapter-to-use-joins-cs/_static/image30.png)

**Figure 12**: configurer ObjectDataSource à utiliser le `EmployeesBLLWithSprocs` classe ([cliquez pour afficher l’image en taille réelle](updating-the-tableadapter-to-use-joins-cs/_static/image32.png))


[![Avoir l’utilisation de l’ObjectDataSource les GetEmployees et les méthodes de DeleteEmployee](updating-the-tableadapter-to-use-joins-cs/_static/image34.png)](updating-the-tableadapter-to-use-joins-cs/_static/image33.png)

**Figure 13**: ont l’utilisation de l’ObjectDataSource le `GetEmployees` et `DeleteEmployee` méthodes ([cliquez pour afficher l’image en taille réelle](updating-the-tableadapter-to-use-joins-cs/_static/image35.png))


Visual Studio ajoutera un BoundField au GridView pour chacun de la `EmployeesDataTable` des colonnes de s. Supprimez tous ces BoundFields à l’exception de `Title`, `LastName`, `FirstName`, `ManagerFirstName`, et `ManagerLastName` et renommer le `HeaderText` propriétés pour les quatre derniers BoundFields pour nom, prénom, prénom Manager s, et Gestionnaire de s nom, respectivement.

Pour permettre aux utilisateurs de supprimer des employés à partir de cette page, nous devons faire deux choses. Tout d’abord, demandez à GridView pour fournir des capacités de suppression en sélectionnant l’option Activer la suppression à partir de sa balise active. Ensuite, modifiez le s ObjectDataSource `OldValuesParameterFormatString` propriété à partir de la valeur définie par l’Assistant ObjectDataSource (`original_{0}`) à sa valeur par défaut (`{0}`). Après avoir apporté ces modifications, votre balisage déclaratif s GridView et ObjectDataSource doit ressembler à ce qui suit :


[!code-aspx[Main](updating-the-tableadapter-to-use-joins-cs/samples/sample7.aspx)]

Tester la page en accédant à via un navigateur. Comme le montre la Figure 14, la page répertorie chaque employé et son gestionnaire s nom (en supposant qu’il en a une).


[![La jointure dans la Employees_Select procédure stockée renvoie le nom de s Manager](updating-the-tableadapter-to-use-joins-cs/_static/image37.png)](updating-the-tableadapter-to-use-joins-cs/_static/image36.png)

**Figure 14**: le `JOIN` dans le `Employees_Select` procédure stockée retourne le nom de gestionnaire ([cliquez pour afficher l’image en taille réelle](updating-the-tableadapter-to-use-joins-cs/_static/image38.png))


En cliquant sur le bouton Supprimer démarre le workflow de suppression, ce qui se termine par l’exécution de la `Employees_Delete` procédure stockée. Toutefois, la tentative `DELETE` instruction dans la procédure stockée échoue en raison d’une violation de contrainte de clé étrangère (voir Figure 15). Plus précisément, chaque employé possède un ou plusieurs enregistrements la `Orders` table, à l’origine de la suppression échoue.


[![Suppression d’un employé qui a des résultats de commandes correspondantes dans une Violation de contrainte de clé étrangère](updating-the-tableadapter-to-use-joins-cs/_static/image40.png)](updating-the-tableadapter-to-use-joins-cs/_static/image39.png)

**Figure 15**: suppression d’un employé qui a des résultats de commandes correspondantes dans une Violation de contrainte de clé étrangère ([cliquez pour afficher l’image en taille réelle](updating-the-tableadapter-to-use-joins-cs/_static/image41.png))


Pour autoriser un employé à être supprimé vous pouvez :

- Mettre à jour de la contrainte de clé étrangère pour enchaîner des suppressions,
- Supprimez manuellement les enregistrements à partir de la `Orders` table des employés que vous souhaitez supprimer, ou
- Mise à jour le `Employees_Delete` procédure stockée pour tout d’abord supprimer les enregistrements associés à partir de la `Orders` table avant de supprimer le `Employees` enregistrement. Nous avons abordé cette technique dans le [à l’aide des procédures stockées existantes pour s DataSet typée TableAdapters](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs.md) didacticiel.

Je laisse cela en guise d’exercice pour le lecteur.

## <a name="summary"></a>Récapitulatif

Lorsque vous travaillez avec des bases de données relationnelle, il est courant pour les requêtes extraire leurs données à partir de plusieurs des tables associées. Sous-requêtes en corrélation et `JOIN` fournissent deux techniques différentes pour accéder aux données à partir de tables associées dans une requête. Dans des didacticiels précédents, nous avons apporté plus fréquemment utilisent des sous-requêtes corrélées, car le TableAdapter ne peut pas générer automatiquement `INSERT`, `UPDATE`, et `DELETE` instructions pour les requêtes impliquant `JOIN` s. Bien que ces valeurs peuvent être fournies manuellement, lors de l’utilisation d’instructions SQL ad hoc toutes les personnalisations sont remplacées lorsque l’Assistant Configuration de TableAdapter est terminé.

Heureusement, les TableAdapters créés à l’aide de procédures stockées ne sont pas confrontés à partir de la fragilité de même que ceux créés à l’aide d’instructions SQL ad hoc. Par conséquent, il est possible de créer un TableAdapter dont requête principale utilise un `JOIN` lors de l’utilisation de procédures stockées. Dans ce didacticiel, nous avons vu comment créer un TableAdapter de ce type. Nous avons commencé à l’aide un `JOIN`-moins `SELECT` de requête pour la requête principale de TableAdapter s afin que le correspondante insert, update et les procédures de suppression stockée soient créées automatiquement. Le TableAdapter s initial configuration terminée, nous avons augmenté la `SelectCommand` une procédure stockée à utiliser un `JOIN` et ré-exécuté l’Assistant Configuration de TableAdapter pour mettre à jour le `EmployeesDataTable` des colonnes de s.

Exécuter à nouveau l’Assistant Configuration de TableAdapter automatiquement mis à jour le `EmployeesDataTable` colonnes afin de refléter les champs de données retournés par la `Employees_Select` procédure stockée. Vous pouvez également nous aurions pu ajouter ces colonnes manuellement au DataTable. Nous allons explorer les ajouter manuellement des colonnes au DataTable dans le didacticiel suivant.

Bonne programmation !

## <a name="about-the-author"></a>À propos de l’auteur

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), auteur de sept les livres sur ASP/ASP.NET et fondateur de [4GuysFromRolla.com](http://www.4guysfromrolla.com), travaille avec les technologies Web Microsoft depuis 1998. Scott fonctionne comme un consultant indépendant, formateur et writer. Son dernier ouvrage est [*SAM animer vous-même ASP.NET 2.0 des dernières 24 heures*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Il peut être contacté à [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou via son blog, qui se trouve à [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Remerciements

Cette série de didacticiels a été révisée par plusieurs réviseurs utiles. Les réviseurs tête pour ce didacticiel ont été Hilton Geisenow, David Suru et Teresa Murphy. Qui souhaitent consulter mes prochains articles MSDN ? Dans ce cas, envoyez-moi une ligne à [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Précédent](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs.md)
> [Suivant](adding-additional-datatable-columns-cs.md)
