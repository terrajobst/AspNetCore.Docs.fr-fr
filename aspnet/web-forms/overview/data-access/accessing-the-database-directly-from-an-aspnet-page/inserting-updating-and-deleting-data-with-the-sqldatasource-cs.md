---
uid: web-forms/overview/data-access/accessing-the-database-directly-from-an-aspnet-page/inserting-updating-and-deleting-data-with-the-sqldatasource-cs
title: Insertion, mise à jour et suppression de données avec SqlDataSource (c#) | Microsoft Docs
author: rick-anderson
description: Dans les didacticiels précédents, nous avons appris comment le contrôle ObjectDataSource autorisé pour l’insertion, la mise à jour et suppression de données. Le contrôle SqlDataSource prend en charge t...
ms.author: aspnetcontent
ms.date: 02/20/2007
ms.assetid: a526f0ec-779e-4a2b-a476-6604090d25ce
msc.legacyurl: /web-forms/overview/data-access/accessing-the-database-directly-from-an-aspnet-page/inserting-updating-and-deleting-data-with-the-sqldatasource-cs
msc.type: authoredcontent
ms.openlocfilehash: d2958cb9cb0dd5ed89988a969663022a920a3dca
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37812845"
---
<a name="inserting-updating-and-deleting-data-with-the-sqldatasource-c"></a>Insertion, mise à jour et suppression de données avec SqlDataSource (c#)
====================
par [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Télécharger l’exemple d’application](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_49_CS.exe) ou [télécharger le PDF](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/datatutorial49cs1.pdf)

> Dans les didacticiels précédents, nous avons appris comment le contrôle ObjectDataSource autorisé pour l’insertion, la mise à jour et suppression de données. Le contrôle SqlDataSource prend en charge les mêmes opérations, mais l’approche est différente, et ce didacticiel montre comment configurer SqlDataSource pour insérer, mettre à jour et supprimer des données.


## <a name="introduction"></a>Introduction

Comme indiqué dans [une vue d’ensemble d’insertion, mise à jour et suppression](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs.md), le contrôle GridView fournit la mise à jour intégrée et prennent en charge des fonctionnalités de suppression, tandis que les contrôles DetailsView et FormView incluent insertion avec modification et suppression de fonctionnalités. Ces fonctionnalités de modification de données peuvent être intégrées directement dans un contrôle de source de données sans une ligne de code devant être écrit. [Une vue d’ensemble d’insertion, mise à jour et suppression](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs.md) examiné à l’aide d’ObjectDataSource pour faciliter l’insertion, de la mise à jour et de suppression avec les contrôles GridView, DetailsView et FormView. Vous pouvez également SqlDataSource peut être utilisé à la place de l’ObjectDataSource.

N’oubliez pas que pour prendre en charge d’insertion, de la mise à jour et de suppression, avec ObjectDataSource dont nous avions besoin pour spécifier les méthodes de couche objet à appeler pour effectuer l’insertion, mettre à jour ou supprimer l’action. Avec SqlDataSource, nous devons fournir `INSERT`, `UPDATE`, et `DELETE` SQL instructions (ou des procédures stockées) à exécuter. Comme nous le verrons dans ce didacticiel, ces instructions peuvent être créées manuellement ou peuvent être générées automatiquement par l’Assistant Configurer la Source de données de s SqlDataSource.

> [!NOTE]
> Dans la mesure où ve vu l’insertion, la modification et la suppression des fonctionnalités du contrôle GridView, DetailsView et contrôle FormView, ce didacticiel se concentrera sur la configuration du contrôle SqlDataSource pour prendre en charge ces opérations. Si vous souhaitez rafraîchir vos connaissances sur l’implémentation de ces fonctionnalités dans le retour de GridView, DetailsView et FormView, à des didacticiels Édition, insertion et suppression de données, en commençant par [une vue d’ensemble d’insertion, mise à jour et suppression](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs.md).


## <a name="step-1-specifyinginsertupdate-anddeletestatements"></a>Étape 1 : Spécifier`INSERT`,`UPDATE`, et`DELETE`instructions

Comme nous ve vu dans les deux derniers didacticiels, pour récupérer des données à partir d’un contrôle SqlDataSource que nous devons définie deux propriétés :

1. `ConnectionString`, qui spécifie ce que la base de données pour envoyer la requête, et
2. `SelectCommand`, qui spécifie l’instruction de SQL ad hoc ou le nom de la procédure stockée à exécuter pour retourner les résultats.

Pour `SelectCommand` valeurs avec des paramètres, le paramètre de valeurs sont spécifiées par le biais de la s SqlDataSource `SelectParameters` collection et peut inclure des valeurs codées en dur, valeurs source des paramètres communs (champs querystring, les variables de session, les valeurs de contrôle Web, et ainsi de suite), ou peut être affectée par programmation. Lorsque le contrôle SqlDataSource s `Select()` méthode est appelée par programme ou automatiquement à partir d’un contrôle Web de données une connexion à la base de données est établie, les valeurs de paramètre sont assignées à la requête, et la commande est transmise désactivé au base de données. Les résultats sont ensuite renvoyés comme un jeu de données ou d’un DataReader, selon la valeur du contrôle s `DataSourceMode` propriété.

Ainsi que la sélection de données, le contrôle SqlDataSource peut être utilisé pour insérer, mettre à jour et supprimer des données en fournissant `INSERT`, `UPDATE`, et `DELETE` instructions SQL de la même façon. Il vous suffit d’affecter la `InsertCommand`, `UpdateCommand`, et `DeleteCommand` propriétés la `INSERT`, `UPDATE`, et `DELETE` instructions SQL à exécuter. Si les instructions ont des paramètres (comme ils le seront plus toujours), les inclure dans le `InsertParameters`, `UpdateParameters`, et `DeleteParameters` collections.

Une fois un `InsertCommand`, `UpdateCommand`, ou `DeleteCommand` valeur a été spécifiée, l’option Activer l’insertion, activer la modification ou activer la suppression dans les données correspondantes de balise active du contrôle s Web n’est disponible. Pour illustrer ceci, s permettent de prendre un exemple à partir de la `Querying.aspx` page que nous avons créé dans le [interrogation des données avec le contrôle SqlDataSource](querying-data-with-the-sqldatasource-control-cs.md) didacticiel et accroître ce qu’il inclue supprimer des fonctionnalités.

Commencez par ouvrir le `InsertUpdateDelete.aspx` et `Querying.aspx` pages à partir de la `SqlDataSource` dossier. À partir du concepteur sur le `Querying.aspx` , sélectionnez le SqlDataSource et GridView du premier exemple (le `ProductsDataSource` et `GridView1` contrôles). Après avoir sélectionné les deux contrôles, accédez au menu Edition et choisissez Copier (ou simplement appuyer sur Ctrl + C). Ensuite, accédez au Concepteur de `InsertUpdateDelete.aspx` et collez dans les contrôles. Après avoir déplacé les deux contrôles à `InsertUpdateDelete.aspx`, testez la page dans un navigateur. Vous devez voir les valeurs de la `ProductID`, `ProductName`, et `UnitPrice` colonnes pour tous les enregistrements dans la `Products` table de base de données.


[![Tous les produits sont répertoriés, classés par ProductID](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image1.gif)](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image1.png)

**Figure 1**: tous les produits sont répertoriés, classés par `ProductID` ([cliquez pour afficher l’image en taille réelle](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image2.png))


## <a name="adding-the-sqldatasource-sdeletecommandanddeleteparametersproperties"></a>Ajout de la s SqlDataSource`DeleteCommand`et`DeleteParameters`propriétés

À ce stade, nous avons un SqlDataSource qui renvoie simplement tous les enregistrements à partir de la `Products` table et un GridView qui affiche ces données. Notre objectif est d’étendre cet exemple pour autoriser l’utilisateur peut supprimer des produits via le contrôle GridView. Pour cela nous devons spécifier des valeurs pour le contrôle SqlDataSource s `DeleteCommand` et `DeleteParameters` propriétés, puis configurez le contrôle GridView pour prendre en charge la suppression.

Le `DeleteCommand` et `DeleteParameters` propriétés peuvent être spécifiées dans une de plusieurs façons :

- Via la syntaxe déclarative
- À partir de la fenêtre Propriétés dans le Concepteur
- À partir de l’écran de procédure stockée dans l’Assistant Configurer la Source de données ou de spécifier une instruction SQL personnalisée
- Via le bouton Avancé dans Spécifiez les colonnes d’une table de l’écran d’affichage dans l’Assistant Configurer la Source de données, ce qui génère réellement automatiquement le `DELETE` collection d’instructions et des paramètres SQL utilisée dans les `DeleteCommand` et `DeleteParameters` propriétés

Nous allons examiner comment ont automatiquement la `DELETE` instruction créée à l’étape 2. Pour l’instant, permettent de s utiliser la fenêtre Propriétés dans le concepteur, bien que l’Assistant Configurer la Source de données ou l’option de syntaxe déclarative fonctionnerait tout aussi bien.

À partir du concepteur dans `InsertUpdateDelete.aspx`, cliquez sur le `ProductsDataSource` SqlDataSource puis affichez la fenêtre Propriétés (dans le menu Affichage, choisissez la fenêtre Propriétés, ou simplement appuyer sur F4). Sélectionnez la propriété DeleteQuery, qui fait apparaître un ensemble de points de suspension.


![Sélectionnez la propriété DeleteQuery à partir de la fenêtre Propriétés](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image2.gif)

**Figure 2**: sélectionnez la propriété DeleteQuery à partir de la fenêtre Propriétés


> [!NOTE]
> SqlDataSource n t ont une propriété DeleteQuery. Au lieu de cela, DeleteQuery est une combinaison de la `DeleteCommand` et `DeleteParameters` propriétés et est uniquement répertorié dans la fenêtre Propriétés lorsque vous affichez la fenêtre via le concepteur. Si vous examinez la fenêtre Propriétés dans la vue de Source, vous trouverez le `DeleteCommand` propriété à la place.


Cliquez sur le bouton de sélection dans la propriété DeleteQuery pour afficher la boîte de dialogue Éditeur de commandes et paramètre zone (voir Figure 3). À partir de cette boîte de dialogue, vous pouvez spécifier la `DELETE` instruction SQL et spécifiez les paramètres. Entrez la requête suivante dans le `DELETE` zone de texte de commande (soit manuellement ou en utilisant le Générateur de requêtes, si vous préférez) :

[!code-sql[Main](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/samples/sample1.sql)]

Ensuite, cliquez sur le bouton Actualiser les paramètres pour ajouter le `@ProductID` paramètre à la liste des paramètres ci-dessous.


[![Sélectionnez la propriété DeleteQuery à partir de la fenêtre Propriétés](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image3.gif)](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image3.png)

**Figure 3**: sélectionnez la propriété DeleteQuery à partir de la fenêtre Propriétés ([cliquez pour afficher l’image en taille réelle](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image4.png))


Faire *pas* fournir une valeur pour ce paramètre (laisser la source de son paramètre à None). Une fois que nous ajoutons la suppression de prise en charge pour le contrôle GridView, le contrôle GridView fournit automatiquement cette valeur de paramètre, à l’aide de la valeur de son `DataKeys` collection de la ligne dont bouton Supprimer l’utilisateur a cliqué.

> [!NOTE]
> Le nom du paramètre utilisé dans le `DELETE` requête *doit* être le même que le nom de la `DataKeyNames` valeur dans le GridView, DetailsView ou FormView. Autrement dit, le paramètre dans le `DELETE` instruction est délibérément nommée `@ProductID` (au lieu de, par exemple, `@ID`), car le nom de colonne de clé primaire dans la table Products (et, par conséquent, la valeur DataKeyNames dans le contrôle GridView) est `ProductID`.


Si le nom du paramètre et `DataKeyNames` correspondance de valeur n t, le contrôle GridView ne peut pas attribuer automatiquement le paramètre la valeur à partir de la `DataKeys` collection.

Après avoir entré les informations relatives à la suppression dans la boîte de dialogue Éditeur de commandes et paramètres, cliquez sur OK et accédez à la vue de Source à examiner le balisage déclaratif qui en résulte :

[!code-aspx[Main](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/samples/sample2.aspx)]

Notez l’ajout de la `DeleteCommand` propriété ainsi que le `<DeleteParameters>` section et l’objet de paramètre nommé `productID`.

## <a name="configuring-the-gridview-for-deleting"></a>Configurer le contrôle GridView pour la suppression

Avec le `DeleteCommand` propriété ajoutée, la balise active de GridView s contient à présent l’option Activer la suppression. Continuons et activez cette case à cocher. Comme indiqué dans [une vue d’ensemble d’insertion, mise à jour et suppression](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs.md), cela entraîne le contrôle GridView à ajouter un CommandField avec son `ShowDeleteButton` propriété définie sur `true`. Comme la Figure 4 montre, lorsque la page est visitée via un navigateur, un bouton Supprimer est inclus. Testez cette page out en supprimant certains produits.


[![Chaque ligne GridView inclut désormais un bouton Supprimer](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image4.gif)](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image5.png)

**Figure 4**: chaque ligne GridView inclut désormais un bouton Supprimer ([cliquez pour afficher l’image en taille réelle](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image6.png))


Lorsque vous cliquez sur un bouton Supprimer, une publication (postback) se produit, le contrôle GridView affecte le `ProductID` paramètre la valeur de la `DataKeys` valeur de collection pour la ligne dont bouton Supprimer l’utilisateur a cliqué et appelle les opérations de mappage SqlDataSource `Delete()` (méthode). Le contrôle SqlDataSource, puis se connecte à la base de données et exécute la `DELETE` instruction. Le contrôle GridView puis relie à SqlDataSource, récupérer et afficher l’ensemble actuel des produits (qui n’inclut plus l’enregistrement supprimé juste).

> [!NOTE]
> Étant donné que le contrôle GridView utilise son `DataKeys` collection pour remplir les paramètres SqlDataSource, il s vitales qui le s GridView `DataKeyNames` propriété être définie sur l’ou les colonnes qui constituent la clé primaire et que les opérations de mappage SqlDataSource `SelectCommand` retourne ces colonnes. En outre, il s important que le paramètre de nom dans le s SqlDataSource `DeleteCommand` est défini sur `@ProductID`. Si le `DataKeyNames` propriété n’est pas définie ou le paramètre n’est pas nommé `@ProductsID`, en cliquant sur le bouton Supprimer une publication (postback), tout en t gagné / supprime réellement de n’importe quel enregistrement.


Figure 5 illustre cette interaction graphiquement. Faire référence à la [examinant les événements associés insertion, mise à jour et suppression](../editing-inserting-and-deleting-data/examining-the-events-associated-with-inserting-updating-and-deleting-cs.md) didacticiel pour une présentation plus détaillée de la chaîne d’événements associée d’insertion, de la mise à jour et de suppression d’un contrôle Web de données.


![En cliquant sur le bouton Supprimer dans le contrôle GridView appelle la méthode de Delete() s SqlDataSource](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image5.gif)

**Figure 5**: en cliquant sur le bouton Supprimer dans le contrôle GridView appelle les opérations de mappage SqlDataSource `Delete()` (méthode)


## <a name="step-2-automatically-generating-theinsertupdate-anddeletestatements"></a>Étape 2 : Générer automatiquement le`INSERT`,`UPDATE`, et`DELETE`instructions

En tant qu’étape 1 examinées, `INSERT`, `UPDATE`, et `DELETE` instructions SQL peuvent être spécifiées via la fenêtre Propriétés ou de la syntaxe déclarative du contrôle s. Toutefois, cette approche nécessite que nous manuellement écrire les instructions SQL à la main, qui peut être monotone et sujette aux erreurs. Heureusement, l’Assistant Configurer la Source de données fournit une option pour que le `INSERT`, `UPDATE`, et `DELETE` instructions générées automatiquement lors de l’utilisation des spécifiez les colonnes d’une table de l’écran d’affichage.

Permettent d’Explorer cette option de génération automatique de s. Ajouter un contrôle DetailsView au concepteur dans `InsertUpdateDelete.aspx` et définissez son `ID` propriété `ManageProducts`. Ensuite, à partir de la balise active de s DetailsView, choisissez Créer une nouvelle source de données et de créer un SqlDataSource nommé `ManageProductsDataSource`.


[![Créer un nouveau SqlDataSource nommé ManageProductsDataSource](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image6.gif)](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image7.png)

**Figure 6**: créer une nouvelle nommée de SqlDataSource `ManageProductsDataSource` ([cliquez pour afficher l’image en taille réelle](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image8.png))


À partir de l’Assistant Configurer la Source de données, choisir d’utiliser le `NORTHWINDConnectionString` connexion chaîne et cliquez sur Suivant. À partir de la configuration de l’écran de l’instruction Select, laissez les spécifiez les colonnes à partir d’un bouton radio table ou vue sélectionné et choisir le `Products` table dans la liste déroulante. Sélectionnez le `ProductID`, `ProductName`, `UnitPrice`, et `Discontinued` colonnes dans la liste de case à cocher.


[![À l’aide de la Table Products, retourner le ProductID, ProductName, UnitPrice et les colonnes supprimées](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image7.gif)](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image9.png)

**Figure 7**: à l’aide de la `Products` Table, retournez le `ProductID`, `ProductName`, `UnitPrice`, et `Discontinued` colonnes ([cliquez pour afficher l’image en taille réelle](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image10.png))


Pour générer automatiquement `INSERT`, `UPDATE`, et `DELETE` instructions basées sur la table sélectionnée et les colonnes, cliquez sur le bouton Avancé et vérifiez la générer `INSERT`, `UPDATE`, et `DELETE` instructions case.


![Vérifiez la case à cocher de relevés générer INSERT, UPDATE et DELETE](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image8.gif)

**Figure 8**: Vérifiez la générer `INSERT`, `UPDATE`, et `DELETE` instructions case à cocher


La génération `INSERT`, `UPDATE`, et `DELETE` instructions case sera peut être activée uniquement si la table sélectionnée a une clé primaire et la colonne de clé primaire (ou les colonnes) sont incluses dans la liste des colonnes retournées. La case utiliser l’accès concurrentiel optimiste, qui peut être sélectionnée une fois la génération `INSERT`, `UPDATE`, et `DELETE` case à cocher des instructions ont été vérifiés, augmenterons le `WHERE` clauses dans résultant `UPDATE` et `DELETE` instructions pour fournir un contrôle d’accès concurrentiel optimiste. Pour l’instant, laissez cette case à cocher désactivée ; Nous allons examiner l’accès concurrentiel optimiste avec le contrôle SqlDataSource dans le didacticiel suivant.

Après avoir vérifié la générer `INSERT`, `UPDATE`, et `DELETE` instructions case, cliquez sur OK pour revenir à l’écran configurer l’instruction Select, puis cliquez sur Suivant, puis terminez l’Assistant Configurer la Source de données. À la fin de l’Assistant, Visual Studio ajoutera BoundFields pour le contrôle DetailsView pour le `ProductID`, `ProductName`, et `UnitPrice` colonnes et un CheckBoxField pour le `Discontinued` colonne. À partir de la balise active de s DetailsView, cochez la case Activer la pagination afin que l’utilisateur visite cette page peut parcourir les produits. Désactivez également les s DetailsView `Width` et `Height` propriétés.

Notez que la balise active a les activer l’insertion, activer la modification et activer la suppression des options disponibles. Il s’agit, car le SqlDataSource contient des valeurs pour ses `InsertCommand`, `UpdateCommand`, et `DeleteCommand`, comme illustré dans la syntaxe déclarative suivante :

[!code-aspx[Main](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/samples/sample3.aspx)]

Notez comment le contrôle SqlDataSource a les valeurs automatiquement définies pour son `InsertCommand`, `UpdateCommand`, et `DeleteCommand` propriétés. Le jeu de colonnes référencées dans le `InsertCommand` et `UpdateCommand` propriétés sont basées sur celles de la `SELECT` instruction. Autrement dit, au lieu d’avoir *chaque* colonne de produits dans le `InsertCommand` et `UpdateCommand`, il existe uniquement les colonnes spécifiées dans le `SelectCommand` (moins `ProductID`, qui est omis, car il s un [ `IDENTITY` colonne](http://www.sqlteam.com/item.asp?ItemID=102), dont la valeur ne peut pas être modifiée lors de la modification et qui est automatiquement attribué lors de l’insertion). En outre, pour chaque paramètre dans le `InsertCommand`, `UpdateCommand`, et `DeleteCommand` propriétés, il existe des paramètres correspondants dans le `InsertParameters`, `UpdateParameters`, et `DeleteParameters` collections.

Pour activer les fonctionnalités de modification de données s DetailsView, vérifiez les activer l’insertion, activer la modification et activer la suppression des options dans sa balise active. Cette opération ajoute une CommandField avec son `ShowInsertButton`, `ShowEditButton`, et `ShowDeleteButton` propriétés définies sur `true`.

Visitez la page dans un navigateur et notez la modification, les suppressions et les nouveaux boutons inclus dans le contrôle DetailsView. En cliquant sur le bouton modifier Active le contrôle DetailsView en mode édition, qui affiche chaque BoundField dont `ReadOnly` propriété est définie sur `false` (la valeur par défaut) comme une zone de texte et le CheckBoxField comme une case à cocher.


[![Les opérations de mappage DetailsView par défaut d’Interface de modification](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image9.gif)](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image11.png)

**Figure 9**: s DetailsView l’Interface de modification par défaut ([cliquez pour afficher l’image en taille réelle](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image12.png))


De même, vous pouvez supprimer le produit actuellement sélectionné ou ajouter un nouveau produit dans le système. Dans la mesure où le `InsertCommand` instruction fonctionne uniquement avec les `ProductName`, `UnitPrice`, et `Discontinued` colonnes, les autres colonnes ont soit `NULL` ou leur valeur par défaut affectée par la base de données à l’insertion. Tout comme avec ObjectDataSource, si le `InsertCommand` manque à n’importe quelle table de base de données que les colonnes qui ne pas autorisent `NULL` s et ne pas ayant une valeur par défaut, une erreur SQL se produit lorsque vous tentez d’exécuter la `INSERT` instruction.

> [!NOTE]
> Le s DetailsView insertion et la modification des interfaces ne disposent pas d’une forme quelconque de personnalisation ou de validation. Pour ajouter des contrôles de validation ou pour personnaliser les interfaces, vous devez convertir le BoundFields TemplateField. Reportez-vous à la [Ajout de contrôles de Validation pour l’édition et insertion des Interfaces](../editing-inserting-and-deleting-data/adding-validation-controls-to-the-editing-and-inserting-interfaces-cs.md) et [personnalisation de l’Interface de Modification de données](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-cs.md) didacticiels pour plus d’informations.


En outre, n’oubliez pas que pour la mise à jour et la suppression, le contrôle DetailsView utilise le produit actuel s `DataKey` valeur, qui n’est présent que si le `DataKeyNames` propriété est configurée. Si la modification ou la suppression semble n’ont aucun effet, vérifiez que le `DataKeyNames` propriété est définie.

## <a name="limitations-of-automatically-generating-sql-statements"></a>Limitations de générer automatiquement des instructions SQL

Depuis la générer `INSERT`, `UPDATE`, et `DELETE` instructions option est uniquement disponible lors de la sélection des colonnes d’une table, pour les requêtes plus complexes vous devrez écrire votre propre `INSERT`, `UPDATE`, et `DELETE` instructions comme nous l’avons fait à l’étape 1. En général, SQL `SELECT` instructions utilisent `JOIN` s pour ramener à des fins d’affichage des données à partir d’une ou plusieurs tables de choix pour (telles que la remise la `Categories` table s `CategoryName` champ lors de l’affichage des informations sur les produits). En même temps, nous souhaitions autoriser l’utilisateur à modifier, mettre à jour ou insérer des données dans la table de base (`Products`, dans ce cas).

Bien que le `INSERT`, `UPDATE`, et `DELETE` instructions peuvent être entrés manuellement, envisagez de l’info-bulle de gagner du temps suivant. Initialement le programme d’installation SqlDataSource afin qu’il extrait des données uniquement dans le `Products` table. Utiliser les colonnes de spécifier de s d’Assistant Configurer la Source de données à partir d’un écran de la table ou la vue afin que vous pouvez générer automatiquement le `INSERT`, `UPDATE`, et `DELETE` instructions. À l’issue de l’Assistant, puis configurer le SelectQuery à partir de la fenêtre Propriétés (ou, vous pouvez également revenir à l’Assistant Configurer la Source de données, mais utilisez la spécifier une instruction SQL personnalisée ou une option de procédure stockée). Puis mettez à jour le `SELECT` instruction d’inclure le `JOIN` syntaxe. Cette technique offre les avantages de gagner du temps d’instructions SQL générées automatiquement, tout en permettant une plus personnalisée `SELECT` instruction.

Une autre limitation de générer automatiquement le `INSERT`, `UPDATE`, et `DELETE` instructions qui correspond aux colonnes de la `INSERT` et `UPDATE` instructions sont basées sur les colonnes retournées par la `SELECT` instruction. Nous devons mettre à jour ou insérer des champs plus ou moins, toutefois. Par exemple, dans l’exemple à l’étape 2, peut-être que nous souhaitons avoir le `UnitPrice` BoundField être en lecture seule. Dans ce cas, il ne doit pas t s’affichent dans le `UpdateCommand`. Ou bien, nous pouvons choisir de définir la valeur d’un champ de table qui n’apparaît pas dans le contrôle GridView. Par exemple, lorsque vous ajoutez un nouvel enregistrement nous souhaiterons peut-être le `QuantityPerUnit` la valeur TODO.

Si ces personnalisations sont requises, vous devez effectuer manuellement, par le biais de la fenêtre Propriétés, spécifiez une instruction SQL personnalisée ou option de procédure stockée dans l’Assistant ou via la syntaxe déclarative.

> [!NOTE]
> Lors de l’ajout de paramètres qui n’ont pas de champs correspondants dans les données de contrôle Web, n’oubliez pas que ces valeurs de paramètres devrez attribuer des valeurs dans un mode quelconque. Ces valeurs peuvent être : codées en dur directement dans le `InsertCommand` ou `UpdateCommand`; peuvent provenir d’une source prédéfinie (chaîne de requête, l’état de session, les contrôles Web sur la page et ainsi de suite) ; ou peut être affectée par programmation, comme nous l’avons vu dans le didacticiel précédent.


## <a name="summary"></a>Récapitulatif

Afin que les données de contrôles Web afin d’utiliser leurs intégrées d’insertion, la modification et la suppression de fonctionnalités, le contrôle de source de données qu’ils sont liés à doit offrir ce type de fonctionnalité. Pour le SqlDataSource, cela signifie que `INSERT`, `UPDATE`, et `DELETE` instructions SQL doivent être affectées à la `InsertCommand`, `UpdateCommand`, et `DeleteCommand` propriétés. Ces propriétés et les collections de paramètres correspondants, peuvent être ajoutées manuellement ou générées automatiquement via l’Assistant Configurer la Source de données. Dans ce didacticiel, nous avons examiné ces deux techniques.

Nous avons examiné à l’aide de l’accès concurrentiel optimiste avec ObjectDataSource dans le [implémentation de l’accès concurrentiel optimiste](../editing-inserting-and-deleting-data/implementing-optimistic-concurrency-cs.md) didacticiel. Le contrôle SqlDataSource fournit également la prise en charge de l’accès concurrentiel optimiste. Comme indiqué à l’étape 2, lorsque vous générez automatiquement le `INSERT`, `UPDATE`, et `DELETE` offre de l’Assistant d’instructions, utilisez l’option d’accès concurrentiel optimiste. Comme nous le verrons dans le didacticiel suivant, à l’aide de l’accès concurrentiel optimiste avec SqlDataSource modifie le `WHERE` clauses dans le `UPDATE` et `DELETE` instructions pour vous assurer que les valeurs pour les autres colonnes réalisées t modifié les données depuis le derniers affichée sur la page.

Bonne programmation !

## <a name="about-the-author"></a>À propos de l’auteur

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), auteur de sept les livres sur ASP/ASP.NET et fondateur de [4GuysFromRolla.com](http://www.4guysfromrolla.com), travaille avec les technologies Web Microsoft depuis 1998. Scott fonctionne comme un consultant indépendant, formateur et writer. Son dernier ouvrage est [*SAM animer vous-même ASP.NET 2.0 des dernières 24 heures*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Il peut être contacté à [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou via son blog, qui se trouve à [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Précédent](using-parameterized-queries-with-the-sqldatasource-cs.md)
> [Suivant](implementing-optimistic-concurrency-with-the-sqldatasource-cs.md)
