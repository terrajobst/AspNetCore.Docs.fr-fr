---
uid: web-forms/overview/data-access/editing-inserting-and-deleting-data/implementing-optimistic-concurrency-vb
title: Implémentation de l’accès concurrentiel optimiste (VB) | Microsoft Docs
author: rick-anderson
description: Pour une application web qui permet à plusieurs utilisateurs de modifier des données, il existe un risque que deux utilisateurs peuvent modifier les mêmes données en même temps. Dans cette tutori...
ms.author: riande
ms.date: 07/17/2006
ms.assetid: 2646968c-2826-4418-b1d0-62610ed177e3
msc.legacyurl: /web-forms/overview/data-access/editing-inserting-and-deleting-data/implementing-optimistic-concurrency-vb
msc.type: authoredcontent
ms.openlocfilehash: e33e4b401d957f4aa5560193dd8af0e53ca3b631
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41833457"
---
<a name="implementing-optimistic-concurrency-vb"></a>Implémentation de l’accès concurrentiel optimiste (VB)
====================
par [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Télécharger l’exemple d’application](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_21_VB.exe) ou [télécharger le PDF](implementing-optimistic-concurrency-vb/_static/datatutorial21vb1.pdf)

> Pour une application web qui permet à plusieurs utilisateurs de modifier des données, il existe un risque que deux utilisateurs peuvent modifier les mêmes données en même temps. Dans ce didacticiel, nous allons implémenter le contrôle de concurrence optimiste pour gérer ce risque.


## <a name="introduction"></a>Introduction

Pour les applications web qui autorisent uniquement aux utilisateurs d’afficher des données, ou pour ceux qui contiennent un seul utilisateur qui peut modifier des données, il n’existe aucune menace de deux utilisateurs simultanés écrasent les modifications par un autre. Pour les applications web qui autorise plusieurs utilisateurs à mettre à jour ou supprimer des données, toutefois, il existe un risque pour les modifications d’un utilisateur à entrer en conflit avec un autre utilisateur simultané. Aucune stratégie d’accès concurrentiel en place, lorsque deux utilisateurs modifient simultanément un enregistrement unique, l’utilisateur qui valide ses modifications dernière remplace les modifications apportées par le premier.

Par exemple, imaginez que deux utilisateurs, Jisun et Sam, ont été les deux accédant à une page dans l’application, aux visiteurs de mettre à jour et supprimer les produits à travers un contrôle GridView. Cliquez sur le bouton Modifier dans le contrôle GridView en même temps. Jisun remplace le nom de produit « Thé Chai » et clique sur le bouton de mise à jour. Le résultat net est une `UPDATE` instruction est envoyée à la base de données définit *tous les* des champs modifiables du produit (même si Jisun mise à jour uniquement un champ, `ProductName`). À ce stade, la base de données a les valeurs « Chai thé », la catégorie boissons, le fournisseur liquides exotiques, et ainsi de suite de ce produit particulier. Toutefois, le contrôle GridView à l’écran de Sam affiche toujours le nom du produit dans la ligne GridView modifiable en tant que « Martin ». Quelques secondes après que les modifications de Jisun ont été validées, Sam met à jour de la catégorie à Condiments et clique sur mise à jour. Il en résulte un `UPDATE` instruction envoyée à la base de données qui définit le nom de produit à « Martin », le `CategoryID` à l’ID de la catégorie boissons correspondant et ainsi de suite. Modifications du Jisun au nom du produit ont été remplacées. La figure 1 présente graphiquement cette série d’événements.


[![Lorsque deux utilisateurs mettre à jour simultanément un potentiel d’enregistrement s il y a des modifications d’un utilisateur de remplacer l’autre](implementing-optimistic-concurrency-vb/_static/image2.png)](implementing-optimistic-concurrency-vb/_static/image1.png)

**Figure 1**: lorsque deux utilisateurs simultanément mettre à jour un enregistrement il s potentiel de passe d’un utilisateur à remplacer l’autre ([cliquez pour afficher l’image en taille réelle](implementing-optimistic-concurrency-vb/_static/image3.png))


De même, lorsque deux utilisateurs visitent une page, un utilisateur peut être au milieu de la mise à jour un enregistrement lorsqu’il est supprimé par un autre utilisateur. Ou bien, entre quand un utilisateur charge une page et lorsqu’ils cliquent sur le bouton Supprimer, un autre utilisateur a modifié le contenu de cet enregistrement.

