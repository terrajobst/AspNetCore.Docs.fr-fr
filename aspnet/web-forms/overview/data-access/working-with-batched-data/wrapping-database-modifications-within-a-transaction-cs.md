---
uid: web-forms/overview/data-access/working-with-batched-data/wrapping-database-modifications-within-a-transaction-cs
title: Habillage des Modifications de base de données dans une Transaction (c#) | Microsoft Docs
author: rick-anderson
description: Ce didacticiel est le premier des quatre examine la mise à jour, suppression et l’insertion des lots de données. Ce didacticiel explique comment autoriser des transactions de base de données...
ms.author: riande
ms.date: 06/26/2007
ms.assetid: b45fede3-c53a-4ea1-824b-20200808dbae
msc.legacyurl: /web-forms/overview/data-access/working-with-batched-data/wrapping-database-modifications-within-a-transaction-cs
msc.type: authoredcontent
ms.openlocfilehash: ab1ffa147545ab0d4fa0a3cce6f7dca91dfe3ffb
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41825759"
---
<a name="wrapping-database-modifications-within-a-transaction-c"></a>Inclusion de Modifications de base de données dans une Transaction (c#)
====================
par [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Télécharger le Code](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_63_CS.zip) ou [télécharger le PDF](wrapping-database-modifications-within-a-transaction-cs/_static/datatutorial63cs1.pdf)

> Ce didacticiel est le premier des quatre examine la mise à jour, suppression et l’insertion des lots de données. Ce didacticiel explique comment les transactions de base de données autorisent les modifications de traitement par lots à effectuer qu’une opération atomique, ce qui garantit que toutes les étapes réussissent ou échouent de toutes les étapes.


## <a name="introduction"></a>Introduction

Comme nous l’avons vu en commençant par le [une vue d’ensemble d’insertion, mise à jour et suppression des données](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs.md) didacticiel, le contrôle GridView fournit une prise en charge intégrée pour modification au niveau des lignes et la suppression. En quelques clics de souris, il est possible de créer une interface de modification des données riche sans écrire une ligne de code, tant que vous êtes contenu avec la modification et suppression sur une ligne par ligne. Toutefois, dans certains scénarios, cela est insuffisant, et nous avons besoin fournir aux utilisateurs la possibilité de modifier ou supprimer un lot d’enregistrements.

Par exemple, les clients de l’e-mail basés sur le web plus utilisent une grille pour répertorier chaque message où chaque ligne inclut une case à cocher ainsi que les informations de s e-mail (objet, expéditeur et ainsi de suite). Cette interface permet à l’utilisateur à supprimer plusieurs messages en y rechercher la présence puis en cliquant sur un bouton Supprimer les Messages. Une interface de modification d’une sélection est idéale dans les situations où les utilisateurs modifier couramment nombre d’enregistrements. Plutôt que de forcer l’utilisateur à cliquer sur Modifier, apporter leur modification, puis sur mise à jour pour chaque enregistrement qui doit être modifiée, une interface de modification d’une sélection restitue chaque ligne avec son interface de modification. L’utilisateur peut modifier rapidement l’ensemble de lignes qui doivent être modifiées et puis enregistrer ces modifications en cliquant sur un bouton tout mettre à jour. Dans cette série de didacticiels, nous allons examiner comment créer des interfaces pour l’insertion, la modification et la suppression des lots de données.

Lorsque vous effectuez des opérations par lots il s important de déterminer si elle doit être possible pour certaines opérations décrites dans le lot réussisse alors que d’autres échouent. Prendre en compte un lot de suppression de l’interface - ce qui doit se produire si le premier enregistrement sélectionné est supprimé avec succès, mais la deuxième échoue, par exemple, en raison d’une violation de contrainte de clé étrangère ? Doit d’abord le supprimer enregistrement s être restaurée ou est-il acceptable pour le premier enregistrement de rester supprimé ?

Si vous souhaitez l’opération de traitement par lots à traiter comme un [opération atomique](http://en.wikipedia.org/wiki/Atomic_operation), un où soit toutes les étapes réussissent ou toutes les étapes ont échoué, puis la couche d’accès aux données doivent être augmentées pour prendre en charge [base de données transactions](http://en.wikipedia.org/wiki/Database_transaction). Transactions de base de données garantissent l’atomicité pour l’ensemble de `INSERT`, `UPDATE`, et `DELETE` instructions exécutées sous l’égide de la transaction et sont une fonctionnalité prise en charge par la plupart des tous les systèmes de base de données modernes.

Dans ce didacticiel, nous allons examiner comment étendre la couche DAL pour utiliser les transactions de base de données. Les didacticiels suivants examinera l’implémentation de pages web pour le lot d’insertion, de la mise à jour et de suppression des interfaces. Laissez s commencer !

> [!NOTE]
> Lorsque vous modifiez des données dans une transaction par lot, atomicité n’est pas toujours nécessaire. Dans certains scénarios, il peut être acceptable d’avoir quelques modifications de données réussit et que d’autres dans le même lot échoue, par exemple quand la suppression d’un ensemble d’e-mails à partir d’un client de messagerie basée sur le web. Si un base de données erreur au milieu de la suppression de s il traite, il s probablement acceptable que ces enregistrements traités sans erreur restent supprimés. Dans ce cas, la couche DAL pas doit-elle être modifiée pour prendre en charge les transactions de base de données. Il existe d’autres lots opération scénarios, toutefois, où l’atomicité est vital. Lorsqu’un client met son fonds d’un compte bancaire vers un autre, les deux opérations doivent être effectuées : fonds doivent être déduits à partir du premier compte et ensuite ajoutés à la seconde. Bien que la banque ne peut pas peur la première étape réussit mais échouer de la deuxième étape, ses clients naturellement serait contraries. Je vous encourage à utiliser ce didacticiel et implémenter les améliorations apportées à la couche DAL pour prendre en charge les transactions de base de données, même si vous n’envisagez pas de leur utilisation dans le lot d’insertion, la mise à jour et suppression des interfaces que nous allons créer dans les didacticiels suivants de trois.


## <a name="an-overview-of-transactions"></a>Une vue d’ensemble de Transactions

La plupart des bases de données incluent la prise en charge de *transactions*, qui permettent à plusieurs commandes de base de données d’être regroupés en une seule unité logique de travail. Sont garanti que les commandes de base de données qui composent une transaction atomique, ce qui signifie que toutes les commandes échoue ou réussiront tous.

En règle générale, les transactions sont implémentées par le biais des instructions SQL au format suivant :

1. Indiquer le début d’une transaction.
2. Exécutez les instructions SQL qui composent la transaction.
3. S’il existe une erreur dans une des instructions à l’étape 2, restaurez la transaction.
4. Si toutes les instructions à l’étape 2 se termine sans erreur, validez la transaction.

Les instructions SQL utilisées pour créer, valider et restaurer la transaction peut être entrée manuellement lors de l’écriture de scripts SQL ou la création de procédures stockées ou par le biais par programme au moyen d’ADO.NET ou les classes dans le [ `System.Transactions` espace de noms](https://msdn.microsoft.com/library/system.transactions.aspx). Dans ce didacticiel, nous allons examiner uniquement la gestion des transactions à l’aide d’ADO.NET. Dans un futur didacticiel, nous allons examiner comment utiliser des procédures stockées dans la couche d’accès aux données, moment auquel nous allons explorer les instructions SQL pour la création, la restauration et la validation des transactions. En attendant, consultez [la gestion des Transactions dans les procédures stockées SQL Server](http://www.4guysfromrolla.com/webtech/080305-1.shtml) pour plus d’informations.

> [!NOTE]
> Le [ `TransactionScope` classe](https://msdn.microsoft.com/library/system.transactions.transactionscope.aspx) dans le `System.Transactions` espace de noms permet aux développeurs d’encapsuler par programmation une série d’instructions dans l’étendue d’une transaction et inclut la prise en charge pour les transactions complexes qui impliquent plusieurs sources, telles que les deux bases de données différentes ou même hétérogènes types de magasins de données, comme une base de données Microsoft SQL Server, une base de données Oracle et un service Web. Je ve décidé d’utiliser des transactions ADO.NET pour ce didacticiel à la place de la `TransactionScope` classe ADO.NET est plus spécifique pour les transactions de base de données et, dans de nombreux cas, étant donné que nécessite beaucoup moins de ressources. En outre, dans certains scénarios le `TransactionScope` classe utilise Microsoft Distributed Transaction Coordinator (MSDTC). Les problèmes de configuration, de mise en œuvre et de performances MSDTC environnante rend une rubrique plutôt spécialisée et avancée qui dépasse la portée de ces didacticiels.


Lorsque vous travaillez avec le fournisseur SqlClient dans ADO.NET, les transactions sont initiées via un appel à la [ `SqlConnection` classe](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnection.aspx) s [ `BeginTransaction` méthode](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnection.begintransaction.aspx), qui retourne un [ `SqlTransaction` objet](https://msdn.microsoft.com/library/system.data.sqlclient.sqltransaction.aspx). Les instructions de modification de données que la transaction de composition sont placés dans un `try...catch` bloc. Si une erreur se produit dans une instruction dans le `try` bloquer, l’exécution des transferts vers le `catch` bloc dans lequel la transaction peut être restaurée par le biais de la `SqlTransaction` objet s [ `Rollback` (méthode)](https://msdn.microsoft.com/library/system.data.sqlclient.sqltransaction.rollback.aspx). Si toutes les instructions terminent avec succès, un appel à la `SqlTransaction` objet s [ `Commit` méthode](https://msdn.microsoft.com/library/system.data.sqlclient.sqltransaction.commit.aspx) à la fin de la `try` bloc valide la transaction. L’extrait de code suivant illustre ce modèle. Consultez [maintenance de cohérence de base de données avec des Transactions](http://aspnet.4guysfromrolla.com/articles/072705-1.aspx) pour une syntaxe supplémentaire et des exemples d’utilisation de transactions avec ADO.NET.


[!code-csharp[Main](wrapping-database-modifications-within-a-transaction-cs/samples/sample1.cs)]

Par défaut, les TableAdapters dans un DataSet typé n’utilisez pas de transactions. Pour prendre en charge pour les transactions que nous avons besoin d’augmenter les classes TableAdapter pour inclure des méthodes supplémentaires qui utilisent le modèle ci-dessus pour effectuer une série d’instructions de modification de données dans l’étendue d’une transaction. À l’étape 2, nous verrons comment utiliser des classes partielles pour ajouter ces méthodes.

## <a name="step-1-creating-the-working-with-batched-data-web-pages"></a>Étape 1 : Création du travail avec des Pages Web de données par lot

Avant de commencer à Explorer la manière d’améliorer la couche DAL pour prendre en charge les transactions de base de données, laissez s tout d’abord prendre un moment pour créer des pages web ASP.NET que nous aurons besoin pour ce didacticiel et les trois qui suivent. Commencez par ajouter un nouveau dossier nommé `BatchData` , puis ajoutez les pages ASP.NET suivantes, en associant chaque page avec le `Site.master` page maître.

- `Default.aspx`
- `Transactions.aspx`
- `BatchUpdate.aspx`
- `BatchDelete.aspx`
- `BatchInsert.aspx`


![Ajouter les Pages ASP.NET pour les didacticiels de SqlDataSource](wrapping-database-modifications-within-a-transaction-cs/_static/image1.gif)

**Figure 1**: ajoutez les Pages ASP.NET pour les didacticiels de SqlDataSource


Comme avec les autres dossiers, `Default.aspx` utilisera le `SectionLevelTutorialListing.ascx` contrôle utilisateur pour répertorier les didacticiels dans sa section. Par conséquent, ajoutez ce contrôle utilisateur à `Default.aspx` en le faisant glisser à partir de l’Explorateur de solutions sur la page s en mode Création.


[![Ajouter le contrôle utilisateur de SectionLevelTutorialListing.ascx à Default.aspx](wrapping-database-modifications-within-a-transaction-cs/_static/image2.gif)](wrapping-database-modifications-within-a-transaction-cs/_static/image1.png)

**Figure 2**: ajouter la `SectionLevelTutorialListing.ascx` contrôle utilisateur à `Default.aspx` ([cliquez pour afficher l’image en taille réelle](wrapping-database-modifications-within-a-transaction-cs/_static/image2.png))


Enfin, ajoutez ces quatre pages sous forme d’entrées pour le `Web.sitemap` fichier. Plus précisément, ajoutez le balisage suivant après la personnalisation du plan du Site `<siteMapNode>`:


[!code-xml[Main](wrapping-database-modifications-within-a-transaction-cs/samples/sample2.xml)]

Après la mise à jour `Web.sitemap`, prenez un moment pour afficher le site Web de didacticiels via un navigateur. Le menu de gauche inclut désormais les éléments de travail avec les didacticiels de données par lot.


![Le plan de Site inclut maintenant des entrées pour l’utilisation des didacticiels de données par lot](wrapping-database-modifications-within-a-transaction-cs/_static/image3.gif)

**Figure 3**: le plan de Site inclut maintenant des entrées pour l’utilisation des didacticiels de données par lot


## <a name="step-2-updating-the-data-access-layer-to-support-database-transactions"></a>Étape 2 : Mise à jour de la couche d’accès aux données pour prendre en charge les Transactions de base de données

Comme expliqué dans le premier didacticiel [création d’une couche d’accès aux données](../introduction/creating-a-data-access-layer-cs.md), le DataSet typé dans notre DAL se compose de tables de données et les TableAdapters. Les tables de données contiennent des données pendant les TableAdapters fournissent les fonctionnalités pour lire les données à partir de la base de données dans les tables de données, mettre à jour la base de données avec les modifications apportées aux tables de données et ainsi de suite. Rappelez-vous que les TableAdapters fournissent deux modèles pour la mise à jour des données, j’ai appelé mise à jour par lots et Direct à la base de données. Avec le modèle de mise à jour par lots, le TableAdapter est passé d’un DataSet, DataTable ou collection d’objets DataRow. Ces données est énumérées et pour chaque inséré, modifié ou supprimé la ligne, le `InsertCommand`, `UpdateCommand`, ou `DeleteCommand` est exécutée. Avec le modèle Direct à la base de données, le TableAdapter est passé à la place des valeurs des colonnes nécessaires pour l’insertion, la mise à jour ou suppression d’un enregistrement unique. La méthode directe de base de données utilise ensuite ces valeurs dans le passé pour exécuter la commande appropriée `InsertCommand`, `UpdateCommand`, ou `DeleteCommand` instruction.

Quel que soit le modèle de mise à jour utilisé, les méthodes générées automatiquement et les TableAdapters n’utilisent pas de transactions. Par défaut, chaque insertion, mise à jour ou delete exécutée par le TableAdapter est traité comme une seule opération discrète. Par exemple, supposons que le modèle de Direct à la base de données est utilisé par du code dans la couche BLL à insérer des dix enregistrements dans la base de données. Ce code appelle le TableAdapter s `Insert` méthode dix fois. Si les cinq premiers insertions réussissent, mais celui sixième a causé une exception, les cinq premiers enregistrements insérés restent dans la base de données. De même, si le modèle de mise à jour par lots est utilisé pour effectuer des insertions, mises à jour et les suppressions vers inséré, lignes modifiées et supprimées dans un DataTable, si les premier plusieurs modifications a réussi, mais une version plus récente a rencontré une erreur, ces modifications antérieures qui terminée reste dans la base de données.

Dans certains scénarios, nous souhaitons garantir l’atomicité sur une série de modifications. Pour cela nous devons étendre manuellement le TableAdapter en ajoutant de nouvelles méthodes qui exécutent le `InsertCommand`, `UpdateCommand`, et `DeleteCommand` s dans le cadre d’une transaction. Dans [création d’une couche d’accès aux données](../introduction/creating-a-data-access-layer-cs.md) nous avons examiné à l’aide de [des classes partielles](http://en.wikipedia.org/wiki/Partial_type) pour étendre les fonctionnalités des tables dans le DataSet typé. Cette technique peut également être utilisée avec les TableAdapters.

Le DataSet typé `Northwind.xsd` se trouve dans le `App_Code` dossier s `DAL` sous-dossier. Créez un sous-dossier dans le `DAL` dossier nommé `TransactionSupport` et ajoutez un nouveau fichier de classe nommé `ProductsTableAdapter.TransactionSupport.cs` (voir Figure 4). Ce fichier contiendra l’implémentation partielle de la `ProductsTableAdapter` qui inclut des méthodes pour effectuer des modifications de données à l’aide d’une transaction.


![Ajouter un dossier nommé propriété TransactionSupport et un fichier de classe nommé ProductsTableAdapter.TransactionSupport.cs](wrapping-database-modifications-within-a-transaction-cs/_static/image4.gif)

**Figure 4**: ajouter un dossier nommé `TransactionSupport` et un fichier de classe nommé `ProductsTableAdapter.TransactionSupport.cs`


Entrez le code suivant dans le `ProductsTableAdapter.TransactionSupport.cs` fichier :


[!code-csharp[Main](wrapping-database-modifications-within-a-transaction-cs/samples/sample3.cs)]

Le `partial` mot clé dans la déclaration de classe indique au compilateur que les membres ajoutés au sein doivent être ajoutés à la `ProductsTableAdapter` classe dans le `NorthwindTableAdapters` espace de noms. Remarque la `using System.Data.SqlClient` instruction en haut du fichier. Étant donné que le TableAdapter a été configuré pour utiliser le fournisseur SqlClient, en interne qu’il utilise un [ `SqlDataAdapter` ](https://msdn.microsoft.com/library/system.data.sqlclient.sqldataadapter.aspx) objet pour émettre ses commandes dans la base de données. Par conséquent, nous devons utiliser la `SqlTransaction` classe pour commencer la transaction, puis de valider ou restaurer. Si vous utilisez un magasin de données autres que Microsoft SQL Server, vous devez utiliser le fournisseur approprié.

Ces méthodes fournissent les blocs de construction nécessaires au démarrage, restauration et valider une transaction. Ils sont marqués `public`, ce qui leur permet à utiliser à partir du `ProductsTableAdapter`, à partir d’une autre classe dans la couche DAL, ou à partir d’une autre couche dans l’architecture, tels que la couche BLL. `BeginTransaction` Ouvre le TableAdapter interne `SqlConnection` (si nécessaire), démarre la transaction et l’assigne à la `Transaction` propriété et joint la transaction à interne `SqlDataAdapter` s `SqlCommand` objets. `CommitTransaction` et `RollbackTransaction` appeler le `Transaction` objet s `Commit` et `Rollback` méthodes, respectivement, avant de fermer le texte interne `Connection` objet.

## <a name="step-3-adding-methods-to-update-and-delete-data-under-the-umbrella-of-a-transaction"></a>Étape 3 : Ajout de méthodes pour mettre à jour et supprimer des données dans le cadre d’une Transaction

Avec ces méthodes terminées, nous re prêt à ajouter des méthodes à `ProductsDataTable` ou la couche BLL qui effectuent une série de commandes dans le cadre d’une transaction. La méthode suivante utilise le modèle de mise à jour par lot pour mettre à jour un `ProductsDataTable` instance à l’aide d’une transaction. Il démarre une transaction en appelant le `BeginTransaction` (méthode), puis utilise un `try...catch` bloc pour émettre les instructions de modification de données. Si l’appel à la `Adapter` objet s `Update` méthode entraîne une exception, l’exécution est transférés vers le `catch` bloc dans lequel la transaction sera restaurée et l’exception levée à nouveau. N’oubliez pas que le `Update` méthode implémente le modèle de mise à jour par lots en énumérant les lignes de l’élément `ProductsDataTable` et effectuer le nécessaire `InsertCommand`, `UpdateCommand`, et `DeleteCommand` s. Si l’une de ces résultats de commandes dans une erreur, la transaction est restaurée, annulant les modifications précédentes apportées pendant la durée de vie de s de transaction. Doit le `Update` instruction se terminer sans erreur, la transaction est validée dans son intégralité.


[!code-csharp[Main](wrapping-database-modifications-within-a-transaction-cs/samples/sample4.cs)]

Ajouter le `UpdateWithTransaction` méthode à la `ProductsTableAdapter` classe via la classe partielle dans `ProductsTableAdapter.TransactionSupport.cs`. Sinon, cette méthode peut être ajoutée à la couche de logique métier s `ProductsBLL` classe avec quelques modifications syntaxiques mineures. À savoir, le mot clé dans `this.BeginTransaction()`, `this.CommitTransaction()`, et `this.RollbackTransaction()` devra être remplacé par `Adapter` (n’oubliez pas que `Adapter` est le nom d’une propriété dans `ProductsBLL` de type `ProductsTableAdapter`).

Le `UpdateWithTransaction` méthode utilise le modèle de mise à jour par lots, mais une série d’appels de Direct à la base de données peut également servir dans l’étendue d’une transaction, comme dans l’exemple de méthode suivant. Le `DeleteProductsWithTransaction` méthode accepte comme entrée un `List<T>` de type `int`, qui sont le `ProductID` s à supprimer. La méthode lance la transaction via un appel à `BeginTransaction` puis, dans le `try` bloc, effectue une itération dans la liste fournie le modèle Direct à la base de données de l’appel `Delete` méthode pour chaque `ProductID` valeur. Si un des appels à `Delete` échoue, le contrôle est transféré à la `catch` bloc dans lequel la transaction est annulée et l’exception levée à nouveau. Si tous les appels à `Delete` réussisse, puis de la transaction est validée. Ajoutez la méthode suivante à la `ProductsBLL` classe.


[!code-csharp[Main](wrapping-database-modifications-within-a-transaction-cs/samples/sample5.cs)]

## <a name="applying-transactions-across-multiple-tableadapters"></a>Application des Transactions entre plusieurs TableAdapters

Le code liées aux transactions examiné dans ce didacticiel permet pour plusieurs instructions contre le `ProductsTableAdapter` est traitée comme une opération atomique. Mais que se passe-t-il si plusieurs modifications aux tables de base de données différente être exécutée de façon atomique ? Par exemple, lorsque vous supprimez une catégorie, nous pouvons tout d’abord voulez réaffecter ses produits en cours à une autre catégorie. Ces deux étapes en réaffectant les produits et de suppression de la catégorie doivent être exécutés comme une opération atomique. Mais le `ProductsTableAdapter` inclut uniquement les méthodes de modification le `Products` table et le `CategoriesTableAdapter` inclut uniquement les méthodes de modification le `Categories` table. Par conséquent, comment une transaction peut englober les TableAdapters ?

Une option consiste à ajouter une méthode à la `CategoriesTableAdapter` nommé `DeleteCategoryAndReassignProducts(categoryIDtoDelete, reassignToCategoryID)` et avoir cette méthode à appeler une procédure stockée qui réaffecte les produits et supprime la catégorie dans l’étendue d’une transaction définie dans la procédure stockée. Nous allons examiner comment commencer, valider et annuler les transactions dans les procédures stockées dans un futur didacticiel.

Une autre option consiste à créer une classe d’assistance dans la couche DAL qui contient le `DeleteCategoryAndReassignProducts(categoryIDtoDelete, reassignToCategoryID)` (méthode). Cette méthode créerait une instance de la `CategoriesTableAdapter` et `ProductsTableAdapter` puis définissez ces deux TableAdapters `Connection` propriétés au même `SqlConnection` instance. À ce stade, l’une des deux TableAdapters initiait la transaction avec un appel à `BeginTransaction`. Les méthodes de TableAdapters pour réaffecter les produits et la suppression de la catégorie peut être appelés dans un `try...catch` bloc avec la transaction validée ou annulée précédent en fonction des besoins.

## <a name="step-4-adding-theupdatewithtransactionmethod-to-the-business-logic-layer"></a>Étape 4 : Ajout de la`UpdateWithTransaction`à la couche de logique métier (méthode)

À l’étape 3, nous avons ajouté un `UpdateWithTransaction` méthode à la `ProductsTableAdapter` dans la couche DAL. Nous devons ajouter une méthode correspondante à la couche BLL. Alors que la couche de présentation peut appeler directement jusqu'à la couche DAL pour appeler le `UpdateWithTransaction` (méthode), ces didacticiels entièrement définir une architecture en couches qui isole la couche DAL à partir de la couche de présentation. Par conséquent, il appartient nous pour continuer cette approche.

Ouvrez le `ProductsBLL` fichier de classe et ajoutez une méthode nommée `UpdateWithTransaction` qui appelle simplement à la méthode correspondante de la couche DAL. Doit être maintenant deux nouvelles méthodes dans `ProductsBLL`: `UpdateWithTransaction`, ce qui vous venez d’ajouter, et `DeleteProductsWithTransaction`, qui a été ajouté à l’étape 3.


[!code-csharp[Main](wrapping-database-modifications-within-a-transaction-cs/samples/sample6.cs)]

> [!NOTE]
> Ces méthodes n’incluent pas le `DataObjectMethodAttribute` attribut assigné à la plupart des autres méthodes dans la `ProductsBLL` classe, car nous allons appeler ces méthodes directement à partir des classes de code-behind de pages ASP.NET. N’oubliez pas que `DataObjectMethodAttribute` est utilisé pour indiquer quelles méthodes doivent apparaître dans le s ObjectDataSource configurer la Source de données Assistant et sous l’onglet (SELECT, UPDATE, INSERT ou DELETE). Dans la mesure où le contrôle GridView ne dispose pas en charge intégrée pour le lot de modification ou la suppression, nous devrons appeler ces méthodes par programme plutôt que d’utiliser l’approche sans code déclaratif.


## <a name="step-5-atomically-updating-database-data-from-the-presentation-layer"></a>Étape 5 : Atomiquement la mise à jour des données de base de données à partir de la couche de présentation

Pour illustrer l’effet que la transaction a lors de la mise à jour d’un lot d’enregistrements, s permettent de créer une interface utilisateur qui répertorie tous les produits dans un GridView et inclut un site Web de bouton de contrôle qui, lorsque vous cliquez dessus, réaffecte les produits `CategoryID` valeurs. En particulier, de progression de la réaffectation de la catégorie afin que la première série de produits est affectés valide `CategoryID` valeur alors que d’autres sont délibérément attribué un inexistante `CategoryID` valeur. Si nous tentons de mettre à jour de la base de données avec un produit dont `CategoryID` ne correspond pas à une catégorie existante s `CategoryID`, une violation de contrainte de clé étrangère se produira et une exception sera levée. Ce que nous verrons dans cet exemple est que, lorsque vous à l’aide d’une transaction, l’exception levée à partir de la violation de contrainte de clé étrangère entraîne la précédente valide `CategoryID` modifications être annulées. Lorsque vous N'utilisez pas une transaction, toutefois, les modifications aux catégories initiales restera.

Commencez par ouvrir le `Transactions.aspx` page dans le `BatchData` dossier et faites glisser un GridView à partir de la boîte à outils vers le concepteur. Définissez ses `ID` à `Products` et, à partir de sa balise active, liez-le à une nouvelle ObjectDataSource nommé `ProductsDataSource`. Configurer l’ObjectDataSource afin d’extraire ses données à partir de la `ProductsBLL` classe s `GetProducts` (méthode). Cela sera être un GridView en lecture seule, donc définir les listes déroulantes dans la mise à jour, insertion et supprimer des onglets à (None), cliquez sur Terminer.


[![Figure 5 : Configurer ObjectDataSource pour utiliser la méthode de GetProducts ProductsBLL classe s](wrapping-database-modifications-within-a-transaction-cs/_static/image5.gif)](wrapping-database-modifications-within-a-transaction-cs/_static/image3.png)

**Figure 5**: Figure 5 : configurer ObjectDataSource à utiliser le `ProductsBLL` classe s `GetProducts` (méthode) ([cliquez pour afficher l’image en taille réelle](wrapping-database-modifications-within-a-transaction-cs/_static/image4.png))


[![Définir les listes déroulantes dans la mise à jour, insertion et supprimer des onglets à (None)](wrapping-database-modifications-within-a-transaction-cs/_static/image6.gif)](wrapping-database-modifications-within-a-transaction-cs/_static/image5.png)

**Figure 6**: la valeur est la liste déroulante répertorie dans la mise à jour, insertion et supprimer des onglets (aucun) ([cliquez pour afficher l’image en taille réelle](wrapping-database-modifications-within-a-transaction-cs/_static/image6.png))


À l’issue de l’Assistant Configurer la Source de données, Visual Studio créera BoundFields et un CheckBoxField pour les champs de données de produit. Supprimez tous ces champs à l’exception de `ProductID`, `ProductName`, `CategoryID`, et `CategoryName` et renommer le `ProductName` et `CategoryName` BoundFields `HeaderText` propriétés pour le produit et la catégorie, respectivement. À partir de la balise active, activez l’option Activer la pagination. Après avoir apporté ces modifications, le balisage déclaratif s GridView et ObjectDataSource doit ressembler à ce qui suit :


[!code-aspx[Main](wrapping-database-modifications-within-a-transaction-cs/samples/sample7.aspx)]

Ensuite, ajoutez trois contrôles de bouton Web ci-dessus le GridView. La valeur est le premier bouton s propriété Text actualiser la grille, le deuxième s à modifier les catégories (avec TRANSACTION) et le troisième s afin de modifier les catégories (sans TRANSACTION).


[!code-aspx[Main](wrapping-database-modifications-within-a-transaction-cs/samples/sample8.aspx)]

À ce stade le mode de conception dans Visual Studio doit ressembler à l’écran illustré à la Figure 7.


[![La Page contient un GridView et trois contrôles Web](wrapping-database-modifications-within-a-transaction-cs/_static/image7.gif)](wrapping-database-modifications-within-a-transaction-cs/_static/image7.png)

**Figure 7**: la Page contient un GridView et les trois contrôles Web ([cliquez pour afficher l’image en taille réelle](wrapping-database-modifications-within-a-transaction-cs/_static/image8.png))


Créer des gestionnaires d’événements pour chaque du bouton trois s `Click` événements et utilisez le code suivant :


[!code-csharp[Main](wrapping-database-modifications-within-a-transaction-cs/samples/sample9.cs)]

L’actualisation bouton s `Click` Gestionnaire d’événements relie simplement les données au GridView en appelant le `Products` GridView s `DataBind` (méthode).

Le second gestionnaire d’événements réaffecte les produits `CategoryID` s et utilise la nouvelle méthode de transaction à partir de la couche BLL pour effectuer la base de données met à jour dans le cadre d’une transaction. Notez que chaque produit s `CategoryID` est arbitrairement définie sur la même valeur que sa `ProductID`. Cela fonctionne correctement pour la première peu de produits, étant donné que ces produits ont `ProductID` les valeurs qui se produisent pour mapper à valide `CategoryID` s. Mais une fois la `ProductID` début s devienne trop importante, ce chevauchement une coïncidence de `ProductID` s et `CategoryID` s ne s’applique plus.

La troisième `Click` Gestionnaire d’événements met à jour les produits `CategoryID` s de la même manière, envoie, mais la mise à jour à la base de données en utilisant le `ProductsTableAdapter` par défaut s `Update` (méthode). Cela `Update` n’encapsule pas la série de commandes dans une transaction de méthode, donc ces modifications sont apportées avant la première erreur de violation de contrainte de clé étrangère rencontrée persisteront.

Pour illustrer ce comportement, visitez cette page via un navigateur. Initialement, vous devez voir la première page de données comme indiqué dans la Figure 8. Ensuite, cliquez sur le bouton Modifier les catégories (avec TRANSACTION). Cela provoque une publication (postback) et tentez de mettre à jour tous les produits `CategoryID` valeurs, mais entraîne une violation de contrainte de clé étrangère (voir Figure 9).


[![Les produits sont affichés dans un GridView paginable](wrapping-database-modifications-within-a-transaction-cs/_static/image8.gif)](wrapping-database-modifications-within-a-transaction-cs/_static/image9.png)

**Figure 8**: les produits sont affichés dans un GridView paginable ([cliquez pour afficher l’image en taille réelle](wrapping-database-modifications-within-a-transaction-cs/_static/image10.png))


[![En réaffectant les résultats de catégories une violation de contrainte de clé étrangère](wrapping-database-modifications-within-a-transaction-cs/_static/image9.gif)](wrapping-database-modifications-within-a-transaction-cs/_static/image11.png)

**Figure 9**: en réaffectant les résultats de catégories dans une Violation de contrainte de clé étrangère ([cliquez pour afficher l’image en taille réelle](wrapping-database-modifications-within-a-transaction-cs/_static/image12.png))


Appuyez à présent sur le bouton de précédent de votre navigateur s, puis sur le bouton Actualiser la grille. Lors de l’actualisation des données, vous devez voir le même résultat exact comme indiqué dans la Figure 8. Autrement dit, même si certains produits `CategoryID` s étaient des valeurs modifiées Legal et mis à jour dans la base de données, ils ont été annulées lors de la violation de contrainte de clé étrangère s’est produite.

Essayez maintenant en cliquant sur le bouton Modifier les catégories (sans TRANSACTION). Ainsi, la même erreur de violation de contrainte de clé étrangère (voir Figure 9), mais cette fois, les produits dont `CategoryID` valeurs ont été remplacées par un juridiques valeur ne sera pas restaurée. Appuyez sur le bouton de précédent de votre navigateur s, puis sur le bouton Actualiser la grille. Comme le montre la Figure 10, le `CategoryID` s des huit premiers produits ont été réaffectée. Par exemple, dans la Figure 8, suivi modifications avaient un `CategoryID` 1, mais dans la Figure 10 informatique s été réaffectée à 2.


[![Certaines valeurs de CategoryID produits ont été mis à jour alors que d’autres ont été pas](wrapping-database-modifications-within-a-transaction-cs/_static/image10.gif)](wrapping-database-modifications-within-a-transaction-cs/_static/image13.png)

**Figure 10**: certains produits `CategoryID` valeurs ont été mis à jour alors que d’autres ont été pas ([cliquez pour afficher l’image en taille réelle](wrapping-database-modifications-within-a-transaction-cs/_static/image14.png))


## <a name="summary"></a>Récapitulatif

Par défaut, les méthodes de s TableAdapter n’imbriquez pas les instructions de la base de données exécutée dans l’étendue d’une transaction, mais avec un peu de travail, nous pouvons ajouter les méthodes qui créeront, commit et rollback une transaction. Dans ce didacticiel, nous avons créé trois de ces méthodes dans le `ProductsTableAdapter` classe : `BeginTransaction`, `CommitTransaction`, et `RollbackTransaction`. Nous avons vu comment utiliser ces méthodes avec un `try...catch` bloc pour effectuer une série d’instructions de modification de données atomiques. En particulier, nous avons créé le `UpdateWithTransaction` méthode dans le `ProductsTableAdapter`, qui utilise le modèle de mise à jour par lots pour effectuer les modifications nécessaires aux lignes d’un fourni `ProductsDataTable`. Nous avons également ajouté la `DeleteProductsWithTransaction` méthode à la `ProductsBLL` classe dans la couche BLL, qui accepte un `List` de `ProductID` valeurs comme entrée et appelle la méthode de modèle DB directs `Delete` pour chaque `ProductID`. Les deux méthodes commencer par la création d’une transaction, puis en exécutant les instructions de modification de données dans un `try...catch` bloc. Si une exception se produit, la transaction est annulée, sinon elle est validée.

Étape 5 illustre l’effet des mises à jour d’un lot transactionnel et mises à jour de batch qui négligèrent à utiliser une transaction. Dans les trois didacticiels, nous s’appuient sur la base de ce didacticiel et créer des interfaces utilisateur pour effectuer des mises à jour par lots, les suppressions et insertions.

Bonne programmation !

## <a name="further-reading"></a>informations supplémentaires

Pour plus d’informations sur les sujets abordés dans ce didacticiel, consultez les ressources suivantes :

- [Maintien de la cohérence de base de données avec des Transactions](http://aspnet.4guysfromrolla.com/articles/072705-1.aspx)
- [Procédures stockées de gestion des Transactions dans SQL Server](http://www.4guysfromrolla.com/webtech/080305-1.shtml)
- [Transactions effectuées facile : `System.Transactions`](https://blogs.msdn.com/florinlazar/archive/2004/07/23/192239.aspx)
- [TransactionScope et DataAdapters](http://andyclymer.blogspot.com/2007/01/transactionscope-and-dataadapters.html)
- [À l’aide de Transactions de bases de données Oracle dans .NET](http://www.oracle.com/technology/pub/articles/price_dbtrans_dotnet.html)

## <a name="about-the-author"></a>À propos de l’auteur

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), auteur de sept les livres sur ASP/ASP.NET et fondateur de [4GuysFromRolla.com](http://www.4guysfromrolla.com), travaille avec les technologies Web Microsoft depuis 1998. Scott fonctionne comme un consultant indépendant, formateur et writer. Son dernier ouvrage est [*SAM animer vous-même ASP.NET 2.0 des dernières 24 heures*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Il peut être contacté à [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou via son blog, qui se trouve à [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Remerciements

Cette série de didacticiels a été révisée par plusieurs réviseurs utiles. Les réviseurs tête pour ce didacticiel ont été Dave Gardner, Hilton Giesenow et Teresa Murphy. Qui souhaitent consulter mes prochains articles MSDN ? Dans ce cas, envoyez-moi une ligne à [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Next](batch-updating-cs.md)