Il existe trois [contrôle d’accès concurrentiel](http://en.wikipedia.org/wiki/Concurrency_control) stratégies disponibles :

- **Ne rien faire** -si vous modifiant le même enregistrement, d’utilisateurs simultanés permettent la dernière validation win (comportement par défaut)
- [**L’accès concurrentiel optimiste** ](http://en.wikipedia.org/wiki/Optimistic_concurrency_control) -supposent que pouvant disposer de l’accès concurrentiel est en conflit de maintenant, puis, la grande majorité du temps ces conflits ne surviennent ; par conséquent, si un conflit se produit, simplement informer l’utilisateur que leurs modifications ne peut pas être enregistré, car un autre utilisateur a modifié les mêmes données
- **Accès concurrentiel pessimiste** -supposent que les conflits d’accès concurrentiel sont très courants et que les utilisateurs ne sont pas tolérer étant dit que leurs modifications n’ont pas été enregistrées en raison de l’activité simultanée d’un autre utilisateur ; par conséquent, lorsqu’un utilisateur commence la mise à jour un enregistrement, la verrouiller , ce qui empêche tous les autres utilisateurs de modifier ou supprimer cet enregistrement jusqu'à ce que l’utilisateur valide les modifications

Tous les nos didacticiels jusqu'à présent ont utilisé la stratégie de résolution par défaut d’accès concurrentiel : à savoir, nous avons permettent la dernière écriture de gagner. Dans ce didacticiel, nous allons examiner comment implémenter le contrôle d’accès concurrentiel optimiste.

> [!NOTE]
> Nous n’examinerons des exemples d’accès concurrentiel pessimiste dans cette série de didacticiels. Accès concurrentiel pessimiste est rarement utilisée parce que ces verrous, si ce n’est pas correctement libérées, peut empêcher les autres utilisateurs de mettre à jour des données. Par exemple, si un utilisateur verrouille un enregistrement pour la modification, puis quitte pour le jour précédant le déverrouillage, aucun autre utilisateur ne sera en mesure de mettre à jour cet enregistrement jusqu'à ce que l’utilisateur d’origine retourne et termine sa mise à jour. Par conséquent, dans les cas où l’accès concurrentiel pessimiste est utilisé, il est généralement un délai d’attente, si atteinte, annule le verrou. Ticket ventes sites Web, ce qui le bloquer un emplacement particulier assise pour une courte période pendant que l’utilisateur termine le processus de commande, sont un exemple de contrôle d’accès concurrentiel pessimiste.


## <a name="step-1-looking-at-how-optimistic-concurrency-is-implemented"></a>Étape 1 : Examiner la façon dont l’accès concurrentiel optimiste est implémenté.

Contrôle d’accès concurrentiel optimiste fonctionne en veillant à ce que l’enregistrement en cours de mise à jour ou supprimé a les mêmes valeurs, comme c’était le cas au démarrage de la mise à jour ou la suppression des processus. Par exemple, lorsque vous cliquez sur le bouton Modifier dans un GridView modifiable, les valeurs de l’enregistrement sont lire à partir de la base de données et affichées dans les zones de texte et d’autres contrôles Web. Ces valeurs d’origine sont enregistrés par le contrôle GridView. Plus tard, une fois que l’utilisateur effectue ses modifications et clique sur le bouton de mise à jour, les valeurs d’origine ainsi que les nouvelles valeurs sont envoyées à la couche de logique métier, puis à la couche d’accès aux données. La couche d’accès aux données doit émettre une instruction SQL qui met à jour uniquement l’enregistrement si les valeurs d’origine que l’utilisateur a commencé à modifier sont identiques aux valeurs toujours dans la base de données. La figure 2 illustre cette séquence d’événements.


[![Pour la mise à jour ou la suppression réussisse, les valeurs d’origine doivent être égales pour les valeurs actuelles de la base de données](implementing-optimistic-concurrency-vb/_static/image5.png)](implementing-optimistic-concurrency-vb/_static/image4.png)

**Figure 2**: pour la mise à jour ou supprimer pour réussir, le d’origine valeurs doit être égale aux valeurs de base de données ([cliquez pour afficher l’image en taille réelle](implementing-optimistic-concurrency-vb/_static/image6.png))


Il existe différentes approches d’implémentation de l’accès concurrentiel optimiste (consultez [Peter A. Bromberg](http://peterbromberg.net/)de [logique de la mise à jour d’accès concurrentiel Optmistic](http://www.eggheadcafe.com/articles/20050719.asp) pour un bref aperçu présentant un nombre d’options). Le jeu de données typés ADO.NET fournit une implémentation qui peut être configurée avec simplement le cycle d’une case à cocher. L’activation de l’accès concurrentiel optimiste pour un TableAdapter dans le DataSet typé augmente le TableAdapter `UPDATE` et `DELETE` instructions pour inclure une comparaison de toutes les valeurs d’origine dans le `WHERE` clause. Ce qui suit `UPDATE` instruction, par exemple, des mises à jour le nom et le prix d’un produit uniquement si les valeurs actuelles de la base de données sont égales aux valeurs qui ont été récupérées à l’origine lors de la mise à jour l’enregistrement dans le contrôle GridView. Le `@ProductName` et `@UnitPrice` paramètres contiennent les nouvelles valeurs entrées par l’utilisateur, tandis que `@original_ProductName` et `@original_UnitPrice` contiennent les valeurs qui ont été chargées à l’origine dans le contrôle GridView lorsque l’utilisateur a cliqué sur le bouton Modifier :


[!code-sql[Main](implementing-optimistic-concurrency-vb/samples/sample1.sql)]

> [!NOTE]
> Cela `UPDATE` instruction a été simplifiée pour une meilleure lisibilité. Dans la pratique, le `UnitPrice` archiver le `WHERE` clause serait plus complexe depuis `UnitPrice` peut contenir `NULL` s et lors de la vérification `NULL = NULL` retourne toujours False (au lieu de cela, vous devez utiliser `IS NULL`).


En plus d’utiliser un autre sous-jacent `UPDATE` instruction, configurez un TableAdapter à utiliser optimiste concurrentiel modifie également la signature de sa base de données des méthodes directes. Nous avons vu dans notre premier didacticiel, [ *création d’une couche d’accès aux données*](../introduction/creating-a-data-access-layer-cs.md), que les méthodes directes DB étaient ceux qui accepte une liste du scalaire de valeurs comme paramètres d’entrée (au lieu d’un DataRow fortement typée ou Instance de DataTable). Lors de l’utilisation de l’accès concurrentiel optimiste, la base de données direct `Update()` et `Delete()` méthodes incluent des paramètres d’entrée pour les valeurs d’origine ainsi. En outre, le code de la couche BLL pour le lot à l’aide de la mettre à jour le modèle (le `Update()` des surcharges de méthode qui acceptent les DataRows et tables de données plutôt que des valeurs scalaires) doit également être modifiés.

Plutôt que d’étendre notre existant les TableAdapters de couche DAL pour utiliser l’accès concurrentiel optimiste (ce qui nécessiterait la modification de la couche BLL pour prendre en charge), nous allons à la place de créer un nouveau DataSet typé nommé `NorthwindOptimisticConcurrency`, à laquelle nous allons ajouter un `Products` TableAdapter qui utilise l’accès concurrentiel optimiste. Ensuite, nous allons créer un `ProductsOptimisticConcurrencyBLL` classe de couche de logique métier qui a les modifications appropriées pour prendre en charge de la couche DAL l’accès concurrentiel optimiste. Une fois que ce point de départ a été défini, nous serons prêts à créer la page ASP.NET.

## <a name="step-2-creating-a-data-access-layer-that-supports-optimistic-concurrency"></a>Étape 2 : Création d’une couche d’accès aux données qui prend en charge l’accès concurrentiel optimiste

Pour créer un nouveau DataSet typé, cliquez sur le `DAL` dossier au sein de la `App_Code` dossier et ajouter un nouveau DataSet nommé `NorthwindOptimisticConcurrency`. Comme nous l’avons vu dans le premier didacticiel, cela donc ajoutera un nouveau TableAdapter au DataSet typé, lancement automatique de l’Assistant Configuration de TableAdapter. Dans le premier écran, nous allons vous y êtes invités à spécifier la base de données pour se connecter à : se connecter à la même base de données Northwind à l’aide du `NORTHWNDConnectionString` définir à partir de `Web.config`.


[![Se connecter à la même base de données Northwind](implementing-optimistic-concurrency-vb/_static/image8.png)](implementing-optimistic-concurrency-vb/_static/image7.png)

**Figure 3**: se connecter à la même base de données Northwind ([cliquez pour afficher l’image en taille réelle](implementing-optimistic-concurrency-vb/_static/image9.png))


Ensuite, nous allons vous y êtes invités dans la procédure interroger les données : via une instruction SQL ad hoc, une nouvelle procédure stockée ou un existant de la procédure stockée. Étant donné que nous avons utilisé des requêtes SQL ad hoc dans notre DAL d’origine, utilisez cette option ici également.


[![Spécifier les données à récupérer à l’aide d’une instruction SQL de Ad-Hoc](implementing-optimistic-concurrency-vb/_static/image11.png)](implementing-optimistic-concurrency-vb/_static/image10.png)

**Figure 4**: spécifier les données à récupérer à l’aide d’une instruction SQL de Ad-Hoc ([cliquez pour afficher l’image en taille réelle](implementing-optimistic-concurrency-vb/_static/image12.png))


Dans l’écran suivant, entrez la requête SQL à utiliser pour récupérer les informations de produit. Nous allons utiliser la même requête SQL exacte utilisée pour le `Products` TableAdapter à partir de notre DAL d’origine, qui retourne tous les `Product` colonnes ainsi que les noms de fournisseur et la catégorie du produit :


[!code-sql[Main](implementing-optimistic-concurrency-vb/samples/sample2.sql)]


[![Utiliser la même requête SQL à partir du TableAdapter de produits dans la couche DAL d’origine](implementing-optimistic-concurrency-vb/_static/image14.png)](implementing-optimistic-concurrency-vb/_static/image13.png)

**Figure 5**: utiliser la même requête SQL à partir de la `Products` TableAdapter dans la couche DAL d’origine ([cliquez pour afficher l’image en taille réelle](implementing-optimistic-concurrency-vb/_static/image15.png))


Avant de passer à l’écran suivant, cliquez sur le bouton Options avancées. Pour que ce contrôle de l’accès concurrentiel optimiste emploient TableAdapter, simplement cocher la case à cocher « Utiliser l’accès concurrentiel optimiste ».


[![Activer le contrôle d’accès concurrentiel optimiste par chèques le &quot;accès concurrentiel optimiste&quot; case à cocher](implementing-optimistic-concurrency-vb/_static/image17.png)](implementing-optimistic-concurrency-vb/_static/image16.png)

**Figure 6**: activer le contrôle d’accès concurrentiel optimiste en cochant la case « Utiliser l’accès concurrentiel optimiste » ([cliquez pour afficher l’image en taille réelle](implementing-optimistic-concurrency-vb/_static/image18.png))


Enfin, indiquer le TableAdapter d’utiliser les modèles d’accès aux données qui les remplir un DataTable et retournent un DataTable ; Indiquez également que les méthodes directes de base de données doivent être créés. Modifier le nom de méthode pour la valeur de retour un modèle de DataTable à partir de GetData à GetProducts, afin de refléter les conventions d’affectation de noms, que nous avons utilisé dans notre DAL d’origine.


[![Avoir le TableAdapter à utiliser tous les modèles d’accès aux données](implementing-optimistic-concurrency-vb/_static/image20.png)](implementing-optimistic-concurrency-vb/_static/image19.png)

**Figure 7**: ont le TableAdapter utilisent tous les modèles accès aux données ([cliquez pour afficher l’image en taille réelle](implementing-optimistic-concurrency-vb/_static/image21.png))


À l’issue de l’Assistant, le Concepteur de DataSet inclut une fortement typée `Products` DataTable et TableAdapter. Prenez un moment pour renommer la table de données à partir de `Products` à `ProductsOptimisticConcurrency`, ce que vous pouvez faire en cliquant sur la barre de titre de la table de données et en sélectionnant Renommer dans le menu contextuel.


[![Une table de données et d’un TableAdapter ont été ajoutés au DataSet typé](implementing-optimistic-concurrency-vb/_static/image23.png)](implementing-optimistic-concurrency-vb/_static/image22.png)

**Figure 8**: un DataTable et TableAdapter ont été ajoutés au DataSet typé ([cliquez pour afficher l’image en taille réelle](implementing-optimistic-concurrency-vb/_static/image24.png))


Pour voir les différences entre le `UPDATE` et `DELETE` des requêtes entre les `ProductsOptimisticConcurrency` TableAdapter (qui utilise l’accès concurrentiel optimiste) et Products TableAdapter (qui ne), cliquez sur le TableAdapter et accédez à la fenêtre Propriétés. Dans le `DeleteCommand` et `UpdateCommand` propriétés `CommandText` sous-propriétés, vous pouvez voir la syntaxe SQL réelle qui est envoyée à la base de données lors de la mise à jour ou les méthodes de suppression de la couche DAL sont appelées. Pour le `ProductsOptimisticConcurrency` TableAdapter la `DELETE` instruction utilisée est :


[!code-sql[Main](implementing-optimistic-concurrency-vb/samples/sample3.sql)]

Tandis que le `DELETE` instruction du TableAdapter de produit dans notre DAL d’origine est la plus simple :


[!code-sql[Main](implementing-optimistic-concurrency-vb/samples/sample4.sql)]

Comme vous pouvez le voir, le `WHERE` clause dans la `DELETE` instruction du TableAdapter qui utilise l’accès concurrentiel optimiste inclut une comparaison entre chacun de la `Product` table existantes de valeurs de colonne et les valeurs d’origine au moment où le Dernier remplissage GridView (ou DetailsView ou FormView). Depuis tous les champs autres que `ProductID`, `ProductName`, et `Discontinued` peut avoir `NULL` valeurs, les paramètres supplémentaires et les vérifications sont incluses pour comparer correctement `NULL` des valeurs dans le `WHERE` clause.

Nous ne sont pas ajouter les tables de données supplémentaires pour le DataSet d’accès concurrentiel optimiste pour ce didacticiel, comme notre page ASP.NET fournit uniquement la mise à jour et suppression des informations sur le produit. Toutefois, nous devons toujours ajouter la `GetProductByProductID(productID)` méthode à la `ProductsOptimisticConcurrency` TableAdapter.

Pour ce faire, cliquez sur la barre de titre du TableAdapter (le droit de la zone au-dessus de la `Fill` et `GetProducts` les noms de méthode) et choisissez Ajouter une requête dans le menu contextuel. Cette action lance l’Assistant Configuration de requêtes TableAdapter. Comme avec la configuration initiale de notre TableAdapter, choisir de créer le `GetProductByProductID(productID)` à l’aide d’une instruction SQL d’ad-hoc (méthode) (voir Figure 4). Dans la mesure où le `GetProductByProductID(productID)` méthode retourne des informations sur un produit particulier, d’indiquer que cette requête est un `SELECT` type qui retourne des lignes de requête.


[![Marquez le Type de requête comme un &quot;LECT qui retourne des lignes&quot;](implementing-optimistic-concurrency-vb/_static/image26.png)](implementing-optimistic-concurrency-vb/_static/image25.png)

**Figure 9**: Marquez le Type de requête comme un «`SELECT` qui retourne des lignes » ([cliquez pour afficher l’image en taille réelle](implementing-optimistic-concurrency-vb/_static/image27.png))


Dans l’écran suivant, nous avons invités pour la requête SQL à utiliser avec la requête du TableAdapter par défaut préchargée. Augmenter la requête existante pour inclure la clause `WHERE ProductID = @ProductID`, comme illustré dans la Figure 10.


[![Ajouter un WHERE Clause à la requête préchargée renvoie un enregistrement de produit spécifique](implementing-optimistic-concurrency-vb/_static/image29.png)](implementing-optimistic-concurrency-vb/_static/image28.png)

**Figure 10**: ajouter un `WHERE` Clause à la requête Pre-Loaded pour retourner un enregistrement de produit spécifique ([cliquez pour afficher l’image en taille réelle](implementing-optimistic-concurrency-vb/_static/image30.png))


Enfin, modifiez les noms de méthode générée à `FillByProductID` et `GetProductByProductID`.


[![Renommez les méthodes FillByProductID et GetProductByProductID](implementing-optimistic-concurrency-vb/_static/image32.png)](implementing-optimistic-concurrency-vb/_static/image31.png)

**Figure 11**: Renommez les méthodes à `FillByProductID` et `GetProductByProductID` ([cliquez pour afficher l’image en taille réelle](implementing-optimistic-concurrency-vb/_static/image33.png))


Avec cet Assistant terminé, le TableAdapter contient maintenant deux méthodes pour récupérer des données : `GetProducts()`, qui retourne *tous les* produits ; et `GetProductByProductID(productID)`, qui retourne le produit spécifié.

## <a name="step-3-creating-a-business-logic-layer-for-the-optimistic-concurrency-enabled-dal"></a>Étape 3 : Création d’une couche de logique métier pour la couche DAL accès concurrentiel optimiste

Notre existant `ProductsBLL` classe comporte des exemples d’utilisation de la mise à jour par lots et les modèles directs de base de données. Le `AddProduct` (méthode) et `UpdateProduct` surcharges les deux utilisent le modèle de mise à jour par lots, en passant un `ProductRow` instance à la méthode du TableAdapter mise à jour. Le `DeleteProduct` (méthode), utilise en revanche, le modèle direct de base de données, appelant du TableAdapter `Delete(productID)` (méthode).

Avec la nouvelle `ProductsOptimisticConcurrency` TableAdapter, la base de données direct de méthodes nécessitent désormais que les valeurs d’origine également être passé. Par exemple, le `Delete` méthode attend maintenant dix paramètres d’entrée : la version d’origine `ProductID`, `ProductName`, `SupplierID`, `CategoryID`, `QuantityPerUnit`, `UnitPrice`, `UnitsInStock`, `UnitsOnOrder`, `ReorderLevel`, et `Discontinued`. Il utilise les valeurs de ces paramètres d’entrée supplémentaires dans `WHERE` clause de la `DELETE` instruction envoyée à la base de données, uniquement la suppression de l’enregistrement spécifié si les valeurs actuelles de la base de données mappent à celles d’origine.

Alors que la signature de méthode pour le TableAdapter `Update` méthode utilisée dans le modèle de mise à jour de lot n’a pas changé, le code nécessaire pour enregistrer les valeurs d’origine et nouvelles a. Par conséquent, plutôt que de tenter d’utiliser la couche DAL accès concurrentiel optimiste avec notre existant `ProductsBLL` class, nous allons créer une nouvelle classe de couche de logique métier pour travailler avec notre nouvelle couche DAL.

Ajoutez une classe nommée `ProductsOptimisticConcurrencyBLL` à la `BLL` dossier au sein de la `App_Code` dossier.


![Ajoutez la classe ProductsOptimisticConcurrencyBLL dans le dossier de la couche de logique métier](implementing-optimistic-concurrency-vb/_static/image34.png)

**Figure 12**: ajouter la `ProductsOptimisticConcurrencyBLL` classe dans le dossier de la couche de logique métier


Ensuite, ajoutez le code suivant à la `ProductsOptimisticConcurrencyBLL` classe :


[!code-vb[Main](implementing-optimistic-concurrency-vb/samples/sample5.vb)]

Notez l’à l’aide `NorthwindOptimisticConcurrencyTableAdapters` instruction au-dessus du début de la déclaration de classe. Le `NorthwindOptimisticConcurrencyTableAdapters` espace de noms contient le `ProductsOptimisticConcurrencyTableAdapter` classe, qui fournit des méthodes de la couche DAL. En outre avant la déclaration de classe, vous allez trouver les `System.ComponentModel.DataObject` attribut, ce qui indique à Visual Studio pour inclure cette classe dans la liste déroulante de l’Assistant ObjectDataSource.

Le `ProductsOptimisticConcurrencyBLL`de `Adapter` propriété fournit un accès rapide à une instance de la `ProductsOptimisticConcurrencyTableAdapter` classe et suit le modèle utilisé dans nos classes d’origine de la couche BLL (`ProductsBLL`, `CategoriesBLL`, et ainsi de suite). Enfin, le `GetProducts()` méthode appelle simplement vers le bas de la couche DAL `GetProducts()` méthode et retourne un `ProductsOptimisticConcurrencyDataTable` objet rempli avec un `ProductsOptimisticConcurrencyRow` instance pour chaque enregistrement de produit dans la base de données.

## <a name="deleting-a-product-using-the-db-direct-pattern-with-optimistic-concurrency"></a>Suppression d’un produit au format base de données Direct avec l’accès concurrentiel optimiste

Lorsque vous utilisez le modèle de direct de base de données par rapport à une couche DAL qui utilise l’accès concurrentiel optimiste, les méthodes doivent être passés les valeurs nouvelles et originale. Pour la suppression, il n’existe aucune nouvelle valeur, par conséquent, seules les valeurs d’origine doivent être passées dans. Dans notre BLL, alors, nous devons accepter tous les paramètres d’origine en tant que paramètres d’entrée. Examinons le `DeleteProduct` méthode dans la `ProductsOptimisticConcurrencyBLL` classe utiliser la méthode directe de base de données. Cela signifie que cette méthode doit prendre dans tous les champs de données de produit dix en tant que paramètres d’entrée et les envoie vers la couche DAL, comme indiqué dans le code suivant :


[!code-vb[Main](implementing-optimistic-concurrency-vb/samples/sample6.vb)]

Si les valeurs d’origine - valeurs qui ont été dernier chargés dans le GridView (ou DetailsView ou FormView) - diffèrent des valeurs figurant dans la base de données lorsque l’utilisateur clique sur le bouton Supprimer le `WHERE` clause ne correspond à n’importe quel enregistrement de base de données et aucun enregistrement seront affectées. Par conséquent, le TableAdapter `Delete` méthode retournera `0` et de la couche BLL `DeleteProduct` méthode retournera `false`.

## <a name="updating-a-product-using-the-batch-update-pattern-with-optimistic-concurrency"></a>La mise à jour d’un produit à l’aide du modèle de mise à jour par lots avec l’accès concurrentiel optimiste

Comme indiqué précédemment, le TableAdapter `Update` méthode pour le modèle de mise à jour par lots a la même signature de méthode, quel que soit ou non l’accès concurrentiel optimiste est employée. À savoir, le `Update` méthode attend un DataRow, un tableau de DataRows, un DataTable ou un DataSet typé. Il n’existe aucun paramètre d’entrée supplémentaires pour spécifier les valeurs d’origine. Cela est possible, car la table de données effectue le suivi des valeurs d’origine et modifiés pour son DataRow(s). Lorsque la couche DAL émet son `UPDATE` instruction, le `@original_ColumnName` paramètres sont remplis avec les valeurs d’origine de DataRow, tandis que le `@ColumnName` paramètres sont remplis avec les valeurs modifiées de DataRow.

Dans la `ProductsBLL` classe (qui utilise notre d’accès concurrentiel d’origine, non optimiste DAL), lorsque vous utilisez le modèle de mise à jour par lots pour mettre à jour les informations de produit notre code exécute la séquence d’événements suivante :

1. Lire les informations de produit de base de données actuelle dans un `ProductRow` instance à l’aide du TableAdapter `GetProductByProductID(productID)` (méthode)
2. Affecter les nouvelles valeurs à la `ProductRow` instance à partir de l’étape 1
3. Appelez le TableAdapter `Update` méthode, en passant le `ProductRow` instance

Cette séquence d’étapes, toutefois, ne sont pas correctement prendre en charge l’accès concurrentiel optimiste, car le `ProductRow` renseigné dans étape 1 est remplie directement à partir de la base de données, ce qui signifie que les valeurs d’origine utilisés par le DataRow sont ceux qui existent actuellement dans le base de données et non ceux qui ont été liées au GridView au début du processus de modification. Au lieu de cela, lorsque vous à l’aide d’une manière optimiste accès concurrentiel DAL, nous devons modifier la `UpdateProduct` des surcharges de méthode à utiliser comme suit :

1. Lire les informations de produit de base de données actuelle dans un `ProductsOptimisticConcurrencyRow` instance à l’aide du TableAdapter `GetProductByProductID(productID)` (méthode)
2. Affecter le *d’origine* valeurs à la `ProductsOptimisticConcurrencyRow` instance à partir de l’étape 1
3. Appelez le `ProductsOptimisticConcurrencyRow` l’instance `AcceptChanges()` (méthode), qui indique à l’objet DataRow que ses valeurs actuelles sont ceux « original »
4. Affecter le *nouveau* valeurs à la `ProductsOptimisticConcurrencyRow` instance
5. Appelez le TableAdapter `Update` méthode, en passant le `ProductsOptimisticConcurrencyRow` instance

Étape 1 des lectures dans toutes les valeurs actuelles de la base de données pour l’enregistrement de produit spécifié. Cette étape est superflue dans le `UpdateProduct` surcharge qui met à jour *tous les* des colonnes de produit (en tant que ces valeurs sont remplacées à l’étape 2), mais est essentiel pour ces surcharges où uniquement un sous-ensemble des valeurs de colonne sont passés comme paramètres d’entrée. Une fois que les valeurs d’origine ont été attribuées à la `ProductsOptimisticConcurrencyRow` instance, le `AcceptChanges()` méthode est appelée, ce qui marque les DataRow de valeurs actuelles comme valeurs d’origine à utiliser dans le `@original_ColumnName` paramètres dans le `UPDATE` instruction. Ensuite, les nouvelles valeurs de paramètre sont affectés à la `ProductsOptimisticConcurrencyRow` et, enfin, le `Update` méthode est appelée, en passant le DataRow.

Le code suivant illustre la `UpdateProduct` surcharge qui accepte toutes les données de produit champs comme paramètres d’entrée. Bien que non illustrée ici, le `ProductsOptimisticConcurrencyBLL` classe inclus dans le téléchargement de ce didacticiel contient également un `UpdateProduct` surcharge qui accepte uniquement le produit nom et le prix en tant que paramètres d’entrée.


[!code-vb[Main](implementing-optimistic-concurrency-vb/samples/sample7.vb)]

## <a name="step-4-passing-the-original-and-new-values-from-the-aspnet-page-to-the-bll-methods"></a>Étape 4 : En passant les valeurs d’origine et nouvelles à partir de la Page ASP.NET pour les méthodes de la couche de logique métier

Avec la couche DAL et de la couche BLL complète, reste qu’à créer une page ASP.NET qui peut utiliser la logique d’accès concurrentiel optimiste intégrée au système. Plus précisément, les données de contrôle Web (le GridView, DetailsView ou FormView) doivent penser à ses valeurs d’origine et ObjectDataSource doivent passer les deux ensembles de valeurs à la couche de logique métier. En outre, la page ASP.NET doit être configurée pour gérer correctement les violations d’accès concurrentiel.

Commencez par ouvrir le `OptimisticConcurrency.aspx` page dans le `EditInsertDelete` dossier et en ajoutant un GridView vers le concepteur, en définissant son `ID` propriété `ProductsGrid`. À partir de la balise active le contrôle GridView, choisir de créer un nouveau ObjectDataSource nommé `ProductsOptimisticConcurrencyDataSource`. Dans la mesure où nous voulons cette ObjectDataSource à utiliser la couche DAL qui prend en charge l’accès concurrentiel optimiste, configurez-le pour utiliser le `ProductsOptimisticConcurrencyBLL` objet.


[![Que l’utilisation de l’ObjectDataSource l’objet de ProductsOptimisticConcurrencyBLL](implementing-optimistic-concurrency-vb/_static/image36.png)](implementing-optimistic-concurrency-vb/_static/image35.png)

**Figure 13**: ont l’utilisation de l’ObjectDataSource le `ProductsOptimisticConcurrencyBLL` objet ([cliquez pour afficher l’image en taille réelle](implementing-optimistic-concurrency-vb/_static/image37.png))


Choisissez le `GetProducts`, `UpdateProduct`, et `DeleteProduct` méthodes à partir des listes déroulantes dans l’Assistant. Pour la méthode UpdateProduct, utilisez la surcharge qui accepte tous les champs de données du produit.

## <a name="configuring-the-objectdatasource-controls-properties"></a>Configuration des propriétés du contrôle ObjectDataSource

À l’issue de l’Assistant, balisage déclaratif de l’ObjectDataSource doit ressembler à ce qui suit :


[!code-aspx[Main](implementing-optimistic-concurrency-vb/samples/sample8.aspx)]

Comme vous pouvez le voir, le `DeleteParameters` collection contient un `Parameter` instance pour chacun des dix paramètres d’entrée dans le `ProductsOptimisticConcurrencyBLL` la classe `DeleteProduct` (méthode). De même, le `UpdateParameters` collection contient un `Parameter` instance pour chacun des paramètres d’entrée dans `UpdateProduct`.

Pour ces didacticiels précédents qui impliquait une modification des données, nous supprimons l’ObjectDataSource `OldValuesParameterFormatString` propriété à ce stade, dans la mesure où cette propriété indique que la méthode de la couche BLL prévoit les valeurs de l’anciennes (ou d’origine) devant être transmis dans, ainsi que les nouvelles valeurs. En outre, cette valeur de propriété indique les noms de paramètre d’entrée pour les valeurs d’origine. Étant donné que nous sommes en passant les valeurs d’origine dans la couche BLL, faire *pas* supprimez cette propriété.

> [!NOTE]
> La valeur de la `OldValuesParameterFormatString` propriété doit correspondre aux noms de paramètre d’entrée de la couche BLL qui attendent des valeurs d’origine. Étant donné que nous avons nommé ces paramètres `original_productName`, `original_supplierID`, et ainsi de suite, vous pouvez laisser le `OldValuesParameterFormatString` en tant que valeur de la propriété `original_{0}`. Si, toutefois, les paramètres d’entrée des méthodes de la couche BLL avaient noms tels que `old_productName`, `old_supplierID`, et ainsi de suite, vous devez mettre à jour le `OldValuesParameterFormatString` propriété `old_{0}`.


Il existe un paramètre de propriété final qui doit être effectuée dans l’ordre de l’ObjectDataSource correctement transmettre les valeurs d’origine pour les méthodes de la couche BLL. ObjectDataSource a un [propriété ConflictDetection](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.conflictdetection.aspx) pouvant être assigné à [une des deux valeurs](https://msdn.microsoft.com/library/system.web.ui.conflictoptions.aspx):

- `OverwriteChanges` -la valeur par défaut ; n’envoie pas les valeurs d’origine aux paramètres d’entrée d’origine des méthodes de la couche de logique métier
- `CompareAllValues` -envoie les valeurs d’origine pour les méthodes de la couche de logique métier ; Choisissez cette option lors de l’utilisation de l’accès concurrentiel optimiste

Prenez un moment pour définir le `ConflictDetection` propriété `CompareAllValues`.

## <a name="configuring-the-gridviews-properties-and-fields"></a>Configuration des propriétés et champs de la GridView

Avec les propriétés de l’ObjectDataSource correctement configurées, penchons-nous pour configurer le contrôle GridView. Tout d’abord, étant donné que nous voulons le contrôle GridView pour prendre en charge la modification et la suppression, cliquez sur les cases à cocher Activer la modification et activer la suppression à partir de la balise active le contrôle GridView. Cela ajoutera un CommandField dont `ShowEditButton` et `ShowDeleteButton` sont toutes deux définies sur `true`.

Lorsqu’elle est liée à la `ProductsOptimisticConcurrencyDataSource` ObjectDataSource, le contrôle GridView contient un champ pour chacun des champs de données du produit. Alors que ce type d’un GridView peut être modifié, l’expérience utilisateur est n’est pas acceptable. Le `CategoryID` et `SupplierID` BoundFields restitue sous forme de zones de texte, demandant à l’utilisateur à entrer la catégorie appropriée et le fournisseur en tant que numéros d’identification. Il n’y aura aucune mise en forme pour les champs numériques et aucun contrôle de validation pour vous assurer que le nom du produit a été fourni et que le prix unitaire, unités en stock, les unités sur l’ordre et les valeurs de niveau de renouvellement de commande sont les deux valeurs numériques appropriées et sont supérieures à ou égal à à zéro.

Comme expliqué dans la *Ajout de contrôles de Validation pour l’édition et insertion des Interfaces* et *personnalisation de l’Interface de Modification de données* didacticiels, l’interface utilisateur peut être personnalisé par en remplaçant le BoundFields avec TemplateFields. J’ai modifié ce GridView et son interface de modification de plusieurs manières :

- Supprimé le `ProductID`, `SupplierName`, et `CategoryName` BoundFields
- Converti le `ProductName` BoundField en TemplateField et ajouté un contrôle RequiredFieldValidation.
- Converti le `CategoryID` et `SupplierID` BoundFields à TemplateField et ajustée de l’interface de modification pour utiliser DropDownList plutôt que des zones de texte. Dans ces TemplateField `ItemTemplates`, le `CategoryName` et `SupplierName` des champs de données sont affichées.
- Converti le `UnitPrice`, `UnitsInStock`, `UnitsOnOrder`, et `ReorderLevel` BoundFields TemplateField et contrôles CompareValidator ajoutés.

Étant donné que nous avons déjà examiné comment accomplir ces tâches dans les didacticiels précédents, je simplement répertorier la dernière syntaxe déclarative et laissez l’implémentation en pratique.


[!code-aspx[Main](implementing-optimistic-concurrency-vb/samples/sample9.aspx)]

Nous sommes très proche d’avoir un exemple entièrement fonctionnel. Toutefois, il existe quelques subtilités qui CA file et entraîner des problèmes. En outre, nous devons toujours une interface qui avertit l’utilisateur qu’une violation d’accès concurrentiel s’est produite.

> [!NOTE]
> Pour un contrôle Web de données puisse correctement transmettre les valeurs d’origine à ObjectDataSource (qui sont ensuite transmis à la couche BLL), il est essentiel que le GridView `EnableViewState` propriété est définie sur `true` (la valeur par défaut). Si vous désactivez l’état d’affichage, les valeurs d’origine sont perdues lors de la publication.


## <a name="passing-the-correct-original-values-to-the-objectdatasource"></a>En passant les valeurs d’origine correctes à ObjectDataSource

Il existe quelques problèmes avec la manière dont le contrôle GridView a été configuré. Si l’ObjectDataSource `ConflictDetection` propriété est définie sur `CompareAllValues` (en l’état nôtre), lorsque l’ObjectDataSource `Update()` ou `Delete()` méthodes sont appelées par GridView (ou DetailsView ou FormView), ObjectDataSource tente de copier le Les valeurs d’origine du contrôle GridView dans son approprié `Parameter` instances. Font référence à la Figure 2 pour une représentation graphique de ce processus.

Plus précisément, les valeurs d’origine du GridView affectés les valeurs dans les instructions de liaison de données bidirectionnelle chaque fois que les données sont liées au GridView. Par conséquent, il est essentiel que les toutes les valeurs d’origine requises sont capturés par le biais de liaison de données bidirectionnelle et qu’elles sont fournies dans un format convertible.

Pour voir pourquoi il est important, prenez un moment pour visitez notre page dans un navigateur. Comme prévu, le contrôle GridView répertorie chaque produit avec le bouton Edit et Delete dans la colonne de gauche.


[![Les produits sont répertoriés dans un GridView](implementing-optimistic-concurrency-vb/_static/image39.png)](implementing-optimistic-concurrency-vb/_static/image38.png)

**Figure 14**: les produits sont répertoriés dans un GridView ([cliquez pour afficher l’image en taille réelle](implementing-optimistic-concurrency-vb/_static/image40.png))


Si vous cliquez sur le bouton Supprimer pour tout produit, un `FormatException` est levée.


[![Tentez de supprimer n’importe quel produit entraîne une exception FormatException](implementing-optimistic-concurrency-vb/_static/image42.png)](implementing-optimistic-concurrency-vb/_static/image41.png)

**Figure 15**: tentative de supprimer n’importe quel produit entraîne un `FormatException` ([cliquez pour afficher l’image en taille réelle](implementing-optimistic-concurrency-vb/_static/image43.png))


Le `FormatException` est déclenché quand l’ObjectDataSource tente de lire dans l’original `UnitPrice` valeur. Dans la mesure où le `ItemTemplate` a le `UnitPrice` mis en forme comme une devise (`<%# Bind("UnitPrice", "{0:C}") %>`), il inclut un symbole monétaire, comme 19,95 $. Le `FormatException` se produit lorsque l’ObjectDataSource tente de convertir cette chaîne en un `decimal`. Pour contourner ce problème, nous avons un certain nombre d’options :

- Supprimer la devise à partir de la mise en forme le `ItemTemplate`. Autrement dit, au lieu d’utiliser `<%# Bind("UnitPrice", "{0:C}") %>`, utilisez simplement `<%# Bind("UnitPrice") %>`. L’inconvénient de ce est que le prix est n’est plus mis en forme.
- Affichage du `UnitPrice` mis en forme comme une devise dans la `ItemTemplate`, mais utiliser le `Eval` mot clé pour effectuer cette opération. N’oubliez pas que `Eval` effectue la liaison de données unidirectionnelle. Nous devons toujours fournir le `UnitPrice` valeur pour les valeurs d’origine, nous aurons donc toujours besoin une instruction de liaison de données bidirectionnelle dans le `ItemTemplate`, mais cela peut être placé dans un contrôle étiquette Web dont la propriété `Visible` propriété est définie sur `false`. Nous pourrions utiliser le balisage suivant dans le modèle ItemTemplate :


[!code-aspx[Main](implementing-optimistic-concurrency-vb/samples/sample10.aspx)]

- Supprimer la devise à partir de la mise en forme le `ItemTemplate`, à l’aide `<%# Bind("UnitPrice") %>`. Dans le GridView `RowDataBound` Gestionnaire d’événements, l’accès par programme le Web de l’étiquette de contrôle dans lequel le `UnitPrice` valeur est affichée et définir son `Text` propriété vers la version mise en forme.
- Laissez le `UnitPrice` mis en forme comme une devise. Dans le GridView `RowDeleting` Gestionnaire d’événements, remplacez la version d’origine existante `UnitPrice` valeur (19,95$) avec une valeur décimale réelle à l’aide `Decimal.Parse`. Nous avons vu comment accomplir quelque chose de similaire dans le `RowUpdating` Gestionnaire d’événements dans le [ *BLL - gestion et les Exceptions au niveau de la couche DAL dans une Page ASP.NET* ](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs.md) didacticiel.

Pour mon exemple j’ai opté pour la deuxième approche, ajout d’un serveur Web Label masqué contrôle dont `Text` propriété est liées à la non mise en forme de données bidirectionnelle `UnitPrice` valeur.

Après la résolution de ce problème, essayez de cliquer à nouveau sur le bouton Supprimer pour tous les produits. Cette fois, vous obtiendrez un `InvalidOperationException` lorsque ObjectDataSource tente d’appeler la couche de logique métier `UpdateProduct` (méthode).


[![ObjectDataSource ne peut pas trouver de méthode avec les paramètres d’entrée qu’il veut envoyer](implementing-optimistic-concurrency-vb/_static/image45.png)](implementing-optimistic-concurrency-vb/_static/image44.png)

**Figure 16**: ObjectDataSource ne peut pas trouver de méthode avec les paramètres d’entrée qu’il veut envoyer ([cliquez pour afficher l’image en taille réelle](implementing-optimistic-concurrency-vb/_static/image46.png))


Examinez le message de l’exception, il est clair que ObjectDataSource souhaite appeler une couche de logique métier `DeleteProduct` méthode inclut `original_CategoryName` et `original_SupplierName` paramètres d’entrée. Il s’agit, car le `ItemTemplate` s pour le `CategoryID` et `SupplierID` TemplateField actuellement contiennent des instructions de liaison bidirectionnelles avec le `CategoryName` et `SupplierName` des champs de données. Au lieu de cela, nous devons inclure `Bind` instructions avec le `CategoryID` et `SupplierID` des champs de données. Pour ce faire, remplacez les instructions de liaison existantes avec `Eval` instructions, puis ajoutez une étiquette masquée contrôles dont `Text` propriétés sont liées à la `CategoryID` et `SupplierID` des champs de données à l’aide de la liaison de données bidirectionnelle, comme indiqué ci-dessous :


[!code-aspx[Main](implementing-optimistic-concurrency-vb/samples/sample11.aspx)]

Avec ces modifications, nous sommes maintenant en mesure de supprimer correctement et de modifier les informations de produit ! À l’étape 5, nous allons examiner comment vérifier que les violations d’accès concurrentiel sont détectées. Mais pour l’instant, prendre quelques minutes pour essayer la mise à jour et suppression des enregistrements pour vous assurer que la mise à jour et suppression pour un seul utilisateur fonctionne comme prévu.

## <a name="step-5-testing-the-optimistic-concurrency-support"></a>Étape 5 : Test de la prise en charge l’accès concurrentiel optimiste

Pour vérifier que les violations d’accès concurrentiel sont détectées (plutôt que résultant dans aveuglément écrasées des données), que nous devons ouvrir deux fenêtres de navigateur à cette page. Dans les deux instances de navigateur, cliquez sur le bouton Modifier pour Tran. Puis, dans un des navigateurs, remplacez le nom par « Thé Chai » et cliquez sur la mise à jour. La mise à jour doit réussir et retourner le contrôle GridView à son état préalable Édition, avec « Thé Chai » en tant que le nouveau nom de produit.

Dans l’autre instance de fenêtre de navigateur, toutefois, la zone de texte du nom de produit indique toujours « Martin ». Dans cette deuxième fenêtre de navigateur, mettez à jour le `UnitPrice` à `25.00`. Sans prise en charge l’accès concurrentiel optimiste, en cliquant sur la mise à jour dans la deuxième instance de navigateur modifierait le nom du produit à « Martin », en écrasant les modifications apportées par la première instance de navigateur. Avec les employés l’accès concurrentiel optimiste, toutefois, en cliquant sur le bouton de mise à jour dans la deuxième instance du navigateur entraîne un [DBConcurrencyException](https://msdn.microsoft.com/library/system.data.dbconcurrencyexception.aspx).


[![Lorsqu’une Violation d’accès concurrentiel est détectée, un DBConcurrencyException est levé.](implementing-optimistic-concurrency-vb/_static/image48.png)](implementing-optimistic-concurrency-vb/_static/image47.png)

**Figure 17**: lors de la Violation d’accès concurrentiel a est détectée, un `DBConcurrencyException` est levée ([cliquez pour afficher l’image en taille réelle](implementing-optimistic-concurrency-vb/_static/image49.png))


Le `DBConcurrencyException` n’est levée lorsque le modèle de mise à jour de la couche DAL batch est utilisé. Le modèle de base de données direct ne lève pas d’exception, il indique simplement qu’aucune ligne n’a été affectée. Pour illustrer ceci, retourner les deux instances du navigateur GridView à leur état de modification préalable. Ensuite, dans la première instance de navigateur, cliquez sur le bouton Modifier et modifiez le nom du produit à partir de « Thé Chai » à « Martin » et cliquez sur la mise à jour. Dans la deuxième fenêtre de navigateur, cliquez sur le bouton Supprimer de Tran.

Lorsque vous cliquez sur Supprimer, publication de la page, le contrôle GridView appelle l’ObjectDataSource `Delete()` méthode et ObjectDataSource appelle vers le bas dans la `ProductsOptimisticConcurrencyBLL` la classe `DeleteProduct` méthode, en passant les valeurs d’origine. La version d’origine `ProductName` valeur de la deuxième instance de navigateur est « Thé Chai », qui ne correspond pas avec actuel `ProductName` valeur dans la base de données. Par conséquent le `DELETE` instruction émise à la base de données n’affecte aucune ligne dans la mesure où il n’existe aucun enregistrement dans la base de données qui le `WHERE` clause satisfait. Le `DeleteProduct` retourne de la méthode `false` et les données de l’ObjectDataSource sont de nouveau liées au GridView.

Du point de vue de l’utilisateur final, en cliquant sur le bouton Supprimer pour Tea Martin dans la deuxième fenêtre de navigateur a provoqué l’écran pour flash et à fidéliser, le produit est toujours là, mais maintenant il est répertorié comme « Chai » (la produit nom modification apportée par le premier navigateur instance). Si l’utilisateur clique sur le bouton Supprimer à nouveau, la suppression réussit, comme le contrôle GridView d’origine de le `ProductName` valeur (« Chai ») correspond maintenant à avec la valeur dans la base de données.

Dans ces deux cas, l’expérience utilisateur est loin d’être idéal. Nous clairement ne souhaitons montrer l’utilisateur les moindres détails de la `DBConcurrencyException` exception lorsque vous utilisez le modèle de mise à jour par lots. Et le comportement lorsque vous utilisez le modèle de base de données direct est prêter à confusion car échec de la commande d’utilisateurs, mais il n’a aucune indication précise de la raison pour laquelle.

Pour remédier à ces deux problèmes, nous pouvons créer des contrôles Web de l’étiquette sur la page qui fournissent une explication pour des raisons pour lesquelles une mise à jour ou l’échec de la suppression. Pour le modèle de mise à jour par lots, nous pouvons déterminer ou non un `DBConcurrencyException` exception s’est produite dans le Gestionnaire d’événements postérieurs au niveau du GridView, affichage de l’étiquette d’avertissement en fonction des besoins. La méthode directe de base de données, nous pouvons examiner la valeur de retour de la méthode de la couche BLL (c'est-à-dire `true` si une ligne a été affectée, `false` sinon) et afficher un message d’information en fonction des besoins.

## <a name="step-6-adding-informational-messages-and-displaying-them-in-the-face-of-a-concurrency-violation"></a>Étape 6 : Ajout de Messages d’information et les afficher face à une Violation d’accès concurrentiel

Lorsqu’une violation d’accès concurrentiel se produit, le comportement affiché varie selon que la mise à jour par lots ou modèle direct de base de données de la couche Data Access a été utilisée. Notre didacticiel utilise les deux modèles, avec le modèle de mise à jour par lots utilisée pour la mise à jour et le modèle de direct de base de données utilisé pour la suppression. Pour commencer, nous allons ajouter deux contrôles Label à notre page expliquant qu’une violation d’accès concurrentiel s’est produite lors de la tentative de supprimer ou de mettre à jour des données. Définir le contrôle d’étiquette `Visible` et `EnableViewState` propriétés à `false`; ainsi, leur être masqué sur chaque visite de page, sauf pour celles page particulière visite où leurs `Visible` propriété est définie par programmation sur `true`.


[!code-aspx[Main](implementing-optimistic-concurrency-vb/samples/sample12.aspx)]

En plus du paramètre leurs `Visible`, `EnabledViewState`, et `Text` propriétés, j’ai également défini le `CssClass` propriété `Warning`, ce qui entraîne l’étiquette du à afficher dans une police de grande taille, rouge, italique, gras. Cette CSS `Warning` classe a été définie et ajoutée à Styles.css dans le *examinant les événements associés insertion, mise à jour et suppression* didacticiel.

Après avoir ajouté ces étiquettes, le concepteur dans Visual Studio doit ressembler à la Figure 18.


[![Deux contrôles Label ont été ajoutés à la Page](implementing-optimistic-concurrency-vb/_static/image51.png)](implementing-optimistic-concurrency-vb/_static/image50.png)

**Figure 18**: deux étiquette contrôles ont été ajoutés à la Page ([cliquez pour afficher l’image en taille réelle](implementing-optimistic-concurrency-vb/_static/image52.png))


Avec ces contrôles Web Label en place, nous pouvons examiner comment déterminer quand une violation d’accès concurrentiel a eu lieu, l’endroit où l’étiquette appropriée `Visible` propriété peut être définie sur `true`, affichant le message d’information.

## <a name="handling-concurrency-violations-when-updating"></a>Gestion des Violations d’accès concurrentiel lors de la mise à jour

Commençons par examiner comment gérer les violations d’accès concurrentiel lorsque vous utilisez le modèle de mise à jour par lots. Dans la mesure où ces violations avec le traitement par lots Mettre à jour la cause de modèle un `DBConcurrencyException` exception levée, nous devons ajouter du code à notre page ASP.NET pour déterminer si un `DBConcurrencyException` exception s’est produite pendant le processus de mise à jour. Si par conséquent, nous devons afficher un message à l’utilisateur expliquant leurs modifications n’ont pas été enregistrées, car un autre utilisateur a modifié des données entre lorsqu’il a commencé la modification de l’enregistrement et lorsqu’ils cliquent sur le bouton de mise à jour.

Comme nous l’avons vu dans la *BLL - gestion et les Exceptions au niveau de la couche DAL dans une Page ASP.NET* (didacticiel), ces exceptions peuvent être détectées et supprimées dans les gestionnaires d’événements postérieurs au niveau du contrôle Web de données. Par conséquent, nous devons créer un gestionnaire d’événements pour le contrôle GridView `RowUpdated` événement qui vérifie si un `DBConcurrencyException` exception a été levée. Ce gestionnaire d’événements est passé à toute exception qui a été levée pendant le processus de mise à jour, comme indiqué dans l’événement Gestionnaire de code ci-dessous :


[!code-vb[Main](implementing-optimistic-concurrency-vb/samples/sample13.vb)]

Dans d’un `DBConcurrencyException` exception, ce gestionnaire d’événements affiche les `UpdateConflictMessage` contrôle Label et indique que l’exception a été gérée. Avec ce code en place, lorsqu’une violation d’accès concurrentiel se produit quand la mise à jour un enregistrement, les modifications de l’utilisateur sont perdues, dans la mesure où ils seraient ont remplacé les modifications d’un autre utilisateur en même temps. En particulier, le contrôle GridView est rétabli à son état préalable Édition et lié à la base de données actuelle. Cela met à jour la ligne GridView avec les modifications des autres utilisateurs, qui étaient précédemment pas visibles. En outre, le `UpdateConflictMessage` contrôle Label explique à l’utilisateur que s’est-il passé. Cette séquence d’événements est détaillée dans la Figure 19.


[![Un utilisateur s mises à jour sont perdues à la Face d’une Violation d’accès concurrentiel](implementing-optimistic-concurrency-vb/_static/image54.png)](implementing-optimistic-concurrency-vb/_static/image53.png)

**Figure 19**: un utilisateur s mises à jour sont perdues à la Face d’une Violation d’accès concurrentiel ([cliquez pour afficher l’image en taille réelle](implementing-optimistic-concurrency-vb/_static/image55.png))


> [!NOTE]
> Vous pouvez également, plutôt que de retourner le contrôle GridView à l’état de modification préalable, nous pouvons conserver le contrôle GridView dans son état de modification en définissant le `KeepInEditMode` propriété du passé en `GridViewUpdatedEventArgs` objet sur true. Si vous adoptez cette approche, cependant, veillez à lier de nouveau les données pour le contrôle GridView (en appelant son `DataBind()` méthode) afin que les autres valeurs l’utilisateur sont chargées dans l’interface de modification. Le code est disponible en téléchargement avec ce didacticiel a ces deux lignes de code dans le `RowUpdated` commenté de gestionnaire d’événements ; simplement ne pas commenter ces lignes de code pour que le contrôle GridView restent en mode édition après une violation d’accès concurrentiel.


## <a name="responding-to-concurrency-violations-when-deleting"></a>Réponse aux Violations d’accès concurrentiel lors de la suppression

Avec le modèle direct de base de données, il n’existe aucune exception levée en cas d’une violation d’accès concurrentiel. Au lieu de cela, l’instruction de base de données n’affecte simplement aucun enregistrement, comme la clause WHERE ne correspond pas à n’importe quel enregistrement. Toutes les méthodes de modification de données créées dans la couche BLL ont été conçus de sorte qu’elles retournent une valeur booléenne indiquant si elles affectées exactement un seul enregistrement. Par conséquent, pour déterminer si une violation d’accès concurrentiel s’est produite lors de la suppression d’un enregistrement, nous pouvons examiner la valeur de retour de la couche de logique métier `DeleteProduct` (méthode).

La valeur de retour pour une méthode de la couche de logique métier peut être examinée dans les gestionnaires d’événements postérieurs au niveau de l’ObjectDataSource via le `ReturnValue` propriété de la `ObjectDataSourceStatusEventArgs` objet passé dans le Gestionnaire d’événements. Dans la mesure où nous nous intéressons à la détermination de la valeur de retour à partir de la `DeleteProduct` (méthode), nous devons créer un gestionnaire d’événements pour l’ObjectDataSource `Deleted` événement. Le `ReturnValue` propriété est de type `object` et peut être `null` si une exception a été levée et la méthode a été interrompue avant qu’il peut retourner une valeur. Par conséquent, nous devons tout d’abord vous assurer que le `ReturnValue` propriété n’est pas `null` et est une valeur booléenne. En supposant que cette vérification effectuée, nous affichons le `DeleteConflictMessage` Label, contrôle si le `ReturnValue` est `false`. Cela est possible en utilisant le code suivant :


[!code-vb[Main](implementing-optimistic-concurrency-vb/samples/sample14.vb)]

Face à une violation d’accès concurrentiel, la demande de suppression de l’utilisateur est annulée. Le contrôle GridView est actualisé, affichant les modifications qui se sont produites pour cet enregistrement entre le moment où l’utilisateur chargement de la page et quand il a cliqué sur le bouton Supprimer. Lorsqu’une telle violation se passe réellement, le `DeleteConflictMessage` étiquette s’affiche, expliquant que s’est-il passé (voir la Figure 20).


[![Un utilisateur s Delete est annulé en cas d’une Violation d’accès concurrentiel](implementing-optimistic-concurrency-vb/_static/image57.png)](implementing-optimistic-concurrency-vb/_static/image56.png)

**Figure 20**: un utilisateur s Delete est annulée en cas d’une Violation d’accès concurrentiel ([cliquez pour afficher l’image en taille réelle](implementing-optimistic-concurrency-vb/_static/image58.png))


## <a name="summary"></a>Récapitulatif

Il existe des possibilités de violations d’accès concurrentiel dans chaque application qui permet à plusieurs utilisateurs simultanés à mettre à jour ou supprimer des données. Si ces violations ne sont pas prises en compte, lorsque deux utilisateurs mettre à jour simultanément les mêmes données, toute personne qui obtient dans la dernière écriture « wins », en remplaçant l’utilisateur modifications de modifications. Vous pouvez également, les développeurs peuvent implémenter un contrôle d’accès concurrentiel optimiste ou pessimiste. Contrôle d’accès concurrentiel optimiste suppose que les violations d’accès concurrentiel sont peu fréquentes simplement n’autorise pas une mise à jour ou supprimer des commandes qui constituerait une violation d’accès concurrentiel. Contrôle d’accès concurrentiel pessimiste suppose que cette concurrence violations sont fréquents et rejeter tout simplement d’un utilisateur de mettre à jour ou de supprimer la commande n’est pas acceptable. Avec le contrôle d’accès concurrentiel pessimiste, un enregistrement de la mise à jour implique le verrouillage, empêchant ainsi tous les autres utilisateurs de modifier ou supprimer l’enregistrement lorsqu’il est verrouillé.

Le DataSet typé dans .NET fournit des fonctionnalités pour prendre en charge le contrôle d’accès concurrentiel optimiste. En particulier, le `UPDATE` et `DELETE` instructions émises à la base de données incluent toutes les colonnes de la table, ce qui garantit que la mise à jour ou la suppression survient uniquement si les données actuelles de l’enregistrement correspond à avec les données d’origine de l’utilisateur que lorsque exécution de leur mise à jour ou la suppression. Une fois que la couche DAL a été configurée pour prendre en charge l’accès concurrentiel optimiste, les méthodes de la couche de logique métier doivent être mis à jour. En outre, la page ASP.NET qui effectue des appels vers le bas dans la couche BLL doit être configurée que ObjectDataSource récupère les valeurs d’origine à partir de son contrôle Web de données et les transfère vers le bas dans la couche BLL.

Comme nous l’avons vu dans ce didacticiel, l’implémentation de contrôle d’accès concurrentiel optimiste dans une application web ASP.NET implique la mise à jour de la couche DAL et la couche BLL et ajout de prise en charge dans la page ASP.NET. Ce surplus de travail soit ou non un investissement en termes de votre temps et d’efforts dépend de votre application. Si vous avez rarement des utilisateurs simultanés, la mise à jour des données, ou les données qu’ils mettent à jour sont différentes entre eux, contrôle d’accès concurrentiel n’est pas un problème fondamental. Si, toutefois, vous devez régulièrement plusieurs utilisateurs sur votre site fonctionne avec les mêmes données, contrôle d’accès concurrentiel peut aider à empêcher les mises à jour d’un utilisateur ou des suppressions d’involontaire en remplacement d’un autre.

Bonne programmation !

## <a name="about-the-author"></a>À propos de l’auteur

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), auteur de sept les livres sur ASP/ASP.NET et fondateur de [4GuysFromRolla.com](http://www.4guysfromrolla.com), travaille avec les technologies Web Microsoft depuis 1998. Scott fonctionne comme un consultant indépendant, formateur et writer. Son dernier ouvrage est [*SAM animer vous-même ASP.NET 2.0 des dernières 24 heures*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Il peut être contacté à [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou via son blog, qui se trouve à [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Précédent](customizing-the-data-modification-interface-vb.md)
> [Suivant](adding-client-side-confirmation-when-deleting-vb.md)
